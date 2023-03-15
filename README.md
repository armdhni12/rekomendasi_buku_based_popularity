<h1 align="center">
Laporan Proyek Mahchine Learning - Achmad Naila Muna Ramadhani
</h1>

# Domain Project

## *Project Background*:
Berdasarkan studi “Most Littered Nation In The World” yang dilakukan oleh Central Connecticut State University pada maret 2016, Indonesia dinyatakan menduduki peringkat ke-60 dari 61 Negara soal minat membaca [1]. Tentu saja hal ini sangat memperihatinkan karena rendahnya literasi di bangsa ini merupakan salah satu indikator bahwa tingkat pendidikan negara Indonesia masih tergolong sangat rendah. salah satu penyebab rendahnya tingkat baca pada masyarakat adalah tidak mengetahui judul buku apa yang menarik untuk mereka baca oleh karena itu dibuatlah projek ini agar masyarakat dapat menemukan judul buku yang bisa mereka jadikan preferensi untuk dibaca berdasarkan tingkat popularitas dari buku tersebut.  

## *Bussiness Understanding*:

## *Problem Statement*:
* Bagaimana memberikan rekomendasi buku terpopuler dengan menggunakan *Machine Learning*?

## *Goals*
 * Membangun sebuah sistem rekomendasi yang dapat memberikan rekomendasi buku terpopuler dengan menggunakan Popular based recomendation system pada machine learning
 
# *Data Understanding*
Merujuk dari permasalahan yang ada saya menggunakan data yang diperoleh dari platform publik data yaitu Kaggle mengenai rekomendasi buku yang berjumlah sebanyak 271360 data buku, 1149780 data ratings dan 278858 data user. Untuk menentukan rekomendasi buku terpopuler menggunakan parameter nilai rata rata rating yang terdapat pada data. Sehingga hasil penelitian nantinya dapat menjadi bahan acuan penilaian bagaimana tingkat kepopuleran beberapa judul buku berdasarkan besarnya nilai rata rata rating. Dataset yang saya gunakan dapat diperoleh pada platform Kaggle dengan tautan sebagai berikut https://www.kaggle.com/datasets/arashnic/book-recommendation-dataset/code?resource=download

## Variabel Data
Dalam dataset yang digunakan terdapat 3 file yaitu:
* Users
berisi data data pengguna yang dimana pada variabel User-Id telah dianonimkan menjadi bilangan bulat kemudian ada data demografis yang berisi
  * Location= yang merupakan lokasi pengguna
  * Age : yang merupakan usia dari pengguna
* Books
berisi data tentang buku yang diperoleh dari Amazon Web Services. pada data ini terdapat beberapa variabel seperti
  * ISBN : merupakan *International Standart Book Number* dari masing masing judul buku
  * Book-Title : merupakan judul buku
  * Book-Author : merupakan pengarang dari buku
  * Year-of-Publication : merupakan tahun terbitnya buku
  * Publisher : yang merupakan nama penerbit dari buku tersebut
  * kemudian ada Image-URL-S,Image-URL-M,Image-URL-L yang merupakan tautan gambar sampul yang mengarah ke situs amazon dengan tiga ukuran yang berbeda
* Ratings
Berisi informasi rating buku. Peringkat ( Book-Rating) bisa eksplisit, dinyatakan dalam skala 1-10 (nilai lebih tinggi menunjukkan apresiasi lebih tinggi), atau implisit, dinyatakan dengan 0.

# Data Preparation

## Importing Dataset
Tahap pertama dalam melakukan data preparation adalah melakukan importing/mendownload dataset yang akan digunakan dari kaggle dengan  menggunakan kaggle API. karena data yang digunakan menggunakan format csv dalam filenya kemudian dilakukan pengecekan terhadap data secara keseluruhan untuk mengetahui jumlah baris dan kolomnya dapat tnjukan pada tabel 1,2 dan 3 dibawah ini.
* Tabel 1 Dataframe books


