# Laporan Proyek Machine Learning - Abd Salam At Taqwa
## Project Overview

Pasar industri film luar negeri hingga film dalam negeri diperkirakan akan semakin berkembang.  Dilihat dari  jumlah penonton film pada bioskop yang terus meningkat setiap tahunnya. Berdasarkan sumber pada [[1]](https://www.statista.com/statistics/307390/number-of-cinema-admissions-worldwide/) memperkirakan bioskop di seluruh dunia akan menjual 5,7 miliar tiket pada akhir tahun 2022, naik 73 persen dari tahun sebelumnya. Pendapatan bioskop global akan berjumlah sekitar 38 miliar dolar AS pada tahun 2022. Angka tersebut diproyeksikan akan terus tumbuh pada tahun-tahun berikutnya, meskipun dengan laju yang lebih lambat setelah tahun 2023. 

Seiring dengan berkembangnya teknologi, *platform* untuk menonton film atau video mulai bermunculan seperti You Tube, Netflix, Disney+ Hotstar, HBO Go, Vidio, iQiyi, Klik Film, Bioskop Online, Cinema Box, Viu, CatchPlay+, WeTV, Genflix, iFlix, Viki, Prime Video. Perkembangan film terutama pada era modern ini mulai berkembang cepat dengan mulai munculnya berbagai judul yang tak terbatas. Hal ini yang menjadi alasan mengapa sulit bagi orang untuk mencari suatu judul film. Oleh karena itu platform-platform untuk menonton film menerapkan sistem rekomendasi untuk membantu miliaran penggunanya menemukan konten film yang dipersonalisasi dari kumpulan film yang tersedia [[2,](https://www.ijcai.org/Proceedings/2019/0284.pdf)[3](https://dl.acm.org/doi/abs/10.1145/3469213.3470423)]. Sistem rekomendasi pada platform tersebut bekerja dengan cara memberikan informasi film – film kepada penonton, dengan maksud agar penonton tertarik dan berminat untuk menonton film – film yang ditawarkan dan masyarakat mengetahui gambaran film yang akan ditonton. Sebagai cara terbaik untuk mengatasi kelebihan informasi, sistem rekomendasi banyak digunakan untuk menyediakan konten dan layanan yang dipersonalisasi kepada pengguna dengan efisiensi tinggi [[4]](https://www.mdpi.com/2227-7390/11/4/895). Ketika sistem rekomendasi film berbasis konten digunakan, pengguna akan menerima beberapa film rekomendasi yang sangat mirip dengan film yang ditonton pengguna sebelumnya. Dalam sistem jenis ini, film umumnya dikelompokkan ke dalam kategori berbeda sesuai dengan kesamaannya. Atas dasar ini, sistem dapat merekomendasikan beberapa film potensial kepada pengguna berdasarkan preferensi tontonan pengguna yang direkam dalam database. Pada penelitian [[5]](https://j-ptiik.ub.ac.id/index.php/j-ptiik/article/view/9163) membuat sistem rekomendasi *content based filtering* dengan mencari kemiripan bobot dari *term* pada *bag of words*. Penelitian tersebut memanfaatkan fitur sinopsis film dan judul film untuk mencari kemiripan bobot antara film di dataset.

Berdasarkan latar belakang diatas proyek ini ingin membuat Sistem rekomendasi  data film yang mendeteksi kemiripan antara film yang Anda tonton dengan film lainnya menggunakan data fitur *titleType* dan *genres* pada film. Kedua data fitur tersebut selanjutnya digunakan untuk mencari dan mengurutkan film yang memiliki kemiripan yang sama. tahap awal sebelum mencari kemiripan dilakukan dengan proses *preprocessing* dimana data diolah sebelum dimasukkan kedalam model. *Feature extraction* adalah salah satu tahap *preprocessing* dimana teknik pengambilan ciri / *feature* dari suatu bentuk yang nantinya nilai yang didapatkan akan dianalisis untuk proses selanjutnya. Pada sub-tahapan *text feature extraction*, terdapat dua jenis metode yang diujicobakan, yaitu CountVectorizer dan TfidfVectorizer seperti yang dilakukan pada penelitian [[6]](https://pdfs.semanticscholar.org/3e3d/6b45d7e31f8a1e03ca7b94c1e360dac08e30.pdf). Hasil dari *feature extraction* selanjutnya digunakan untuk mencari kemiripan antar video.Kemiripan antara video dicari menggunakan metode *cosine similarity*. *Cosine similarity* adalah ukuran kesamaan antara dua buah vektor dalam sebuah ruang dimensi yang didapat dari nilai cosinus sudut dari perkalian dua buah vektor [[7]](https://ejournal.itn.ac.id/index.php/jati/article/download/364/351). Semakin banyak data maka semakin banyak memori yang digunakan untuk menyimpan hasil perkalian dua vektor dari metode *cosine similarity*. Berdasarkan hal tersebut pada proyek ini data yang digunakan dibatasi hanya berupa data video selama tahun terakhir saja. Selain metode kemiripan, proses pengurutan menggunakan fitur rating video sebagai pertimbangan untuk sistem rekomendasi. fitur rating video digunakan karena semakin tinggi rating video tersebut maka semakin berkualitas dan layak untuk ditonton. Sistem rekomendasi ini diharapkan dapat memudahkan pengguna karena tidak perlu lagi membuang waktu untuk mencari film atau video yang diinginkan satu per satu.


## Business Understanding

### Problem Statements

Berdasarkan latar belakang diatas, masalah yang dapat diangkat pada proyek ini adalah:
- Bagaimana membuat sistem rekomendasi video untuk pengguna agar dapat diterapkan pada platform menonton film dan memudahkan pengguna untuk mencari suatu judul film?
- Bagaimana mencari kemiripan antar video berdasarkan data fitur *titleType* dan *genres* menggunakan *cosine similarity*?
- Bagaimana menambahkan data rating video sebagai parameter tambahan untuk merekomendasikan video?

### Goals

Tujuan proyek adalah sebagai berikut:
- Membuat sistem rekomendasi video untuk memudahkan pengguna agar untuk mencari video yang diinginkan yang tersedia pada dataset platform menonton film.
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
|index|startYear|runtimeMinutes|averageRating|numVotes|
|---|---|---|---|---|
|count|449\.0|449\.0|449\.0|449\.0|
|mean|2024\.0|68\.73942093541203|7\.40467706013363|500\.9755011135857|
|std|0\.0|44\.71313605945213|1\.4733799957902611|2203\.358403913193|
|min|2024\.0|3\.0|2\.5|5\.0|
|25%|2024\.0|42\.0|6\.5|11\.0|
|50%|2024\.0|58\.0|7\.6|32\.0|
|75%|2024\.0|90\.0|8\.4|147\.0|
|max|2024\.0|404\.0|10\.0|29356\.0|

### *Univariate Analysis*

Jumlah *vote* pada film merupakan pemungutan suara oleh pengguna dengan tujuan untuk menilai seberapa bagus video tersebut. Gambar 1 memvisualisasikan distribusi jumlah *vote* pada masing-masing video. Berdasarkan hasil distribusi didapatkan bahwa kebanyakan film memiliki jumlah vote 0 hingga 300. Sangat sedikit video yang memiliki *vote* yang paling banyak. Hal ini menandakan bahwa jumlah video favorit yang membuat pengguna banyak menilai video tersebut sangat sedikit.
![image](https://github.com/abdussalamattaqwa/dicoding-ML_terapan-submission2/assets/67810655/7907ae69-7ffa-4142-ac87-7ad75e33db00)
<br> Gambar 1. Distribusi jumlah *vote* pada video

Gambar 2 merupakan visualisasi distribusi durasi film setiap video. berdasarkan hasil visualisasi didapatkan bahwa kebanyakan video berduarasi 50 menit dan sangat sedikit video yang memiliki durasi yang panjang atau lebih dari 150 menit.
![image](https://github.com/abdussalamattaqwa/dicoding-ML_terapan-submission2/assets/67810655/3c8e2495-c62e-4abb-83b5-dcdccc862644)
<br> Gambar 2. Visualisasi distribusi durasi film setiap video

Distribusi untuk setiap rating video dapat dilihat pada Gambar 3 Berdasarkan hasil visualisasi didapatkan bahwa video yang memiliki rating 1 hingga 5 memiliki jumlah video yang sedikit. Bahkan video yang memiliki rating tertinggi yaitu 10 memiliki jumlah video yang lebih banyak dengan video yang memiliki rating 1 hingga 5 jika digabungkan. Hal ini menandakan bahwa dataset yang digunakan pada proyek ini memiliki dataset video yang cenderung memiliki video dengan rating yang tinggi. <br><br>
![image](https://github.com/abdussalamattaqwa/dicoding-ML_terapan-submission2/assets/67810655/f502b653-938a-453a-8371-31436c3daa80)
<br> Gambar 3. Distribusi rating video

<br> Gambar 4 memvisualisasikan distribusi setiap genre pada video. Berdasarkan hasil visualisasi didapatkan bahwa dataset memiliki lebih banyak video dengan *genre* drama dibandingkan dengan *genre* lainnya pada tahun 2024. Video dengan *genre* *musical* merupakan video yang memiliki jumlah sedikit pada tahun 2024.
![image](https://github.com/abdussalamattaqwa/dicoding-ML_terapan-submission2/assets/67810655/86fe108d-e8e1-4091-a5ad-8ed420e776a8)
<br> Gambar 4. Distribusi *genre* video

Visualisasi untuk distribusi fitur titleType dapat dilihat pada Tabel 6 dan Gambar 5. Berdasarkan hasil visualisasi pada dataset didapatkan bahwa video dengan jenis *tvEpisode* adalah video yang paling banyak dibuat, dan video dengan jenis hanya berupa video memiliki jumlah yang paling sedikit pada tahun 2024. Video dengan jenis tvEpisode memiliki jumlah yang sangat banyak. Pada dataset 70.2% berisi video dengan jenis tvEpisode. Apabila seluruh video dengan jenis yang lain digabungkan jumlahnya maka video dengan jenis tvEpisode tetap memiliki jumlah yang lebih banyak dibandingkan video dengan jenis lainnya

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
<br> Gambar 5. Distribusi jenis video

### *Multivariate Analysis*

Pada gambar 6 memvisualisasikan untuk fitur averageRating, numVotes, dan titleType. Berdasarkan hasil visualisasi didapatkan bahwa video dengan jenis tvEpisode meskipun memilki jumlah yang banyak namun jumlah *vote* pada video tersebut memiliki jumlah *vote* yang sedikit. Voting adalah suatu kegiatan untuk menentukan pendapat melalui suara terbanyak. Video dengan jenis *movie* meskipun jumlahnya lebih sedikit namun kebanayk video dengan jenis ini memiliki jumlah *vote* yang banyak. Hal ini menandakan bahwa meskipun jenis video yang paling banyak dikeluarkan pada tahun 2024 adalah tvEpisode tidak menjamin setiap video memiliki *vote* yang banyak. Terdapat satu data video dengan jenis tvMiniSeries yang memiliki *vote* yang terbanyak dibandingkan video yang lainnya.
![image](https://github.com/abdussalamattaqwa/dicoding-ML_terapan-submission2/assets/67810655/5de1be21-9a69-455a-b1cc-9565ebc33d09)
<br> Gambar 6. Visualisasi fitur averageRating, numVotes, dan titleType

Pada gambar 7 memvisualisasikan fitur averageRating, runtimeMinutes, dan titleType. Berdasarkan hasil visualisasi didapatkan bahwa video yang memiliki durasi paling panjang adalah video dengan jenis tvMiniSeries. Meskipun memiliki durasi paling panjang namun jumlah video dengan jenis tvMiniSeries terbilang sedikit dibandingkan jenis lainnya. Sedangkan video dengan jenis movie memiliki rata-rata durasi yang lebih lama dibandingkan video dengan jenis tvSeries. Video dengan jenis *short* merupakan video dengan durasi paling singkat sesuai dengan jenis video tersebut dibandingkan video-video jenis lainnya.
![image](https://github.com/abdussalamattaqwa/dicoding-ML_terapan-submission2/assets/67810655/7429c0e4-fd5c-4e54-916c-011d5a318c03)
<br> Gambar 7. Visualisasi fitur averageRating, runtimeMinutes, dan titleType

## Data Preparation
Pada tahap akhir dari *preprocessing* adalah *term-weighting*.*Term-weighting* merupakan proses pemberian bobot *term* pada dokumen. Pembobotan ini digunakan nantinya oleh *cosine similarity* untuk mencari kemiripan antar video. Pemberian bobot atau ekstraksi fitur pada setiap data  menggunakan metode CountVectorizer dan TF-IDF. 

* CountVectorizer adalah metode dasar pengukuran kata dengan menghitung jumlah kemunculan masing-masing kata pada dokumen, sehingga dapat disebut juga sebagai metode raw count. Implementasi metode ini akan menghasilkan sparse matrix. Perhitungan kemunculan kata adalah metode yang sederhana, namun jika kata-kata sering muncul pada banyak dokumen, kemungkinan jumlah kemunculan kata yang besar mungkin tidak berarti banyak dalam vektor kepentingan. Ekstraksi fitur CountVectorizer digunakan untuk mengekstraksi fitur pada *genres* karena dalam satu data video memiliki lebih dari satu *genre*.

* *Term Frequency — Inverse Document Frequency* atau TF — IDF adalah cara lain untuk mengevaluasi pentingnya kata dalam sebuah dokumen. Skor yang dihasilkan dari setiap fitur TF-IDF adalah perkalian dari *term frequency* dan *inverse document frequency*. Metode ini juga terkenal efisien, mudah dan memiliki hasil yang akurat. Metode ini akan menghitung nilai *Term Frequency (TF)* dan *Inverse Document Frequency* (IDF) pada setiap token (kata) di setiap dokumen dalam korpus. Secara sederhana, metode TF-IDF digunakan untuk mengetahui berapa sering suatu kata muncul di dalam dokumen. Oleh karena itu metode TF-IDF digunakan untuk mengetahui berapa sering suatu kata yang muncul pada fitur pada titleType.

Pada Tabel 7 merupakan hasil dari ekstraksi fitur menggunakan CountVectorizer pada fitur *genres* dan TF — IDF pada fitur titleType. Terlihat bahwa setiap dataset video memiliki *bag of words* dari fitur *genres* dan titleType. Apabila pada video memiliki *genres* atau titleType tertentu, maka nilai pada kolom *genres* atau titleType tertentu tersebut akan menjadi 1. Jika tidak termasuk *genres* atau titleType tertentu, maka nilai pada kolom tersebut akan bernilai 0. Nilai-nilai inilah yang selnjutnya digunakan untuk mengukur tingkat kemiripan antar video menggunakan *cosine similarity*.
<br> Tabel 7. hasil dari ekstraksi fitur menggunakan CountVectorizer pada fitur *genres* dan TF — IDF pada fitur titleType
|originalTitle|movie|short|tvepisode|tvminiseries|tvmovie|tvseries|tvspecial|video|action|adventure|animation|biography|comedy|crime|documentary|drama|family|fantasy|game-show|history|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|The Windigo|1\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|
|Boy Swallows Universe|0\.0|0\.0|0\.0|1\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|1\.0|0\.0|1\.0|0\.0|0\.0|0\.0|0\.0|
|Qalb|1\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|1\.0|0\.0|0\.0|0\.0|0\.0|
|Lion of Judah Legacy|1\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|1\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|
|Tauba Tera Jalwa|1\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|

## Modelling & Result
### Modelling
Tahapan ini melakukan proses mencari kemiripan antar video berdasarkan ekstraksi fitur menggunakan CountVectorizer dan TfidfVectorizer. Metode yang digunakan adalah *cosine similairty* karena berfungsi untuk mengukur kesamaan dokumen dalam analisis teks. Cosine similairty mengukur kesamaan antara dua vektor. Kesamaan diukur dengan jarak sudut kosinus antara dua vektor dan menentukan apakah dua vektor menunjuk pada arah yang sama. Pada Gambar 8 merupakan rumus dari perhitungan *cosine similarity* berikut
![image](https://miro.medium.com/v2/resize:fit:1400/1*IhpY-6LYV75983THCpWo-w.png)
<br> Gambar 8. Rumus *cosine similarity*

Pada Tabel 8 dapat contoh dari hasil perhitungan kemiripan antara video. Setiap baris dan kolom merepresentasikan juvul video pada dataset. Nilai yang terdapat pada dataset merupakan nilai kemiripan antara video pada baris dan kolom nilai tersebut.

<br> Tabel 8. Hasil dari perhitungan kemiripan menggunakan *cosine similairty*
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


### Result
Pada tahap ini dilakaukan proses mencari rekomendasi video dan memunculkan video-video yang direkomendasikan untuk ditonton. Rekomendasi video dicari menggunakan hasil dari pengukuran *cosine similarity*. Tahap pencarian video dilakukan dengan mencari video yang memiliki kemiripan yang tinggi kemudian diurutkan berdasarkan data rating video. Tabel 9 Memrupakan *final dataset* proses penggabungan antara dataframe IMDB dan Hasil perhitungan kemiripan antara video. Final dataset inilah yang akan memfilter untuk mencari rekomendasi video yang paling mirip dan memiliki rating tertinggi.

<br><br> Tabel 9. Final dataset hasil penggabungan antara dataset IMDB dan hasil perhitungan *cosine similarity*
|index|tconst|titleType|primaryTitle|originalTitle|isAdult|startYear|endYear|runtimeMinutes|genres|averageRating|numVotes|The Windigo|La Storia|Boy Swallows Universe|Who Is James Payton?|Qalb|Lion of Judah Legacy|Tauba Tera Jalwa|We Are All in This Together|High Score|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|0|tt10094388|movie|The Windigo|The Windigo|0|2024|\N|85|Horror,Thriller|6\.4|239|1\.0000000000000002|0\.0|0\.0|0\.408248290463863|0\.3333333333333334|0\.408248290463863|0\.6666666666666669|0\.408248290463863|0\.2886751345948129|
|1|tt10345822|tvSeries|La Storia|La Storia|0|2024|\N|\N|Drama|7\.3|36|0\.0|0\.9999999999999998|0\.35355339059327373|0\.0|0\.408248290463863|0\.0|0\.0|0\.0|0\.35355339059327373|
|2|tt10399902|tvMiniSeries|Boy Swallows Universe|Boy Swallows Universe|0|2024|2024|404|Crime,Drama,Mystery|8\.2|5143|0\.0|0\.35355339059327373|1\.0|0\.0|0\.2886751345948129|0\.0|0\.0|0\.0|0\.25|
|3|tt10469410|movie|Who Is James Payton?|Who Is James Payton?|0|2024|\N|\N|Documentary|8\.2|31|0\.408248290463863|0\.0|0\.0|0\.9999999999999998|0\.408248290463863|0\.4999999999999999|0\.408248290463863|0\.9999999999999998|0\.0|
|4|tt11281192|movie|Qalb|Qalb|0|2024|\N|147|Drama,Romance|9\.6|147|0\.3333333333333334|0\.408248290463863|0\.2886751345948129|0\.408248290463863|1\.0000000000000002|0\.408248290463863|0\.6666666666666669|0\.408248290463863|0\.2886751345948129|

Tabel 10 menunjukkan informasi video yang akan digunakan sebagai dasar untuk mencari rekomendasi video. Data nama dati video tersebut digunakan untuk mencari rekomendasi video yang diperlukan.
<br><br> Tabel 10. Data video yang digunakan sebagai dasar untuk menentukan rekomendasi video.
|index|tconst|titleType|primaryTitle|originalTitle|isAdult|startYear|endYear|runtimeMinutes|genres|averageRating|numVotes|
|---|---|---|---|---|---|---|---|---|---|---|---|
|1|tt10399902|tvMiniSeries|Boy Swallows Universe|Boy Swallows Universe|0|2024|2024|404|Crime,Drama,Mystery|8\.2|5143|

Tabel 11 Merupakan top 5 video yang paling direkomendasikan berdasarkan data nama video sebelumnya. Daftar video yang muncul merupakan video-video yang memiliki kemiripan yang tinggi dari nama video yang diinput dan memiliki rating tertinggi.
<br><br> Tabel 11. Hasil rekomendasi video 
|index|originalTitle|genres|averageRating|
|---|---|---|---|
|441|Fool Me Once|Crime,Drama,Mystery|6\.9|
|27|Boy Loses Dad|Crime,Drama,Mystery|8\.6|
|28|Boy Takes Flight|Crime,Drama,Mystery|8\.5|
|37|Boy Meets End|Crime,Drama,Mystery|8\.4|
|9|Run Boy Run|Crime,Drama,Mystery|8\.3|


## Evaluation
Pada tahap evaluasi, hasil video dari sistem rekomendasi yang dibuat akan dinilai. Evaluasi dilakukan dengan cara menilai relevansi genre antara hasil video rekomendasi dan video masukan. Berikut metric precision pada evaluasi Content Based Filtering.
![image](https://github.com/abdussalamattaqwa/dicoding-ML_terapan-submission2/assets/67810655/4273b1e8-86aa-45bb-9b48-ef1e41b5ac30)

Adapun hasil dari relevansi setiap genre pada video dapat dilihat pada Tabel 12. Berdasarkan hasil pada Tabel 12 didapatkan bahwa sistem rekomendasi yang dibuat termasuk sistem rekomendasi yang baik karena video yang direkomendasikan memiliki relevansi yang sangat baik, yaitu 100%.
<br><br> Tabel 12. Hasil relevansi antara setiap genre dan setiap video yang direkomendasikan
|index|Genres|Precision|
|---|---|---|
|0|Crime|100\.0|
|1|Drama|100\.0|
|2|Mystery|100\.0|

<br>
Proyek ini adalah proyek sistem rekomendasi video untuk memudahkan pengguna mencari video yang diinginkan tanpa perlu mengahabiskan waktu untuk memilah satu per satu video dalam dataset. Sistem rekomendasi ini menggunakan fitur *titleType* dan *genres* untuk mencari kemiripan antar video. Fitur *titleType* dan *genres* diekstraksi fiturnya mennggunakan TF-IDF dan CountVectorizer. Hasil  ekstraksi fitur dijalankan melalui proses komputasi dengan menggunakan *cosine similarity* untuk mencari kesamaan antar video. Hasil perhitungan kemiripan tersebut dijadikan dasar untuk merekomendasikan video berdasarkan tingginya kemiripan video masukan. Video dengan kemiripan tinggi kemudian diurutkan berdasarkan data rating video  tertinggi. Hasil akhir dari proyek ini berupa daftar video rekomendasi berdasakan tingkat kemiripan video dan rating video.
