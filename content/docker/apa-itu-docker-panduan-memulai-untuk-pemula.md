+++
title = 'Apa itu Docker? Panduan Memulai untuk Pemula'
description = 'Pelajari konsep dasar Docker, container, dan cara memulainya di Linux. Panduan praktis untuk pemula yang ingin memahami container dan Docker.'
slug = 'apa-itu-docker-panduan-memulai-untuk-pemula'
date = '2026-07-16T08:30:00+07:00'
lastmod = '2026-07-16T08:30:00+07:00'
draft = false
categories = ['Docker']
tags = ['docker', 'container', 'pemula', 'devops']
author = 'GezyTech'
featured = true
cover = ''
summary = 'Pelajari konsep dasar Docker, cara kerja container, dan langkah-langkah praktis untuk memulai Docker di Linux. Cocok untuk pemula yang baru pertama kali mendengar istilah container.'
+++

## TLDR

Docker adalah platform yang memungkinkan kamu menjalankan aplikasi dalam **container** — lingkungan terisolasi yang ringan dan portabel. Bayangkan container sebagai "paket lengkap" berisi aplikasi beserta semua yang dibutuhkannya (library, konfigurasi, dependensi) yang bisa berjalan di mana saja tanpa ribet install ulang. Artikel ini akan membantumu memahami konsep dasar Docker dan langsung praktik dalam 10 menit.

---

## Pendahuluan

Pernah mengalami masalah ini? Kamu sudah coding berjam-jam, aplikasi berjalan lancar di laptopmu. Tapi ketika dikirim ke teman atau di-deploy ke server, tiba-tiba error. "Kok di laptop saya jalan?" — kalimat klasik yang bikin frustrasi.

Penyebabnya sederhana: **lingkungan yang berbeda**. Laptopmu punya versi Node.js yang berbeda, library yang tidak sama, atau konfigurasi sistem operasi yang lain.

Docker hadir untuk menyelesaikan masalah ini dengan konsep **container**.

Container bukanlah hal baru — teknologi ini sudah ada sejak era 1970-an dalam bentuk UNIX chroot. Tapi Docker, yang dirilis pada 2013, membuat container menjadi mudah dan praktis digunakan oleh developer di seluruh dunia. Hingga 2026, Docker telah menjadi standar industri untuk pengembangan dan deployment aplikasi.

Artikel ini akan membimbingmu dari nol: apa itu Docker, bagaimana cara kerjanya, dan bagaimana memulainya di Linux.

---

## Konten Utama

### 1. Apa Itu Docker?

Docker adalah platform **open-source** untuk mengembangkan, mengirim, dan menjalankan aplikasi di dalam container. Docker memisahkan aplikasi dari infrastruktur sehingga kamu bisa mengirimkan perangkat lunak dengan cepat.

**Apa bedanya dengan virtual machine (VM)?**

| Aspek | Virtual Machine | Docker Container |
|-------|----------------|------------------|
| Sistem Operasi | Setiap VM punya OS sendiri (guest OS) | Berbagi OS host |
| Ukuran | Gigabyte (GB) | Megabyte (MB) |
| Waktu boot | Menit | Detik |
| Resource | Berat (butuh RAM & CPU besar) | Ringan (minimal overhead) |
| Isolasi | Sangat kuat (hardware-level) | Cukup kuat (kernel-level) |

Karena container berbagi kernel dengan sistem host, container jauh lebih ringan dan cepat daripada VM. Ini kenapa kamu bisa menjalankan puluhan container di satu server tanpa masalah.

### 2. Konsep Dasar Docker

Ada tiga konsep utama yang harus kamu pahami:

#### a. Image

Image adalah template **read-only** yang berisi instruksi untuk membuat container. Bayangkan image seperti resep masakan — dia berisi daftar bahan dan langkah-langkah, tapi belum ada masakannya.

Image Docker biasanya didasarkan pada sistem operasi minimal (seperti Alpine Linux yang hanya 5 MB) ditambah aplikasi dan dependensinya.

#### b. Container

Container adalah **instance** dari sebuah image yang sedang berjalan. Jika image adalah resep, container adalah masakan yang sudah jadi dan siap dimakan.

Kamu bisa memiliki banyak container dari image yang sama. Misalnya, kamu punya image Nginx, lalu menjalankan 3 container Nginx untuk 3 website berbeda.

#### c. Registry / Docker Hub

Registry adalah tempat menyimpan dan mendistribusikan image Docker. **Docker Hub** adalah registry publik default yang berisi ribuan image siap pakai: Ubuntu, Nginx, PostgreSQL, Node.js, dan masih banyak lagi.

### 3. Istilah Penting Lainnya

| Istilah | Penjelasan |
|---------|------------|
| **Dockerfile** | File teks berisi instruksi untuk membangun image Docker |
| **docker-compose.yml** | File konfigurasi untuk menjalankan multi-container (misal: app + database + cache) |
| **Volume** | Penyimpanan persisten untuk data container (data tidak hilang saat container dihapus) |
| **Port Mapping** | Meneruskan port dari container ke host (misal: port 3000 container → port 80 host) |
| **Bind Mount** | Menghubungkan folder di host ke folder di container (untuk development) |

### 4. Instalasi Docker di Linux

Cara termudah install Docker di Linux (Ubuntu/Debian):

```bash
# Hapus paket lama jika ada
sudo apt remove docker docker-engine docker.io containerd runc

# Update paket
sudo apt update

# Install dependensi
sudo apt install ca-certificates curl gnupg

# Tambah GPG key resmi Docker
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# Tambah repository Docker
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Verifikasi instalasi
sudo docker run hello-world

# Agar bisa pakai Docker tanpa sudo (tambahkan user ke grup docker)
sudo usermod -aG docker $USER
# Logout lalu login kembali atau jalankan: newgrp docker
```

