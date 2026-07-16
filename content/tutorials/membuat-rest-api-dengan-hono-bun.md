+++
title = 'Membuat REST API dengan Hono + Bun'
description = 'Panduan lengkap membuat REST API dari nol menggunakan Hono framework dan Bun runtime. Cocok untuk pemula yang ingin belajar backend modern.'
slug = 'membuat-rest-api-dengan-hono-bun'
date = '2026-07-16T08:45:00+07:00'
lastmod = '2026-07-16T08:45:00+07:00'
draft = false
categories = ['Tutorials']
tags = ['api', 'server', 'nginx', 'ssl', 'hono', 'bun', 'vibecoding', 'pemula', 'devops']
author = 'GezyTech'
featured = true
cover = ''
summary = 'Pelajari cara membuat REST API dari nol menggunakan Hono framework dan Bun runtime. CRUD lengkap dengan TypeScript, middleware, validasi, dan deploy.'
+++

## TLDR

Hono adalah framework web ringan yang dirancang untuk runtime modern seperti Bun, Deno, dan Cloudflare Workers. Kombinasinya dengan Bun — runtime JavaScript tercepat saat ini — menghasilkan REST API yang bisa ditulis dalam hitungan menit. Artikel ini akan memandumu dari instalasi hingga deploy REST API lengkap dengan CRUD, validasi, dan koneksi database.

---

## Pendahuluan

Membangun REST API dulu terasa berat: setup Express, TypeScript, ts-node, Nodemon, dan berbagai middleware yang harus dikonfigurasi satu per satu. Prosesnya bisa memakan waktu 30 menit sebelum kamu benar-benar menulis kode bisnis.

Dengan Bun + Hono, semua itu berubah.

Bun adalah runtime all-in-one yang menjalankan TypeScript langsung tanpa konfigurasi, package manager cepat bawaan, dan test runner terintegrasi. Hono adalah framework web minimalis dengan performa tinggi — bobotnya hanya 14 KB dan tidak punya dependensi eksternal.

Gabungan keduanya memungkinkan kamu membuat REST API dari nol hingga selesai dalam waktu kurang dari 10 menit. Cocok untuk kamu yang suka vibe coding dan ingin hasil cepat.

---

## Konten Utama

### 1. Kenapa Hono + Bun?

| Fitur | Express + Node.js | Hono + Bun |
|-------|------------------|-------------|
| Setup awal | 10-15 menit (config TypeScript, ts-node, dll) | **30 detik** (bun init + bun add hono) |
| Ukuran bundle | ~500 KB + middleware | **~14 KB** (zero dependencies) |
| Kecepatan | Standar | **3-5x lebih cepat** |
| TypeScript | Perlu tsconfig + ts-node | **Native** (jalan langsung) |
| Middleware | body-parser, cors terpisah | **Built-in** (cors, json, dll) |
| Deploy | Butuh setup PM2/forever | **Satu perintah** (bun run) |
| Edge ready | Tidak | Ya (Cloudflare Workers, Deno) |

### 2. Inisialisasi Proyek

```bash
# Buat folder proyek
mkdir hono-bun-api
cd hono-bun-api

# Inisialisasi
bun init

# Install Hono
bun add hono
```

Hasil `bun init` akan membuat:
- `package.json`
- `tsconfig.json`
- `index.ts` (entry point)

### 3. Hello World Pertama

Buka `index.ts` dan tulis:

```typescript
import { Hono } from 'hono';

const app = new Hono();

app.get('/', (c) => {
  return c.text('Hello dari Hono + Bun!');
});

export default app;
```

Jalankan:

```bash
bun run index.ts
```

Akses `http://localhost:3000` — lihat? REST API pertamamu sudah hidup dalam 2 menit! 🚀

### 4. REST API CRUD Lengkap

Mari kita buat API untuk mengelola data **buku perpustakaan** dengan operasi CRUD lengkap.

