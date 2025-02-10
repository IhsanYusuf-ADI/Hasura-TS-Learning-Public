# Peningkatan

Manajemen siklus hidup klaster sangat erat kaitannya dengan penerapan klaster. Sistem penerapan klaster tidak selalu harus memperhitungkan peningkatan di masa depan; namun, ada cukup banyak kekhawatiran yang tumpang tindih sehingga disarankan untuk mempertimbangkannya. Setidaknya, strategi peningkatan Anda perlu diselesaikan sebelum masuk ke produksi. Mampu menerapkan platform tanpa kemampuan untuk meningkatkannya dan memeliharanya sangat berisiko. Ketika Anda melihat beban kerja produksi berjalan pada versi Kubernetes yang jauh tertinggal dari rilis terbaru, itu adalah hasil dari pengembangan sistem penerapan klaster yang telah diterapkan ke produksi sebelum kemampuan peningkatan ditambahkan ke sistem. Saat pertama kali masuk ke produksi dengan beban kerja yang menghasilkan pendapatan, banyak anggaran teknik akan digunakan untuk memperhatikan fitur yang kurang atau masalah tajam yang ditemukan tim Anda. Seiring waktu, fitur-fitur tersebut akan ditambahkan dan masalah-masalah tersebut diatasi, tetapi intinya adalah bahwa fitur-fitur tersebut akan menjadi prioritas sementara strategi peningkatan tetap berada di latar belakang dan menjadi usang. Anggarkan lebih awal untuk kekhawatiran hari ke-2 ini. Diri Anda di masa depan akan berterima kasih.

Dalam membahas topik peningkatan ini, kita akan melihat versi platform Anda untuk memastikan dependensi dipahami dengan baik untuk platform itu sendiri dan beban kerja yang akan menggunakannya. Kami juga akan membahas bagaimana merencanakan rollback jika terjadi masalah dan pengujian untuk memverifikasi bahwa semuanya berjalan sesuai rencana. Terakhir, kita akan membandingkan dan membedakan strategi spesifik untuk meningkatkan Kubernetes.

## Versi Platform

Pertama-tama, beri versi pada platform Anda dan dokumentasikan versi semua perangkat lunak yang digunakan dalam platform tersebut. Ini mencakup versi sistem operasi mesin dan semua paket yang diinstal pada mereka, seperti runtime container. Ini jelas mencakup versi Kubernetes yang digunakan. Dan juga harus mencakup versi setiap add-on yang ditambahkan untuk membentuk platform aplikasi Anda. Beberapa tim mengadopsi versi Kubernetes sebagai versi platform mereka sehingga semua orang tahu bahwa versi 1.18 dari platform aplikasi menggunakan Kubernetes versi 1.18 tanpa perlu berpikir panjang atau mencari informasi tambahan. Ini bukan masalah besar dibandingkan dengan hanya melakukan versi dan mendokumentasikannya. Gunakan sistem apa pun yang disukai tim Anda. Tetapi miliki sistem tersebut, dokumentasikan, dan gunakan secara konsisten.

## Rencana untuk Gagal

Mulailah dengan asumsi bahwa sesuatu akan salah selama proses peningkatan. Bayangkan diri Anda dalam situasi harus memulihkan dari kegagalan yang buruk, dan gunakan ketakutan serta penderitaan itu sebagai motivasi untuk mempersiapkan dengan matang. Bangun otomatisasi untuk mengambil dan memulihkan cadangan untuk sumber daya Kubernetes Anda—baik dengan snapshot langsung dll maupun cadangan Velero yang diambil melalui API. Lakukan hal yang sama untuk data persisten yang digunakan oleh aplikasi Anda. Dan tangani pemulihan bencana untuk aplikasi kritis Anda dan dependensinya secara langsung.

## Pengujian Integrasi

Memiliki sistem versi yang terdokumentasi dengan baik yang mencakup semua versi komponen adalah satu hal, tetapi bagaimana Anda mengelola versi ini adalah hal lain. Dalam sistem yang kompleks seperti platform berbasis Kubernetes, merupakan tantangan besar untuk memastikan semuanya terintegrasi dan bekerja bersama seperti yang diharapkan setiap saat. Tidak hanya kompatibilitas antara semua komponen platform yang penting, tetapi juga kompatibilitas antara beban kerja yang berjalan di platform dan platform itu sendiri harus diuji dan dikonfirmasi.

## Strategi Peningkatan

Kami akan melihat tiga strategi untuk meningkatkan platform berbasis Kubernetes Anda:
1. Penggantian klaster
2. Penggantian node
3. Peningkatan di tempat

Kami akan membahasnya dalam urutan dari biaya tertinggi dengan risiko terendah hingga biaya terendah dengan risiko tertinggi. Seperti kebanyakan hal, ada trade-off yang menghilangkan kemungkinan solusi satu ukuran untuk semua. Biaya dan manfaat perlu dipertimbangkan untuk menemukan solusi yang tepat untuk persyaratan, anggaran, dan toleransi risiko Anda.

### Penggantian Klaster

Penggantian klaster adalah solusi dengan biaya tertinggi tetapi risiko terendah. Ini adalah risiko rendah karena mengikuti prinsip infrastruktur yang tidak dapat diubah yang diterapkan ke seluruh klaster. Peningkatan dilakukan dengan menerapkan klaster baru sepenuhnya di samping yang lama. Beban kerja dimigrasikan dari klaster lama ke yang baru. Selama proses peningkatan, ada tambahan klaster baru yang sepenuhnya berbeda dan biaya yang terkait dengannya.

### Penggantian Node

Opsi penggantian node mewakili jalan tengah untuk biaya dan risiko. Ini adalah pendekatan umum dan didukung oleh Cluster API. Ini adalah opsi yang dapat diterima jika Anda mengelola klaster yang lebih besar dan masalah kompatibilitas sudah dipahami dengan baik. Masalah kompatibilitas tersebut merupakan salah satu risiko terbesar untuk metode ini karena Anda meningkatkan control plane secara langsung sebagaimana layanan klaster dan beban kerja Anda yang berjalan.

### Peningkatan di Tempat

Peningkatan di tempat sesuai untuk lingkungan dengan sumber daya terbatas di mana mengganti node tidak praktis. Jalur rollback lebih sulit dan, karenanya, risikonya lebih tinggi. Namun, ini dapat dan harus dikurangi dengan pengujian yang komprehensif. Ingat juga bahwa Kubernetes dalam konfigurasi produksi adalah sistem yang sangat tersedia. Jika peningkatan di tempat dilakukan satu node pada satu waktu, risiko dapat dikurangi secara signifikan.

## Kesimpulan

Itulah berbagai opsi peningkatan yang dapat diterapkan pada platform berbasis Kubernetes. Sekarang, mari kita kembali ke awal cerita—mekanisme apa yang Anda gunakan untuk memicu opsi penyediaan dan peningkatan klaster ini. Kami membahas topik ini terakhir karena memerlukan konteks dari semua yang telah kami bahas sejauh ini dalam bab ini.
