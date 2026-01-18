
# Laporan Praktikum Minggu [X]
Topik:Docker – Resource Limit (CPU & Memori)

---

## Identitas
- **Nama**  : Rizzca Anggraeny
- **NIM**   : 250320578
- **Kelas** : 1DSRA

---

## A. Deskripsi Singkat
Pada praktikum minggu ini, mahasiswa mempelajari konsep **containerization** menggunakan Docker, serta bagaimana sistem operasi membatasi pemakaian sumber daya proses melalui mekanisme isolasi dan kontrol resource (mis. *cgroups* pada Linux).

Fokus praktikum adalah:
1. Membuat **Dockerfile sederhana** untuk menjalankan aplikasi/skrip.
2. Menjalankan container dengan **pembatasan resource** (CPU dan memori).
3. Mengamati dampak pembatasan resource melalui output program dan monitoring sederhana.

---

## B. Tujuan
Setelah menyelesaikan tugas ini, mahasiswa mampu:
1. Menulis Dockerfile sederhana untuk sebuah aplikasi/skrip.
2. Membangun image dan menjalankan container.
3. Menjalankan container dengan pembatasan **CPU** dan **memori**.
4. Mengamati dan menjelaskan perbedaan eksekusi container dengan dan tanpa limit resource.
5. Menyusun laporan praktikum secara runtut dan sistematis.
---

## Dasar Teori
1. Pengaturan Sumber Daya dengan cgroups, docker memanfaatkan **Linux cgroups** untuk membatasi dan memonitor penggunaan
   CPU serta memori tiap container. Contohnya, memori dapat dibatasi dengan `-m 512m` dan CPU dengan `--cpus="1.5"`.
2. Isolasi Container melalui cgroups dan namespaces, isolasi container dicapai dengan kombinasi **cgroups** (pengendali
   sumber daya) dan **namespaces** (pemisah proses, jaringan, filesystem, dan user), sehingga setiap container berjalan
   dalam lingkungan terpisah.
3. Virtualisasi Berbasis Konsep Sistem Operasi, Sesuai konsep **OSTEP**, Docker menggunakan *time-sharing* untuk CPU
   melalui scheduler CFS dan pembatasan memori tetap. Jika batas memori terlampaui, kernel akan menghentikan container
   menggunakan mekanisme **OOM Killer**.
4. Konfigurasi Fleksibel
   Batas sumber daya dapat diatur secara proporsional atau tetap, misalnya dengan `--cpu-shares` dan `--memory
   reservation`, baik saat container dijalankan maupun melalui antarmuka cgroups.
5. Dampak pada Stabilitas dan Efisiensi
   Pembatasan sumber daya mencegah satu container mendominasi sistem, meningkatkan stabilitas, keamanan, dan efisiensi,
   dengan overhead rendah (±1–5%) serta kepadatan container yang tinggi.

---

## C. Ketentuan Teknis
- Sistem operasi host bebas (Windows/macOS/Linux). Disarankan memakai **Docker Desktop** (atau Docker Engine di Linux).
- Program berbasis **terminal**.
- Fokus penilaian pada **keberhasilan build & run container**, **penerapan resource limit**, serta **kualitas analisis**.

Struktur folder (sesuaikan dengan template repo):
```
praktikum/week13-docker-resource-limit/
├─ code/
│  ├─ Dockerfile
│  └─ app.*
├─ screenshots/
│  └─ hasil_limit.png
└─ laporan.md
```

---

## D. Langkah Pengerjaan
1. **Persiapan Lingkungan**

   - Pastikan Docker terpasang dan berjalan.
   - Verifikasi:
     ```bash
     docker version
     docker ps
     ```

2. **Membuat Aplikasi/Skrip Uji**

   Buat program sederhana di folder `code/` (bahasa bebas) yang:
   - Melakukan komputasi berulang (untuk mengamati limit CPU), dan/atau
   - Mengalokasikan memori bertahap (untuk mengamati limit memori).

3. **Membuat Dockerfile**

   - Tulis `Dockerfile` untuk menjalankan program uji.
   - Build image:
     ```bash
     docker build -t week13-resource-limit .
     ```

4. **Menjalankan Container Tanpa Limit**

   - Jalankan container normal:
     ```bash
     docker run --rm week13-resource-limit
     ```
   - Catat output/hasil pengamatan.

5. **Menjalankan Container Dengan Limit Resource**

   Jalankan container dengan batasan resource (contoh):
   ```bash
   docker run --rm --cpus="0.5" --memory="256m" week13-resource-limit
   ```
   Catat perubahan perilaku program (mis. lebih lambat, error saat memori tidak cukup, dll.).

6. **Monitoring Sederhana**

   - Jalankan container (tanpa `--rm` jika perlu) dan amati penggunaan resource:
     ```bash
     docker stats
     ```
   - Ambil screenshot output eksekusi dan/atau `docker stats`.

