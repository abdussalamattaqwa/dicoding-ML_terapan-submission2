# Laporan Proyek Machine Learning - Abd Salam At Taqwa
## Project Overview

Pasar industri film luar negeri hingga film dalam negeri diperkirakan akan semakin berkembang.  Dilihat dari  jumlah penonton film pada bioskop yang terus meningkat setiap tahunnya. Berdasarkan sumber pada [[1]](https://www.statista.com/statistics/307390/number-of-cinema-admissions-worldwide/) memperkirakan bioskop di seluruh dunia akan menjual 5,7 miliar tiket pada akhir tahun 2022, naik 73 persen dari tahun sebelumnya. Pendapatan bioskop global akan berjumlah sekitar 38 miliar dolar AS pada tahun 2022. Angka tersebut diproyeksikan akan terus tumbuh pada tahun-tahun berikutnya, meskipun dengan laju yang lebih lambat setelah tahun 2023.

Seiring dengan berkembangnya teknologi *platform* untuk menonton film mulai bermunculan seperti Netflix, Disney+ Hotstar, HBO Go, Vidio, iQiyi, Klik Film, Bioskop Online, Cinema Box, Viu, CatchPlay+, WeTV, Genflix, iFlix, Viki, Prime Video. Platform-platform tersebut menerapkan sistem rekomendasi untuk membantu miliaran penggunanya menemukan konten yang dipersonalisasi dari kumpulan film yang tersedia [[2,](https://www.ijcai.org/Proceedings/2019/0284.pdf)[3](https://dl.acm.org/doi/abs/10.1145/3469213.3470423)]. Sebagai cara terbaik untuk mengatasi kelebihan informasi, sistem rekomendasi banyak digunakan untuk menyediakan konten dan layanan yang dipersonalisasi kepada pengguna dengan efisiensi tinggi [[4]](https://www.mdpi.com/2227-7390/11/4/895). Ketika sistem rekomendasi film berbasis konten digunakan, pengguna akan menerima beberapa film rekomendasi yang sangat mirip dengan film yang ditonton pengguna sebelumnya. Dalam sistem jenis ini, film umumnya dikelompokkan ke dalam kategori berbeda sesuai dengan kesamaannya. Atas dasar ini, sistem dapat merekomendasikan beberapa film potensial kepada pengguna berdasarkan preferensi tontonan pengguna yang direkam dalam database. Pada penelitian [[5]](https://j-ptiik.ub.ac.id/index.php/j-ptiik/article/view/9163) membuat sistem rekomendasi *content based filtering* dengan mencari kemiripan bobot dari *term* pada *bag of words*. Penelitian tersebut memanfaatkan fitur sinopsis film dan judul film untuk mencari kemiripan bobot antara film di dataset.

Berdasarkan latar belakang diatas proyek ini ingin membuat Sistem rekomendasi  data film yang mendeteksi kemiripan antara film yang Anda tonton dengan film lainnya menggunakan data fitur *titleType* dan *genres* pada film. Kedua data fitur tersebut selanjutnya digunakan untuk mencari dan mengurutkan film yang memiliki kemiripan yang sama. tahap awal sebelum mencari kemiripan dilakukan dengan proses *preprocessing* dimana data diolah sebelum dimasukkan kedalam model. *Feature extraction* adalah salah satu tahap *preprocessing* dimana teknik pengambilan ciri / *feature* dari suatu bentuk yang nantinya nilai yang didapatkan akan dianalisis untuk proses selanjutnya. Pada sub-tahapan *text feature extraction*, terdapat dua jenis metode yang
diujicobakan, yaitu CountVectorizer dan TfidfVectorizer seperti yang dilakukan pada penelitian [[6]](https://pdfs.semanticscholar.org/3e3d/6b45d7e31f8a1e03ca7b94c1e360dac08e30.pdf). Hasil dari *feature extraction* selanjutnya digunakan untuk mencari kemiripan antar video.Kemiripan antara video dicari menggunakan metode *cosine similarity*. *Cosine similarity* adalah ukuran kesamaan antara dua buah vektor dalam sebuah ruang dimensi yang didapat dari nilai cosinus sudut dari perkalian dua buah vektor [[7]](https://ejournal.itn.ac.id/index.php/jati/article/download/364/351). Semakin banyak data maka semakin banyak memori yang digunakan untuk menyimpan hasil perkalian dua vektor dari metode *cosine similarity*. Berdasarkan hal tersebut pada proyek ini data yang digunakan dibatasi hanya berupa data video selama tahun terakhir saja. Selain metode kemiripan, proses pengurutan menggunakan fitur rating video sebagai pertimbangan untuk sistem rekomendasi. fitur rating video digunakan karena semakin tinggi rating video tersebut maka semakin berkualitas dan layak untuk ditonton. Sistem rekomendasi ini diharapkan dapat memudahkan pengguna karena tidak perlu lagi membuang waktu untuk mencari film atau video yang diinginkan satu per satu.


## Business Understanding

### Problem Statements

Berdasarkan latar belakang diatas, masalah yang dapat diangkat pada proyek ini adalah:
- Bagaimana membuat sistem rekomendasi video untuk pengguna?
- Bagaimana mencari kemiripan antar video berdasarkan data fitur *titleType* dan *genres* menggunakan *cosine similarity*?
- Bagaimana menambahkan data rating video sebagai parameter tambahan untuk merekomendasikan video?

### Goals

Tujuan proyek adalah sebagai berikut:
- Membuat sistem rekomendasi video untuk memudahkan pengguna agar untuk mencari video yang diinginkan.
- Menggunakan fitur *titleType* dan *genres* pada video untuk mencari kemiripan antar video pada dataset.
- Menggunakan fitur *rating* video sebagai parameter tambahan untuk mengurutkan daftar-daftar rekomendasi video.

## Data Understanding
Dataset pada proyek ini menggunakan dataset IMDB yang menyediakan data-data mengenai berbagai macam jenis film. Dataset tersedia dan dapat diunduh pada situs [kaggle](https://www.kaggle.com/datasets/ashirwadsangwan/imdb-dataset/data).

Variabel-variabel pada IMDB dataset adalah sebagai berikut:
* tconst : Pengidentifikasi unik alfanumerik dari judul.
* titleType: jenis/format judul (misalnya *movie*, *short*,
*tvseries*, *tvepisode*, *video*, dll).
* primaryTitle : Versi paling terkenal dari judulnya
* originalTitle : Judul asli, dalam bahasa aslinya.
* isAdult : 0 untuk judul non-dewasa, dan 1 untuk judul dewasa.
* startYear : Mewakili tahun rilis suatu judul. Dalam kasus *tvseries*, ini adalah tahun awal serial tersebut.
* endYear : *tvseries* akhir tahun. untuk semua jenis judul lainnya.
* runtimeMinutes : Runtime utama dari judul, dalam hitungan menit
* genres : Mencakup hingga tiga genre yang terkait dengan judul.



## Data Preparation
Data preparation dilakukan menyiapkan data mentah sehingga layak untuk diproses kedalam model.

### *Assessing Data*
Penilaian data dapat membantu organisasi meningkatkan kualitas datanya dan menghindari kesalahan yang merugikan. Hal ini juga membantu untuk mengambil keputusan yang lebih baik dengan memberikan wawasan tentang bagaimana data digunakan.

Pada Tabel 1 diperlihatkan beberapa baris dataset video yang akan digunakan pada proyek ini.  Dataset ini berisi data video beserta fitur-fiturnya. Adapun fitur yang digunakan pada Tabel1 untuk menghitung kemiripan antar video adalah fitur *titleType* dan *genres*.
<br><br> Tabel 1. IMDB dataset

|index|tconst|titleType|primaryTitle|originalTitle|isAdult|startYear|endYear|runtimeMinutes|genres|
|---|---|---|---|---|---|---|---|---|---|
|0|tt0000001|short|Carmencita|Carmencita|0|1894|\N|1|Documentary,Short|
|1|tt0000002|short|Le clown et ses chiens|Le clown et ses chiens|0|1892|\N|5|Animation,Short|
|2|tt0000003|short|Pauvre Pierrot|Pauvre Pierrot|0|1892|\N|4|Animation,Comedy,Romance|
|3|tt0000004|short|Un bon bock|Un bon bock|0|1892|\N|12|Animation,Short|
|4|tt0000005|short|Blacksmith Scene|Blacksmith Scene|0|1893|\N|1|Comedy,Short|

Pada Tabel 2 membaca dataset rating untuk masing-masing video. Fitur pada dataset ini, yaitu averageRating dibutuhkan sebagai parameter tambahan untuk sistem rekomendasi video yang akan dibuat. Video dengan rating tertinggi menyediakan video yang berkualitas berdasarkan penilaian pengguna dan memungkinkan akan dipilih oleh pengguna lain.
<br><br> Tabel 2. IMDB rating dataset
|index|tconst|averageRating|numVotes|
|---|---|---|---|
|0|tt0000001|5\.7|2016|
|1|tt0000002|5\.7|270|
|2|tt0000003|6\.5|1940|
|3|tt0000004|5\.5|178|
|4|tt0000005|6\.2|2718|

Pada Tabel 3 kedua dataset video dan rating video digabungkanmenjadi satu dataset. Dataset yang disatukan selanjutnya akan menjadi dataset utama untuk sistem rekomendasi proyek ini. Dataset video yang digunakan hanya berupa dataset yang memiliki rating video saja. Penggabungan kedua dataset menggunakan *right join* akan menghilangkan video yang tidak memiliki data rating dan tidak akan digunakan pada proyek ini.

Seperti yang terlihat pada dataset, dataset yang tidak memiliki data akan diisi dengan nilai "\N". Pada proyek ini data-data yang memiliki nilai "\N" pada fitur startYear, genres, dan titleType akan dihilangkan. Dataset selanjutnya akan memfilter video-video yang keluar ditahun saat ini saja. Video dengan keluaran tahun saat ini, yaitu tahun 2024 yang akan digunakan untuk proyek ini. Hal ini dikarenakan metode untuk mencari kemiripan video menggunakan *cosine similaity* memerlukan memori yang besar sehingga dataset dibatasi hanay berupa video ditahun 2024 saja.
<br><br> Tabel 3. *Final* dataset
|index|tconst|titleType|primaryTitle|originalTitle|isAdult|startYear|endYear|runtimeMinutes|genres|averageRating|numVotes|
|---|---|---|---|---|---|---|---|---|---|---|---|
|0|tt0000001|short|Carmencita|Carmencita|0|1894|\N|1|Documentary,Short|5\.7|2016|
|1|tt0000002|short|Le clown et ses chiens|Le clown et ses chiens|0|1892|\N|5|Animation,Short|5\.7|270|
|2|tt0000003|short|Pauvre Pierrot|Pauvre Pierrot|0|1892|\N|4|Animation,Comedy,Romance|6\.5|1940|
|3|tt0000004|short|Un bon bock|Un bon bock|0|1892|\N|12|Animation,Short|5\.5|178|
|4|tt0000005|short|Blacksmith Scene|Blacksmith Scene|0|1893|\N|1|Comedy,Short|6\.2|2718|

Tabel 4 menyediakan informasi dataset yang digunakan. Berdasarkan informasi pada dataset didapatkan bahwa fitur pada dataset berjumlah 11 dan dataset memiliki jumlah 1194. Setiap fitur pada dataset tidak memiliki nilai null atau kosong. Tujuan untuk pengecekan ini adalah untuk melakukan penanganan jika terdapat nilai null atau data yang hilang pada dataset. Penyebab data yang hilang dapat berupa kerusakan data atau kegagalan pencatatan data. Penanganan data yang hilang sangat penting selama prapemrosesan kumpulan data karena banyak algoritma yang tidak mendukung dataset dengan nilai yang hilang.
<br> <br>Tabel 4. Informasi dataset 
<br>`<class 'pandas.core.frame.DataFrame'>`
<br>`Int64Index: 1194 entries, 1020009 to 10390288`
<br>`Data columns (total 11 columns):`
| # |  Column       |   Non-Null Count | Dtype | 
|---|  ------       | --------------| ----- | 
| 0  | tconst         | 1194 non-null |   | object |
| 1  | titleType      | 1194 non-null |   | object |
| 2  | primaryTitle   | 1194 non-null |   | object |
| 3  | originalTitle  | 1194 non-null |   | object |
| 4  | isAdult        | 1194 non-null |   | object |
| 5  | startYear      | 1194 non-null |   | int64 | 
| 6  | endYear        | 1194 non-null |   | object |
| 7  | runtimeMinutes | 1194 non-null |   | object |
| 8  |  genres        | 1194 non-null |   | object |
| 9  | averageRating  | 1194 non-null |   | float64 |
| 10 | numVotes       | 1194 non-null |   | int64 | 

<br>`dtypes: float64(1), int64(2), object(8)`
<br>`memory usage: 111.9+ KB`


Analisis statistik juga diperlukan dalam dataset. Analisis statistik dibutuhkan untuk menganalisis penyebaran data masing-masing fitur dengan tipe data *integer* untuk melihat apakah ada data anomali. Tabel 5 menyediakan informasi statistik pada dataset yang digunakan. *Count* mrtupakan jumlah data untuk masing-masing fitur. *Min* dan *max* selanjutnya adalah nilai terkecil dan nilai terbesar dari suatu fitur. *Mean* (rata-rata) merupakan suatu ukuran pemusatan data. *Mean* suatu data juga merupakan statistik karena mampu menggambarkan bahwa data tersebut berada pada kisaran *mean* data tersebut. *Median* menentukan letak tengah data setelah data disusun menurut urutan nilainya. Bisa juga nilai tengah dari data-data yang terurut. Simbol untuk *median* adalah Me. Dengan *median* Me, maka 50% dari banyak data nilainya paling tinggi sama dengan Me, dan 50% dari banyak data nilainya paling rendah sama dengan Me. Dalam mencari *median*, dibedakan untuk banyak data ganjil dan banyak data genap. Standar deviasi adalah ukuran statistik yang digunakan untuk mengukur sejauh mana data dalam sebuah himpunan cenderung bervariasi dari nilai rata-ratanya. Standar Deviasi dan *Varians* Salah satu teknik statistik yg digunakan untuk menjelaskan homogenitas kelompok. *Varians* merupakan jumlah kuadrat semua deviasi nilai-nilai individual thd rata-rata kelompok. Sedangkan akar dari *varians* disebut dengan standar deviasi atau simpangan baku. Standar Deviasi dan *Varians* Simpangan baku merupakan variasi sebaran data. Semakin kecil nilai sebarannya berarti variasi nilai data makin sama Jika sebarannya bernilai 0, maka nilai semua datanya adalah sama. Semakin besar nilai sebarannya berarti data semakin bervariasi.

Informmasi statistik dataset pada Tabel 5 didapatkan bahwa keseluruhan videopada dataset adalah video dengan keluaran tahun 2024. Video dengan rating terkecil dan terbesar adalah video dengan rating 2 dan 10. Rata-rata video pada dataset memiliki rating sebesar 7,5. Jumlah *vote* pada video yang paling kecil adalah 5 dan video yang memiliki *vote* terbanyak adalah sebanyak 29356.
<br><br> Tabel 5 Informasi statistik dataset
|index|startYear|averageRating|numVotes|
|---|---|---|---|
|count|1194\.0|1194\.0|1194\.0|
|mean|2024\.0|7\.5574539363484075|259\.76214405360133|
|std|0\.0|1\.484414323663427|1449\.559752355421|
|min|2024\.0|2\.0|5\.0|
|25%|2024\.0|6\.8|9\.0|
|50%|2024\.0|7\.7|20\.0|
|75%|2024\.0|8\.6|66\.75|
|max|2024\.0|10\.0|29356\.0|

### *Univariate Analysis*

Distribusi untuk setiap rating video dapat dilihat pada Gambar 1. Berdasarkan hasil visualisasi didapatkan bahwa video yang memiliki rating 1 hingga 5 memiliki jumlah video yang sedikit. Bahkan video yang memiliki rating tertinggi yaitu 10 memiliki jumlah video yang lebih banyak dengan video yang memiliki rating 1 hingga 5 jika digabungkan. Hal ini menandakan bahwa dataset yang digunakan pada proyek ini memiliki dataset video yang cenderung memiliki video dengan rating yang tinggi. <br><br>
![image](https://github.com/abdussalamattaqwa/dicoding-ML_terapan-submission2/assets/67810655/f502b653-938a-453a-8371-31436c3daa80)
<br> Gambar 1. Distribusi rating video

<br> Gambar 2 memvisualisasikan distribusi setiap genre pada video. Berdasarkan hasil visualisasi didapatkan bahwa dataset memiliki lebih banyak video dengan *genre* drama dibandingkan dengan *genre* lainnya pada tahun 2024. Video dengan *genre* *musical* merupakan video yang memiliki jumlah sedikit pada tahun 2024.
![image](https://github.com/abdussalamattaqwa/dicoding-ML_terapan-submission2/assets/67810655/86fe108d-e8e1-4091-a5ad-8ed420e776a8)
<br> Gambar 2. Distribusi *genre* video

Visualisasi untuk distribusi fitru titleType dapat dilihat pada Tabel 6 dan Gambar 3. Berdasarkan hasil visualisasi pada dataset didapatkan bahwa video dengan jenis *tvEpisode* adalah video yang paling banyak dibuat, dan video dengan jenis hanya berupa video memiliki jumlah yang paling sedikit pada tahun 2024. Video dengan jenis tvEpisode memiliki jumlah yang sangat banyak. Pada dataset 70.2% berisi video dengan jenis tvEpisode. Apabila seluruh video dengan jenis yang lain digabungkan jumlahnya maka video dengan jenis tvEpisode tetap memiliki jumlah yang lebih banyak dibandingkan video dengan jenis lainnya

<br> Tabel 6. Distribusi jenis video
|Jenis Video| jumlah sampel |persentase
|---|---|---|
|tvEpisode              | 838 |  70.2 |
|movie                  | 164 |  13.7 |
|tvSeries               | 113 |  9.5 |
|tvMiniSeries           |  27 |  2.3 |
|tvMovie                |  20 |  1.7 |
|short                  |  18 |  1.5 |
|tvSpecial              |  10 |  0.8 |
|videoGame              |  2  |  0.2 |
|video                  |  2  |  0.2 |

![image](https://github.com/abdussalamattaqwa/dicoding-ML_terapan-submission2/assets/67810655/7cb2f017-b44c-4a2c-88d3-7c2f83dac815)
<br> Gambar 3. Distribusi jenis video

### *Multivariate Analysis*

Pada Gambar 4 memvisualisasikan untuk fitur averageRating, numVotes, dan titleType. Berdasarkan hasil visualisasi didapatkan bahwa video dengan jenis tvEpisode meskipun memilki jumlah yang banyak namun jumlah *vote* pada video tersebut memiliki jumlah *vote* yang sedikit. Voting adalah suatu kegiatan untuk menentukan pendapat melalui suara terbanyak. Video dengan jenis *movie* meskipun jumlahnya lebih sedikit namun kebanayk video dengan jenis ini memiliki jumlah *vote* yang banyak. Hal ini menandakan bahwa meskipun jenis video yang paling banyak dikeluarkan pada tahun 2024 adalah tvEpisode tidak menjamin setiap video memiliki *vote* yang banyak. Terdapat satu data video dengan jenis tvMiniSeries yang memiliki *vote* yang terbanyak dibandingkan video yang lainnya.
![image](https://github.com/abdussalamattaqwa/dicoding-ML_terapan-submission2/assets/67810655/5de1be21-9a69-455a-b1cc-9565ebc33d09)
<br> Gambar 3. Visualisasi fitur averageRating, numVotes, dan titleType

## Data Preparation
Pada tahap akhir dari *preprocessing* adalah *term-weighting*.*Term-weighting* merupakan proses pemberian bobot *term* pada dokumen. Pembobotan ini digunakan nantinya oleh *cosine similarity* untuk mencari kemiripan antar video. Pemberian bobot atau ekstraksi fitur pada setiap data  menggunakan metode CountVectorizer dan TF-IDF. 

* CountVectorizer adalah metode dasar pengukuran kata dengan menghitung jumlah kemunculan masing-masing kata pada dokumen, sehingga dapat disebut juga sebagai metode raw count. Implementasi metode ini akan menghasilkan sparse matrix. Perhitungan kemunculan kata adalah metode yang sederhana, namun jika kata-kata sering muncul pada banyak dokumen, kemungkinan jumlah kemunculan kata yang besar mungkin tidak berarti banyak dalam vektor kepentingan. Ekstraksi fitur CountVectorizer digunakan untuk mengekstraksi fitur pada *genres* karena dalam satu data video memiliki lebih dari satu *genre*.

* *Term Frequency — Inverse Document Frequency* atau TF — IDF adalah cara lain untuk mengevaluasi pentingnya kata dalam sebuah dokumen. Skor yang dihasilkan dari setiap fitur TF-IDF adalah perkalian dari *term frequency* dan *inverse document frequency*. Metode ini juga terkenal efisien, mudah dan memiliki hasil yang akurat. Metode ini akan menghitung nilai *Term Frequency (TF)* dan *Inverse Document Frequency* (IDF) pada setiap token (kata) di setiap dokumen dalam korpus. Secara sederhana, metode TF-IDF digunakan untuk mengetahui berapa sering suatu kata muncul di dalam dokumen. Oleh karena itu metode TF-IDF digunakan untuk mengetahui berapa sering suatu kata yang muncul pada fitur pada titleType.


## Modeling
Tahapan ini melakukan proses mencari kemiripan antar video berdasarkan ekstraksi fitur menggunakan CountVectorizer dan TfidfVectorizer. Metode yang digunakan adalah *cosine similairty* karena berfungsi untuk mengukur kesamaan dokumen dalam analisis teks. Cosine similairty mengukur kesamaan antara dua vektor. Kesamaan diukur dengan jarak sudut kosinus antara dua vektor dan menentukan apakah dua vektor menunjuk pada arah yang sama. Pada Tabel 7 dapat contoh dari hasil perhitungan kemiripan antara video. Setiap baris dan kolom merepresentasikan juvul video pada dataset. Nilai yang terdapat pada dataset merupakan nilai kemiripan antara video pada baris dan kolom nilai tersebut.

<br> Tabel 7. Hasil dari perhitungan kemiripan menggunakan *cosine similairty*
|originalTitle|Onty Bunty Love Story|In Season: Miami Dolphins 7|Le clown malheureux|The Roman Emperor&\#39;s Bathhouse|Trauma Bond|
|---|---|---|---|---|---|
|Jealousy|0\.0|0\.25|0\.7071067811865475|0\.35355339059327373|0\.35355339059327373|
|La tentation du mal|0\.3333333333333334|0\.2886751345948129|0\.408248290463863|0\.408248290463863|0\.408248290463863|
|The Truth Is Really Out There|0\.0|0\.2886751345948129|0\.816496580927726|0\.408248290463863|0\.408248290463863|
|New Year, New Mammy|0\.0|0\.35355339059327373|0\.9999999999999998|0\.4999999999999999|0\.4999999999999999|
|Run Boy Run|0\.2886751345948129|0\.25|0\.35355339059327373|0\.35355339059327373|0\.35355339059327373|
|Moonbreak Messenger|0\.0|0\.25|0\.7071067811865475|0\.35355339059327373|0\.35355339059327373|
|Tati Part Time|0\.408248290463863|0\.0|0\.4999999999999999|0\.0|0\.0|
|Radhika Ki Asliyat|0\.6666666666666669|0\.2886751345948129|0\.408248290463863|0\.408248290463863|0\.408248290463863|
|Oegye+in 2bu|0\.2886751345948129|0\.0|0\.0|0\.0|0\.0|
|Episode \#2\.7|0\.0|0\.5773502691896258|0\.408248290463863|0\.408248290463863|0\.408248290463863|


## Evaluation
Pada tahap ini dilakaukan proses mencari rekomendasi video dan memunculkan video-video yang direkomendasikan untuk ditonton. Rekomendasi video dicari menggunakan hasil dari pengukuran *cosine similarity*. Tahap pencarian video dilakukan dengan mencari video yang memiliki kemiripan yang tinggi kemudian diurutkan berdasarkan data rating video. Tabel 7 Memrupakan *final dataset* proses penggabungan antara dataframe IMDB dan Hasil perhitungan kemiripan antara video. Final dataset inilah yang akan memfilter untuk mencari rekomendasi video yang paling mirip dan memiliki rating tertinggi.

<br> Tabel 7. Final dataset hasil penggabungan antara dataset IMDB dan hasil perhitungan *cosine similarity*
|index|tconst|titleType|primaryTitle|originalTitle|isAdult|startYear|endYear|runtimeMinutes|genres|averageRating|numVotes|The Windigo|La Storia|Boy Swallows Universe|Who Is James Payton?|Qalb|Lion of Judah Legacy|Tauba Tera Jalwa|We Are All in This Together|High Score|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|0|tt10094388|movie|The Windigo|The Windigo|0|2024|\N|85|Horror,Thriller|6\.4|239|1\.0000000000000002|0\.0|0\.0|0\.408248290463863|0\.3333333333333334|0\.408248290463863|0\.6666666666666669|0\.408248290463863|0\.2886751345948129|
|1|tt10345822|tvSeries|La Storia|La Storia|0|2024|\N|\N|Drama|7\.3|36|0\.0|0\.9999999999999998|0\.35355339059327373|0\.0|0\.408248290463863|0\.0|0\.0|0\.0|0\.35355339059327373|
|2|tt10399902|tvMiniSeries|Boy Swallows Universe|Boy Swallows Universe|0|2024|2024|404|Crime,Drama,Mystery|8\.2|5143|0\.0|0\.35355339059327373|1\.0|0\.0|0\.2886751345948129|0\.0|0\.0|0\.0|0\.25|
|3|tt10469410|movie|Who Is James Payton?|Who Is James Payton?|0|2024|\N|\N|Documentary|8\.2|31|0\.408248290463863|0\.0|0\.0|0\.9999999999999998|0\.408248290463863|0\.4999999999999999|0\.408248290463863|0\.9999999999999998|0\.0|
|4|tt11281192|movie|Qalb|Qalb|0|2024|\N|147|Drama,Romance|9\.6|147|0\.3333333333333334|0\.408248290463863|0\.2886751345948129|0\.408248290463863|1\.0000000000000002|0\.408248290463863|0\.6666666666666669|0\.408248290463863|0\.2886751345948129|

Tabel 8 menunjukkan informasi video yang akan digunakan sebagai dasar untuk mencari rekomendasi video. Data nama dati video tersebut digunakan untuk mencari rekomendasi video yang diperlukan.
<br> Tabel 8. Data video yang digunakan sebagai dasar untuk menentukan rekomendasi video.
|index|tconst|titleType|primaryTitle|originalTitle|isAdult|startYear|endYear|runtimeMinutes|genres|averageRating|numVotes|
|---|---|---|---|---|---|---|---|---|---|---|---|
|594|tt30430918|movie|Tati Part Time|Tati Part Time|0|2024|\N|\N|Comedy|7\.4|401|

Tabel 9 Merupakan top 5 video yang paling direkomendasikan berdasarkan data nama video sebelumnya. Daftar video yang muncul merupakan video-video yang memiliki kemiripan yang tinggi dari nama video yang diinput dan memiliki rating tertinggi.
<br> Tabel 9. Hasil rekomendasi video 
|index|originalTitle|genres|averageRating|
|---|---|---|---|
|269|Vivekanandan Viralaanu|Comedy|9\.7|
|813|Intention|Comedy|9\.5|
|459|Plant Man|Comedy|9\.0|
|1122|Especial Éste|Comedy|8\.8|
|5|Lion of Judah Legacy|Comedy|7\.7|


Proyek ini adalah proyek sistem rekomendasi video untuk memudahkan pengguna mencari video yang diinginkan tanpa perlu mengahabiskan waktu untuk memilah satu per satu video dalam dataset. Sistem rekomendasi ini menggunakan fitur *titleType* dan *genres* untuk mencari kemiripan antar video. Fitur *titleType* dan *genres* diekstraksi fiturnya mennggunakan TF-IDF dan CountVectorizer. Hasil  ekstraksi fitur dijalankan melalui proses komputasi dengan menggunakan *cosine similarity* untuk mencari kesamaan antar video. Hasil perhitungan kemiripan tersebut dijadikan dasar untuk merekomendasikan video berdasarkan tingginya kemiripan video masukan. Video dengan kemiripan tinggi kemudian diurutkan berdasarkan data rating video  tertinggi. Hasil akhir dari proyek ini berupa daftar video rekomendasi berdasakan tingkat kemiripan video dan rating video.
