# Analisis Prediksi Churn Pelanggan E-Commerce

Proyek eksplorasi end-to-end untuk memahami pola churn pelanggan pada dataset e-commerce Olist (Brazil). Membangun pipeline analisis mulai dari integrasi data multi-sumber, feature engineering, hingga perbandingan dua model klasifikasi: **Logistic Regression** dan **Random Forest**.

> Proyek ini merupakan *proof of concept* dalam konteks akademik. Hasil model mencerminkan karakteristik dataset, mayoritas pelanggan Olist hanya bertransaksi satu kali yang menjadi batasan natural untuk pemodelan churn. Keterbatasan ini didokumentasikan secara eksplisit sebagai bagian dari pembelajaran.

---

## Konteks

| | |
|---|---|
| **Mata Kuliah** | Teknologi Big Data |
| **Semester** | 5 |
| **Jenis** | Proyek Kelompok (6 orang) |
| **Peran** | Project Manager — koordinasi tim, perencanaan pipeline, penyusunan laporan |
| **Periode** | 2025 |

---

## Tujuan Proyek

1. Mengidentifikasi faktor yang paling berpengaruh terhadap churn pelanggan e-commerce.
2. Membangun model klasifikasi sebagai *proof of concept* untuk prediksi churn.
3. Menyusun laporan yang merangkum metodologi, temuan, dan rekomendasi.

---

## Dataset

**Sumber:** [Brazilian E-Commerce Public Dataset by Olist (Kaggle)](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)

**Skala data setelah integrasi:**
- 119.143 baris transaksi
- 25 kolom
- 5 tabel yang di-*join*: orders, order_reviews, order_payments, order_items, customers

Dataset tidak di-*commit* ke repo ini karena ukurannya dan ketentuan lisensi Kaggle (CC BY-NC-SA). Silakan unduh langsung dari tautan di atas.

---

## Metodologi

### 1. Data Preparation
- Integrasi 5 tabel menggunakan `pandas.merge` dengan skema *left join* berpusat pada tabel `orders`.
- Pengecekan *missing values* dan duplikat.

### 2. Feature Engineering
Dibangun empat fitur agregat per pelanggan unik (`customer_unique_id`):

| Fitur | Deskripsi |
|---|---|
| `total_transaksi` | Total nilai transaksi (harga + ongkir) |
| `jumlah_order` | Jumlah order unik |
| `rata_sentimen` | Rata-rata skor ulasan (1–5), *missing* diisi 3 (netral) |
| `durasi_hari` | Selisih hari antara order pertama dan terakhir |

### 3. Definisi Label Churn
Pelanggan diklasifikasikan sebagai **churn (= 1)** jika tidak melakukan transaksi dalam **180 hari** terakhir dari tanggal observasi.

### 4. Pemodelan
Dua algoritma dibandingkan dengan konfigurasi `class_weight="balanced"` untuk menangani ketidakseimbangan kelas:
- **Logistic Regression** (`max_iter=1000`)
- **Random Forest** (`n_estimators=100`)

---

## Hasil

### Logistic Regression

| Metrik | Nilai |
|---|---|
| Accuracy | 0.494 |
| Macro F1 | 0.486 |
| Precision (churn) | 0.746 |
| Recall (churn) | 0.433 |

### Random Forest
Diterapkan sebagai pembanding dengan performa lebih tinggi, namun menunjukkan indikasi *overfitting* pada *top predicted customers* (probabilitas prediksi = 1.0 untuk 10 pelanggan teratas).

### Visualisasi
Notebook berisi tujuh visualisasi utama:

**Analisis Deskriptif:**
1. Heatmap korelasi antar variabel numerik
2. Frekuensi pembelian per bulan
3. Distribusi sentimen ulasan pelanggan
4. Korelasi fitur dengan label churn

**Evaluasi Model:**
5. Distribusi prediksi churn
6. ROC Curve — Logistic Regression
7. ROC Curve — Random Forest

---

## Keterbatasan dan Pembelajaran

Transparansi atas keterbatasan proyek:

1. **Karakteristik dataset membatasi pemodelan churn.** Mayoritas pelanggan Olist adalah pembeli *single-transaction*, sehingga definisi churn berbasis waktu menghasilkan distribusi yang sangat miring dan membatasi kekuatan prediksi.
2. **Konfigurasi *train-test split* non-standar** (`test_size=0.7`). Idealnya proporsi data latih lebih besar; ini menjadi catatan untuk iterasi berikutnya.
3. **Logistic Regression memiliki akurasi mendekati tebakan acak** pada data yang sangat tidak seimbang ini. Hasil ini lebih tepat dibaca sebagai titik awal benchmark, bukan model siap pakai.
4. **Fitur yang digunakan terbatas**; penambahan fitur perilaku pembelian (RFM lengkap, kategori produk, waktu respon pengiriman) berpotensi meningkatkan daya prediktif.

Pembelajaran utama dari proyek ini bagi penulis: pentingnya **memahami karakteristik data sebelum memilih pendekatan pemodelan**, dan nilai dari dokumentasi keterbatasan yang jujur dalam laporan analisis.

---

## Teknologi

- **Bahasa:** Python 3
- **Lingkungan:** Google Colab
- **Pustaka utama:** Pandas, NumPy, Scikit-learn, Matplotlib, Seaborn

---

## Struktur Repository

```
├── notebook/
│   └── ANALISIS_CHURN.ipynb      # Notebook utama
├── laporan/
│   └── Laporan_Analisis_Churn.pdf # Laporan lengkap kelompok
├── visualisasi/                   # Screenshot visualisasi utama
│   ├── heatmap_korelasi.png
│   ├── frekuensi_pembelian.png
│   ├── distribusi_sentimen.png
│   ├── korelasi_fitur_churn.png
│   ├── distribusi_prediksi_churn.png
│   ├── roc_curve_logistic_regression.png
│   └── roc_curve_random_forest.png
└── README.md
```

---

## Kontak

**Azizah Awaliyah**
Mahasiswa Teknik Informatika — Universitas Hasanuddin
Email: azizahawaliyahjijah@gmail.com
