# `src/constants/`

Folder ini menyimpan **nilai-nilai tetap (konstanta)** yang tidak berubah selama aplikasi berjalan dan tidak bergantung pada environment variables.

> **Penting:** Berbeda dengan `src/config/` yang membaca environment variables, `src/constants/` hanya berisi nilai literal statis yang didefinisikan langsung dalam kode.

## Kegunaan

- Menyimpan nilai enum, label, kode status, atau opsi dropdown yang dipakai di banyak tempat
- Menghindari magic number/string yang tersebar di seluruh kode
- Memudahkan perubahan nilai terpusat tanpa perlu mencari satu per satu

## Struktur yang Disarankan

```
src/constants/
├── routes.js           # Path URL halaman aplikasi
├── roles.js            # Peran/role pengguna
├── status.js           # Status order, transaksi, dsb.
└── options.js          # Opsi dropdown atau pilihan form
```

## Contoh Penggunaan

**`src/constants/routes.js`**

```js
export const ROUTES = {
  HOME: '/',
  LOGIN: '/login',
  REGISTER: '/register',
  DASHBOARD: '/dashboard',
  PROFILE: '/profile',
  USERS: '/dashboard/users',
};
```

**`src/constants/roles.js`**

```js
export const ROLES = {
  ADMIN: 'admin',
  USER: 'user',
  MODERATOR: 'moderator',
};
```

**`src/constants/status.js`**

```js
export const ORDER_STATUS = {
  PENDING: 'pending',
  PROCESSING: 'processing',
  SHIPPED: 'shipped',
  DELIVERED: 'delivered',
  CANCELLED: 'cancelled',
};
```

**Penggunaan:**

```js
import { ROUTES } from '@/constants/routes';
import { ROLES } from '@/constants/roles';

// Di komponen navigasi
<Link href={ROUTES.DASHBOARD}>Dashboard</Link>;

// Di guard/middleware
if (user.role !== ROLES.ADMIN) redirect(ROUTES.HOME);
```
