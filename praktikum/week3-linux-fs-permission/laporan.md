
# Laporan Praktikum Minggu [3]
Topik: Manajemen File dan Permission di Linux

---

## Identitas
- **Nama**  : Rizzca Anggraeny 
- **NIM**   : 250320578
- **Kelas** : 1 DSRA

---

## Tujuan
Tuliskan tujuan praktikum minggu ini.  
- Mahasiswa dapat menggunakan perintah `ls`, `pwd`, `cd`, `cat` untuk navigasi file dan direktori.
- Mahasiswa dapat menggunakan `chmod` dan `chown` untuk manajemen hak akses file.
- Mahasiswa dapat menjelaskan hasil output dari perintah linux dasar.

---

## Dasar Teori
Tuliskan ringkasan teori (3–5 poin) yang mendasari percobaan.
1. *Konsep Fundamental Sistem File*: Sistem file Linux berbentuk hierarki pohon yang berawal dari root (/), memanfaatkan inode untuk
   menyimpan informasi metadata file seperti kapasitas dan posisi, serta mencakup berbagai tipe file termasuk biasa, folder, dan tautan
   simbolik. Proses mounting memfasilitasi penggabungan sistem file luar ke dalam struktur utama (Silberschatz et al., 2018, Bab 11;
   Tanenbaum & Bos, 2015, Bab 4; OSTEP, Bab 39).

2. *Sistem Izin Akses*: Izin mengatur kontrol terhadap file berdasarkan pemilik (user), kelompok (group), dan pihak lain (others), dengan
   elemen baca (r), tulis (w), dan eksekusi (x). Ditampilkan melalui notasi oktal (0-7) atau simbolik, contohnya 755 (rwxr-xr-x), serta
   bisa diperkaya dengan izin khusus seperti setuid untuk meningkatkan perlindungan (Silberschatz et al., 2018, Bab 14; Tanenbaum & Bos,
   2015, Bab 9; man chmod; OSTEP, Bab 39).

3. *Perintah ls untuk Menampilkan Daftar File*: Instruksi ls memperlihatkan konten folder serta rincian izin, misalnya dengan pilihan -l
   yang mengungkap format -rw-r--r-- guna memahami tingkat akses file. Hal ini krusial untuk eksplorasi dan validasi arsitektur file (man
   ls; Silberschatz et al., 2018, Bab 11).

4. *Perintah chmod untuk Memodifikasi Izin*: chmod menyesuaikan izin file melalui format simbolik (misal, u+x) atau oktal (misal, 755),
   yang memungkinkan penyesuaian akses bagi user, group, atau others. Ini bertujuan mencegah intrusi ilegal dan mendukung latihan praktis
   terkait keamanan (man chmod; OSTEP, Bab 39; Tanenbaum & Bos, 2015, Bab 9).

5. *Perintah chown serta Penerapan dalam Praktikum*: chown mengganti pemilik dan kelompok file (contoh, chown user:group file), umumnya
    dilakukan oleh root untuk pengendalian kepemilikan. Di sesi praktikum, peserta berlatih navigasi, konfigurasi izin, serta
   penyelesaian masalah akses menggunakan akun berbeda, guna menjaga keutuhan data (man chown; Silberschatz et al., 2018, Bab 14; OSTEP,
   Bab 39).
---

## Langkah Praktikum
1. ## Setup Environment
   - Gunakan Linux (Ubuntu/WSL)
   - Pastikan folder kerja berada di dalam direktori repositori Git praktikum:
     ```bash
     praktikum/week3-linux-fs-permission/
     ```
2. ## Eksperimen 1 – Navigasi Sistem File Jalankan perintah berikut:
   ```bash
   pwd
   ls -l
   cd /tmp
   ls -a
   ```
4. ## Eksperimen 2 – Membaca File Jalankan perintah:
   ```bash
   cat /etc/passwd | head -n 5
   ```
6. ## Eksperimen 3 – Permission & Ownership Buat file baru:
   ```bash
   echo "Hello <RIZZCA ANGGRAENY><250320578>" > percobaan.txt
   ls -l percobaan.txt
   chmod 600 percobaan.txt
   ls -l percobaan.txt
   ```
7. File dan kode yang dibuat.  
8. Commit message yang digunakan.pwd