### 5. Perintah Docker Dasar yang Wajib Diketahui

#### Manajemen Image

```bash
# Melihat daftar image lokal
docker images

# Mendownload image dari Docker Hub
docker pull nginx:alpine
docker pull ubuntu:22.04

# Menghapus image
docker rmi nginx:alpine
```

#### Manajemen Container

```bash
# Menjalankan container
docker run nginx:alpine

# Menjalankan container di background (detached)
docker run -d nginx:alpine

# Menjalankan container dengan port mapping
docker run -d -p 8080:80 nginx:alpine

# Melihat container yang berjalan
docker ps

# Melihat semua container (termasuk yang berhenti)
docker ps -a

# Menghentikan container
docker stop <container_id>

# Menghapus container
docker rm <container_id>
```

#### Interaksi dengan Container

```bash
# Masuk ke dalam container yang sedang berjalan
docker exec -it <container_id> /bin/sh

# Melihat log container
docker logs <container_id>

# Melihat resource yang dipakai container
docker stats
```

### 6. Membuat Image Sendiri dengan Dockerfile

Ini bagian yang paling penting. Mari kita buat aplikasi web sederhana dengan Node.js.

Buat folder proyek dan file-file berikut:

**app.js**
```javascript
const express = require('express');
const app = express();
const PORT = 3000;

app.get('/', (req, res) => {
    res.send('Halo dari Docker! 🐳');
});

app.listen(PORT, () => {
    console.log(`Server berjalan di port ${PORT}`);
});
```

**package.json**
```json
{
  "name": "docker-demo",
  "version": "1.0.0",
  "dependencies": {
    "express": "^4.18.2"
  }
}
```

**Dockerfile**
```dockerfile
# Gunakan image Node.js versi 20 (ringan)
FROM node:20-alpine

# Buat folder kerja di dalam container
WORKDIR /app

# Copy file dependensi
COPY package.json ./

# Install dependensi
RUN npm install

# Copy semua file aplikasi
COPY . .

# Buka port 3000
EXPOSE 3000

# Perintah saat container dijalankan
CMD ["node", "app.js"]
```

**Build dan jalankan:**

```bash
# Build image
docker build -t docker-demo .

# Jalankan container
docker run -d -p 3000:3000 docker-demo

# Buka browser: http://localhost:3000
```

### 7. Menggunakan Docker Compose untuk Multi-Container

Aplikasi nyata biasanya butuh lebih dari satu service. Contoh: aplikasi web + database MySQL.

**docker-compose.yml**
```yaml
version: '3.8'

services:
  app:
    build: .
    ports:
      - "3000:3000"
    depends_on:
      - db
    environment:
      - DB_HOST=db
      - DB_USER=root
      - DB_PASSWORD=secret

  db:
    image: mysql:8.0
    environment:
      - MYSQL_ROOT_PASSWORD=secret
      - MYSQL_DATABASE=myapp
    volumes:
      - mysql_data:/var/lib/mysql

volumes:
  mysql_data:
```

Jalankan dengan satu perintah:
```bash
docker compose up -d
```

Docker Compose akan:
1. Build image aplikasi
2. Download image MySQL
3. Jalankan kedua container
4. Hubungkan mereka dalam satu network internal

### 8. Praktik Terbaik Docker

| Praktik | Penjelasan |
|---------|------------|
| **Gunakan image kecil** | Pilih `alpine` atau `slim` variant untuk ukuran minimal |
| **Multi-stage build** | Pisahkan environment build dan runtime dalam satu Dockerfile |
| **Jangan simpan data di container** | Gunakan **volume** untuk data yang perlu bertahan |
| **Satu proses per container** | Jangan jalankan banyak service dalam satu container |
| **Gunakan .dockerignore** | Sama seperti .gitignore, untuk mengecualikan file yang tidak perlu |
| **Tag image dengan versi** | Jangan pakai `latest` untuk production; gunakan `v1.0`, `v2.0`, dll |
| **Scan keamanan** | Gunakan `docker scan` untuk memeriksa kerentanan image |

### 9. Contoh Multi-stage Build

```dockerfile
# Stage 1: Build
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Stage 2: Production
FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

Hasilnya: image final hanya berisi file statis (HTML, JS, CSS) tanpa Node.js — ukuran turun dari 200 MB menjadi hanya 20 MB!

---

## Kesimpulan

Docker adalah alat yang powerful untuk mengatasi masalah "bekerja di laptop saya" dan menyederhanakan deployment aplikasi. Dengan Docker, kamu bisa:

- Menjamin konsistensi lingkungan dari development ke production
- Mengisolasi aplikasi tanpa overhead VM
- Memulai dan menghentikan aplikasi dalam hitungan detik
- Berbagi environment dengan tim melalui Dockerfile
- Mengelola aplikasi multi-service dengan Docker Compose

**Langkah selanjutnya setelah membaca artikel ini:**
1. Install Docker di Linux-mu
2. Coba `docker run hello-world`
3. Jalankan container Nginx: `docker run -d -p 8080:80 nginx:alpine`
4. Akses `http://localhost:8080` — lihat? Semudah itu!
5. Buat Dockerfile untuk proyekmu sendiri

Docker bukan lagi "opsional" — ini sudah menjadi keterampilan dasar yang harus dimiliki setiap developer. Semakin cepat kamu memulainya, semakin banyak waktu yang bisa kamu hemat di masa depan.

Selamat mencoba, dan selamat datang di dunia container! 🐳

---

*Artikel ini ditulis dengan bantuan AI dan telah diedit oleh tim GezyTech.*
