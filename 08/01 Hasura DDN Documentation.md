# INTRODUCTION  
Hasura DDN (*Data Delivery Network*) adalah platform yang mempermudah pengembangan dan pengelolaan API data modern, terutama yang bersifat terfederasi (menggabungkan data dari berbagai sumber). Dengan alat yang kuat dan alur kerja berbasis kode, Hasura DDN menyederhanakan akses dan integrasi data, menjadikan pengelolaan API lebih efisien dan mudah. Fitur utamanya termasuk kemudahan dalam menghubungkan berbagai sumber data, otomatisasi, dan peningkatan performa API, sehingga mempercepat siklus pengembangan aplikasi tanpa memerlukan banyak konfigurasi manual.

# BASICS  
Hasura DDN memudahkan penghubungan sumber data ke Hasura Engine menggunakan standar metadata fleksibel dan konektor data native. Arsitektur ini, disebut *data supergraph*, memungkinkan pendekatan berbasis kode penuh dengan API yang langsung tersedia.
  
Dengan Hasura DDN, kita bisa dengan mudah menggabungkan data dari berbagai sumber, mengatasi silo data. DDN memberikan visibilitas jelas terhadap kueri, kontrol keamanan yang kuat, dan sistem yang andal. Alur kerja pengembangan jadi lebih efisien dengan alat CI/CD, performa optimal lewat kueri native, sambil memastikan keamanan data dan logika bisnis tetap terpusat.

## Background  
Hasura DDN dibangun untuk mengatasi hambatan umum dalam pengembangan aplikasi modern, terutama dalam menyediakan akses data yang aman dan efisien. Dengan arsitektur *data supergraph*, Hasura DDN meningkatkan produktivitas tim dan developer dengan menyederhanakan pengelolaan API dan federasi data.

Beberapa tantangan yang diatasi Hasura DDN adalah:
- Pengembangan produk yang lambat.
- Kesulitan dalam menggabungkan sumber data dan komposisi API.
- Kompleksitas dan penyebaran layanan mikro yang berlebihan.
- Tata kelola yang tidak terstruktur dan kolaborasi yang sulit.
- Keamanan yang rumit.
- Kinerja yang terganggu dan sulit ditingkatkan.
- Kurangnya visibilitas kueri.
- Beragam kebutuhan konsumen API.
  
**Untuk Pengembang API**, Hasura DDN menawarkan:
- Pemodelan domain kaya fitur untuk data kompleks.
- Komposisi data yang mudah dari berbagai sumber.
- Pengaturan keamanan yang mudah dipahami.
- Pengalaman developer yang dioptimalkan.
- Fleksibilitas dalam pemilihan bahasa pemrograman.
  
**Untuk Tim Pemelihara API**, Hasura DDN memprioritaskan:
- Efisiensi kinerja dan operasional.
- Peluncuran tanpa gangguan.
- CI/CD federasi untuk kemandirian tim.
- Topologi terdistribusi untuk skalabilitas global.
- Onboarding cepat dan penemuan domain data.
  
**Untuk Konsumen API**, Hasura DDN memberikan:
- API yang andal dan terdokumentasi.
- Konsistensi pengambilan data dari berbagai sumber.
- Kemudahan dalam menemukan dan mencoba API.
  
Hasura DDN mempercepat pengembangan API modern yang lebih cepat, kuat, fleksibel, dan mudah diskalakan.

## Core Concepts  
Perkembangan microservices dan API yang dibutuhkan oleh sebuah produk menyebabkan tantangan baru bagi pengembang dan arsitek. Di era modern ini, lapisan data semakin kompleks, bukan hanya satu database, tetapi koleksi dari berbagai database, layanan, dan API.
  
Hasura DDN memperkenalkan konsep supergraph data untuk membantu mengelola kompleksitas ini.
  
