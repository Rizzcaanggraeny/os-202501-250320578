
# Laporan Praktikum Format IMRAD
## Topik : Perbandingan Algoritma Page Replacement FIFO dan LRU dalam Manajemen Memori Virtual

---

## Identitas
- **Nama**  : Rizzca Anggraeny
- **NIM**   : 250320578 
- **Kelas** : 1DSRA
---

## 1. Pendahuluan (Introduction)

### 1.1 Latar Belakang
Manajemen memori merupakan salah satu fungsi kritis dalam sistem operasi modern. Dengan adanya konsep memori virtual, sistem operasi dapat menjalankan program yang ukurannya melebihi kapasitas memori fisik yang tersedia dengan memanfaatkan mekanisme paging [1]. Ketika suatu halaman (*page*) yang diperlukan oleh proses tidak berada di memori utama, terjadi kondisi yang disebut *page fault*, yang mengharuskan sistem operasi memuat halaman tersebut dari penyimpanan sekunder.

Ketika memori penuh dan terjadi *page fault*, sistem operasi harus memilih halaman mana yang akan diganti untuk memberi ruang bagi halaman baru. Proses pemilihan halaman yang akan diganti ini dilakukan oleh algoritma *page replacement*. Pemilihan algoritma yang tepat sangat penting karena berpengaruh langsung terhadap jumlah *page fault* yang terjadi, yang pada gilirannya mempengaruhi performa sistem secara keseluruhan karena operasi I/O ke disk sangat mahal dari segi waktu [2].

### 1.2 Rumusan Masalah
Berdasarkan latar belakang tersebut, rumusan masalah dalam praktikum ini adalah:
1. Bagaimana cara kerja algoritma FIFO dan LRU dalam menangani *page replacement*?
2. Algoritma mana yang menghasilkan jumlah *page fault* lebih sedikit pada dataset yang sama?
3. Mengapa terdapat perbedaan performa antara kedua algoritma tersebut?

### 1.3 Tujuan Penelitian
Praktikum ini bertujuan untuk:
1. Mengimplementasikan dan mensimulasikan algoritma FIFO dan LRU dalam program
2. Membandingkan performa kedua algoritma berdasarkan jumlah *page fault* yang dihasilkan
3. Menganalisis penyebab perbedaan performa antara FIFO dan LRU pada pola akses tertentu

---

## 2. Metode (Methods)

### 2.1 Lingkungan Eksperimen
Simulasi dilakukan dengan spesifikasi sebagai berikut:
- **Bahasa Pemrograman**: Python 3.x
- **Reference String**: [7, 0, 1, 2, 0, 3, 0, 4, 2, 3, 0, 3, 2]
- **Jumlah Frame Memori**: 3 frame
- **Metode Pengukuran**: Penghitungan jumlah *page fault* untuk setiap algoritma

### 2.2 Algoritma yang Diimplementasikan

**Algoritma FIFO (First-In First-Out)**

FIFO mengganti halaman berdasarkan urutan kedatangannya ke memori. Halaman yang paling lama berada di memori akan diganti terlebih dahulu, tanpa mempertimbangkan kapan halaman tersebut terakhir diakses. Implementasi menggunakan indeks pointer yang berputar (*circular*) untuk menentukan posisi penggantian.

**Algoritma LRU (Least Recently Used)**

LRU mengganti halaman yang paling lama tidak digunakan dengan memanfaatkan informasi riwayat akses. Setiap kali halaman diakses, waktu aksesnya dicatat. Ketika terjadi *page fault* dan memori penuh, halaman dengan waktu akses paling lama akan diganti.

### 2.3 Prosedur Eksperimen

Langkah-langkah eksperimen yang dilakukan:

1. **Persiapan Dataset**: Menyiapkan *reference string* yang akan digunakan untuk simulasi kedua algoritma
2. **Implementasi FIFO**: 
   - Inisialisasi array memori dengan ukuran 3 frame
   - Untuk setiap halaman dalam *reference string*, cek apakah halaman sudah ada di memori
   - Jika tidak ada (*page fault*), ganti halaman pada posisi FIFO pointer
   - Catat setiap kejadian *hit* dan *fault*
3. **Implementasi LRU**:
   - Inisialisasi array memori dan struktur data untuk menyimpan waktu akses terakhir
   - Untuk setiap halaman, cek keberadaannya di memori
   - Jika tidak ada, ganti halaman dengan waktu akses paling lama
   - Perbarui waktu akses setiap halaman yang diakses
