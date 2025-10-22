
# Laporan Praktikum Minggu [2]
Topik: " Struktur System Call dan Fungsi Kernel"

---

## Identitas
- **Nama**  : Rizzca Anggraeny  
- **NIM**   : 250320578 
- **Kelas** : 1DSRA

---

## Tujuan
Tuliskan tujuan praktikum minggu ini.   
- Mahasiswa mampu menjelaskan konsep dan fungsi system call dalam sistem operasi
- Mahasiswa mampu mengidentifikasi jenis-jenis system call dan fungsinya
- Mahasiswa mampu mengamati alur perpindahan mode user ke kernel saat system call terjadi
- Mahasiswa mampu menggunakan perintah linux untuk menampilkan dan menganalisis system call

---

## Dasar Teori
Tuliskan ringkasan teori (3–5 poin) yang mendasari percobaan.
1. System call adalah antarmuka yang memungkinkan program aplikasi di user space untuk meminta layanan dari kernel sistem operasi. Secara
   teoritis, system call penting untuk mengakses sumber daya terlindungi seperti hardware, memori, dan proses, sambil menjaga isolasi dan
   keamanan. Program user tidak bisa langsung mengakses kernel, mereka harus melalui system call untuk transisi aman dari user mode ke
   kernel mode (Silberschatz et al., 2018).
2. Fungsi Utama System Call
   Akses Terkontrol ke sumber daya: System call menyediakan fungsi seperti membaca file (read) atau membuat proses (fork), memastikan
   aplikasi hanya mendapat akses minimal yang diperlukan (least privilege).
   Isolasi dan Keamanan: Kernel bertindak sebagai gatekeeper, mencegah aplikasi user mengubah struktur kernel secara langsung
   (Tanenbaum & Bos, 2015).
   Abstraksi Hardware: System call menyederhanakan interaksi dengan hardware, seperti I/O, tanpa aplikasi perlu mengetahui detail
   perangkat keras.
3. Kategori System Call berdasarkan fungsi:
   - Process Control: Mengelola proses, e.g., fork(), exec(), exit().
   - File Management: Mengelola file, e.g., open(), read(), write(), close().
   - Device Management: Mengontrol perangkat, e.g., ioctl(), read/write untuk device.
   - Information Maintenance: Mengambil info sistem, e.g., getpid(), time().
4. Cara Kerja System Call
   - Transisi Mode: Program user memanggil system call melalui library (e.g., glibc). CPU memicu trap/interrupt, menyimpan konteks user,
     dan beralih ke kernel mode.
   - Eksekusi dan Verifikasi: Kernel memvalidasi parameter (e.g., alamat memori), mengeksekusi operasi, lalu kembali ke user mode.
   - Nomor System Call: Setiap system call memiliki nomor unik di syscall table, yang dipetakan ke fungsi kernel (Linux Manual Pages
     man 2 syscalls).

---

## Langkah Praktikum
1. Setup Environment
   - Gunakan Linux (Ubuntu/WSL).
   - Pastikan perintah `strace` dan man sudah terinstal.
   - Konfigurasikan Git (jika belum dilakukan di minggu sebelumnya).
2.Perintah yang di jalankan
   - **Eksperimen 1 - Analisis System Call** Jalankan perintah berikut:
      `strace ls`
    - **Eksperimen 2 - Menelusuri System Call file I/O** jalankan perintah:
      `strace -e trace=open,read,write,close cat /etc/passwd`
    - **Eksperimen 3 - Mode user vs Kernel** jalankan perintah
      `dmesg | tail -n 10`
5. File dan kode yang dibuat.  
6. Commit message yang digunakan.

---

## Kode / Perintah
Tuliskan potongan kode atau perintah utama:
```bash
strace ls
```
| Perintah | Penjelasan                                         | 
|----------|----------------------------------------------------|
|strace    | alat untuk debugging di linux, mendiagnosis error  |
| ls       | menampilkan semua system call                      |

```bash
strace -e trace=open,read,write,close cat /etc/passwd
```
| Perintah        | Penjelasan                                                        |
|-----------------|-------------------------------------------------------------------|
| strace          |tools untuk melacak system call yang dilakukan oleh suatu program  |
| -e trace=       |hanya menampilkan sistem call tertentu                             |
| open            |membuka file                                                       |
| read            |membaca file                                                       |
| write           |menulis ke output (biasanya terminal)                              |
| close           |menutup file yang sudah dibuka                                     |
| cat /etc/passwd |membaca file /etc/passwd dan menampilkan ke layar                  |

