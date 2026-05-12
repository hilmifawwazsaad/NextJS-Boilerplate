# `public/images/`

Folder ini menyimpan **file gambar statis** yang dapat diakses langsung melalui URL tanpa diproses oleh bundler.

> **Penting:** File di dalam `public/` dapat diakses via URL root (`/images/foto.jpg`), bukan melalui `import`. Untuk gambar yang dioptimasi secara otomatis oleh Next.js, gunakan komponen `<Image>` dari `next/image` dan letakkan file di sini atau gunakan URL eksternal.

## Kegunaan

- Menyimpan gambar yang direferensikan langsung di HTML/CSS (og:image, favicon, dsb.)
- Gambar yang perlu URL statis dan tidak berubah-ubah
- Asset gambar yang digunakan bersama komponen `<Image>` dari `next/image`

## Struktur yang Disarankan

```
public/images/
├── og/
│   └── og-default.jpg      # Open Graph image untuk social media preview
├── icons/
│   ├── logo.svg            # Logo aplikasi
│   └── favicon.ico         # Favicon (bisa juga di root public/)
└── placeholders/
    └── avatar.png          # Placeholder untuk foto profil
```

## Contoh Penggunaan

**Dengan komponen `<Image>` (direkomendasikan):**

```jsx
import Image from 'next/image';

export default function Avatar({ src, name }) {
  return (
    <Image
      src={src ?? '/images/placeholders/avatar.png'}
      alt={name}
      width={40}
      height={40}
      className="rounded-full"
    />
  );
}
```

**Sebagai Open Graph image di metadata:**

```js
// app/layout.js
export const metadata = {
  openGraph: {
    images: [{ url: '/images/og/og-default.jpg', width: 1200, height: 630 }],
  },
};
```

**Di CSS (background image):**

```css
.hero {
  background-image: url('/images/hero-bg.jpg');
}
```

## Catatan

- Hindari menyimpan gambar berukuran besar tanpa kompresi terlebih dahulu
- Format modern (`.webp`, `.avif`) lebih direkomendasikan untuk performa
- Komponen `<Image>` dari `next/image` otomatis melakukan lazy loading, resize, dan konversi format
