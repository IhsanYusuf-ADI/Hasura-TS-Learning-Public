
# Setup Hasura V2 di Kubernetes dengan Integrasi OpenTelemetry

## Deployment Hasura V2 di Kubernetes  

Langkah-langkah deployment Hsura V2 telah didokumentasikan pada BAB sebelumnya. Dokumentasi dapat dilihat [disini](https://github.com/IhsanYusuf-ADI/Hasura-TS-Learning-Public/blob/main/03/01%20Preparation%20VPN%2C%20Akses%20Rancher%2C%20dan%20Deployment%20Hasura.md#3-deployment-hasura-di-kubernetes).

---

## Install OpenTelemetry Collector  

### Akses Server

Kita akan menginstall **OpenTelemetry** pada server yang berbeda dengan server Hasura yang berjalan. Terlebih dahulu Login ke server yang akan dipakai menggunakan Windows Powershell atau Command Prompt.

### Install Docker

Kita akan menginstall **OpenTelemetry** menggunakan **Docker**. Pastikan **Docker** telah terinstall pada server yang akan digunakan untuk menginstall **OpenTelemetry**. Jika belum, berikut langkah-langkah untuk menginstall **Docker**.

#### Prasyarat
Pastikan Anda menggunakan sistem operasi Ubuntu atau turunannya. Panduan ini juga berlaku untuk distribusi seperti Linux Mint, tetapi Anda mungkin perlu menggunakan `UBUNTU_CODENAME` alih-alih `VERSION_CODENAME`.

---

#### Langkah-Langkah Instalasi Docker Engine

##### 1. Tambahkan Repository Apt Docker

1. **Perbarui Daftar Paket dan Instal Dependensi:**
   ```bash
   sudo apt-get update
   sudo apt-get install ca-certificates curl
   ```

2. **Tambahkan GPG Key Resmi Docker:**
   ```bash
   sudo install -m 0755 -d /etc/apt/keyrings
   sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
   sudo chmod a+r /etc/apt/keyrings/docker.asc
   ```

3. **Tambahkan Repository Docker ke Sumber Apt:**
   ```bash
   echo      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu      $(. /etc/os-release && echo "$VERSION_CODENAME") stable" |      sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

4. **Perbarui Kembali Daftar Paket:**
   ```bash
   sudo apt-get update
   ```

##### 2. Instal Paket Docker

1. **Instal Versi Terbaru Docker Engine:**
   ```bash
   sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   ```

2. **Verifikasi Instalasi dengan Menjalankan Container `hello-world`:**
   ```bash
   sudo docker run hello-world
   ```

   Perintah ini akan mengunduh image uji coba, menjalankannya di container, dan mencetak pesan konfirmasi jika berhasil.

---

##### Catatan Tambahan
- Jika Anda menggunakan distribusi turunan Ubuntu seperti Linux Mint, pastikan Anda menggunakan `UBUNTU_CODENAME` alih-alih `VERSION_CODENAME` saat menambahkan repository Docker ke sumber Apt.

---

##$$$ Verifikasi
Jika instalasi berhasil, Anda akan melihat pesan dari container `hello-world` yang menyatakan bahwa Docker Engine telah terinstal dan berjalan dengan baik.

---
  
Baca referensi selengkapnya [disini](https://docs.docker.com/engine/install/ubuntu/).

### Install OpenTelemetry

1. **Buat folder OTEL**

Kita akan menginstall **OTEL** pada folder **OTEL**. Sehingga, terlebih dahulu kita membuat folder OTEL.
  
```
mkdir OTEL
```

2. **Masuk ke dalam folder OTEL**

```
cd OTEL
``` 

3. **Buat file otel-collector-config.yaml**
  
Kita akan membuat konfigurasi **OTEL** secara custom sehingga terlebih dahulu membuat filte **otel-collector-config.yaml**
  
```
touch otel-collector-config.yaml
```

4. **Kemudian kita edit file "otel-collector-config.yaml" untuk mengatur konfigurasi otel**
  
```
nano otel-collector-config.yaml
```

![image](https://github.com/user-attachments/assets/cfb30143-47b6-4984-8d5a-f0736b1193db)


  
Kemudian kita masukkan File Configurasi berikut ini :
```
receivers:
  otlp:
    protocols:
      grpc:
        endpoint: 0.0.0.0:4317
      http:
        endpoint: 0.0.0.0:4318
processors:
  batch:

exporters:
  debug: {}
  verbosity: detailed

extensions:
  health_check:
  pprof:
    endpoint: 0.0.0.0:1777
  zpages:
    endpoint: 0.0.0.0:55679

service:
  pipelines:
    traces:
      receivers: [otlp]
      processors: [batch]
      exporters: [debug, otlp]
    metrics:
      receivers: [otlp]
      processors: [batch]
      exporters: [debug, otlp]
    logs:
      receivers: [otlp]
      processors: [batch]
      exporters: [debug, otlp]
```

![image](https://github.com/user-attachments/assets/f4daddcc-67e0-4eab-bca8-5824398afec1)

  
5. **Kemudian Save and Exit.**

Kita ketik `ctrl + x` untuk Exit. Kemudian kita pilih "yes" untuk Save.

6. **Kemudian tampilkan daftar file pada folder OTEL**

Cek daftar file secara detail dengan log level untuk melihat apakah config otel sudah dibuat dengan baik.
   
```
ls -ll
```

![image](https://github.com/user-attachments/assets/7460a383-d9dc-4d87-a088-36c383c69ae3)

  
6. **Kemudian kita jalankan Command berikut**

Jalankan command berikut menarik Docker Image dan menjalankan Kolektor dalam sebuah kontainer.

```
docker pull otel/opentelemetry-collector-contrib:0.114.0
```

Untuk memuat file konfigurasi khusus dari direktori kerja kita, pasang file tersebut sebagai volume dengan jalankan command berikut.
   
```
docker run -d -v $(pwd)/otel-collector-config.yaml:/root/OTEL/otel-collector-config.yaml otel/opentelemetry-collector-contrib:0.114.0
```

Baca referensi selengkapnya disini:  
[Install The collector](https://opentelemetry.io/docs/collector/installation/)   
[Collector Configuration](https://opentelemetry.io/docs/collector/configuration/)  
  
7. **Install command "OpenTelemetry Collector" (otelcol)**
  
```
which otelcol
```
  
Jika balasan kosong maka tidak terdapat otelcol atau otelcol tidak terinstall. Selanjutnya, jalankan comman

#### [1] maka kita jalankan Command berikut : <br/>

```
which otelcol
```

```
curl -sL https://github.com/open-telemetry/opentelemetry-collector-releases/releases/download/v0.81.0/otelcol_0.81.0_linux_amd64.deb -o otelcol.deb
```

```
sudo dpkg -i otelcol.deb

```

```
sudo nano /etc/systemd/system/otelcol.service
```

<br/> <br/>

#### [2] Kita masukkan "File Configurasi" untuk otelcol "OpenTelemetry Collector" :

```
[Unit]
Description=OpenTelemetry Collector
After=network.target

[Service]
Type=simple
ExecStart=/usr/local/bin/otelcol --config=/etc/otelcol-config.yaml
Restart=always
RestartSec=10
User=otel
Group=otel

[Install]
WantedBy=multi-user.target

```

#### [3] Untuk melihat Log, kita jalankan Command berikut ini :
```
journalctl -u otelcol -f
```

#### [4] Sambungkan Server kita dengan Hasura pada Rancher :
(Sambungkan Server yang ada di Powershell dengan Hasura pada Rancher ) :

(1) Buka Hasura pada Rancher  <br/> <br/>

(2) Pilih Setting : <br/>
![image](https://github.com/user-attachments/assets/06b482a8-ae7b-4df4-bce6-eaa36a1250c7)     <br/>
![image](https://github.com/user-attachments/assets/ee113a11-0b68-446c-b231-841b375ada26)

<br/><br/>
(2) Pilih "Open Telemetry Exporter" <br/>
![image](https://github.com/user-attachments/assets/8163f19c-0a65-41f0-8eaf-f1551906986d)    <br/>
![image](https://github.com/user-attachments/assets/3e9f610e-5ef8-4897-a27a-d871026d5798)    <br/><br/>

(3) masukkan Endpoint pada Hasura di Rancher : <br/>
-> disini kita masukkan Endpoint "4318". <br/>
yang mana "4318" merupakan Port http pada Configuration File <br/> <br/>

![image](https://github.com/user-attachments/assets/f0becc6c-669f-46bb-b45a-2cee508e7b13)    <br/>
![image](https://github.com/user-attachments/assets/cd1f9fca-5eec-4ea0-a92c-c5923c8cb870)      <br/>



#### [5] Kemudian lakukan "Hit" dengan menjalankan Query di Hasura

![image](https://github.com/user-attachments/assets/47d8e418-403c-42a8-a1ad-e83a5bef489f)

-> Query yang di-Hit tersebut mempunyai :
- nama Tabel "todo"
- dan salah satu Nama Kolom nya adalah "name" yang mempunyai nilai "ferdy"
