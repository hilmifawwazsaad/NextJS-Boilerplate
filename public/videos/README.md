# `public/videos/`

Folder ini menyimpan **file video statis** yang dapat diakses langsung melalui URL tanpa diproses oleh bundler.

> **Penting:** File di dalam `public/` dapat diakses via URL root (`/videos/intro.mp4`). Next.js tidak melakukan optimasi otomatis untuk video seperti yang dilakukan pada gambar. Untuk video berukuran besar, pertimbangkan menggunakan layanan hosting video eksternal (YouTube, Vimeo, Cloudinary, dsb.).

## Kegunaan

- Video pendek untuk hero section, background, atau demonstrasi fitur
- File video yang perlu URL statis dan dapat diputar langsung di browser
- Animasi video (format `.webm` atau `.mp4`) sebagai alternatif GIF berukuran besar

## Struktur yang Disarankan

```
public/videos/
├── hero-bg.mp4             # Video background untuk halaman utama
├── demo.mp4                # Video demo/tutorial aplikasi
└── onboarding.webm         # Video onboarding untuk pengguna baru
```

## Contoh Penggunaan

**Video background (autoplay, muted, loop):**

```jsx
export default function HeroSection() {
  return (
    <div className="relative h-screen">
      <video
        autoPlay
        muted
        loop
        playsInline
        className="absolute inset-0 h-full w-full object-cover"
      >
        <source src="/videos/hero-bg.webm" type="video/webm" />
        <source src="/videos/hero-bg.mp4" type="video/mp4" />
      </video>
      <div className="relative z-10">{/* konten di atas video */}</div>
    </div>
  );
}
```

**Video dengan kontrol:**

```jsx
export default function DemoVideo() {
  return (
    <video controls width="100%" poster="/images/thumbnails/demo-poster.jpg">
      <source src="/videos/demo.mp4" type="video/mp4" />
      Browser Anda tidak mendukung pemutaran video.
    </video>
  );
}
```

## Catatan

- Selalu sediakan dua format: `.webm` (lebih kecil, didukung browser modern) dan `.mp4` (fallback)
- Kompres video sebelum dimasukkan — ukuran besar memperlambat loading halaman
- Untuk video > 10 MB, gunakan layanan eksternal (Cloudinary, Mux, YouTube embed) agar tidak membebani repository dan CDN
- Tambahkan atribut `playsInline` agar video autoplay berfungsi di iOS Safari