|       ISBN |                                        Book-Title |          Book-Author | Year-Of-Publication |                  Publisher |                                       Image-URL-S |                                       Image-URL-M |                                       Image-URL-L |
|-----------:|--------------------------------------------------:|---------------------:|--------------------:|---------------------------:|--------------------------------------------------:|--------------------------------------------------:|--------------------------------------------------:|
| 0195153448 |                               Classical Mythology |   Mark P. O. Morford |                2002 |    Oxford University Press | http://images.amazon.com/images/P/0195153448.0... | http://images.amazon.com/images/P/0195153448.0... | http://images.amazon.com/images/P/0195153448.0... |
| 0002005018 |                                      Clara Callan | Richard Bruce Wright |                2001 |      HarperFlamingo Canada | http://images.amazon.com/images/P/0002005018.0... | http://images.amazon.com/images/P/0002005018.0... | http://images.amazon.com/images/P/0002005018.0... |
| 0060973129 |                              Decision in Normandy |         Carlo D'Este |                1991 |            HarperPerennial | http://images.amazon.com/images/P/0060973129.0... | http://images.amazon.com/images/P/0060973129.0... | http://images.amazon.com/images/P/0060973129.0... |
| 0374157065 | Flu: The Story of the Great Influenza Pandemic... |     Gina Bari Kolata |                1999 |       Farrar Straus Giroux | http://images.amazon.com/images/P/0374157065.0... | http://images.amazon.com/images/P/0374157065.0... | http://images.amazon.com/images/P/0374157065.0... |
| 0393045218 |                            The Mummies of Urumchi |      E. J. W. Barber |                1999 | W. W. Norton &amp; Company | http://images.amazon.com/images/P/0393045218.0... | http://images.amazon.com/images/P/0393045218.0... | http://images.amazon.com/images/P/0393045218.0... |

* Tabel 2 Dataframe Rating

| User-ID |       ISBN | Book-Rating |
|--------:|-----------:|------------:|
|  276725 | 034545104X |           0 |
|  276726 | 0155061224 |           5 |
|  276727 | 0446520802 |           0 |
|  276729 | 052165615X |           3 |
|  276729 | 0521795028 |           6 |

* Tabel 3 Dataframe Users

| User-ID |                           Location |  Age |
|--------:|-----------------------------------:|-----:|
|       1 |                 nyc, new york, usa |  NaN |
|       2 |          stockton, california, usa | 18.0 |
|       3 |    moscow, yukon territory, russia |  NaN |
|       4 |          porto, v.n.gaia, portugal | 17.0 |
|       5 | farnborough, hants, united kingdom |  NaN |

# Data Preparation

## Melakukan Pengecekan terhadap missing value

tahap selanjutnya adalah preprocessing yang dimana tahap preprocessing yang pertama dilakukan adalah melakukan pengecekan terhadap jumlah *missing value* yang berada pada tiap tiap dataframe kemudian melakukan dropping pada data yang memiliki nilai missing value
* Tabel 4 jumlah nilai missing value pada books dataframe

| ISBN                | 0 |
|---------------------|---|
| Book-Title          | 0 |
| Book-Author         | 1 |
| Year-Of-Publication | 0 |
| Publisher           | 2 |
| Image-URL-S         | 0 |
| Image-URL-M         | 0 |
| Image-URL-L         | 3 |

* tabel 5 jumlah nilai missing value pada Ratings

| User-ID     | 0 |
|-------------|---|
| ISBN        | 0 |
| Book-Rating | 0 |

dapat dilihat pada tabel 4 dan 5 bahwa dataframe books memiliki beberapa missing value pada beberapa kolomnya sedangkan pada dataframe ratings tidak terdapat adanya *missing value*. Untuk dataframe usesrs tidak perlu digunakan karena akan membuat sistem rekomendasi berdasarkan tingkat rating rata rata pada setiap buku.

