# Laporan Proyek Machine Learning - Arif Maulana Insaf

## Project Overview
Di era informasi, pembaca dihadapkan pada ribuan pilihan buku, membuat pencarian bacaan yang relevan menjadi tantangan tersendiri. Sistem rekomendasi hadir untuk membantu pengguna menemukan buku yang sesuai dengan minat dan preferensinya secara otomatis. Sistem ini tidak hanya meningkatkan kenyamanan pengguna tetapi juga mendorong peningkatan engagement serta penjualan buku.

Menurut riset [McKinsey](https://www.mckinsey.com/industries/technology-media-and-telecommunications/our-insights/the-future-of-personalization-and-how-to-get-ready-for-it), 35% dari pembelian di Amazon berasal dari sistem rekomendasi. Oleh karena itu, membangun sistem rekomendasi yang akurat sangat penting untuk mengoptimalkan pengalaman pengguna.

Pada bagian ini, Kamu perlu menuliskan latar belakang yang relevan dengan proyek yang diangkat.
kenapa sistem rekomendasi ini dibutuhkan:
| # | mengapa | alasan |
|----|-------|---------|
| 1 | meningkatkan penjualan | dengan adanya sistem rekomendasi iniperningkatannya bisa sampai 30% |
| 2 | mengurangin waktu untuk mencari buku | dengan adanya sistem rekomendasi ini user dapat dengan mudah mencari buku yang sesuai dengan keinginanya |



**Rubrik/Kriteria Tambahan (Opsional)**:
- Jelaskan mengapa dan bagaimana masalah tersebut harus diselesaikan
- Menyertakan hasil riset terkait atau referensi. Referensi yang diberikan harus berasal dari sumber yang kredibel dan author yang jelas.
- Format Referensi dapat mengacu pada penulisan sitasi [IEEE](https://journals.ieeeauthorcenter.ieee.org/wp-content/uploads/sites/7/IEEE_Reference_Guide.pdf), [APA](https://www.mendeley.com/guides/apa-citation-guide/) atau secara umum seperti [di sini](https://penerbitdeepublish.com/menulis-buku-membuat-sitasi-dengan-mudah/)
- Sumber yang bisa digunakan [Scholar](https://scholar.google.com/)

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

Semua poin di atas harus diuraikan dengan jelas. Anda bebas menuliskan berapa pernyataan masalah dan juga goals yang diinginkan.

**Rubrik/Kriteria Tambahan (Opsional)**:
- Menambahkan bagian “Solution Approach” yang menguraikan cara untuk meraih goals. Bagian ini dibuat dengan ketentuan sebagai berikut: 

    ### Solution statements
    - Mengajukan 2 atau lebih solution approach (algoritma atau pendekatan sistem rekomendasi).
  
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


Selanjutnya, uraikanlah seluruh variabel atau fitur pada data. Sebagai contoh:  

Variabel-variabel pada Restaurant UCI dataset adalah sebagai berikut:
- accepts : merupakan jenis pembayaran yang diterima pada restoran tertentu.
- cuisine : merupakan jenis masakan yang disajikan pada restoran.
- dst

**Rubrik/Kriteria Tambahan (Opsional)**:
- Melakukan beberapa tahapan yang diperlukan untuk memahami data, contohnya teknik visualisasi data beserta insight atau exploratory data analysis.
# Exploratory Data Analysis (EDA)
analysis eksporasi data mengungkapkan beberapa hal penting

#### univariate
![image](https://github.com/user-attachments/assets/3d559930-e10e-4233-832f-81f6a326610b)
![image](https://github.com/user-attachments/assets/808ef4bc-6ff6-42c2-8140-fc44393f61e1)

distribusi rating 

![image](https://github.com/user-attachments/assets/6265d300-332f-49c3-84d7-8d117a977d38)

top 10 bahasa 

#### multivariate
![image](https://github.com/user-attachments/assets/1abbb954-4d8a-41c9-a271-d2ba68ed66da)
korelasi fitur numerik
![image](https://github.com/user-attachments/assets/b61784f8-5ff0-446e-b53a-6d63a561621f)
hubungan antara jumlah rating dan rata-rata rating
![image](https://github.com/user-attachments/assets/b5162071-4a96-4ca9-a7de-c37a5b7123b3)
hubungan antara jumlah review dan rata-rata rating


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

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menjelaskan proses data preparation yang dilakukan
- Menjelaskan alasan mengapa diperlukan tahapan data preparation tersebut.

## Modeling
Tahapan ini membahas mengenai model sisten rekomendasi yang Anda buat untuk menyelesaikan permasalahan. Sajikan top-N recommendation sebagai output.

## 📊 Modeling and Results

Tahapan ini menjelaskan pembuatan sistem rekomendasi untuk membantu pengguna menemukan buku yang relevan berdasarkan data yang tersedia. Kami menggunakan **dua pendekatan berbeda** agar sistem lebih komprehensif: **Content-Based Filtering** dan **Collaborative Filtering**.

---

### 🧠 1. Content-Based Filtering (CBF)

#### ✔️ Penjelasan
Pendekatan ini merekomendasikan buku berdasarkan kemiripan konten (fitur) seperti:
- Penulis (`authors`)
- Penerbit (`publisher`)
- Bahasa (`language_code`)
- Rating rata-rata, jumlah halaman, jumlah rating, dan jumlah review.

Semua informasi dikombinasikan menjadi representasi fitur menggunakan:
- `TF-IDF Vectorizer` untuk fitur teks.
- `MinMaxScaler` untuk fitur numerik.
- `cosine_similarity` untuk menghitung kemiripan antar buku.

#### 📌 Output Top-5 Recommendation (contoh: *Harry Potter*):


#### ➕ Kelebihan
- Tidak membutuhkan data user-rating.
- Cocok untuk item baru (cold start item).
- Rekomendasi sangat relevan karena berdasarkan isi buku.

#### ➖ Kekurangan
- Terlalu fokus pada kemiripan konten, bisa membatasi eksplorasi.
- Tidak bisa memberikan rekomendasi yang bersifat “serendipitous” atau mengejutkan.

---

### 👥 2. Collaborative Filtering (CF) - Matrix Factorization (SVD)

#### ✔️ Penjelasan
Pendekatan ini merekomendasikan buku berdasarkan pola rating dari pengguna lain. Karena data pengguna tidak tersedia, kami menggunakan **dummy dataset** (10 user × 10 buku) dengan rating acak.

Menggunakan algoritma:
- `SVD (Singular Value Decomposition)` dari library `surprise`.
- Evaluasi menggunakan **Root Mean Square Error (RMSE)**.

#### 🧪 Output Top-5 Recommendation untuk `user_1`:




**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menyajikan dua solusi rekomendasi dengan algoritma yang berbeda.
- Menjelaskan kelebihan dan kekurangan dari solusi/pendekatan yang dipilih.

## Evaluation
Pada bagian ini Anda perlu menyebutkan metrik evaluasi yang digunakan. Kemudian, jelaskan hasil proyek berdasarkan metrik evaluasi tersebut.

## ✅ Evaluation

Evaluasi dilakukan untuk mengukur seberapa baik sistem rekomendasi memberikan hasil yang relevan terhadap pengguna. Karena proyek ini menggunakan dua pendekatan yang berbeda, maka metrik evaluasi yang digunakan juga berbeda dan disesuaikan dengan masing-masing metode.

---

### 🧠 1. Content-Based Filtering Evaluation

#### ✔️ Metrik: Cosine Similarity

Pendekatan content-based menggunakan **cosine similarity** untuk mengukur kemiripan antar buku. Semakin tinggi skor cosine similarity, semakin mirip dua buku berdasarkan fitur yang digunakan.

#### 📌 Formula Cosine Similarity:

\[
\text{cosine\_similarity} = \frac{A \cdot B}{\|A\| \times \|B\|}
\]

Di mana:
- \( A \) dan \( B \) adalah vektor representasi dua item.
- Hasil berada dalam rentang [0, 1], dengan 1 menunjukkan kemiripan sempurna.

#### 📈 Hasil Evaluasi:
Model mampu merekomendasikan buku-buku yang sangat mirip dari segi konten (penulis, genre, popularitas), terutama untuk buku-buku populer seperti "Harry Potter". Namun, model cenderung hanya merekomendasikan buku-buku dari seri yang sama, sehingga eksplorasi terbatas.

---

### 👥 2. Collaborative Filtering Evaluation (SVD)

#### ✔️ Metrik: Root Mean Square Error (RMSE)

Untuk model **Collaborative Filtering** menggunakan **Singular Value Decomposition (SVD)**, evaluasi dilakukan dengan membandingkan prediksi rating terhadap data aktual menggunakan RMSE.

#### 📌 Formula RMSE:

\[
\text{RMSE} = \sqrt{\frac{1}{n} \sum_{i=1}^{n}(r_i - \hat{r}_i)^2}
\]

Di mana:
- \( r_i \) adalah rating aktual,
- \( \hat{r}_i \) adalah rating hasil prediksi,
- Semakin kecil nilai RMSE, semakin baik performa model.

#### 📈 Hasil Evaluasi:
Model menghasilkan **RMSE = 0.83** pada data uji simulasi. Nilai ini menunjukkan bahwa prediksi cukup dekat dengan nilai sebenarnya, mengingat data yang digunakan adalah hasil random sampling yang terbatas. Dalam konteks data asli (tanpa rating user), RMSE ini bersifat ilustratif.

---

### 🔍 Kesimpulan Evaluasi

| Pendekatan            | Metrik         | Nilai       | Interpretasi                                                  |
|-----------------------|----------------|-------------|---------------------------------------------------------------|
| Content-Based         | Cosine Similarity | Top-N hasil | Memberikan hasil relevan, tapi eksplorasi terbatas             |
| Collaborative (SVD)   | RMSE           | 0.83        | Prediksi cukup akurat meskipun dengan data simulasi sederhana |

Evaluasi menunjukkan bahwa kedua pendekatan memiliki potensi yang kuat dan bisa saling melengkapi dalam sistem rekomendasi hybrid yang lebih robust.





Ingatlah, metrik evaluasi yang digunakan harus sesuai dengan konteks data, problem statement, dan solusi yang diinginkan.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menjelaskan formula metrik dan bagaimana metrik tersebut bekerja.

**---Ini adalah bagian akhir laporan---**

_Catatan:_
- _Anda dapat menambahkan gambar, kode, atau tabel ke dalam laporan jika diperlukan. Temukan caranya pada contoh dokumen markdown di situs editor [Dillinger](https://dillinger.io/), [Github Guides: Mastering markdown](https://guides.github.com/features/mastering-markdown/), atau sumber lain di internet. Semangat!_
- Jika terdapat penjelasan yang harus menyertakan code snippet, tuliskan dengan sewajarnya. Tidak perlu menuliskan keseluruhan kode project, cukup bagian yang ingin dijelaskan saja.
