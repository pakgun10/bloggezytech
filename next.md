# Roadmap BlogGezyTech

## Tahap 1 — Branding

- [x] Ganti logo Blowfish
- [x] Ganti favicon
- [x] Ganti warna tema
- [x] Homepage GezyTech
- [x] Menu
- [x] Footer

---

## Tahap 2 — Struktur Konten

Daripada semua artikel masuk ke `posts`, saya lebih suka struktur seperti ini:

```
content/
├── ai/
├── tutorials/
├── linux/
├── server/
├── docker/
├── bun/
├── nodejs/
├── mariadb/
└── sqlite/
```

> AI nanti tinggal memilih folder berdasarkan topik.

---

## Tahap 3 — Konfigurasi SEO

- [x] `sitemap.xml`
- [x] `robots.txt`
- [x] OpenGraph
- [x] Twitter Card
- [x] JSON-LD Schema
- [x] Canonical URL

---

## Tahap 4 — Search

- [x] Aktifkan pencarian bawaan Blowfish.

---

## Tahap 5 — AI Pipeline

Alur kerja AI:

```
Keyword
    │
    ▼
Research
    │
    ▼
Outline
    │
    ▼
Markdown
    │
    ▼
content/tutorials/
    │
    ▼
Hugo Build
    │
    ▼
public/
    │
    ▼
Nginx
```

**Prinsip:**
- AI **tidak pernah** menyentuh HTML
- AI **hanya** membuat Markdown

---

## Standar Frontmatter Artikel

AI tidak boleh langsung membuat file Markdown secara bebas. AI harus menghasilkan struktur frontmatter seperti ini **terlebih dahulu**, baru kemudian isi artikel.

```yaml
---
title:
description:
slug:
date:
lastmod:
draft:
categories:
tags:
author:
featured:
cover:
summary:
---
```

### Keuntungan

| No | Keuntungan |
|----|------------|
| 1  | SEO lebih bagus |
| 2  | Semua artikel memiliki format yang konsisten |
| 3  | AI lebih mudah dikendalikan |

- [x] Template artikel AI (`archetypes/article.md`)
- [x] Archetype default dengan frontmatter standar (`archetypes/default.md`)

---

## Rekomendasi

**Sekarang jangan buat artikel dulu.**

Mari kita jadikan BlogGezyTech sebagai fondasi yang benar-benar siap untuk AI. Setelah konfigurasi tema, branding, SEO, dan struktur konten selesai, baru kita membuat template artikel yang nantinya akan dipakai AI untuk menghasilkan artikel secara otomatis.

Dengan cara itu, setiap artikel yang dibuat agent akan langsung mengikuti standar yang sama tanpa perlu diperbaiki satu per satu.
