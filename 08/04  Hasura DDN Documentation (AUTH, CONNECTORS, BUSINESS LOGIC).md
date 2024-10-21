# AUTH  

## Autentikasi & Hasura

Autentikasi adalah proses untuk memverifikasi identitas pengguna. 

Hasura DDN memanfaatkan variabel sesi yang mencakup informasi seperti pengguna, peran, organisasi, dan informasi lain yang diperlukan untuk menentukan hak akses data pengguna. Dengan variabel sesi ini, kita dapat menetapkan aturan izin pada domain data untuk memberikan kontrol akses yang lebih terperinci terhadap sumber daya.

Hasura bersifat netral terhadap layanan autentikasi yang kita gunakan. Tanggung jawab untuk menghasilkan variabel sesi diserahkan kepada layanan autentikasi baru atau yang sudah ada, sehingga memberikan fleksibilitas dan berbagai pilihan untuk kebutuhan autentikasi kita.

Autentikasi dapat dikonfigurasi melalui JSON Web Tokens (JWT) atau layanan webhook, dan dapat diintegrasikan dengan penyedia apa pun yang kita pilih (misalnya, Auth0, Firebase Auth, AWS Cognito, atau solusi kustom) untuk memverifikasi pengguna dan mengatur variabel sesi yang mengontrol akses ke data.

## Otorisasi Hasura

Otorisasi menentukan apa yang dapat diakses oleh pengguna yang telah diverifikasi.

Kita dapat menetapkan izin (juga dikenal sebagai aturan kontrol akses) pada jenis keluaran, model, dan perintah dalam domain data kita. Ada tiga bentuk izin:

1. **Izin Tipe**: Menentukan bidang mana dalam ObjectType yang dapat diakses oleh peran tertentu.
2. **Izin Model**: Menentukan baris mana dari Model yang dapat diakses oleh peran tertentu.
3. **Izin Perintah**: Menentukan apakah suatu Perintah dapat dieksekusi oleh peran tertentu.

Sebuah peran akan muncul ketika didefinisikan dengan salah satu dari tiga cara ini. Setiap permintaan ke Hasura harus menyertakan variabel sesi atau peran yang diperlukan dari layanan autentikasi kita. Kehadiran dan nilai dari peran-peran ini menentukan izin mana yang berlaku untuk permintaan tersebut. Tidak ada lagi konsep peran admin super-user bawaan di Hasura DDN.

Peran dan izin di Hasura diimplementasikan di lapisan Hasura Engine. Mereka tidak memiliki hubungan langsung dengan pengguna dan peran dari sumber data mana pun.

### Contoh
Untuk contoh izin otorisasi dalam metadata, lihat halaman izin di bagian pemodelan Supergraph.

### Menguji Izin
Kita dapat menguji izin secara langsung di antarmuka API Console Hasura:

1. Tetapkan izin yang diinginkan untuk tipe, model, atau perintah tertentu dalam metadata kita.
2. Lakukan permintaan melalui antarmuka API GraphiQL Console Hasura dengan token autentikasi yang sesuai dengan variabel sesi yang diperlukan.
3. Periksa data yang dikembalikan untuk memastikan bahwa itu sesuai dengan konfigurasi izin kita.

# CONNECTORS
  
## Apa itu Connector Data?
Connector data adalah layanan HTTP yang mengekspos serangkaian API yang digunakan Hasura untuk berkomunikasi dengan sumber data. Connector data bertanggung jawab untuk menafsirkan pekerjaan yang harus dilakukan atas nama Hasura Engine, menggunakan bahasa kueri asli dari sumber data.

Connector data yang dibangun sesuai dengan Spesifikasi NDC (Native Data Connector) menggunakan salah satu SDK yang tersedia memungkinkan siapa saja untuk menghubungkan sumber data yang kaya dan sangat asli ke supergraph mereka.

Spesifikasi NDC mendefinisikan serangkaian API yang harus diimplementasikan oleh sebuah connector data. API ini, yang menerima dan mengembalikan JSON, digunakan oleh Hasura untuk berkomunikasi dengan connector data. Spesifikasi NDC juga mendefinisikan serangkaian metadata yang harus disediakan oleh connector data kepada Hasura. Metadata ini digunakan oleh Hasura untuk mengintrospeksi connector data, menghasilkan skema GraphQL, dan mengeksekusi operasi terhadap sumber data.

Kita dapat membangun connector data sendiri atau menggunakan salah satu dari banyak connector yang tersedia di Hub Connector.

