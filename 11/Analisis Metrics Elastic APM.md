# Analisa Metrics Hasura GraphQL dari APM Elastic

Analisa dilakukan dengan melihat data pada **`APM Elastic`** setelah dilakukan query GraphQL Hasura.

![image](https://github.com/user-attachments/assets/f1425ac7-8bed-45db-861b-f4b95a7b176e)

![image](https://github.com/user-attachments/assets/ca1ca11c-8e15-43fe-996d-f3fce42fa933)

---

![image](https://github.com/user-attachments/assets/4e2bb3ed-c9fe-4823-ae30-8f4ab0f008c4)

## Throughput dan Transactions
**APM Elastic** menyediakan observabilitas terhadap performa aplikasi. Berikut penjelasan mengenai Throughput dan Transactions yang terlihat di dashboard APM:

### 1. **Throughput**
  - **Definisi**: Throughput mengukur **jumlah transaksi atau request** yang diproses oleh aplikasi dalam satuan waktu tertentu.
  - **Satuan**: Biasanya dalam **transactions per minute (tpm).**
  - **Arti**: 
      - Semakin tinggi throughput, semakin banyak request yang diproses oleh aplikasi.
      - Throughput mencerminkan load atau beban kerja pada aplikasi.
  - **Grafik**: Menunjukkan jumlah transaksi per menit dalam rentang waktu tertentu.

**Contoh pada gambar**:
  - Throughput untuk endpoint /v1/graphql adalah 62.7 tpm (transactions per minute).
  - Endpoint /v1/entitlement memiliki throughput < 0.1 tpm, artinya jarang diakses.

---

### 2. **Transactions**
- **Definisi**: Transaksi adalah **unit request atau panggilan API** yang diterima oleh aplikasi dan dipantau oleh APM.
- **Fungsi**:
   - Setiap endpoint atau operasi aplikasi yang dipantau dianggap sebagai transaksi.
   - Transaksi melacak **latency** (waktu respons rata-rata), **throughput**, **error rate**, dan **impact**.
- **Rincian di tabel**:
   - **Latency (avg.)**: Rata-rata waktu yang dibutuhkan untuk menyelesaikan request (ms).
   - **Throughput**: Jumlah transaksi (request) per menit.
   - **Failed Transaction Rate**: Persentase request yang gagal.
   - **Impact**: Pengaruh endpoint terhadap performa keseluruhan aplikasi.

**Contoh pada gambar:**
- **`/v1/graphql`**:
   - Latency: **2.5 ms** (cepat)
   - Throughput: **62.7 tpm** (sering diakses)
   - Impact: Tinggi (indikasi bahwa endpoint ini penting dalam aplikasi).
- **`/v1/entitlement`**:
   - Latency: **0.2 ms** (sangat cepat)
   - Throughput: **< 0.1 tpm** (jarang digunakan).
   - Impact: Rendah (penggunaan jarang).

---

![image](https://github.com/user-attachments/assets/32515e0f-2f7e-4537-ada8-7152a30d44a8)

### **Rentang Waktu pada Grafik**
- Rentang waktu **awal**: Sekitar pukul **13:55:00**.  
- Rentang waktu **akhir**: Sekitar pukul **14:20:00**.  
- **Durasi**: Grafik mencakup periode sekitar **25 menit**.

### **Detail Data Sesuai Rentang Waktu**
#### **Grafik Latency (Kiri)**
- **Awal (13:55:00 - 14:10:00)**:  
   - Tidak ada aktivitas signifikan yang tercatat di awal.  
   - Waktu respons mulai muncul mendekati pukul **14:10**, naik ke sekitar **2 ms**.

- **Puncak Aktivitas (14:10:00 - 14:15:00)**:  
   - Latency **stabil** di kisaran **2 ms**, menunjukkan respons aplikasi tetap optimal saat jumlah request meningkat.

- **Akhir (14:15:00 - 14:20:00)**:  
   - Latency **menurun drastis mendekati 0 ms**.
   - Penurunan ini terjadi seiring berkurangnya request atau throughput di akhir periode.

#### **Grafik Throughput (Kanan)**
- **Awal (13:55:00 - 14:10:00)**:  
   - Throughput **0 tpm** pada awal rentang waktu. Tidak ada request atau aktivitas yang diproses.

- **Puncak Aktivitas (14:10:00 - 14:15:00)**:  
   - Throughput mengalami lonjakan tajam hingga mencapai **1,000 tpm**.
   - Ini menunjukkan peningkatan signifikan dalam jumlah transaksi atau request yang diterima.

- **Akhir (14:15:00 - 14:20:00)**:  
   - Throughput mulai menurun dan mendekati **0 tpm** pada akhir grafik.  
   - Hal ini menunjukkan bahwa permintaan berkurang atau berhenti sepenuhnya dalam periode tersebut.

---

![image](https://github.com/user-attachments/assets/ee69c138-7858-4c39-99b9-5f67de8626ed)

### **Latency Distribution**
- **Definisi**: Distribusi **latency** mengukur waktu yang dibutuhkan untuk menyelesaikan transaksi (request) dalam aplikasi. Grafik ini menunjukkan berapa banyak transaksi (request) terjadi pada interval waktu latency tertentu.
- **Sumbu X (Horizontal)**: Menampilkan **latency** dalam satuan **microsecond (µs)** hingga **millisecond (ms)**.  
- **Sumbu Y (Vertikal)**: Menampilkan jumlah transaksi untuk setiap interval latency.  
- **Data Utama**:
   - **Current Sample**: Ditandai dengan garis hijau, latency berada di sekitar **600 µs (microseconds)**.
   - **95th Percentile (95p)**: Ini menunjukkan bahwa **95% dari transaksi** memiliki latency di bawah **5 ms**.

### **Distribusi Latency Berdasarkan Grafik**
- **Rentang Utama**:
   - Sebagian besar transaksi memiliki latency **antara 400 µs hingga 5 ms**.
   - Distribusi membentuk dua kelompok utama:
      - **Kelompok 1**: Banyak transaksi berada di rentang **400-900 µs**, menunjukkan bahwa sebagian besar request memiliki latency rendah.
      - **Kelompok 2**: Lonjakan transaksi kedua terjadi di sekitar **3-6 ms**, menunjukkan sebagian request membutuhkan waktu lebih lama.
- **Kelompok Latency Tinggi**:
   - Terdapat transaksi dengan latency **20 ms hingga 300 ms**, tetapi jumlahnya sangat sedikit.

---

![image](https://github.com/user-attachments/assets/0d6610b8-5783-47c8-9eec-30848e2ca835)

## Metadata

![image](https://github.com/user-attachments/assets/42514182-12fd-41f4-a1dc-22580c99fd8b)

![image](https://github.com/user-attachments/assets/cf160cd0-f7e5-4f12-886b-b0c2709c1e55)

![image](https://github.com/user-attachments/assets/a4d31bf1-d42f-45b2-a19b-71f72b8841ad)

![image](https://github.com/user-attachments/assets/36ff3925-025c-429d-8ca4-3e3eb988d88e)

## Timeline **/v1/graphql**

View transaction in Discover
![image](https://github.com/user-attachments/assets/994bfb48-67ed-40f6-8af9-170f9071b825)

```json
{
  "_index": "apm-7.17.18-transaction-000001",
  "_id": "u2l3r5MB2LTZdF1KamMS",
  "_version": 1,
  "_source": {
    "observer": {
      "hostname": "worker1.k8s.alldataint.com",
      "id": "4fa9447e-2492-4e9b-af43-0977fa67cb28",
      "type": "apm-server",
      "ephemeral_id": "f1168f81-ff63-485a-a3de-7bad197ee4f4",
      "version": "7.17.18",
      "version_major": 7
    },
    "agent": {
      "name": "otlp",
      "version": "unknown"
    },
    "trace": {
      "id": "fe75ad67ea3f85dd40b48cf819cdbaee"
    },
    "@timestamp": "2024-12-10T07:22:49.236Z",
    "ecs": {
      "version": "1.12.0"
    },
    "service": {
      "framework": {
        "name": "hasura",
        "version": "v2.42.0"
      },
      "name": "hasura",
      "language": {
        "name": "unknown"
      }
    },
    "event": {
      "ingested": "202