```typescript
import { Hono } from 'hono';
import { cors } from 'hono/cors';

const app = new Hono();

// Middleware CORS
app.use('/*', cors());

// In-memory database (nanti bisa diganti dengan database sungguhan)
interface Book {
  id: number;
  title: string;
  author: string;
  year: number;
  is_read: boolean;
}

let books: Book[] = [
  { id: 1, title: 'Pemrograman Web dengan Bun', author: 'Budi', year: 2025, is_read: false },
  { id: 2, title: 'Belajar Docker untuk Developer', author: 'Ani', year: 2026, is_read: true },
];

let nextId = 3;

// CREATE - Tambah buku baru
app.post('/api/books', async (c) => {
  const body = await c.req.json();

  const book: Book = {
    id: nextId++,
    title: body.title,
    author: body.author,
    year: body.year,
    is_read: body.is_read ?? false,
  };

  books.push(book);
  return c.json(book, 201);
});

// READ ALL - Ambil semua buku
app.get('/api/books', (c) => {
  // Filter query parameters?search=
  const search = c.req.query('search');
  if (search) {
    const filtered = books.filter(
      (b) =>
        b.title.toLowerCase().includes(search.toLowerCase()) ||
        b.author.toLowerCase().includes(search.toLowerCase()),
    );
    return c.json(filtered);
  }
  return c.json(books);
});

// READ ONE - Ambil satu buku berdasarkan ID
app.get('/api/books/:id', (c) => {
  const id = Number(c.req.param('id'));
  const book = books.find((b) => b.id === id);

  if (!book) {
    return c.json({ message: 'Buku tidak ditemukan' }, 404);
  }

  return c.json(book);
});

// UPDATE - Ubah data buku
app.put('/api/books/:id', async (c) => {
  const id = Number(c.req.param('id'));
  const index = books.findIndex((b) => b.id === id);

  if (index === -1) {
    return c.json({ message: 'Buku tidak ditemukan' }, 404);
  }

  const body = await c.req.json();

  books[index] = {
    ...books[index],
    title: body.title ?? books[index].title,
    author: body.author ?? books[index].author,
    year: body.year ?? books[index].year,
    is_read: body.is_read ?? books[index].is_read,
  };

  return c.json(books[index]);
});

// DELETE - Hapus buku
app.delete('/api/books/:id', (c) => {
  const id = Number(c.req.param('id'));
  const index = books.findIndex((b) => b.id === id);

  if (index === -1) {
    return c.json({ message: 'Buku tidak ditemukan' }, 404);
  }

  books.splice(index, 1);
  return c.json({ message: 'Buku berhasil dihapus' });
});

export default app;
```

### 5. Menguji API dengan curl

Buka terminal lain dan coba:

```bash
# GET all books
curl http://localhost:3000/api/books

# GET search
curl "http://localhost:3000/api/books?search=docker"

# GET single book
curl http://localhost:3000/api/books/1

# POST new book
curl -X POST http://localhost:3000/api/books \
  -H "Content-Type: application/json" \
  -d '{"title":"Matematika Kelas 8","author":"Gunanto","year":2026,"is_read":false}'

# PUT update book
curl -X PUT http://localhost:3000/api/books/1 \
  -H "Content-Type: application/json" \
  -d '{"is_read":true}'

# DELETE book
curl -X DELETE http://localhost:3000/api/books/2
```

### 6. Validasi dengan Zod

Agar API lebih aman, tambahkan validasi menggunakan **Zod**:

```bash
bun add zod
```

```typescript
import { z } from 'zod';

const BookSchema = z.object({
  title: z.string().min(1, 'Judul tidak boleh kosong'),
  author: z.string().min(1, 'Penulis tidak boleh kosong'),
  year: z.number().int().min(1900).max(2030),
  is_read: z.boolean().optional(),
});

app.post('/api/books', async (c) => {
  const body = await c.req.json();
  const result = BookSchema.safeParse(body);

  if (!result.success) {
    return c.json({ 
      message: 'Validasi gagal', 
      errors: result.error.flatten().fieldErrors 
    }, 400);
  }

  // Lanjutkan simpan data...
});
```

### 7. Koneksi ke Database SQLite

Untuk data yang persisten, kita bisa tambahkan SQLite menggunakan **Bun SQLite** (built-in, tanpa package tambahan):

```typescript
import { Database } from 'bun:sqlite';

// Buat database
const db = new Database('library.db');

// Buat tabel
db.run(`
  CREATE TABLE IF NOT EXISTS books (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    title TEXT NOT NULL,
    author TEXT NOT NULL,
    year INTEGER NOT NULL,
    is_read INTEGER DEFAULT 0
  )
`);

// INSERT
app.post('/api/books', async (c) => {
  const body = await c.req.json();
  const result = db.query(
    'INSERT INTO books (title, author, year, is_read) VALUES ($title, $author, $year, $is_read)'
  ).run({
    $title: body.title,
    $author: body.author,
    $year: body.year,
    $is_read: body.is_read ? 1 : 0,
  });

  const book = db.query('SELECT * FROM books WHERE id = $id').get({
    $id: Number(result.lastInsertRowid),
  });

  return c.json(book, 201);
});

// SELECT ALL
app.get('/api/books', (c) => {
  const books = db.query('SELECT * FROM books').all();
  return c.json(books);
});
```

