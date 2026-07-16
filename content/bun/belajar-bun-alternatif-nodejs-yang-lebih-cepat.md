+++
title = 'Belajar Bun: Alternatif Node.js yang Lebih Cepat'
description = 'Pelajari Bun, runtime JavaScript modern yang 4x lebih cepat dari Node.js. Panduan lengkap instalasi, fitur unggulan, dan cara mulai membuat REST API.'
slug = 'belajar-bun-alternatif-nodejs-yang-lebih-cepat'
date = '2026-07-16T08:35:00+07:00'
lastmod = '2026-07-16T08:35:00+07:00'
draft = false
categories = ['Bun']
tags = ['bun', 'vibecoding', 'docker', 'pemula', 'devops']
author = 'GezyTech'
featured = true
cover = ''
summary = 'Bun adalah runtime JavaScript all-in-one yang dirancang untuk kecepatan. Pelajari cara instalasi, fitur unggulan, dan cara membuat REST API dengan Bun dan Hono.'
+++

## TLDR

Bun adalah runtime JavaScript modern yang dikembangkan oleh Jarred Sumner dan dirilis pada Juli 2022. Bun bukan sekadar pengganti Node.js — ia adalah **toolkit all-in-one** yang mencakup runtime, package manager (bun install), bundler, dan test runner dalam satu file biner. Dibangun dengan Zig dan menggunakan JavaScriptCore (bukan V8), Bun mampu menjalankan kode 4 kali lebih cepat dari Node.js. Artikel ini akan memandumu memulai Bun dari instalasi hingga membuat REST API sederhana.

---

## Pendahuluan

Node.js telah menjadi standar runtime JavaScript selama 15 tahun terakhir. Namun seiring waktu, beberapa kelemahannya mulai terasa: package manager npm yang lambat, konfigurasi bundler yang rumit (Webpack, Vite, Rollup), dan inisialisasi proyek yang membutuhkan banyak boilerplate.

Bun hadir untuk menjawab semua masalah ini dalam satu paket.

Dengan Bun, kamu tidak perlu menginstal npm terpisah, tidak perlu Webpack untuk bundling, dan tidak perlu Jest atau Vitest untuk testing. Semua sudah termasuk dalam satu file biner berukuran sekitar 40 MB. Yang paling penting: Bun bisa 4-10 kali lebih cepat dalam instalasi package dan 2-4 kali lebih cepat dalam eksekusi kode.

Bagi kamu yang suka vibe coding — menulis kode cepat tanpa banyak setup — Bun adalah pilihan yang tepat.

---

## Konten Utama

### 1. Apa Keunggulan Bun Dibanding Node.js?

| Fitur | Node.js | Bun |
|-------|---------|-----|
| Runtime Engine | V8 (C++) | JavaScriptCore (WebKit) |
| Ukuran instalasi | ~30 MB + npm | ~40 MB (all-in-one) |
| Package manager | npm / yarn / pnpm (terpisah) | `bun install` (built-in, 10-30x lebih cepat) |
| Bundler | Webpack, Rollup, Vite (terpisah) | `bun build` (built-in, esbuild-based) |
| Test runner | Jest, Vitest (terpisah) | `bun test` (built-in, kompatibel Jest API) |
| TypeScript | Butuh ts-node atau compile dulu | **Native** (jalan langsung tanpa config) |
| File I/O | Libuv + callback | Libuv + POSIX API async |
| Kecepatan | Standar | 4x lebih cepat pada benchmark |

### 2. Instalasi Bun

Instalasi Bun sangat mudah. Cukup satu perintah:

```bash
curl -fsSL https://bun.sh/install | bash
```

Atau jika lebih suka menggunakan npm:

```bash
npm install -g bun
```

Untuk pengguna Linux (Debian/Ubuntu), bisa juga pakai:

```bash
sudo apt install unzip
curl -fsSL https://bun.sh/install | bash
```

Setelah terinstal, verifikasi:

