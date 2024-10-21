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

# PLUGIN  

Hasura memungkinkan kita untuk menambahkan hook HTTP di berbagai tahap eksekusi engine. Ini memungkinkan kita untuk meningkatkan DDN dengan menambahkan fungsionalitas khusus melalui kode kita sendiri.

Plugin ini dapat diterapkan pada langkah-langkah berikut:

| Langkah Eksekusi | Deskripsi | Contoh Penggunaan |
| --- | --- | --- |
| Pre-Parse | Langkah pertama dalam pipeline eksekusi, di mana logika khusus dapat diterapkan sebelum kueri diurai dan representasi internalnya dibuat. | Tambahkan lapisan daftar izinkan untuk membatasi akses ke kueri dan mutasi tertentu. |
| Pre-Response | Langkah terakhir dalam pipeline eksekusi, di mana logika khusus dapat ditambahkan setelah kueri dieksekusi tetapi sebelum respons dikirim ke klien. | Memicu notifikasi Slack setelah mutasi dieksekusi. |

## Arsitektur  
  
![image](https://github.com/user-attachments/assets/ed55d0d4-05c7-48a6-a0dd-d97cfe9c3342)


Plugin engine adalah server HTTP yang berjalan bersamaan dengan DDN dan dapat ditulis dalam bahasa apa pun yang mampu menjalankan server HTTP. Plugin dikonfigurasi di DDN menggunakan metadata DDN. Engine mengirimkan permintaan HTTP ke plugin pada langkah eksekusi yang ditentukan, plugin memproses permintaan tersebut, dan kemudian mengirimkan respons kembali ke engine, yang melanjutkan eksekusi berdasarkan respons plugin.

## Konfigurasi Plugin
Plugin engine dikonfigurasi di DDN menggunakan metadata. Metadata menentukan URL plugin engine dan langkah eksekusi di mana plugin harus dipanggil. Konfigurasi juga dapat mengontrol permintaan yang dikirim ke plugin engine.

Berikut adalah contoh konfigurasi plugin dalam metadata DDN:

```yaml
kind: LifecyclePluginHook
version: v1
definition:
  name: cloudflare allowlist
  url:
    valueFromEnv: ALLOW_LIST_URL
  pre: parse
  config:
    request:
      headers:
        additional:
          hasura-m-auth:
            value: "your-strong-m-auth-key"
      session: {}
      rawRequest:
        query: {}
        variables: {}
```

Dalam contoh ini, plugin dikonfigurasi untuk dijalankan pada langkah eksekusi pre-parse. Plugin ini disebut cloudflare allowlist. URL plugin dibaca dari variabel lingkungan `ALLOW_LIST_URL`. Plugin ini dikonfigurasi untuk menambahkan header `hasura-m-auth` ke permintaan dengan nilai `your-strong-m-auth-key`.

Permintaan yang dikirim ke plugin dikonfigurasi untuk memiliki kueri dan variabel dari permintaan GraphQL yang masuk. Permintaan ini juga dikonfigurasi untuk memiliki informasi sesi dari permintaan yang masuk.

## Plugin Pre-Parse
Plugin `pre-parse` dipicu pada langkah pertama dalam pipeline eksekusi, sebelum kueri diurai. Gunakan langkah ini untuk menambahkan logika khusus sebelum penguraian dimulai.

### Permintaan Plugin Pre-Parse
Contoh permintaan yang dikirim ke plugin `pre-parse` adalah sebagai berikut:

```json
{
  "rawRequest": {
    "query": "query MyQuery { getAuthorById(author_id: 10) { first_name } }",
    "variables": {},
    "operationName": "MyQuery"
  },
  "session": {
    "role": "user",
    "variables": {
      "x-hasura-role": "user",
      "x-hasura-user-id": "123"
    }
  }
}
```

### Respons Plugin Pre-Parse
Plugin `pre-parse` dapat mengontrol pipeline eksekusi dengan mengembalikan respons ke DDN. Respons tersebut bisa berupa:

| Jenis Respons | Kode Status HTTP | Isi Respons | Deskripsi |
| --- | --- | --- | --- |
| Continue | 204 | - | Lanjutkan eksekusi. |
| Response | 200 | Isi respons | Hentikan eksekusi dan kirimkan respons ke klien. |
| User Error | 400 | Objek kesalahan | Hentikan eksekusi dan kirimkan kesalahan ke klien. |
| Internal Error | 500 | Objek kesalahan | Hentikan eksekusi dan kirimkan kesalahan internal ke klien. |

### Validasi Respons
Respons dari plugin tidak divalidasi oleh DDN. iNI Adalah tanggung jawab plugin untuk mengembalikan respons yang valid berdasarkan logika plugin.

### Use Cases
Plugin `pre-parse` dapat digunakan untuk menambahkan berbagai fungsionalitas ke DDN. Beberapa kasus penggunaan adalah:

- **Allowlist**: Tambahkan lapisan daftar izinkan untuk membatasi akses ke kueri dan mutasi tertentu berdasarkan permintaan yang masuk dan informasi sesi.
- **Basic Rate Limiting**: Terapkan pembatasan laju untuk membatasi jumlah total permintaan yang dapat dilakukan ke DDN dalam jangka waktu tertentu.
- **Custom Query Validation**: Tambahkan logika validasi kueri kustom untuk memastikan bahwa kueri yang masuk valid berdasarkan logika bisnis kustom.
- **Cache Get**: Implementasikan lapisan cache get untuk mengambil respons dari cache sebelum mengeksekusi kueri.

### Multiple Plugin Pre-Parse  
Multiple plugin pre-parse dapat dikonfigurasi dalam metadata DDN. Plugin-plugin ini dieksekusi secara berurutan sesuai dengan urutan yang didefinisikan dalam metadata.

Catatan penting: Jika sebuah plugin mengembalikan `Response`, `User Error`, atau `Internal Error`, maka eksekusi akan berhenti dan respons dikirimkan ke klien. Plugin selanjutnya tidak akan dijalankan.

Mari kita ambil contoh di mana mesin dikonfigurasi dengan dua plugin pre-parse: `Pre-parse hook 1` dan `Pre-parse hook 2`.

![image](https://github.com/user-attachments/assets/7afe78eb-3fff-4c0d-9b21-344884778a37)

  
Dalam contoh ini, mesin dikonfigurasi dengan dua plugin pre-parse. Mesin akan mengirim permintaan ke `Pre-parse hook 1`, yang kemudian memproses permintaan dan mengirimkan respons. Tergantung pada respons yang dikembalikan, mesin akan melanjutkan eksekusi atau menghentikannya dan mengirimkan respons ke klien.

Case 1: Continue
Jika `Pre-parse hook 1` mengembalikan respons `Continue` (kode status HTTP adalah 204), mesin akan mengirim permintaan ke `Pre-parse hook 2`. Mesin akan terus melanjutkan proses ini hingga semua plugin pre-parse dieksekusi.

**Catatan**: Isi dari respons `Continue` akan diabaikan oleh DDN. Plugin dapat mengembalikan body respons yang kosong.

Kasus 2: Response / User Error / Internal Error
Jika `Pre-parse hook 1` mengembalikan respons `Response` (kode status HTTP 200), `User Error` (kode status HTTP 400), atau `Internal Error` (kode status HTTP 500), maka mesin akan menghentikan eksekusi dan mengirimkan respons ke klien. Plugin `pre-parse` selanjutnya tidak akan dieksekusi.
  
![image](https://github.com/user-attachments/assets/b1c36501-06bc-4716-b2d3-61073da93b67)


**Apakah plugin `pre-response` berikutnya akan dieksekusi?** Ya, plugin `pre-response` berikutnya akan tetap dieksekusi meskipun `plugin pre-parse` mengembalikan `Response`, `User Error`, atau `Internal Error`.

Jika semua plugin `pre-parse` mengembalikan `Continue` (kode status HTTP 204), maka mesin akan melanjutkan eksekusi dan mengirimkan respons yang dihasilkan oleh mesin ke klien.

**Apakah plugin `pre-parse` dapat mengubah permintaan?** Tidak, plugin `pre-parse` tidak dapat memodifikasi permintaan dalam pipeline eksekusi. Plugin hanya dapat mengendalikan pipeline eksekusi dengan mengembalikan respons yang sesuai.
  
## Plugin Pre-Response
Plugin `pre-response` dipicu pada langkah terakhir dalam pipeline eksekusi setelah kueri dieksekusi. Gunakan langkah ini untuk menambahkan webhook setelah kueri dieksekusi.

### Request Plugin Pre-Response  

Contoh request yang dikirimkan ke plugin pra-respons adalah sebagai berikut:

```json
{
  "response": {
    "data": {
      "getAuthorById": {
        "first_name": "John"
      }
    }
  },
  "session": {
    "role": "user",
    "variables": {
      "x-hasura-role": "user",
      "x-hasura-user-id": "123"
    }
  },
  "rawRequest": {
    "query": "query MyQuery { getAuthorById(author_id: 10) { first_name } }",
    "variables": {},
    "operationName": "MyQuery"
  }
}
```  

### Respons Plugin Pre-Response
Plugin `pre-response` tidak dapat mengontrol pipeline eksekusi. Respons dari plugin ini diabaikan oleh DDN.

### Use Cases
Plugin `pre-response` dapat digunakan untuk menambahkan sejumlah fungsionalitas ke DDN. Beberapa kasus penggunaan adalah:

- **Slack Notifications**: Memicu notifikasi Slack setelah kueri/mutasi dieksekusi.
- **Cache Set**: Implementasikan lapisan cache set untuk menyimpan respons di cache.
- **Cache Invalidation**: Implementasikan lapisan invalidasi cache untuk membatalkan cache berdasarkan permintaan yang masuk dan informasi sesi.
- **Audit Log**: Tambahkan log audit untuk melacak kueri dan mutasi yang dieksekusi oleh pengguna.
  
### Multiple Plugin Pre-Response
Multiple plugin `pre-response` dapat dikonfigurasi dalam metadata DDN. Plugin-plugin ini dieksekusi secara paralel.
  
Mari kita ambil contoh di mana mesin dikonfigurasi dengan dua plugin `pre-response`: `Pre-response hook 1` dan `Pre-response hook 2`.

![image](https://github.com/user-attachments/assets/efd15dbe-f552-4545-8c65-95ac942d00e4)

  
Dalam contoh ini, mesin dikonfigurasi dengan dua plugin `pre-response`. Mesin akan mengirim permintaan ke kedua plugin secara paralel. Mesin tidak menunggu respons dari plugin tersebut dan langsung mengirimkan respons yang dihasilkan oleh mesin ke klien.

# Deployment

Dibandingkan dengan Hasura v2, ada peningkatan arsitektur yang mendasar di Hasura v3 di mana buildtime dan runtime dipisahkan menjadi komponen yang berbeda. Control plane membangun metadata proyek dan membuatnya tersedia untuk mesin Hasura v3 yang berjalan di data plane.  

![image](https://github.com/user-attachments/assets/fc135352-a6c0-4689-8cea-12cf76f7314f)


## Control Plane
Pengembang berinteraksi dengan control plane Hasura DDN untuk membuat supergraph builds. Setiap build supergraph menghasilkan API GraphQL yang unik, di-host oleh data plane. Sebagai pengembang, kita menggunakan konsol Hasura di control plane untuk memvisualisasikan supergraph dan berinteraksi dengan API GraphQL.

Control plane juga menyediakan fitur monitoring dan observability. Konsol memungkinkan kita menambahkan kolaborator sehingga kita dapat mengundang pengembang lain untuk berkontribusi pada supergraph dan juga berbagi API GraphQL dengan konsumen.

Control plane memungkinkan deployment preview, dalam bentuk builds, dengan menerima metadata proyek kita dan membuatnya tersedia di data plane dalam format yang dioptimalkan untuk runtime Hasura v3 Engine.

Setiap build yang kita buat akan menghasilkan API GraphQL yang unik dan immutable yang dapat digunakan untuk mengeksekusi operasi secara independen, tanpa memengaruhi build lainnya.

Control plane memproses data observability yang dikirim oleh data plane dan membuatnya tersedia untuk dikonsumsi di konsol. Ini termasuk jejak untuk setiap operasi GraphQL yang dieksekusi dan metrik keseluruhan tentang penggunaan API.

## Data Plane
Hasura v3 Engine dan data connectors berjalan di data plane. Data plane Hasura DDN menjalankan versi v3 engine yang ditingkatkan yang mampu melayani beberapa API GraphQL secara bersamaan. Ini adalah runtime mirip serverless di mana konfigurasi untuk mengeksekusi permintaan GraphQL dimuat sesuai permintaan dan dibuang setelah memproses permintaan. Ini membuat runtime sangat efisien dan sangat dapat diskalakan.

Beberapa connectors juga menggunakan strategi serupa untuk secara efisien menangani connection pools, hanya membuatnya sesuai permintaan.

Data plane dapat dideploy di beberapa wilayah, memungkinkan permintaan API diarahkan ke lokasi klien terdekat untuk latensi optimal melalui load-balancing canggih dengan alamat IP Anycast.

Fitur ini tersedia secara langsung di GCP, AWS, dan Azure, dan juga dapat di-deploy pada Hasura DDN yang di-host sendiri, asalkan infrastruktur mendukung kemampuan jaringan yang diperlukan.

## Deployment Private di Hasura DDN

Dengan deployment private untuk Hasura DDN, kita dapat menjalankan Hasura dan konektor kita baik pada infrastruktur yang didedikasikan oleh Hasura atau pada infrastruktur kita sendiri.

Hasura DDN Private menawarkan keamanan dan isolasi yang ditingkatkan dengan memungkinkan konektivitas private untuk basis data, API, dan konektor lainnya. Hasura berkomunikasi dengan sumber kita melalui jaringan private khusus, tanpa melewati internet publik.

### Deployment Private Hasura-Hosted (VPC)

Hasura mengelola penerapan, waktu aktif, dan ketersediaan *data plane* untuk kita.

Hasura-Hosted DDN tersedia di GCP, AWS, dan Azure. Ini dapat diterapkan di wilayah mana pun yang kita pilih. Kita dapat memilih beberapa wilayah untuk mengatur konfigurasi *distributed-edge* guna memberikan latensi terendah kepada klien kita.

*Data plane* beroperasi pada infrastruktur komputasi dan jaringan yang didedikasikan dan diisolasi, memastikan keamanan dan kepatuhan yang lebih baik untuk beban kerja kita. Infrastruktur ini diatur menggunakan tingkat isolasi tertinggi yang disediakan oleh penyedia cloud.

Konektivitas jaringan private tersedia melalui VPC Network Peering, Private Link / Private Service Connect, Transit Gateway, atau opsi konektivitas lain yang setara yang ditawarkan oleh penyedia cloud.

![image](https://github.com/user-attachments/assets/12218e5a-2aea-469d-af99-0788bd910c24)
  

### Arsitektur Private DDN yang Di-host oleh Hasura
Mulai sekarang, untuk memulai dengan Hasura DDN dalam mode penerapan VPC yang di-host oleh Hasura, hubungi bagian penjualan.

## Deployment Private Self-Hosted (BYOC)

Hasura DDN juga tersedia untuk di-host sendiri jika kita memerlukan kendali penuh atas infrastruktur.

Ada dua model penerapan dengan DDN yang di-host sendiri:

1. **Hasura mengelola *data plane* di akun cloud pelanggan**.

   *Data plane* akan berjalan di kluster Kubernetes pilihan kita dan Hasura mengelola waktu aktif, pembaruan, dll. *Control Plane* Hasura akan diberikan hak istimewa yang cukup untuk menginstal, mengonfigurasi, dan mengelola beban kerja serta komponen infrastruktur terkait. Pengalaman ini sama seperti menggunakan DDN yang di-host oleh Hasura, tetapi berjalan di infrastruktur kita. Semua lalu lintas API tetap berada di dalam jaringan kita dan tidak keluar dari batas sistem kita.

2. **Pelanggan mengelola *data plane* di akun cloud mereka sendiri**.

   *Data plane* dapat diinstal di kluster Kubernetes pilihan kita dan kita mengontrol waktu aktif, pembaruan, dll. Semua lalu lintas API tetap berada di dalam jaringan kita dan tidak keluar dari batas sistem kita.

Dalam kedua kasus, *control plane* selalu di-host oleh Hasura.  

![image](https://github.com/user-attachments/assets/f2db41f6-da94-4981-9ea5-1c2cd367617b)


### Aliran Data dan Keamanan
Semua operasi data yang penting terjadi secara eksklusif di dalam infrastruktur pelanggan. Ketika pengguna API mengirimkan permintaan kueri GraphQL, itu diterima oleh Hasura di *Data Plane*. Hasura kemudian secara langsung mengakses sumber data yang diperlukan di dalam infrastruktur pelanggan untuk memenuhi permintaan. Akses langsung ini memastikan bahwa data sensitif tidak pernah meninggalkan lingkungan yang dikendalikan oleh pelanggan, menjaga ketahanan data dan keamanan.

Sementara *Control Plane*, yang terletak di infrastruktur Hasura, berkomunikasi dengan *Data Plane*, komunikasi ini secara ketat terbatas pada konfigurasi dan tujuan manajemen. Pentingnya, komunikasi ini tidak melibatkan data pelanggan atau respons API, yang semakin meningkatkan keamanan data.

Pemisahan antara *Control Plane* dan *Data Plane* menciptakan batas keamanan yang terdefinisi dengan baik. Dengan menjaga *Data Plane* dan semua sumber data di dalam perimeter keamanan pelanggan, arsitektur memastikan bahwa informasi sensitif selalu tetap di bawah kendali pelanggan.

### Interaksi dengan *Control Plane*
Dalam Hasura DDN yang di-host sendiri, *Data Plane* yang berjalan di infrastruktur kita berkomunikasi dengan *Control Plane* hanya dalam skenario yang sangat spesifik:

- *Data Plane* mengakses bucket penyimpanan metadata untuk mengambil artefak build yang diperlukan untuk memproses permintaan API.
- *Data Plane* mengakses API *Control Plane* untuk mengambil informasi tentang build (yang diterapkan) untuk keperluan routing.
- (Opsional) *Data Plane* mengirimkan data observabilitas ke *Control Plane* sehingga kita dapat memvisualisasikannya di konsol; itu tidak berisi data respons API atau variabel.
- *Control Plane* berinteraksi dengan kluster Kubernetes untuk mengelola beban kerja *Data Plane* (hanya dalam *Data Plane* yang dikelola Hasura).

![image](https://github.com/user-attachments/assets/cfbb46df-3208-40be-bf2a-073f9c24364e)


## Kolaborasi Data Plane di DDN

Fitur Kolaborasi *Data Plane* memungkinkan kita mengelola dan memfasilitasi kolaborasi di *Data Plane* di mana kita adalah pemiliknya. Pemilik *Data Plane* dapat mengundang pengguna lain untuk berkolaborasi/membuat proyek di *Data Plane*. Pengguna yang diundang dapat membuat dan mengelola proyek di *Data Plane*. Fitur ini memungkinkan pengguna untuk mengundang, menerima, menolak, dan menghapus kolaborator untuk kolaborasi lintas tim.

### Cara Memulai Kolaborasi *Data Plane*
- **Mengundang Kolaborator**: Untuk mengundang pengguna ke *Data Plane* kita, buka *Data Plane Management Dashboard*. Dasbor akan menampilkan semua *data plane* yang tersedia. Pilih *data plane* di mana kita memiliki peran pemilik. Klik *Invite Collaborator*. Masukkan alamat email pengguna yang ingin kita undang dan klik *Send Invites*. Pengguna yang diundang akan menerima email dengan tautan undangan.

![image](https://github.com/user-attachments/assets/8e5f3b6c-9216-4671-8a87-2e01337d3bb3)

  
- **Menerima atau Menolak Undangan**: Pengguna yang diundang dapat menerima atau menolak undangan dengan mengklik tautan undangan yang diterima melalui email atau pergi ke *Data Plane Management Dashboard*. Dasbor akan menampilkan semua undangan yang diterima oleh pengguna. Pengguna dapat menerima atau menolak undangan dengan mengklik *Accept* atau *Decline*.

![image](https://github.com/user-attachments/assets/54ba7c4f-d706-45e9-9636-06df29b8946b)


- **Menghapus Kolaborator**: Kita dapat menghapus kolaborator *data plane* dengan membuka *Data Plane Management Dashboard*. Pilih *data plane* di mana kita memiliki peran pemilik, dan kita akan melihat semua kolaborator *data plane*. Klik *Remove* untuk menghapus kolaborator.

![image](https://github.com/user-attachments/assets/766e74bd-2154-4f29-ac5d-cd8280d4bac7)


### Izin Kolaborasi *Data Plane*
Saat ini hanya ada dua peran yang tersedia untuk kolaborasi *data plane*:

- **Owner**: Pemilik *data plane* memiliki akses penuh ke *data plane* dan dapat mengundang atau menghapus kolaborator.
- **Member**: Anggota *data plane* dapat membuat proyek di *data plane* dan mengelolanya.

### Apa yang Dapat Dilakukan Kolaborator?
Kolaborator dapat membuat proyek di *data plane* dan mengelolanya. Untuk membuat proyek di *data plane*, kolaborator perlu memilih *data plane* saat membuat proyek menggunakan Hasura DDN CLI.

```bash
ddn project create --data-plane-id <data-plane-id> --plan <plan>
```

  
