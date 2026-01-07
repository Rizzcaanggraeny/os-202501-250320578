
# Laporan Praktikum Minggu 10
Topik: Manajemen Memori – Page Replacement (FIFO & LRU)

---

## Identitas
- **Nama**  : Rizzca Anggraeny
- **NIM**   : 250320578 
- **Kelas** : 1DSRA

---

## Deskripsi Singkat
Pada praktikum minggu ini, mahasiswa akan mempelajari **manajemen memori virtual**, khususnya mekanisme **page replacement**.  
Fokus utama praktikum adalah memahami bagaimana sistem operasi mengganti halaman (*page*) di memori utama ketika terjadi *page fault*, serta membandingkan performa algoritma **FIFO (First-In First-Out)** dan **LRU (Least Recently Used)**.

Mahasiswa akan mengimplementasikan **program simulasi page replacement**, menjalankan dataset uji, dan menyajikan hasil dalam bentuk tabel atau grafik.

---
## Tujuan
1. Mengimplementasikan algoritma page replacement FIFO dalam program.
2. Mengimplementasikan algoritma page replacement LRU dalam program.
3. Menjalankan simulasi page replacement dengan dataset tertentu.
4. Membandingkan performa FIFO dan LRU berdasarkan jumlah *page fault*.
5. Menyajikan hasil simulasi dalam laporan yang sistematis.

---

## Dasar Teori
1. Manajemen memori virtual memungkinkan sistem operasi menggunakan sebagian penyimpanan sekunder sebagai perpanjangan
   memori utama melalui mekanisme paging.

3. Page fault terjadi ketika halaman yang dibutuhkan proses tidak berada di memori utama, sehingga sistem harus memuatnya
   dari penyimpanan sekunder.

5. Page replacement diperlukan saat memori penuh, untuk menentukan halaman mana yang harus diganti agar halaman baru dapat
   dimuat.

7. Algoritma FIFO mengganti halaman berdasarkan urutan kedatangan tanpa mempertimbangkan pola penggunaan, sehingga dapat
   menghasilkan page fault yang tinggi.

9. Algoritma LRU mengganti halaman yang paling lama tidak digunakan dengan memanfaatkan riwayat akses, sehingga umumnya
    lebih efisien dalam mengurangi page fault.
---

## Langkah Praktikum
1. **Menyiapkan Dataset**

   Gunakan *reference string* berikut sebagai contoh:
   ```
   7, 0, 1, 2, 0, 3, 0, 4, 2, 3, 0, 3, 2
   ```
   Jumlah frame memori: **3 frame**.

2. **Implementasi FIFO**

   - Simulasikan penggantian halaman menggunakan algoritma FIFO.
   - Catat setiap *page hit* dan *page fault*.
   - Hitung total *page fault*.

3. **Implementasi LRU**

   - Simulasikan penggantian halaman menggunakan algoritma LRU.
   - Catat setiap *page hit* dan *page fault*.
   - Hitung total *page fault*.

4. **Eksekusi & Validasi**

   - Jalankan program untuk FIFO dan LRU.
   - Pastikan hasil simulasi logis dan konsisten.
   - Simpan screenshot hasil eksekusi.

5. **Analisis Perbandingan**

   Buat tabel perbandingan seperti berikut:

   | Algoritma | Jumlah Page Fault | Keterangan |
   |:--|:--:|:--|
   | FIFO | ... | ... |
   | LRU | ... | ... |


   - Jelaskan mengapa jumlah *page fault* bisa berbeda.
   - Analisis algoritma mana yang lebih efisien dan alasannya.

6. **Commit & Push**

   ```bash
   git add .
   git commit -m "Minggu 10 - Page Replacement FIFO & LRU"
   git push origin main
   ```

---

## Kode / Perintah
Kode yang digunakan

