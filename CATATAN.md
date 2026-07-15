# Catatan Proyek BlogGezyTech

> Dibuat: 2026-07-15
> Status: Fondasi Hugo + Blowfish siap, siap untuk tahap artikel

---

## 📌 Info Penting

### Versi Hugo

- Hugo yang terinstall di sistem: **0.147.9** (di `/usr/local/bin/hugo`) — TIDAK kompatibel dengan Blowfish
- Hugo yang dipakai proyek: **0.163.3** (di `~/.local/bin/hugo`) — kompatibel
- **Wajib** jalankan ini sebelum build:
  ```bash
  export PATH="$HOME/.local/bin:$PATH"
  ```
- Atau tambahkan ke `~/.bashrc` / `~/.zshrc` permanen:
  ```bash
  echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
  source ~/.bashrc
  ```

### Domain

- Blog: `info.gezytech.web.id` (subdomain dari `gezytech.web.id`)
- BaseURL sudah diset: `https://info.gezytech.web.id/`
- Sudah ada di: `config/_default/hugo.toml`

### Tema

- Tema: **Blowfish** (versi main branch, download tarball)
- Lokasi: `themes/blowfish/`
- Cara update tema: download ulang tarball dari GitHub
  ```bash
  cd themes && rm -rf blowfish
  curl -sL https://github.com/nunocoracao/blowfish/tarball/main -o /tmp/blowfish.tar.gz
  tar -xzf /tmp/blowfish.tar.gz -C .
  mv nunocoracao-blowfish-* blowfish
  ```

---

## 🗂️ Struktur Proyek

```
blog-hugo/
├── archetypes/
│   ├── default.md          # Template default (frontmatter standar)
│   └── article.md         # Template artikel (TLDR, Pendahuluan, Konten, Kesimpulan)
├── assets/
│   ├── css/
│   │   ├── custom.css                    # CSS custom override
│   │   └── schemes/gezytech.css          # Color scheme Sky Blue + Teal
│   ├── icons/
│   │   ├── logo.svg                      # Logo utama (PLACEHOLDER)
│   │   └── secondary-logo.svg            # Logo dark mode (PLACEHOLDER)
│   └── img/
│       └── og-image.svg                  # Open Graph image (PLACEHOLDER)
├── config/_default/
│   ├── hugo.toml           # Konfigurasi utama Hugo
│   ├── languages.id.toml   # Bahasa Indonesia + author + logo
│   ├── markup.toml         # Goldmark markdown config
│   ├── menus.id.toml       # Menu utama + footer
│   └── params.toml         # Parameter tema Blowfish (branding, SEO, search)
├── content/
│   ├── _index.md           # Homepage
│   ├── ai/_index.md
│   ├── tutorials/_index.md
│   ├── linux/_index.md
│   ├── server/_index.md
│   ├── docker/_index.md
│   ├── bun/_index.md
│   ├── nodejs/_index.md
│   ├── mariadb/_index.md
│   ├── sqlite/_index.md
│   └── privacy/_index.md
├── layouts/
│   ├── partials/           # (kosong, untuk custom partials)
│   ├── shortcodes/         # (kosong, untuk custom shortcodes)
│   └── robots.txt          # Custom robots.txt
├── static/
│   └── img/favicon.svg     # Favicon (PLACEHOLDER)
├── themes/blowfish/        # Tema Blowfish
├── .gitignore
├── next.md                 # Roadmap (checklist)
└── CATATAN.md              # File ini
```

---

## ✅ Yang Sudah Selesai

### Tahap 1 — Branding
- [x] Ganti logo Blowfish → `assets/icons/logo.svg` (placeholder SVG)
- [x] Ganti favicon → `static/img/favicon.svg` (placeholder SVG)
- [x] Ganti warna tema → `assets/css/schemes/gezytech.css` (Sky Blue + Teal)
- [x] Homepage GezyTech → layout `profile` + show recent posts
- [x] Menu → `config/_default/menus.id.toml` (AI, Tutorials, Linux, Server, Docker, Database[submenu], Runtime[submenu], Tags)
- [x] Footer → Privacy, Tags, Categories + copyright + attribution + scroll-to-top