---

## Kode / Perintah
Tuliskan potongan kode atau perintah utama:
```bash
- pwd
 ls -l
 cd /tmp
 ls -a
- cat /etc/passwd | head -n 5
- echo "Hello <NAME><NIM>" > percobaan.txt
  ls -l percobaan.txt
  chmod 600 percobaan.txt
  ls -l percobaan.txt
- sudo chown root percobaan.txt
  ls -l percobaan.txt
```

---

## Hasil Eksekusi
Sertakan screenshot hasil percobaan atau diagram:
![Screenshot hasil](screenshots/example.png)

```bash
 pwd
 ls -l
 cd /tmp
 ls -a
```
| Baris Output                                                                     | Permission | Penjelasan |
|----------------------------------------------------------------------------------|------------|------------|
|snap-private-tap                                                                  | drwx------ | Direktori privat untuk Snap (sistem paket aplikasi di Linux). Permission hanya user (root) yang bisa read/write/execute, mencegah akses oleh group/others. Ini penting untuk isolasi aplikasi, menghindari konflik data.
| systemd-private-118a9a8c69b94430bb7d14188124afa7-polkit.service-1uYxvQ           | drwx------ | Direktori privat systemd untuk layanan polkit (policy kit, mengelola otentikasi). UUID unik memastikan isolasi per proses. Permission ketat untuk keamanan; group/others tidak bisa akses
| systemd-private-118a9a5c69694430bb7d14188124afa7-systemd-Logind.service-BefkAu   | drwx------ | Direktori privat untuk systemd-logind (manajer sesi login). Permission root-only mencegah manipulasi sesi oleh user lain. Dalam praktikum, ini menunjukkan bagaimana systemd mengisolasi data sensitif di /tmp. |
| systemd-private-118a9a8c69694438bb7614188124afa7-systemd-resolved.service-xlLiFE | drwx------ | Direktori privat untuk systemd-resolved (resolver DNS). Permission ketat melindungi cache DNS dari akses tidak sah. Observasi: UUID berbeda dari yang lain, menunjukkan instance terpisah untuk setiap layanan. |
| systemd-private-118a9a8c69694438bb7d14188124afa7-systemd-timesyncd.service-Ix5101| drwx------ | Direktori privat untuk systemd-timesyncd (sinkronisasi waktu NTP). Permission root-only memastikan integritas waktu sistem. Dalam observasi praktikum, ini menekankan pentingnya permission untuk layanan jaringan. |
|snap-private-top                                                                  | drwx------ | Direktori privat Snap lainnya, mungkin untuk top-level aplikasi. Ukuran lebih besar (4896 bytes) dibanding yang lain, menunjukkan lebih banyak data. Permission sama: akses terbatas pada root. Observasi: Mirip dengan item 1, tapi ukuran berbeda – mungkin ada subfile.
| systemd-private-118a9a8c69694438bb7d14188124afa7-ssl-pro.service-Xewyxt          |drwx------  | Direktori privat untuk layanan SSL proxy systemd. Permission ketat melindungi koneksi aman. Observasi: Nama "ssl-pro" menunjukkan fokus pada keamanan; dalam praktikum, coba ubah permission dengan chmod untuk lihat efek. |
| percobaan.txt                                                                    | -rw-r--r-- | File teks biasa (nama "percobaan" berarti eksperimen). Permission: user (root) bisa read/write, group/others hanya read. Ini lebih permisif dibanding direktori lain. Observasi: Kemungkinan file uji praktikum; bisa dibaca oleh semua user, tapi edit hanya root | 
| systemd-private-118a9a8c69b94u30bb7d14188124afa7-polkit.service-iuYxvQ           | drwx------ | Variasi dari item 2 (polkit), dengan UUID sedikit berbeda. Permission sama: root-only. Observasi: Duplikasi nama layanan menunjukkan multiple instance; tanggal sama, menunjukkan pembuatan bersamaan. |
| systemd-private-118a9aBc69b938bb7d14188124afa7-systemd-Logind.service-BefkAu     | drwx------ | Variasi dari item 3 (logind), dengan UUID berbeda. Permission ketat. Observasi: Huruf kapital di UUID (e.g., "Bc") mungkin typo dalam output; fokus pada isolasi sesi.|
| systemd-private-115a9a8c69694436bb7d14188124afa7-systemd-resolved.service-xBLIFE | drwx------| Variasi dari item 4 (resolved), UUID berbeda. Permission root-only. Observasi: UUID dimulai dengan "115" bukan "118", menunjukkan versi atau instance berbeda|

