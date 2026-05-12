# `src/lib/`

Folder ini menyimpan **konfigurasi library pihak ketiga dan helper umum** yang tidak spesifik pada domain bisnis aplikasi.

> **Penting:** Berbeda dengan `src/services/` yang berisi logika bisnis spesifik domain, `src/lib/` berisi setup dan utilitas teknis yang mendukung infrastruktur aplikasi — seperti koneksi database, konfigurasi auth, atau inisialisasi ORM.

## Kegunaan

- Inisialisasi dan konfigurasi library (Prisma, NextAuth, Nodemailer, dsb.)
- Helper teknis yang dipakai lintas domain (format tanggal, enkripsi, dsb.)
- Singleton instance yang perlu diinisialisasi sekali (koneksi DB, client S3)

## Struktur yang Disarankan

```
src/lib/
├── prisma.js           # Singleton Prisma client
├── auth.js             # Konfigurasi NextAuth (providers, callbacks)
├── mail.js             # Konfigurasi Nodemailer / Resend
└── cloudinary.js       # Konfigurasi Cloudinary untuk upload
```

## Contoh Penggunaan

**`src/lib/prisma.js`** — Singleton Prisma agar tidak membuat koneksi baru di tiap request (dev mode):

```js
import { PrismaClient } from '@prisma/client';

const globalForPrisma = globalThis;

const prisma = globalForPrisma.prisma ?? new PrismaClient();

if (process.env.NODE_ENV !== 'production') globalForPrisma.prisma = prisma;

export default prisma;
```

**`src/lib/mail.js`**

```js
import nodemailer from 'nodemailer';

const transporter = nodemailer.createTransport({
  host: process.env.MAIL_HOST,
  port: Number(process.env.MAIL_PORT),
  auth: {
    user: process.env.MAIL_USER,
    pass: process.env.MAIL_PASS,
  },
});

export async function sendMail({ to, subject, html }) {
  return transporter.sendMail({
    from: `"${process.env.MAIL_FROM_NAME}" <${process.env.MAIL_FROM}>`,
    to,
    subject,
    html,
  });
}
```

**Penggunaan di service:**

```js
import prisma from '@/lib/prisma';
import { sendMail } from '@/lib/mail';

export async function createUser(data) {
  const user = await prisma.user.create({ data });
  await sendMail({
    to: user.email,
    subject: 'Selamat datang!',
    html: '<p>Akun berhasil dibuat.</p>',
  });
  return user;
}
```
