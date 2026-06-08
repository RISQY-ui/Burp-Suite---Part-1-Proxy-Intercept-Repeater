# Burp-Suite---Part-1-Proxy-Intercept-Repeater
Dokumentasi praktik Burp Suite Part 1 - Proxy, Intercept, Repeater, dan koneksi ke sqlmap.


# Burp Suite Setup & Interception Guide

## Overview

Dokumentasi lengkap untuk setup Burp Suite di Kali Linux, intercept traffic, manipulasi request dengan Repeater, dan integrasi dengan sqlmap.

---

## Tahap 1: Membangun Infrastruktur (Localhost & Docker)

1. **Aktifkan Docker** - Pastikan service Docker berjalan
2. **Run Your Web** - Jalankan container PHP dan MySQL. Pastikan web bisa diakses via browser di Kali Linux (contoh: `http://localhost:8080`)

---

## Tahap 2: Konfigurasi "Radar" (Burp Suite)

1. **Buka Burp Suite** - Di menu Kali Linux, klik ikon Burp Suite
2. **Gunakan "Burp Browser"**:
   - Buka tab `Proxy` -> `Intercept`
   - Klik tombol `Open Browser` (browser ini sudah otomatis terhubung ke Burp tanpa setting proxy manual)

---

## Tahap 3: Menangkap Sinyal (The Capture)

1. Ketik alamat web Docker di Burp Browser: `http://localhost:8080`
2. Pastikan tombol `Intercept is on` di Burp berwarna biru
3. Klik apa saja di web kamu, maka Burp akan menangkap data mentah

---

## Panduan Intercept

1. **Pilih Project**: Klik `Next` -> `Start Burp` (gunakan Temporary project)
2. **Masuk ke Proxy**: Klik tab `Proxy` di bagian atas
3. **Buka Browser Khusus**: Klik tombol `Open Browser` (di sub-tab Intercept)
4. **Akses Target**: Di browser yang terbuka, ketik `http://localhost:8080`
5. **Tahan Data**: Pastikan tombol `Intercept is on` menyala. Saat web diakses, loading akan berhenti dan data mentah muncul di Burp

---

## Fitur Repeater (Manipulasi Data)

### Mengirim ke Repeater

- Di jendela Burp pada tab Intercept, klik kanan pada kode
- Pilih `Send to Repeater`

### Menggunakan Repeater

1. Klik tab `Repeater` di bagian atas (sebelah tab Proxy)
2. Di dalam tab Repeater, lihat ke pojok kiri atas - ada tombol orange/merah `Send`
3. **Eksperimen**:
   - Ubah salah satu kata/angka di sisi kiri (Request)
   - Klik `Send`
   - Lihat perubahan di sisi kanan (Response)

### Struktur Tampilan Repeater

- **Sisi Kiri (Request)** : Data yang dikirim ke web
- **Sisi Kanan (Response)** : Jawaban dari server

---

## Eksperimen Manipulasi User-Agent

1. Cari baris `User-Agent: Mozilla/5.0 (X11; Linux x86_64) ...`
2. Hapus isinya dan ganti, contoh: `User-Agent: FarisGanteng-Agent-007`
3. Klik tombol `Send`
4. Lihat Response di kolom kanan

> **Catatan**: Server akan mencatat akses dari "FarisGanteng-Agent-007" bukan dari browser Linux biasa.

---

## Integrasi Burp Suite dengan sqlmap

### Kenapa Kombinasi Ini Penting

| Fitur | Burp Suite | sqlmap |
|-------|------------|--------|
| Fungsi | Kacamata X-ray + manipulasi presisi | Palu godam otomatis |
| Kelebihan | Tangkap request spesifik, analisis manual | Otomatis deteksi & eksploitasi SQLi |

### Keunggulan Kombinasi

1. **Presisi Tinggi**: Tangkap request spesifik di Burp, baru berikan ke sqlmap
2. **Tembus Pertahanan**: Atur parameter (User-Agent/Cookie) di Burp agar sqlmap lancar
3. **Analisis Manual**: Lihat langsung respon server untuk validasi celah

### Cara Menghubungkan

1. **Simpan Request dari Burp**:
   - Di tab Repeater atau Proxy, klik kanan pada request
   - Pilih `Copy to file` atau copy seluruh teks request ke editor (simpan sebagai `request.txt`)

2. **Panggil sqlmap**:
   ```bash
   sqlmap -r request.txt --batch --dbs