4. **Pengumpulan Data**: Mencatat langkah demi langkah proses penggantian halaman dan menghitung total *page fault*
5. **Analisis**: Membandingkan hasil kedua algoritma dan menganalisis penyebab perbedaan

### 2.4 Parameter Pengukuran
- **Page Hit**: Halaman yang diminta sudah berada di memori
- **Page Fault**: Halaman yang diminta tidak ada di memori, sehingga harus dimuat dari penyimpanan sekunder
- **Total Page Fault**: Jumlah keseluruhan *page fault* yang terjadi selama simulasi

---

## 3. Hasil (Results)

### 3.1 Hasil Simulasi FIFO



| Page | Frame 1 | Frame 2 | Frame 3 | Status |
|:----:|:-------:|:-------:|:-------:|:------:|
| 7    | 7       | -       | -       | FAULT  |
| 0    | 7       | 0       | -       | FAULT  |
| 1    | 7       | 0       | 1       | FAULT  |
| 2    | 2       | 0       | 1       | FAULT  |
| 0    | 2       | 0       | 1       | HIT    |
| 3    | 2       | 3       | 1       | FAULT  |
| 0    | 2       | 3       | 0       | FAULT  |
| 4    | 4       | 3       | 0       | FAULT  |
| 2    | 4       | 2       | 0       | FAULT  |
| 3    | 4       | 2       | 3       | FAULT  |
| 0    | 4       | 2       | 3       | HIT    |
| 3    | 4       | 2       | 3       | HIT    |
| 2    | 4       | 2       | 3       | HIT    |

**Total Page Fault FIFO: 10**

### 3.2 Hasil Simulasi LRU


| Page | Frame 1 | Frame 2 | Frame 3 | Status |
|:----:|:-------:|:-------:|:-------:|:------:|
| 7    | 7       | -       | -       | FAULT  |
| 0    | 7       | 0       | -       | FAULT  |
| 1    | 7       | 0       | 1       | FAULT  |
| 2    | 2       | 0       | 1       | FAULT  |
| 0    | 2       | 0       | 1       | HIT    |
| 3    | 2       | 0       | 3       | FAULT  |
| 0    | 2       | 0       | 3       | HIT    |
| 4    | 4       | 0       | 3       | FAULT  |
| 2    | 4       | 0       | 2       | FAULT  |
| 3    | 4       | 3       | 2       | FAULT  |
| 0    | 0       | 3       | 2       | FAULT  |
| 3    | 0       | 3       | 2       | HIT    |
| 2    | 0       | 3       | 2       | HIT    |

**Total Page Fault LRU: 9**

### 3.3 Perbandingan Performa FIFO dan LRU


| Algoritma | Jumlah Frame | Page Fault | Page Hit | Hit Rate |
|:---------:|:------------:|:----------:|:--------:|:--------:|
| FIFO      | 3            | 10         | 3        | 23.08%   |
| LRU       | 3            | 9          | 4        | 30.77%   |

### 3.4 Visualisasi Hasil

Berdasarkan data di atas, dapat diamati bahwa LRU menghasilkan *page fault* 10% lebih sedikit dibandingkan FIFO pada *reference string* yang digunakan. Hal ini menunjukkan bahwa LRU lebih efisien dalam memanfaatkan memori yang terbatas.

---

## 4. Pembahasan 

### 4.1 Analisis Perbedaan Performa

Hasil eksperimen menunjukkan bahwa algoritma LRU menghasilkan 9 *page fault*, sedangkan FIFO menghasilkan 10 *page fault*. Perbedaan ini terjadi karena cara masing-masing algoritma memilih halaman yang akan diganti.

FIFO menggunakan prinsip antrian sederhana di mana halaman yang paling lama masuk ke memori akan diganti terlebih dahulu, tanpa mempertimbangkan apakah halaman tersebut masih sering digunakan atau tidak. Pada *reference string* yang digunakan, terdapat pola penggunaan ulang halaman seperti halaman 0 dan 3 yang muncul beberapa kali. FIFO tidak dapat memanfaatkan informasi penggunaan ini, sehingga kadang mengganti halaman yang sebenarnya akan segera digunakan kembali.

