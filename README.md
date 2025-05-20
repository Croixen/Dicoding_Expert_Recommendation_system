# Laporan Proyek Machine Learning - Kelvin Riyanto
# Project Overview
---
Dalam era modern ini, platform **Streaming** berkembang dengan sangat pesat, yang di mulai dari *Youtube*, hingga platform streaming dengan skema berlangganan seperti *Hulu*, *Netflix*, *Disney Plus*, dan *Amazon Prime*, di Indonesia sendiri pun, tidak ketinggalan dengan hadirnya platform seperti *Vidio*.

Platform-platform tersebut, terutama yang menggunakan skema berlangganan, berlomba-lomba menarik customer yang mereka berikan, dengan data yang dirilis oleh *Statista*, hingga pada tahun 2025, platform streaming berlangganan memiliki *market share* sebagai berikut:
![Streaming Market Share](Gambar/US-Video-Streaming-Market-Share-1.png.avif)

> Sumber diperoleh dari [Evoca.tv](https://evoca.tv/streaming-service-market-share/)

Dengan ini, akan dibutukan sebuah sistem untuk merekomendasikan produk kepada user, dengan harapan user itu dapat tertarik untuk terus berada di model bisnis yang telah ditetapkan, maka dari itu dibuatlah **Sistem Rekomendasi.**

**Sistem rekomendasi** adalah sebuah algoritma kecerdasan buatan, yang umumnya di hubungkan dengan pembelajaran mesin, yang menggunakan **Big Data** untuk menyarankan atau merekomendasikan sebuah produk tambahan untuk user. Ini dapat berbasis pada berbagai kriteria, termasuk pembelian terdahulu, riwayat pencarian, informasi demografi, dan faktor lain[1].

**Sistem Rekomendasi** sendiri memiliki 2 pendekatan yang berbeda, yakni *Collaborative Filtering* dan *Content-Based Filtering*, terdapat juga pendekatan lainnya, yaitu *Hybrid Recommendation System* yang menggabungkan kelebihan daripada kedua algoritma yang telah disebutkan sebelumnya. Pada *Collaborative Learning*, algoritma berfokus pada kesamaan preferensi antar satu user dan user yang lain. Berbeda pada *Content-Based Learning* yang menggunakan atribut atau fitur dari sebuah barang untuk merekomendasikan barang lainnya yang memiliki kesamaan dengan barang yang disukai oleh pegguna[1].

Proyek ini akan menggunakan dataset yang di dapatkan melalui *kaggle* untuk melakukan percobaan dalam menerapkan sistem *Collaborative Filtering* dan *Content-Based Filtering*. Dataset yang digunakan adalah dataset yang terdiri dari rating user, dan genre terhadap film-film yang dapat ditonton. Data ini akan menjadi landasan dalam membangun sistem *Collaborative Learning* dan *Content-Based Filtering*. 

Proyek ini bertujuan untuk melakukan pembangunan dan penerapan sederhana Sistem Rekomendasi dengan pendekatan *Collaborative Learning* dan *Content-Based Filtering* dengan menggunakan dataset film dari periode sekitar tahun 1990-an hingga pada tahun 2010-an sebagai kasus.

# Business Understanding
---
## Problem Statements
Masalah yang ingin diselesaikan dirumuskan sebagai berikut:
1. Bagaimana cara membangun sistem rekomendasi berbasis Collaborative Filtering dan Content-Based Filtering untuk memberikan rekomendasi film yang relevan kepada pengguna berdasarkan data dari pengguna dan juga film?
2. Bagaimana mengoptimalkan akurasi rekomendasi menggunakan metode kesamaan antar preferensi user dan film?
## Goals
Adapun tujuan dari penelitian sebagai berikut:
1. Membangun sistem rekomendasi sederhana berbasis Collaborative Filtering dan Content-Based Filtering yang mampu menyarankan film berdasarkan kesamaan preferensi antar pengguna.
2. Meningkatkan akurasi rekomendasi dengan mengimplementasikan metode pengukuran kesamaan antar pengguna, seperti Cosine Similarity atau Pearson Correlation, atau dengan memanfaatkan deep learning agar dapat menghadirkan sebuah sistem rekomendasi yang lebih komprehensif.
## Solution
Solusi yang akan diajukan adalah sebagai berikut:
1. Menerapkan algoritma Collaborative Filtering dan Content-Based Filtering menggunakan dataset film yang tersedia dari Kaggle.
2. Menggunakan metode perhitungan kesamaan antar pengguna (seperti Cosine Similarity atau Pearson Correlation) untuk membangun rekomendasi berbasis objek yang akurat, dan juga memanfaatkan deep learning untuk rekomendasi berdasar pada user lain.
# Data Understanding
---

[Link Dataset](https://www.kaggle.com/datasets/nicoletacilibiu/movies-and-ratings-for-recommendation-system)

Dataset yang meliputi dua file, yaitu `movies.csv` dan `ratings.csv`.
Masing masing memiliki jumlah data yang berbeda, dimana terdapat 9742 baris pada dataset `movies` dan 100836 baris pada dataset `ratings`

| Movies                 | Ratings                            |
| ---------------------- | ---------------------------------- |
| 9742 rows, 3 Columns              | 100836 rows, 4 Columns                        |
| movieId, title, genres | userId, movieId, rating, timestamp |
## Fitur Movies

| Fitur   | Penjelasan                     | tipe data |
| ------- | ------------------------------ | --------- |
| movieId | id unik dari film.             | int64     |
| title   | judul dari film.               | object    |
| genre   | genre atau kategori dari film. | object    |
## Fitur Ratings

| Fitur     | Penjelasan                                                                | tipe data |
| --------- | ------------------------------------------------------------------------- | --------- |
| userId    | id unik dari user.                                                        | int64     |
| movieId   | id unik dari film.                                                        | int64     |
| rating    | rating yang diberikan kepada user terhadap film yang sesuai pada movieId. | float64   |
| timestamp | data yang berupa kapan data tersebut direkam.                             | int64     |

## Deskripsi data rating

| idx   | rating        |
| ----- | ------------- |
| count | 100836.000000 |
| mean  | 3.501557      |
| std   | 1.042529      |
| min   | 0.500000      |
| 25%   | 3.000000      |
| 50%   | 3.500000      |
| 75%   | 4.000000      |
| max   | 5.000000      |

![Distribusi Per Judul Film](Gambar/Untitled.png)
*Gambar Distribusi Rating per judul film*
Bila dibandingkan dengan data yang didapat melalui deskripsi data dan visualisasi data, dapat disimpulkan bahwa rata-rata data disini didominasi oleh rating berkisar 3.5 hingga 5.

|     | title                                     | rating | avg_rating | count    |
| --- | ----------------------------------------- | ------ | ---------- | -------- |
| 0   | Shawshank Redemption, The (1994)          | 5.0    | 153        | 4.429022 |
| 1   | Pulp Fiction (1994)                       | 5.0    | 123        | 4.197068 |
| 2   | Forrest Gump (1994)                       | 5.0    | 116        | 4.164134 |
| 3   | Matrix, The (1999)                        | 5.0    | 109        | 4.192446 |
| 4   | Star Wars: Episode IV - A New Hope (1977) | 5.0    | 104        | 4.231076 |
| 5   | Silence of the Lambs, The (1991)          | 4.0    | 97         | 4.161290 |
| 6   | Jurassic Park (1993)                      | 4.0    | 97         | 3.750000 |
| 7   | Forrest Gump (1994)                       | 4.0    | 94         | 4.164134 |
| 8   | Schindler's List (1993)                   | 5.0    | 92         | 4.225000 |
| 9   | Silence of the Lambs, The (1991)          | 5.0    | 92         | 4.161290 |

![Distribusi keluaran film pertahun](Gambar/Untitled-1.png)
*Distribusi keluaran film pertahun*
Dari visualisasi ini juga didapatkan, film yang di muat dlaam dataset merupakan film dari tahun 1900-an hingga pada tahun 2018.

![Distribusi genre film](Gambar/Untitled_1.png)

*Distribusi Genre Film*
Dataset yang didapatkan didominasi dengan genre Drama, Comedy, Action, Thriller, sehingga kita dapat mengekspektasikan bahwa kita akan lebih sering mendapatkan rekomendasi dengan genre tersebut.

### Missing Value & Duplicate Value

**Missing Value**
|           | 0   |
| --------- | --- |
| movieId   | 0   |
| title     | 0   |
| genres    | 0   |
| userId    | 18  |
| rating    | 18  |
| timestamp | 18  |

**Duplicate Value**
Tidak ditemukan duplicated value.

# Data Preparation
---
Tahapan ini berguna untuk mempersiapkan data sebelum di feed ke dalam model. Data Preparation bertujuan untuk menghasilkan data yang lebih bersih, dan menentukan data pipeline yang dapat disesuaikan dengan kebutuhan model. Langkah yang dilakukan dalam Data Preparation adalah sebagia berikut:
1. Menggabungkan data rating dan film menjadi satu. Merging ini dilakukan berdasarkan pada identifikasi unik film (movieId).
2. Melakukan eliminasi terhadap data kosong dan duplikat. Data yang memiliki nilai NaN ataupun terduplikasi akan di hapus agar menghindari model yang menangkap noise dalam data.
3. Encode untuk content-based filtering, dengan melakukan embedding (tfidf) yang kemudian dilakukan perhitungan jarak kosinus terhadap setiap nilai genre. Ini dilakukan untuk mencari korelasi antar produk yang kurang lebih mirip, nilai korelasi ini akan dijadikan basis dalam memberikan rekomendasi.
4. Melakukan encode data user dan film untuk collaborative learning, yang kemudian dilakukan juga scaling menggunakan minmax scaling.
5. Melakukan pembagian data dengan rasio 90/10.
Tahap ini dilakukan agar untuk mendapatkan system atau model dengan generalisasi yang lebih baik, dan juga memastikan data yang bersih dan rata.
# Modeling and Result
---
## Content-Based Filtering
Collaborative filtering adalah teknik dalam sistem rekomendasi yang memprediksi preferensi atau minat pengguna terhadap suatu item (seperti film, produk, lagu, dll.) berdasarkan kesamaan dengan pengguna lain atau pola perilaku sebelumnya.

Model pada content-based filtering menggunakan tfidf untuk melakukan embedding ada genre film, yang kemudian setiap kemiripannya akan di hitung menggunakan cosine similarity.
Metode ini akan menghasilkan hal berikut:
Dengan input **id film 123** dengan film:
```
 Untouchables, The (1987) Action|Crime|Drama
```
menghasilkan rekomendasi:


| idx  | movie_id | title                                                  | genres                                  | rating | similarity Score |
|------|----------|--------------------------------------------------------|------------------------------------------|--------|------------------|
| 64064| 31658    | Howl's Moving Castle (Hauru no ugoku shiro) (2004)    | Adventure\|Animation\|Fantasy\|Romance   | 4.0    | 1.0              |
| 64065| 2064     | Roger & Me (1989)                                      | Documentary                              | 5.0    | 1.0              |
| 64034| 2421     | Karate Kid, Part II, The (1986)                        | Action\|Adventure\|Drama                 | 2.0    | 1.0              |
| 64035| 92535    | Louis C.K.: Live at the Beacon Theater (2011)         | Comedy                                   | 4.5    | 1.0              |
System ini memiliki maksimal rekomendasi sebanyak 6, tetapi disini kita hanya mendapat 4 rekomendasi.

### Pertimbangan
#### Kelebihan:
- Cepat untuk diimplementasi.
- Sistem ini dapat bekerja meskipun dengan data preferensi user yang minimal.
- Struktur dari pipeline data yang lebih sederhana.
#### Kekurangan:
- Sifat dari sistem ini yang tidak mempedulikan preferensi user dan hanya mempedulikan genre, berkemungkinan untuk terjadinya rekomendasi yang kurang beragam.
- Perhitungan nilai cosinus yang menjadi lebih lambat seiring bertambahnya data, ini perlu diperhatikan dalam tahap pemeliharaan.
## Collaborative Filtering
Collaborative filtering adalah teknik dalam sistem rekomendasi yang memprediksi preferensi atau minat pengguna terhadap suatu item (seperti film, produk, lagu, dll.) berdasarkan kesamaan dengan pengguna lain atau pola perilaku sebelumnya.


Model ini menghasilkan rekomendasi sebagai berikut:
**Input id**:
userid: 524
5 sample yang telah ditonton oleh user 524 dengan bintang diatas 4


| title                                     | genres                            | rating |
| ----------------------------------------- | --------------------------------- | ------ |
| Star Wars: Episode IV - A New Hope (1977) | Action\|Adventure\|Sci-Fi         | 5.0    |
| Aliens (1986)                             | Action\|Adventure\|Horror\|Sci-Fi | 5.0    |
| Right Stuff, The (1983)                   | Drama                             | 4.0    |
| Bridge on the River Kwai, The (1957)      | Adventure\|Drama\|War             | 4.0    |
| Back to the Future (1985)                 | Adventure\|Comedy\|Sci-Fi         | 5.0    |

Memiliki hasil sebagai berikut:


Top 10 movie recommendations
--------------------------------
Hoop Dreams (1994) : Documentary
Heavenly Creatures (1994) : Crime|Drama
Streetcar Named Desire, A (1951) : Drama
Paths of Glory (1957) : Drama|War
Lawrence of Arabia (1962) : Adventure|Drama|War
Goodfellas (1990) : Crime|Drama
Ran (1985) : Drama|War
Life Is Beautiful (La Vita è bella) (1997) : Comedy|Drama|Romance|War
There Will Be Blood (2007) : Drama|Western
Separation, A (Jodaeiye Nader az Simin) (2011) : Drama

### Pertimbangan
#### Kelebihan:
- Collaborative Learning menggunakan deep learning memberikan hasil yang lebih beragam dan personal, jika dibandingkan dengan Conten-Based Filtering yang menggunakan TFIDF, mengingat deep learning yang memerhatikan rating user A ke user yang lain.
- Hasil yang akan menjadi lebih baik, seiring bertambahnya user base dan rating yang diberikan oleh para user.
#### Kekurangan:
- Membutuhkan data yang besar untuk menghasilkan rekomendasi yang relevan dan terpersonil.
- Nature dari deep learning yang memang akan membutuhkan kekuatan komputasi yang besar.
- Sifatnya yang blackbox sehingga menghasilkan sebuah model yang sulit untuk diprediksi keluarannya.
# Evaluation
---
## Content Based Filtering
Content-Based Filtering memiliki hasil yang dapat diukur menggunakan precision@k.
Presisi tersebut dapat dihasilkan melalui formula berikut:
$$
Presisi = \frac{rekomendasi\_relevan}{jumlah\_rekomendasi}
$$
System yang telah dibuat mendapatkan nilai 1.0. Sebuah nilai yang menandakan bahwa konten yang direkomendasikan adalah konten yang relevan,.
Tetapi perlu diperhatikan juga, apakah rekomendasi tersebut memang relevan dan cocok terhadap user yang mengkonsumsi suatu konten atau tidak, mengingat nilai presisinya yang sangat tinggi.
## Collaborative Filtering
Dalam collaborative filtering, akan digunakan Binary Cross Entropy untuk mengukur perbedaan antara dua label 0 dan 1, yang umumnya digunakan untuk klasifikasi biner.
Binary cross entropy dihitung dengan rumus berikut:
$$L=-\frac{N}{1}​∑_{i=1}^{N}​[yi​log(pi​)+(1−yi​)log(1−pi​)]$$
Dalam collaborative filtering, digunakan klasifikasi biner dikarenakan hanya untuk melihat apakah akun ini cocok dengan user target apa tidak.

Plot loss daripada model:
![Loss Graphic](Gambar/Untitled_2.png)
Bila diperhatikan, secara grafik, perbedaan loss pada train dan test memang terpauk jauh, tetapi ini dikarenakan **scale** dalam grafik yang melakukan zoom terlalu dalam.
Dari sini, dapat diperoleh 2 kesimpulan terhadap model:
1. Loss score yang cukup baik untuk validasi dan train, yaitu ada di sekitar 0.6.
2. Dengan score validasi yang konveregen terhadap train tidak memiliki nilai yang terlalu jauh, dapat disimpulkan bahwa model ini tidak mengalami overfitting.

Melihat daripada kedua solution yang telah diberikan,
1. Penulis telah menerapkan sistem rekomendasi berbasis content-based dan collaborative filtering.
2. Penulis telah menerapkan cosine similarity dan deep learning dengan hasil presisi dan loss yang baik

# Daftar Referensi
---
[1] Recommendation System, Nvidia, https://www.nvidia.com/en-us/glossary/recommendation-system/. Diakses pada 9 Mei 2025.