### 8. Mode Development dengan Hot Reload

Bun mendukung **watch mode** yang otomatis merestart server saat file berubah:

```bash
bun --watch run index.ts
```

Untuk development yang lebih nyaman, tambahkan script di `package.json`:

```json
{
  "scripts": {
    "dev": "bun --watch run index.ts",
    "start": "bun run index.ts"
  }
}
```

Jalankan:

```bash
bun run dev
```

Setiap kali kamu mengubah file, server akan restart dalam milidetik.

### 9. Struktur Folder untuk Proyek Nyata

Untuk proyek yang lebih besar, gunakan struktur folder seperti ini:

```
hono-bun-api/
├── src/
│   ├── index.ts          # Entry point
│   ├── routes/
│   │   ├── books.ts      # Route handler buku
│   │   └── users.ts      # Route handler user
│   ├── middleware/
│   │   └── auth.ts       # Middleware autentikasi
│   ├── db/
│   │   └── schema.ts     # Database schema
│   ├── validators/
│   │   └── book.ts       # Validasi Zod
│   └── types/
│       └── index.ts      # Type definitions
├── package.json
└── tsconfig.json
```

Contoh route terpisah (`src/routes/books.ts`):

```typescript
import { Hono } from 'hono';

const books = new Hono();

books.get('/', (c) => c.json({ message: 'Daftar buku' }));
books.get('/:id', (c) => c.json({ message: `Buku ${c.req.param('id')}` }));
books.post('/', async (c) => {
  const body = await c.req.json();
  return c.json(body, 201);
});

export default books;
```

Lalu di `src/index.ts`:

```typescript
import { Hono } from 'hono';
import booksRoute from './routes/books';

const app = new Hono();

app.route('/api/books', booksRoute);

export default app;
```

### 10. Middleware Bawaan Hono yang Berguna

Hono menyediakan berbagai middleware bawaan:

```typescript
import { Hono } from 'hono';
import { cors } from 'hono/cors';
import { logger } from 'hono/logger';
import { secureHeaders } from 'hono/secure-headers';
import { etag } from 'hono/etag';

const app = new Hono();

// Logging setiap request
app.use('*', logger());

// Security headers
app.use('*', secureHeaders());

// ETag untuk caching
app.use('*', etag());

// CORS dengan konfigurasi spesifik
app.use('/api/*', cors({
  origin: ['https://example.com', 'http://localhost:5173'],
  allowMethods: ['GET', 'POST', 'PUT', 'DELETE'],
  allowHeaders: ['Content-Type', 'Authorization'],
  maxAge: 600,
}));
```

### 11. Deploy dengan Docker

Setelah API selesai, deploy dengan Docker:

**Dockerfile**
```dockerfile
FROM oven/bun:latest

WORKDIR /app

COPY package.json bun.lock ./
RUN bun install --frozen-lockfile

COPY . .

EXPOSE 3000

CMD ["bun", "run", "index.ts"]
```

**docker-compose.yml** (dengan Nginx reverse proxy)

```yaml
version: '3.8'

services:
  api:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
    restart: always

  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
      - ./ssl:/etc/nginx/ssl
    depends_on:
      - api
    restart: always
```

---

## Kesimpulan

Bun + Hono adalah kombinasi yang sangat powerful untuk membangun REST API modern. Dengan setup yang minimal, performa tinggi, dan ekosistem yang terus berkembang, pasangan ini menjadi pilihan tepat untuk proyek baru — baik untuk prototyping cepat maupun production.

**Yang sudah kamu pelajari:**
- Setup proyek Bun + Hono dalam 30 detik
- CRUD lengkap dengan validasi Zod
- Koneksi SQLite dengan Bun bawaan
- Struktur folder untuk proyek nyata
- Middleware, hot reload, dan deployment Docker

**Langkah selanjutnya:**
1. Coba buat API sendiri dengan Bun + Hono malam ini
2. Tambahkan autentikasi JWT
3. Deploy ke VPS dengan Nginx + SSL
4. Explore fitur Hono lainnya seperti WebSocket dan SSE

Selamat vibe coding! ⚡🚀

---

*Artikel ini ditulis dengan bantuan AI dan telah diedit oleh tim GezyTech.*
