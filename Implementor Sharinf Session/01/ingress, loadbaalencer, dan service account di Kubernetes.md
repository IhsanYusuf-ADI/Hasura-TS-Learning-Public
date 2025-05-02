# Penjelasan Ingress, LoadBalancer, dan Service Account di Kubernetes

## 1. Ingress

Ingress adalah komponen dalam Kubernetes yang mengatur akses masuk ke dalam cluster dari luar (external access), biasanya melalui HTTP dan HTTPS. Ingress memungkinkan Anda mendefinisikan aturan routing untuk mengarahkan trafik ke service tertentu di dalam cluster berdasarkan URL path atau host.

### Fitur utama:

* Routing berbasis URL dan host
* Mendukung TLS (HTTPS)
* Load balancing HTTP
* Terminasi SSL

### Contoh sederhana konfigurasi Ingress:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
spec:
  rules:
  - host: example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: example-service
            port:
              number: 80
```

## 2. LoadBalancer

LoadBalancer adalah salah satu jenis Service di Kubernetes yang digunakan untuk mengekspos aplikasi ke internet. Ketika Service bertipe LoadBalancer dibuat, cloud provider akan secara otomatis membuat load balancer eksternal (misalnya AWS ELB atau GCP Load Balancer).

### Ciri khas:

* Mengarahkan trafik dari IP publik ke service di dalam cluster
* Umumnya digunakan untuk aplikasi yang harus diakses dari luar
* Membutuhkan dukungan dari cloud provider

### Contoh Service LoadBalancer:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: LoadBalancer
  selector:
    app: my-app
  ports:
  - port: 80
    targetPort: 8080
```

## 3. Service Account

Service Account di Kubernetes digunakan untuk memberikan identitas kepada pod agar dapat mengakses resource API Kubernetes. Setiap pod dijalankan dengan service account tertentu, yang digunakan untuk otentikasi ke Kubernetes API Server.

### Fungsi utama:

* Otentikasi internal antar komponen
* Memberikan akses terbatas berdasarkan RBAC (Role-Based Access Control)
* Default service account otomatis dibuat di setiap namespace

### Contoh pembuatan Service Account:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: my-service-account
```

### Contoh penggunaan pada Pod:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  serviceAccountName: my-service-account
  containers:
  - name: my-container
    image: my-image
```

---

Catatan ini memberikan gambaran dasar tentang bagaimana komponen-komponen ini bekerja di dalam ekosistem Kubernetes dan perannya dalam mengelola trafik dan keamanan.
