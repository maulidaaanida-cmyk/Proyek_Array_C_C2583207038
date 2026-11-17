# Sistem Monitoring Stok dan Penjualan Kantin Kampus

## Identitas

| Keterangan   | Isi                                                |
|--------------|----------------------------------------------------|
| Nama Lengkap | Hermawati                                          |
| NIM          | C2583207038                                        |
| Kelas        | PTI3B                                              |
| Mata Kuliah  | Algoritma dan Pemrograman Lanjutan                 |
| Dosen        | Taofik Muhammad, M.Kom                             |
| Nama Proyek  | Sistem Monitoring Stok dan Penjualan Kantin Kampus |
| Universitas  | Universitas Muhammadiyah Tasikmalaya               |

## Deskripsi Singkat

Aplikasi console berbasis bahasa C untuk membantu pengelola kantin kampus memantau stok, memproses transaksi penjualan beberapa produk dalam satu transaksi, mencari dan mengurutkan produk, serta melihat laporan penjualan. Seluruh data disimpan di dalam array selama program berjalan (tanpa database atau file eksternal). Kapasitas maksimal adalah 50 produk dan program menyediakan 6 produk sebagai data awal.

## Daftar Fitur

| Menu | Fitur | Keterangan |
|------|-------|------------|
| 1 | Lihat Daftar Produk | Menampilkan nomor, nama, harga, dan stok seluruh produk |
| 2 | Tambah Produk | Menambah produk baru (nama, harga, stok awal); menolak nama kosong atau duplikat |
| 3 | Ubah Produk | Mengubah harga dan stok produk berdasarkan nomor produk |
| 4 | Transaksi Penjualan | Membeli beberapa jenis produk dalam satu transaksi, menampilkan rincian subtotal dan total pembayaran, lalu konfirmasi (y/n) |
| 5 | Tambah Stok | Menambah stok produk yang sudah ada |
| 6 | Cari Produk | Mencari produk berdasarkan nama (tidak peka huruf besar/kecil, bisa sebagian nama) |
| 7 | Urutkan Produk | Urut harga termurah atau jumlah terjual terbanyak (bubble sort) |
| 8 | Laporan Penjualan | Jumlah terjual dan pendapatan per produk, total unit, total pendapatan, dan produk terlaris |
| 9 | Cek Stok Menipis | Menampilkan peringatan untuk produk dengan stok kurang dari 5 |
| 0 | Keluar | Mengakhiri program |

**Validasi input dan penanganan kesalahan:**
- Pilihan menu tidak tersedia, atau input bukan angka.
- Nomor produk tidak ditemukan.
- Jumlah pembelian nol atau negatif.
- Harga atau stok bernilai negatif.
- Jumlah pembelian melebihi stok yang tersedia (termasuk jumlah yang sudah ada di keranjang transaksi yang sama).
- Jumlah produk sudah mencapai kapasitas maksimal (50 produk).

## Penjelasan Penggunaan Array

Array adalah struktur data utama pada program ini. Data produk disimpan secara paralel: indeks yang sama pada semua array merujuk ke produk yang sama.

| Array | Tipe | Fungsi |
|-------|------|--------|
| `char nama[MAX_PRODUK][PANJANG_NAMA]` | Array karakter 2 dimensi (50 x 30) | Menyimpan nama produk; baris = produk, kolom = karakter nama |
| `int harga[MAX_PRODUK]` | Array 1 dimensi | Menyimpan harga satuan tiap produk |
| `int stok[MAX_PRODUK]` | Array 1 dimensi | Menyimpan stok tiap produk |
| `int terjual[MAX_PRODUK]` | Array 1 dimensi | Menyimpan jumlah unit terjual tiap produk |
| `long pendapatan[MAX_PRODUK]` | Array 1 dimensi | Menyimpan akumulasi pendapatan tiap produk |
| `int jumlah` | Variabel | Jumlah data aktif; indeks valid adalah `0` sampai `jumlah - 1` |

Contoh: produk "Nasi Goreng" berada di indeks 0, sehingga datanya adalah `nama[0]`, `harga[0]`, `stok[0]`, `terjual[0]`, dan `pendapatan[0]`.

**Pengelolaan indeks dan kapasitas:**
- Produk baru disimpan pada indeks `jumlah`, lalu `jumlah` bertambah satu.
- Jika `jumlah >= MAX_PRODUK` (50), penambahan produk ditolak dengan pesan kapasitas penuh.
- Pada transaksi, array lokal `qtyBeli[MAX_PRODUK]` menyimpan jumlah beli per produk sebelum transaksi dikonfirmasi, sehingga stok baru berkurang setelah transaksi berhasil.
- Pengurutan memakai array bantu `urut[]` yang berisi indeks produk, sehingga urutan data asli tidak berubah.
- Pencarian memakai perulangan linear pada `nama[i]` dengan `strstr()` setelah nama diubah ke huruf kecil.

## Petunjuk Kompilasi dan Menjalankan

Prasyarat: compiler GCC (Windows: MinGW atau MSYS2; Linux/macOS: `gcc` bawaan).

```bash
# Kompilasi
gcc kantin.c -o kantin

# Menjalankan (Linux / macOS)
./kantin

# Menjalankan (Windows)
kantin.exe
```

## Data Awal

