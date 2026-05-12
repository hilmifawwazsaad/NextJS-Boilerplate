# `src/contexts/`

Folder ini menyimpan **React Context** beserta Provider-nya untuk state global yang perlu diakses banyak komponen tanpa prop drilling.

> **Penting:** Gunakan Context untuk state yang benar-benar global (auth, tema, bahasa). Untuk state yang hanya dipakai di satu fitur, pertimbangkan state lokal atau state management yang lebih ringan.

## Kegunaan

- Menyediakan state global seperti data user yang sedang login, tema, atau preferensi bahasa
- Menghindari prop drilling yang panjang antar komponen
- Membungkus Provider agar dapat digunakan di seluruh aplikasi melalui layout

## Struktur yang Disarankan

```
src/contexts/
├── AuthContext.jsx      # Context untuk autentikasi (user, login, logout)
├── ThemeContext.jsx     # Context untuk tema (light/dark mode)
└── CartContext.jsx      # Context untuk keranjang belanja
```

## Contoh Penggunaan

**`src/contexts/AuthContext.jsx`**

```jsx
'use client';

import { createContext, useContext, useState } from 'react';

const AuthContext = createContext(null);

export function AuthProvider({ children }) {
  const [user, setUser] = useState(null);

  const login = (userData) => setUser(userData);
  const logout = () => setUser(null);

  return (
    <AuthContext.Provider value={{ user, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
}

export function useAuth() {
  const ctx = useContext(AuthContext);
  if (!ctx) throw new Error('useAuth must be used within AuthProvider');
  return ctx;
}
```

**Didaftarkan di layout (`app/layout.jsx`):**

```jsx
import { AuthProvider } from '@/contexts/AuthContext';

export default function RootLayout({ children }) {
  return (
    <html lang='id'>
      <body>
        <AuthProvider>{children}</AuthProvider>
      </body>
    </html>
  );
}
```

**Digunakan di komponen:**

```jsx
import { useAuth } from '@/contexts/AuthContext';

export default function Navbar() {
  const { user, logout } = useAuth();

  return (
    <nav>
      <span>Halo, {user?.name ?? 'Tamu'}</span>
      {user && <button onClick={logout}>Keluar</button>}
    </nav>
  );
}
```
