# `src/utils/`

Folder ini menyimpan **fungsi utilitas murni (pure functions)** yang tidak memiliki side effect dan tidak bergantung pada state atau library besar.

> **Penting:** Berbeda dengan `src/lib/` yang berisi konfigurasi library dan singleton, `src/utils/` berisi fungsi-fungsi kecil yang bisa dipanggil kapan saja tanpa inisialisasi — seperti formatter, helper string, atau kalkulasi matematis.

## Kegunaan

- Fungsi pembantu yang digunakan berulang di banyak bagian aplikasi
- Transformasi data, format tampilan, dan kalkulasi sederhana
- Tidak boleh mengandung logika bisnis domain — itu masuk ke `src/services/`

## Struktur yang Disarankan

```
src/utils/
├── format.js           # Format angka, tanggal, mata uang
├── string.js           # Manipulasi string (slugify, truncate, capitalize)
├── array.js            # Helper array (groupBy, unique, chunk)
└── cn.js               # Penggabungan class Tailwind (clsx + tailwind-merge)
```

## Contoh Penggunaan

**`src/utils/format.js`**

```js
export function formatCurrency(amount, currency = 'IDR') {
  return new Intl.NumberFormat('id-ID', {
    style: 'currency',
    currency,
    minimumFractionDigits: 0,
  }).format(amount);
}

export function formatDate(date, options = {}) {
  return new Intl.DateTimeFormat('id-ID', {
    day: 'numeric',
    month: 'long',
    year: 'numeric',
    ...options,
  }).format(new Date(date));
}
```

**`src/utils/string.js`**

```js
export function slugify(str) {
  return str
    .toLowerCase()
    .trim()
    .replace(/[^\w\s-]/g, '')
    .replace(/[\s_-]+/g, '-')
    .replace(/^-+|-+$/g, '');
}

export function truncate(str, maxLength = 100) {
  if (str.length <= maxLength) return str;
  return str.slice(0, maxLength).trimEnd() + '…';
}
```

**`src/utils/cn.js`**

```js
import { clsx } from 'clsx';
import { twMerge } from 'tailwind-merge';

export function cn(...inputs) {
  return twMerge(clsx(inputs));
}
```

**Penggunaan di komponen:**

```jsx
import { formatCurrency, formatDate } from '@/utils/format';
import { cn } from '@/utils/cn';

export default function ProductCard({ product, isActive }) {
  return (
    <div className={cn('rounded border p-4', isActive && 'border-blue-500')}>
      <p>{product.name}</p>
      <p>{formatCurrency(product.price)}</p>
      <p>{formatDate(product.createdAt)}</p>
    </div>
  );
}
```
