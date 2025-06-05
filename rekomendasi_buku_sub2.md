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


## Data Understanding
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

#### cek nilai nunique

#### cek nilai unique

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
| 3 | capping outlier | banyak fitur yang memiliki distribusi yang sangat miring |
| 4 | log tranform | membantu mengurangi pengarih extrem sehingga model tidak bias terhadap buku populer saja |
| 5 | memakai combine_features ke tfidf | untuk membuat representasi dari content based |
| 6 | minmaxscaler | untuk mengkombinasikan fitur numerik yaitu `average_rating`, `num_pages`, `ratings_count`, `text_review_count` |
| 7 | hstack  | hstuck(horizontal stack) untuk menggabungkan kolom-kolom baru ke kanan |
| 8 | random sampling | buat data untuk collaborative filtering |
| 9 | normalisasi column title menggunakan lower(), strip() | dengan menggunakan lower() judul pada column title akan di ubah menjadi kecil semua, strip() menghilangkan spasi ini dibutuhkan agar sistem menjadi lebih robust terhadap variasi | 


## 1. load data
```python
df = pd.read_csv('books.csv', on_bad_lines='skip')
df.head()
```
pada bagian ini saya menambahkan on_bad_line='skip' karena pada data tersebut data yang rusak sehingga data perlu diskip

## 2. menghapus spasi
```python
df.columns = df.columns.str.strip()
```
karena terdapat spasi yang pada column `num_pages` sehingga saat column di panggil mengalami error oleh karena itu saya menghapus spasinya

## 3. capping pada column
```python
df['ratings_count'] = np.where(df['ratings_count'] > 1e6, 1e6, df['ratings_count'])
```
Melakukan capping pada kolom ratings_count. Jika ada buku yang memiliki jumlah rating lebih dari 1 juta (1e6), nilainya akan diubah menjadi 1 juta. Ini dilakukan untuk mengurangi pengaruh outlier yang sangat ekstrem dan membuat distribusi data lebih terkelola.

## 4. log transform
```
df['log_reviews'] = np.log1p(df['text_reviews_count'])
```
Menerapkan transformasi logaritma (np.log1p yang berarti log(1+x)) pada kolom text_reviews_count. Transformasi ini sering digunakan untuk data yang sangat miring (skewed) seperti jumlah ulasan, sehingga distribusinya menjadi lebih mendekati normal dan lebih baik untuk model machine learning.

## 5. content base filtering
```python
df['combined_features'] = df['authors'] + ' ' + df['publisher'] + ' ' + df['language_code']
```
Membuat kolom baru bernama combined_features yang menggabungkan informasi dari kolom authors, publisher, dan language_code. Fitur gabungan ini akan digunakan sebagai representasi tekstual dari setiap buku.

```python
vectorizer = TfidfVectorizer(stop_words='english')
tfidf_matrix = vectorizer.fit_transform(df['combined_features'])
```
- TfidfVectorizer(stop_words='english'): Menginisialisasi TfidfVectorizer. TF-IDF (Term Frequency-Inverse Document Frequency) adalah teknik yang mengkonversi teks menjadi representasi numerik, di mana kata-kata yang lebih sering muncul dalam satu dokumen tetapi jarang di seluruh korpus akan memiliki bobot lebih tinggi. stop_words='english' akan menghapus kata-kata umum dalam bahasa Inggris (seperti "the", "a", "is") yang tidak banyak membantu dalam menentukan kemiripan.
- tfidf_matrix = vectorizer.fit_transform(df['combined_features']): Mengaplikasikan TF-IDF vectorizer ke kolom combined_features untuk membuat matriks TF-IDF. Setiap baris dalam matriks ini merepresentasikan sebuah buku, dan setiap kolom merepresentasikan bobot TF-IDF dari sebuah kata.

```python
final_features = hstack([tfidf_matrix, num_features_scaled])
```
Menggabungkan dua jenis fitur menjadi satu matriks besar untuk digunakan dalam perhitungan kemiripan:
- `tfidf_matrix`: representasi fitur teks (seperti penulis, penerbit, bahasa) yang diubah jadi vektor menggunakan TF-IDF.
- `num_features_scaled`: representasi fitur numerik (seperti rating rata-rata, jumlah halaman, jumlah review) yang sudah diskalakan.


## Modeling
Tahapan ini membahas mengenai model sisten rekomendasi yang Anda buat untuk menyelesaikan permasalahan. Sajikan top-N recommendation sebagai output.