```bash        
`dmesg | tail -n 10`
```
| Perintah     | Penjelasan                                                                 |
|--------------|----------------------------------------------------------------------------|
|dmesg         | menampilkan log kernel yaitu pesan-pesan yang dicatat oleh kernel linux    |
| tail -n 10   | menampilkan 10 baris terakhir dari data yang diterima                      |
---

## Hasil Eksekusi
Sertakan screenshot hasil percobaan atau diagram:
![Screenshot hasil](screenshots/example.png)

---

## Analisis
- Jelaskan makna hasil percobaan.
  
```bash
  strace ls
```
|System Call                    | Fungsi	Tujuan/Deskripsi	                 |Hasil/Status	      |Keterangan
|-------------------------------|--------------------------------------------|--------------------|--------------------------------------|
|execve()	                      |Menjalankan program ls	                     |Berhasil (= 0)	     |Memulai proses ls                    |
|brk()	                        |Mengatur batas heap memori	                 |Alamat dikembalikan	 |Digunakan untuk alokasi memori dinami|
|mmap()	                        |Mapping memori untuk program/lib	           |Alamat dikembalikan	 |Untuk stack, library, heap, dll      |
|access("/etc/ld.so.preload")  |Mengecek preload library           	         |Gagal (ENOENT)	     |File tidak ada, normal               |
|openat("/etc/ld.so.cache")	    |Membuka cache loader library	               |berhasil	           |Mempercepat pencarian lib            |
|fstat()	                      |Mendapatkan info file descriptor	           |Berhasil	           |Digunakan setelah openat()           |
|read()	                        |Membaca isi file / data	                   |Berhasil	           |Digunakan untuk membaca ELF header,                                                                                                       config, dll                          |
|close()	                      |Menutup file descriptor	                   |Berhasil	           |Standar setelah baca file
|openat() (berulang)	          |Membuka file library (libc, libselinux, dll)|Berhasil	           |Dependency ls
|mprotect()	                    |Proteksi memori (read/write/exec)	         |Berhasil	           |Keamanan memori library
|arch_prctl()	                  |Setup thread-local storage	                 |Berhasil             |Pengaturan internal proses
|set_tid_address()	            |Inisialisasi ID thread	                     |Berhasil	           |Untuk thread manajemen
|set_robust_list()	            |Registrasi struktur untuk thread robust	   |Berhasil	           |Untuk manajemen thread yang aman
|rseq()	                        |Setup struktur untuk optimisasi CPU	       |Berhasil	           |Digunakan oleh glibc
|prlimit64()	                  |Cek batas stack	                           |Berhasil	           |Limitasi resource proses
|munmap()	                      |Melepas mapping memori	                     |Berhasil	           |Pembersihan memori
|statfs("/selinux")	            |Cek keberadaan SELinux	                     |ENOENT	             |SELinux tidak aktif di sistem
|getrandom()	                  |Ambil data acak dari kernel	               |Berhasil             |(8 byte)	Untuk seed atau ASLR
|access("/etc/selinux/config")	|Cek config SELinux	                         |ENOENT	             |Konfigurasi tidak ditemukan
|openat("/proc/filesystems")	  |Cek sistem file yang tersedia	             |Berhasil	           |Digunakan oleh ls secara tidak                                                                                                           langsung
|futex()	                      |Sinkronisasi thread	                       |Berhasil	           |Tidak ada thread dibangunkan
|readlink("/proc/self/exe")	    |Dapatkan path executable saat ini	         |Berhasil	           |Internal dari glibc
|openat(".", ...)	              |Membuka direktori saat ini	                 |Berhasil	           |ls akan membaca isi direktori
|getdents64()	                  |Membaca isi direktori	                     |Berhasil	           |Mengambil daftar file untuk                                                                                                               ditampilkan
|write()	                      |Menulis hasil ke stdout	                   |Berhasil	           |Output ls ke layar
|exit_group()	                  |Mengakhiri proses	                         |Berhasil	           |Proses ls selesai


```bash
strace -e trace=open,read,write,close cat /etc/passwd
```