```bash
bun --version
# contoh output: 1.2.8
```

### 3. Perintah Dasar Bun

#### Inisialisasi Proyek

```bash
# Buat folder dan masuk
mkdir my-bun-project
cd my-bun-project

# Inisialisasi package.json
bun init
```

Perintah `bun init` akan membuat file berikut secara interaktif:
- `package.json`
- `tsconfig.json` (jika pakai TypeScript)
- `index.ts` (file entry point)

#### Manajemen Package

```bash
# Install semua dependensi dari package.json
bun install

# Tambah package baru (dengan nama pendek dan cepat!)
bun add express
bun add zod
bun add @types/node -d

# Hapus package
bun remove express

# Update semua package
bun update
```

Bun menggunakan binary cache yang membuatnya **10-30x lebih cepat** dari npm. Bahkan untuk proyek dengan 100+ dependensi, bun install selesai dalam 1-2 detik.

#### Menjalankan Kode

```bash
# Jalankan file
bun run index.ts

# Jalankan script dari package.json
bun run dev

# Jalankan langsung kode inline
bun -e "console.log('Hello Bun!')"
```

### 4. Bun Native TypeScript — Tanpa Config

Salah satu fitur terbaik Bun: **tidak perlu konfigurasi TypeScript**. Bun bisa menjalankan file `.ts` langsung tanpa `tsconfig.json` atau `ts-node`.

```typescript
// hello.ts
const name: string = "Bun";
console.log(`Hello ${name}!`);

// Jalankan langsung:
// bun run hello.ts
```

Fitur yang langsung berfungsi:
- Tipe statis TypeScript
- Import path alias
- Environment variable dari `.env`
- JSX/TSX untuk React

### 5. Membuat REST API dengan Bun + Hono

Mari kita buat REST API sederhana menggunakan **Hono** — framework web ringan yang cocok dipasangkan dengan Bun.

#### Step 1: Inisialisasi proyek

```bash
mkdir bun-hono-api
cd bun-hono-api
bun init
```

#### Step 2: Install Hono

```bash
bun add hono
```

#### Step 3: Buat file `index.ts`

```typescript
import { Hono } from 'hono';
import { cors } from 'hono/cors';

const app = new Hono();

// Middleware CORS
app.use('/*', cors());

// Route: GET / — Hello
app.get('/', (c) => {
  return c.json({ message: 'Halo dari Bun + Hono!' });
});

// Route: GET /api/users
app.get('/api/users', (c) => {
  const users = [
    { id: 1, name: 'Budi' },
    { id: 2, name: 'Ani' },
    { id: 3, name: 'Citra' },
  ];
  return c.json(users);
});

// Route: POST /api/users
app.post('/api/users', async (c) => {
  const body = await c.req.json();
  return c.json(
    { message: 'User created', user: body },
    201,
  );
});

// Route: GET /api/hello/:name
app.get('/api/hello/:name', (c) => {
  const name = c.req.param('name');
  return c.json({ message: `Hello, ${name}!` });
});

export default app;
```

#### Step 4: Jalankan server

```bash
bun run index.ts
```

Atau gunakan mode watch (auto restart saat file berubah):

```bash
bun --watch run index.ts
```

Server berjalan di `http://localhost:3000`. Coba akses:

```bash
curl http://localhost:3000/
curl http://localhost:3000/api/users
curl http://localhost:3000/api/hello/Pak%20Gun
```

### 6. Bun Package Manager — Kenapa Lebih Cepat?

Perbandingan kecepatan install package (100 package):

| Tool | Waktu Instalasi |
|------|----------------|
| npm | ~45 detik |
| yarn | ~35 detik |
| pnpm | ~25 detik |
| **bun install** | **~1.5 detik** 🚀 |

Bun menggunakan beberapa teknik untuk mencapai kecepatan ini:
- **Global binary cache**: Package yang pernah diunduh disimpan, install berikutnya instan
- **Request batching**: Semua request HTTP dikirim paralel
- **Zig implementation**: Manajemen memori lebih efisien tanpa GC overhead
- **JavaScriptCore optimization**: Start-up time lebih cepat dari V8

