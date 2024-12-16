---
# Operasi File dan Direktori
Bagian ini menjelaskan berbagai operasi manajemen yang dapat dilakukan pada file dan direktori. Operasi ini mencakup pembuatan, menampilkan isi, menyalin, memindahkan, mengganti nama, dan menghapus file serta direktori. Operasi ini dapat dilakukan oleh pengguna biasa yang memiliki hak akses yang sesuai. Pengguna root dapat melakukan tugas-tugas ini pada file atau direktori apa pun dalam sistem, terlepas dari siapa pemiliknya. Jika terjadi kekurangan hak akses, pesan error akan ditampilkan.

---
## Membuat File dan Direktori
File dapat dibuat dengan berbagai cara menggunakan perintah yang berbeda; namun, hanya ada satu perintah untuk membuat direktori.

### Membuat File Kosong Menggunakan `touch`
Perintah `touch` digunakan untuk membuat file kosong. Jika file sudah ada, perintah ini hanya memperbarui timestamp-nya agar sesuai dengan tanggal dan waktu sistem saat ini. Jalankan perintah berikut sebagai root di direktori home pengguna root untuk membuat file1, lalu gunakan perintah `ls` untuk memverifikasinya:

Kolom ke-5 (kolom ukuran) dalam output menunjukkan nilai 0, yang berarti file1 dibuat dengan ukuran nol byte. Jika Anda menjalankan kembali perintah `touch` pada file ini setelah beberapa menit, timestamp baru akan diterapkan padanya.

Perintah `touch` memiliki beberapa opsi menarik. Opsi `-d` dan `-t` memungkinkan Anda untuk mengatur tanggal dan waktu tertentu pada file; opsi `-a` dan `-m` memungkinkan Anda mengubah waktu akses atau modifikasi file ke waktu sistem saat ini; dan opsi `-r` mengatur waktu modifikasi file berdasarkan file referensi. Contoh:
- Untuk mengatur tanggal file1 ke 20 September 2019:
- Untuk mengubah waktu modifikasi file1 ke waktu sistem saat ini:

Cobalah opsi lainnya untuk latihan.

### Membuat File Pendek Menggunakan `cat`
Perintah `cat` memungkinkan Anda membuat file teks pendek. Tanda sudut tutup `>` harus digunakan untuk mengalihkan output ke file yang ditentukan (misalnya catfile1):

Saat Anda menjalankan perintah tersebut, sistem akan menunggu input dari Anda. Ketikkan beberapa teks. Tekan tombol Enter untuk membuka baris baru dan lanjutkan mengetik. Setelah selesai, tekan Ctrl+d untuk menyimpan teks di catfile1 dan kembali ke prompt perintah. Verifikasikan pembuatan file dengan perintah `ls`.

### Membuat File Menggunakan `vim`
Anda dapat menggunakan editor `vim` untuk membuat dan memodifikasi file teks dalam ukuran apa pun. Lihat bagian sebelumnya dalam bab ini untuk panduan menggunakan `vim`.

### Membuat Direktori Menggunakan `mkdir`
Perintah `mkdir` digunakan untuk membuat direktori. Perintah ini akan menampilkan output jika dijalankan dengan opsi `-v`. Contoh berikut menunjukkan pembuatan direktori bernama dir1 di direktori home pengguna root:

Anda dapat membuat hierarki subdirektori dengan opsi `-p` (parent). Contoh:
```bash
mkdir -p dir2/perl/perl5
```

Perhatikan penempatan opsi dalam dua contoh ini. Banyak perintah di Linux menerima format serupa.

---
## Menampilkan Isi File
RHEL menawarkan berbagai alat untuk menampilkan isi file. Isi direktori hanyalah file dan subdirektori yang dikandungnya. Gunakan perintah `ls` untuk melihat isi direktori. Untuk melihat isi file, Anda dapat menggunakan perintah seperti `cat`, `more`, `less`, `head`, dan `tail`.