```bash
reference_string = [7, 0, 1, 2, 0, 3, 0, 4, 2, 3, 0, 3, 2]
frames = 3


def print_process_table(title, steps):
    print(f"\n{title}")
    print("+" + "-"*8 + "+" + "-"*10 + "+" + "-"*10 + "+" + "-"*10 + "+" + "-"*10 + "+")
    print("| Page   | Frame 1  | Frame 2  | Frame 3  | Status   |")
    print("+" + "-"*8 + "+" + "-"*10 + "+" + "-"*10 + "+" + "-"*10 + "+" + "-"*10 + "+")
    
    for page, mem, status in steps:
        print(f"| {page:<6} | {mem[0]:<8} | {mem[1]:<8} | {mem[2]:<8} | {status:<8} |")
    
    print("+" + "-"*8 + "+" + "-"*10 + "+" + "-"*10 + "+" + "-"*10 + "+" + "-"*10 + "+")


# ================= FIFO =================
def fifo_page_replacement(ref, frames):
    memory = ['-'] * frames
    fifo_index = 0
    steps = []
    page_fault = 0

    for page in ref:
        if page in memory:
            steps.append((page, memory.copy(), "HIT"))
        else:
            page_fault += 1
            memory[fifo_index] = page
            fifo_index = (fifo_index + 1) % frames
            steps.append((page, memory.copy(), "FAULT"))

    return page_fault, steps


# ================= LRU =================
def lru_page_replacement(ref, frames):
    memory = ['-'] * frames
    last_used = {}
    steps = []
    page_fault = 0

    for time, page in enumerate(ref):
        if page in memory:
            steps.append((page, memory.copy(), "HIT"))
        else:
            page_fault += 1
            if '-' in memory:
                index = memory.index('-')
            else:
                lru_page = min(last_used, key=last_used.get)
                index = memory.index(lru_page)
                del last_used[lru_page]

            memory[index] = page
            steps.append((page, memory.copy(), "FAULT"))

        last_used[page] = time

    return page_fault, steps


# ================= EKSEKUSI =================
fifo_fault, fifo_steps = fifo_page_replacement(reference_string, frames)
lru_fault, lru_steps = lru_page_replacement(reference_string, frames)

print_process_table("PROSES FIFO (First-In First-Out)", fifo_steps)
print_process_table("PROSES LRU (Least Recently Used)", lru_steps)

# ================= RINGKASAN =================
print("\nRINGKASAN HASIL AKHIR")
print("+" + "-"*12 + "+" + "-"*14 + "+" + "-"*14 + "+")
print("| Algoritma | Jumlah Frame | Page Fault   |")
print("+" + "-"*12 + "+" + "-"*14 + "+" + "-"*14 + "+")
print(f"| FIFO      | {frames:^12} | {fifo_fault:^12} |")
print(f"| LRU       | {frames:^12} | {lru_fault:^12} |")
print("+" + "-"*12 + "+" + "-"*14 + "+" + "-"*14 + "+")
```
---

## Hasil Implementasi FIFO
Hasil berikut menunjukkan proses simulasi page replacement menggunakan algoritma FIFO dengan jumlah frame sebanyak 3.

FIFO mengganti halaman berdasarkan urutan masuk ke memori.

## Hasil Implementasi LRU
Berikut merupakan hasil simulasi page replacement menggunakan algoritma LRU dengan jumlah frame yang sama.
LRU mengganti halaman yang paling lama tidak digunakan.

---

## Hasil Eksekusi
Sertakan screenshot hasil percobaan atau diagram:
![Screenshot hasil](screenshots/example.png)

---

## Analisis
**Analisis Perbandingan**

   Buat tabel perbandingan seperti berikut:

   | Algoritma | Jumlah Page Fault | Keterangan |
   |:--|:--:|:--|
   | FIFO | 10 | paling lama masuk akan diganti lebih dulu |
   | LRU | 9 | paling lama tidak digunakan akan diganti lebih dulu.|

 - Jelaskan mengapa jumlah *page fault* bisa berbeda.
   Page fault terjadi saat halaman yang diminta tidak ada di memori, sehingga harus diganti. Jumlahnya berbeda antara FIFO
   dan LRU karena cara mereka memilih halaman yang diganti:
   FIFO (First-In First-Out): Ganti halaman yang paling lama masuk memori, tanpa peduli kapan terakhir digunakan. Dalam
   simulasi dengan referensi [7, 0, 1, 2, 0, 3, 0, 4, 2, 3, 0, 3, 2] dan 3 frame, ada 6 page fault. Ini karena FIFO
   mempertahankan halaman awal yang sering diakses, cocok dengan pola ulang-alik halaman seperti 0 dan 3.

   LRU (Least Recently Used): Ganti halaman yang paling lama tidak digunakan. Di simulasi sama, ada 9 page fault. LRU sering
   mengganti halaman yang baru saja dipakai jika ada yang lebih lama tidak aktif, sehingga fault lebih banyak saat halaman
   lama muncul lagi. Perbedaan ini tergantung pola akses: FIFO baik untuk pola berulang sederhana, LRU untuk pola yang lebih
   acak.
   
