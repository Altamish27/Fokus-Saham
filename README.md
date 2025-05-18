# Fokus Saham - API Data Saham

## Ikhtisar
Fokus Saham adalah aplikasi web yang menyediakan data keuangan dan pasar saham melalui API REST. Aplikasi ini mengambil data dari koleksi MongoDB dan menyajikannya melalui endpoint Flask, sehingga dapat diakses oleh aplikasi frontend seperti React.

## Arsitektur Sistem

```
┌─────────────┐     ┌─────────────┐     ┌─────────────────┐
│             │     │             │     │                 │
│  MongoDB    │────▶│  Flask API  │────▶│  Client (React) │
│  Database   │     │  Server     │     │  Application    │
│             │     │             │     │                 │
└─────────────┘     └─────────────┘     └─────────────────┘
```

## Struktur Database MongoDB

Aplikasi ini terhubung ke instans MongoDB yang berjalan di `localhost:27017` dan menggunakan database `stock_data`.

### Koleksi:
- `dataUmum`: Berita keuangan umum
- `beritaTicker`: Berita khusus untuk ticker saham
- `lapkeu2024q1`: Laporan keuangan Q1 2024
- `lapkeu2024q2`: Laporan keuangan Q2 2024
- `lapkeu2024q3`: Laporan keuangan Q3 2024
- `lapkeu2024q4`: Laporan keuangan Q4 2024
- `daily_aggregation_ticker`: Data saham harian yang dikelompokkan berdasarkan ticker
- `monthly_aggregation_ticker`: Data saham bulanan yang dikelompokkan berdasarkan ticker
- `yearly_aggregation_ticker`: Data saham tahunan yang dikelompokkan berdasarkan ticker

## Endpoint API

### Data Berita
- **GET `/api/beritaUmum`**: Mengambil berita keuangan umum
- **GET `/api/beritaTicker`**: Mengambil berita khusus untuk ticker saham

### Laporan Keuangan
- **GET `/api/lapkeuQ1/2024`**: Mengambil laporan keuangan Q1 2024
- **GET `/api/lapkeuQ2/2024`**: Mengambil laporan keuangan Q2 2024
- **GET `/api/lapkeuQ3/2024`**: Mengambil laporan keuangan Q3 2024
- **GET `/api/lapkeuQ4/2024`**: Mengambil laporan keuangan Q4 2024

### Data Saham
- **GET `/api/daily/<ticker>/<column>`**: Mengambil data harian untuk ticker dan kolom tertentu
- **GET `/api/monthly/<ticker>/<column>`**: Mengambil data bulanan untuk ticker dan kolom tertentu
- **GET `/api/yearly/<ticker>/<column>`**: Mengambil data tahunan untuk ticker dan kolom tertentu

## Alur Aplikasi

1. **Pengumpulan Data**: Data dikumpulkan dan disimpan dalam koleksi MongoDB 
2. **Server API**: Aplikasi Flask terhubung ke MongoDB dan mengekspos endpoint REST
3. **Pengambilan Data**:
   - Ketika endpoint API dipanggil, aplikasi melakukan kueri ke koleksi MongoDB yang relevan
   - Data diambil berdasarkan parameter seperti simbol ticker dan nama kolom
   - Untuk data saham, agregasi waktu yang berbeda (harian, bulanan, tahunan) ditangani oleh koleksi terpisah
4. **Pemformatan Respons**:
   - Data yang diambil diformat menjadi JSON
   - Informasi tanggal/waktu disertakan berdasarkan tingkat agregasi
   - Penanganan kesalahan diterapkan untuk kasus seperti ticker yang tidak ditemukan atau kesalahan server
5. **Konsumsi Klien**: Data dapat dikonsumsi oleh aplikasi frontend melalui endpoint API yang diaktifkan CORS

## Penanganan Kesalahan

Aplikasi ini menyertakan penanganan kesalahan untuk berbagai skenario:
- Error 404 ketika ticker yang diminta tidak ditemukan
- Error 500 untuk masalah server internal
- Pencatatan error terperinci untuk tujuan debugging

## Pengaturan dan Eksekusi

Untuk menjalankan aplikasi:

```bash
python app.py
```

Ini akan memulai server Flask di localhost:5000 dengan mode debugging diaktifkan.

## Persyaratan

- Python 3.x
- Flask
- PyMongo
- Flask-CORS

## Catatan Pengembangan

- CORS diaktifkan untuk mengizinkan akses dari domain yang berbeda (misalnya, frontend React)
- Pencatatan debug diaktifkan untuk membantu pemecahan masalah
- Aplikasi ini dirancang untuk bekerja dengan struktur data MongoDB yang spesifik untuk pasar keuangan
