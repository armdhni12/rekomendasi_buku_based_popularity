<h1 align="center">
Laporan Proyek Mahchine Learning - Achmad Naila Muna Ramadhani
</h1>

# Domain Project

## *Project Background*:
Berdasarkan studi “Most Littered Nation In The World” yang dilakukan oleh Central Connecticut State University pada maret 2016, Indonesia dinyatakan menduduki peringkat ke-60 dari 61 Negara soal minat membaca [1]. Tentu saja hal ini sangat memperihatinkan karena rendahnya literasi di bangsa ini merupakan salah satu indikator bahwa tingkat pendidikan negara Indonesia masih tergolong sangat rendah. salah satu penyebab rendahnya tingkat baca pada masyarakat adalah tidak mengetahui judul buku apa yang menarik untuk mereka baca oleh karena itu dibuatlah projek ini agar masyarakat dapat menemukan judul buku yang bisa mereka jadikan preferensi untuk dibaca berdasarkan tingkat popularitas dari buku tersebut.  

## *Bussiness Understanding*:

## *Problem Statement*:
* Bagaimana memberikan rekomendasi buku terpopuler dengan menggunakan Machine Learning?

## *Goals*
 * Membangun sebuah sistem rekomendasi yang dapat memberikan rekomendasi buku terpopuler dengan menggunakan Popular based recomendation system mpada machine learning
 
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
Tahap pertama dalam melakukan data preparation adalah melakukan importing/mendownload dataset yang akan digunakan dari kaggle dengan  menggunakan kaggle API. karena data yang digunakan menggunakan format csv dalam filenya kemudian dilakukan pengecekan terhadap data secara keseluruhan untuk mengetahui jumlah baris dan kolomnya.
* Book :


|       ISBN |                                        Book-Title |          Book-Author | Year-Of-Publication |                  Publisher |                                       Image-URL-S |                                       Image-URL-M |                                       Image-URL-L |
|-----------:|--------------------------------------------------:|---------------------:|--------------------:|---------------------------:|--------------------------------------------------:|--------------------------------------------------:|--------------------------------------------------:|
| 0195153448 |                               Classical Mythology |   Mark P. O. Morford |                2002 |    Oxford University Press | http://images.amazon.com/images/P/0195153448.0... | http://images.amazon.com/images/P/0195153448.0... | http://images.amazon.com/images/P/0195153448.0... |
| 0002005018 |                                      Clara Callan | Richard Bruce Wright |                2001 |      HarperFlamingo Canada | http://images.amazon.com/images/P/0002005018.0... | http://images.amazon.com/images/P/0002005018.0... | http://images.amazon.com/images/P/0002005018.0... |
| 0060973129 |                              Decision in Normandy |         Carlo D'Este |                1991 |            HarperPerennial | http://images.amazon.com/images/P/0060973129.0... | http://images.amazon.com/images/P/0060973129.0... | http://images.amazon.com/images/P/0060973129.0... |
| 0374157065 | Flu: The Story of the Great Influenza Pandemic... |     Gina Bari Kolata |                1999 |       Farrar Straus Giroux | http://images.amazon.com/images/P/0374157065.0... | http://images.amazon.com/images/P/0374157065.0... | http://images.amazon.com/images/P/0374157065.0... |
| 0393045218 |                            The Mummies of Urumchi |      E. J. W. Barber |                1999 | W. W. Norton &amp; Company | http://images.amazon.com/images/P/0393045218.0... | http://images.amazon.com/images/P/0393045218.0... | http://images.amazon.com/images/P/0393045218.0... |

* Ratings :

| User-ID |       ISBN | Book-Rating |
|--------:|-----------:|------------:|
|  276725 | 034545104X |           0 |
|  276726 | 0155061224 |           5 |
|  276727 | 0446520802 |           0 |
|  276729 | 052165615X |           3 |
|  276729 | 0521795028 |           6 |

* Users :

| User-ID |                           Location |  Age |
|--------:|-----------------------------------:|-----:|
|       1 |                 nyc, new york, usa |  NaN |
|       2 |          stockton, california, usa | 18.0 |
|       3 |    moscow, yukon territory, russia |  NaN |
|       4 |          porto, v.n.gaia, portugal | 17.0 |
|       5 | farnborough, hants, united kingdom |  NaN |

# Data Preprocessing

## Melakukan Pengecekan terhadap missing value

tahap selanjutnya adalah preprocessing yang dimana tahap preprocessing yang pertama dilakukan adalah melakukan pengecekan terhadap jumlah *missing value* yang berada pada tiap tiap dataframe kemudian melakukan dropping pada data yang memiliki nilai missing value
* Books

| ISBN                | 0 |
|---------------------|---|
| Book-Title          | 0 |
| Book-Author         | 1 |
| Year-Of-Publication | 0 |
| Publisher           | 2 |
| Image-URL-S         | 0 |
| Image-URL-M         | 0 |
| Image-URL-L         | 3 |

* Ratings

| User-ID     | 0 |
|-------------|---|
| ISBN        | 0 |
| Book-Rating | 0 |

dapat dilihat bahwa dataframe books memiliki beberapa missing value pada beberapa kolomnya sedangkan pada dataframe ratings tidak terdapat adanya *missing value*. Untuk dataframe usesrs tidak perlu digunakan karena akan membuat sistem rekomendasi berdasarkan tingkat rating rata rata pada setiap buku.

## Membersihkan nilai Missing Value pada Books dataframe

tahapan preprocessing yang selanjutnya adalah melakukan pembersihan nilai *missing value* yang terdapat pada books dataframe dengan melakukan dropping pada data yang terdapat nilai *missing Value*, berikut adalah tabel data setelah dilakukan pembersihan *missing value*
| ISBN                | 0 |
|---------------------|---|
| Book-Title          | 0 |
| Book-Author         | 0 |
| Year-Of-Publication | 0 |
| Publisher           | 0 |
| Image-URL-S         | 0 |
| Image-URL-M         | 0 |
| Image-URL-L         | 0 |

## melakukan pengecekan statistik pada books dataframe

|        |       ISBN |     Book-Title |     Book-Author | Year-Of-Publication | Publisher |                                       Image-URL-S |                                       Image-URL-M |                                       Image-URL-L |
|-------:|-----------:|---------------:|----------------:|--------------------:|----------:|--------------------------------------------------:|--------------------------------------------------:|--------------------------------------------------:|
|  count |     271354 |         271354 |          271354 |              271354 |    271354 |                                            271354 |                                            271354 |                                            271354 |
| unique |     271354 |         242130 |          102020 |                 200 |     16803 |                                            271038 |                                            271038 |                                            271038 |
|   top  | 0195153448 | Selected Poems | Agatha Christie |                2002 | Harlequin | http://images.amazon.com/images/P/014062080X.0... | http://images.amazon.com/images/P/014062080X.0... | http://images.amazon.com/images/P/014062080X.0... |
|  freq  |          1 |             27 |             632 |               13902 |      7535 |                                                 2 |                                                 2 |                                                 2 |
|        |            |                |                 |                     |           |                                                   |                                                   |                                                   |

tahapan selanjutnya adalah melakukan pengecekan statistik pada books dataframe yang mana dapat kita lihat judul buku yang paling populer adalah Selected Poem
