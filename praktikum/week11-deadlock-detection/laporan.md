
# Laporan Praktikum Minggu 11
# Topik: Simulasi dan Deteksi Deadlock

---

## Identitas
- **Nama**  : Rizzca Anggraeny
- **NIM**   : 250320578 
- **Kelas** : 1DSRA

---

## A. Deskripsi Singkat
Pada praktikum minggu ini, mahasiswa akan mempelajari **mekanisme deteksi deadlock** dalam sistem operasi.  
Berbeda dengan Minggu 7 yang berfokus pada *pencegahan dan penghindaran deadlock*, pada minggu ini mahasiswa diarahkan untuk **mendeteksi deadlock yang telah terjadi** menggunakan pendekatan algoritmik.

Mahasiswa akan membuat **program simulasi sederhana deteksi deadlock**, menjalankan dataset uji, serta menyajikan hasil analisis dalam bentuk tabel dan interpretasi logis.

---

## B. Tujuan
Setelah menyelesaikan tugas ini, mahasiswa mampu:
1. Membuat program sederhana untuk mendeteksi deadlock.  
2. Menjalankan simulasi deteksi deadlock dengan dataset uji.  
3. Menyajikan hasil analisis deadlock dalam bentuk tabel.  
4. Memberikan interpretasi hasil uji secara logis dan sistematis.  
5. Menyusun laporan praktikum sesuai format yang ditentukan.

---

## Dasar Teori
1. Definisi Deteksi Deadlock: Deteksi deadlock adalah cara reaktif untuk menemukan dan menyelesaikan deadlock setelah
   terjadi, berbeda dari pencegahan atau penghindaran risiko. Simulasi digunakan untuk memodelkan alokasi sumber daya dan
   cek siklus tanpa mengubah sistem asli.

2. Konsep dari Silberschatz (Operating System Concepts, Bab 7): Deadlock muncul saat proses saling tunggu sumber daya,
   membentuk siklus. Gunakan Resource Allocation Graph (RAG) untuk visualisasi; algoritma deteksi cek graf berkala. Simulasi
   alokasi hipotetis prediksi siklus, efisien jika deadlock jarang, tapi ada overhead dan biaya pemulihan.

3. Simulasi dari Tanenbaum (Modern Operating Systems, Bab 6): Simulasi model skenario alokasi tanpa risiko nyata. Versi
   Banker's Algorithm simulasi permintaan proses untuk cek keadaan aman; jika tidak, deadlock diduga. Cocok untuk multi
   threaded, kurangi false positive, dan deteksi tanpa hentikan eksekusi.

4. Deteksi Praktis dari OSTEP (Bab 32): Gunakan wait-for graph di mana edge tunjuk proses menunggu yang lain. Algoritma
   sederhana iterasi cari proses macet; simulasi alokasi hipotetis konfirmasi. Pseudocode termasuk rollback jika deadlock
   ditemukan, seimbangkan overhead dan responsivitas untuk sistem Unix-like.

5. Deteksi deadlock lengkapi strategi lain dengan fokus identifikasi siklus via simulasi grafik/algoritma, izinkan pemulihan
   seperti preemption. Optimal untuk sistem deadlock jarang, simulasi validasi tanpa ganggu operasional; lihat bab terkait
   buku untuk detail.
   
---
## C. Ketentuan Teknis
- Bahasa pemrograman **bebas** (Python / C / Java / lainnya).  
- Program berbasis **terminal**, tidak memerlukan GUI.  
- Fokus penilaian pada **logika algoritma deteksi deadlock**, bukan kompleksitas bahasa.

Struktur folder (sesuaikan dengan template repo):
```
praktikum/week11-deadlock-detection/
├─ code/
│  ├─ deadlock_detection.*
│  └─ dataset_deadlock.csv
├─ screenshots/
│  └─ hasil_deteksi.png
└─ laporan.md
```

---
## Langkah Praktikum
1. **Menyiapkan Dataset**

   Gunakan dataset sederhana yang berisi:
   - Daftar proses  
   - Resource Allocation  
   - Resource Request / Need

   Contoh tabel:

   | Proses | Allocation | Request |
   |:--:|:--:|:--:|
   | P1 | R1 | R2 |
   | P2 | R2 | R3 |
   | P3 | R3 | R1 |