### Tahap 2 — Struktur Konten
- [x] 9 folder konten dibuat: `ai`, `tutorials`, `linux`, `server`, `docker`, `bun`, `nodejs`, `mariadb`, `sqlite`
- [x] Setiap folder punya `_index.md` dengan title + description
- [x] Halaman `privacy/_index.md` untuk footer menu

### Tahap 3 — Konfigurasi SEO
- [x] `sitemap.xml` → otomatis oleh Hugo (`public/sitemap.xml`)
- [x] `robots.txt` → custom di `layouts/robots.txt` (Allow all + Sitemap URL)
- [x] OpenGraph → otomatis oleh Blowfish (og:url, og:title, og:description, og:locale, og:type)
- [x] Twitter Card → otomatis oleh Blowfish (summary)
- [x] JSON-LD Schema → otomatis oleh Blowfish (`application/ld+json`)
- [x] Canonical URL → otomatis oleh Blowfish (`<link rel="canonical">`)

### Tahap 4 — Search
- [x] `enableSearch = true` di `config/_default/params.toml`
- [x] Search index di-generate di `public/index.json`

### Tahap 5 — AI Pipeline
- [x] `archetypes/default.md` — frontmatter standar (title, description, slug, date, lastmod, draft, categories, tags, author, featured, cover, summary)
- [x] `archetypes/article.md` — template artikel dengan struktur (TLDR, Pendahuluan, Konten Utama, Kesimpulan) + instruksi untuk AI di komentar HTML
- [x] Frontmatter standar sudah disesuaikan dengan roadmap di `next.md`

---

## ❌ Yang Belum Selesai / Perlu Dilanjutkan

### Prioritas Tinggi

1. **Ganti logo & favicon asli**
   - Logo SVG sekarang masih placeholder (teks "GezyTech" dengan icon garis)
   - Siapkan logo asli GezyTech (SVG atau PNG)
   - Taruh di: `assets/icons/logo.svg` dan `assets/icons/secondary-logo.svg`
   - Favicon: `static/img/favicon.svg`

2. **OG Image asli**
   - Sekarang placeholder SVG 1200x630
   - Siapkan gambar OG (1200x630px) untuk sharing media sosial
   - Taruh di: `assets/img/og-image.png` atau `.svg`
   - Update path di: `config/_default/languages.id.toml` atau `params.toml` jika perlu

3. **Konfigurasi Google Search Console**
   - Daftarkan `info.gezytech.web.id` di GSC
   - Verifikasi domain
   - Submit sitemap.xml

