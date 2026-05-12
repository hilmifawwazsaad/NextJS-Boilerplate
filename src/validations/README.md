# `src/validations/`

Folder ini menyimpan **skema validasi** yang digunakan untuk memvalidasi input pengguna di form, Route Handlers, dan Server Actions.

> **Penting:** Validasi sebaiknya didefinisikan sekali di sini dan digunakan di dua tempat: sisi client (validasi form real-time) dan sisi server (validasi sebelum menyimpan ke database). Jangan hanya validasi di salah satu sisi.

## Kegunaan

- Mendefinisikan aturan validasi menggunakan library seperti Zod atau Yup
- Memastikan data yang masuk ke server sesuai dengan schema yang diharapkan
- Menghasilkan pesan error yang konsisten antara client dan server
- Dapat diekstrak tipe TypeScript dari skema (Zod inference)

## Struktur yang Disarankan

```
src/validations/
├── auth.validation.js      # Validasi login, register, reset password
├── user.validation.js      # Validasi update profil, ganti password
├── post.validation.js      # Validasi pembuatan dan edit postingan
└── upload.validation.js    # Validasi ukuran dan tipe file
```

## Contoh Penggunaan

**`src/validations/auth.validation.js`**

```js
import { z } from 'zod';

export const loginSchema = z.object({
  email: z.string().email('Format email tidak valid'),
  password: z.string().min(8, 'Password minimal 8 karakter'),
});

export const registerSchema = z
  .object({
    name: z.string().min(2, 'Nama minimal 2 karakter'),
    email: z.string().email('Format email tidak valid'),
    password: z.string().min(8, 'Password minimal 8 karakter'),
    confirmPassword: z.string(),
  })
  .refine((data) => data.password === data.confirmPassword, {
    message: 'Password tidak cocok',
    path: ['confirmPassword'],
  });
```

**Digunakan di Server Action (`app/auth/actions.js`):**

```js
'use server';

import { registerSchema } from '@/validations/auth.validation';
import { createUser } from '@/services/auth.service';

export async function registerAction(formData) {
  const raw = {
    name: formData.get('name'),
    email: formData.get('email'),
    password: formData.get('password'),
    confirmPassword: formData.get('confirmPassword'),
  };

  const result = registerSchema.safeParse(raw);

  if (!result.success) {
    return { success: false, errors: result.error.flatten().fieldErrors };
  }

  await createUser(result.data);
  return { success: true };
}
```

**Digunakan di form client dengan React Hook Form:**

```jsx
'use client';

import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { loginSchema } from '@/validations/auth.validation';

export default function LoginForm() {
  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm({
    resolver: zodResolver(loginSchema),
  });

  const onSubmit = async (data) => {
    // data sudah tervalidasi
    console.log(data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input {...register('email')} placeholder='Email' />
      {errors.email && <p>{errors.email.message}</p>}

      <input {...register('password')} type='password' placeholder='Password' />
      {errors.password && <p>{errors.password.message}</p>}

      <button type='submit'>Masuk</button>
    </form>
  );
}
```
