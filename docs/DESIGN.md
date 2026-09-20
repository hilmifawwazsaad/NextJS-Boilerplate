# DESIGN — Prinsip UI/UX Next.js TSX Boilerplate

Boilerplate ini **tidak** menyediakan komponen visual jadi atau design system
final — halaman default masih scaffold bawaan `create-next-app`
(`src/app/page.tsx`, `src/app/globals.css`). Dokumen ini berisi prinsip desain
yang harus diikuti begitu proyek turunan mulai membangun UI sungguhan, baik
oleh developer manual maupun AI coding agent. Sumber aturan lengkap:
`.agents/frontend-design/SKILL.md`.

## 1. Status Saat Ini

| Aspek          | Kondisi sekarang                                                                                              |
| -------------- | ------------------------------------------------------------------------------------------------------------- |
| Font           | Geist Sans / Geist Mono (default `create-next-app`)                                                           |
| Warna          | `--background` / `--foreground` monokrom, mengikuti `prefers-color-scheme`                                    |
| Komponen       | Belum ada — folder `components/ui`, `components/layout`, `components/form` masih berupa contoh di `README.md` |
| Tone/aesthetic | Belum ditentukan — wajib dipilih di awal proyek turunan (lihat §2)                                            |

Token di `src/app/globals.css` ini adalah **placeholder**, bukan keputusan
desain final. Ganti begitu proyek turunan menentukan arah visualnya sendiri.

## 2. Proses Berpikir Sebelum Membangun UI

Sebelum menulis kode komponen, tentukan dulu:

1. **Purpose** — masalah apa yang diselesaikan UI ini, siapa penggunanya?
2. **Tone** — pilih satu dan konsisten: minimal · editorial · playful ·
   brutalist · luxury · retro-futuristic · organic · industrial · soft ·
   geometric.
3. **Differentiation** — satu hal yang akan diingat pengguna dari UI ini.

Jangan konvergen ke pilihan "aman" yang generik antar proyek berbeda.

## 3. Tipografi

- Jangan pakai Inter, Roboto, Arial, atau system font sebagai default.
- Pilih font display yang berkarakter, dipadukan dengan body font yang lebih netral.
- Font adalah keputusan desain pertama — menentukan arah estetika keseluruhan.

## 4. Warna & Komposisi

- Rasio **60-30-10**: warna dominan (60%) · sekunder (30%) · aksen (10%).
- Setiap warna dipetakan ke peran semantik: `background`, `surface`,
  `foreground`, `muted`, `primary`, `accent`, `destructive`, `success` —
  jangan pakai hex mentah ad hoc di komponen.
- Kontras minimum WCAG AA: teks normal ≥ 4.5:1, elemen UI ≥ 3:1.
- Komit ke light atau dark sebagai identitas — jangan default ke abu-abu netral.
- Layout boleh asimetris/overlap/diagonal — tidak harus grid simetris.
- Pilih salah satu: negative space luas, atau densitas terkontrol — eksekusi penuh, jangan setengah-setengah.

## 5. Spatial Design (8-Point Grid)

- Semua spacing, sizing, layout **wajib kelipatan 4px**, utamakan kelipatan 8px.
- Berlaku untuk: padding, margin, gap, width, height, border-radius, ukuran ikon.
- Minimum touch target: 44×44px.
- Border radius: pilih satu skala per proyek (`4 · 8 · 12 · 16 · 24 · 9999px`) dan pakai konsisten di semua komponen.

## 6. Prinsip UI/UX

- **Hierarchy** — satu primary CTA per tampilan; gunakan ukuran + kontras + weight untuk menandakan prioritas.
- **Proximity** — elemen yang berkaitan diletakkan lebih dekat dibanding yang tidak berkaitan.
- **Consistency** — komponen yang sama tampil sama di mana pun; satu gaya ikon (outline atau filled), jangan dicampur.
- **Empat state wajib** di setiap elemen interaktif: ideal · loading (skeleton lebih diutamakan daripada spinner) · empty (dengan langkah lanjutan yang jelas) · error (spesifik & bisa dipulihkan).
- **Accessibility** — bisa dinavigasi keyboard dengan focus ring terlihat; `aria-label` pada tombol ikon-saja; bungkus animasi dengan `prefers-reduced-motion`.
- **Responsive** — mobile-first; tipografi/spacing fluid dengan `clamp()`.

## 7. Motion

Fokus pada momen berdampak tinggi: staggered reveal saat page load, hover
state yang mengejutkan, transisi scroll-triggered. Satu entrance yang
dirancang matang lebih berdampak daripada micro-interaction yang tersebar di
mana-mana. Pilih tool motion sesuai kompleksitas visi desain (CSS animation,
Framer Motion, GSAP, dst.).

## 8. Detail Visual

Tambahkan kedalaman lewat gradient mesh, noise texture, pola geometris,
layered transparency, shadow dramatis, atau border dekoratif — sesuaikan
dengan tone yang sudah dipilih. Jangan tambahkan tekstur yang bertentangan
dengan arah estetika yang sudah dikomit.

## 9. Jangan Lakukan

- Estetika AI generik: Inter/Roboto/Arial, gradient ungu-di-atas-putih, layout kartu klise.
- Pilihan visual yang bertentangan dengan tone yang sudah dikomit di awal.
- Menyamakan estetika antar proyek berbeda yang dibangun dari boilerplate ini.
- `outline: none` tanpa pengganti focus indicator yang terlihat.
- Warna sebagai satu-satunya penanda makna/status (harus dibarengi ikon/teks).
