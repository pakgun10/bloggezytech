+++
title = '{{ replace .File.ContentBaseName "-" " " | title }}'
description = ''
slug = '{{ .File.ContentBaseName }}'
date = '{{ .Date }}'
lastmod = '{{ .Date }}'
draft = true
categories = []
tags = []
author = 'GezyTech'
featured = false
cover = ''
summary = ''
+++

<!--
INSTRUKSI UNTUK AI:
1. Isi semua field frontmatter di atas sebelum menulis artikel.
2. Pilih satu category utama dan 3-5 tags yang relevan.
3. slug harus pendek, hyphenated, dan mengandung keyword target.
4. description: 150-160 karakter, mengandung keyword + call-to-action.
5. summary: 2-3 kalimat ringkasan artikel.
6. cover: jika ada gambar featured, isi path relatif ke /images/ (contoh: "/images/linux-mint-cover.jpg")
7. Artikel dimulai setelah baris ini. Jangan ubah frontmatter setelah diisi.

CARA MENYISIPKAN GAMBAR:
- Simpan gambar di: static/images/nama-file.png
- Di markdown, gunakan: ![Alt text](/images/nama-file.png)
- Atau pakai shortcode figure (lebih rapi):
  {{< figure
      src="/images/nama-file.png"
      alt="Deskripsi gambar"
      caption="Opsional: caption gambar" >}}
- Untuk screenshot terminal:
  {{< screenshot src="nama-screenshot" alt="Terminal output" >}}
  (gambar harus ada di folder yang sama dengan file artikel)
- Gambar harus berformat .png, .jpg, atau .webp
- Alt text wajib diisi (untuk SEO + aksesibilitas)
- Ukuran gambar maksimal 200KB, lebar maksimal 1200px
--> 