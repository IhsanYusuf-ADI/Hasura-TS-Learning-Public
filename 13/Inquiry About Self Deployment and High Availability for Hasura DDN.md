# Supergraph Manifesto

Supergraph adalah kerangka arsitektur yang menawarkan:
- **Arsitektur Referensi:** Panduan tentang bagaimana merancang sistem menggunakan Supergraph.
- **Prinsip Desain:** Panduan desain untuk membangun sistem yang efisien.
- **Model Operasional:** Cara kerja yang mendukung kolaborasi antar tim dalam platform mandiri (self-serve) untuk akses data terfederasi, integrasi API, atau komposisi API GraphQL.

Artefak implementasi dari arsitektur Supergraph ini disebut **supergraph** (menggunakan huruf kecil "s").

---

## Sebelum dan Sesudah Supergraph

Ketika supergraph dibangun menggunakan tumpukan federasi GraphQL:
- Mesin (engine) sering disebut sebagai **gateway** atau **router**.
- Konektor subgraph biasanya berupa layanan GraphQL.

---

## Penggunaan Supergraph
Supergraph umumnya digunakan untuk dua kasus utama:

### 1. **Self-Serve API Composition Platform**
   - Model operasional yang memungkinkan tim untuk mengintegrasikan, mengorkestrasi, dan mengagregasi API secara mandiri (self-serve).

### 2. **Federated Data Access Layer**
   - Lapisan data terfederasi yang menyediakan akses real-time ke sumber data dengan kemampuan komposisi lintas domain (seperti joins, filtering, dll).
   - Berkaitan dengan konsep seperti **Data Mesh** dan **Produk Data**.

# Supergraph Manifesto

## Apa itu Supergraph?
Supergraph adalah kerangka arsitektur yang menawarkan:
- **Arsitektur Referensi:** Panduan tentang bagaimana merancang sistem menggunakan Supergraph.
- **Prinsip Desain:** Panduan desain untuk membangun sistem yang efisien.
- **Model Operasional:** Cara kerja yang mendukung kolaborasi antar tim dalam platform mandiri (self-serve) untuk akses data terfederasi, integrasi API, atau komposisi API GraphQL.

Artefak implementasi dari arsitektur Supergraph ini disebut **supergraph** (menggunakan huruf kecil "s").

---

## Sebelum dan Sesudah Supergraph

Ketika supergraph dibangun menggunakan tumpukan federasi GraphQL:
- Mesin (engine) sering disebut sebagai **gateway** atau **router**.
- Konektor subgraph biasanya berupa layanan GraphQL.

---

## Penggunaan Supergraph
Supergraph umumnya digunakan untuk dua kasus utama:

### 1. **Platform Komposisi API Mandiri**
   - Model operasional yang memungkinkan tim untuk mengintegrasikan, mengorkestrasi, dan mengagregasi API secara mandiri (self-serve).

### 2. **Lapisan Akses Data Terfederasi**
   - Lapisan data terfederasi yang menyediakan akses real-time ke sumber data dengan kemampuan komposisi lintas domain (seperti joins, filtering, dll).
   - Berkaitan dengan konsep seperti **Data Mesh** dan **Produk Data**.

---

# Strategi dan Konsep Inti
Pendekatan supergraph bertujuan untuk membangun "flywheel" akses dan pasokan data guna meningkatkan akses mandiri (self-service) ke data dan API secara bertahap.

## Flywheel Platform Supergraph

