# Exploratory Data Analysis on Cafe Sales Dataset

Pembersihan dan eksplorasi 10.000 transaksi kafe tahun 2023 dengan Python (Pandas, Matplotlib, Seaborn) untuk memulihkan visibilitas pendapatan dari data yang kotor, lalu menemukan pola produk, metode pembayaran, lokasi, dan tren bulanan.

---

## Daftar Isi

- [Latar Belakang](#latar-belakang)
- [Tujuan Analisis](#tujuan-analisis)
- [Struktur Repositori](#struktur-repositori)
- [Dataset](#dataset)
- [Metodologi](#metodologi)
- [Temuan Utama](#temuan-utama)
- [Rekomendasi](#rekomendasi)
- [Keterbatasan](#keterbatasan)
- [Cara Menjalankan](#cara-menjalankan)
- [Penulis](#penulis)

---

## Latar Belakang

Data transaksi kafe yang tercatat tidak rapi (nilai `ERROR`, `UNKNOWN`, dan kosong di banyak kolom) membuat pemilik kafe sulit melihat pendapatan, produk unggulan, dan pola permintaan yang sebenarnya. Proyek ini membersihkan data tersebut dan mengeksplorasinya agar keputusan operasional bisa didasarkan pada angka yang andal.

## Tujuan Analisis

1. Membersihkan data transaksi mentah tanpa membuang informasi yang masih bisa dipulihkan.
2. Mengidentifikasi produk dengan volume dan pendapatan tertinggi.
3. Memahami kebiasaan pembayaran, lokasi transaksi, dan pola permintaan bulanan.
4. Menyusun rekomendasi operasional berbasis data.

## Struktur Repositori

```text
exploratory_data_analysis_on_caffe_sales_dataset/
├── analysis_process/
│   └── ahmad_faiz_ali_azmi_mini_project_data_visualisasi_dan_eda.ipynb   # Cleaning dan EDA
├── dataset/
│   └── Caffe Sales.csv                                                    # Dataset mentah (10.000 baris, 8 kolom)
├── Exploratory Data Analysis Cafe Sales.pdf                               # Laporan presentasi (12 slide)
└── README.md
```

## Dataset

- **Jumlah baris:** 10.000 transaksi
- **Jumlah kolom:** 8 (menjadi 12 setelah feature engineering)
- **Periode:** tahun 2023
- **Mata uang:** dolar (`$`)

| Kolom | Keterangan |
| --- | --- |
| `Transaction ID` | ID unik transaksi |
| `Item` | Produk (Coffee, Cake, Cookie, Juice, Salad, Sandwich, Smoothie, Tea) |
| `Quantity` | Jumlah unit |
| `Price Per Unit` | Harga per unit |
| `Total Spent` | Total belanja (`Quantity × Price Per Unit`) |
| `Payment Method` | Cash, Credit Card, Digital Wallet |
| `Location` | In-store atau Takeaway |
| `Transaction Date` | Tanggal transaksi |

Kondisi data mentah:

| Kolom | Kosong | `ERROR` | `UNKNOWN` |
| --- | ---: | ---: | ---: |
| `Item` | 333 | 292 | 344 |
| `Payment Method` | 2.579 | 306 | 293 |
| `Location` | 3.265 | 358 | 338 |
| `Quantity` | 138 | – | – |
| `Price Per Unit` | 179 | – | – |
| `Total Spent` | 173 | – | – |
| `Transaction Date` | 159 | – | – |

Tidak ada baris atau `Transaction ID` duplikat.

## Metodologi

### 1. Data Cleaning

| Masalah | Penanganan |
| --- | --- |
| `ERROR` dan `UNKNOWN` | Diubah menjadi nilai kosong agar diperlakukan seragam |
| `Item` kosong | Dipulihkan dari `Price Per Unit` melalui pemetaan harga → produk; sisa yang tidak terpetakan diisi modus |
| `Quantity` kosong | Dihitung ulang dari `Total Spent / Price Per Unit` |
| `Total Spent` kosong | Dihitung ulang dari `Quantity × Price Per Unit`; baris yang tetap kosong dihapus |
| `Payment Method` dan `Location` kosong | Diisi label `Not Specified` (tidak ditebak) |
| `Transaction Date` kosong | Diisi dengan forward-fill |
| Outlier (IQR) | **Dipertahankan** karena merupakan transaksi valid |

Hasil akhir: **9.980 baris**.

### 2. Feature Engineering

Kolom turunan: `Month`, `Month_Name`, `Day_Name`, `Quarter`, serta hasil one-hot encoding untuk variabel kategorikal.

### 3. Eksplorasi dan Visualisasi

Distribusi `Total Spent`, volume dan pendapatan per produk, proporsi metode pembayaran dan lokasi, tren transaksi dan pendapatan bulanan, serta heatmap korelasi.

## Temuan Utama

> Angka diambil dari laporan PDF dan notebook, lalu dicocokkan dengan dataset.

### 1. Coffee ramai, Salad paling menguntungkan

| Produk | Pendapatan |
| --- | ---: |
| Salad | $19.075 |
| Sandwich | $13.716 |
| Smoothie | $13.344 |
| Coffee | $13.044 |
| Juice | $10.515 |
| Cake | $10.404 |
| Tea | $5.433 |
| Cookie | $3.585 |

Coffee tercatat paling laku (1.780 unit transaksi) tetapi hanya peringkat keempat dalam pendapatan, sedangkan Salad unggul karena harga per unit lebih tinggi.

### 2. Permintaan stabil sepanjang tahun

Transaksi bulanan berkisar **766–886** dan pendapatan bulanan sekitar **$6,9 ribu–$7,7 ribu**. Puncak pada Oktober ($7.738,5; 886 transaksi) dan titik terendah pada Februari.

### 3. Nilai transaksi dan korelasi

Rata-rata `Total Spent` $8,93, median $8, Q3 $12, rentang $1–$25.

| Pasangan | Korelasi |
| --- | ---: |
| Quantity – Total Spent | 0,70 |
| Price Per Unit – Total Spent | 0,64 |
| Quantity – Price Per Unit | 0,01 |

### 4. Banyak data pembayaran dan lokasi tidak tercatat

| Metode pembayaran | Porsi | Lokasi | Porsi |
| --- | ---: | --- | ---: |
| Digital Wallet | 22,9% | Takeaway | 30,2% |
| Credit Card | 22,7% | In-store | 30,2% |
| Cash | 22,6% | Not Specified | 39,6% |
| Not Specified | 31,8% | | |

Ketiga metode pembayaran hampir seimbang, tetapi sepertiga hingga dua perlima catatan tidak memiliki informasi sehingga kesimpulan tentang pembayaran dan lokasi harus dibaca hati-hati.

## Rekomendasi

1. **Buat bundle Coffee + Salad/Sandwich.** Coffee menarik pelanggan, sedangkan Salad dan Sandwich menaikkan nilai transaksi.
2. **Jaga stok dan jadwal shift tetap merata** sepanjang tahun, dengan penguatan ringan pada bulan puncak seperti Oktober.
3. **Perbaiki pencatatan di kasir** agar metode pembayaran dan lokasi selalu terisi.

## Penulis

**Ahmad Faiz Ali Azmi**
Lulusan Teknik Informatika, Universitas Brawijaya. Peserta Offline Bootcamp Data Analyst Batch 4 di dibimbing.id.

GitHub: [@faizlzm](https://github.com/faizlzm)
