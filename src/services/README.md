# `src/services/`

Folder ini menyimpan **logika bisnis dan akses data** yang memisahkan Route Handlers dan Server Actions dari detail implementasi (database, API eksternal, dsb.).

## Kegunaan

- Menampung query database, pemanggilan API eksternal, dan operasi bisnis utama
- Membuat Route Handlers dan Server Actions tetap tipis — hanya menerima input, memanggil service, dan mengembalikan respons
- Berbeda dengan `src/lib/` yang berisi konfigurasi dan helper umum, `services/` berisi logika spesifik domain aplikasi

## Struktur yang Disarankan

```
src/services/
├── user.service.js     # Operasi terkait user (getUser, createUser, dsb.)
├── auth.service.js     # Logika autentikasi (login, register, token)
├── post.service.js     # Operasi terkait konten/postingan
└── upload.service.js   # Penanganan upload file
```

## Contoh Penggunaan

**`src/services/user.service.js`**

```js
import prisma from '@/lib/prisma';

export async function getUserById(id) {
  return prisma.user.findUnique({ where: { id } });
}

export async function getAllUsers({ page = 1, limit = 10 } = {}) {
  const [items, totalItems] = await Promise.all([
    prisma.user.findMany({
      skip: (page - 1) * limit,
      take: limit,
      orderBy: { createdAt: 'desc' },
    }),
    prisma.user.count(),
  ]);

  return {
    items,
    pagination: {
      page,
      limit,
      totalItems,
      totalPages: Math.ceil(totalItems / limit),
    },
  };
}

export async function createUser(data) {
  return prisma.user.create({ data });
}
```

**Dipanggil dari Route Handler (`app/api/users/route.js`):**

```js
import { getAllUsers, createUser } from '@/services/user.service';

export async function GET(request) {
  const { searchParams } = new URL(request.url);
  const result = await getAllUsers({
    page: Number(searchParams.get('page') ?? 1),
    limit: Number(searchParams.get('limit') ?? 10),
  });

  return Response.json({
    success: true,
    data: result,
    message: 'OK',
    error: null,
  });
}

export async function POST(request) {
  const body = await request.json();
  const user = await createUser(body);

  return Response.json(
    { success: true, data: user, message: 'OK', error: null },
    { status: 201 }
  );
}
```

**Dipanggil dari Server Action (`app/users/actions.js`):**

```js
'use server';

import { createUser } from '@/services/user.service';
import { revalidatePath } from 'next/cache';

export async function createUserAction(formData) {
  const data = { name: formData.get('name'), email: formData.get('email') };
  await createUser(data);
  revalidatePath('/users');
  return { success: true };
}
```