## Membersihkan nilai Missing Value pada Books dataframe

* tabel 6 jumlah missing value pada books dataframe setelah dilakukan pembersihan data

| ISBN                | 0 |
|---------------------|---|
| Book-Title          | 0 |
| Book-Author         | 0 |
| Year-Of-Publication | 0 |
| Publisher           | 0 |
| Image-URL-S         | 0 |
| Image-URL-M         | 0 |
| Image-URL-L         | 0 |

tahapan preparation yang selanjutnya adalah melakukan pembersihan nilai *missing value* yang terdapat pada books dataframe dengan melakukan dropping pada data yang terdapat nilai *missing Value* sehingga menghasilkan nilai missing value yang telah dihapus seperti pada tabel 6 diatas

## melakukan pengecekan statistik pada books dataframe

* tabel 7 statistik pada books dataframe 

|        |       ISBN |     Book-Title |     Book-Author | Year-Of-Publication | Publisher |                                       Image-URL-S |                                       Image-URL-M |                                       Image-URL-L |
|-------:|-----------:|---------------:|----------------:|--------------------:|----------:|--------------------------------------------------:|--------------------------------------------------:|--------------------------------------------------:|
|  count |     271354 |         271354 |          271354 |              271354 |    271354 |                                            271354 |                                            271354 |                                            271354 |
| unique |     271354 |         242130 |          102020 |                 200 |     16803 |                                            271038 |                                            271038 |                                            271038 |
|   top  | 0195153448 | Selected Poems | Agatha Christie |                2002 | Harlequin | http://images.amazon.com/images/P/014062080X.0... | http://images.amazon.com/images/P/014062080X.0... | http://images.amazon.com/images/P/014062080X.0... |
|  freq  |          1 |             27 |             632 |               13902 |      7535 |                                                 2 |                                                 2 |                                                 2 |
|        |            |                |                 |                     |           |                                                   |                                                   |                                                   |

tahapan selanjutnya adalah melakukan pengecekan statistik pada books dataframe yang mana dapat kita lihat judul buku yang paling populer adalah Selected Poem seperti pada tabel 7 diatas

## melakukan pengecekan data duplikat
tahapan selanjutnya adalah melakukan pengecekan terhadap data yang duplikat pada books dan rating dataframe

# Modeling

* tabel 8 gabungan antara books dataframe dengan ratings dataframe

|   |       ISBN |          Book-Title |          Book-Author | Year-Of-Publication |               Publisher |                                       Image-URL-S |                                       Image-URL-M |                                       Image-URL-L | User-ID | Book-Rating |
|--:|-----------:|--------------------:|---------------------:|--------------------:|------------------------:|--------------------------------------------------:|--------------------------------------------------:|--------------------------------------------------:|--------:|------------:|
| 0 | 0195153448 | Classical Mythology |   Mark P. O. Morford |                2002 | Oxford University Press | http://images.amazon.com/images/P/0195153448.0... | http://images.amazon.com/images/P/0195153448.0... | http://images.amazon.com/images/P/0195153448.0... |       2 |           0 |
| 1 | 0002005018 |        Clara Callan | Richard Bruce Wright |                2001 |   HarperFlamingo Canada | http://images.amazon.com/images/P/0002005018.0... | http://images.amazon.com/images/P/0002005018.0... | http://images.amazon.com/images/P/0002005018.0... |       8 |           5 |
| 2 | 0002005018 |        Clara Callan | Richard Bruce Wright |                2001 |   HarperFlamingo Canada | http://images.amazon.com/images/P/0002005018.0... | http://images.amazon.com/images/P/0002005018.0... | http://images.amazon.com/images/P/0002005018.0... |   11400 |           0 |
| 3 | 0002005018 |        Clara Callan | Richard Bruce Wright |                2001 |   HarperFlamingo Canada | http://images.amazon.com/images/P/0002005018.0... | http://images.amazon.com/images/P/0002005018.0... | http://images.amazon.com/images/P/0002005018.0... |   11676 |           8 |
| 4 | 0002005018 |        Clara Callan | Richard Bruce Wright |                2001 |   HarperFlamingo Canada | http://images.amazon.com/images/P/0002005018.0... | http://images.amazon.com/images/P/0002005018.0... | http://images.amazon.com/images/P/0002005018.0... |   41385 |           0 |