```bash
cat /etc/passwd | head -n 5
```

| No | Baris Output                                    | Username | UID | GID | Home Directory | Shell     | Penjelasan |
|----|-------------------------------------------------|----------|-----|-----|----------------|-----------|------------|
| 1  |root:x:0:0:root:/root:/bin/bash                  | root     | 0   | 0   | /root          | /bin/bash | User superuser dengan UID 0 (tertinggi). Memiliki akses penuh ke sistem. Shell bash memungkinkan interaksi penuh. Dalam praktikum, root bisa mengubah permission file apa saja tanpa batas. |
| 2  | daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin | daemon   | 1   | 1   | /usr/sbin      | /usr/sbin/nologin | User untuk proses daemon (layanan latar belakang). UID 1 adalah standar. Shell nologin mencegah login interaktif, hanya untuk proses otomatis. Home di /usr/sbin, bukan direktori pribadi. |
| 3  | bin:x:2:2:bin:/bin:/usr/sbin/nologin            | bin      | 2   | 2   | /bin           | /usr/sbin/nologin | User untuk binary sistem (perintah dasar). UID 2. Shell nologin untuk keamanan; home di /bin (tempat executable). Dalam praktikum, ini menunjukkan user sistem tidak untuk login manusia. |
| 4  | sys:x:3:3:sys:/dev:/usr/sbin/nologin            | sys      | 3   | 3   | /dev           | /usr/sbin/nologin | User untuk sistem device. UID 3. Home di /dev (device files). Shell nologin; fokus pada akses hardware. Observasi: GID sama dengan UID, umum untuk user sistem. |
| 5  | sync:x:4:65534:sync:/bin:/bin/sync              | sync     | 4   | 65534 | /bin         | /bin/sync         | User untuk perintah sync (sinkronisasi disk). UID 4. GID 65534 (nobody group). Shell adalah /bin/sync (executable, bukan shell interaktif). Home di /bin; jarang digunakan untuk login. |


```bash
echo "Hello <NAME><NIM>" > percobaan.txt
  ls -l percobaan.txt
  chmod 600 percobaan.txt
  ls -l percobaan.txt
- sudo chown root percobaan.txt
  ls -l percobaan.txt
```

