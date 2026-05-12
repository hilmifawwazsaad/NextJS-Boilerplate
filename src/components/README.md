# `src/components/`

Folder ini menyimpan **komponen React yang dapat digunakan kembali** di seluruh aplikasi.

> **Penting:** Komponen di sini bersifat umum dan tidak terikat pada halaman tertentu. Komponen yang hanya digunakan di satu halaman sebaiknya diletakkan langsung di dalam folder halaman tersebut (`app/(route)/_components/`).

## Kegunaan

- Menyimpan UI komponen yang digunakan di banyak tempat (Button, Modal, Input, dsb.)
- Mengelompokkan komponen berdasarkan kategori (ui, layout, form, dsb.)
- Memisahkan tampilan dari logika bisnis

## Struktur yang Disarankan

```
src/components/
├── ui/
│   ├── Button.jsx          # Komponen tombol
│   ├── Modal.jsx           # Komponen modal/dialog
│   └── Badge.jsx           # Komponen badge/label
├── layout/
│   ├── Navbar.jsx          # Navigasi utama
│   ├── Sidebar.jsx         # Sidebar navigasi
│   └── Footer.jsx          # Footer halaman
└── form/
    ├── InputField.jsx      # Input teks dengan label dan error
    └── SelectField.jsx     # Dropdown select dengan label
```

## Contoh Penggunaan

**`src/components/ui/Button.jsx`**

```jsx
export default function Button({
  children,
  variant = 'primary',
  onClick,
  disabled,
}) {
  const base = 'px-4 py-2 rounded font-medium transition-colors';
  const variants = {
    primary: 'bg-blue-600 text-white hover:bg-blue-700',
    secondary: 'bg-gray-200 text-gray-800 hover:bg-gray-300',
    danger: 'bg-red-600 text-white hover:bg-red-700',
  };

  return (
    <button
      onClick={onClick}
      disabled={disabled}
      className={`${base} ${variants[variant]} disabled:opacity-50`}
    >
      {children}
    </button>
  );
}
```

**Penggunaan di halaman:**

```jsx
import Button from '@/components/ui/Button';

export default function Page() {
  return (
    <div>
      <Button variant='primary' onClick={() => console.log('clicked')}>
        Simpan
      </Button>
      <Button variant='danger'>Hapus</Button>
    </div>
  );
}
```
