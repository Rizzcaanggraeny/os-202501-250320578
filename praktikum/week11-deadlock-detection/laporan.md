
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
   membentuk siklus menggunakan Resource Allocation Graph (RAG) untuk visualisasi algoritma deteksi cek graf berkala.

3. Simulasi dari Tanenbaum (Modern Operating Systems, Bab 6): Simulasi model skenario alokasi tanpa risiko nyata. Versi
   Banker's Algorithm simulasi permintaan proses untuk cek keadaan aman; jika tidak, deadlock diduga. Cocok untuk multi
   threaded, kurangi false positive, dan deteksi tanpa hentikan eksekusi.

4. Deteksi Praktis dari OSTEP (Bab 32): Gunakan wait-for graph di mana edge tunjuk proses menunggu yang lain. Algoritma
   sederhana iterasi cari proses macet simulasi alokasi hipotetis konfirmasi. Pseudocode termasuk rollback jika deadlock
   ditemukan, seimbangkan overhead dan responsivitas untuk sistem Unix-like.

5. Deteksi deadlock lengkapi strategi lain dengan fokus identifikasi siklus via simulasi grafik/algoritma, izinkan
   pemulihan seperti preemption. Optimal untuk sistem deadlock jarang, simulasi validasi tanpa ganggu operasional.
   
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

```bash
processes = ["P1", "P2", "P3"]

allocation = {
    "P1": "R1",
    "P2": "R2",
    "P3": "R3"
}

request = {
    "P1": "R2",
    "P2": "R3",
    "P3": "R1"
}

# Resource -> Process
resource_owner = {v: k for k, v in allocation.items()}

# Membuat Wait-For Graph
wait_for = {p: resource_owner[request[p]] for p in processes}

# Cetak tabel Wait-For Graph
print("TABEL WAIT-FOR GRAPH")
print("-" * 40)
print(f"{'Proses':<10}{'Menunggu Proses':<20}")
print("-" * 40)
for p, w in wait_for.items():
    print(f"{p:<10}{w:<20}")
print("-" * 40)

# Deteksi siklus sederhana
visited = []
current = processes[0]

while current not in visited:
    visited.append(current)
    current = wait_for[current]

deadlock_processes = visited

# Cetak tabel hasil deteksi
print("\nTABEL HASIL DETEKSI DEADLOCK")
print("-" * 40)
print(f"{'Proses':<10}{'Status':<20}")
print("-" * 40)
for p in processes:
    status = "Deadlock" if p in deadlock_processes else "Tidak Deadlock"
    print(f"{p:<10}{status:<20}")
print("-" * 40)

```

---

## Hasil Eksekusi
Sertakan screenshot hasil percobaan atau diagram:
![Screenshot hasil](screenshots/example.png)

---

## Analisis
- Sajikan hasil deteksi dalam tabel (proses deadlock / tidak).
  
| Proses | Resource Allocation | Resource Request |
| :----: | :-----------------: | :--------------: |
|   P1   |          R1         |        R2        |
|   P2   |          R2         |        R3        |
|   P3   |          R3         |        R1        |

Keterangan:

- P1 menunggu R2 yang sedang dipegang oleh P2
- P2 menunggu R3 yang sedang dipegang oleh P3
- P3 menunggu R1 yang sedang dipegang oleh P1

Terbentuk siklus:
P1 → P2 → P3 → P1
Hasil program sesuai analisi 

- Jelaskan mengapa deadlock terjadi atau tidak terjadi.
  - Deadlock terjadi karena keempat kondisi deadlock terpenuhi secara bersamaan.
    Tidak ada proses yang dapat melanjutkan eksekusi, sehingga sistem berada dalam kondisi deadlock.
- Kaitkan hasil dengan teori deadlock (empat kondisi).
  - Deadlock terjadi karena keempat kondisi deadlock terpenuhi:
    1. Mutual Exclusion
       Resource (R1, R2, R3) hanya dapat digunakan satu proses dalam satu waktu.
    2. Hold and Wait
       Setiap proses memegang satu resource dan menunggu resource lain.
    3. No Preemption
       Resource tidak dapat diambil paksa, hanya dilepas secara sukarela.
    4. Circular Wait
       P1 → P2 → P3 → P1 membentuk siklus.
    Karena semua kondisi terpenuhi, sistem pasti deadlock.