|Perintah yang dijalankan |Output yang dihasilkan| Penjelasan observasi| Analisis sebelum dan sesudah chmod|
|-------------------------|----------------------|---------------------|-----------------------------------|
|echo "Hello <RIZZCA ANGGRAENY><250320578>" > percobaan.txt | (Tidak ada output eksplisit; file dibuat) | Perintah ini membuat atau menimpa file percobaan.txt dengan teks "Hello <RIZZCA ANGGRAENY><250320578>". Ukuran file sekitar 36 bytes (terlihat di output selanjutnya). Sebagai root, file dibuat dengan permission default (biasanya 644 atau 664, tergantung umask). Dalam praktikum, ini menunjukkan cara membuat file teks sederhana; isi mencakup nama dan NIM, untuk identifikasi. | Tidak berlaku (belum ada cek permission sebelum chmod). |
| 1s -1 percobaan.txt (kemungkinan typo untuk ls -l percobaan.txt) | -rw-r--r-- 1 root root 36 Oct 29 18:44 percobaan.txt | Output menunjukkan file regular (-), permission rw-r--r-- (644: user read/write, group/others read-only), owner/group root, ukuran 36 bytes, tanggal modifikasi 18:44. Observasi: Permission awal memungkinkan pembacaan oleh semua, tapi hanya root yang bisa edit. Ini standar untuk file baru; dalam praktikum, gunakan ls -l untuk verifikasi permission sebelum/sesudah perubahan. | *Sebelum chmod*: Permission 644 (rw-r--r--), memungkinkan group dan others untuk read. Ini kurang aman untuk file sensitif, karena user lain bisa baca isi tanpa edit. |
| chmod 600 percobaan.txt | (Tidak ada output; permission diubah) | Perintah mengubah permission file menjadi 600 (oktal: rw-------), artinya hanya owner (root) yang bisa read/write, group/others tidak ada akses. Dalam praktikum, ini mendemonstrasikan chmod untuk meningkatkan keamanan; coba akses file sebagai user lain – akan ditolak. Referensi: man chmod; OSTEP Bab 39. | Tidak berlaku (ini adalah perintah chmod itu sendiri, bukan hasil ls). |
| 15 -1 percobaan.txt (kemungkinan typo untuk ls -l percobaan.txt) | -rw- 1 root root 36 Oct 29 18:44 percobaan.txt | Output menunjukkan permission berubah menjadi -rw- (terpotong, tapi lengkapnya -rw------- atau 600). Ukuran dan tanggal sama. Observasi: Perubahan permission berhasil; sekarang file privat untuk root saja. Dalam praktikum, bandingkan dengan output sebelumnya untuk lihat efek chmod. | *Sesudah chmod*: Permission 600 (rw-------), hanya owner (root) yang bisa read/write. Perbedaan utama: Group dan others kehilangan akses read (dari 644 ke 600), meningkatkan keamanan. Ukuran/tanggal sama, isi tidak berubah. Dalam praktikum, ini menunjukkan chmod efektif untuk isolasi file; coba cat percobaan.txt sebagai user lain untuk konfirmasi akses ditolak. |
| sudo chown root percobaan.txt | (Tidak ada output; ownership tidak berubah karena sudah root) | Perintah menggunakan sudo (meski sudah root, tidak perlu) untuk ubah owner menjadi root. Karena file sudah milik root, tidak ada perubahan. Dalam praktikum, gunakan chown untuk transfer ownership ke user lain (e.g., chown user percobaan.txt); referensi: man chown; Silberschatz et al., Bab 14. | Tidak berlaku (tidak terkait chmod). |
| ls -1 percobaan.txt (kemungkinan typo untuk ls -l percobaan.txt) | -rw--- 1 root root 36 Oct 29 18:44 percobaan.txt | Output akhir menunjukkan permission tetap -rw--- (terpotong, tapi 600), owner/group root, ukuran 36 bytes. Observasi: Tidak ada perubahan dari langkah 5 karena ownership sudah benar. Dalam praktikum, ini menunjukkan chown tidak efektif jika owner sama; coba ubah ke user lain untuk lihat perbedaan. | Tidak berlaku (tidak terkait chmod). | 

## Analisis
1. Jelaskan makna hasil percobaan.
  - *Ilustrasi Pengendalian Akses File*: Eksperimen menunjukkan bahwa file di Linux dilengkapi izin yang bisa disesuaikan untuk membatasi
    akses. Pada awalnya, file percobaan.txt memiliki izin 644 (baca/tulis untuk pemilik, baca untuk kelompok/pihak lain), yang
    mengizinkan siapa saja membacanya. Setelah chmod 600, izin berubah menjadi pribadi (hanya pemilik yang bisa baca/tulis), menegaskan
    signifikansi izin dalam menghalangi akses tanpa izin, seperti pada data rahasia (misalnya, informasi pribadi).
  - *Fungsi Root dalam Sistem*: Semua instruksi dieksekusi sebagai root (UID 0), yang memiliki akses total. Ini menunjukkan bahwa root
    dapat memodifikasi izin dan kepemilikan tanpa batasan, namun dalam eksperimen sebenarnya, ini bisa berbahaya jika disalahgunakan.
    Eksperimen ini menggambarkan struktur hierarki pengguna di Linux, dengan root sebagai pengguna super.
  - *Dampak Modifikasi Izin dan Kepemilikan*: chmod berhasil mengubah akses dari terbuka menjadi terbatas, sedangkan chown tidak berubah
    karena file sudah dimiliki root. Ini menunjukkan bahwa izin lebih sering dimodifikasi dibanding kepemilikan, dan keduanya krusial
    untuk pengelolaan file. Ukuran file (36 byte) dan konten tetap tidak berubah, menandakan modifikasi hanya pada metadata, bukan isi.
  - *Konsekuensi untuk Eksperimen*: Eksperimen ini sesuai untuk latihan keamanan, seperti membuat file bersama kelompok atau menguji
    akses sebagai pengguna biasa. Artinya adalah menyadari bahwa izin merupakan mekanisme utama untuk menegakkan keamanan di Linux,
    mencegah pertentangan atau pelanggaran data. 
