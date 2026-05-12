# `src/config/`

Folder ini menyimpan **nilai konfigurasi aplikasi** yang bersumber dari environment variables atau konstanta yang memerlukan pemrosesan ringan.

> **Penting:** Berbeda dengan `src/constants/` yang menyimpan nilai statis murni, `src/config/` membaca `process.env` dan mengekspornya dalam bentuk yang terstruktur sehingga mudah digunakan di seluruh aplikasi.

## Kegunaan

- Membaca dan mengekspos environment variables secara terpusat
- Konfigurasi koneksi database, pihak ketiga, atau fitur aplikasi
- Menghindari penggunaan `process.env.XYZ` secara langsung di banyak tempat

## Struktur yang Disarankan

```
src/config/
├── app.config.js       # Konfigurasi umum aplikasi (nama, URL, environment)
├── db.config.js        # Konfigurasi database (URL, pool size)
├── auth.config.js      # Konfigurasi autentikasi (secret, provider OAuth)
└── storage.config.js   # Konfigurasi penyimpanan file (S3, Cloudinary)
```

## Contoh Penggunaan

**`src/config/app.config.js`**

```js
export const appConfig = {
  name: process.env.NEXT_PUBLIC_APP_NAME ?? 'My App',
  url: process.env.NEXT_PUBLIC_APP_URL ?? 'http://localhost:3000',
  isDev: process.env.NODE_ENV === 'development',
  isProd: process.env.NODE_ENV === 'production',
};
```

**`src/config/auth.config.js`**

```js
export const authConfig = {
  secret: process.env.NEXTAUTH_SECRET,
  google: {
    clientId: process.env.GOOGLE_CLIENT_ID,
    clientSecret: process.env.GOOGLE_CLIENT_SECRET,
  },
  tokenExpiry: 60 * 60 * 24 * 7, // 7 hari dalam detik
};
```

**Penggunaan:**

```js
import { appConfig } from '@/config/app.config';
import { authConfig } from '@/config/auth.config';

console.log(`Running: ${appConfig.name} (${appConfig.isDev ? 'dev' : 'prod'})`);
console.log(`Token expiry: ${authConfig.tokenExpiry}s`);
```
