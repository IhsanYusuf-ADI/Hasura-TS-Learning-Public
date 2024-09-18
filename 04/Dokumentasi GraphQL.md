# Dokumentasi GraphQL

Dokumentasi ini mencakup **GraphQL** dan komponen seperti `limit`, `offset`, `where`, `order_by`, `aggregate`, `by_pk`, `and`, `or`, `not`, serta operator perbandingan lainnya. Termasuk contoh penerapannya dan penambahan operator baru seperti `_ilike`, `_like`, dan `contains`.

## 1. Limit & Offset

- **limit**: Membatasi jumlah data yang dikembalikan.
- **offset**: Melewati sejumlah record pertama dan menampilkan sisanya mulai dari posisi tertentu.

### Contoh:

```graphql
query getLimitedSchedules {
  schedules(limit: 5, offset: 2) {
    id
    day_of_week
    start_time
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:
  
![image](https://github.com/user-attachments/assets/79c02aff-801b-4b7f-b008-c0d2aaa8e2ad)

 
Query ini mengambil 5 data dimulai dari baris ketiga.  

## 2. Klausa Where

Klausa where digunakan untuk memfilter data berdasarkan kondisi tertentu.  
  
- **eq**: Sama dengan (=)
- **neq**: Tidak sama dengan (<>)
- **_gt**: Lebih besar dari (>)
- **_gte**: Lebih besar atau sama dengan (>=)
- **_lt**: Lebih kecil dari (<)
- **_lte**: Lebih kecil atau sama dengan (<=)
- **_in**: Nilai harus ada di dalam daftar
- **_nin**: Nilai tidak boleh ada di dalam daftar
- **_is_null**: Memeriksa apakah kolom memiliki nilai NULL
- **_like**: Melakukan pencarian string berdasarkan pola yang sensitif terhadap huruf besar/kecil.
- **_ilike**: Melakukan pencarian string berdasarkan pola yang tidak sensitif terhadap huruf besar/kecil (case-insensitive).
- **_contains**: Memeriksa apakah array berisi nilai tertentu.

### Contoh:

```graphql
query getSchedulesByDay {
  schedules(where: { day_of_week: { _eq: "Monday" } }) {
    id
    start_time
    end_time
  }
}
```
  
Berikut adalah hasil ketika diterapkan pada GraphQL:
  
![image](https://private-user-images.githubusercontent.com/180915097/368454618-79c02aff-801b-4b7f-b008-c0d2aaa8e2ad.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjY2NTE3MTAsIm5iZiI6MTcyNjY1MTQxMCwicGF0aCI6Ii8xODA5MTUwOTcvMzY4NDU0NjE4LTc5YzAyYWZmLTgwMWItNGI3Zi1iMDA4LWMwZDJhYWE4ZTJhZC5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwOTE4JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDkxOFQwOTIzMzBaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0xZTJkZGYyYmIwNDJlYjUzNWYwYjBkZTA2ZjMwYmFiMTMxMjM2NmE3NDUxNmMwZTZiNzJlMWFhMGRhNjU5NWYzJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.3HMe7nZX1P4CPfD05AKs_53fS7nUa_Gjbjc7i_NENrw)

  
Query ini mengambil semua jadwal pada hari Senin.   
  
### Menggunakan Operator Like dan ILike: 

```graphql
query searchSchedulesByTeacher {
  schedules(
    where: { teacher: { name: { _like: "%Brown%" } } }
  ) {
    id
    day_of_week
    teacher {
      name
    }
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/75f2d698-dead-4b99-bc88-d5f3c66b1472)

  
Mengambil jadwal yang memiliki guru dengan nama mengandung "Brown", sensitif terhadap huruf besar/kecil.

```graphql
query searchSchedulesByTeacherIlike {
  schedules(
    where: { teacher: { name: { _ilike: "%orange%" } } }
  ) {
    id
    day_of_week
    teacher {
      name
    }
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/334d37f4-578a-490d-b847-b9801074f27f)

   
Mengambil jadwal yang memiliki guru dengan nama mengandung "orange", tidak sensitif terhadap huruf besar/kecil.  

### Menggunakan Contains: 
  
```graphql
query getSchedulesByTeacherIds {
  schedules(where: {teacher: {name: {contains: "Gray"}}}) {
    id
    day_of_week
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/432f9a52-7ab8-45f5-853c-ff5ff71d966c)

  
Mengambil semua jadwal yang memiliki `teacher name` Gray.  

## 3. Order By  

Digunakan untuk mengurutkan data berdasarkan kolom tertentu dalam urutan menaik (`asc`) atau menurun (`desc`).  

### Contoh:  

```graphql
query getSchedulesOrdered {
  schedules(order_by: { start_time: asc }) {
    id
    start_time
    end_time
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/d8d268dc-e6a4-47d5-a1c8-0d67b29a0c06)

  
Mengambil semua jadwal diurutkan berdasarkan `start_time` secara menaik.  

## 4. Aggregate  

Digunakan untuk melakukan fungsi agregat seperti menghitung jumlah (`count`), rata-rata (`avg`), jumlah total (`sum`), dan sebagainya.

### Contoh:  

```graphql
query getSchedulesAggregate {
  schedules_aggregate {
    aggregate {
      count
      avg {
        class_id
      }
    }
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/c5c4640b-9bdb-4610-80e8-de76d93c78cf)
  

Hasil dari query ini akan memberikan gambaran umum tentang jumlah data dan nilai rata-rata untuk kolom `class_id` dalam `schedules`.

## 5. Berdasarkan Primary Key (by_pk)

Mengambil data berdasarkan primary key.

### Contoh:  

```graphql
query getScheduleById {
  schedules_by_pk(id: 22) {
    id
    day_of_week
    start_time
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/51159782-ed80-427d-9ddc-7ba4480682d9)

  
Mengambil jadwal dengan primary key `id = 22`.  

## 6. Operator Logika (_and, _or, _not)

`AND`, `OR`, dan `NOT` digunakan untuk menggabungkan beberapa kondisi filter.
- **_and**: Kondisi logika AND.
- **_or**: Kondisi logika OR.
- **_not**: Kondisi logika NOT.

### Contoh:  

```graphql
query getSchedulesWithOrCondition {
  schedules(
    where: {
      _or: [
        { day_of_week: { _eq: "Monday" } },
        { day_of_week: { _eq: "Friday" } }
      ]
    }
  ) {
    id
    day_of_week
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/3f4c5f1b-39a0-4e6b-a023-1a0d95c4f471)

  
Mengambil jadwal pada hari Senin atau Jumat.  

## 7. Operator Perbandingan (_eq, _neq, _gt, _gte, _lt, _lte, _in, _nin, _is_null, _like, _ilike, contains)

Operator pembanding digunakan untuk membandingkan nilai dalam query.
- **_eq**: Sama dengan.
- **_neq**: Tidak sama dengan.
- **_gt**: Lebih besar dari.
- **_gte**: Lebih besar atau sama dengan.
- **_lt**: Lebih kecil dari.
- **_lte**: Lebih kecil atau sama dengan.
- **_in**: Nilai harus ada dalam daftar.
- **_nin**: Nilai tidak boleh ada dalam daftar.
- **_is_null**: Memeriksa apakah nilai kolom NULL.

### Contoh:  

```graphql
query getSchedulesByCondition {
  schedules(where: { start_time: { _gt: "08:00:00" } }) {
    id
    start_time
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/62ba5565-2ac4-4b00-ae37-73df385f1e52)

  
Mengambil jadwal yang dimulai setelah pukul 08:00.  

### 8. String Matching (`_like`, `_ilike`, `contains`)  
  
- `_like` digunakan untuk pencocokan pola dengan case-sensitive.
- `_ilike` digunakan untuk pencocokan pola tanpa memperhatikan besar-kecil huruf.
- `contains` digunakan untuk memeriksa apakah suatu string mengandung nilai tertentu.

**Contoh:**
```graphql
query GetTeachersByName {
  teachers(where: { name: { _ilike: "%john%" } }) {
    id
    name
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/ec78d97a-8e60-42dc-a262-33bb63f7c42c)


## 9. Mutasi (Insert, Update, Delete)

### `insert`, `update`, dan `delete`
- `insert` digunakan untuk memasukkan data baru.
- `update` digunakan untuk mengubah data yang sudah ada.
- `delete` digunakan untuk menghapus data.
  
### Menambahkan Data (Insert):  

```graphql
mutation insertSchedule {
  insert_schedules(objects: {
    day_of_week: "Monday",
    start_time: "09:00:00",
    end_time: "10:00:00",
    teacher_id: 1
  }) {
    returning {
      id
      day_of_week
    }
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/b9113d1d-c1c4-4d3b-afe3-3d7d79be4b80)

  
Menambahkan jadwal baru untuk hari Senin pukul 09:00 sampai 10:00.

### Memperbarui Data (Update):

```graphql
mutation updateSchedule {
  update_schedules(
    where: { id: { _eq: 1 } },
    _set: { day_of_week: "Friday" }
  ) {
    affected_rows
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/47c17fc3-da40-4733-abb2-f7ed7fbb482b)

  
Memperbarui jadwal dengan `id = 1` menjadi hari Jumat.  

### Menghapus Data (Delete):

```graphql
mutation deleteSchedule {
  delete_schedules(where: { id: { _eq: 31 } }) {
    affected_rows
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/74d78b5d-2520-4309-a460-b92a510ab96f)

  
Menghapus jadwal dengan `id = 31`.

## 10. affected_rows dan returning dalam Mutation

### **affected_rows**
- Mengembalikan jumlah baris yang terpengaruh oleh operasi **mutation**.

### **returning**
- Mengembalikan data yang dimodifikasi setelah operasi **mutation**.

### Contoh:
```graphql
mutation {
  update_schedules(where: { id: { _eq: 24 } }, _set: { day_of_week: "Friday" }) {
    affected_rows
    returning {
      id
      day_of_week
    }
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/user-attachments/assets/6823cd36-9556-482a-a6ec-b11cfe0dfb60)


### Error Handling
Jika salah satu dari `affected_rows` atau `returning` tidak digunakan dalam query, error tidak akan terjadi. Keduanya opsional tergantung dari kebutuhan pengguna. Tetapi, harus ada salah satu dari keduanya untuk mengetahui respon dari mutasi.

### Contoh:
```graphql
mutation {
  update_schedules(where: {id: {_eq: 25}}, _set: {day_of_week: "Friday"}) {
    returning {
      id
      day_of_week
    }
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:
  
![image](https://github.com/user-attachments/assets/854a6dde-8aa1-446a-817b-c590f63a4ea5)


Kita dapat menjelajahi situs web [graphql.com](https://graphql.com/learn/mutations/) untuk contoh lebih lanjut dan penjelasan lebih mendalam tentang bagaimana konsep-konsep ini diterapkan dalam praktik.  

## 11. Query Join GraphQL

Di GraphQL, **join** dalam basis data dilakukan melalui **relationships** antar tabel. Dalam Hasura GraphQL, misalnya, kamu bisa melakukan query dengan `join` antara tabel menggunakan **relationships** yang telah ditentukan pada metadata. Ini memungkinkan kamu untuk meng-query data yang terkait dalam satu permintaan.

### Misalkan:
- Kamu punya dua tabel: `teachers` dan `schedules`.
- Tabel `schedules` memiliki foreign key `teacher_id` yang berhubungan dengan `id` di tabel `teachers`.

### Query untuk melakukan join antar dua tabel tersebut:

```graphql
query GetSchedulesWithTeacherInfo {
  schedules {
    id
    day_of_week
    start_time
    end_time
    teacher {
      id
      name
      email
    }
  }
}
```
  
Berikut adalah hasil ketika diterapkan pada GraphQL: 

![image](https://github.com/user-attachments/assets/f21c5f8d-d8a4-4297-8707-a6ac8370b120)

  
#### Penjelasan:
- **schedules**: Query ini mengambil data dari tabel `schedules`.
- **teacher**: Ini adalah hubungan antara tabel `schedules` dan `teachers` melalui kolom `teacher_id`. Dengan menggunakan hubungan ini, kita bisa mengambil data dari tabel `teachers` yang terkait, seperti `id`, `name`, dan `email`.

### Contoh query dengan kondisi (`where`) dan sorting (`order_by`):

```graphql
query GetSchedulesForMonday {
  schedules(where: {day_of_week: {_eq: "Monday"}}, order_by: {start_time: asc}) {
    id
    start_time
    end_time
    teacher {
      name
    }
  }
}
```
  
Berikut adalah hasil ketika diterapkan pada GraphQL: 

![image](https://github.com/user-attachments/assets/85b11bf6-97a5-47fe-af7a-d816a5f352d5)

  
#### Penjelasan tambahan:
- **where**: Filter data berdasarkan kondisi tertentu, di sini `day_of_week` harus bernilai `"Monday"`.
- **order_by**: Mengurutkan hasil query berdasarkan `start_time` secara ascending.

Dengan cara ini, kamu bisa melakukan query yang mirip dengan `join` dalam SQL dengan mengambil data dari tabel-tabel terkait di GraphQL.  
  
