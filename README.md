# Deteksi Jumlah Jari Menggunakan Mediapipe dan OpenCV dengan Perhitungan Akurasi

## Penulis
Rivan Febrian, Ihsan Hafizh Kurniawan  
Institut Teknologi Garut, Indonesia  
Email: 12206111@itg.ac.id, 22206130@itg.ac.id  

## 1. Pendahuluan

### 1.1 Latar Belakang
Pendeteksian jari merupakan bidang dalam pemrosesan citra dan visi komputer yang digunakan dalam berbagai aplikasi seperti pengenalan gestur dan autentikasi biometrik. MediaPipe, pustaka open-source dari Google, memungkinkan pelacakan tangan secara real-time dengan efisiensi tinggi. Teknologi ini diterapkan dalam berbagai inovasi seperti augmented reality (AR) dan gaming.

### 1.2 Rumusan Masalah
1. Bagaimana cara mengintegrasikan library MediaPipe dan algoritma deteksi tepi Canny untuk mendeteksi angka yang ditunjukkan oleh jari secara akurat?
2. Bagaimana sistem ini dapat diimplementasikan secara real-time dengan mempertimbangkan berbagai kondisi lingkungan?

### 1.3 Tujuan
1. Mengembangkan sistem real-time untuk mendeteksi angka pada jari menggunakan MediaPipe dan algoritma deteksi tepi Canny.
2. Mengevaluasi akurasi dan keandalan sistem dalam berbagai kondisi lingkungan.

## 2. Metode Penelitian