7. **Commit & Push**

   ```bash
   git add .
   git commit -m "Minggu 13 - Docker Resource Limit"
   git push origin main
   ```

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
1. Mengapa container perlu dibatasi CPU dan memori?
 **jawaban:**
   - Container di Docker perlu dibatasi CPU dan memori agar tidak menggunakan sumber daya host secara berlebihan, yang
     bisa menyebabkan sistem lambat atau bahkan berhenti berfungsi. Tanpa batasan, sebuah container mungkin mengambil
     semua daya pemrosesan atau ruang penyimpanan sementara, sehingga mengganggu container lain atau proses di komputer
     utama, dan ini tidak adil dalam lingkungan bersama seperti server cloud. Selain itu, pembatasan ini meningkatkan
     keamanan dengan mencegah container saling mengganggu atau dieksploitasi, serta membantu mengoptimalkan performa untuk
     jenis aplikasi tertentu, seperti yang butuh banyak CPU untuk perhitungan atau memori untuk penyimpanan data. Docker
     menerapkan ini melalui teknologi cgroups dari kernel Linux, memungkinkan kontrol tepat seperti membatasi jumlah inti
     CPU atau ukuran RAM.
3. Apa perbedaan VM dan container dalam konteks isolasi resource?
 **jawaban:**
   - Virtual Machines (VM) dan Containers berbeda dalam pendekatan isolasi sumber daya seperti CPU, memori, dan
     penyimpanan. VM memanfaatkan hypervisor (misalnya, VMware atau KVM) untuk menciptakan mesin virtual utuh, termasuk
     sistem operasi (OS) sendiri, kernel, dan hardware maya. Hal ini menghasilkan isolasi total karena setiap VM
     beroperasi seperti komputer fisik terpisah, sehingga sumber daya tidak saling berbagi langsung dengan host contohnya,
     VM satu tidak bisa menyentuh memori VM lain tanpa akses khusus. Akan tetapi, ini membuat VM lebih berat, dengan
     overhead tinggi (seperti RAM untuk OS virtual) dan waktu startup yang lebih lama.

   - Sebaliknya, Containers (seperti yang ada di Docker) menggunakan kernel OS host bersama dan teknologi Linux seperti
     cgroups (untuk membatasi CPU/memori) serta namespaces (untuk memisahkan proses, jaringan, dan sistem file). Isolasi
     di sini lebih sederhana dan hemat, karena container hanya menyimpan aplikasi dan kebutuhannya tanpa OS lengkap,
     sehingga startup cepat dan konsumsi sumber daya lebih rendah. Namun, isolasi tidak sekuat VM—container bisa rentan
     terhadap kelemahan kernel host, dan sumber daya seperti CPU/memori dibagikan lebih erat, meskipun tetap bisa
     dikontrol dengan opsi seperti --cpus atau --memory.

5. Apa dampak limit memori terhadap aplikasi yang boros memori?
 **jawaban:**
- Dampak Positif: Membantu mencegah aplikasi mengonsumsi seluruh memori host, yang bisa menyebabkan sistem crash atau out
  of-memory (OOM) kill. Ini memaksa aplikasi untuk lebih efisien, seperti menggunakan teknik optimasi memori (misalnya,
  garbage collection di Java atau paging), dan memastikan fairness di lingkungan multi-container. Di cloud, ini juga
  mengurangi biaya karena penggunaan sumber daya terkontrol.

- Dampak Negatif: Jika aplikasi melebihi batas, Docker bisa menghentikan kontainer secara otomatis (OOM killer),
  menyebabkan downtime atau kehilangan data jika tidak ada mekanisme recovery. Performa aplikasi mungkin menurun karena
  thrashing (memori swap berlebihan) atau error seperti "Cannot allocate memory", terutama jika aplikasi tidak dirancang
  untuk batasan ketat. Ini bisa membuat aplikasi lambat atau tidak stabil.

---

## Refleksi Diri
Tuliskan secara singkat:
- Apa bagian yang paling menantang minggu ini?
  - menantang adalah menerapkan dan memantau batasan memori (--memory) pada container Docker untuk aplikasi boros memori,
    termasuk membedakan thrashing vs. OOM kill, serta mengintegrasikan kode uji di praktikum/week13-docker-resource
    limit/code/ dengan screenshot dan laporan.
- Bagaimana cara Anda mengatasinya?
  - Dengan membaca dokumentasi Docker dan OSTEP, bereksperimen menggunakan docker stats, membuat kode uji sederhana,
    mencatat di laporan, dan mendiskusikan dengan teman, lalu commit ke GitHub.

---

**Credit:**  
_Template laporan praktikum Sistem Operasi (SO-202501) – Universitas Putra Bangsa_