### Kondisi Lapisan Data  
Bayangkan beberapa tim dalam sebuah perusahaan bekerja dengan sistem yang berbeda (basis data, API, dll.). Misalnya:  
- **Product Management Team** mengelola basis data relasional untuk data produk.
- **User Experience Team** menggunakan basis data MongoDB untuk informasi pengguna.
- **Search Optimization Team** bekerja dengan layanan pencarian seperti Algolia.
- **Finance and Transactions Team** menangani pembayaran melalui Stripe.
- **Logistics and Shipping Team** mengelola pengiriman melalui layanan seperti ShipStation.
- **Data Science and Recommendations Team** menggunakan basis data vektor (seperti Weaviate) untuk saran yang dipersonalisasi.
  
Setiap tim ini menggunakan API, skema, dan metode yang berbeda. Fragmentasi ini menyulitkan pengembangan aplikasi dan membutuhkan keterlibatan arsitek data yang ahli untuk mengintegrasikan dan mengelola sumber data yang beragam tersebut.
  
### Subgraph  
Setiap layanan yang digunakan dalam contoh aplikasi e-commerce ini disebut **subgraph**. Subgraph adalah lebih dari sekadar sumber data, ini mencakup semua metadata yang dibutuhkan untuk beroperasi secara mandiri dan kemudian terhubung sebagai bagian dari API terpadu.

Untuk pengembang API, ini berarti mereka bisa membangun API dari berbagai sumber secara sederhana dan deklaratif. Setiap subgraph juga dapat menggunakan aturan akses, mekanisme autentikasi, dan logika bisnis khusus untuk menciptakan API yang aman dan tangguh.

### Supergraph  
**Supergraph** adalah kerangka kerja yang menghubungkan semua subgraph menjadi satu API yang aman. Ini memungkinkan aplikasi untuk menggunakan berbagai sumber data, layanan, API pihak ketiga, dan logika bisnis dalam satu lapisan yang terpadu.

Manfaatnya bagi tim pengelola data adalah mereka hanya perlu mengelola satu API terpadu daripada banyak sumber data yang berbeda. Bagi konsumen API, mereka hanya perlu mengakses satu endpoint untuk semua data yang dibutuhkan, sehingga aplikasi mereka menjadi lebih sederhana.

### Cara Membangun Subgraph dan Supergraph  
Untuk membangun subgraph, pengembang cukup menghubungkan sumber data mereka menggunakan konektor data. Hasura DDN menyediakan konektor data untuk berbagai layanan populer, atau kita dapat membuat konektor sendiri. Ketika subgraph sudah terhubung, Hasura secara otomatis membangun supergraph dari semua sumber data yang ada, memungkinkan hubungan antar sumber data serta kontrol akses yang fleksibel.

Dengan menggunakan supergraph, organisasi bisa mengurangi kerumitan infrastruktur data dan meningkatkan kolaborasi antar tim.

## Sebelum Hasura DDN
Saat bisnis berusaha memodernisasi diri, mereka menghadapi tantangan dalam menyampaikan pengalaman digital secara cepat dan efisien. Tim pengembangan produk sering kali terhambat oleh tim backend yang harus menyediakan akses data yang aman dan efisien. Hal ini membuat mereka terpaksa membuat API kustom secara manual, memicu pertumbuhan berlebih pada microservice, mengatur kebijakan keamanan yang rumit, sehingga banyak waktu terbuang dan inovasi menjadi terhambat.

