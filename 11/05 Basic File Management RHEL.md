# Manajemen File Dasar di Linux

Linux mendukung berbagai jenis file yang diidentifikasi berdasarkan jenis data yang disimpan. File-file ini bisa berupa teks biasa atau format biner, yang merupakan jenis file paling umum. Ada juga file lain yang menyimpan informasi perangkat atau hanya menunjuk ke data yang sama di disk. Pemahaman yang baik tentang jenis-jenis file Linux penting bagi pengguna dan administrator Linux.

## Jenis File Umum di Linux
RHEL mendukung tujuh jenis file:
1. **File reguler**
2. **Direktori**
3. **Block special device file**
4. **Character special device file**
5. **Symbolic link**
6. **Named pipe**
7. **Socket**

Dua jenis pertama adalah yang paling umum digunakan. File perangkat digunakan oleh sistem operasi untuk berkomunikasi dengan perangkat periferal. Symbolic link sering digunakan, sedangkan named pipe dan socket digunakan untuk komunikasi antar proses.

Linux tidak memerlukan ekstensi file untuk mengidentifikasi jenis file. Perintah `file` dan `stat` dapat digunakan untuk mengetahui jenis data dalam file selain perintah `ls`.

## File Reguler
File reguler dapat berupa teks atau data biner. File ini mungkin berisi skrip shell atau perintah dalam bentuk biner. Saat Anda menjalankan perintah `ls` pada direktori, entri file reguler akan diawali dengan tanda hubung (-). Contohnya:

```bash
-rw-r--r--  1 root root 1234 Jan 1 12:34 contoh.txt
```

Tanda hubung di kolom pertama menunjukkan file tersebut adalah file reguler. Perintah `file` dan `stat` dapat digunakan untuk memverifikasi jenis file ini.

## Direktori
Direktori adalah wadah logis yang berisi file dan subdirektori. Saat Anda menjalankan perintah `ls` pada direktori, entri direktori akan diawali dengan huruf "d". Contohnya:

```bash
drwxr-xr-x  2 root root 4096 Jan 1 12:34 contoh_direktori
```

## File Perangkat (Block dan Character Special)
File perangkat di direktori `/dev` digunakan oleh sistem untuk berkomunikasi dengan perangkat keras. Ada dua jenis file perangkat:

- **Block Device**: Digunakan untuk perangkat yang mentransfer data dalam blok (misalnya, disk).
- **Character Device**: Digunakan untuk perangkat yang mentransfer data secara byte (misalnya, terminal).

Huruf "b" atau "c" di awal kolom pertama menunjukkan jenis file ini. Contohnya:

```bash
brw-r-----  1 root disk 8, 0 Jan 1 12:34 /dev/sda
crw-r-----  1 root tty 4, 0 Jan 1 12:34 /dev/console
```

Kolom kelima dan keenam menunjukkan **major number** dan **minor number** yang digunakan kernel untuk mengenali perangkat.

## Symbolic Link
Symbolic link (symlink) adalah shortcut ke file atau direktori lain. Saat Anda menjalankan perintah `ls -l` pada symbolic link, entri akan diawali dengan huruf "l" dan akan menunjukkan target link. Contohnya:

```bash
lrwxrwxrwx  1 root root 12 Jan 1 12:34 symlink -> /path/target
```

## Kompresi dan Pengarsipan
Kompresi dan pengarsipan digunakan untuk menghemat ruang disk dan mempercepat proses penyalinan file secara remote. RHEL menyediakan beberapa alat bawaan untuk kebutuhan ini, seperti `gzip`, `bzip2`, dan `tar`.

### Menggunakan gzip dan gunzip
- **gzip**: Digunakan untuk mengompres file, menambahkan ekstensi `.gz` pada file.
- **gunzip**: Digunakan untuk mendekompres file yang dikompres oleh gzip.

Contoh:
```bash
# Kompresi file
gzip contoh.txt

# Dekompresi file
gunzip contoh.txt.gz
```

### Menggunakan bzip2 dan bunzip2
- **bzip2**: Mengompres file dengan rasio kompresi yang lebih baik tetapi lebih lambat.
- **bunzip2**: Mendekompres file yang dikompres oleh bzip2.

Contoh:
```bash
# Kompresi file
bzip2 contoh.txt

# Dekompresi file
bunzip2 contoh.txt.bz2
```

### Perbedaan gzip dan bzip2
- **gzip**: Lebih cepat tetapi dengan rasio kompresi yang lebih rendah.
- **bzip2**: Lebih lambat tetapi dengan rasio kompresi yang lebih tinggi.

### Menggunakan tar
Perintah `tar` digunakan untuk membuat, menambahkan, memperbarui, daftar, dan mengekstrak arsip. Opsi umum:

| Opsi | Deskripsi |
|------|-----------|
| -c   | Membuat arsip |
| -f   | Menentukan nama arsip |
| -p   | Mempertahankan izin file |
| -r   | Menambahkan file ke arsip |
| -t   | Melihat isi arsip |
| -u   | Memperbarui arsip jika file baru lebih baru |
| -v   | Mode verbose |
| -x   | Mengekstrak arsip |

Contoh penggunaan:
```bash
# Membuat arsip tanpa kompresi
tar -cvf arsip.tar /path/direktori

# Membuat arsip dengan kompresi gzip
tar -czvf arsip.tar.gz /path/direktori

# Membuat arsip dengan kompresi bzip2
tar -cjvf arsip.tar.bz2 /path/direktori

# Melihat isi arsip
tar -tvf arsip.tar

# Mengekstrak arsip
tar -xvf arsip.tar -C /path/tujuan
```

Dengan memahami konsep dasar ini, pengguna dan administrator Linux dapat mengelola file dan direktori dengan lebih efektif.
