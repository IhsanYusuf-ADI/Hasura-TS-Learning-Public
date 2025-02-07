# **Arsitektur Kubernetes dan Cara Kerjanya**

Kubernetes adalah sistem orkestrasi container yang membantu mengelola aplikasi yang berjalan dalam container secara otomatis. Arsitektur Kubernetes terdiri dari beberapa komponen utama yang bekerja sama untuk mengatur, mengontrol, dan menjalankan aplikasi secara efisien.

---

## **1. Arsitektur Kubernetes**

Arsitektur Kubernetes terdiri dari **dua bagian utama**:

1. **Control Plane (Master Node)**
2. **Worker Nodes**

### **A. Control Plane (Master Node)**

Control Plane adalah pusat kendali dari klaster Kubernetes. Ia bertanggung jawab untuk mengatur status klaster, menjadwalkan pod, serta mengelola komunikasi antara komponen.

#### **Komponen Control Plane**:

1. **API Server (`kube-apiserver`)**
    - Menyediakan RESTful API untuk berkomunikasi dengan komponen internal dan eksternal.
    - Semua perintah dari `kubectl`, Dashboard, atau API eksternal diterima oleh API Server.
2. **Controller Manager (`kube-controller-manager`)**
    - Mengelola berbagai kontrol dalam Kubernetes, seperti:
        - **Node Controller**: Memantau status node.
        - **Replication Controller**: Memastikan jumlah replika pod sesuai.
        - **Endpoint Controller**: Menghubungkan service dengan pod.
3. **Scheduler (`kube-scheduler`)**
    - Menentukan di node mana sebuah pod akan dijalankan, berdasarkan resource yang tersedia.
4. **Etcd** (Key-Value Store)
    - Menyimpan semua konfigurasi klaster Kubernetes secara konsisten.
    - Bertindak sebagai database pusat untuk status klaster.

---

### **B. Worker Node**

Worker Nodes menjalankan aplikasi dalam container. Setiap worker node memiliki agen yang memastikan pod berjalan dengan baik.

#### **Komponen Worker Node**:

1. **Kubelet**
    - Agen utama di setiap node yang bertanggung jawab untuk menjalankan pod dan berkomunikasi dengan API Server.
2. **Container Runtime**
    - Menjalankan container dalam pod, misalnya Docker, containerd, atau CRI-O.
3. **Kube Proxy**
    - Mengatur jaringan dalam klaster Kubernetes, termasuk routing traffic antar pod dan ke dunia luar.

---

## **2. Cara Kerja Kubernetes**

### **A. Proses Deploy Aplikasi dalam Kubernetes**

1. **Developer membuat deployment** menggunakan `kubectl apply -f deployment.yaml` atau melalui API.
2. **API Server menerima request** dan menyimpannya di etcd.
3. **Scheduler memilih node** yang cocok untuk menjalankan pod.
4. **Kubelet pada worker node mengambil tugas** dari API Server dan meminta container runtime untuk menjalankan pod.
5. **Kube Proxy mengatur komunikasi** antara pod dalam satu klaster maupun ke eksternal.
6. **Controller Manager memastikan pod tetap berjalan** sesuai dengan yang ditentukan dalam deployment.

---

## **3. Fitur Utama Kubernetes**

- **Self-healing** → Jika pod mati, Kubernetes akan membuat ulang secara otomatis.
- **Scaling otomatis** → Bisa menambah/mengurangi jumlah pod berdasarkan traffic.
- **Load Balancing** → Mengarahkan traffic ke pod yang tersedia.
- **Service Discovery** → Pod dalam satu klaster bisa saling berkomunikasi tanpa konfigurasi manual.

---

Itulah ringkasan arsitektur Kubernetes dan cara kerjanya. Jika Anda ingin mengimplementasikan atau memiliki pertanyaan lebih lanjut, saya siap membantu! 🚀

# **Arsitektur Kubernetes dan Implementasi dalam SLA**

## **1. Pendahuluan**
Kubernetes adalah sistem orkestrasi container yang dapat dikonfigurasi untuk memenuhi **Service Level Agreement (SLA)**, memastikan **ketersediaan (availability), keandalan (reliability), dan performa (performance)** layanan sesuai kebutuhan bisnis.

---

## **2. Arsitektur Kubernetes untuk SLA**

### **A. High Availability (HA) Cluster**
Agar tidak ada single point of failure, arsitektur Kubernetes harus mendukung High Availability (HA) dengan:

- **Multiple Control Plane Nodes** → Menggunakan etcd yang direplikasi.
- **Load Balancer untuk API Server** → Menggunakan HAProxy atau Cloud Load Balancer.
- **Multiple Worker Nodes** → Pod dipindahkan ke node lain jika terjadi kegagalan.

📌 **Contoh SLA:**
> *"SLA menjamin 99.9% uptime dengan arsitektur Kubernetes yang memiliki redundansi pada master dan worker node."*

---

### **B. Auto Scaling untuk Menjaga Performa**
Agar sistem tetap optimal dalam menghadapi lonjakan beban:

- **Horizontal Pod Autoscaler (HPA)** → Menambah/mengurangi jumlah pod berdasarkan CPU/memory usage.
- **Cluster Autoscaler** → Menyesuaikan jumlah node secara dinamis.

📌 **Contoh SLA:**
> *"SLA menjamin latensi API di bawah 100ms dengan autoscaling yang aktif berdasarkan beban sistem."*

---

### **C. Multi-Region & Multi-Cluster Deployment**
Untuk mencapai **99.99% availability**, Kubernetes dapat dikonfigurasi dengan:

- **Global Load Balancer** untuk mendistribusikan traffic.
- **Kubernetes Federation** untuk mengelola banyak klaster.
- **Database Replication** untuk menjaga ketersediaan data antar-region.

📌 **Contoh SLA:**
> *"SLA menjamin bahwa jika satu region mengalami kegagalan, sistem tetap berjalan dengan failover otomatis ke region lain dalam waktu kurang dari 30 detik."*

---

## **3. Observability & Monitoring untuk SLA**

### **A. Logging & Monitoring**
- **Prometheus + Grafana** → Memantau metrik sistem.
- **Elasticsearch + Fluentd + Kibana (EFK)** → Menganalisis log aplikasi.

### **B. Distributed Tracing**
- **OpenTelemetry atau Jaeger** → Menelusuri performa API dari ujung ke ujung.

📌 **Contoh SLA:**
> *"SLA menjamin deteksi anomali dalam waktu kurang dari 5 menit menggunakan observability tools."*

---

## **4. Disaster Recovery & Backup**
Jika SLA mencakup pemulihan dari kegagalan, strategi berikut diterapkan:

- **Backup otomatis menggunakan Velero**.
- **Database Replication & PITR (Point-In-Time Recovery)**.
- **Failover Cluster** dengan **RTO (Recovery Time Objective)** yang jelas.

📌 **Contoh SLA:**
> *"SLA menjamin pemulihan data dalam waktu kurang dari 15 menit jika terjadi kehilangan data kritis."*

---

## **5. Kesimpulan**
Untuk memenuhi SLA yang tinggi, Kubernetes harus memiliki:
✅ **High Availability**
✅ **Autoscaling**
✅ **Multi-Region Deployment**
✅ **Monitoring & Observability**
✅ **Disaster Recovery**

📌 **Contoh SLA yang dapat diterapkan:**
- *99.9% uptime*
- *Latency API <100ms*
- *Recovery dalam 15 menit*

Jika Anda ingin membuat SLA Kubernetes yang lebih spesifik, pastikan untuk mendefinisikan SLO (Service Level Objective) yang jelas sesuai kebutuhan bisnis!