![image](https://github.com/user-attachments/assets/be5e1a92-7aa6-46cc-bbf7-0cc579ad350b)


## Dengan Hasura DDN  
Dengan beralih ke arsitektur supergraph menggunakan Hasura, organisasi dapat mengurangi kerumitan infrastruktur data secara signifikan. Hasura DDN memungkinkan koneksi langsung ke berbagai sumber data menggunakan native data connectors (NDCs), sehingga kebutuhan untuk membuat API dan microservice secara manual menjadi sangat berkurang. Selain itu, kolaborasi antara tim backend dapat ditingkatkan karena mereka dapat bekerja secara independen.

Data dapat diakses dengan aman dan saling terhubung, memungkinkan pembuatan data graph terpadu yang dapat digunakan untuk berbagai kebutuhan, mulai dari aplikasi klien hingga API publik. Semua ini dapat diterapkan di infrastruktur global yang mendukung latensi minimal, skalabilitas tinggi, dan keandalan yang tidak tergoyahkan.

![image](https://github.com/user-attachments/assets/a9ba5b6d-70ef-4b94-89a7-a90ad8d1352d)


# GETTING STARTED    
Dalam beberapa langkah, kita bisa menghubungkan sumber data, mendefinisikan API, dan mulai membangun data supergraph. Alat Hasura yang kuat, seperti CLI dan konsol, memudahkan pembuatan dan pengelolaan supergraph sambil memastikan integrasi data yang mulus.

Baik menggunakan proyek sampel atau data kita sendiri, kita bisa langsung menjalankannya. Proses setup Hasura yang intuitif memungkinkan kita fokus pada pengembangan, bukan konfigurasi, sehingga kita dapat dengan cepat merilis pembaruan produk.

## Quickstart Supergraph API - Hasura DDN

### Prasyarat  

1. **Install DDN CLI**  
   Unduh CLI terbaru untuk Windows:  
   ```bash
   curl.exe -L https://graphql-engine-cdn.hasura.io/ddn/cli/v4/latest/cli-ddn-windows-amd64.exe -o ddn.exe
   ```
     
   Tambahkan executable ke PATH sistem agar bisa dijalankan dari terminal.

2. **Install Docker**  
   Pastikan Docker Compose versi 2.27.1 atau lebih baru sudah terpasang.
     
### Langkah-Langkah  

1. **Login ke CLI**
   Jalankan perintah berikut untuk autentikasi:

   ```bash
   ddn auth login
   ```
  
2. **Inisialisasi Supergraph**
   Buat direktori proyek dan inisialisasi supergraph:

   ```bash
   mkdir hasura_project && cd hasura_project
   ddn supergraph init .
   ```
  
3. **Hubungkan ke Sumber Data**
   Inisialisasi konektor data dengan memilih jenis konektor dan memasukkan variabel yang dibutuhkan:

   ```bash
   ddn connector init my_connector -i
   ```
  
4. **Introspeksi Sumber Data**
   Jalankan introspeksi sumber data:
   
   ```bash
   ddn connector introspect my_connector
   ```
   
5. **Tambahkan Sumber Daya**
   Buat metadata untuk model, command, dan relasi:
   
   ```bash
   ddn model add my_connector '*'
   ddn relationship add my_connector '*'
   ```
   
6. **Build Supergraph untuk Pengembangan Lokal**
   Buat build lokal supergraph:
   
   ```bash
   ddn supergraph build local
   ```
   
7. **Jalankan Supergraph**
   Mulai supergraph menggunakan Docker:
   ```bash
   ddn run docker-start
   ```
   
8. **Deploy ke Hasura DDN**
   Inisialisasi proyek di Hasura DDN:
   
   ```bash
   ddn project init
   ```
   
9. **Build dan Deploy Supergraph**
   Bangun dan deploy supergraph ke Hasura DDN:
   
   ```bash
   ddn supergraph build create
   ```

### Langkah Selanjutnya  
- **Integrasi Logika Bisnis**: Tambahkan logika khusus ke API menggunakan lambda connectors (TypeScript/Python).
- **Tambah Otorisasi**: Buat aturan akses untuk entitas di supergraph.
- **Iterasi API**: Lakukan pembaruan pada metadata Hasura saat ada perubahan pada schema sumber data.
- **Hubungkan Sumber Data**: Buat relasi antar sumber data untuk query efisien.
- **Tambah Subgraph Baru**: Mudah mengelola subgraph oleh tim berbeda dengan metadata terpisah.
  
## Eksplorasi Supergraph yang Sudah Ada - Hasura DDN  

Navigasi API sering kali terasa rumit, dari pengaturan lingkungan, visualisasi struktur API, hingga pemantauan performa. Tantangan yang dihadapi pengembang termasuk:

- **Pengaturan Manual**: Konfigurasi lingkungan API secara manual yang rentan terhadap kesalahan.
- **Kurangnya Visualisasi**: Sulit memahami struktur API tanpa alat visualisasi yang baik.
- **Keterbatasan Insight**: Pemantauan performa API memerlukan beberapa alat yang terpisah, yang bisa membuat proses terputus-putus.

Hasura menawarkan solusi dengan GUI yang mudah digunakan, disebut *console*.

### Apa itu console?
Console adalah GUI berbasis web yang memungkinkan tim untuk berinteraksi, memvisualisasikan, mengeksplorasi, dan memonitor API. Bisa digunakan baik pada instance lokal maupun proyek yang di-host.

![image](https://github.com/user-attachments/assets/5a2f74b0-d109-4ee1-b633-17a14c4a8677)


Console memudahkan pengujian query, memberikan feedback, mengeksplorasi API, serta mendiagnosa *error* atau *bottleneck*.

### Preparation Eksplorasi Supergraph

1. **Buat Akun atau Login**  
   Buat akun atau masuk ke akunmu di https://console.hasura.io.

2. **Buat Proyek atau Eksplorasi Proyek Contoh**  
   Kamu bisa mengikuti quickstart untuk membuat proyekmu sendiri, atau eksplorasi proyek contoh eCommerce Supergraph API yang dibangun di Hasura DDN. Cari proyek contoh "eCommerce App" di bagian *Sample Projects*.

Mulai gunakan *explorer* untuk mengeksplorasi lebih lanjut API melalui console.  
  
### Eksplorasi Supergraph - Hasura DDN  
Menjelajahi struktur dan hubungan API seringkali menjadi tantangan bagi pengembang. Mengandalkan alat eksternal terbatas dapat menghasilkan representasi API yang tidak lengkap atau tidak akurat. Dokumentasi manual memperumit proses, rentan terhadap kesalahan, dan memperlambat pengembangan.

Hasura menyediakan *console* — GUI berbasis web yang memiliki halaman *explorer* yang menawarkan:

- **Visualisasi Komprehensif**: Representasi real-time API, termasuk hubungan dan izin.
- **Dokumentasi Otomatis**: Dokumentasi API yang selalu akurat dan diperbarui otomatis.
- **Kolaborasi yang Ditingkatkan**: Alat visual untuk meningkatkan komunikasi tim.

Console juga memungkinkan interaksi dan pemantauan API secara langsung.

#### Langkah Eksplorasi Supergraph

##### Langkah 1. Eksplorasi Supergraph
Klik tombol *Explorer* di menu samping untuk melihat visualisasi keseluruhan supergraph. 

![image](https://github.com/user-attachments/assets/1569893a-6dc1-4386-b8b7-bda9ae24bdf9)


Kamu bisa melihat subgraph, sumber data, dan hubungan di antara mereka. Ada opsi untuk mengubah tampilan sesuai kebutuhan.

##### Langkah 2. Eksplorasi Subgraph
Klik salah satu subgraph untuk melihat rinciannya, seperti konektor, model, dan perintah yang ada di dalamnya.

![image](https://github.com/user-attachments/assets/9313bfae-50c5-496f-8073-5e2026099603)


##### Langkah 3. Eksplorasi Sumber Data
Jika terdapat banyak konektor di dalam subgraph, kamu bisa fokus pada satu konektor dan melihat model serta perintah terkaitnya.

![image](https://github.com/user-attachments/assets/0ff7f346-062a-42ca-b71b-710ee7c537aa)


##### Langkah 4. Eksplorasi Model atau Perintah
Console memungkinkan pencarian entitas berdasarkan nama. Misalnya, kamu bisa mengetik "Carts" dan melihat detail model yang terkait. 

![image](https://github.com/user-attachments/assets/7fae9899-2888-412f-8055-66b514eb09aa)


Dari sini, kamu dapat melihat visualisasi entitas, hubungan, izin, dan bahkan menjalankan contoh query.

##### Signature
Setiap model atau perintah memiliki *signature* yang menunjukkan input dan outputnya.

- **Model**: Biasanya tidak memiliki input dan mengembalikan tipe data model.
  - Contoh: `Carts() => Carts`
 
    ![image](https://github.com/user-attachments/assets/2403ec5d-9ddb-43d6-aeba-8aed6634027c)

  
- **Perintah**: Memiliki argumen input dan output berupa tipe data yang dikembalikan.
  - Contoh: `GetFruits() => [Fruit!]!`

    ![image](https://github.com/user-attachments/assets/2311f2d8-3a30-476f-8941-27acdde814d7)



##### Root Fields di GraphQL
Root fields menentukan bagaimana data dapat diakses melalui query GraphQL. Contoh untuk model Cart:

- **Select Many**: `app_carts() => Carts` - Untuk query beberapa entri cart.
- **Select Unique**: `app_cartsById(id: String!) => Carts` - Untuk query cart spesifik berdasarkan ID.

![image](https://github.com/user-attachments/assets/14380a42-2f96-4dc1-b983-f01f996f1120)


Contoh query:
```graphql
query {
  app_carts {
    id
    isComplete
  }
}

query {
  app_cartsById(id: "123") {
    id
    isComplete
  }
}
```  
  
Query pertama menggunakan kolom "Select Many" untuk mengambil beberapa keranjang, sedangkan kueri kedua menggunakan kolom "Select Unique" untuk mengambil keranjang tertentu berdasarkan ID.
  
##### Output Fields
Output fields menunjukkan struktur dan tipe data yang bisa diambil dari model atau perintah.

![image](https://github.com/user-attachments/assets/ae697c1f-333e-4565-9ce5-e10bed3fdd52)


##### Arguments
Argumen memungkinkan penyaringan, pengurutan, atau modifikasi data dalam query atau mutasi. Di Hasura, argumen membuat API lebih fleksibel.

![image](https://github.com/user-attachments/assets/9e4ebde9-c607-4eff-9933-59c6b58dafbe)


##### Relationships
Hubungan menunjukkan bagaimana model terhubung dengan model lainnya. Misalnya, model Cart memiliki hubungan dengan pengguna melalui field userId yang terhubung dengan id di tabel Users.

![image](https://github.com/user-attachments/assets/13daa706-3c15-4fbc-b8d7-f3ec5870bc25)


##### Permissions
Izin mendefinisikan kontrol akses untuk model atau perintah. Misalnya, pengguna dengan peran admin dapat melihat informasi cart tetapi tidak bisa membuat, mengubah, atau menghapus cart.

![image](https://github.com/user-attachments/assets/ed5e16cd-51ee-4f5d-85dc-3458c95f8f6c)


### Berinteraksi dengan Supergraph Anda  
Sekarang, kamu bisa mulai berinteraksi dan menguji API menggunakan GraphiQL explorer.Berinteraksi dengan API seharusnya menjadi tugas yang sederhana, namun sering kali menjadi rumit dan membingungkan. Developer sering harus berpindah antar berbagai alat untuk menguji query, menambahkan header, dan melacak permintaan. Pendekatan ini tidak hanya memperlambat alur kerja, tetapi juga tidak efisien dalam proses debugging. Tanpa tracing terintegrasi, mengidentifikasi dan menyelesaikan masalah menjadi sangat memakan waktu.

Kurangnya umpan balik secara langsung juga berarti bahwa setiap perubahan pada API atau query memerlukan verifikasi manual, yang semakin memperlambat pengembangan. Menyadari masalah ini, Hasura DDN menyediakan solusi komprehensif melalui GraphiQL explorer, dirancang untuk menyederhanakan dan mempercepat proses interaksi API.

Dengan Hasura DDN, Anda dapat menggunakan GraphiQL explorer untuk membuat dan menguji query langsung dalam console, dengan fitur berikut:

- **Definisi Skema**: Melihat seluruh skema GraphQL dan dokumentasi untuk semua query dan mutasi yang tersedia.
- **Pengujian Query Terintegrasi**: Menguji query dengan header dan melihat hasil secara real-time, semuanya dalam satu tempat.
- **Tracing Terbina**: Melacak query dengan cepat untuk mengidentifikasi hambatan kinerja dan mengoptimalkan secara efisien.
- **Alur Kerja Terpadu**: Menghemat waktu dan mengurangi perpindahan antar alat dengan semua yang Anda butuhkan dalam satu antarmuka.

  