4. **Setup Nginx di VPS**
   - Konfigurasi server block untuk `info.gezytech.web.id`
   - SSL certificate (Let's Encrypt / Certbot)
   - Deploy: copy isi `public/` ke `/var/www/info.gezytech.web.id/`
   - Contoh config:
     ```nginx
     server {
         listen 80;
         server_name info.gezytech.web.id;
         root /var/www/info.gezytech.web.id;
         index index.html;

         location / {
             try_files $uri $uri/ =404;
         }
     }
     ```

### Prioritas Sedang

5. **Google Analytics atau alternatif privacy-first**
   - Uncomment & isi `googleAnalytics` di `config/_default/hugo.toml`
   - Atau pakai Plausible/Umami (config ada di `params.toml` bagian `[fathomAnalytics]` / `[umamiAnalytics]`)

6. **Deploy automation (opsional)**
   - Git push → auto build → deploy ke VPS
   - Bisa pakai GitHub Actions atau webhook

7. **Halaman About / Tentang**
   - Buat `content/about/_index.md` jika perlu
   - Tambahkan ke menu jika perlu

### Prioritas Rendah

8. **Custom shortcodes** (jika perlu)
   - Folder `layouts/shortcodes/` masih kosong
   - Contoh: shortcode untuk YouTube embed, code tabs, callout, dll

9. **Komentar** (jika perlu)
   - Blowfish mendukung remark42, giscus, utterances
   - Konfigurasi ada di `params.toml` (belum diset)

10. **Newsletter integration** (jika perlu)
    - Blowfish mendukung email signup
    - ConvertKit, Mailchimp, dll

---

## 🚀 Cara Menjalankan

### Build (generate static files)
```bash
export PATH="$HOME/.local/bin:$PATH"
cd ~/dev/gezy/blog-hugo
hugo build
# Output ada di: public/
```

### Dev Server (preview lokal)
```bash
export PATH="$HOME/.local/bin:$PATH"
cd ~/dev/gezy/blog-hugo
hugo server -D
# -D = include drafts
# Buka: http://localhost:1313/
```

### Buat artikel baru
```bash
# Pakai archetype default
hugo new content tutorials/judul-artikel.md

# Pakai archetype article (dengan struktur TLDR dll)
hugo new content tutorials/judul-artikel.md --kind article
```

### Deploy ke VPS
```bash
# 1. Build dulu
hugo build

# 2. Copy ke VPS
rsync -avz --delete public/ user@vps:/var/www/info.gezytech.web.id/

# 3. Atau pakai scp
scp -r public/* user@vps:/var/www/info.gezytech.web.id/
```

---

## 🔑 Konfigurasi Penting

### Bahasa
- `defaultContentLanguage = "id"` (Bahasa Indonesia)
- Locale: `id-ID`
- Date format: `2 January 2006`

### Tema
- `colorScheme = "gezytech"` (custom scheme di `assets/css/schemes/gezytech.css`)
- `defaultAppearance = "light"` (default light mode)
- `autoSwitchAppearance = true` (auto switch dark/light berdasarkan OS)

### Homepage
- Layout: `profile` (profile + recent posts)
- `showRecent = true` (tampilkan 6 artikel terbaru)
- `showMoreLink = true` (link "lihat semua")

### Artikel
- `showDate = true`, `showDateUpdated = true`
- `showAuthor = true`, `showReadingTime = true`
- `showTableOfContents = true`
- `showBreadcrumbs = true`
- `showTaxonomies = true` (categories + tags)
- `showWordCount = true`
- Sharing links: LinkedIn, Twitter, Bluesky, Reddit, Facebook, Email, WhatsApp, Telegram

### Frontmatter Standar (untuk AI)
```yaml
+++
title = ''
description = ''
slug = ''
date = ''
lastmod = ''
draft = true
categories = []
tags = []
author = 'GezyTech'
featured = false
cover = ''
summary = ''
+++
```

---

## 📝 Catatan Teknis

1. **Hugo 0.163.3** ada di `~/.local/bin/hugo`, bukan di sistem. Jangan lupa set PATH sebelum build.

2. **Tema Blowfish** diinstall via tarball (bukan git submodule). Jika `git status` menampilkan banyak file di `themes/blowfish/`, tambahkan ke `.gitignore`:
   ```
   themes/blowfish/
   ```
   Atau biarkan jika ingin track tema di repo.

3. **`.gitmodules`** ada di proyek tapi tidak aktif. Bisa dihapus atau dipertahankan.

4. **SVG favicon** sudah didukung Blowfish. Jika ingin PNG favicon juga, taruh di `static/img/`.

5. **Build berhasil**: 34 halaman, 8 static files, 11 aliases, 352ms.

---

## 📅 Tolong Dilanjutkan Besok

> Urutan kerja:

1. Ganti logo & favicon asli GezyTech
2. Setup DNS subdomain `info.gezytech.web.id` → VPS IP
3. Install Nginx + SSL di VPS
4. Deploy hasil build ke VPS
5. Daftar Google Search Console, submit sitemap
6. Buat artikel pertama pakai archetype `article`
7. Tes search, SEO tags, dan tampilan di mobile

---

*Catatan ini dibuat oleh Zed AI Agent pada 2026-07-15.*