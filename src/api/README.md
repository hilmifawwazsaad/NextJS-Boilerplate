# `src/api/`

Folder ini menyimpan **fungsi-fungsi untuk melakukan HTTP request** ke API eksternal atau backend server.

> **Penting:** Folder ini berbeda dengan `app/api/` yang digunakan untuk membuat **API Route Handlers** di Next.js (endpoint server-side). Folder `src/api/` khusus untuk kode sisi klien yang memanggil API dari luar.

## Kegunaan

- Fungsi fetch/axios untuk memanggil REST API atau GraphQL eksternal
- Pengelompokan request berdasarkan resource (auth, users, products, dll.)
- Konfigurasi base URL, headers, dan interceptors

## Struktur yang Disarankan

```
src/api/
├── axiosInstance.js    # Konfigurasi axios (base URL, interceptor, token)
├── auth.js             # Fungsi API untuk autentikasi
├── users.js            # Fungsi API untuk data user
└── products.js         # Fungsi API untuk produk
```

## Contoh Penggunaan

**`src/api/axiosInstance.js`**

```js
import axios from 'axios';

const api = axios.create({
  baseURL: process.env.NEXT_PUBLIC_API_URL,
  headers: {
    'Content-Type': 'application/json',
  },
});

// Interceptor untuk menyisipkan token otomatis
api.interceptors.request.use((config) => {
  const token = localStorage.getItem('token');
  if (token) config.headers.Authorization = `Bearer ${token}`;
  return config;
});

export default api;
```

**`src/api/auth.js`**

```js
import api from './axiosInstance';

export const login = (credentials) => api.post('/auth/login', credentials);
export const logout = () => api.post('/auth/logout');
export const getProfile = () => api.get('/auth/me');
```

**Penggunaan di komponen:**

```js
import { login } from '@/api/auth';

const handleLogin = async (formData) => {
  const response = await login({
    email: formData.email,
    password: formData.password,
  });
  console.log(response.data);
};
```