Sebaliknya, LRU mempertimbangkan waktu akses terakhir setiap halaman. Dengan melacak kapan terakhir kali setiap halaman digunakan, LRU dapat mempertahankan halaman-halaman yang baru saja diakses dan lebih mungkin digunakan kembali dalam waktu dekat, sesuai dengan prinsip lokalitas temporal [3]. Hal ini membuat LRU lebih adaptif terhadap pola akses program.

### 4.2 Pengaruh Pola Akses

Perbedaan performa antara FIFO dan LRU sangat dipengaruhi oleh pola akses dalam *reference string*. Pada kasus ini, terdapat beberapa halaman yang diakses berulang kali dalam jangka waktu yang berdekatan, seperti:
- Halaman 0 muncul pada posisi ke-2, 5, 7, 11
- Halaman 3 muncul pada posisi ke-6, 10, 12
- Halaman 2 muncul pada posisi ke-4, 9, 13

LRU dapat memanfaatkan pola penggunaan berulang ini dengan mempertahankan halaman yang sering diakses, sementara FIFO tidak memiliki mekanisme untuk mengenali pola tersebut.

### 4.3 Implikasi pada Sistem Operasi Nyata

Dalam sistem operasi nyata, perbedaan satu *page fault* dapat berdampak signifikan terhadap performa sistem. Setiap *page fault* memerlukan akses ke disk yang membutuhkan waktu beberapa milidetik, jauh lebih lambat dibandingkan akses memori yang hanya membutuhkan nanosecond. Pada sistem dengan beban kerja tinggi yang mengalami ribuan *page fault* per detik, pengurangan 10% seperti yang ditunjukkan LRU dapat menghasilkan peningkatan performa yang substansial.

### 4.4 Belady's Anomaly pada FIFO

Salah satu kelemahan unik dari FIFO adalah kemungkinan terjadinya *Belady's Anomaly*, yaitu fenomena di mana penambahan jumlah frame justru dapat meningkatkan jumlah *page fault* [1]. Anomali ini tidak terjadi pada algoritma berbasis stack seperti LRU, yang membuatnya lebih dapat diprediksi dalam berbagai kondisi.

### 4.5 Keterbatasan Penelitian

Beberapa keterbatasan dalam praktikum ini perlu dicatat:
1. *Reference string* yang digunakan relatif pendek (13 referensi), sehingga mungkin tidak merepresentasikan pola akses program nyata yang lebih kompleks
2. Hanya menggunakan 3 frame memori, padahal sistem nyata biasanya memiliki ratusan hingga ribuan frame
3. Tidak mempertimbangkan overhead implementasi LRU yang lebih kompleks dibanding FIFO
4. Simulasi tidak memperhitungkan faktor-faktor lain seperti waktu eksekusi dan penggunaan CPU

### 4.6 Rekomendasi

Berdasarkan hasil eksperimen, untuk sistem dengan pola akses yang menunjukkan lokalitas temporal yang kuat, LRU merupakan pilihan yang lebih baik. Namun, untuk sistem dengan keterbatasan sumber daya komputasi atau yang memerlukan implementasi sederhana, FIFO masih dapat dipertimbangkan meskipun dengan performa yang sedikit lebih rendah.

---

## 5. Kesimpulan

Berdasarkan hasil eksperimen dan analisis yang telah dilakukan, dapat disimpulkan bahwa:

1. **Perbedaan Mekanisme**: FIFO mengganti halaman berdasarkan urutan masuk tanpa mempertimbangkan pola penggunaan, sedangkan LRU mengganti halaman yang paling lama tidak digunakan dengan memanfaatkan informasi riwayat akses.

2. **Performa Relatif**: Pada *reference string* [7, 0, 1, 2, 0, 3, 0, 4, 2, 3, 0, 3, 2] dengan 3 frame memori, LRU menghasilkan 9 *page fault* (lebih efisien 10%) dibandingkan FIFO yang menghasilkan 10 *page fault*.

3. **Pengaruh Pola Akses**: Perbedaan performa disebabkan oleh kemampuan LRU dalam memanfaatkan prinsip lokalitas temporal, yaitu mempertahankan halaman yang baru saja diakses karena cenderung akan digunakan kembali.