| No | Nama Produk  | Harga  | Stok |
|----|--------------|--------|------|
| 1  | Nasi Goreng  | 12.000 | 20   |
| 2  | Mie Ayam     | 10.000 | 15   |
| 3  | Es Teh Manis | 4.000  | 30   |
| 4  | Gorengan     | 2.000  | 40   |
| 5  | Air Mineral  | 3.000  | 4    |
| 6  | Kopi Susu    | 8.000  | 12   |

## Pengujian (Tugas 6)

Pengujian dijalankan berurutan dari kondisi awal program. Perhitungan manual dicantumkan sebagai pembanding hasil program.

### Skenario 1 - Tambah Produk
- **Input/Tindakan:** Menu 2, masukkan nama `Roti Bakar`, harga `6000`, stok `25`. Lalu menu 1.
- **Hasil yang diharapkan:** Produk muncul dalam daftar sebagai nomor 7.

![Skenario 1 - Tambah Produk](screenshots/01-tambah-produk.png)

**Keterangan:** Produk baru disimpan pada indeks `jumlah` lalu `jumlah` bertambah, sehingga "Roti Bakar" tampil di daftar dengan harga 6000 dan stok 25.

### Skenario 2 - Transaksi
- **Input/Tindakan:** Menu 4, beli produk nomor 1 (Nasi Goreng) sebanyak 2 dan nomor 3 (Es Teh Manis) sebanyak 3, ketik `0` untuk selesai.
- **Hasil yang diharapkan:** Subtotal dan total benar.

![Skenario 2 - Transaksi](screenshots/02-transaksi.png)

**Keterangan:** Perhitungan manual: Nasi Goreng 2 x 12.000 = 24.000; Es Teh Manis 3 x 4.000 = 12.000; total pembayaran = 36.000. Hasil program sama dengan perhitungan manual.

### Skenario 3 - Pembaruan Stok
- **Input/Tindakan:** Konfirmasi transaksi skenario 2 dengan `y`, lalu menu 1 dan menu 8.
- **Hasil yang diharapkan:** Stok berkurang dan jumlah terjual bertambah.

![Skenario 3 - Pembaruan Stok](screenshots/03-pembaruan-stok.png)

**Keterangan:** Stok Nasi Goreng berubah dari 20 menjadi 18 dan stok Es Teh Manis dari 30 menjadi 27. Jumlah terjual Nasi Goreng menjadi 2 dan Es Teh Manis menjadi 3.

### Skenario 4 - Stok Tidak Cukup
- **Input/Tindakan:** Menu 4, beli produk nomor 2 (Mie Ayam, stok 15) sebanyak 50.
- **Hasil yang diharapkan:** Transaksi ditolak dan stok tetap.

![Skenario 4 - Stok Tidak Cukup](screenshots/04-stok-tidak-cukup.png)

**Keterangan:** Program menampilkan pesan stok tidak cukup dan produk tidak masuk ke transaksi. Setelah dicek di menu 1, stok Mie Ayam tetap 15.

### Skenario 5 - Laporan
- **Input/Tindakan:** Menu 8.
- **Hasil yang diharapkan:** Unit terjual, total pendapatan, dan produk terlaris benar.

![Skenario 5 - Laporan](screenshots/05-laporan.png)

**Keterangan:** Total unit terjual = 2 + 3 = 5; total pendapatan = 24.000 + 12.000 = 36.000; produk terlaris adalah Es Teh Manis (3 unit).

### Skenario 6 - Pengurutan
- **Input/Tindakan:** Menu 7 pilih `1` (harga termurah), lalu menu 7 pilih `2` (terjual terbanyak).
- **Hasil yang diharapkan:** Urutan sesuai ketentuan.

![Skenario 6a - Urut Harga Termurah](screenshots/06a-urut-harga.png)

![Skenario 6b - Urut Terjual Terbanyak](screenshots/06b-urut-terjual.png)

**Keterangan:** Urutan harga: Gorengan (2.000), Air Mineral (3.000), Es Teh Manis (4.000), Roti Bakar (6.000), Kopi Susu (8.000), Mie Ayam (10.000), Nasi Goreng (12.000). Urutan terjual dimulai dari Es Teh Manis (3), lalu Nasi Goreng (2), kemudian produk lain yang belum terjual (0).

### Skenario 7 - Pencarian dan Stok
- **Input/Tindakan:** Menu 6 cari `kopi`, lalu menu 9.
- **Hasil yang diharapkan:** Hasil pencarian dan peringatan stok tepat.

![Skenario 7a - Pencarian](screenshots/07a-pencarian.png)

![Skenario 7b - Stok Menipis](screenshots/07b-stok-menipis.png)

**Keterangan:** Pencarian "kopi" menampilkan Kopi Susu (harga 8.000, stok 12). Menu 9 menampilkan peringatan untuk Air Mineral karena stoknya 4, yaitu kurang dari 5.

## Struktur Repository

```
Proyek-Array-C-C2583207038/
├── kantin.c
├── README.md
└── screenshots/
    ├── 01-tambah-produk.png
    ├── 02-transaksi.png
    ├── 03-pembaruan-stok.png
    ├── 04-stok-tidak-cukup.png
    ├── 05-laporan.png
    ├── 06a-urut-harga.png
    ├── 06b-urut-terjual.png
    ├── 07a-pencarian.png
    └── 07b-stok-menipis.png
```
