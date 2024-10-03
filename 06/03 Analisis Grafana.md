
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

## Fitur Utama Grafana:

### 1. Dashboard
Fitur inti Grafana adalah kemampuan untuk membuat **dashboard**. Dashboard terdiri dari panel-panel yang menampilkan data dari berbagai sumber. Fleksibilitas dashboard memungkinkan pengguna untuk menyesuaikan cara data divisualisasikan dan diinteraksikan.

### 2. Data Sources
Grafana terintegrasi dengan berbagai sumber data, termasuk:
- **Prometheus**: Alat monitoring dan pemberi peringatan open-source.
- **Elasticsearch**: Mesin pencarian dan analisis terdistribusi.
- **InfluxDB**: Basis data time-series yang dioptimalkan untuk menyimpan metrik kecepatan tinggi.
- **MySQL, PostgreSQL, dan basis data SQL lainnya**: Untuk menjalankan query dan memvisualisasikan data terstruktur.

### 3. Total Queries
Metrik ini menunjukkan jumlah total query yang dieksekusi di dashboard atau panel tertentu. Ini adalah metrik penting untuk mengamati **beban sistem** dan mengidentifikasi potensi bottleneck.

### 4. Query Latency
Menampilkan waktu yang dibutuhkan untuk menjalankan query dan mengembalikan data. **Latency** adalah indikator kinerja yang penting, karena query yang lambat dapat mempengaruhi responsivitas sistem secara keseluruhan.

### 5. Top Queries
Menampilkan query yang paling sering dijalankan atau yang paling banyak menggunakan sumber daya, membantu mengidentifikasi masalah kinerja pada pemrosesan query.

### 6. Panel dan Visualisasi
Grafana mendukung berbagai jenis panel untuk visualisasi data, antara lain:
- **Grafik**: Grafik garis, batang, dan scatter.
- **Stat**: Data numerik dengan ambang batas yang dapat dikonfigurasi untuk peringatan.
- **Gauge dan Progress Bar**: Untuk menampilkan nilai dalam rentang tertentu.
- **Heatmap**: Visualisasi kepadatan atau volume data dari waktu ke waktu.
- **Tabel**: Menampilkan data dalam bentuk tabel dengan filter.

### 7. Query Editor
**Query Editor** memungkinkan pengguna menulis dan mengubah query untuk mengambil data dari sumber data mereka. Fitur ini juga menyediakan:
- **Auto-complete**: Membantu menulis query dengan lebih cepat.
- **Dukungan Variabel**: Memungkinkan query dinamis menggunakan variabel dari dashboard.

### 8. Alerting
Grafana menyediakan mekanisme **alerting** bawaan untuk memberi tahu pengguna saat ambang batas tertentu tercapai. Anda dapat mengonfigurasi aturan peringatan, menetapkan tingkat keparahan, dan memilih di mana peringatan akan dikirim (email, Slack, PagerDuty, dll.).

### 9. Variabel
**Variabel** dalam Grafana berfungsi seperti template, memungkinkan Anda menggunakan pengaturan dashboard di beberapa tampilan. Ini berguna untuk mengubah query secara dinamis tanpa mengedit satu per satu.

### 10. Templating
Templating memungkinkan pengguna membuat dashboard yang dapat digunakan kembali dengan menggunakan variabel untuk memperbarui query dan visualisasi secara dinamis. Ini memudahkan penerapan dashboard yang sama untuk beberapa lingkungan atau dataset.

### 11. Tombol View, Edit, dan Inspect
- **View**: Memungkinkan pengguna untuk melihat lebih dalam dashboard atau panel tanpa mengubah konfigurasinya.
- **Edit**: Membuka editor panel di mana Anda dapat mengubah query, tipe visualisasi, dan pengaturan lainnya.
- **Inspect**: Menyediakan detail lebih lanjut tentang data panel, termasuk data mentah, waktu eksekusi query, dan statistik kinerja.

### 12. Anotasi
**Anotasi** adalah penanda yang dapat Anda tambahkan ke grafik Grafana untuk menandai kejadian penting, memudahkan korelasi antara perubahan metrik dengan kejadian seperti deployment atau insiden.

### 13. Tim dan Izin
Grafana mendukung **multi-user environments** dan memungkinkan pembuatan tim. Izin dapat diberikan pada dashboard atau panel tertentu, membatasi siapa yang dapat melihat atau mengedit konten.

### 14. Plugin
Grafana mendukung berbagai **plugin** untuk memperluas fungsionalitasnya, dari opsi visualisasi tambahan hingga sumber data dan aplikasi seperti **Loki** untuk log, **Tempo** untuk tracing, dan lainnya.

### 15. Snapshots
Pengguna dapat membuat **snapshot** dari dashboard mereka, yang menangkap keadaan dashboard pada waktu tertentu. Snapshot ini dapat dibagikan secara publik atau disimpan untuk analisis di kemudian hari.

### 16. Ekspor dan Impor Data
Grafana menyediakan kemampuan untuk mengekspor data atau panel ke dalam berbagai format untuk dibagikan atau dianalisis. Grafana juga mendukung impor dashboard yang sudah dibuat sebelumnya dari Grafana Labs dan komunitas.

### 17. Pelaporan
Grafana menawarkan fitur **pelaporan** di mana pengguna dapat mengekspor dashboard ke format PDF atau gambar dan mengirim laporan terjadwal melalui email.

### 18. Fitur Cloud dan Enterprise
- **Grafana Cloud**: Layanan Grafana yang sepenuhnya dikelola.
- **Grafana Enterprise**: Menawarkan fitur tambahan untuk tim, seperti RBAC (Role-Based Access Control), integrasi sumber data tambahan, dan dukungan jangka panjang.

![image](https://github.com/user-attachments/assets/013b006e-f43b-4447-859a-c1142fc78a6d)


![image](https://github.com/user-attachments/assets/e61f4157-b4cf-4439-9dd8-4abc673d57ff)

