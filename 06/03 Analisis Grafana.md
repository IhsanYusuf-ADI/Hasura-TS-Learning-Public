
# Gambaran Umum Grafana

**Grafana** adalah sebuah platform open-source untuk pemantauan, visualisasi, dan pemberian peringatan (alerting) pada data yang dikumpulkan dari berbagai sumber seperti basis data, sistem log, aplikasi, dan lainnya. Grafana digunakan untuk membuat **dashboard** interaktif yang menampilkan visualisasi data secara real-time dan memantau sistem.

## Fungsi Utama Grafana:
1. **Monitoring dan Observability**: Memantau performa aplikasi dan sistem secara real-time.
2. **Visualisasi Data**: Menyediakan berbagai cara untuk menampilkan data, termasuk grafik, tabel, dan diagram.
3. **Alerting**: Mengirim peringatan berdasarkan ambang batas yang telah ditentukan pada metrik penting.

Monitoring Dashboard :
- **Hasura HTTP Graphql**
- **Hasura Health**

Tugas:
1. Lakukan Hit di API Hasura mas Ferdy
2. Lakukan Analiasa Panel

## Hasura HTTP Graphql
**Dashboard** ini menyediakan gambaran menyeluruh tentang performa query dan mutation di Hasura, termasuk metrik seperti latency, error rate, dan transfer data. Anda dapat menggunakan data ini untuk memantau performa aplikasi secara real-time dan melakukan analisis jika ada masalah performa atau error. 