### 2.1 MediaPipe
Pengenalan gerakan tangan dengan MediaPipe adalah suatu teknik untuk mengubah input video dengan mempertahankan informasi penting tentang gerakan tangan, sementara gaya atau tekstur visual dari video tersebut diubah  .[![DOI](https://zenodo.org/badge/924583453.svg)](https://doi.org/10.5281/zenodo.14773745)  Informasi penting tersebut terkait dengan pengenalan gestur tangan, baik oleh manusia maupun mesin, dan dapat dimanfaatkan dalam berbagai aplikasi seperti interaksi manusia dan komputer.

#### a. Pengumpulan Data
Pemilihan dataset merupakan langkah awal yang penting dalam penelitian ini. Dataset yang digunakan merupakan hasil rekaman gesture tangan dengan menggunakan MediaPipe. MediaPipe menyediakan model untuk mendeteksi landmark tangan, dimana model ini mendeteksi posisi tangan dengan 21 keypoint. Keypoint inilah yang disimpan sebagai dataset untuk melatih model agar bisa mendeteksi gesture tangan apa yang sedang dibuat. Ilustrasi keypoint dalam mediapipe ditunjukan pada Gambar.
![Image](https://github.com/user-attachments/assets/fac4f00c-97da-4aba-aa8b-176d6b8ccd13)
Proses pengumpulan data melibatkan beberapa variasi gesture. Setiap gesture disimpan keypoint-nya, masing-masing gestur direkam untuk tangan kanan dan tangan kiri. Gestur yang dipilih ditunjukan pada Gambar

![Image](https://github.com/user-attachments/assets/d9f2ab2d-b899-45f1-9073-e52a81d15422)
![Image](https://github.com/user-attachments/assets/bca7dd6a-aa36-41d8-8a86-4dbbcefce551)
![Image](https://github.com/user-attachments/assets/7e5f6c5a-692a-4633-a753-46fc3b947492)
![Image](https://github.com/user-attachments/assets/d2538ab5-e122-4adc-ad14-d0d05da2a3b3)
![Image](https://github.com/user-attachments/assets/833fd18d-5341-4559-bae6-2c753027bd31)

#### b. Struktur Model
![Image](https://github.com/user-attachments/assets/f85ab3c7-31f1-4499-bec8-bfb20e3dacc3)

Dataset yang digunakan untuk training dan menguji model merupakan data keypoint dari gestur yang didapatkan dari MediaPipe yang disimpan dalam csv saat pengumpulan data. Data train yang digunakan sebanyak 75% dan data test sebanyak 25% dari dataset yang ada. Proses pelatihan model dimulai dengan membangun arsitektur jaringan saraf menggunakan TensorFlow Keras. Model dibuat dalam bentuk sekuensial dengan beberapa lapisan untuk memproses input data. Lapisan pertama menerima vektor dengan dimensi 21×2, yang merupakan data koordinat x dan y dari 21 keypointtangan. Untuk mencegah overfitting, dua lapisan dropout diterapkan dengan probabilitas masing-masing 0,2 dan 0,4. Model juga memiliki dua lapisan dense (fully connected) dengan masing-masing 20 dan 10 neuron, menggunakan fungsi aktivasi ReLU untuk menangkap pola non-linear. Lapisan keluaran menggunakan fungsi aktivasi softmax dengan jumlah neuron yang sesuai dengan jumlah kelas yaitu 4, untuk mengklasifikasikan gestur tangan berdasarkan probabilitas. Model dikompilasi menggunakan Adam optimizer, yang merupakan algoritma pembaruan bobot yang efisien, serta fungsi loss sparse categorical crossentropy, karena data label diberikan dalam format integer. Metrik accuracy dipilih untuk memantau performa model selama pelatihan. Proses pelatihan dilakukan hingga maksimal 1000 epoch, dengan dua callback yang digunakan dalam proses ini adalah ModelCheckpoint untuk menyimpan bobot model terbaik secara otomatis setelah setiap epoch, dan EarlyStopping dengan toleransi 20 epoch untuk menghentikan pelatihan jika performa validasi tidak meningkat, sehingga menghindari overfitting. Dengan konfigurasi ini, model dapat dilatih secara optimal untuk mengenali gestur tangan berdasarkan dataset yang tersedia.

### 2.2 Deteksi Canny

Operator Canny, yang dikemukakan oleh John Canny pada tahun 1986,  terkenal sebagai operator deteksi tepi yang optimal. Algoritma ini memberikan tingkat kesalahan yang rendah, melokalisasi titik-titik tepi (jarak piksel-piksel tepi yang ditemukan deteksi dan tepi yang sesungguhnya sangat pendek), dan hanya memberikan satu tanggapan untuk satu tepi. berdasarkan [![DOI](https://zenodo.org/badge/924583453.svg)](https://doi.org/10.5281/zenodo.14773745), terdapat enam langkah yang dilakukan untuk mengimplementasikan deteksi tepi Canny. Keenam langkah tersebut adalah :
1. Penghalusan citra dengan filter Gaussian.
2. Perhitungan gradien citra.
3. Penentuan arah tepi.
4. Penghilangan non-maksimum.
5. Pengambangan histeresis dengan ambang batas.

## 3. Hasil dan Pembahasan
Pada penelitian ini, dilakukan pengembangan sistem deteksi jumlah jari menggunakan MediaPipe dan OpenCV serta perhitungan akurasi hasil deteksi. Metodologi yang digunakan mencakup beberapa tahapan utama yang dilakukan secara sistematis, yaitu inisialisasi pustaka dan model deteksi tangan, membaca gambar input, deteksi tangan dan jumlah jari yang terangkat, penerapan algoritma deteksi tepi menggunakan Canny, perhitungan akurasi, serta penampilan hasil deteksi dalam bentuk visual dan numerik.
### 3.1 Implementasi Sistem
1. **Inisialisasi Library dan Model Deteksi Tangan**
   - Tahapan pertama adalah inisialisasi pustaka dan model deteksi tangan. Pada tahap ini, pustaka yang diperlukan seperti OpenCV digunakan untuk membaca dan memproses gambar, MediaPipe digunakan untuk mendeteksi tangan beserta landmark jari, serta pustaka google.colab.patches digunakan untuk menampilkan gambar dalam lingkungan Google Colab. Selanjutnya, model MediaPipe Hands diinisialisasi untuk mendeteksi tangan dan mengidentifikasi 21 titik landmark tangan dalam gambar yang akan diproses.
   
2. **Membaca Gambar**
   - Setelah model diinisialisasi, langkah berikutnya adalah membaca gambar input yang berisi tangan menggunakan OpenCV. Gambar dibaca dari path file yang telah ditentukan dan ditampilkan sebelum diproses lebih lanjut. Tahap ini penting untuk memastikan bahwa gambar dapat diakses dan digunakan sebagai input untuk sistem.
3. **Deteksi Tangan dan Jari**
   - Tahapan selanjutnya adalah deteksi tangan dan jumlah jari yang terangkat. Gambar yang telah dibaca dikonversi ke format RGB karena model MediaPipe memproses gambar dalam format ini. Setelah itu, model MediaPipe digunakan untuk mendeteksi tangan dalam gambar dan mengidentifikasi landmark jari. Jika tangan terdeteksi, sistem akan menggambar landmark tangan pada gambar untuk memvisualisasikan hasil deteksi. Untuk menentukan jumlah jari yang terangkat, sistem mengevaluasi posisi ujung jari (tip) dan dasar jari (base). Jika posisi ujung jari lebih tinggi dari dasar jari dalam sumbu vertikal, maka jari tersebut dianggap terangkat. Proses ini dilakukan untuk empat jari utama, yaitu telunjuk, jari tengah, jari manis, dan kelingking.
4. **Deteksi Tepi dengan Algoritma Canny**
   - Setelah jumlah jari terangkat terdeteksi, sistem akan melakukan deteksi tepi menggunakan algoritma Canny Edge Detection. Proses ini dimulai dengan mengonversi gambar ke dalam format grayscale, karena deteksi tepi lebih optimal pada gambar hitam putih. Selanjutnya, algoritma Canny diterapkan dengan menggunakan threshold atas dan bawah tertentu untuk mengekstrak kontur dan tepi tangan. Hasil dari proses ini akan menunjukkan garis-garis tepi tangan dan jari yang terdeteksi.
5. **Perhitungan Akurasi**
   - Tahapan berikutnya adalah perhitungan akurasi deteksi jari. Akurasi dihitung dengan membandingkan jumlah jari yang terdeteksi dengan jumlah jari yang sebenarnya dalam gambarAkurasi dihitung menggunakan rumus:
     ```
     Akurasi = (1 - |Prediksi - Aktual| / Aktual) × 100%
     ```
Jika jumlah jari sebenarnya adalah 0 dan sistem juga mendeteksi 0 jari, maka akurasi 100%. Namun, jika jumlah jari yang terdeteksi tidak sesuai dengan jumlah jari sebenarnya, maka akurasi akan berkurang. Perhitungan ini bertujuan untuk mengevaluasi sejauh mana sistem dapat mengenali jumlah jari dengan benar.
6. **Menampilkan Hasil Deteksi**
   - Tahapan terakhir adalah menampilkan hasil deteksi dan akurasi. Hasil deteksi jari akan ditampilkan dalam bentuk visual, di mana sistem akan menambahkan teks pada gambar yang menunjukkan jumlah jari yang terdeteksi serta tingkat akurasinya. Selain itu, gambar hasil deteksi tepi juga akan ditampilkan sebagai referensi tambahan. Informasi numerik mengenai jumlah jari yang sebenarnya, jumlah jari yang terdeteksi, serta akurasi deteksi juga dicetak di layar untuk dianalisis lebih lanjut.
![Image](https://github.com/user-attachments/assets/bfe3f41b-71e2-4016-bb86-e41ecdb34bf5)

![Image](https://github.com/user-attachments/assets/f72e78d0-f07c-402a-9985-0e31dcdc45ba)

![Image](https://github.com/user-attachments/assets/bfdfeba4-417f-4682-9b3c-ff0e04864987)
![Image](https://github.com/user-attachments/assets/3cda5652-713f-4f0c-a843-681fdb091c1f)


![Image](https://github.com/user-attachments/assets/dd64f46b-1c78-45ab-bada-77e7c708a459)

![Image](https://github.com/user-attachments/assets/1b07d5d8-624b-46d0-8260-96aa5bf0238b)
### 3.2 Hasil Akurasi
| No | Deteksi | Aktual | Akurasi (%) |
|----|--------|--------|-------------|
| 1  | 1      | 1      | 100         |
| 2  | 2      | 2      | 100         |
| 3  | 3      | 4      | 67          |
| 4  | 4      | 4      | 100         |
| 5  | 4      | 5      | 80          |
| **Rata-rata** |  |  | **89.4%** |

## 4. Kesimpulan
Penelitian ini mengembangkan sistem deteksi jumlah jari menggunakan MediaPipe dan OpenCV dengan akurasi rata-rata 89,4%. Sistem ini efektif mendeteksi jari yang terangkat pada gambar statis, meskipun terdapat batasan pada pencahayaan buruk, latar belakang kompleks, atau posisi tangan yang tidak jelas. Deteksi tepi dengan algoritma Canny memberikan visual tambahan untuk analisis kontur tangan. Untuk pengembangan selanjutnya, disarankan untuk menggunakan teknik pra-pemrosesan gambar lebih lanjut dan model deep learning untuk meningkatkan akurasi serta menguji sistem pada berbagai kondisi pencahayaan dan latar belakang. Implementasi real-time juga direkomendasikan untuk aplikasi seperti pengenalan gestur.

## 5. Referensi
1. Adolph, R. (2016). Digital Image Processing. 
2. Beno, J., Silen, A., & Yanti, M. (2022). Gesture Recognition with MediaPipe. 
3. Daniel Tanugraha, F. et al. (2022). Long Short-Term Memory for Hand Gesture Recognition. 
4. Riana, D. et al. (2023). AlexNet Classification and Canny Edge Detection for Image Recognition. 
5. Supriyatin, W. (2020). Comparative Study of Edge Detection Methods.
6. Tsani, N. B., & Harliana, H. (2019). Implementation of Canny Edge Detection for Cancer Stage Detection.
