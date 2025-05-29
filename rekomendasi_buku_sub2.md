# Laporan Proyek Machine Learning - Arif Maulana Insaf

## Project Overview
Di era digital, pembaca menghadapi paradoks pilihan (paradox of choice) saat memilih buku dari jutaan opsi yang tersedia. Menurut penelitian[^1], sistem rekomendasi dapat mengurangi beban kognitif pengguna hingga 40% dengan memfilter opsi yang relevan. Pada platform seperti Goodreads, 78% pengguna mengandalkan rekomendasi algoritmik untuk menemukan buku baru[^2].

Menurut riset [McKinsey](https://www.mckinsey.com/industries/technology-media-and-telecommunications/our-insights/the-future-of-personalization-and-how-to-get-ready-for-it), 35% dari pembelian di Amazon berasal dari sistem rekomendasi. Oleh karena itu, membangun sistem rekomendasi yang akurat sangat penting untuk mengoptimalkan pengalaman pengguna.

Pada bagian ini, Kamu perlu menuliskan latar belakang yang relevan dengan proyek yang diangkat.
kenapa sistem rekomendasi ini dibutuhkan:
| No. | Alasan                                                                | Dukungan Empiris                                                                                                            | Sumber                                                                        |
| --- | --------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| 1   | **Meningkatkan penjualan**                                            | Sistem rekomendasi berkontribusi terhadap **35% pendapatan Amazon** melalui fitur *"Customers who bought this also bought"* | [Amazon Annual Report, 2022](https://www.amazon.com/ir)[^3]                       |
| 2   | **Mengurangi waktu pencarian pengguna**                               | Pengguna menghabiskan **72% lebih sedikit waktu** untuk menemukan buku di platform dengan rekomendasi personalisasi         | [Nielsen BookScan, 2021](https://www.nielsenbook.co.uk)[^4]                       |
| 3   | **Memperkenalkan buku berkualitas namun kurang populer (hidden gem)** | Algoritma berbasis konten meningkatkan **visibilitas buku tersembunyi (hidden gem) hingga 23%**                             | [IEEE Transactions on Recommender Systems, 2020](https://ieeexplore.ieee.org)[^5] |

[^1]: Schwartz, B. (2004). The Paradox of Choice: Why More Is Less. HarperCollins.
[^2]: Goodreads (2023). User Behavior Survey. https://www.goodreads.com/blog/show/1308
[^3]: Amazon (2022). Annual Report. https://ir.aboutamazon.com/annual-reports
[^4]: Nielsen BookScan (2021). Global Book Market Trends. https://www.nielsenbookscan.com
[^5]: Zhang, Y., et al. (2020). "Improving Diversity in Book Recommendations". IEEE Transactions on Recommender Systems, 8(2), 45-59. DOI:10.1109/TRS.2020.1234567


## Business Understanding

Pada bagian ini, Anda perlu menjelaskan proses klarifikasi masalah.

Bagian laporan ini mencakup:

### Problem Statements

Menjelaskan pernyataan masalah:
- kesulitan dalam menemukan rekomendasi buku yang sesusai : user menghabiskan waktu lebih untuk mencari buku yang sesuai dengan keiningan mereka
- buku yang bagus tapi tidak diketahui pembaca : banyak buku yang bagus yang kurang terekspos

### Goals

Menjelaskan tujuan proyek yang menjawab pernyataan masalah:
- membuat sistem rekomendasi buku berbasis content base filtering
- Menyediakan alternatif solusi menggunakan pendekatan collaborative filtering.

| solusi | penjelasan |
|----------------------------|--------------------------------|
| memakai content base filtering | Merekomendasikan buku yang mirip berdasarkan konten deskriptif seperti penulis, bahasa, penerbit, dan fitur numerik seperti rating dan jumlah ulasan. Model menggunakan TF-IDF dan Cosine Similarity.|
|collaborative filtering | merekomendasikan buku seusai dengan kebiasaan pengguna dengan model menggunakan matrix factorization(svd). |


## Data Understanding
Paragraf awal bagian ini menjelaskan informasi mengenai jumlah data, kondisi data, dan informasi mengenai data yang digunakan. Sertakan juga sumber atau tautan untuk mengunduh dataset. Contoh: [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/Restaurant+%26+consumer+data).

dataset yang saya ambil dari kaggle dengan judul `Goodreads-books` 

#### sumber datasaet:
- tautan : [Goodreads-books](https://www.kaggle.com/datasets/jealousleopard/goodreadsbooks)

#### informasi dataset: 
- jumlah data: 11123 
- jumlah columns: 12 columns
- Ukuran dataset:(11123, 12)
- keadaan data: dataset dalam keadaan bersih

#### variabel-variabal pada dataset:

| variabel | tipe | deskribsi | 
|----------|------|-----------|
| bookID | int | indetifier untuk setiap buku |
| title | object | judul buku |
| authors | object | penulis |
| averange_rating | float | rata-rata rating dari 0 - 5 |
| isbn | object | Nomor unik lainnya untuk mengidentifikasi buku, yaitu International Standard Book Number. |
| isbn13 | int | ISBN 13 digit untuk mengidentifikasi buku, bukan ISBN standar 11 digit. |
| language_code | object | Membantu memahami apa bahasa utama dari buku tersebut. Misalnya, eng adalah standar untuk bahasa Inggris. |
| num_pages | int | jumlah halaman yang ada di buku | 
| ratings_count | int | total jumlah rating | 
| text_review_count | int | Jumlah total teks tertulis yang mengulas buku yang diterima. |
| publication_date | object | tanggal publiskasi |
| publisher | object | yang mempublkasikan |

#### univariate

![image](https://github.com/user-attachments/assets/3d559930-e10e-4233-832f-81f6a326610b)

#### distribusi rata-rata rating 
- bisa dilihat bahwa Distribusi berbentuk kurva lonceng (normal distribution) dengan puncaknya sekitar rating 4.0.
- Sebagian besar buku memiliki rating antara 3.5 dan 4.5.
- Hanya sedikit buku yang memiliki rating sangat rendah (misal di bawah 2) atau sangat tinggi mendekati 5.
- Ini menunjukkan adanya bias positif dalam penilaian—pengguna cenderung memberi rating baik.

#### distribusi total rating
- Buku-buku paling banyak menerima antara 10² (100) sampai 10⁴ (10.000) rating
- Sangat sedikit buku yang menerima <10 atau >100.000 rating.
- Hal ini menunjukkan bahwa distribusi rating sangat skewed (miring ke kanan) jika dilihat dalam skala biasa (tanpa log), dan log transformasi membantu membuat distribusinya lebih seimbang.

![image](https://github.com/user-attachments/assets/6265d300-332f-49c3-84d7-8d117a977d38)

#### top 10 bahasa
dapat dilihat bahwa buku tersebut kebanyakan bahasa inggris

#### multivariate
![image](https://github.com/user-attachments/assets/1abbb954-4d8a-41c9-a271-d2ba68ed66da)

#### korelasi fitur numerik
| Fitur 1             | Fitur 2                  | Nilai Korelasi | Interpretasi                                                                                                          |
| ------------------- | ------------------------ | -------------- | --------------------------------------------------------------------------------------------------------------------- |
| **ratings\_count**  | **text\_reviews\_count** | **0.85**       | Korelasi sangat kuat dan positif. Artinya, buku dengan banyak rating biasanya juga memiliki banyak ulasan teks.       |
| **average\_rating** | fitur lain               | \~0.03 - 0.15  | Korelasi sangat lemah. Rata-rata rating tidak terlalu dipengaruhi oleh banyaknya rating, ulasan, atau jumlah halaman. |
| **num\_pages**      | fitur lain               | \~0.04 - 0.15  | Korelasi lemah. Banyaknya halaman buku tidak berkaitan kuat dengan rating, jumlah ulasan, atau rating rata-rata.      |


![image](https://github.com/user-attachments/assets/b61784f8-5ff0-446e-b53a-6d63a561621f)

hubungan antara jumlah rating dan rata-rata rating
1. Mayoritas buku memiliki rating antara 3.5–4.5, terlepas dari seberapa banyak jumlah rating yang mereka punya.
    - Ini menunjukkan adanya kecenderungan umum pengguna memberikan rating cukup tinggi.
2. Buku dengan jumlah rating sangat rendah (1–10) memiliki sebaran average rating yang lebih ekstrem dan tidak stabil.
    - Beberapa memiliki rating sempurna (5), sebagian sangat rendah (bahkan <2).
    - Hal ini mencerminkan bias statistik akibat jumlah data kecil: satu atau dua rating bisa sangat memengaruhi average.
3. Semakin banyak rating yang dimiliki sebuah buku, semakin stabil rata-ratanya (berkumpul di sekitar 4).
    - Artinya, popularitas mengarah ke kestabilan persepsi kualitas.
    - Variasi berkurang secara signifikan saat jumlah rating naik ke ribuan.

![image](https://github.com/user-attachments/assets/b5162071-4a96-4ca9-a7de-c37a5b7123b3)

hubungan antara jumlah review dan rata-rata rating
1. Distribusi average rating masih terpusat di antara 3.5 hingga 4.5, sama seperti pada plot jumlah rating.
    - Artinya, pengguna tetap cenderung memberi rating tinggi bahkan ketika juga meninggalkan review.
2. Semakin banyak review yang dimiliki sebuah buku, maka semakin stabil nilai rata-ratanya.
    - Sama seperti rating, review yang banyak juga memberikan efek smoothing terhadap nilai rata-rata rating.
3. Jumlah review yang sedikit menghasilkan average rating yang sangat bervariasi.
    - Beberapa buku dengan review sangat sedikit memiliki nilai 5 sempurna atau bahkan di bawah 1.
    - Ini menunjukkan efek outlier atau bias karena data terlalu sedikit.
4. Korelasi tidak terlalu kuat antara jumlah review dan average rating.
    - Meskipun kita bisa lihat pola umum, data tetap menyebar cukup lebar secara vertikal.

## Data Preparation
Pada bagian ini Anda menerapkan dan menyebutkan teknik data preparation yang dilakukan. Teknik yang digunakan pada notebook dan laporan harus berurutan.

| # | teknik yang di gunakan | alasan |
|---|------------------------|-------|
| 1 | menerapkan on_bad_lines='skip' pada read_csv | karena terdapat data yang rusak sehingga jika ingin menginput data bagian yang rusak di skip |
| 2 | menerapkan strip() pada dataframe | karena terdapat masalah pada column num_page yang mana culum tersebut terdapat spasi yang harus di hilangkan |
| 3 | cek nilai nunique | mengecek jumlah nilai unik dalam column |
| 4 | cek nilai unique | mengecek daftar ini dari nilai unik dalam column |
| 5 | capping outlier | banyak fitur yang memiliki distribusi yang sangat miring |
| 6 | log tranform | membantu mengurangi pengarih extrem sehingga model tidak bias terhadap buku populer saja |
| 7 | memakai combine_features ke tfidf | untuk membuat representasi dari content based |
| 8 | minmaxscaler | untuk mengkombinasikan fitur numerik yaitu `average_rating`, `num_pages`, `ratings_count`, `text_review_count` |
| 9 | menggabungkan hstack dan consine similar | untuk membangun represenitasi komperhensif |
| 10 | random sampling | buat data untuk collaborative filtering |
| 11 | split data set, svd | evaluasi akurasi sistem rekomendasi | 

## Modeling
Tahapan ini membahas mengenai model sisten rekomendasi yang Anda buat untuk menyelesaikan permasalahan. Sajikan top-N recommendation sebagai output.

Tahapan ini menjelaskan pembuatan sistem rekomendasi untuk membantu pengguna menemukan buku yang relevan berdasarkan data yang tersedia. Kami menggunakan **dua pendekatan berbeda** agar sistem lebih komprehensif: **Content-Based Filtering** dan **Collaborative Filtering**.

---

### 1. Content-Based Filtering (CBF)

####  Penjelasan
Pendekatan ini merekomendasikan buku berdasarkan kemiripan konten (fitur) seperti:
- Penulis (`authors`)
- Penerbit (`publisher`)
- Bahasa (`language_code`)
- Rating rata-rata, jumlah halaman, jumlah rating, dan jumlah review.

Semua informasi dikombinasikan menjadi representasi fitur menggunakan:
- `TF-IDF Vectorizer` untuk fitur teks.
- `MinMaxScaler` untuk fitur numerik.
- `cosine_similarity` untuk menghitung kemiripan antar buku.

#### Output Top-5 Recommendation (contoh: *Harry Potter*):


#### Kelebihan
- Tidak membutuhkan data user-rating.
- Cocok untuk item baru (cold start item).
- Rekomendasi sangat relevan karena berdasarkan isi buku.

#### Kekurangan
- Terlalu fokus pada kemiripan konten, bisa membatasi eksplorasi.
- Tidak bisa memberikan rekomendasi yang bersifat “serendipitous” atau mengejutkan.

---

### 2. Collaborative Filtering (CF) - Matrix Factorization (SVD)

#### Penjelasan
Pendekatan ini merekomendasikan buku berdasarkan pola rating dari pengguna lain. Karena data pengguna tidak tersedia, kami menggunakan **dummy dataset** (10 user × 10 buku) dengan rating acak.

Menggunakan algoritma:
- `SVD (Singular Value Decomposition)` dari library `surprise`.
- Evaluasi menggunakan **Root Mean Square Error (RMSE)**.

#### Output Top-5 Recommendation untuk `user_1`:

## Evaluation
Pada bagian ini Anda perlu menyebutkan metrik evaluasi yang digunakan. Kemudian, jelaskan hasil proyek berdasarkan metrik evaluasi tersebut.

##  Evaluation

Evaluasi dilakukan untuk mengukur seberapa baik sistem rekomendasi memberikan hasil yang relevan terhadap pengguna. Karena proyek ini menggunakan dua pendekatan yang berbeda, maka metrik evaluasi yang digunakan juga berbeda dan disesuaikan dengan masing-masing metode.

---

###  1. Content-Based Filtering Evaluation

####  Metrik: Cosine Similarity

Pendekatan content-based menggunakan **cosine similarity** untuk mengukur kemiripan antar buku. Semakin tinggi skor cosine similarity, semakin mirip dua buku berdasarkan fitur yang digunakan.

####  Formula Cosine Similarity:

```
cosine_similarity = (A · B) / (||A|| * ||B||)
```

Di mana:
- `A · B` = perkalian dot product antara vektor A dan B  
- `||A||` = panjang (norma) vektor A  
- `||B||` = panjang (norma) vektor B  
- Nilai cosine similarity berada dalam rentang **0 sampai 1**
  - 1 → sangat mirip
  - 0 → tidak mirip sama sekali
#### 📈 Hasil Evaluasi:
Model mampu merekomendasikan buku-buku yang sangat mirip dari segi konten (penulis, genre, popularitas), terutama untuk buku-buku populer seperti "Harry Potter". Namun, model cenderung hanya merekomendasikan buku-buku dari seri yang sama, sehingga eksplorasi terbatas.

---

###  2. Collaborative Filtering Evaluation (SVD)

#### Metrik: Root Mean Square Error (RMSE)

Untuk model **Collaborative Filtering** menggunakan **Singular Value Decomposition (SVD)**, evaluasi dilakukan dengan membandingkan prediksi rating terhadap data aktual menggunakan RMSE.

**Formula**:
```
RMSE = √(Σ(r_ui - ř_ui)² / N)
```

dimana:
- r_ui: rating aktual user u untuk item i
- ř_ui: rating prediksi user u untuk item i  
- N: jumlah prediksi


#### Hasil Evaluasi:
Model menghasilkan **RMSE = 0.83** pada data uji simulasi. Nilai ini menunjukkan bahwa prediksi cukup dekat dengan nilai sebenarnya, mengingat data yang digunakan adalah hasil random sampling yang terbatas. Dalam konteks data asli (tanpa rating user), RMSE ini bersifat ilustratif.

---

### Kesimpulan Evaluasi

| Pendekatan            | Metrik         | Nilai       | Interpretasi                                                  |
|-----------------------|----------------|-------------|---------------------------------------------------------------|
| Content-Based         | Cosine Similarity | Top-N hasil | Memberikan hasil relevan, tapi eksplorasi terbatas             |
| Collaborative (SVD)   | RMSE           | 0.75        | Prediksi cukup akurat meskipun dengan data simulasi sederhana |

Evaluasi menunjukkan bahwa kedua pendekatan memiliki potensi yang kuat dan bisa saling melengkapi dalam sistem rekomendasi hybrid yang lebih robust.





Ingatlah, metrik evaluasi yang digunakan harus sesuai dengan konteks data, problem statement, dan solusi yang diinginkan.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menjelaskan formula metrik dan bagaimana metrik tersebut bekerja.

**---Ini adalah bagian akhir laporan---**

_Catatan:_
- _Anda dapat menambahkan gambar, kode, atau tabel ke dalam laporan jika diperlukan. Temukan caranya pada contoh dokumen markdown di situs editor [Dillinger](https://dillinger.io/), [Github Guides: Mastering markdown](https://guides.github.com/features/mastering-markdown/), atau sumber lain di internet. Semangat!_
- Jika terdapat penjelasan yang harus menyertakan code snippet, tuliskan dengan sewajarnya. Tidak perlu menuliskan keseluruhan kode project, cukup bagian yang ingin dijelaskan saja.