2. **Implementasi Algoritma Deteksi Deadlock**

   Program minimal harus:
   - Membaca data proses dan resource.  
   - Menentukan apakah sistem berada dalam kondisi deadlock.  
   - Menampilkan proses mana saja yang terlibat deadlock.

3. **Eksekusi & Validasi**

   - Jalankan program dengan dataset uji.  
   - Validasi hasil deteksi dengan analisis manual/logis.  
   - Simpan hasil eksekusi dalam bentuk screenshot.

4. **Analisis Hasil**

   - Sajikan hasil deteksi dalam tabel (proses deadlock / tidak).  
   - Jelaskan mengapa deadlock terjadi atau tidak terjadi.  
   - Kaitkan hasil dengan teori deadlock (empat kondisi).


---

## Kode / Perintah
Tuliskan potongan kode atau perintah utama:
```bash
uname -a
lsmod | head
dmesg | head
```

---

## Hasil Eksekusi
Sertakan screenshot hasil percobaan atau diagram:
![Screenshot hasil](screenshots/example.png)

---

## Analisis
- Jelaskan makna hasil percobaan.  
- Hubungkan hasil dengan teori (fungsi kernel, system call, arsitektur OS).  
- Apa perbedaan hasil di lingkungan OS berbeda (Linux vs Windows)?  

---

## Kesimpulan
Tuliskan 2–3 poin kesimpulan dari praktikum ini.

---

## Quiz
### Quiz
Jawab pada bagian **Quiz** di laporan:
1. Apa perbedaan antara *deadlock prevention*, *avoidance*, dan *detection*?
   - Prevention: Mencegah adanya deadlock dari awal dengan aturan ketat.
   - Avoidance: Mengecek risiko sebelum bertindak, lebih dinamis.
   - Detection: Menangani setelah terjadi deadlock, reaktif dan berisiko gangguan. Prevention paling sederhana, avoidance
     paling seimbang, detection paling efisien untuk kasus jarang.
3. Mengapa deteksi deadlock tetap diperlukan dalam sistem operasi?
   - Meski ada prevention (mencegah dengan aturan ketat) dan avoidance (hindari risiko dengan cek), detection tetap penting
     karena:
     - Prevention terlalu kaku: Batasi sumber daya, kurangi efisiensi.
     - Avoidance sulit: Butuh data akurat, overhead tinggi, risiko kesalahan.
     - Deadlock bisa muncul tak terduga: Kesalahan manusia atau bug bisa sebabkan, detection biarkan sistem normal lalu
       pulihkan.
       Efisien untuk sistem besar: Deadlock jarang, jadi detection hemat biaya daripada cegah semua.
       Detection melengkapi yang lain, seimbangkan efisiensi dan keamanan. Tanpa itu, sistem bisa terjebak deadlock tanpa
       solusi.
5. Apa kelebihan dan kekurangan pendekatan deteksi deadlock?
   - Efisien Saat Sistem Berjalan Normal: Sistem tidak dibatasi aturan ketat, jadi sumber daya bisa digunakan sepenuhnya.
     Bagus jika deadlock jarang, seperti di server besar atau cloud.
   - Fleksibel dan Bisa Menyesuaikan: Tidak perlu data detail tentang kebutuhan proses seperti metode avoidance, dan bisa
     mengatasi masalah tak terduga (misal, bug atau kerusakan hardware).
    Gangguan Sedikit: Deteksi dilakukan sesekali atau saat dibutuhkan, tanpa mengganggu kerja sehari-hari sampai deadlock
    ketahuan.

### Kekurangan Pendekatan Deteksi Deadlock

- *Biaya Komputasi Tinggi: Algoritma untuk mendeteksi (misal, mencari lingkaran dalam grafik alokasi) butuh waktu dan
  sumber daya, apalagi di sistem besar.
- *Pemulihan Mahal: Setelah ketahuan, memperbaiki (misal, matikan proses atau ambil sumber daya) bisa buang pekerjaan
  hentikan sistem, atau rusak data.
- Risiko Masalah Lama: Jika deteksi telat, sistem bisa macet lama, dan ini tidak cegah deadlock—cuma tangani setelah
  terjadi.
---

## Refleksi Diri
Tuliskan secara singkat:
- Apa bagian yang paling menantang minggu ini?  
- Bagaimana cara Anda mengatasinya?  

---

**Credit:**  
_Template laporan praktikum Sistem Operasi (SO-202501) – Universitas Putra Bangsa_
