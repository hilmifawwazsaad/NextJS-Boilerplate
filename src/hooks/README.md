# `src/hooks/`

Folder ini menyimpan **custom React hooks** yang mengenkapsulasi logika stateful yang dapat digunakan kembali di banyak komponen.

> **Penting:** Custom hook harus dimulai dengan prefix `use` dan hanya boleh dipanggil dari dalam komponen React atau custom hook lainnya. Hook di sini bersifat umum; hook yang spesifik untuk satu halaman dapat diletakkan di dekat halaman tersebut.

## Kegunaan

- Mengekstrak logika stateful dari komponen agar lebih bersih dan mudah diuji
- Menggabungkan beberapa hook bawaan React atau library menjadi satu antarmuka yang simpel
- Berbagi logika yang sama (fetch data, form, timer, dsb.) antar banyak komponen

## Struktur yang Disarankan

```
src/hooks/
├── useDebounce.js      # Menunda eksekusi nilai hingga user berhenti mengetik
├── useLocalStorage.js  # Sinkronisasi state dengan localStorage
├── useMediaQuery.js    # Deteksi breakpoint responsive
└── useFetch.js         # Wrapper fetch dengan state loading/error/data
```

## Contoh Penggunaan

**`src/hooks/useDebounce.js`**

```js
import { useState, useEffect } from 'react';

export function useDebounce(value, delay = 300) {
  const [debounced, setDebounced] = useState(value);

  useEffect(() => {
    const timer = setTimeout(() => setDebounced(value), delay);
    return () => clearTimeout(timer);
  }, [value, delay]);

  return debounced;
}
```

**`src/hooks/useLocalStorage.js`**

```js
import { useState } from 'react';

export function useLocalStorage(key, initialValue) {
  const [storedValue, setStoredValue] = useState(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch {
      return initialValue;
    }
  });

  const setValue = (value) => {
    setStoredValue(value);
    window.localStorage.setItem(key, JSON.stringify(value));
  };

  return [storedValue, setValue];
}
```

**Penggunaan di komponen:**

```jsx
import { useDebounce } from '@/hooks/useDebounce';

export default function SearchBar() {
  const [query, setQuery] = useState('');
  const debouncedQuery = useDebounce(query, 500);

  useEffect(() => {
    if (debouncedQuery) fetchResults(debouncedQuery);
  }, [debouncedQuery]);

  return <input value={query} onChange={(e) => setQuery(e.target.value)} />;
}
```
