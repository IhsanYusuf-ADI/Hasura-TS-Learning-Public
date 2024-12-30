# Federated Data Access Layer

## Apa itu Federated Data Access Layer (FDAL)?
Federated Data Access Layer (FDAL) adalah lapisan akses data yang memungkinkan akses yang fleksibel dan aman ke berbagai sumber data yang dikelola secara independen.

---

## Konsumen Umum FDAL
Konsumen FDAL mencakup berbagai kebutuhan, mulai dari akses data realtime dengan latensi rendah dan kapasitas tinggi hingga akses data analitik. Berikut adalah kategori konsumen FDAL:

### Produk dan Layanan di Lapisan Pengalaman
- **BFFs (Backend for Frontends)**
- **Produk & layanan internal** seperti pelaporan dan Business Intelligence (BI)
- **Pengembang pihak ketiga**
- **Aplikasi AI dan LLMs (Large Language Models)** yang sedang berkembang

---

## Manfaat FDAL
### 1. Menurunkan Biaya dan Meningkatkan Efisiensi
- Menghindari pemindahan data di berbagai platform dan lapisan (terutama ETL yang prematur).
- Meningkatkan konsistensi dan kualitas data.
- Mencegah duplikasi data.

### 2. Meningkatkan Kepatuhan dan Tata Kelola
- Menerapkan model otorisasi yang konsisten, dan jika diperlukan, terpusat.
- Memberikan akses realtime ke data segera setelah data dibuat.
- Membuat API standar untuk berbagai jenis beban kerja.

### 3. Meningkatkan Produktivitas dan Kolaborasi
- Memberikan API yang stabil untuk memisahkan lapisan penyimpanan dan pemodelan fisik dari pemodelan domain.
- Menyediakan model operasional untuk mengintegrasikan logika bisnis lintas domain ("process layer" atau "orchestration layer").
- Mendefinisikan harapan performa & stabilitas sebagai SLA.
- Mengintegrasikan dinamika pasar: analitik penggunaan dan kolaborasi produser/konsumen untuk meningkatkan sumber data yang mendasarinya.

---

## Checklist Platform Supergraph
Ada 6 aspek utama untuk membangun FDAL dengan referensi arsitektur supergraph. Platform supergraph dan stack teknologinya harus mendukung kemampuan berikut:

### I. Model Semantik Terpadu
Model semantik terpadu untuk memahami berbagai entitas di dalam supergraph (sumber daya, metode bisnis, kebijakan otorisasi).

### II. Pemodelan Data & Logika Bisnis
- **Subgraph connectors** memungkinkan pemodelan domain atau API dengan memanfaatkan bahasa sumber data yang mendasarinya.
- Menambahkan logika bisnis untuk transformasi, validasi, dan otorisasi.

Subgraph harus cukup **"tipis"** sehingga karakteristik performanya ditentukan oleh sumber data yang mendasarinya secara prediktif, dan cukup **"tebal"** untuk mengintegrasikan logika bisnis yang diperlukan.

### III. Otorisasi
Mesin kebijakan otorisasi yang mendukung:
- Komposisi aturan/kebijakan.
- Kontrol akses tingkat kolom (field/column level visibility).
- Kontrol akses tingkat entitas (row/entity level access control).
- Integrasi optimal dengan pengambilan data (misalnya: projection & predicate push-down).

### IV. Desain API
Desain API standar yang dapat menangani:
#### Beban Kerja yang Didorong Domain:
- Querying sumber daya & memanggil metode bisnis.
- Operasi filter, sorting, pagination, & agregasi pada sumber daya.
- Konvensi untuk beban kerja relational, event, KV, dan graph.
#### Kebutuhan API di Lapisan Agregasi & Orkestrasi:
- Penggabungan data (joining data).
- Filter bersarang, sorting, pagination, & agregasi.
- Logika bisnis orkestrasi yang memerlukan querying sumber daya atau metode dari beberapa domain.

### V. Performa & Observabilitas
#### Perencanaan Query:
- Visibilitas/penjelasan di lingkungan pengembangan (development, staging).
- Tracing di produksi.
- Optimisasi spesifik sumber data untuk mengurangi beban pada sumber data upstream.

#### Pajak Latensi:
- Mengukur dan mengoptimalkan latensi tambahan di atas sumber data yang mendasarinya.

### VI. Kepemilikan Terfederasi
- CI/CD independen untuk pemilik domain (sumber data).
- Komposabilitas (agregasi, orkestrasi) lintas domain.
- Alat analitik & kolaborasi untuk produsen dan konsumen.

---