2. Hubungkan hasil dengan teori (fungsi kernel, system call, arsitektur OS).
   Praktikum ini melibatkan penggunaan perintah Linux seperti echo, ls, chmod, dan chown untuk menciptakan serta mengelola file percobaan.txt, dengan perubahan izin dari 644 menjadi 600 yang menunjukkan pengendalian akses yang kuat untuk membuat file bersifat pribadi bagi root, dan kaitan antara hasil praktikum dengan teori sistem operasi, dengan penekanan pada peran kernel, system call, serta struktur OS. Kernel Linux berfungsi sebagai inti OS yang mengatur sumber daya tingkat bawah, termasuk sistem file dan hak akses, dengan menyimpan file percobaan.txt dalam inode pada ext4 di /tmp, di mana metadata izin disimpan, dan ukuran 36 byte yang stabil menunjukkan efisiensi kernel dalam memperbarui atribut tanpa mengubah data fisik, sekaligus menegakkan izin melalui bit rwx untuk mencegah akses tidak sah, serta memberikan isolasi proses dengan memberikan hak istimewa penuh kepada root sambil mencegah penyalahgunaan; system call bertindak sebagai penghubung antara ruang pengguna dan kernel, memungkinkan aplikasi memanggil fungsi OS tanpa akses langsung, seperti open() dan write() untuk membuat file via echo, stat() untuk listing dengan ls, serta chmod() dan chown() untuk modifikasi, yang hanya dapat dilakukan oleh root, mendukung arsitektur multi-pengguna dengan verifikasi UID/GID; sementara arsitektur OS Linux yang monolitik dengan modul, memisahkan ruang kernel dan pengguna, menunjukkan lapisan keamanan melalui cincin perlindungan, di mana kernel mengelola sistem file pohon dari /tmp, inode, dan blok data untuk isolasi seperti direktori privat systemd, serta mendukung multi-tugas dengan memaksa izin untuk menghindari konflik proses; secara keseluruhan, praktikum ini secara praktis menunjukkan teori OS, dengan kernel sebagai pusat, system call sebagai jembatan, dan arsitektur sebagai kerangka yang memungkinkan pengelolaan file yang aman, menegaskan bahwa izin bukan sekadar fitur, melainkan fondasi keamanan OS, dan untuk eksplorasi lebih lanjut, pengguna dapat mencoba melacak system call dengan 'strace ls -l percobaan.txt' untuk melihat interaksi dengan kernel.
3. Apa perbedaan hasil di lingkungan OS berbeda (Linux vs Windows)?
  mengelola file dan izin di Linux, yang menggunakan perintah seperti cat /etc/passwd, echo, ls -l, chmod, dan chown untuk membuat file percobaan.txt dan mengubah izinnya dari 644 ke 600 agar hanya bisa diakses oleh root, akan memberikan hasil yang sangat berbeda jika dilakukan di Windows. Hal ini terjadi karena Linux dan Windows memiliki cara kerja sistem operasi, penyimpanan file, dan aturan keamanan yang berbeda, di mana Linux menggunakan sistem sederhana berbasis angka untuk izin pada file, sedangkan Windows menggunakan daftar izin yang lebih rumit. Misalnya, di Linux, file bisa langsung diatur agar privat dengan perintah mudah, tapi di Windows memerlukan menggunakan alat khusus atau program tambahan seperti WSL untuk mencapai hal serupa. Perintah untuk melihat detail file di Linux menampilkan semua info izin dengan jelas, sementara di Windows, Anda harus menggunakan perintah lain untuk melihat izin yang lebih kompleks. Membuat file di Linux otomatis memberi izin standar, tapi di Windows, pengaturan izin tidak semudah itu. Mengubah pemilik file juga lebih sederhana di Linux, sedangkan di Windows memerlukan langkah tambahan bahkan melihat daftar pengguna sistem pun berbeda linux menggunakan file teks sederhana, sementara Windows menyimpannya dalam bentuk yang lebih aman dan sulit diakses.

---

