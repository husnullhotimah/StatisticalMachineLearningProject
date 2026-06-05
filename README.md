# Segmentasi Respon Emosional Audiens Berdasarkan Tipe Konten pada Facebook Live Sellers di Thailand

> Analisis clustering untuk memetakan pola respons emosional penonton Facebook Live Sellers di Thailand menggunakan algoritma **K-Means** dan **Simple K-Medoids (SKM)**.

---

## Deskripsi Proyek

Proyek ini bertujuan untuk mengelompokkan postingan Facebook Live Sellers di Thailand berdasarkan pola respons emosional audiens, yaitu **Love**, **Wow**, **Haha**, **Sad**, dan **Angry**. Data diperoleh dari [UCI Machine Learning Repository](https://archive.ics.uci.edu/).

Reaksi *Like* sengaja dipisahkan dari analisis karena bersifat *passive engagement* dan tidak mencerminkan kedalaman ekspresi emosional yang spesifik. Dua metode clustering, yaitu K-Means dan K-Medoids, dibandingkan untuk menguji konsistensi serta performa terbaik dalam mengelompokkan karakteristik postingan secara objektif.

---

## Dataset

| Variabel | Tipe | Keterangan |
|---|---|---|
| `status_id` | Integer | ID unik setiap postingan |
| `status_type` | Character | Tipe konten: `video`, `photo`, `link`, `status` |
| `status_published` | Character | Tanggal dan jam publikasi |
| `num_reactions` | Integer | Total seluruh reaksi |
| `num_comments` | Integer | Jumlah komentar |
| `num_shares` | Integer | Jumlah berbagi ulang |
| `num_likes` | Integer | Frekuensi reaksi Like |
| `num_loves` | Integer | Frekuensi reaksi Love |
| `num_wows` | Integer | Frekuensi reaksi Wow |
| `num_hahas` | Integer | Frekuensi reaksi Haha |
| `num_sads` | Integer | Frekuensi reaksi Sad |
| `num_angrys` | Integer | Frekuensi reaksi Angry |

- **Jumlah data awal:** 7050 postingan × 16 variabel
- **Jumlah data setelah preprocessing:** 3094 postingan (hanya postingan dengan minimal 1 reaksi emosional)

---

## Metodologi

### Pra-Proses Data
- Menghapus 4 kolom kosong (`Column1`-`Column4`)
- Memfilter postingan yang tidak memiliki reaksi emosional sama sekali (3956 postingan dikeluarkan)
- Feature engineering: mengubah frekuensi reaksi menjadi **proporsi**
- Standardisasi fitur menggunakan `scale()`

### Algoritma yang Digunakan

| Algoritma | K Optimal | Metode Penentuan K |
|---|---|---|
| K-Means | 5 | Elbow Method (WSS) |
| Simple K-Medoids (SKM) | 4 | Silhouette Method |

---

## Hasil Clustering

### K-Means (k = 5)

| Cluster | Emosi Dominan | Proporsi | Jumlah Postingan | Interpretasi |
|---|---|---|---|---|
| 1 | Sad | 76,0% | 102 (3,30%) | Konten memunculkan empati/simpati |
| 2 | Wow + Love | 65,8% + 31,2% | 580 (18,75%) | Konten mengagumkan dan disukai |
| 3 | Love | 90,8% | 2302 (74,40%) | Konten sangat positif |
| 4 | Haha | 77,4% | 87 (2,81%) | Konten humor/menghibur |
| 5 | Angry + Haha | 68,0% + 6,8% | 23 (0,74%) | Konten kontroversial/provokatif |

### Simple K-Medoids (k = 4)

| Cluster | Emosi Dominan | Proporsi | Jumlah Postingan | Interpretasi |
|---|---|---|---|---|
| 1 | Wow + Love | 56,3% + 40,2% | 770 (24,89%) | Konten mengejutkan sekaligus positif |
| 2 | Sad | 79,0% | 94 (3,04%) | Konten empatik |
| 3 | Love | 94,5% | 1972 (63,74%) | Konten sangat positif |
| 4 | Love + Haha | 44,4% + 40,5% | 258 (8,34%) | Konten positif dan menghibur |

---

## Evaluasi Model

| Metode | Rata-rata Silhouette Width |
|---|---|
| K-Means | 0.6818 |
| Simple K-Medoids | 0.6091 |

- **Rand Index** (kemiripan kedua metode): **0.8216** hasil kedua metode hampir konsisten
- K-Means menghasilkan segmentasi yang lebih detail, sedangkan SKM lebih ringkas namun robust terhadap pencilan

---

## Alat Analisis

- **Bahasa:** R
- **Packages:**
  - `tidyverse` - manipulasi dan visualisasi data
  - `psych` - statistik deskriptif
  - `ggcorrplot` - heatmap korelasi
  - `factoextra` - visualisasi clustering
  - `cluster` - Silhouette Coefficient
  - `kmed` - Simple K-Medoids (SKM)
  - `clusterCrit` - Rand Index
  - `gridExtra` - komposisi plot

---

## Temuan Utama

- Sebagian besar postingan (>60%) memperoleh dominasi reaksi **Love**, mengindikasikan bahwa konten live selling di Thailand umumnya diterima secara positif oleh audiens.
- Konten **video** cenderung mendapatkan lebih banyak reaksi Love, sedangkan konten **photo** lebih bervariasi dalam memunculkan respons emosional lainnya.
- Korelasi negatif terkuat ditemukan antara reaksi **Love** dan **Wow** (r = -0,76), menunjukkan bahwa keduanya jarang muncul bersamaan secara dominan.
- K-Means lebih unggul dalam nilai Silhouette, namun SKM lebih robust terhadap distribusi data yang sangat skewed.

---

## Lisensi

Dataset bersumber dari [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/488/facebook+live+sellers+in+thailand) dan digunakan untuk keperluan akademis.