![image](https://github.com/user-attachments/assets/df77d06a-4b4b-4316-9869-b0b884fec371)

### Total Queries
Metrik ini menunjukkan jumlah total query yang dieksekusi di dashboard atau panel tertentu. Ini adalah metrik penting untuk mengamati **beban sistem** dan mengidentifikasi potensi bottleneck.  

![image](https://github.com/user-attachments/assets/4d69ab3b-ec32-4c2d-8f4b-fe2cca3864fc)

  
Angka ini menunjukkan total permintaan GraphQL yang diajukan ke server Hasura dalam periode waktu yang dipilih. Di sini terlihat bahwa ada 54 permintaan.  

### Query Latency (P95):
Menyediakan metrik yang berguna untuk mengukur performa aplikasi. P95 lebih mencerminkan latensi dalam skenario dunia nyata dibandingkan rata-rata (mean), karena ia memperhitungkan lonjakan dan outlier yang dapat mempengaruhi pengalaman pengguna.  

![image](https://github.com/user-attachments/assets/eaadc894-a339-4dd4-b1df-ad42ffc5adbc)


Bagian ini menunjukkan latency (waktu tunda) untuk permintaan query GraphQL yang masuk. Angka "P95" artinya latensi di percentil ke-95, yaitu latensi maksimum yang dialami 95% dari total query. Di sini ditampilkan bahwa latensinya adalah 42.2 ms.  
  
### Total Mutations:
Berguna untuk memahami seberapa sering data berubah di sistem. Banyaknya mutation bisa berarti pengguna aktif memodifikasi data atau aplikasi banyak melakukan operasi backend.

![image](https://github.com/user-attachments/assets/65682042-7646-40f1-ae7c-b179d198c4e0)

  
Menunjukkan jumlah total mutation (modifikasi data) yang dilakukan melalui query GraphQL dalam periode waktu yang dipilih. Di sini ada satu mutation.  

### Mutation Latency (P95):
Latensi rendah di sini berarti mutation diproses dengan cepat, yang penting dalam aplikasi yang membutuhkan data yang selalu diperbarui secara real-time atau hampir real-time.

![image](https://github.com/user-attachments/assets/77681e56-155d-48a3-bc06-39bdf49e73bf)


Mirip dengan Query Latency, bagian ini menunjukkan latency dari operasi mutation dengan nilai percentile ke-95. Latensinya saat ini adalah 9.50 ms.

### Top Queries:
Membantu mengidentifikasi query mana yang paling sering digunakan atau mengonsumsi banyak sumber daya. Ini bermanfaat untuk mengoptimalkan atau memperbaiki performa dari query tersebut.

![image](https://github.com/user-attachments/assets/612be44e-051a-418d-8a3a-69dbf5e5e481)


Menampilkan layanan atau operasi GraphQL yang paling sering digunakan selama periode yang dipilih. Dalam contoh ini, layanan "hasuraferdy" yang melakukan dua query.

### Top Mutations:
Berguna untuk mengetahui operasi perubahan data yang paling sering dilakukan. Misalnya, jika ada mutation spesifik yang sering terjadi, Anda bisa memantau apakah ada masalah kinerja terkait.

![image](https://github.com/user-attachments/assets/984a5776-daa6-40fe-b7fa-b6b2979787e4)

  
Dalam hal ini adalah daftar mutation yang paling sering dilakukan dalam periode waktu yang ditentukan, tetapi di sini tidak ada data yang ditampilkan.  

### Query Request Rate:
Berguna untuk melacak seberapa banyak permintaan yang masuk ke server dalam kurun waktu tertentu. Lonjakan pada request rate bisa menunjukkan lonjakan aktivitas pengguna.

![image](https://github.com/user-attachments/assets/1ab116d5-6079-4b80-9c32-f1b61d0810e9)


Grafik ini menampilkan jumlah permintaan query yang diajukan ke server Hasura per satuan waktu. Pada grafik ini, tidak ada permintaan yang terlihat pada interval waktu terakhir.

### Mutation Request Rate:
Memantau frekuensi perubahan data. Metrik ini bisa membantu memahami kapan aplikasi melakukan banyak operasi perubahan data, yang mungkin berdampak pada kinerja.  

![image](https://github.com/user-attachments/assets/199137ec-1a69-4cbd-a88e-4f669cd835a0)


Grafik ini menampilkan frekuensi permintaan mutation yang dilakukan selama periode waktu tertentu. Pada gambar ini juga tidak ada data yang terlihat.

### Query Error Rate:
Sangat penting untuk melacak error. Jika tingkat error tinggi, ini bisa menunjukkan masalah pada aplikasi, misalnya karena query yang tidak valid atau masalah dengan infrastruktur.

![image](https://github.com/user-attachments/assets/2fc9a2d1-2f58-495f-9384-9459fd5f2329)


Grafik ini menunjukkan frekuensi error yang terjadi selama pemrosesan query GraphQL. Pada gambar ini, tidak ada error yang tercatat.

### Mutation Error Rate:
Memantau error dalam mutation penting karena kegagalan mutation bisa menyebabkan data yang salah atau tidak konsisten. Hal ini juga dapat mengganggu pengalaman pengguna aplikasi.

![image](https://github.com/user-attachments/assets/b04d5463-a083-413d-b09a-a575fe81dc5e)


Menunjukkan jumlah error yang terjadi selama operasi mutation. Tidak ada error yang tercatat di sini.

### Query Latency (P95) (Detail per waktu):
Bermanfaat untuk melihat pola perubahan latensi. Misalnya, lonjakan pada waktu tertentu mungkin menunjukkan waktu beban puncak atau masalah performa yang bersifat sementara.

![image](https://github.com/user-attachments/assets/14a60a49-ced2-4c22-b2cd-98f9b876f479)


Grafik ini memetakan latensi query ke waktu selama periode tertentu, membantu Anda melihat bagaimana latensi berubah sepanjang waktu. Ada beberapa lonjakan latensi, tetapi secara umum tetap rendah.

### Mutation Latency (P95) (Detail per waktu):
Berguna untuk memastikan operasi mutation tetap cepat dan tidak mengalami penundaan selama penggunaan.

![image](https://github.com/user-attachments/assets/1bc7bcd2-74dd-4a36-98d5-7a3af5714b9b)

  
Grafik yang sama seperti pada Query Latency, tetapi untuk operasi mutation. Di sini mutation tampaknya jarang terjadi, dengan beberapa titik data pada grafik.

### HTTP Connections:
Memantau jumlah koneksi membantu mengidentifikasi beban pada server. Peningkatan tajam dalam jumlah koneksi mungkin mengindikasikan peningkatan lalu lintas atau adanya masalah dengan penanganan sesi koneksi.

![image](https://github.com/user-attachments/assets/fae5fd6f-c0e8-4a96-9444-f83a86ebb2df)


Grafik ini menunjukkan jumlah koneksi HTTP yang sedang berlangsung antara klien dan server Hasura. Jumlahnya bervariasi, tetapi sebagian besar stabil di sekitar 1 koneksi.

### Cache Request Rate:
Pemanfaatan cache yang baik bisa meningkatkan performa aplikasi. Ketiadaan data di sini mungkin menunjukkan bahwa cache tidak aktif atau tidak dioptimalkan.

![image](https://github.com/user-attachments/assets/325f9108-c87c-4160-96de-b9a648f233a8)


Grafik ini seharusnya menunjukkan permintaan yang masuk ke cache, tetapi saat ini tidak ada data yang tercatat di sini.

### HTTP Data Transfer:
Metrik ini bisa memberikan wawasan tentang berapa banyak data yang dikirim dan diterima aplikasi. Jika ada transfer data yang besar, itu bisa menjadi indikator penggunaan berat oleh pengguna atau operasi besar di aplikasi.

![image](https://github.com/user-attachments/assets/8af0adb0-f420-43c7-ad19-3ab4db95d89f)


Grafik ini menampilkan jumlah data yang ditransfer melalui HTTP selama periode waktu tertentu. Tampak ada fluktuasi pada transfer data di berbagai waktu.

Action Data Transfer:
Memantau action di Hasura membantu memastikan bahwa data yang terkait dengan operasi spesifik berjalan dengan lancar dan efisien.

![image](https://github.com/user-attachments/assets/2a303715-2383-42d5-9a80-4d0c34404c42)


Ini menampilkan data yang ditransfer terkait dengan action di Hasura. Namun, tidak ada data yang tercatat pada grafik ini.


![image](https://github.com/user-attachments/assets/4af59f25-c8b7-46a6-aef2-7651ad864556)

![image](https://github.com/user-attachments/assets/36983f5f-f4bc-4aba-b23f-c368b2d672ee)