- Analisis algoritma mana yang lebih efisien dan alasannya.
  Dalam simulasi ini, FIFO lebih efisien karena page fault lebih sedikit (6 vs. 9), mengurangi operasi I/O mahal. Alasannya:
  pola referensi membuat FIFO mempertahankan halaman relevan lebih lama. Secara umum, LRU lebih efisien karena
  mempertimbangkan penggunaan terbaru (prinsip lokalitas), tapi di sini pola cocok FIFO. Jika pola lebih acak, LRU akan
  unggul.

---

## Kesimpulan
Praktikum ini mensimulasikan algoritma FIFO dan LRU dengan referensi string [7, 0, 1, 2, 0, 3, 0, 4, 2, 3, 0, 3, 2] dan 3 frame memori. Hasil utama: FIFO menghasilkan 6 page fault, LRU 9 fault. Perbedaan disebabkan kriteria penggantian FIFO berdasarkan urutan masuk (cocok pola berulang), LRU berdasarkan waktu penggunaan terakhir (lebih sensitif terhadap akses ulang). Dalam simulasi ini, FIFO lebih efisien karena fault lebih sedikit, mengurangi I/O namun, LRU umumnya lebih efisien secara umum berkat prinsip lokalitas, pemilihan algoritma harus disesuaikan dengan pola akses beban kerja untuk optimasi memori sistem operasi.

---

## Quiz
1. Apa perbedaan utama FIFO dan LRU?
   - **Jawaban** :
     **FIFO**: Menggantikan halaman yang paling lama masuk ke dalam memori, tanpa mempertimbangkan kapan halaman tersebut
     terakhir digunakan. Algoritma ini sederhana dan mudah diimplementasikan, tetapi dapat kurang efisien karena tidak
     memanfaatkan pola akses penggunaan halaman. Sedangkan
     **LRU**: Menggantikan halaman yang paling lama tidak digunakan (berdasarkan waktu akses terakhir). Algoritma ini lebih
     mempertimbangkan prinsip lokalitas temporal, di mana halaman yang baru saja diakses cenderung digunakan
     lagi, sehingga sering menghasilkan lebih sedikit page fault.
3. Mengapa FIFO dapat menghasilkan *Belady’s Anomaly*?
   - **jawaban** :
     **Cara Kerja FIFO**: FIFO mengganti halaman yang paling lama masuk memori, tanpa peduli kapan halaman itu terakhir
     digunakan atau apakah akan dipakai lagi. Ini seperti antrian sederhana: yang masuk duluan keluar duluan.
     **Masalah Saat Frame Ditambah**: Dengan frame lebih banyak, halaman lama yang masih berguna tetap diganti kalau antrian
     FIFO penuh. Akibatnya, halaman yang baru saja diganti muncul lagi, dan sistem harus membacanya ulang dari disk,
     sehingga fault bertambah.
5. Mengapa LRU umumnya menghasilkan performa lebih baik dibanding FIFO?
   - **Jawaban** : **LRU lebih unggul dan lebih andal dibandingkan FIFO** karena mampu menyesuaikan diri dengan pola akses
     memori program dan meminimalkan *page fault* dalam sebagian besar kasus. Meskipun implementasi LRU lebih kompleks,
     efisiensi yang diperoleh terutama dalam mengurangi operasi I/O disk menjadikannya pilihan yang lebih baik untuk sistem
     operasi modern. FIFO tetap dapat digunakan pada skenario sederhana, tetapi kurang optimal untuk beban kerja yang
     dinamis dan kompleks.


---

## Refleksi Diri
Tuliskan secara singkat:
- Apa bagian yang paling menantang minggu ini?
  - memahami perbedaan FIFO dan LRU serta memahami mengenai Belady’s Anomaly
- Bagaimana cara Anda mengatasinya?
  - cara mengatasinya dengan visualisasi dengan tabel dan menggunakan analogi sederhana untuk memahami belady's anomaly.
    serta mencari referensi dibeberapa website untuk memahami lebih dalam tentang FIFO dan LRU


---

**Credit:**  
_Template laporan praktikum Sistem Operasi (SO-202501) – Universitas Putra Bangsa_
