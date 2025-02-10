# **Pembaruan (Upgrades)**

Manajemen siklus hidup klaster sangat erat kaitannya dengan proses deployment klaster. Sistem deployment klaster tidak harus selalu mempertimbangkan pembaruan di masa mendatang; namun, ada cukup banyak keterkaitan yang membuat hal ini perlu dipertimbangkan sejak awal. Setidaknya, strategi pembaruan perlu dirancang sebelum masuk ke tahap produksi. Mampu melakukan deployment platform tanpa kemampuan untuk memperbarui dan memeliharanya sangatlah berisiko. Jika Anda menemukan beban kerja produksi berjalan di versi Kubernetes yang jauh tertinggal dari versi terbaru, itu menandakan bahwa sistem deployment klaster telah digunakan di produksi sebelum fitur pembaruan ditambahkan ke sistem tersebut.

Ketika pertama kali masuk ke produksi dengan beban kerja yang menghasilkan pendapatan, sebagian besar anggaran rekayasa akan digunakan untuk menambahkan fitur yang masih kurang atau memperbaiki masalah yang dihadapi tim Anda. Seiring berjalannya waktu, fitur-fitur tersebut akan ditambahkan dan masalah-masalah akan diperbaiki, tetapi strategi pembaruan akan cenderung tertunda dan akhirnya menjadi usang. Oleh karena itu, sejak awal, alokasikan anggaran untuk permasalahan di tahap selanjutnya (day-2 concerns). Ini akan sangat membantu Anda di masa depan.

Dalam pembahasan mengenai pembaruan ini, kita akan melihat bagaimana melakukan **versioning** (pengelolaan versi) pada platform untuk memastikan bahwa semua dependensi dipahami dengan baik, baik untuk platform itu sendiri maupun beban kerja yang berjalan di atasnya. Kita juga akan membahas bagaimana merencanakan **rollback** jika terjadi masalah, serta pengujian untuk memastikan bahwa semua proses berjalan sesuai rencana. Terakhir, kita akan membandingkan berbagai strategi pembaruan Kubernetes.

---

## **Pengelolaan Versi Platform (Platform Versioning)**

Pertama-tama, lakukan **versioning** pada platform Anda dan dokumentasikan versi dari semua perangkat lunak yang digunakan dalam platform tersebut. Ini mencakup:
- Versi sistem operasi dari mesin yang digunakan,
- Semua paket yang diinstal, termasuk container runtime,
- Versi Kubernetes yang digunakan,
- Versi dari setiap add-on yang membentuk platform aplikasi Anda.

Banyak tim yang mengadopsi versi Kubernetes sebagai versi platform mereka, misalnya **platform versi 1.18 menggunakan Kubernetes versi 1.18**. Namun, yang lebih penting dari sekadar mengikuti pola ini adalah **melakukan versioning dan mendokumentasikannya dengan baik**. Gunakan sistem yang paling sesuai untuk tim Anda, tetapi pastikan sistem tersebut terdokumentasi dengan baik dan digunakan secara konsisten.

Disarankan untuk memberikan **versi independen** bagi platform Anda, terpisah dari versi komponen di dalamnya. Jika menggunakan **Semantic Versioning** (major.minor.patch), pastikan bahwa perubahan pada komponen tertentu, seperti container runtime, direfleksikan dalam versi platform tanpa menimbulkan kebingungan dengan versi Kubernetes. Dengan mengikuti konvensi versioning yang sama seperti perangkat lunak lainnya, maknanya akan lebih jelas bagi semua engineer.

---

## **Persiapkan Diri untuk Kegagalan (Plan to Fail)**

Anggaplah sejak awal bahwa **sesuatu akan salah** selama proses pembaruan. Bayangkan situasi di mana Anda harus memulihkan sistem dari kegagalan besar, dan gunakan ketakutan serta kekhawatiran tersebut sebagai motivasi untuk mempersiapkan segala sesuatunya dengan baik.

Beberapa langkah yang perlu diambil:
- **Otomatisasi pencadangan dan pemulihan:**
  - Ambil snapshot langsung dari **etcd**.
  - Gunakan **Velero** untuk mencadangkan sumber daya Kubernetes melalui API.
