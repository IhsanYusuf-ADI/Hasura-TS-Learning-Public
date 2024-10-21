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

| Fitur                          | Didukung | Catatan                          |
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
  
Konektor ini membutuhkan URL JDBC agar berfungsi. Contoh:

```
JDBC_URL="jdbc:phoenix:localhost:2181:/hbase"
```
  
# SUPERGRAPH MODELING
  