## Kesimpulan
Tuliskan 2–3 poin kesimpulan dari praktikum ini.
- *Kontrol Akses Efektif di Linux*: Praktikum menunjukkan bagaimana perintah seperti chmod dan chown memungkinkan pengaturan izin file percobaan.txt dari 644 (baca/tulis untuk owner, baca untuk group/others) ke 600 (hanya owner yang bisa baca/tulis), menciptakan akses privat untuk root dengan prinsip keamanan berbasis permission Unix.
- *Perbedaan Signifikan dengan Windows*: Di Windows, praktikum ini tidak dapat direplikasi secara langsung karena model NTFS dengan ACL yang lebih kompleks, di mana perintah seperti chmod tidak ada dan memerlukan tools seperti icacls atau WSL, menekankan bahwa Linux lebih sederhana untuk praktikum permission, sementara Windows lebih cocok untuk skenario enterprise dengan risiko keamanan yang berbeda.
- kode permission rwxr--xr-- (754) berarti:
   - Pemilik bisa melakukan semua tindakan (membaca, menulis dan menjalankan) (rwx)
   - Grup hanya bisa membaca dan menjalankan tidak bisa menulis (r-x)
   - Pengguna lain hanya bisa membaca (r--)
- Perintah chmod berfungsi untuk mengatur izin akses pada file atau folder dengan menentukan pengguna mana yang diperbolehkan
  membaca, menulis, atau menjalankannya. Sementara itu, chown digunakan untuk mengubah kepemilikan file atau folder, yaitu menentukan
  pengguna dan grup yang menjadi pemiliknya. Dengan demikian, chmod berperan dalam pengaturan hak akses, sedangkan chown mengelola
  kepemilikan file, dan keduanya memiliki peran penting dalam keamanan serta pengelolaan sistem Linux.

---

## Quiz
1. Apa fungsi dari perintah `chmod` ?
   Jawaban : Perintah `chmod` berfungsi untuk mengatur hak akses membaca, menulis, atau menjalankan file/direktori, sehingga
   menjadi bagian penting dalam keamanan dan manajemen akses di sistem linux.  
3. Apa arti dari kode permission rwxr-xr-- ?
   Jawaban : kode permission rwxr--xr-- (754) berarti:
   - Pemilik bisa melakukan semua tindakan (membaca, menulis dan menjalankan) (rwx)
   - Grup hanya bisa membaca dan menjalankan tidak bisa menulis (r-x)
   - Pengguna lain hanya bisa membaca (r--)
    dalam bentuk angka 754 yang berarti pemilik hak akses penuh, sedangkan pengguna lain dibatasi untuk memjaga keamanan file.
5. Jelaskan perbedaan antara chown dan chmod
   Jawaban :  Perintah chmod berfungsi untuk mengatur izin akses pada file atau folder dengan menentukan pengguna mana yang diperbolehkan
   membaca, menulis, atau menjalankannya. Sementara itu, chown digunakan untuk mengubah kepemilikan file atau folder, yaitu menentukan
   pengguna dan grup yang menjadi pemiliknya. Dengan demikian, chmod berperan dalam pengaturan hak akses, sedangkan chown mengelola
   kepemilikan file, dan keduanya memiliki peran penting dalam keamanan serta pengelolaan sistem Linux.

---

## Refleksi Diri
Tuliskan secara singkat:
- Apa bagian yang paling menantang minggu ini?
  Jawaban : Bagian yang paling menantang adalah memahami cara kerja izin dan kepemilikan file menggunakan perintah chmod dan chown.
  Kesulitannya terletak pada menafsirkan kode izin seperti 644, 600, atau 755 serta membedakan antara pengaturan hak akses dan
  kepemilikan file.
- Bagaimana cara Anda mengatasinya?
  Jawaban : Untuk mengatasinya, langkah pertama adalah memahami dulu arti izin dan kepemilikan file, lalu berlatih menggunakan perintah
  chmod dan chown sedikit demi sedikit pada beberapa file atau folder lalu menggunakan perintah ls -l untuk melihat apakah perubahan izin
  sudah benar, dan mencatat hasilnya agar mudah diingat. Selain itu, membaca panduan bawaan Linux seperti man chmod dan man chown, serta
  berlatih di WSL.

---

**Credit:**  
_Template laporan praktikum Sistem Operasi (SO-202501) – Universitas Putra Bangsa_