- **Cadangkan data persisten aplikasi:**
  - Pastikan data dapat dipulihkan dalam urutan yang benar, terutama untuk aplikasi yang bersifat stateful dan terdistribusi.
- **Siapkan strategi pemulihan bencana (disaster recovery):**
  - Untuk aplikasi yang sangat kritis, siapkan klaster cadangan yang bisa digunakan untuk failover.
  - Uji otomatisasi failover untuk memastikan sistem bekerja dengan baik dalam skenario darurat.
- **Rencanakan rollback:**
  - Pastikan ada opsi rollback yang bisa digunakan jika terjadi kesalahan yang sulit didiagnosis.
  - Tetapkan titik “tidak bisa kembali” (point of no return) dalam proses pembaruan agar tidak melakukan rollback pada tahap yang berisiko.

Dengan memiliki rencana rollback dan prosedur pemulihan yang terotomatisasi, tim Anda akan lebih siap menghadapi skenario terburuk tanpa stres yang berlebihan.

---

## **Pengujian Integrasi (Integration Testing)**

Memiliki sistem versioning yang terdokumentasi adalah satu hal, tetapi bagaimana cara mengelola versi tersebut adalah hal lain. Pada sistem Kubernetes yang kompleks, **memastikan semua komponen bekerja bersama dengan baik setiap saat adalah tantangan besar**.

Beberapa langkah yang perlu dilakukan:
- **Lakukan pengujian kompatibilitas:**
  - Pastikan kompatibilitas antara platform dan beban kerja yang berjalan di atasnya.
  - Kurangi ketergantungan aplikasi pada fitur platform tertentu untuk menghindari masalah kompatibilitas.
- **Gunakan alat uji konformitas:**
  - **Sonobuoy** dapat digunakan untuk menjalankan pengujian end-to-end Kubernetes guna memastikan klaster berfungsi dengan baik.
  - Buat plug-in khusus untuk menguji fitur spesifik yang digunakan oleh organisasi Anda.
  - Jalankan pengujian ini secara berkala menggunakan **CronJob** di Kubernetes.
- **Pastikan pengujian mencakup aplikasi:**
  - Aplikasi yang bergantung pada fitur Kubernetes, seperti operator, harus diuji dengan cermat saat melakukan pembaruan.

Dengan menjadikan pengujian sebagai bagian dari sistem pemantauan dan peringatan (monitoring dan alerting), Anda dapat mendeteksi dan menangani potensi masalah sejak dini.

---

## **Strategi Pembaruan Kubernetes**

Ada tiga strategi utama untuk memperbarui klaster Kubernetes:
1. **Penggantian Klaster (Cluster Replacement)**
2. **Penggantian Node (Node Replacement)**
3. **Pembaruan di Tempat (In-place Upgrade)**

Strategi ini memiliki **trade-off** antara **biaya dan risiko**:
- **Cluster Replacement:** Biaya tinggi, risiko rendah.
- **Node Replacement:** Biaya sedang, risiko sedang.
- **In-place Upgrade:** Biaya rendah, risiko tinggi.

### **1. Penggantian Klaster (Cluster Replacement)**
Metode ini menggunakan prinsip **infrastruktur yang tidak dapat diubah (immutable infrastructure)**, di mana klaster baru dibuat dan klaster lama dihentikan secara bertahap.
- **Keuntungan:** Risiko rendah karena tidak ada perubahan langsung pada klaster lama.
- **Tantangan:**
  - **Mengelola lalu lintas masuk (ingress traffic).**
  - **Menyediakan penyimpanan persisten.**

### **2. Penggantian Node (Node Replacement)**
Node lama digantikan secara bertahap dengan node baru yang telah diperbarui.
- **Keuntungan:** Biaya lebih rendah dibandingkan penggantian klaster.
- **Tantangan:** Memerlukan manajemen beban kerja yang baik.

### **3. Pembaruan di Tempat (In-place Upgrade)**
Pembaruan langsung pada klaster Kubernetes tanpa mengganti node atau klaster.
- **Keuntungan:** Biaya rendah.
- **Tantangan:** Risiko tinggi.

---

**Dokumentasi dari halaman 52-60**

---