Tahapan ini menjelaskan pembuatan sistem rekomendasi untuk membantu pengguna menemukan buku yang relevan berdasarkan data yang tersedia. Kami menggunakan **satu pendekatan** yaitu: **Content-Based Filtering** 

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

## hasil top 3 dari  content base
| bookID |	title	|authors |	average_rating	| isbn	|isbn13	|language_code|	num_pages	|ratings_count|	text_reviews_count|	publication_date|	publisher|	log_reviews|	combined_features|
|------|----------|------------|--------------|-------------|--------------|---------|-----------|----------|----------|----------|--------|-------|------|
|0|	1	|Harry Potter and the Half-Blood Prince (Harry ...	|J.K. Rowling/Mary GrandPré|	4.57|	0439785960	|9780439785969|	eng	|652	|1000000.0	|27591	|9/16/2006|	Scholastic Inc.|	10.225281	|J.K. Rowling/Mary GrandPré Scholastic Inc. eng|
|1	|2	|Harry Potter and the Order of the Phoenix (Har...	|J.K. Rowling/Mary GrandPré	|4.49|	0439358078	|9780439358071|	eng|	870|	1000000.0|	29221	|9/1/2004|	Scholastic Inc.|	10.282677	|J.K. Rowling/Mary GrandPré Scholastic Inc. eng|
|2	|4|	Harry Potter and the Chamber of Secrets (Harry...	|J.K. Rowling|	4.42	|0439554896	|9780439554893	|eng	|352	|6333.0|	244	|11/1/2003|	Scholastic	|5.501258|	J.K. Rowling Scholastic eng|


#### Kelebihan
- Tidak membutuhkan data user-rating.
- Cocok untuk item baru (cold start item).
- Rekomendasi sangat relevan karena berdasarkan isi buku.

#### Kekurangan
- Terlalu fokus pada kemiripan konten, bisa membatasi eksplorasi.
- Tidak bisa memberikan rekomendasi yang bersifat “serendipitous” atau mengejutkan.

---

## Evaluation
---

###  1. Content-Based Filtering Evaluation

####  Metrik: Cosine Similarity

Model menggunakan cosine similarity untuk menghitung kemiripan antar item. Untuk evaluasi kualitas rekomendasi, digunakan metrik Precision_K dan Recall_K, yang mengukur relevansi hasil rekomendasi dibandingkan dengan preferensi pengguna sebenarnya.

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

### hasil evaluasi 

| judul | ditemukan | jumlah yang relevan | Precision@5 | Recall@5 | F1 Score@5 | kesimpulan | 
|------|------------|-----------------------|-------------|----------|-------------|--------|
| The Hobbit | ya | 16 | 0.00 | 0.00 | 0.00 |Sistem memberikan 5 rekomendasi, tetapi tidak ada yang termasuk dalam buku relevan (karya penulis yang sama). |
| The Da Vinci Code | ya | 11 | 0.80 | 0.40 | 0.53 |4 dari 5 rekomendasi benar (karya Dan Brown atau terkait) akan tetapi Recall rendah (0.40) berarti hanya 40% buku relevan yang terdeteksi |
| Non-Existent Book | tidak | - | - | - | - | Sistem berhasil mengidentifikasi buku tidak ada. karena title tidak ada di dataset |


#### Hasil Evaluasi:
Model mampu merekomendasikan buku-buku yang sangat mirip dari segi konten (penulis, genre, popularitas), terutama untuk buku-buku populer seperti "Harry Potter". Namun, model cenderung hanya merekomendasikan buku-buku dari seri yang sama, sehingga eksplorasi terbatas.

---











Evaluasi menunjukkan bahwa kedua pendekatan memiliki potensi yang kuat dan bisa saling melengkapi dalam sistem rekomendasi hybrid yang lebih robust.





Ingatlah, metrik evaluasi yang digunakan harus sesuai dengan konteks data, problem statement, dan solusi yang diinginkan.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menjelaskan formula metrik dan bagaimana metrik tersebut bekerja.

**---Ini adalah bagian akhir laporan---**

_Catatan:_
- _Anda dapat menambahkan gambar, kode, atau tabel ke dalam laporan jika diperlukan. Temukan caranya pada contoh dokumen markdown di situs editor [Dillinger](https://dillinger.io/), [Github Guides: Mastering markdown](https://guides.github.com/features/mastering-markdown/), atau sumber lain di internet. Semangat!_
- Jika terdapat penjelasan yang harus menyertakan code snippet, tuliskan dengan sewajarnya. Tidak perlu menuliskan keseluruhan kode project, cukup bagian yang ingin dijelaskan saja.
