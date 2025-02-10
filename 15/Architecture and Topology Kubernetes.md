# Arsitektur dan Topologi

Bagian ini mencakup keputusan arsitektur yang memiliki implikasi luas bagi sistem yang Anda gunakan untuk menyediakan dan mengelola klaster Kubernetes Anda. Ini termasuk model penerapan untuk etcd dan pertimbangan unik yang harus Anda perhatikan untuk komponen tersebut. Di antara topik-topik ini adalah bagaimana Anda mengatur berbagai klaster yang dikelola ke dalam tingkatan berdasarkan tujuan tingkat layanan (SLO) mereka. Kami juga akan melihat konsep pool node dan bagaimana mereka dapat digunakan untuk berbagai tujuan dalam suatu klaster. Terakhir, kami akan membahas metode yang dapat Anda gunakan untuk manajemen federasi klaster Anda dan perangkat lunak yang Anda terapkan ke dalamnya.

## Model Penerapan etcd
Sebagai basis data untuk objek dalam klaster Kubernetes, etcd memerlukan perhatian khusus. etcd adalah penyimpanan data terdistribusi yang menggunakan algoritma konsensus untuk menjaga salinan status klaster Anda di beberapa mesin. Ini memperkenalkan pertimbangan jaringan bagi node dalam klaster etcd agar mereka dapat mempertahankan konsensus tersebut dengan andal melalui koneksi jaringan mereka. etcd memiliki persyaratan latensi jaringan yang unik yang perlu dirancang dengan mempertimbangkan topologi jaringan. Kami akan membahas topik tersebut dalam bagian ini dan juga melihat dua pilihan arsitektur utama dalam model penerapan etcd: dedicated versus colocated serta apakah akan dijalankan dalam kontainer atau diinstal langsung di host.

### Pertimbangan Jaringan
Pengaturan default dalam etcd dirancang untuk latensi dalam satu pusat data. Jika Anda mendistribusikan etcd di beberapa pusat data, Anda harus menguji rata-rata round-trip antara anggota dan menyesuaikan interval heartbeat serta waktu pemilihan untuk etcd jika diperlukan. Kami sangat tidak menyarankan penggunaan klaster etcd yang tersebar di berbagai wilayah. Jika menggunakan beberapa pusat data untuk meningkatkan ketersediaan, pusat data tersebut setidaknya harus berada dalam jarak dekat dalam satu wilayah.

### Dedicated versus Colocated
Pertanyaan umum tentang penerapan etcd adalah apakah memberikan mesin khusus untuk etcd atau mengkolokasikannya pada mesin kontrol dengan server API, scheduler, dan controller manager. Hal pertama yang perlu dipertimbangkan adalah ukuran klaster yang akan Anda kelola, yaitu jumlah node pekerja yang akan Anda jalankan per klaster. Jika Anda memiliki klaster besar dengan lebih dari 50 node pekerja, lebih baik menggunakan mesin khusus untuk etcd guna menghindari kontensi sumber daya dengan komponen kontrol lainnya. Untuk klaster kecil, menggunakan model colocated akan menghemat biaya dan overhead manajemen. Jika jumlah pekerja lebih dari 200, sebaiknya Anda menggunakan klaster etcd khusus.

### Kontainerisasi versus Instalasi di Host
Pertanyaan berikutnya adalah apakah akan menginstal etcd langsung di mesin atau menjalankannya dalam kontainer. Jika Anda menjalankan etcd dalam mode colocated, disarankan untuk menjalankannya dalam kontainer. Jika Anda menjalankan etcd pada mesin khusus, Anda memiliki opsi untuk menginstalnya langsung di host atau menjalankannya dalam kontainer dengan menggunakan kubelet dan manifes statis. Menggunakan pola yang sama dengan komponen kontrol lainnya dapat bermanfaat, tetapi pilihan ini lebih merupakan preferensi pribadi.

## Tingkatan Klaster
Mengorganisasi klaster berdasarkan tingkatan adalah pola yang hampir universal. Biasanya, tingkatan ini mencakup pengujian, pengembangan, staging, dan produksi. Setiap tingkatan memiliki SLO dan SLA yang berbeda serta tujuan yang berbeda dalam jalur menuju produksi.

### Pengujian
Klaster dalam tingkatan pengujian bersifat sementara (ephemeral) dan sering kali memiliki waktu hidup (TTL) yang menghapusnya secara otomatis setelah beberapa waktu, biasanya kurang dari seminggu. Tidak ada SLO atau SLA untuk klaster ini.

### Pengembangan
Klaster pengembangan bersifat permanen dan sering kali multi-tenant. Mereka memiliki fitur yang sama dengan klaster produksi tetapi digunakan untuk pengujian integrasi pertama. Klaster ini memiliki SLO tetapi tidak memiliki perjanjian formal terkait keandalannya.

### Staging
Klaster staging juga bersifat permanen dan digunakan untuk pengujian integrasi akhir sebelum rilis ke produksi. Mereka sering memiliki SLO yang mirip dengan klaster pengembangan dan mungkin memiliki SLA jika digunakan oleh pelanggan eksternal atau pemangku kepentingan lainnya.

### Produksi
Klaster produksi digunakan untuk aplikasi yang menghadapi pelanggan dan menghasilkan pendapatan. Hanya versi perangkat lunak yang telah diuji dengan stabil yang dijalankan di sini. SLO yang jelas dan terdefinisi dengan baik digunakan dan dipantau, sering kali dengan SLA yang mengikat secara hukum.

## Node Pools
Node pools adalah cara untuk mengelompokkan jenis node dalam satu klaster Kubernetes. Node dalam pool dapat dikelompokkan berdasarkan karakteristik atau peran mereka.

| Keuntungan | Kerugian |
|------------|----------|
| Mengurangi jumlah klaster yang dikelola | Memerlukan penyeleksi node untuk workloads |
| Target klaster lebih sedikit untuk workloads | Perlu menerapkan dan mengelola taints node |
| | Operasi penskalaan klaster menjadi lebih rumit |

### Pool Berdasarkan Karakteristik
Pool berdasarkan karakteristik mencakup node yang memiliki komponen atau atribut tertentu yang diperlukan untuk workloads tertentu, seperti GPU atau jenis antarmuka jaringan tertentu.

### Pool Berdasarkan Peran
Pool berdasarkan peran digunakan untuk memisahkan fungsi tertentu dalam klaster, seperti lapisan ingress. Ini mengisolasi workloads dari kontensi sumber daya dan menyederhanakan model jaringan.

---
