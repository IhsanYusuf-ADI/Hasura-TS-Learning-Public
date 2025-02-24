# Catatan Hasura DDN

## Hasura DDN Console Demo

### GraphiQL Explorer
- **Supergraph Explorer**
- **PromptQL Playground**
  - Mempermudah developer dan tim data untuk melakukan develop atau eksekusi filtering data tanpa query.

### Observability, Analytics, Collaboration

## Komponen Hasura DDN

![image](https://github.com/user-attachments/assets/f2fbfb5c-53c6-408b-a4ac-715e72df374c)


### Data Plane
- **V3-engine**
- **Connectors**
- **OTEL Collector**
- **Customer Managed**

### Control Plane
- **Console**
- **Builds dan Deployments**
- **Collaboration**
- **Observability**
- **Analytics**
- **Hasura Managed**

## Arsitektur Hasura DDN
- Memerlukan **connector phoenix** untuk terhubung ke Phoenix.
- Berbeda dengan **Hasura V2** yang tidak memerlukan connector.

## Perbedaan Arsitektur Hasura V2 dan DDN
| Hasura V2 | Hasura DDN |
|-----------|-----------|
| Control dan Data Plane tergabung kecuali pada beberapa enterprise self-hosted | Control dan Data Plane terpisah |
| Build dan runtime tergabung, menyebabkan masalah performa dan deployment lebih lambat | Build dan runtime terpisah, memungkinkan perubahan tanpa restart server |
| Tidak mendukung federated supergraph architecture | Dibangun untuk mendukung federated subgraph |
| Monolithic architecture, integrasi data sulit | NDC connector architecture memudahkan penambahan data sources |
| Local development dan cloud deployment dikelola manual | Local development dan cloud deployment terintegrasi |

## Perbedaan Tooling Hasura V2 dan DDN
| Hasura V2 | Hasura DDN |
|-----------|-----------|
| Console digunakan untuk konfigurasi proyek dan metadata | Console hanya untuk eksplorasi dan manajemen |
| CLI digunakan untuk metadata, migrasi, dan CI/CD | CLI utama untuk membangun API |
| Tidak ada integrasi IDE | Mendukung ekstensi VSCode |
| Database migrations didukung | Database migrations tidak didukung, gunakan alat eksternal |

## Perbedaan Fitur Hasura V2 dan DDN
| Fitur | Hasura V2 | Hasura DDN |
|--------|------------|------------|
| Streaming | Didukung | Dalam pengembangan |
| Event Triggers | Didukung | Dalam pengembangan |
| Cron Triggers | Didukung | Tidak didukung |
| API Limits | Didukung | Dalam pengembangan |
| Admin Secret | Didukung | Tidak didukung |
| Relay API | Didukung | Dalam pengembangan |
| Aggregate Queries | Didukung | Didukung dengan sedikit perbedaan |
| Native Mutations | Tidak didukung | Didukung |
| Business Logic | Actions | Lambda Connectors |
| Caching & Restified Endpoints | Didukung | Implementasi sebagai engine plugin |

## Metadata Utama dalam Hasura DDN

### Models (Entitas yang bisa di-query)
- **Tables**
- **Views**
- **Collections**
- **Native Queries**

### Commands (Operasi yang dapat dilakukan)
- **Insert, Update, Delete** pada data connectors
- **Menjalankan business logic dengan lambda connectors**
- **Query dan Mutasi di GraphQL connectors**

### Relationships (Hubungan antar model dan command)
- **Model ke Model**
- **Model ke Command**
- **Command ke Model**
- **Command ke Command**

## Business Logic dengan Lambda Connectors

### Node.js
- Menambahkan dependensi di `package.json` dan menjalankan `npm install`
- Menulis fungsi di `functions.ts`
- Gunakan `@readonly` untuk query dan hilangkan untuk mutasi

### Python
- Tambahkan dependensi di `requirements.txt` dan jalankan `pip install`
- Tambahkan fungsi di `functions.py`
- Gunakan `@connector.register_query` untuk query dan `@connector.register_mutation` untuk mutasi

### Go
- Instal dependensi dengan `go get`
- Tambahkan fungsi di file `.go`
- Gunakan prefix `Function` untuk query dan `Procedure` untuk mutasi

## Observability dalam Hasura DDN

### Traces
- Perjalanan visual dari request
- Identifikasi sumber kegagalan dan masalah performa

### Metrics
- Jumlah request, error rate, dan latency
- Visualisasi performa API

### Analytics
- Insight operasi GraphQL
- Optimasi sumber daya dan performa

### Query Plan
- Detail eksekusi query
- Optimasi query kompleks

## Tracing dalam Hasura DDN
- **OTEL Collector** mengumpulkan data dari Data Plane
- Data diteruskan ke **Control Plane** atau **3rd Party APM Tool**
- Dapat divisualisasikan di **DDN Console** atau **APM Console**

## Authentication Modes & Configuration

### NoAuth
- Endpoint publik tanpa autentikasi
- Cocok untuk testing dan development

### JWT
- Autentikasi pengguna sekali, kemudian sesi dikelola dengan JWT claims
- Didukung oleh Auth0, AWS Cognito, Firebase, dll.

### Webhook
- Setiap request diteruskan ke webhook untuk autentikasi
- Dinamis tetapi lebih lambat karena membutuhkan panggilan webhook

## Permissions dalam Hasura DDN

### Type Permissions
- Mengatur akses field untuk role tertentu

### Model Permissions
- Menentukan baris atau data yang dapat diakses oleh role

### Command Permissions
- Menentukan apakah role dapat menjalankan command
- Didefinisikan dalam file metadata subgraph

# QnA



---