### 7. Bun Test Runner

Bun memiliki test runner internal yang kompatibel dengan Jest API:

```typescript
// math.ts
export function add(a: number, b: number): number {
  return a + b;
}

export function divide(a: number, b: number): number {
  if (b === 0) throw new Error('Cannot divide by zero');
  return a / b;
}
```

```typescript
// math.test.ts
import { describe, expect, test } from 'bun:test';
import { add, divide } from './math';

describe('Math functions', () => {
  test('add(2, 3) should return 5', () => {
    expect(add(2, 3)).toBe(5);
  });

  test('divide(10, 2) should return 5', () => {
    expect(divide(10, 2)).toBe(5);
  });

  test('divide by zero should throw', () => {
    expect(() => divide(5, 0)).toThrow('Cannot divide by zero');
  });
});
```

Jalankan:

```bash
bun test
```

Hasil: output berwarna, waktu eksekusi cepat, dan format yang mudah dibaca.

### 8. Bun Build — Bundler Bawaan

Bun bisa menggantikan Webpack atau Rollup untuk proyek sederhana:

```bash
# Build file TypeScript ke JavaScript
bun build index.ts --outdir=./dist

# Build untuk production (minify)
bun build index.ts --outdir=./dist --minify

# Build dengan sourcemap
bun build index.ts --outdir=./dist --sourcemap=external
```

Contoh bundling aplikasi Hono:

```bash
bun build index.ts --outdir=./dist --target=bun
```

File output di `./dist/index.js` siap di-deploy.

### 9. Deploy Bun dengan Docker

Bun juga bisa di-container-kan dengan Docker. Contoh Dockerfile minimal:

```dockerfile
FROM oven/bun:latest

WORKDIR /app

# Copy package files
COPY package.json bun.lock ./
RUN bun install

# Copy source code
COPY . .

# Expose port
EXPOSE 3000

# Run
CMD ["bun", "run", "index.ts"]
```

Build dan jalankan:

```bash
docker build -t bun-api .
docker run -d -p 3000:3000 bun-api
```

Dengan ukuran image sekitar 150 MB (vs Node.js yang 300+ MB), Bun sangat cocok untuk deployment container yang efisien.

### 10. Kapan Sebaiknya Pakai Bun?

**Cocok untuk:**
- REST API dengan framework ringan (Hono, Elysia)
- CLI tools dan script automation
- Proyek vibe coding yang butuh setup cepat
- Aplikasi real-time (WebSocket)
- Microservices dengan container

**Kurang cocok untuk:**
- Proyek yang bergantung pada native Node.js modules (C++ addons)
- Aplikasi yang membutuhkan ekosistem tools lama (gulp, grunt, dll.)
- Proyek enterprise dengan ribuan dependensi yang sudah mature di Node.js

---

## Kesimpulan

Bun adalah masa depan runtime JavaScript. Dengan kecepatan eksekusi yang luar biasa, tool all-in-one yang terintegrasi, dan dukungan TypeScript native, Bun membuat pengalaman coding menjadi lebih cepat dan menyenangkan.

Bagi kamu yang suka vibe coding — tinggal install, buat file, dan langsung jalan tanpa ribet setup — Bun adalah pilihan yang sulit ditandingi. Mulailah dengan proyek kecil seperti REST API dengan Hono, dan rasakan sendiri perbedaan kecepatannya.

**Langkah selanjutnya:**
1. Install Bun di sistemmu
2. Coba `bun init` untuk proyek pertama
3. Buat REST API sederhana dengan Hono
4. Bandingkan sendiri kecepatannya dengan Node.js

Selamat vibe coding dengan Bun! ⚡🚀

---

*Artikel ini ditulis dengan bantuan AI dan telah diedit oleh tim GezyTech.*