### Menggunakan `cat`
`cat` digunakan untuk menampilkan isi file teks. Biasanya digunakan untuk file pendek. Contoh:
```bash
cat .bash_profile
```
Anda dapat menambahkan opsi `-n` untuk melihat output dalam format bernomor.

### Menggunakan `tac`
`tac` menampilkan isi file teks secara terbalik. Contoh:
```bash
tac .bash_profile
```

### Menggunakan `less` dan `more`
`less` dan `more` adalah filter teks untuk melihat file teks panjang satu halaman dalam satu waktu. `less` lebih canggih dibandingkan `more`. Contoh:
```bash
less /etc/profile
more /etc/profile
```
Navigasi menggunakan tombol seperti di bawah:

| Tombol        | Fungsi                              |
|---------------|-------------------------------------|
| Spasi / `f`   | Scroll maju satu layar             |
| Enter         | Scroll maju satu baris             |
| `b`           | Scroll mundur satu layar           |
| `d`           | Scroll maju setengah layar         |
| `h`           | Menampilkan bantuan                |
| `q`           | Keluar                             |
| `/string`     | Mencari string maju                |
| `?string`     | Mencari string mundur (hanya `less`)|
| `n`           | Temukan string berikutnya          |
| `N`           | Temukan string sebelumnya (hanya `less`) |

### Menggunakan `head` dan `tail`
`head` menampilkan beberapa baris awal dari file teks, defaultnya 10 baris pertama. Contoh:
```bash
head /etc/profile
```
`tail` menampilkan 10 baris terakhir secara default. Contoh:
```bash
tail /etc/profile
```
Untuk melihat pembaruan file log secara real-time, gunakan opsi `-f` pada `tail`.

---
## Menghitung Kata, Baris, dan Karakter pada File Teks
Perintah `wc` (word count) digunakan untuk menampilkan jumlah baris, kata, dan karakter dalam file teks. Contoh:
```bash
wc /etc/profile
```
Opsi yang tersedia:

| Opsi | Aksi                                |
|------|-------------------------------------|
| `-l` | Menampilkan jumlah baris            |
| `-w` | Menampilkan jumlah kata             |
| `-c` | Menampilkan jumlah byte             |
| `-m` | Menampilkan jumlah karakter         |

---
## Menyalin File dan Direktori
Operasi penyalinan menduplikasi file atau direktori. Gunakan perintah `cp` dengan berbagai opsinya.

### Menyalin File
Untuk menyalin file dalam direktori yang sama:
```bash
cp file1 newfile1
```
Untuk menyalin file ke direktori lain:
```bash
cp file1 dir1/
```
Gunakan opsi `-i` untuk meminta konfirmasi sebelum menimpa file:
```bash
cp -i file1 dir1/
```

### Menyalin Direktori
Gunakan opsi `-r` untuk menyalin seluruh pohon direktori. Contoh:
```bash
cp -r dir1 dir2
```
Opsi `-p` dapat digunakan untuk mempertahankan atribut file atau direktori.

---
## Memindahkan dan Mengganti Nama File dan Direktori
Perintah `mv` digunakan untuk memindahkan atau mengganti nama file dan direktori.

### Memindahkan dan Mengganti Nama File
Contoh memindahkan file:
```bash
mv file1 dir1/
```
Contoh mengganti nama file:
```bash
mv newfile1 newfile2
```

### Memindahkan dan Mengganti Nama Direktori
Untuk memindahkan direktori:
```bash
mv dir1 dir2/
```
Untuk mengganti nama direktori:
```bash
mv dir2 dir20
```

---
## Menghapus File dan Direktori
Gunakan perintah `rm` untuk menghapus file atau direktori.

### Menghapus File
Contoh:
```bash
rm newfile2
```
Untuk meminta konfirmasi sebelum menghapus, gunakan opsi `-i`.

### Menghapus Direktori
Gunakan opsi `-r` untuk menghapus direktori beserta isinya:
```bash
rm -r dir1

**Dokumentasi dari halaman 142 sampai pada terakhir `Removing Files and Directories` Halaman:151**
