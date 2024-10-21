# FEDERATION  
  
Federasi dalam Hasura DDN meningkatkan cara kita membangun dan mengelola API.

Ini adalah proses menggabungkan beberapa subgraf dengan beberapa sumber data menjadi satu *supergraph* untuk menciptakan API GraphQL terpadu yang menyediakan akses ke semua domain data kita melalui satu titik akhir.

Jika digabungkan dengan fitur kolaborasi di Hasura DDN, arsitektur ini memungkinkan alur kerja yang lebih kolaboratif dan memungkinkan tim untuk mengembangkan serta menerapkan subgraf secara mandiri sambil tetap menjaga tata kelola yang kuat atas proses pengembangan.

![image](https://github.com/user-attachments/assets/8abb3233-7713-4f9c-88f5-9fb9b6d02683)
  

## Manfaat
- **Pengembangan Modular**: Subgraf mempromosikan pengembangan modular dengan memungkinkan tim mengembangkan, menguji, dan menerapkan kode mereka secara mandiri tanpa mempengaruhi fungsionalitas subgraf lain. Ini juga dapat dilakukan di repositori terpisah untuk setiap subgraf, dalam pengaturan "multi-repo". Pendekatan modular ini menyederhanakan manajemen kode dan mengurangi risiko terjadinya perubahan yang merusak. Subgraf dapat diuji dalam konteks *supergraph* penuh untuk memastikan mereka bekerja sebagaimana mestinya saat digabungkan.
  
- **Penerapan Mandiri**: Subgraf juga dapat diterapkan secara individu, memastikan bahwa pembaruan pada satu subgraf tidak memerlukan waktu henti untuk seluruh *supergraph*. Fleksibilitas ini memungkinkan penerapan praktik CI/CD (Continuous Integration/Continuous Delivery) yang mempercepat peluncuran fitur.

- **Kolaborasi yang Ditingkatkan**: Subgraf memungkinkan tim yang berbeda untuk fokus pada domain data khusus mereka, meningkatkan kolaborasi dengan memungkinkan mereka berbagi API dan data melalui antarmuka terpadu. Lingkungan kerja yang kolaboratif namun terisolasi ini mempercepat waktu pengembangan dan mengurangi gesekan antar tim.

- **Tata Kelola yang Kuat**: Hasura DDN menyediakan fitur tata kelola yang kuat, seperti peran dan izin kolaborasi proyek yang memastikan integritas dan keamanan data di seluruh subgraf. Fitur-fitur ini memungkinkan tim untuk menerapkan kebijakan dan pembatasan data, melindungi informasi sensitif, serta menjaga kepatuhan dengan persyaratan regulasi.

- **Manajemen Data yang Efisien**: Federasi menyederhanakan manajemen data dengan memungkinkan tim bekerja dengan domain data yang lebih kecil dan lebih mudah diatur. Tim dapat fokus pada persyaratan data spesifik mereka tanpa kewalahan oleh skema data yang luas.

## Subgraf
Dalam Hasura DDN, subgraf mewakili modul metadata mandiri beserta *data connector*-nya yang menggabungkan domain data tertentu. Subgraf dapat dibangun secara independen satu sama lain dan dikelola dalam repositori yang terisolasi.

Field subgraf yang diekspos ke *supergraph* penuh dapat diberi prefiks untuk mencegah konflik dalam skema.

Data dapat dihubungkan antar subgraf menggunakan *relationships* bahkan antara subgraf yang ada di repositori terpisah.

## Subgraf Global
Saat menjalankan perintah `ddn supergraph init`, subgraf *globals* dibuat secara otomatis. Subgraf ini digunakan untuk menampung objek konfigurasi global untuk *supergraph*, seperti konfigurasi API dan pengaturan autentikasi.

Objek konfigurasi ini termasuk `AuthConfig`, `CompatibilityConfig`, dan `GraphqlConfig` serta file konfigurasi `subgraph.yaml` yang mendefinisikan subgraf *globals* itu sendiri.

Objek-objek ini terletak di subgraf *globals* secara default, tetapi dapat dipindahkan ke subgraf lain jika diperlukan.

## Otorisasi di Tingkat Subgraf
Autentikasi dikelola di tingkat *supergraph* di Hasura DDN sebagaimana didefinisikan oleh objek `AuthConfig` dan tidak dapat disesuaikan di tingkat subgraf.

Namun, otorisasi dikelola di tingkat subgraf. Ini berarti bahwa izin untuk model dan perintah didefinisikan dalam konteks objek-objek tersebut di subgraf masing-masing, dan tidak mempengaruhi subgraf lain.

Aturan otorisasi dalam satu subgraf juga dapat didefinisikan untuk merujuk pada data di subgraf lain, bahkan jika subgraf tersebut berada di repositori lain.

## Data Connectors
*Data Connector* menghubungkan subgraf ke sumber data. Mereka dapat berkomunikasi dengan sumber data dalam bahasa aslinya dan spesifik untuk setiap sumber data sehingga kita dapat memanfaatkan seluruh fungsionalitas sumber data tersebut.

Setiap subgraf independen dapat memiliki satu atau lebih *data connector*.

*Data Connector* tersedia untuk berbagai jenis sumber data, termasuk basis data, fungsi logika bisnis, REST API, dan GraphQL API. Kita juga dapat membuat *data connector* khusus untuk terintegrasi dengan sumber data lainnya.

Sumber data yang sama dapat dihubungkan ke beberapa subgraf melalui instance *data connector* di masing-masing subgraf. Ini memungkinkan tim yang berbeda untuk bekerja dengan sumber yang sama dari perspektif domain data yang berbeda.

## Relasi
Mendefinisikan *relationship* memungkinkan kita melakukan kueri di seluruh informasi yang terhubung di dalam dan antar subgraf.

Seperti biasa ketika menulis metadata, ekstensi Hasura untuk VS Code dapat membantu dengan fitur *auto-complete* dan validasi. Saat bekerja dengan *relationship* antar subgraf di repositori lain, ada beberapa perbedaan yang perlu diperhatikan. Cari tahu lebih lanjut tentang *cross-repo relationships* di sini.

## Contoh
Bayangkan sebuah aplikasi e-commerce dengan subgraf terpisah untuk manajemen katalog produk, informasi profil pengguna, dan pemrosesan pesanan.

1. **Subgraf Katalog Produk**: Tim yang bertanggung jawab atas pengelolaan informasi produk memiliki dan memelihara subgraf ini. Mereka menggunakan *data connector* PostgreSQL yang terdedikasi untuk terhubung dengan basis data relasional yang berisi detail produk, gambar, dan data inventaris.

2. **Subgraf Profil Pengguna**: Tim Data Pengguna memiliki subgraf ini, menggunakan *data connector* MongoDB untuk berinteraksi dengan basis data dokumen yang berisi profil pengguna, preferensi, dan riwayat pesanan.

3. **Subgraf Pemrosesan Pesanan**: Dikelola oleh tim pemenuhan, subgraf ini memanfaatkan *custom connector* TypeScript untuk menangani logika bisnis terkait pembuatan pesanan, pemrosesan pembayaran, dan pelacakan pengiriman, mengintegrasikan API dan layanan eksternal.

Setiap subgraf dikembangkan, diuji, dan diterapkan secara independen, memungkinkan tim untuk fokus pada domain data spesifik mereka tanpa mengganggu pekerjaan tim lain. *Supergraph* menggabungkan subgraf ini menjadi satu API terpadu yang menyediakan akses ke semua domain data melalui satu titik akhir GraphQL.