tahapan pertama dalam membentuk model *Popularity Based recomendation system* adalah melakukan *merging* antara books dan ratings dataframe yang menghasilkan tabel seperti tabel 8 diatas

## Melakukan Cek jumlah baris dan kolom pada dataframe gabungan
tahapan selanjutnya adalah melakukan pengecekan jumlah kolom dan baris pada dataframe baru yang merupakan gabungan dari dataframe book dan rating yang dimana pada dataframe gabungan ini terdisi dari 1031129 baris dan 10 kolom

## membuat dataframe berdasarkan pengelompokan judul buku dan menghitung jumlah nilai rating keseluruhan

* tabel 8 dataframe df_with_counted_ratings dengan pengelompokan judul buku dan perhitungan jumlah nilai rating keseluruhan
|                                       Book-Title | Counted-Rating |
|-------------------------------------------------:|---------------:|
| A Light in the Storm: The Civil War Diary of ... |              4 |
|                            Always Have Popsicles |              1 |
|             Apple Magic (The Collector's series) |              1 |
| Ask Lily (Young Women of Faith: Lily Series, ... |              1 |
| Beyond IBM: Leadership Marketing and Finance ... |              1 |

tahapan selanjutnya adalah membuat dataframe baru yang memuat judul buku dan nilai jumlah keseluruhan rating dengan melakukan pengelompokan berdasarkan kritera book title kemudian melakukan reset index dengan melakukan drop kolom pada df_new selain kolom Book_title dan Book-Rating

## melakukan pengecekan statistik

* tabel 10 statistik dari tabel df_with_counted_ratings
|       | Counted-Rating |
|------:|----------------|
| count |  241066.000000 |
|  mean |       4.277372 |
|  std  |      16.738847 |
|  min  |       1.000000 |
|  25%  |       1.000000 |
|  50%  |       1.000000 |
|  75%  |       3.000000 |
|  max  |    2502.000000 |

dapat dilihat setelah dilakukan pengecekan statistik bahwa jumlah nilai rating terbanyak ada pada angka 2502,kemudian pada tahapan selanjutnya kita akan melakukan pengecekan judul buku apa yang memiliki rating terbanyak

## pengecekan judul buku dengan jumlah pemberian rating terbesar 

* tabel 11 judul buku dengan jumlah rating terbanyak

|                Book-Title | Counted-Rating |
|--------------------------:|----------------|
|               Wild Animus |           2502 |
| The Lovely Bones: A Novel |           1295 |
|         The Da Vinci Code |            898 |

dapat dilihat pada tabel 11 buku dengan judul Wild Animus memiliki jumlah penilaian terbesar yaitu 2502

## Membuat dataframe dengan nilai rating rata rata

* tabel 12 df_with_avg_ratings yang merupakan pengelompokan dari Book-Title dan perhitungan rata rata dari Ratings pada masing masing buku

|                                       Book-Title | Average-Rating |
|-------------------------------------------------:|----------------|
| A Light in the Storm: The Civil War Diary of ... |           2.25 |
|                            Always Have Popsicles |           0.00 |
|             Apple Magic (The Collector's series) |           0.00 |
| Ask Lily (Young Women of Faith: Lily Series, ... |           8.00 |
| Beyond IBM: Leadership Marketing and Finance ... |           0.00 |

proses selanjutnya adalah membuat dataframe baru yang terdiri dari judul buku dan nilai rata rata rating yang dapat dilihat seperti pada tabel 12 diatas

## penggabungan dataframe antara counted rating dan average rating


* tabel 13 dataframe yang terdiri dari gabungan antara counted-rating dan average rating yang dikelompokan berdasarkan Book-title

|                 Book-Title | Counted-Rating | Average-Rating |
|---------------------------:|---------------:|----------------|
|                Wild Animus |           2502 |       1.019584 |
|  The Lovely Bones: A Novel |           1295 |       4.468726 |
|          The Da Vinci Code |            898 |       4.642539 |
|            A Painted House |            838 |       3.231504 |
| The Nanny Diaries: A Novel |            828 |       3.530193 |

tahapan selanjutnya yaitu melakukan penggabungan dataframe counted rating dan average rating berdasarkan Book-Title kemudian mengurutkan nilai dengan besaran nilai pada Counted-Rating yang menghasilkan tabel dataframe seperti pada tabel 13

## Membuat popular dataframe
* tabel 14 dataframe gabungan antara counted-rating dan average rating yang telah diurutkan berdasarkan nilai average rating dari yang terbesar

|                                        Book-Title | Counted-Rating | Average-Rating |
|--------------------------------------------------:|---------------:|----------------|
|  Harry Potter and the Chamber of Secrets (Book 2) |            556 |       5.183453 |
| Harry Potter and the Sorcerer's Stone (Harry P... |            575 |       4.895652 |
|                             To Kill a Mockingbird |            510 |       4.700000 |
|                                 The Da Vinci Code |            898 |       4.642539 |
|                         The Lovely Bones: A Novel |           1295 |       4.468726 |
|                           The Secret Life of Bees |            774 |       4.447028 |
|               The Red Tent (Bestselling Backlist) |            723 |       4.334716 |
|                         Girl with a Pearl Earring |            526 |       4.218631 |
| Where the Heart Is (Oprah's Book Club (Paperba... |            585 |       4.105983 |
|                                        Life of Pi |            664 |       4.088855 |

proses berikutnya yaitu membuat dataframe popular_books dengan menggabungkan data pada dataframe book_sorted yang memiliki nilai Counted-Rating lebih dari 500 lalu mengurutkannya berdasarkan nilai Average-Rating 

## Top-N Recomendation
berikutnya adalah mendapatkan rekomendasi 10 buku terpopuler berdasarkan nilai rata rata ratings yang diberikan pengguna dengan hasil sebagai berikut:

* tabel 15 sepuluh rekomendasi judul teratas

| Book-Title                                        |
|---------------------------------------------------|
|  Harry Potter and the Chamber of Secrets (Book 2) |
| Harry Potter and the Sorcerer's Stone (Harry P... |
|                             To Kill a Mockingbird |
|                                 The Da Vinci Code |
|                         The Lovely Bones: A Novel |
|                           The Secret Life of Bees |
|               The Red Tent (Bestselling Backlist) |
|                         Girl with a Pearl Earring |
| Where the Heart Is (Oprah's Book Club (Paperba... |
|                                        Life of Pi |

# Evaluation

berikutnya adalah evaluasi berdasarkan pembentukan sistem rekomendasi menggunakan *Popularity Based Filtering* dapat memberikan rekomendasi judul buku terpopuler melalui  plotting dari hasil perhitungan perbandingan nilai rata rata rating yang diberikan pengguna terhadap seluruh judul buku memberikan hasil bahwa buku dengan judul Harry Potter and the Chamber of Secrets (Book 2) merupakan buku terpopuler dengan nilai rating rata rata sebesar  5.183453

#References
[1] M. Prasrihamni, Zulela, and Edwita, “OPTIMALISASI PENERAPAN KEGIATAN LITERASI DALAM MENINGKATKAN MINAT BACA SISWA SEKOLAH DASAR Mega,” J. Cakrawala Pendas, vol. 8, no. 1, pp. 128–134, 2022.

