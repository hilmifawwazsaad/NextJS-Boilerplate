# `src/types/`

Folder ini menyimpan **definisi tipe TypeScript** yang digunakan lintas aplikasi — interface, type alias, enum, dan deklarasi tipe global.

> **Penting:** Tipe yang hanya relevan untuk satu file atau komponen cukup didefinisikan di file tersebut. Folder `src/types/` khusus untuk tipe yang dipakai di banyak tempat atau yang perlu dijaga konsistensinya di seluruh codebase.

## Kegunaan

- Mendefinisikan model data utama (User, Product, Order, dsb.) sebagai interface terpusat
- Menyimpan tipe untuk respons API agar konsisten antara client dan server
- Mendefinisikan tipe utilitas atau global yang dipakai di mana-mana

## Struktur yang Disarankan

```
src/types/
├── user.types.ts       # Tipe untuk entitas user
├── api.types.ts        # Tipe respons API umum (ApiResponse, Pagination)
├── auth.types.ts       # Tipe untuk sesi dan autentikasi
└── index.ts            # Re-export semua tipe (opsional)
```

## Contoh Penggunaan

**`src/types/user.types.ts`**

```ts
export interface User {
  id: string;
  name: string;
  email: string;
  role: 'admin' | 'user' | 'moderator';
  createdAt: string;
}

export type CreateUserPayload = Pick<User, 'name' | 'email'> & {
  password: string;
};

export type UpdateUserPayload = Partial<Pick<User, 'name' | 'email'>>;
```

**`src/types/api.types.ts`**

```ts
export interface ApiResponse<T = unknown> {
  success: boolean;
  data: T;
  message: string;
  error: string | null;
}

export interface PaginatedData<T> {
  items: T[];
  pagination: {
    page: number;
    limit: number;
    totalItems: number;
    totalPages: number;
  };
}
```

**Penggunaan di service dan komponen:**

```ts
import type { User, CreateUserPayload } from '@/types/user.types';
import type { ApiResponse, PaginatedData } from '@/types/api.types';

export async function getAllUsers(): Promise<ApiResponse<PaginatedData<User>>> {
  const res = await fetch('/api/users');
  return res.json();
}
```
