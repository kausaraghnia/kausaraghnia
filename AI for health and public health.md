# AI for health and public health
Membuat AI untuk memberi tahu resep kesehatan yang akurat berdasarkan data individu yang diberikan.

## Introduction
Kesehatan dan kesejahteraan adalah nikmat yang sangat berharga bagi setiap manusia , namun manusia hanya paham begitu berharganya nikmat kesehatan ketika mereka diberikan rasa sakit. Setiap manusia pasti ingin yang namanya sehat terus. Untuk mewujudkan hal tersebut maka kita harus menerapkan pola hidup sehat. Adapun ketika sudah terlanjur merasakan yang namanya sakit , maka manusia akan berusaha berobat ke dokter ataupun puskesmas agar mendapatkan obat atau pun solusi dari sakit yang dideritanya. Dokter yang memriksa lalu memberikan diagnosa dan meberikan obat kadang tidak selalu manjur. Dalam bebrapa kasus ada Dokter yang salah mendiagnosa penyakit pasiennya yang membuat si pasien tidak sembuh-sembuh. Walaupun sedikit terjadi dalam masyarakat namun merupakan kesalaha yang fatal jika sampai membuat pasien menderita penyakit yang baru akibat salah diagnosa. Itu merupakan salah satu human eror. Maka dari itu disini AI diciptakan untuk memberikan diagnosa penyakit yang dialami lebih awal , lalu memberikan solusi gaya hidup sehat untuk mencegah penyakit tersebut tambah parah.

## End Goal
1. AI dapat memperkirakan penyakit yang dialami.
2. AI memberikan solusi pencegahan agar tidak semakin memburuk.

## How it affect people
1. Dokter dapat memberikan diagnosa yang sesuai berdasarkan data dan memberikan obat yang benar.
2. Pasien tidak perlu bingung pola hidup yang bagaimana yang harus dilakukan ketika sedang menderita penyakit tertentu.
3. Dapat mendeteksi penyakit sedini mungkin.

## Metodologi
Untuk gambaran metode yang digunakan dalam sistem AI ini bisa dilihat pada gambar dibawah
![image](https://github.com/kausaraghnia/kausaraghnia/assets/148691014/a3738fa6-4cf7-4ed3-a119-49b342b49cf7)

KNN atau K-Nearest Neighbors digunakan untuk memprediksi berdasarkan data yang diberikan. KNN adalah salah satu alogaritma yang cocok digunakan untuk melakukan klasifikasi dan regresi pada suau data. Kegunaan tersebut sangat tepat pada prediksi diagnosa penyakit pasien karena pada dasarnya KNN membandingkan data yang diberikan dengan data yang sudah ada untuk menghasilkan data baru atau presiksi, data baru di sini adalah hasil pemeriksaan pasien. KNN akan diberikan data data tentang hasil pemeriksaan dan hasil diagnosanya yang mana itu merupakan data yang nantinya akan dibandingkan dengan data yang diberikan. Semakin banyak data pembanding maka semakin bagus juga nanti presiksi yang diberikan atau semakin akurat. Data yang diberikan atau data yang baru ini adalah data hasil pemeriksaan pasien yang nanti masuk kedalam KNN untuk dibandingkan. Keluaran dari KNN adalah prediksi penyakit dari pasien tersebut. lalu dari hasil prediksi tersebut nanti akan diproses untuk dicarikan solusi yang tepat dalam penanganan agar dapat mencegah atau mengurangi penyakit tersebut agar tidak kambuh atau pun agar tidak semakin parah. 

## Conclusion
Didunia ini setiap waktu pasti ada orang yang sakit dan juga ada yang berobat namun tak kunjung sembuh karena diagnosa dan obat yang dberikan salah. Dalam hal ini AI berperan membantu manusia sebagai pemberi prediksi dan solusi sehingga mengurangi terjadinya salah diagnosa terhadap pasien. Metode KNN digunakan dalam perancangan AI ini yang mana digunakan untuk memprediksi penyakit yang di derita pasien dan memberikan solusi pola hidup yang tepat. Penggunaan Alogaritma KNN ini tepat karena pasti akan banyak data pasien yang bisa digunakan sebagai data pemabanding untuk menghasilkan prediksi yang akurat. Akan tetapi juga data pembanding harus diperhatikan kualitasnya karena akan berdampak pada keakuratan prediksi dan solusi pencegahan yang mana itu merupakan langkah yang penting dalam pemeliharaan kesehatan pribadi manusia. Hasil akhinrnya Ai ini dapt membantu dokter dan pasien dan bisa memberikan kesejahteraan umum yang diinginkan.

## Reference
1. http://scholar.unand.ac.id/8060/2/BAB%20I.pdf
2. https://eprints.ums.ac.id/43093/6/BAB%20I.pdf
3. http://repository.syekhnurjati.ac.id/5166/2/BAB%20I%20hesti.pdf
4. https://www.ibm.com/topics/knn 
5. https://www.w3schools.com/python/python_ml_knn.asp
