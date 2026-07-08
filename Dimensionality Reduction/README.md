# Dimensionality Reduction: PCA & t-SNE untuk Segmentasi Pelanggan Kartu Kredit

## Deskripsi Kasus

Kasus penggunaan yang diangkat adalah **eksplorasi cluster** untuk segmentasi pelanggan. Perusahaan kartu kredit ingin mengetahui ada berapa "tipe" pelanggan berdasarkan pola penggunaan kartu mereka (saldo, jumlah belanja, penggunaan cash advance, dll), agar dapat memberikan strategi marketing/penawaran yang sesuai untuk tiap tipe pelanggan. Dataset ini memiliki 17 fitur perilaku per pelanggan, yang mustahil dilihat pola pengelompokannya secara langsung. Dimensionality reduction dibutuhkan untuk meringkas 17 fitur ini menjadi 2 dimensi yang bisa divisualisasikan, sehingga pola/cluster pelanggan bisa terlihat dan diinterpretasikan.

## Dataset yang Digunakan

**[Credit Card Dataset for Clustering](https://www.kaggle.com/datasets/arjunbhasin2013/ccdata)** (Kaggle, oleh arjunbhasin2013):
- Merangkum perilaku penggunaan kartu kredit dari sekitar **8.950 pelanggan aktif** selama 6 bulan terakhir
- **17 variabel perilaku** per pelanggan: BALANCE, PURCHASES, ONEOFF_PURCHASES, INSTALLMENTS_PURCHASES, CASH_ADVANCE, PURCHASES_FREQUENCY, CASH_ADVANCE_FREQUENCY, CREDIT_LIMIT, PAYMENTS, MINIMUM_PAYMENTS, PRC_FULL_PAYMENT, TENURE, dll
- Terdapat sedikit missing values pada kolom MINIMUM_PAYMENTS (313 baris) dan CREDIT_LIMIT (1 baris), yang diisi menggunakan median kolom masing-masing

## Ringkasan Metode

1. **Data Cleaning:** Membuang kolom CUST_ID (hanya identifier), mengisi missing value dengan median.
2. **Preprocessing:** Data distandardisasi menggunakan `StandardScaler`, karena baik PCA maupun t-SNE sensitif terhadap skala fitur.
3. **KMeans (k=4):** Dijalankan pada data yang sudah distandardisasi untuk mendapatkan label cluster pelanggan, yang digunakan untuk mewarnai visualisasi hasil PCA dan t-SNE.
4. **PCA:** Mereduksi 17 dimensi menjadi 2 dimensi dengan mencari kombinasi linear fitur yang menangkap variansi terbesar.
5. **t-SNE:** Mereduksi 17 dimensi (dari subset 1.000 sampel) menjadi 2 dimensi dengan mempertahankan hubungan tetangga terdekat secara non-linear (`perplexity=30`, `max_iter=1000`, `random_state=42`).

## Karakteristik Cluster Pelanggan

| Cluster | Jumlah Pelanggan | Karakteristik |
|---|---|---|
| 0 | 3.977 | Saldo & belanja rendah, limit kecil → **pelanggan pasif** |
| 1 | 409 | Saldo & belanja tertinggi, cukup sering bayar penuh → **pelanggan premium / big spender** |
| 2 | 1.197 | Cash advance sangat tinggi, jarang bayar penuh → **pengguna cash advance (revolver)** |
| 3 | 3.367 | Belanja moderat, disiplin bayar penuh → **pelanggan sehat & disiplin** |

## Analisis Hasil

### Hasil PCA

PCA berhasil mereduksi data dari bentuk (8950, 17) menjadi (8950, 2), dengan PC1 dan PC2 bersama-sama menangkap sekitar **47,6%** variansi data (PC1 ≈ 27,3%, PC2 ≈ 20,3%). Angka ini jauh lebih tinggi dibanding kasus data gambar pada umumnya, karena fitur-fitur perilaku finansial ini banyak yang berkorelasi linear satu sama lain (misalnya BALANCE dengan CREDIT_LIMIT). Dari visualisasi, cluster 1 (pelanggan premium) terlihat cukup jelas terpisah di salah satu sisi plot, sedangkan cluster 0 dan 3 (pelanggan pasif dan disiplin) cenderung tumpang tindih karena keduanya sama-sama memiliki saldo yang relatif rendah.

### Hasil t-SNE

Dibandingkan PCA, hasil t-SNE menunjukkan pemisahan antar cluster yang jauh lebih rapi. Kelompok pelanggan revolver (cash advance tinggi) dan pelanggan premium (big spender) terlihat membentuk kelompok yang lebih terpisah dari kelompok pelanggan pasif/disiplin, karena t-SNE mampu menangkap hubungan non-linear antar fitur perilaku yang tidak sepenuhnya tertangkap oleh PCA.

### Perbandingan PCA vs t-SNE

| Aspek | PCA | t-SNE |
|---|---|---|
| Jenis hubungan yang ditangkap | Linear | Non-linear |
| Kecepatan komputasi | Cepat | Lebih lambat |
| Sifat hasil | Deterministik | Stokastik (dipengaruhi random_state) |
| Interpretasi komponen | Jelas secara matematis (arah variansi terbesar) | Jarak antar cluster tidak selalu bermakna literal |
| Pemisahan cluster pada kasus ini | Cukup baik, tapi ada tumpang tindih | Lebih jelas dan rapi |

### Metode Mana yang Lebih Sesuai

Untuk kasus **eksplorasi cluster pelanggan**, **t-SNE lebih sesuai** karena mampu menunjukkan pemisahan segmen pelanggan yang lebih jelas, sehingga tim marketing bisa lebih yakin dalam menentukan strategi per segmen. Namun demikian, PCA tetap berguna sebagai langkah awal yang cepat, misalnya untuk mereduksi redundansi fitur (karena beberapa fitur asli seperti BALANCE dan CREDIT_LIMIT memang saling berkorelasi tinggi) sebelum clustering dijalankan pada dataset yang jauh lebih besar.

## Anggota Kelompok
- Muhammad Fadhil Aprilino (24523175)
- Pradipta Pramatya Panhar (24523052)