![image](https://github.com/user-attachments/assets/99f0d7aa-b555-44aa-bc4f-f32f249501ca)


### I. **CONNECT Domains**
Pemilik domain (atau pemilik data, atau produsen API) harus dapat menghubungkan domain mereka ke platform dengan mudah. Tantangan utama dalam membangun supergraph adalah resistensi terhadap perubahan dari pemilik domain, yang sering kali tidak ingin harus membangun, mengoperasikan, dan memelihara lapisan API tambahan seperti GraphQL server. Kekhawatiran ini valid dan harus ditangani dengan strategi platform supergraph dan arsitektur referensi supergraph.

Dua implikasi utama untuk siklus hidup dan runtime konektor subgraph:

1. **CI/CD Konektor Subgraph**:
   - Ketika pemilik domain mengubah domain mereka, kontrak API yang diterbitkan melalui supergraph engine harus tetap sinkron dengan overhead minimal bagi pemilik domain.
   - Proses SDLC, manajemen perubahan, atau CI/CD pemilik domain harus mencakup pembaruan kontrak API mereka (misalnya: versioning), mencegah perubahan yang merusak, dan menjaga dokumentasi tetap mutakhir.

2. **Performa Konektor Subgraph**:
   - Konektor subgraph tidak boleh mengurangi performa dibandingkan dengan akses langsung ke domain yang mendasarinya.
   - Karakteristik performa API diukur melalui latensi, ukuran payload, dan konkurensi.

Menjamin proses CI/CD yang lancar dan konektivitas berkinerja tinggi memberikan kepercayaan kepada pemilik domain bahwa mereka dapat menghubungkan domain mereka ke platform supergraph dan mengubah domain mereka tanpa rasa takut.

Ini membuka konektivitas mandiri bagi pemilik domain.

### II. **CONSUME APIs**
Konsumen API harus dapat menemukan dan menggunakan API tanpa memerlukan integrasi manual, agregasi, atau komposisi API sebanyak mungkin. Konsumen API memiliki beberapa kebutuhan umum saat menangani endpoint API tetap atau kueri data tertentu:

- Mengambil proyeksi data yang berbeda untuk mencegah over-fetching.
- Menggabungkan data dari berbagai tempat untuk mencegah under-fetching.
- Memfilter, mempaginasi, mengurutkan, dan mengagregasi data dari berbagai tempat.

Untuk memberikan pengalaman API yang benar-benar mandiri, ada dua persyaratan utama:

1. **Desain API yang Dapat Dikomposisi**:
   - API yang disajikan oleh supergraph engine harus memungkinkan komposisi sesuai permintaan. GraphQL adalah API yang hebat untuk mengekspresikan semantik komposisi, tetapi terlepas dari format API yang digunakan, desain API yang terstandar dan dapat dikomposisi adalah persyaratan kritis.

2. **Portal API**:
   - Pencarian, penemuan, dan dokumentasi berkualitas tinggi untuk API dan model API yang mendasarinya sangat penting untuk memungkinkan konsumsi mandiri.
   - Semakin banyak informasi yang tersedia bagi konsumen API, semakin baik. Contohnya: Data lineage, kebijakan otorisasi, dll.

Ini membuka konsumsi mandiri bagi konsumen API.

### III. **DISCOVER Demand**
Memahami bagaimana konsumen API menggunakan domain mereka dan mengidentifikasi kebutuhan yang belum terpenuhi sangat penting bagi produsen API. Wawasan ini memungkinkan produsen API untuk meningkatkan domain mereka dan menemukan pemilik domain baru untuk menghubungkan domain mereka ke supergraph.

Hal ini memerlukan dua kemampuan utama dari platform supergraph untuk menciptakan budaya yang berpusat pada konsumen dan gesit:

1. **Analitik Konsumsi API, Skema API, dan Portal**:
   - Supergraph mirip dengan pasar (marketplace) dan perlu memberikan wawasan kepada pemilik dan produsen pasar untuk membantu meningkatkan pasar bagi konsumen.

2. **Integrasi Ekosistem**:
   - Platform supergraph harus dapat berintegrasi dengan alat komunikasi dan katalog yang ada, terutama untuk membantu memahami permintaan yang tidak terpenuhi dari konsumen API.

Ini menutup siklus dan memungkinkan platform supergraph menciptakan siklus keberhasilan yang berkelanjutan bagi produsen dan konsumen.

# Manifesto Supergraph

## Panduan Arsitektur

### Sistem CI/CD dan Build (Control Plane)
Control plane dari supergraph sangat penting untuk memungkinkan pemilik domain menghubungkan domain mereka ke supergraph dengan lancar. Sistem ini memastikan bahwa domain tetap terintegrasi dan tersinkronisasi saat terjadi perubahan.

#### Komponen pada Control Plane
Terdapat tiga komponen utama dalam control plane supergraph:

1. **Domain Itu Sendiri**
   - Merupakan sumber utama data atau API.

2. **Subgraph**
   - Berfungsi sebagai konektor perantara yang menghubungkan domain dengan supergraph.

3. **Supergraph**
   - Lapisan integrasi utama yang menggabungkan beberapa subgraph untuk menyediakan platform akses data terpadu dan komposisi API.

#### Komponen dalam Control Plane Supergraph
Untuk menjaga sinkronisasi antara supergraph dan domain yang mendasarinya, control plane harus mendefinisikan dan mengelola proses Software Development Life Cycle (SDLC) berikut:

- **Kontrol Versi**: Untuk melacak perubahan dan memastikan kompatibilitas ke belakang.
- **Continuous Integration dan Continuous Deployment (CI/CD)**: Mengotomatiskan pembaruan pada subgraph saat domain berkembang.
- **Pengujian dan Validasi**: Untuk mencegah perubahan yang merusak dan menjaga kontrak API tetap berkualitas tinggi.

### Data Plane Terdistribusi
Data plane terdistribusi dari supergraph sangat penting untuk memberikan akses berperforma tinggi ke domain upstream. Hal ini memastikan bahwa:

- **Produsen API** dapat memelihara domain mereka secara efisien tanpa beban pemeliharaan tak terduga di masa depan.
- **Metrik Performa** seperti latensi, ukuran payload, dan konkurensi dioptimalkan untuk pengguna akhir.

Dengan menyeimbangkan konektivitas, performa, dan skalabilitas, data plane terdistribusi mendukung operasi supergraph yang lancar, menjadikannya arsitektur yang kokoh dan berkelanjutan.


# Supergraph Manifesto

## Panduan Desain Skema API

### Standardisasi

Desain skema API Supergraph harus menciptakan konvensi standar pada atribut berikut:

#### Atribut Standardisasi dan Kemampuannya

| Atribut Standardisasi | Kemampuan |
|------------------------|-----------|
| **S1** | Memisahkan model (resources) dan perintah (methods) |

**Contoh:**
- *Model* adalah kumpulan data yang dapat di-query secara standar tanpa tergantung sumber data tertentu.
- *Perintah* adalah metode yang terkait dengan logika bisnis tertentu dan dapat mengembalikan referensi ke model atau perintah lainnya.

```graphql
# Cara standar untuk mengambil daftar penulis
query GetAuthors {
  authors {
    id
    name
  }
}

# Metode spesifik untuk mencari penulis
query findAuthors {
  search_authors(args: {search: "Einstein"}) {
    id
    name
  }
}
```

| **S2** | Penyaringan model |

**Contoh:**
Mengambil daftar artikel yang diterbitkan tahun ini:
```graphql
query articlesThisYear {
  articles(where: {publishDate: {_gt: "2024-01-01"}}) {
    id
    name
  }
}
```

| **S3** | Pengurutan model |

**Contoh:**
Mengambil daftar artikel yang diurutkan berdasarkan tanggal publikasi secara terbalik:
```graphql
query sortedArticles {
  article(order_by: {publishDate: desc}) {
    id
    title
    author_id
  }
}
```

| **S4** | Paginasi model |

**Contoh:**
Melakukan paginasi pada daftar dengan 20 objek per halaman dan mengambil halaman ke-3:
```graphql
query sortedArticlesThirdPage {
  article(order_by: {publishDate: desc}, offset: 40, limit: 20) {
    id
    title
    author_id
  }
}
```

| **S5** | Agregasi model pada bidang tertentu |

**Contoh:**
Mengambil jumlah penulis dan rata-rata usia mereka:
```graphql
query authorStatistics {
  author_aggregate {
    aggregate {
      count # dukungan agregasi dasar pada model apa pun
      avg { # dukungan pada bidang numerik
        age
      }
    }
  }
}
```

#### Referensi Sebelumnya
- **Panduan Desain API Google Cloud:**
  - *Resource*: API yang berorientasi pada sumber daya dimodelkan sebagai hierarki sumber daya.
  - *Method*: Sumber daya dimanipulasi melalui sekumpulan metode kecil.

---

### Komposabilitas

API Supergraph biasanya adalah API GraphQL/JSON. Berbagai tingkat komposabilitas API dijelaskan di tabel berikut:

#### Atribut Komposabilitas dan Kemampuannya

| Atribut Komposabilitas | Kemampuan | Deskripsi |
|-------------------------|-----------|-----------|
| **C1** | Menggabungkan data | Menggabungkan data terkait seperti "foreign key" join |

**Contoh:**
Mengambil daftar penulis dan artikel mereka:
```graphql
query authorWithArticles {
  author {
    id
    name
    articles {
      id
      title
    }
  }
}
```

| **C2** | Penyaringan bersarang | Menyaring induk berdasarkan properti entitas anak |

**Contoh:**
Mengambil daftar penulis yang telah menerbitkan artikel tahun ini:
```graphql
query recentlyActiveAuthors {
  author(where: {articles: {publishDate: {_gt: "2024-01-01"}}}) {
    id
    name
  }
}
```

| **C3** | Pengurutan bersarang | Mengurutkan induk berdasarkan properti entitas anak |

**Contoh:**
Mengambil daftar artikel yang diurutkan berdasarkan nama penulisnya:
```graphql
query sortedArticles {
  article(order_by: {author: {name: asc}}) {
    id
    title
  }
}
```

| **C4** | Paginasi bersarang | Mengambil daftar induk dan anak dengan paginasi |

**Contoh:**
Mengambil halaman kedua daftar penulis dan halaman pertama artikel mereka yang diurutkan berdasarkan judul:
```graphql
query paginatedAuthorsWithSortedPaginatedArticles {
  author(offset: 10, limit: 20) {
    id
    name
    articles(offset: 0, limit: 25, order_by: {title: asc}) {
      title
      publishDate
    }
  }
}
```

| **C5** | Agregasi bersarang | Mengagregasi anak/induk dalam konteks induk/anak |

**Contoh:**
Mengambil daftar penulis dan jumlah artikel yang ditulis oleh masing-masing:
```graphql
query prolificAuthors {
  author(limit: 10) {
    id
    name
    articles_aggregate {
      count
    }
  }
}
```

Atribut-atribut komposabilitas ini meningkatkan kemampuan untuk melakukan komposisi mandiri dan mengurangi kebutuhan akan agregasi manual.