## Jenis Connector
Ada dua jenis utama connector yang tersedia untuk digunakan dalam proyek Hasura DDN:

### Connector Terverifikasi
Connector terverifikasi terdaftar di Hub Connector dan dapat dideploy ke Hasura DDN menggunakan Hasura CLI.

### Connector Tidak Terverifikasi
Connector tidak terverifikasi belum diverifikasi oleh Hasura. Connector ini hanya dapat dideploy di infrastruktur kita sendiri.

## Membangun Connector
Kita dapat membangun connector sendiri menggunakan salah satu SDK Hasura. Saat ini, Hasura memiliki SDK berikut yang tersedia:

- [**Rust Data Connector SDK**](https://github.com/hasura/ndc-sdk-rs)
- [**TypeScript Data Connector SDK**](https://github.com/hasura/ndc-sdk-typescript)

### Contoh Connector
- **Connector CSV** - Implementasi spesifikasi NDC langsung. Dokumentasi spesifikasi NDC dapat ditemukan di sini, dan panduan berdasarkan pengembangan connector untuk CSV dapat ditemukan di sini. Kita dapat menggunakan panduan ini sebagai referensi untuk membangun connector kita sendiri.
- **Connector Sendgrid** - Contoh implementasi API Sendgrid dapat ditemukan di GitHub di sini.

## Konektor Apache Phoenix  
Dengan konektor ini, Hasura memungkinkan Anda untuk secara instan membuat API GraphQL real-time di atas model data di Phoenix. Konektor ini mendukung fungsi-fungsi Phoenix yang tercantum dalam tabel di bawah ini, memungkinkan operasi data yang efisien dan skalabel. Selain itu, pengguna mendapatkan manfaat dari semua fitur kuat dari platform Hasura Data Delivery Network (DDN), termasuk kemampuan `query pushdown` yang mendelegasikan operasi kueri ke database, sehingga meningkatkan optimasi kueri dan performa.

### Fitur  
Berikut adalah matriks semua fitur yang didukung untuk konektor Phoenix:

| Feature                          | Supported | Notes                          |
|---------------------------------|----------|----------------------------------|
| Native Queries + Logical Models | ✅        |                                  |
| Native Mutations                | ❌        |                                  |
| Simple Object Query             | ✅        |                                  |
| Filter / Search                 | ✅        |                                  |
| Simple Aggregation              | ✅        |                                  |
| Sort                            | ✅        |                                  |
| Paginate                        | ✅        |                                  |
| Table Relationships             | ❌        |                                  |
| Views                           | ✅        |                                  |
| Remote Relationships            | ✅        |                                  |
| Custom Fields                   | ❌        |                                  |
| Mutations                       | ❌        |                                  |
| Distinct                        | ❌        |                                  |
| Enums                           | ❌        |                                  |
| Naming Conventions              | ❌        |                                  |
| Default Values                  | ❌        |                                  |
| User-defined Functions          | ❌        |                                  |

### Sebelum Memulai  
- Buat akun Hasura Cloud  
- Instal CLI  
- Instal ekstensi Hasura di VS Code  
- Buat *supergraph*  
- Buat *subgraph*

### Menggunakan Konektor  
  
Apache Phoenix connector termasuk connector tidak terverifikasi. Connector ini belum diverifikasi oleh Hasura, sehingga tidak terdaftar di Hasura Hub dan tidak bisa langsung dideploy ke Hasura DDN menggunakan Hasura CLI. Connector Phoenix hanya bisa diimplementasikan dan di-deploy di infrastruktur Anda sendiri.

Meskipun belum terverifikasi, Anda tetap bisa menggunakan Phoenix connector dengan mengatur konfigurasi secara manual, seperti menyediakan URL JDBC yang tepat untuk menghubungkan database Phoenix dengan Hasura. Karena belum diverifikasi, Anda harus memastikan sendiri performa, keamanan, dan kompatibilitas connector ini.

Contoh pengaturan JDBC untuk Phoenix connector:

```
JDBC_URL="jdbc:phoenix:localhost:2181:/hbase"
```
  
Connector ini berlisensi di bawah Apache License 2.0, sehingga bebas digunakan dan dimodifikasi dalam infrastruktur yang Anda kelola sendiri.

  
# SUPERGRAPH MODELING
**Struktur Supergraph di Hasura DDN**  

![image](https://github.com/user-attachments/assets/f7f6a79d-38fc-4f45-a81d-5efc3feca0a1)


**Penjelasan Struktur Supergraph:**  

1. **Supergraph:**
   Pada bagian paling atas terdapat entitas yang disebut `my_supergraph`, yang terdiri dari beberapa subgraph. Supergraph adalah kombinasi dari beberapa subgraph yang dihubungkan bersama untuk membentuk satu API terpadu.

2. **Subgraph:**
   - Setiap *subgraph* berisi beberapa komponen, salah satunya adalah `engine metadata`.
   - Subgraph seperti `my_subgraph`, `another_subgraph`, dan `yet_another_subgraph` memiliki struktur serupa. Masing-masing memiliki metadata mesin yang mengelola beberapa aspek seperti:
     - **Models**
     - **Relationships**
     - **Permissions**
     - **Commands**

3. **ConnectorLink:**
   Setiap subgraph dihubungkan ke `a_connector` melalui elemen yang disebut *ConnectorLink*. *ConnectorLink* ini bertindak sebagai penghubung antara metadata mesin di subgraph dan sumber data yang ada di luar.

4. **a_connector:**
   `a_connector` adalah komponen yang memungkinkan setiap subgraph terhubung ke sumber data yang berbeda. Beberapa contoh sumber data:
   - **Data Source**: Sumber data yang disambungkan ke subgraph pertama.
   - Pada subgraph lainnya, sumber data mungkin berbeda seperti data di cloud atau sumber data eksternal lainnya.

5. **Data Source:**
   - Terdapat berbagai jenis sumber data yang dapat digunakan seperti yang terlihat di sisi kanan diagram:
     - **Data Source biasa**
     - **Cloud Data Source**
     - **Star-shaped Data Source** (kemungkinan mengacu pada sumber data khusus).

Secara keseluruhan, gambar ini mengilustrasikan bagaimana beberapa subgraph dikelola dalam *Supergraph*, di mana setiap subgraph memiliki konektor yang menghubungkannya ke sumber data eksternal melalui *ConnectorLink*.

# BUSINESS LOGIC  

Dengan Hasura, Anda dapat secara instan dan mudah menggabungkan logika bisnis khusus sebagai bagian dari Supergraph API menggunakan konektor Hasura Lambda.

Transformasikan, mutasi, atau perkaya data, sambungkan ke layanan yang ada, atau perluas fungsionalitas data asli Anda menggunakan fungsi, semuanya dihosting oleh Hasura atau di infrastruktur Anda sendiri.  

## Pengenalan Lambda Connectors

Lambda connectors memungkinkan Anda dengan mudah mengimplementasikan dan meng-host logika bisnis kustom dalam proyek Hasura Anda.

Dengan lambda connectors, Anda cukup menulis fungsi, melacaknya, dan fungsi-fungsi tersebut secara otomatis tersedia di API Anda.

## Pendekatan Tradisional
Bayangkan Anda ditugaskan untuk menangani logika bisnis kustom dalam setup middleware tradisional.

Setiap hari, Anda terjun ke kode kustom dan menulis skrip untuk memparsing dan merutekan permintaan yang masuk ke handler yang sesuai. Anda bahkan belum mulai menulis logika bisnis kustom.

Selanjutnya, Anda perlu memperkaya data. Ini kemungkinan melibatkan mengelola beberapa API yang berbeda untuk meningkatkan informasi mentah. Setelah itu, Anda dengan cermat memformat dan mentransformasi data agar sesuai dengan struktur yang diinginkan. Penanganan kesalahan memerlukan logika kustom tersendiri untuk mengelola berbagai kasus kegagalan dengan efektif. Akhirnya, Anda memformat respons dan mengirimkannya kembali ke klien, berharap tidak ada yang rusak dalam prosesnya.

Langkah-langkah manual ini memakan waktu, rentan terhadap kesalahan, dan semakin sulit dipertahankan seiring dengan berkembangnya aplikasi dan tim Anda. Kebutuhan konstan untuk kode kustom di setiap tahap proses bisa sangat melelahkan dan membuat frustrasi.

### Arsitektur
Setup tradisional seperti ini membutuhkan layanan terpisah yang berjalan pada berbagai komponen infrastruktur. Anda mungkin meng-host sendiri atau bergantung pada banyak penyedia cloud. Sistem terdistribusi ini menyebabkan bottleneck, meningkatkan latensi, dan memerlukan lebih banyak sumber daya untuk pemeliharaan dan penskalaan. Koordinasi antara berbagai layanan bisa menjadi tantangan, menyebabkan potensi masalah sinkronisasi dan kompleksitas dalam proses deployment.

## Dengan Hasura DDN
Sekarang, bayangkan skenario berbeda dengan Hasura DDN.

Alih-alih memparsing dan merutekan permintaan secara manual, Hasura secara otomatis menangani tugas-tugas ini, membebaskan Anda dari menulis kode kustom untuk setiap permintaan yang masuk.

Pengambilan data menjadi mudah karena Anda tidak lagi harus mengelola banyak API eksternal. Hasura sudah memfederasi semua sumber Anda dan menghasilkan kueri GraphQL untuk Anda, menyederhanakan proses secara signifikan.

Anda cukup menulis fungsi kustom dalam TypeScript atau Python, Hasura mengintrospeksinya, dan kemudian mengintegrasikan fungsi-fungsi tersebut ke dalam API GraphQL Anda secara mulus.

Dengan Hasura DDN, langkah-langkah manual yang membosankan dari middleware tradisional digantikan oleh proses yang efisien dan terstruktur yang memungkinkan Anda fokus pada penulisan logika bisnis yang bermakna daripada kode boilerplate.

### Arsitektur
Kami dapat meng-host logika bisnis kustom ini untuk Anda. Atau, jika Anda mau, Anda dapat meng-host di infrastruktur Anda sendiri. Bagaimanapun, proses deployment Anda telah disederhanakan secara signifikan. Hasura DDN menyediakan platform terpadu yang meminimalkan latensi, mengurangi kompleksitas koordinasi layanan, dan menawarkan kemampuan penskalaan yang mulus. Alat observasi dan manajemen terintegrasi memastikan bahwa logika bisnis Anda berjalan secara efisien dan andal, sambil mengekspos logika bisnis kustom Anda langsung ke konsumen API.

# FEDERATION  
  
Federasi dalam Hasura DDN meningkatkan cara Anda membangun dan mengelola API.

Ini adalah proses menggabungkan beberapa subgraf dengan beberapa sumber data menjadi satu *supergraph* untuk menciptakan API GraphQL terpadu yang menyediakan akses ke semua domain data Anda melalui satu titik akhir.

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

*Data Connector* tersedia untuk berbagai jenis sumber data, termasuk basis data, fungsi logika bisnis, REST API, dan GraphQL API. Anda juga dapat membuat *data connector* khusus untuk terintegrasi dengan sumber data lainnya.

Sumber data yang sama dapat dihubungkan ke beberapa subgraf melalui instance *data connector* di masing-masing subgraf. Ini memungkinkan tim yang berbeda untuk bekerja dengan sumber yang sama dari perspektif domain data yang berbeda.

## Relasi
Mendefinisikan *relationship* memungkinkan Anda melakukan kueri di seluruh informasi yang terhubung di dalam dan antar subgraf.

Seperti biasa ketika menulis metadata, ekstensi Hasura untuk VS Code dapat membantu dengan fitur *auto-complete* dan validasi. Saat bekerja dengan *relationship* antar subgraf di repositori lain, ada beberapa perbedaan yang perlu diperhatikan. Cari tahu lebih lanjut tentang *cross-repo relationships* di sini.

## Contoh
Bayangkan sebuah aplikasi e-commerce dengan subgraf terpisah untuk manajemen katalog produk, informasi profil pengguna, dan pemrosesan pesanan.

1. **Subgraf Katalog Produk**: Tim yang bertanggung jawab atas pengelolaan informasi produk memiliki dan memelihara subgraf ini. Mereka menggunakan *data connector* PostgreSQL yang terdedikasi untuk terhubung dengan basis data relasional yang berisi detail produk, gambar, dan data inventaris.

2. **Subgraf Profil Pengguna**: Tim Data Pengguna memiliki subgraf ini, menggunakan *data connector* MongoDB untuk berinteraksi dengan basis data dokumen yang berisi profil pengguna, preferensi, dan riwayat pesanan.

3. **Subgraf Pemrosesan Pesanan**: Dikelola oleh tim pemenuhan, subgraf ini memanfaatkan *custom connector* TypeScript untuk menangani logika bisnis terkait pembuatan pesanan, pemrosesan pembayaran, dan pelacakan pengiriman, mengintegrasikan API dan layanan eksternal.

Setiap subgraf dikembangkan, diuji, dan diterapkan secara independen, memungkinkan tim untuk fokus pada domain data spesifik mereka tanpa mengganggu pekerjaan tim lain. *Supergraph* menggabungkan subgraf ini menjadi satu API terpadu yang menyediakan akses ke semua domain data melalui satu titik akhir GraphQL.


