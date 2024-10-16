# INTRODUCTION  
Hasura DDN (*Data Delivery Network*) adalah platform yang mempermudah pengembangan dan pengelolaan API data modern, terutama yang bersifat terfederasi (menggabungkan data dari berbagai sumber). Dengan alat yang kuat dan alur kerja berbasis kode, Hasura DDN menyederhanakan akses dan integrasi data, menjadikan pengelolaan API lebih efisien dan mudah. Fitur utamanya termasuk kemudahan dalam menghubungkan berbagai sumber data, otomatisasi, dan peningkatan performa API, sehingga mempercepat siklus pengembangan aplikasi tanpa memerlukan banyak konfigurasi manual.

# BASICS  
Hasura DDN memudahkan penghubungan sumber data ke Hasura Engine menggunakan standar metadata fleksibel dan konektor data native. Arsitektur ini, disebut *data supergraph*, memungkinkan pendekatan berbasis kode penuh dengan API yang langsung tersedia.
  
Dengan Hasura DDN, Anda bisa dengan mudah menggabungkan data dari berbagai sumber, mengatasi silo data. DDN memberikan visibilitas jelas terhadap kueri, kontrol keamanan yang kuat, dan sistem yang andal. Alur kerja pengembangan jadi lebih efisien dengan alat CI/CD, performa optimal lewat kueri native, sambil memastikan keamanan data dan logika bisnis tetap terpusat.

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
Untuk membangun subgraph, pengembang cukup menghubungkan sumber data mereka menggunakan konektor data. Hasura DDN menyediakan konektor data untuk berbagai layanan populer, atau Anda dapat membuat konektor sendiri. Ketika subgraph sudah terhubung, Hasura secara otomatis membangun supergraph dari semua sumber data yang ada, memungkinkan hubungan antar sumber data serta kontrol akses yang fleksibel.

Dengan menggunakan supergraph, organisasi bisa mengurangi kerumitan infrastruktur data dan meningkatkan kolaborasi antar tim.

## Sebelum Hasura DDN
Saat bisnis berusaha memodernisasi diri, mereka menghadapi tantangan dalam menyampaikan pengalaman digital secara cepat dan efisien. Tim pengembangan produk sering kali terhambat oleh tim backend yang harus menyediakan akses data yang aman dan efisien. Hal ini membuat mereka terpaksa membuat API kustom secara manual, memicu pertumbuhan berlebih pada microservice, mengatur kebijakan keamanan yang rumit, sehingga banyak waktu terbuang dan inovasi menjadi terhambat.

![image](https://github.com/user-attachments/assets/be5e1a92-7aa6-46cc-bbf7-0cc579ad350b)


## Dengan Hasura DDN  
Dengan beralih ke arsitektur supergraph menggunakan Hasura, organisasi dapat mengurangi kerumitan infrastruktur data secara signifikan. Hasura DDN memungkinkan koneksi langsung ke berbagai sumber data menggunakan native data connectors (NDCs), sehingga kebutuhan untuk membuat API dan microservice secara manual menjadi sangat berkurang. Selain itu, kolaborasi antara tim backend dapat ditingkatkan karena mereka dapat bekerja secara independen.

Data dapat diakses dengan aman dan saling terhubung, memungkinkan pembuatan data graph terpadu yang dapat digunakan untuk berbagai kebutuhan, mulai dari aplikasi klien hingga API publik. Semua ini dapat diterapkan di infrastruktur global yang mendukung latensi minimal, skalabilitas tinggi, dan keandalan yang tidak tergoyahkan.

![image](https://github.com/user-attachments/assets/a9ba5b6d-70ef-4b94-89a7-a90ad8d1352d)

