
# Laporan Praktikum Minggu 5
Topik: Penjadwalan CPU - FCFS dan SJF

---

## Identitas
- **Nama**  : Rizzca Anggraeny 
- **NIM**   : 250320578
- **Kelas** : 1DSRA

---

## Tujuan
Setelah menyelesaikan tugas ini, mahasiswa mampu:
1. Menghitung waiting time dan turnaround time untuk algoritma FCFS dan SJF.
2. Menyajikan hasil perhitungan dalam tabel yang rapi dan mudah dibaca.
3. Membandingkan performa FCFS dan SJF berdasarkan hasil analisis.
4. Menjelaskan kelebihan dan kekurangan masing-masing algoritma.
5. Menyimpulkan kapan algoritma FCFS atau SJF lebih sesuai digunakan.

---

## Dasar Teori
1. Konsep penjadwalan CPU
   - Penjadwalan CPU merupakan mekanisme dalam sistem operasi yang bertujuan mengatur urutan eksekusi proses di CPU agar penggunaan
     prosesor menjadi optimal. Penjadwalan dilakukan oleh CPU Scheduler untuk memilih proses yang akan dijalankan selanjutnya dari
     antrian.
   - > (sumber Silberschatz et.al (2018, operating sistem concepts, bab 5)
3. FCFS (First Come, First Served)
   FCFS adalah metode sederhana yang menjalankan proses berdasarkan urutan kedatangan, seperti antrean. Mudah diterapkan, namun bisa
   menyebabkan proses lain menunggu lama jika ada proses besar di awal (convoy effect).
   - > (Sumber: Silberschatz et al. (2018, Bab 5.2)
4. SJF (Shortest Job First)
   SJF menjalankan proses dengan waktu terpendek lebih dulu. Algoritma ini bisa non-preemptive atau preemptive (SRTF). Hasilnya efisien
   karena mengurangi waktu tunggu, tapi butuh perkiraan waktu eksekusi yang tepat dan berisiko membuat proses panjang tertunda
   (starvation).
   - > (Sumber: Tanenbaum & Bos (2015, Bab 2.4.2)
5. Perbandingan FCFS dan SJF
   FCFS lebih mudah dan adil, sedangkan SJF lebih cepat karena meminimalkan waktu tunggu hingga 50%. Namun, SJF bisa tidak adil bagi
   proses besar.
   - > (Sumber: OSTEP (2018, Bab 8.3).
6. Penerapan di Sistem Linux
   FCFS mirip dengan algoritma Completely Fair Scheduler (CFS) di Linux yang berfokus pada keadilan waktu CPU. Sedangkan konsep SJF bisa
   ditiru dengan pengaturan prioritas atau mode SCHED_FIFO.
   - > (Sumber: Tanenbaum & Bos (2015, Bab 10.3); OSTEP (2018, Bab 9); Linux Manual Pages (Scheduling).

---

## Langkah Praktikum
1. **Siapkan Data Proses**
   Gunakan tabel proses berikut sebagai contoh (boleh dimodifikasi dengan data baru):
   | Proses | Burst Time | Arrival Time |
   |:--:|:--:|:--:|
   | P1 | 6 | 0 |
   | P2 | 8 | 1 |
   | P3 | 7 | 2 |
   | P4 | 3 | 3 |

2. **Eksperimen 1 – FCFS (First Come First Served)**
   - Urutkan proses berdasarkan *Arrival Time*.  
   - Hitung nilai berikut untuk tiap proses:
     ```
     Waiting Time (WT) = waktu mulai eksekusi - Arrival Time
     Turnaround Time (TAT) = WT + Burst Time
     ```
   - Hitung rata-rata Waiting Time dan Turnaround Time.  
   - Buat Gantt Chart sederhana:  
     ```
     | P1 | P2 | P3 | P4 |
     0    6    14   21   24
     ```

3. **Eksperimen 2 – SJF (Shortest Job First)**
   - Urutkan proses berdasarkan *Burst Time* terpendek (dengan memperhatikan waktu kedatangan).  
   - Lakukan perhitungan WT dan TAT seperti langkah sebelumnya.  
   - Bandingkan hasil FCFS dan SJF pada tabel berikut:

     | Algoritma | Avg Waiting Time | Avg Turnaround Time | Kelebihan | Kekurangan |
     |------------|------------------|----------------------|------------|-------------|
     | FCFS | ... | ... | Sederhana dan mudah diterapkan | Tidak efisien untuk proses panjang |
     | SJF | ... | ... | Optimal untuk job pendek | Menyebabkan *starvation* pada job panjang |

4. **Eksperimen 3 – Visualisasi Spreadsheet (Opsional)**
   - Gunakan Excel/Google Sheets untuk membuat perhitungan otomatis:
     - Kolom: Arrival, Burst, Start, Waiting, Turnaround, Finish.
     - Gunakan formula dasar penjumlahan/subtraksi.
   - Screenshot hasil perhitungan dan simpan di:
     ```
     praktikum/week5-scheduling-fcfs-sjf/screenshots/
     ```

5. **Analisis**
   - Bandingkan hasil rata-rata WT dan TAT antara FCFS & SJF.  
   - Jelaskan kondisi kapan SJF lebih unggul dari FCFS dan sebaliknya.  
   - Tambahkan kesimpulan singkat di akhir laporan.

6. **Commit & Push**
   ```bash
   git add .
   git commit -m "Minggu 5 - CPU Scheduling FCFS & SJF"
   git push origin main
   ```

---

## Hasil Eksekusi
Sertakan screenshot hasil percobaan atau diagram:
![Screenshot hasil](screenshots/example.png)

---

## Analisis
1. Bandingkan hasil rata-rata WT dan TAT antara FCFS & SJF.

| Algoritma | Rata-rata Waiting Time (WT) | Rata-rata Turnaround Time (TAT) |   Keterangan   |
|--------------|-------------------------------|-----------------------------------|----|
| FCFS (First Come, First Served) | 8,75 | 14,75 | Proses dijalankan berdasarkan urutan kedatangan tanpa memperhatikan lama waktu eksekusi. Waktu tunggu menjadi lebih lama jika proses pertama membutuhkan waktu panjang. |
| SJF (Shortest Job First) | 6,25 | 12,25 | Proses dengan waktu eksekusi paling pendek dijalankan lebih dulu, sehingga total waktu tunggu dan penyelesaian lebih singkat. |

| Algoritma | Kelebihan | Kekurangan |
|--------------|--------------|---------------|
| FCFS | - Sederhana dan mudah diterapkan.<br>- Adil karena dijalankan berdasarkan urutan datang.<br>- Tidak memerlukan informasi lama eksekusi proses. | - Menyebabkan convoy effect (proses cepat menunggu proses lama).<br>- Kurang efisien jika durasi proses bervariasi. |
| SJF | - Lebih efisien karena waktu tunggu dan turnaround lebih kecil.-<br>- Cocok untuk proses dengan waktu eksekusi berbeda-beda. | - Sulit diterapkan karena harus tahu lama eksekusi proses.<br>- Dapat menyebabkan starvation bagi proses panjang.|

2. Jelaskan kondisi kapan SJF lebih unggul dari FCFS dan sebaliknya
   -  Kondisi SJF Lebih Unggul
     Berdasarkan tabel di atas, SJF menghasilkan waktu tunggu rata-rata 6.25 dan turnaround time rata-rata 12.25, yang lebih kecil
     dibanding FCFS (WT = 8.75, TAT = 14.75).Ini menunjukkan bahwa SJF lebih efisien dalam mengatur proses.
   - Kondisi unggulnya SJF
     - Ketika burst time tiap proses berbeda-beda, seperti pada data: P1 = 6, P2 = 8, P3 = 7, P4 = 3
     - Karena perbedaan waktu eksekusi cukup besar, SJF memprioritaskan proses dengan waktu lebih singkat (misalnya P4 yang hanya
       detik), sehingga proses cepat tidak menunggu lama.
    - SJF mengurangi waktu tunggu total karena proses pendek selesai duluan, sehingga tidak menghambat proses lainnya.
      Contoh pada data:
      Dalam SJF, P2 yang memiliki burst time hanya 3 dijalankan lebih awal daripada P3 (burst 7) dan P4 (burst 8). Akibatnya, waktu
      tunggu rata-rata seluruh proses turun menjadi 6.25 saja.
   - Kondisi FCFS Lebih Unggul
     - Ketika semua proses memiliki burst time yang hampir sama, FCFS akan bekerja dengan baik karena perbedaan waktu tunggu tidak
       signifikan.
     - Sangat adil, karena proses dilayani berdasarkan urutan datang tanpa diskriminasi terhadap durasi eksekusi.
     - Contoh dari data:
       Pada FCFS, proses dijalankan berdasarkan waktu kedatangan:
       Urutannya P1 → P2 → P3 → P4, tanpa memperhatikan lamanya burst time.
       Namun akibat P2 dan P3 memiliki waktu eksekusi yang lama (8 dan 7), proses terakhir (P4) menunggu terlalu lama (WT = 18).
       Inilah yang menyebabkan rata-rata WT dan TAT lebih besar dibanding SJF.
       
3. Tambahkan kesimpulan singkat di akhir laporan.
   - SJF lebih unggul karena menghasilkan waktu tunggu (6.25) dan waktu penyelesaian (12.25) yang lebih kecil daripada FCFS (8.75 dan
     14.75).
   - SJF lebih efisien dan cepat, cocok digunakan saat burst time proses bervariasi.
   -  FCFS lebih sederhana dan adil, cocok digunakan bila durasi proses hampir sama atau sistem tidak dapat memperkirakan waktu eksekusi.


---

## Kesimpulan
- Hasil percobaan, algoritma SJF (Shortest Job First) terbukti lebih efisien dibandingkan FCFS (First Come First Served). Hal ini
  terlihat dari rata-rata waktu tunggu dan waktu penyelesaian SJF yang lebih kecil (WT = 6.25 dan TAT = 12.25) dibandingkan FCFS (WT
  8.75 dan TAT = 14.75).
- SJF lebih cepat karena menjalankan proses dengan waktu eksekusi terpendek terlebih dahulu, sehingga efisiensi CPU meningkat.
- FCFS lebih sederhana dan adil karena mengeksekusi proses sesuai urutan kedatangan, namun kurang optimal saat durasi proses berbeda jauh.

---
### Tugas
1. Hitung *waiting time* dan *turnaround time* dari minimal 2 skenario FCFS dan SJF.
- Skenario 1
  - FCFS
    - Waiting Time : 8,75
    - Turnaround Time : 14,75
  - SJF
    - Waiting Time :6,25
    - Turnaround Time : 12,25

- Skenario 2
  - FCFS
    - Waiting Time : 4,75
    - Turnaround Time : 8,5
  - SJF
    - Waiting Time :2,25
    - Turnaround Time : 6,5

2. Sajikan hasil perhitungan dalam tabel perbandingan (FCFS vs SJF).  
  
   
- FCFS (First Come First Served) pertama


| Proses | Burst Time | Arrival Time | Start Time | Finish Time | Waiting Time | Turnaround Time |
|:--------|:-----------:|:--------------:|:-------------:|:--------------:|:----------------:|:------------------:|
| P1 | 6 | 0 | 0 | 6 | 0 | 6 |
| P2 | 8 | 1 | 6 | 14 | 5 | 13 |
| P3 | 7 | 2 | 14 | 21 | 12 | 19 |
| P4 | 3 | 3 | 21 | 24 | 18 | 21 |
| *Jumlah* |  |  |  |  | *35* | *59* |
| *Rata-rata* |  |  |  |  | *8.75* | *14.75* |




- SJF (Shortest Job First) pertama

| Proses | Burst Time | Arrival Time | Start Time | Finish Time | Waiting Time | Turnaround Time |
|:--------|:-----------:|:--------------:|:-------------:|:--------------:|:----------------:|:------------------:|
| P1 | 6 | 0 | 0 | 6 | 0 | 6 |
| P2 | 3 | 3 | 6 | 9 | 3 | 6 |
| P3 | 7 | 2 | 9 | 16 | 7 | 14 |
| P4 | 8 | 1 | 16 | 24 | 15 | 23 |
| Jumlah |  |  |  |  | 25 | 49 |
| Rata-rata |  |  |  |  | 6.25 | 12.25 |

- FCFS (First Come First Served) kedua
  
| proses | burts time | arrival time | start | waiting time | turnaround time | finish |
|:--------:|:------------:|:--------------:|:-------:|:--------------:|:-----------------:|:--------:|
| P1 | 3 | 0 | 0 | 0 | 3 | 3 |
| P2 | 6 | 1 | 3 | 2 | 8 | 9 |
| P3 | 4 | 2 | 9 | 7 | 11 | 13 |
| P4 | 2 | 3 | 13 | 10 | 12 | 15 |
| Jumlah |  |  |  | 19 | 34 |  |
| Rata-rata |  |  |  | 4.75 | 8.5 |  |



- SJF (Shortest Job First) kedua
  
| proses | burts time | arrival time | start | waiting time | turnaround time | finish |
|---|---|---|---|---|---|---|
| p1 | 3 | 0 | 0 | 0 | 3 | 3 |
| p2 | 2 | 3 | 3 | 0 | 2 | 5 |
| p3 | 4 | 2 | 5 | 3 | 7 | 9 |
| p4 | 6 | 1 | 9 | 8 | 14 | 15 |
| Jumlah |  |  |  | 11 | 26 |  |
| Rata-rata |  |  |  | 2.75 | 6.5 |  |


4. Analisis kelebihan dan kelemahan tiap algoritma.
   
| *Algoritma* | *Kelebihan* | *Kelemahan* |
|:--------------:|:--------------|:---------------|
| *FCFS (First Come First Served)* | - Konsep sederhana dan mudah diterapkan. <br> - Proses dijalankan sesuai urutan kedatangan (adil). <br> - Cocok jika waktu proses relatif sama. | - Tidak efisien bila waktu eksekusi antar proses sangat berbeda. <br> - Dapat terjadi convoy effect (proses lain menunggu lama). <br> - Waktu tunggu (WT) dan waktu penyelesaian (TAT) rata-rata lebih tinggi. |
| *SJF (Shortest Job First)* | - Waktu tunggu (WT) dan turnaround time (TAT) rata-rata lebih rendah. <br> - Efisiensi CPU lebih baik. <br> - Proses cepat diselesaikan lebih dahulu. | - Sulit diterapkan karena CPU tidak selalu tahu waktu eksekusi tiap proses. <br> - Dapat menyebabkan starvation untuk proses berdurasi panjang. <br> - Kurang adil jika banyak proses pendek datang berturut-turut. |

6. Simpan seluruh hasil dan analisis ke `laporan.md`.  

## Quiz
1. Apa perbedaan utama antara FCFS dan SJF?
   jawaban :
   - Urutan proses dijalankan
     FCFS menjalankan proses berdasarkan urutan kedatangan — siapa yang datang dulu, dia yang diproses lebih dulu.
     Sedangkan SJF memilih proses dengan waktu pengerjaan paling singkat untuk dijalankan terlebih dahulu.
   - Jenis penjadwalan (preemption)
     FCFS bersifat non-preemptive, artinya satu proses akan berjalan sampai selesai tanpa bisa dihentikan.
     SJF bisa non-preemptive maupun preemptive (disebut juga Shortest Remaining Time First), di mana proses bisa dihentikan jika ada
     proses baru dengan waktu yang lebih singkat.
   - Kinerja dan efisiensi
     SJF biasanya memberikan waktu tunggu rata-rata yang lebih kecil, sehingga sistem terasa lebih cepat.
     FCFS lebih sederhana, tetapi kadang membuat proses yang pendek harus menunggu lama jika ada proses panjang lebih dulu.
   - Kelemahan masing-masing
     FCFS bisa menyebabkan convoy effect, yaitu proses cepat tertunda karena menunggu proses panjang selesai.
     SJF bisa menyebabkan starvation, yaitu proses panjang menunggu terus karena selalu ada proses singkat yang datang.
3. Mengapa SJF dapat menghasilkan rata-rata waktu tunggu minimum?
   jawaban : Algoritma Shortest Job First (SJF) menghasilkan waktu tunggu rata-rata paling kecil karena selalu menjalankan proses dengan
   waktu eksekusi paling singkat terlebih dahulu. Dengan cara ini, proses pendek cepat selesai dan CPU bisa segera menangani proses
   berikutnya, sehingga waktu tunggu total menjadi lebih efisien. Secara teori, urutan ini membuat waktu tunggu rata-rata lebih rendah
   dibanding algoritma lain seperti FCFS, karena proses singkat tidak harus menunggu proses panjang selesai. Efisiensi ini berlaku baik
   pada versi non-preemptive maupun preemptive (SRTF), asalkan waktu eksekusi tiap proses dapat diperkirakan dengan cukup akurat. 
5. Apa kelemahan SJF jika diterapkan pada sistem interaktif?
   jawaban : Algoritma Shortest Job First (SJF) kurang cocok untuk sistem interaktif karena memiliki beberapa kelemahan. Proses yang
   membutuhkan waktu eksekusi lama bisa terus tertunda jika selalu ada proses yang lebih singkat, sehingga terjadi starvation dan sistem
   menjadi tidak adil. Selain itu, SJF sulit memberikan respons cepat terhadap input pengguna karena tidak mempertimbangkan prioritas
   atau kebutuhan real-time. Di sistem interaktif, perkiraan waktu eksekusi (burst time) juga sering tidak akurat karena prosesnya
   bergantung pada interaksi pengguna yang tidak bisa diprediksi. Akibatnya, performa sistem bisa menurun dan tidak efisien. 

---

## Refleksi Diri
Tuliskan secara singkat:
- Apa bagian yang paling menantang minggu ini?
  - yang paling menantang minggu ini memahami perbedaan setiap algortma dan menentukan start dan finish  
- Bagaimana cara Anda mengatasinya?
  - Cara mengatasinya adalah dengan memahami konsep tiap algoritma, berlatih menghitung waktu tunggu dan turnaround, membuat diagra
     Gantt untuk membantu urutan proses, serta sering berlatih dan berdiskusi dengan teman agar lebih paham dan tidak keliru saat praktik.

---

**Credit:**  
_Template laporan praktikum Sistem Operasi (SO-202501) – Universitas Putra Bangsa_