| System Call	 |Fungsi	Argumen                       | File Terkait	                 |Hasil	          |Keterangan
|--------------|--------------------------------------|--------------------------------|----------------|-------------------------------|
|open()	       |Membuka file library atau konfigurasi	|(Beragam, default lib & locale  |Berhasil (= 3)	|File deskriptor 3 digunakan
|read()	       |Membaca data binary                   |(mis. ELF)	fd = 3	             |= 832 byte	    |Membaca metadata binary
|close()	     |Menutup file setelah dibaca	          |fd = 3	                         |= 0	            |Penutupan file ELF / |lib	|read()	       |Membaca file locale alias	            |fd = 3	                         |= 2996 byte	    |Locale config file
|read()	       |Membaca ulang file yang sama	        |fd = 3	                         |= 0	            |Menandakan EOF	
|close()	     |Menutup file	                        |fd = 3	                         |= 0	            |File locale alias
|close()	     |Penutupan descriptor library lain	    |fd = 3	                         |= 0	            |File lib/locale ditutup
|read()	       |Membaca isi /etc/passwd	              |fd = 3	                         |= 1444 byte	    |Isi file berhasil dibaca
|write()	     |Menampilkan isi ke stdout	            |fd = 1 (stdout)	               | = 1444 byte	  |Isi /etc/passwd ditampilkan
|close()	     |Menutup file /etc/passwd	            |fd = 3	                         |= 0	            |Akhir operasi pembacaan

```bash
dmesg | tail -n 10
```

Timestamp (detik)	  |Komponen	        |Pesan Kernel	                               |Keterangan
|-------------------|-----------------|--------------------------------------------|-----------------------------------------|
[ 8.534065 ]	      |FS-Cache	        |N-key=[10] '34323934393338313434	           |Pesan internal dari sistem cache file
[ 49.195291 ]	      |Hyper-V Balloon	|Max. dynamic memory size: 3184 MB	         |Batas maksimum memori dinamis (WSL2)
[ 65.856038 ]	      |WSL2	            |Performing memory compaction                |Proses penghematan memori oleh WSL2
[ 186.885772 ]	    |WSL2	            |Performing memory compaction	               |Berulang, menunjukkan manajemen memori
[ 520.231929 ]	    |TCP              |Driver	Driver has suspect GRO implementation|Driver eth0 mungkin tidak optimal
[ 547.895374 ]	    |WSL2	            |Performing memory compaction	               |Kompaksi memori kembali terjadi

> Seluruh percobaan menggunakan strace terhadap perintah ls dan cat /etc/passwd menunjukkan bahwa eksekusi program berjalan normal melalui serangkaian system call inti seperti execve, open, read, write, dan close tanpa error, serta hasil dmesg mengonfirmasi tidak ada gangguan kernel yang signifikan selain aktivitas rutin WSL2 terkait manajemen memori dan peringatan driver jaringan.

- Hubungkan hasil dengan teori (fungsi kernel, system call, arsitektur OS).
Hasil percobaan menggunakan strace menunjukkan bahwa setiap perintah yang dijalankan di terminal Linux, seperti ls dan cat /etc/passwd, akan melewati serangkaian system call yang merupakan jembatan komunikasi antara aplikasi di user space dengan kernel space. System call yang terlihat dalam percobaan — seperti execve, open, read, write, dan close — adalah contoh nyata bagaimana proses pengguna meminta layanan kepada kernel, seperti membuka file, membaca isi file, menuliskannya ke layar, dan mengelola memori. Hal ini mendukung teori bahwa fungsi utama kernel adalah mengatur manajemen proses, manajemen memori, sistem file, dan perangkat input/output secara langsung.
Hasil dmesg yang menampilkan aktivitas rutin seperti kompaksi memori oleh WSL2 dan peringatan terkait driver jaringan juga memperkuat peran kernel dalam pengelolaan memori dan perangkat keras, sesuai dengan teori arsitektur OS di mana kernel memiliki kontrol penuh terhadap hardware. Selain itu, percobaan memperlihatkan adanya pemisahan antara ruang pengguna (user space) dan ruang kernel (kernel space), yang merupakan bagian dari arsitektur sistem operasi modern yang bertujuan untuk menjamin keamanan, stabilitas, dan efisiensi sistem.
- Apa perbedaan hasil di lingkungan OS berbeda (Linux vs Windows)?  
Perbedaan hasil di lingkungan Linux dan Windows terletak pada cara sistem operasi menangani system call: Linux menggunakan strace untuk menampilkan system call secara langsung seperti open, read, dan write, karena Linux bersifat terbuka dan transparan terhadap aktivitas kernel. Sementara itu, Windows menggunakan API seperti CreateFile dan ReadFile yang tidak langsung mengakses kernel, sehingga membutuhkan alat khusus seperti Process Monitor untuk melihat aktivitas serupa. Selain itu, log kernel seperti dmesg di Linux tidak tersedia secara langsung di Windows, yang mengandalkan Event Viewer untuk pemantauan sistem.
---

## Kesimpulan
Tuliskan 2–3 poin kesimpulan dari praktikum ini.
1. System call berperan sebagai penghubung aman antara user mode dan kernel mode.
   System call menjadi satu-satunya mekanisme resmi bagi aplikasi untuk meminta layanan dari kernel, sehingga menjaga agar aplikasi tidak
   langsung mengakses instruksi berhak istimewa yang dapat mengancam stabilitas dan keamanan sistem operasi.
3. System call memastikan pengendalian hak akses dan validasi operasi.
   Ketika aplikasi melakukan system call, OS akan melakukan pengecekan izin dan validasi parameter sebelum mengeksekusi operasi yang
   diminta. Proses ini mencegah aplikasi jahat melakukan tindakan berbahaya, seperti mengakses data sensitif atau merusak sistem.
5. Keamanan system call dijaga melalui kombinasi mekanisme perangkat keras dan perangkat lunak.
   Perangkat keras menyediakan mode privilege dan mekanisme trap, sedangkan perangkat lunak (kernel) menangani pemeriksaan, penyimpanan
   state, dan eksekusi layanan secara aman. Kombinasi ini menciptakan batas keamanan yang ketat antara aplikasi dan kernel.
---
**Tugas**
System call merupakan antarmuka krusial antara aplikasi di user space dan kernel sistem operasi, yang sangat penting untuk menjaga keamanan OS. Tanpa system call, aplikasi pengguna bisa langsung mengakses sumber daya kernel seperti memori, hardware, atau proses sistem, membuka peluang eksploitasi oleh malware. Misalnya, kode berbahaya bisa mengubah struktur data kernel atau mengakses perangkat keras secara bebas, menyebabkan kerusakan sistem, kebocoran data, atau serangan privilege escalation. System call menerapkan prinsip isolasi dan least privilege, di mana aplikasi hanya diberi akses minimal yang diperlukan. Di OS seperti Linux, kernel bertindak sebagai gatekeeper, memverifikasi setiap permintaan sebelum mengeksekusi operasi sensitif. Ini mengurangi risiko ancaman seperti buffer overflow atau denial-of-service, karena aplikasi jahat tidak bisa mengganggu stabilitas kernel secara langsung. Selain itu, system call mendukung audit trail dan logging, memungkinkan deteksi anomali atau intrusi. Tanpa kontrol ini, OS rentan terhadap rootkit atau serangan jaringan, karena dunia user dan kernel tidak terpisah dengan baik. Dengan demikian, system call bukan sekadar mekanisme fungsional, melainkan lapisan keamanan utama yang memastikan integritas OS.

### Memastikan Transisi User-Kernel yang Aman

Transisi dari mode user ke mode kernel melalui system call dirancang untuk aman guna mencegah eksploitasi. OS menggunakan hardware dan software untuk ini. Secara hardware, arsitektur seperti x86 atau ARM memanfaatkan ring protection model, di mana kernel berjalan di ring 0 (hak tertinggi), sementara aplikasi user di ring 3. Transisi dipicu oleh instruksi seperti syscall (di x86-64) atau svc (di ARM), memicu trap atau interrupt software (OSTEP, 2018). Saat aplikasi memanggil system call, CPU menyimpan konteks user (register, stack pointer) dalam struktur kernel yang aman, lalu beralih ke mode kernel.

Kernel memvalidasi parameter—misalnya, memeriksa alamat memori valid dan tidak melampaui batas user space—untuk menghindari serangan seperti null pointer dereference atau buffer overflow. Jika tidak sah, kernel menolak dan mengembalikan error. Eksekusi dilakukan dengan hak penuh, tapi hanya untuk operasi diizinkan. Setelah selesai, OS melakukan context switch kembali ke user mode, memulihkan konteks. Di Linux, syscall table memetakan nomor panggilan ke fungsi kernel, dan teknik seperti copy_from_user() mentransfer data aman, mencegah manipulasi memori. Jika kesalahan terjadi, OS mengirim signal atau menghentikan aplikasi, bukan kernel, menjaga stabilitas. Mekanisme ini memastikan transisi satu arah dan terkontrol, meminimalkan risiko dari kode malicious.

### Contoh System Call yang Sering Digunakan di Linux

Di Linux, system call sering digunakan untuk operasi dasar dan dipanggil melalui library seperti glibc. Beberapa contoh umum meliputi:

- *open() dan close()*: Membuka dan menutup file, dengan kernel memverifikasi izin akses (misalnya, read-only) untuk mencegah akses
  ilegal
- *read() dan write()*: Membaca dari atau menulis ke file/descriptor, sambil memastikan batas memori dan hak akses, menghindari overflow 
- *fork() dan exec()*: fork() membuat proses anak untuk multitasking, sementara exec() mengganti image proses, menjaga isolasi antar
  proses.
- *socket() dan bind()*: Untuk operasi jaringan, seperti membuat socket dan mengikat ke port, dengan kernel menangani protokol dan
  firewall (man 2 socket).
- *chmod() dan chown()*: Mengubah izin atau pemilik file, hanya untuk root atau pemilik, mencegah eskalasi privilege.

System call ini, dengan ratusan varian, penting untuk keamanan karena setiap panggilan diawasi kernel, memastikan akses terkontrol
Secara keseluruhan, system call membentuk fondasi keamanan OS dengan mengatur interaksi user-kernel, memastikan transisi aman melalui verifikasi ketat dan menyediakan fungsi esensial yang terlindungi. Tanpa mereka, OS seperti Linux tidak bisa menjaga integritas di lingkungan multi-user.


## Quiz
1. Apa fungsi utama system call dalam sistem operasi   
**Jawaban: Fungsi utamanya adalah menyediakan akses terkontrol ke sumber daya sistem seperti hardware, memori, dan proses,                  sambil menjaga keamanan dan isolasi antara aplikasi user dan kernel. Tanpa system call, aplikasi tidak bisa melakukan operasi             dasar seperti membaca file atau membuat proses baru, karena kernel melindungi sumber daya ini dari akses langsung. Ini                    mencegah eksploitasi dan memastikan stabilitas OS**  
2. Sebutkan 4 kategori system call yang umum digunakan  
   **Jawaban:
- Process Control*: Mengelola proses, seperti membuat proses baru (fork), mengganti program (exec), atau mengakhiri proses (exit). Ini
  mendukung multitasking dan isolasi antar-proses.
- File Management*: Mengelola file dan direktori, seperti membuka file (open), membaca/menulis (read/write), atau menutup file (close).
  Ini memastikan akses terkontrol ke sistem file.
- Device Management*: Mengelola perangkat keras, seperti mengontrol I/O (ioctl) atau mengakses perangkat melalui read/write. Ini
  memungkinkan interaksi dengan hardware sambil menjaga keamanan.
- information Maintenance*: Mengambil informasi sistem, seperti mendapatkan ID proses (getpid), waktu sistem (time), atau statistik
  sistem. Ini membantu aplikasi memantau lingkungan tanpa akses langsung ke kernel**  
3. Mengapa system call tidak bisa dipanggil langsung oleh user program ?
 **Jawaban: System call tidak bisa dipanggil langsung oleh user program karena program tersebut berjalan di user mode, yang memiliki hak
  akses terbatas untuk mencegah kerusakan sistem. Kernel beroperasi di privileged mode (seperti ring 0 di x86), dan transisi ke mode ini
  memerlukan instruksi khusus seperti syscall atau trap, yang dipicu oleh interrupt software. Jika user program mencoba mengakses kernel
  secara langsung, CPU akan menolaknya karena pelanggaran privilege, mencegah serangan seperti privilege escalation. Mekanisme ini
  memastikan isolasi dan keamanan**  

---

## Refleksi Diri
Tuliskan secara singkat:
- Apa bagian yang paling menantang minggu ini?
  menganalisis hasil dari praktikum yang telah dilakukan mengenai system call 
- Bagaimana cara Anda mengatasinya?
  mecari referensi dari web yang disediakan di github dan refensi web lainnya di google 

---

**Credit:**  
_Template laporan praktikum Sistem Operasi (SO-202501) – Universitas Putra Bangsa_
