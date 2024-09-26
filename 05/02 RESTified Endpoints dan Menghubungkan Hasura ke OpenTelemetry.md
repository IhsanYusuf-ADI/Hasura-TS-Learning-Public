# RESTified Endpoints dan Menghubungkan Hasura ke OpenTelemetry

## RESTified Endpoints

### 1. Masuk ke Menu API
Pertama, masuklah ke menu **API** di Hasura Console. Setelah itu, buatlah query GraphQL yang ingin diubah menjadi RESTful endpoint (RESTified). Anda dapat melihat query yang tersedia dengan mengeklik tombol **Explorer** yang terdapat di bagian atas field query GraphQL.

![image](https://github.com/user-attachments/assets/c41d8265-03cb-4903-b6f8-ed9e6f62c5a9)

  
### 2. Membuat REST Endpoint
Setelah menentukan query yang akan diubah, klik tombol **REST** di bagian atas field query GraphQL. Anda akan diarahkan ke halaman REST seperti pada gambar berikut.  

![image](https://github.com/user-attachments/assets/370d59d0-e1c2-49a5-95eb-6c92b9923dfe)

  
### 3. Konfigurasi REST Endpoint
- Masukkan nama untuk REST endpoint.
- Berikan deskripsi mengenai query (opsional).
- Centang box method yang ingin diubah menjadi REST endpoint (RESTified). Misalnya, pilih method `GET` untuk mempermudah developer dalam mengakses query.
- Setelah konfigurasi selesai, klik tombol **Create** untuk membuat REST endpoint.

Jika berhasil, REST endpoint yang telah Anda buat akan muncul pada daftar REST endpoint seperti gambar berikut.

![image](https://github.com/user-attachments/assets/26db2051-fd19-4f05-8205-9499eaceb849)

  
### 4. Menggunakan REST Endpoint di Postman
- Buka Postman dan masukkan link REST endpoint yang telah dibuat.
- Pilih method yang sesuai dengan REST endpoint tersebut (misalnya `GET`).
- Jangan lupa untuk menambahkan header `x-hasura-admin-secret` beserta valuenya.
- Klik tombol **Send** untuk menjalankan REST endpoint dan melihat hasilnya pada kolom `Response`.

![Screenshot (237)](https://github.com/user-attachments/assets/77b4fbec-33de-43f8-abdb-988b42a85a5a)


### Contoh REST Endpoint yang Telah Dibuat
1. **getAllRelationTodos**  
   Query ini digunakan untuk mengambil semua data pada tabel `todo` beserta semua tabel yang berelasi dengan tabel `todo`.

![image](https://github.com/user-attachments/assets/d6d970d1-611b-46f5-9996-d4c399d25520)

  
2. **getAllUsers**  
   Query ini digunakan untuk mengambil semua data pada tabel `user`.

![image](https://github.com/user-attachments/assets/1f9160f9-99bc-436f-bd46-344fa23a8ad1)

  
3. **getTodoById**  
   Query ini digunakan untuk mengambil data pada tabel `todo` berdasarkan `id` tertentu, misalnya `id = 1`.
  
![image](https://github.com/user-attachments/assets/6b4526ad-c741-48c2-be26-bfbe9f4c5031)

  
4. **getUserById**  
   Query ini digunakan untuk mengambil data pada tabel `user` berdasarkan `id` tertentu, misalnya `id = 1`.

![image](https://github.com/user-attachments/assets/be0e3b6a-6b19-4914-a44d-624d24bb5126)

  
## Menghubungkan Hasura ke OpenTelemetry

Tujuan dari menghubungkan Hasura ke OpenTelemetry adalah untuk mengakses **Traces**, **Metrics**, dan **Logs** secara lebih efisien.

### 1. Masuk ke Pengaturan OpenTelemetry
Buka menu **Settings** pada Hasura Console untuk diarahkan ke halaman pengaturan. Selanjutnya, klik **OpenTelemetry Exporter** pada navigasi di sebelah kiri. Anda akan diarahkan ke halaman konfigurasi OpenTelemetry.

### 2. Konfigurasi OpenTelemetry
- Pilih **Status: Enabled** untuk mengaktifkan OpenTelemetry.
- Masukkan endpoint yang digunakan untuk `Traces`, `Metrics`, dan `Logs`.
- Klik tombol **Connect/Update** untuk menerapkan konfigurasi koneksi OpenTelemetry.

![image](https://github.com/user-attachments/assets/8b383ee9-5cb6-474b-842f-87c9d0592f43)


Adapun URL Endpoint yang saya gunakan adalah sebagai berikut:    
Traces Endpoint

```bash
http://10.100.13.205:8889/v1/traces
```

Metrics Endpoint  
  
```bash
http://10.100.13.205:8889/v1/metrics
```

Logs Endpoint  
  
```bash
http://10.100.13.205:8889/v1/logs
```
  
Dengan langkah-langkah ini, Anda dapat menghubungkan Hasura ke OpenTelemetry untuk memantau performa, error, dan logging pada aplikasi Anda secara terintegrasi.