4. **Implikasi Praktis**: Meskipun LRU lebih kompleks dalam implementasi, efisiensi yang diperoleh dalam mengurangi *page fault* menjadikannya pilihan yang lebih baik untuk sistem operasi modern, terutama pada beban kerja dengan pola akses yang menunjukkan lokalitas tinggi.

---

## Quiz

### 1. Mengapa format IMRAD membantu membuat laporan praktikum lebih ilmiah dan mudah dievaluasi?

**Jawaban:**

Format IMRAD memberikan struktur yang terorganisir dan sistematis dalam menyajikan penelitian atau eksperimen. Pendahuluan (*Introduction*) menjelaskan konteks dan tujuan, Metode (*Methods*) memungkinkan replikasi eksperimen, Hasil (*Results*) menyajikan data objektif tanpa interpretasi, dan Pembahasan (*Discussion*) memberikan analisis mendalam. Struktur ini memudahkan pembaca untuk memahami alur logis dari latar belakang masalah hingga kesimpulan, serta memungkinkan evaluasi kritis terhadap validitas eksperimen dan interpretasi hasil. Format ini juga merupakan standar internasional dalam publikasi ilmiah, sehingga melatih mahasiswa untuk menulis secara profesional.

### 2. Apa perbedaan antara bagian Hasil dan Pembahasan?

**Jawaban:**

Bagian **Hasil** menyajikan data dan temuan eksperimen secara objektif dalam bentuk tabel, grafik, atau deskripsi faktual tanpa interpretasi atau opini. Contohnya: "Algoritma LRU menghasilkan 9 *page fault*". Bagian ini hanya melaporkan "apa yang terjadi".

Bagian **Pembahasan** menginterpretasi dan menganalisis hasil tersebut, menjelaskan "mengapa terjadi" dan "apa artinya". Di sini peneliti membandingkan hasil dengan teori, menjelaskan penyebab perbedaan, mengidentifikasi keterbatasan, dan memberikan implikasi dari temuan. Contohnya: "LRU menghasilkan *page fault* lebih sedikit karena memanfaatkan prinsip lokalitas temporal dengan mempertahankan halaman yang baru diakses".

Singkatnya, Hasil bersifat objektif dan deskriptif, sedangkan Pembahasan bersifat interpretatif dan analitis.

### 3. Mengapa sitasi dan daftar pustaka penting, bahkan untuk laporan praktikum?

**Jawaban:**

Sitasi dan daftar pustaka penting karena:

**Integritas Akademik**: Memberikan kredit kepada peneliti atau penulis yang idenya digunakan, menghindari plagiarisme dan menunjukkan kejujuran intelektual.

**Validasi Informasi**: Memungkinkan pembaca untuk memverifikasi keakuratan informasi dengan mengecek sumber aslinya, meningkatkan kredibilitas laporan.

**Dasar Teoritis**: Menunjukkan bahwa eksperimen atau analisis didasarkan pada pengetahuan yang telah ada, bukan sekadar spekulasi, serta menghubungkan praktikum dengan literatur ilmiah yang relevan.

**Pembelajaran Profesional**: Melatih mahasiswa untuk menulis secara profesional sesuai standar akademik dan ilmiah yang berlaku di dunia penelitian dan industri.

**Kemudahan Penelusuran**: Membantu pembaca yang ingin mempelajari topik lebih lanjut untuk menemukan sumber-sumber yang relevan dengan mudah.

Bahkan dalam praktikum sederhana, kebiasaan mengutip sumber yang tepat membentuk fondasi penting untuk penulisan ilmiah di masa depan.

---

## Daftar Pustaka

[1] Silberschatz, A., Galvin, P. B., & Gagne, G. (2018). *Operating System Concepts* (10th ed.). John Wiley & Sons.

[2] Tanenbaum, A. S., & Bos, H. (2014). *Modern Operating Systems* (4th ed.). Pearson Education.

[3] Remzi H. Arpaci-Dusseau and Andrea C. Arpaci-Dusseau. (2018). *Operating Systems: Three Easy Pieces*. Arpaci-Dusseau Books. Chapter 22: Beyond Physical Memory: Policies.

---


## Refleksi Diri
Tuliskan secara singkat:
- Apa bagian yang paling menantang minggu ini?  
- Bagaimana cara Anda mengatasinya?  

---

**Credit:**  
_Template laporan praktikum Sistem Operasi (SO-202501) – Universitas Putra Bangsa_