---

## Kesimpulan
1. Algoritma deteksi deadlock berhasil mengidentifikasi deadlock melalui siklus pada Wait-For Graph.
2. Berdasarkan hasil analisis teori deadlock dan sistem terbukti berada dalam kondisi deadlock karena terdapat siklus yang
   melibatkan seluruh proses.

---

## Quiz
1. Apa perbedaan antara *deadlock prevention*, *avoidance*, dan *detection*?
- **jawaban:**
   - Prevention: Mencegah adanya deadlock dari awal dengan aturan ketat.
   - Avoidance: Mengecek risiko sebelum bertindak, lebih dinamis.
   - Detection: Menangani setelah terjadi deadlock, reaktif dan berisiko gangguan. Prevention paling sederhana, avoidance
     paling seimbang, detection paling efisien untuk kasus jarang.
3. Mengapa deteksi deadlock tetap diperlukan dalam sistem operasi?
- **jawaban:karena**
     - Prevention terlalu kaku: Batasi sumber daya, kurangi efisiensi.
     - Avoidance sulit: Butuh data akurat, overhead tinggi, risiko kesalahan.
     - Deadlock bisa muncul tak terduga: Kesalahan manusia atau bug bisa sebabkan, detection biarkan sistem normal lalu
       pulihkan.
     - Efisien untuk sistem besar: Deadlock jarang, jadi detection hemat biaya daripada cegah semua.
       Detection melengkapi yang lain, seimbangkan efisiensi dan keamanan. Tanpa itu, sistem bisa terjebak deadlock tanpa
       solusi.
3. Apa kelebihan dan kekurangan pendekatan deteksi deadlock?
- **jawaban:**
- **Kelebihan Pendekatan Deteksi Deadlock:** 
   - Efisien Saat Sistem Berjalan Normal: Sistem tidak dibatasi aturan ketat, jadi sumber daya bisa digunakan sepenuhnya.
     Bagus jika deadlock jarang, seperti di server besar atau cloud.
   - Fleksibel dan Bisa Menyesuaikan: Tidak perlu data detail tentang kebutuhan proses seperti metode avoidance, dan bisa
     mengatasi masalah tak terduga (misal, bug atau kerusakan hardware).
     Gangguan Sedikit: Deteksi dilakukan sesekali atau saat dibutuhkan, tanpa mengganggu kerja sehari-hari sampai deadlock
     ketahuan.
- **Kekurangan Pendekatan Deteksi Deadlock:**
   - Biaya Komputasi Tinggi: Algoritma untuk mendeteksi (misal, mencari lingkaran dalam grafik alokasi) butuh waktu dan
     sumber daya, apalagi di sistem besar.
   - Pemulihan Mahal: Setelah ketahuan, memperbaiki (misal, matikan proses atau ambil sumber daya) bisa buang pekerjaan
     hentikan sistem, atau rusak data.
   - Risiko Masalah Lama: Jika deteksi telat, sistem bisa macet lama, dan ini tidak cegah deadlock—cuma tangani setelah
     terjadi.
---

## Refleksi Diri
Tuliskan secara singkat:
- Apa bagian yang paling menantang minggu ini?
  - Bagian yang paling menantang adalah memahami alur terjadinya deadlock dan merepresentasikannya dalam bentuk algoritma,
    khususnya saat menentukan hubungan antar proses dalam Wait-For Graph.
- Bagaimana cara Anda mengatasinya?
  - Saya mengatasinya dengan menganalisis kasus secara manual terlebih dahulu, menggambar alur tunggu antar proses, lalu
    mencocokkannya dengan teori deadlock. Setelah alurnya jelas, implementasi program menjadi lebih mudah dan hasilnya
    sesuai dengan analisis.

---

**Credit:**  
_Template laporan praktikum Sistem Operasi (SO-202501) – Universitas Putra Bangsa_
