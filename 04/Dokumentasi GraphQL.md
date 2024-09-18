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
  
![image](https://github.com/user-attachments/assets/277c0a3b-d33d-4b09-b780-5c8dc1ece39f)


 
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
  
![image](https://private-user-images.githubusercontent.com/180915097/368455627-aafe7074-7a22-49ed-a656-7eac975dbaac.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjY2NTE3MTAsIm5iZiI6MTcyNjY1MTQxMCwicGF0aCI6Ii8xODA5MTUwOTcvMzY4NDU1NjI3LWFhZmU3MDc0LTdhMjItNDllZC1hNjU2LTdlYWM5NzVkYmFhYy5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwOTE4JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDkxOFQwOTIzMzBaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0yNTVjOWFmYWY0Y2JlMTQ2Y2I2NzE2MGY1NDZjOGRjNzQxZWRlNDc3MzZiYTUzYTBkM2E0Mzk0NjI0MTljZjBlJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.zk-jiaTJCPziHo1qXftS0RhFHGJIXSLFJf35f4D4qAU)

  
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

![image](https://private-user-images.githubusercontent.com/180915097/368457099-75f2d698-dead-4b99-bc88-d5f3c66b1472.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjY2NTE3MTAsIm5iZiI6MTcyNjY1MTQxMCwicGF0aCI6Ii8xODA5MTUwOTcvMzY4NDU3MDk5LTc1ZjJkNjk4LWRlYWQtNGI5OS1iYzg4LWQ1ZjNjNjZiMTQ3Mi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwOTE4JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDkxOFQwOTIzMzBaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1mMGRiZWVmZDE1MmI1OWUzYTNlZGMyMWQxYzBmZTBhYTNkMGJlNTFmZWE2NWIzYWNkZDBmNDNjNTZkYzk1ZTljJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.SHWGwv99xVTevKP2WLb97ffRJFLSdh5bDTBYQ4USUmo)

  
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

![image](https://private-user-images.githubusercontent.com/180915097/368457503-334d37f4-578a-490d-b847-b9801074f27f.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjY2NTE3MTAsIm5iZiI6MTcyNjY1MTQxMCwicGF0aCI6Ii8xODA5MTUwOTcvMzY4NDU3NTAzLTMzNGQzN2Y0LTU3OGEtNDkwZC1iODQ3LWI5ODAxMDc0ZjI3Zi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwOTE4JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDkxOFQwOTIzMzBaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT04Y2NhZWJlZjM2MDRmOWI0NTliNWU0MTNkYzIyMzdhMjk3MTY1ZTIzOTRlMmViNTZjZGM3ZjlhNzBlMTQyYWVjJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.2vlzMlbZCUzL1zW4HVm6vHf4EpCbMe5FIqVjx5p8vSU)

   
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

![image](https://private-user-images.githubusercontent.com/180915097/368458951-432f9a52-7ab8-45f5-853c-ff5ff71d966c.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjY2NTE3MTAsIm5iZiI6MTcyNjY1MTQxMCwicGF0aCI6Ii8xODA5MTUwOTcvMzY4NDU4OTUxLTQzMmY5YTUyLTdhYjgtNDVmNS04NTNjLWZmNWZmNzFkOTY2Yy5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwOTE4JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDkxOFQwOTIzMzBaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT05OWRlMTU1MDJiNDU2NTU1MWI3MzRlYmEwYmU0ZDM0ZDdkZjE2ZDA3ZjIyZDFlNTIxMDI2MDY1ZjM1ZGU3MWFmJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.jE5WsF1S-QZkj-lJKVC_cvzX-IdX9x8IIOUJYesdcH4)

  
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

![image](https://private-user-images.githubusercontent.com/180915097/368460369-d8d268dc-e6a4-47d5-a1c8-0d67b29a0c06.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjY2NTE3MTAsIm5iZiI6MTcyNjY1MTQxMCwicGF0aCI6Ii8xODA5MTUwOTcvMzY4NDYwMzY5LWQ4ZDI2OGRjLWU2YTQtNDdkNS1hMWM4LTBkNjdiMjlhMGMwNi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwOTE4JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDkxOFQwOTIzMzBaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1hNjA1YTAzYTk0YWQ4NmJmOWYzZDk5MTUxOTQ5ODVlOGM1N2YxYzVhNzc1ZDQ3YzI0YWFjNmI5NDEyZDFmOTk4JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.ZL5X8q6olKUW8ou82IfziMQw_fNNNwIAhPWBSmoKYEo)

  
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

![image](https://private-user-images.githubusercontent.com/180915097/368462830-c5c4640b-9bdb-4610-80e8-de76d93c78cf.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjY2NTE3MTAsIm5iZiI6MTcyNjY1MTQxMCwicGF0aCI6Ii8xODA5MTUwOTcvMzY4NDYyODMwLWM1YzQ2NDBiLTliZGItNDYxMC04MGU4LWRlNzZkOTNjNzhjZi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwOTE4JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDkxOFQwOTIzMzBaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1kNTBhZTAxYzcwYWNiNzZjOWMwMGM2ZmZmZjk3ZTI2ZTk4N2Y1YmU2M2Q1N2Q2ODhkYTY1ODExN2VkYjM2MWYxJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.g21E80vUtGRlCSsCDcgzKQtMqPxFiOGqwrnsagwG41s)
  

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

![image](https://private-user-images.githubusercontent.com/180915097/368463229-51159782-ed80-427d-9ddc-7ba4480682d9.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjY2NTE3MTAsIm5iZiI6MTcyNjY1MTQxMCwicGF0aCI6Ii8xODA5MTUwOTcvMzY4NDYzMjI5LTUxMTU5NzgyLWVkODAtNDI3ZC05ZGRjLTdiYTQ0ODA2ODJkOS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwOTE4JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDkxOFQwOTIzMzBaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0xNWExMjRhMGJhZWMxNzJhYTIyMzljZmQzODIyY2VjYjQ2NDI3Nzk2NDJiNzU0YjE5ZTZkMzRkZmNlNjQyNDY2JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.prBm7_dFRN3I7-BbOV_zHOyD0BPA-fyI4WhKiCtOrm4)

  
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

![image](https://private-user-images.githubusercontent.com/180915097/368463592-3f4c5f1b-39a0-4e6b-a023-1a0d95c4f471.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjY2NTE3MTAsIm5iZiI6MTcyNjY1MTQxMCwicGF0aCI6Ii8xODA5MTUwOTcvMzY4NDYzNTkyLTNmNGM1ZjFiLTM5YTAtNGU2Yi1hMDIzLTFhMGQ5NWM0ZjQ3MS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwOTE4JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDkxOFQwOTIzMzBaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0xMzViMjM3ZTBlYTU4ZmFjZThiMzhiYjJlYmU5NDkyNmZlYjE0NGU2ZGZlNWE2OTQ4NGYwMTE4OTc4YjQ2M2MzJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.BfDGpToa57ghai6pS4kL2vR1BYedPgS72zfgDXkH4wI)

  
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

![image](https://private-user-images.githubusercontent.com/180915097/368463907-62ba5565-2ac4-4b00-ae37-73df385f1e52.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjY2NTE3MTAsIm5iZiI6MTcyNjY1MTQxMCwicGF0aCI6Ii8xODA5MTUwOTcvMzY4NDYzOTA3LTYyYmE1NTY1LTJhYzQtNGIwMC1hZTM3LTczZGYzODVmMWU1Mi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwOTE4JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDkxOFQwOTIzMzBaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1mMjBlMzg3NGM4YTJlMmU0NTI3YmQyZGM2MmFiNzNkYzVjYjZhMWMwN2ViNDU0ZGYzNTJlZDk1NGU2MGY0NDA3JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.GX7A5ZMav9QgeevTrm6i7U1ISjzPOkIAyaqpgRn0T_c)

  
Mengambil jadwal yang dimulai setelah pukul 08:00.  

### 8. String Matching (`_like`, `_ilike`, `contains`)  
  
- `_like` digunakan untuk pencocokan pola dengan case-sensitive.
- `_ilike` digunakan untuk pencocokan pola tanpa memperhatikan besar-kecil huruf.
- `contains` digunakan untuk memeriksa apakah suatu string mengandung nilai tertentu.

**Contoh:**
```graphql
query GetTeachersByName {
  teachers(where: { name: { _ilike: "%yellow%" } }) {
    id
    name
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://private-user-images.githubusercontent.com/180915097/368464357-ec78d97a-8e60-42dc-a262-33bb63f7c42c.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjY2NTE3MTAsIm5iZiI6MTcyNjY1MTQxMCwicGF0aCI6Ii8xODA5MTUwOTcvMzY4NDY0MzU3LWVjNzhkOTdhLThlNjAtNDJkYy1hMjYyLTMzYmI2M2Y3YzQyYy5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwOTE4JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDkxOFQwOTIzMzBaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT05NmExYTVjMWI2OGQ4MjEzYmZiZTgzMWE3NTU3MzUzYjQ2ZTQzODE3YzcxNzI1MGUwZmFkZDk1YjgwOGM5MmU3JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.Maekgh0zUUkyRsJrBUAZ51WU5VI8P7atZL2ZknEMSX4)


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

![image](https://private-user-images.githubusercontent.com/180915097/368464741-b9113d1d-c1c4-4d3b-afe3-3d7d79be4b80.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjY2NTE3MTAsIm5iZiI6MTcyNjY1MTQxMCwicGF0aCI6Ii8xODA5MTUwOTcvMzY4NDY0NzQxLWI5MTEzZDFkLWMxYzQtNGQzYi1hZmUzLTNkN2Q3OWJlNGI4MC5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwOTE4JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDkxOFQwOTIzMzBaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1kZjE2Yjc1NzQ4ZGQ3OWQ1MzE1Y2RlY2YzNTg0ZDRmYThkN2EzYTJhMWY4YmE3NTlhZWM4YTI2ZThlYzM0NDZmJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.5eoI59gq_VLHNZ5OcPFlwuTE1LO9xeqKa5S_32-I2F0)

  
Menambahkan jadwal baru untuk hari Senin pukul 09:00 sampai 10:00.

### Memperbarui Data (Update):

```graphql
mutation updateSchedule {
  update_schedules(
    where: { id: { _eq: 23 } },
    _set: { day_of_week: "Friday" }
  ) {
    affected_rows
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://private-user-images.githubusercontent.com/180915097/368465052-47c17fc3-da40-4733-abb2-f7ed7fbb482b.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjY2NTE3MTAsIm5iZiI6MTcyNjY1MTQxMCwicGF0aCI6Ii8xODA5MTUwOTcvMzY4NDY1MDUyLTQ3YzE3ZmMzLWRhNDAtNDczMy1hYmIyLWY3ZWQ3ZmJiNDgyYi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwOTE4JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDkxOFQwOTIzMzBaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0xOTM1YTdiNmRhY2U1OTU3OTQxOGMyZDU4NDEwYjRhNGM1YTdhM2MyOTA4NTYzOWYxY2YwZTdkYzAxNThlMWIwJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.1nvEBW1Ke5hbmmSbTVPp63FWDJrhtw25NcBYqZRajFc)

  
Memperbarui jadwal dengan `id = 23` menjadi hari Jumat.  

### Menghapus Data (Delete):

```graphql
mutation deleteSchedule {
  delete_schedules(where: { id: { _eq: 31 } }) {
    affected_rows
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://private-user-images.githubusercontent.com/180915097/368465479-74d78b5d-2520-4309-a460-b92a510ab96f.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjY2NTE3MTAsIm5iZiI6MTcyNjY1MTQxMCwicGF0aCI6Ii8xODA5MTUwOTcvMzY4NDY1NDc5LTc0ZDc4YjVkLTI1MjAtNDMwOS1hNDYwLWI5MmE1MTBhYjk2Zi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwOTE4JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDkxOFQwOTIzMzBaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT03NjQyZjBkNmU1ZjAyYzgyYjYzYzU3YTFiNjJmNmJlZTU5ZDU0OTNkNDUwMTlhNjFiYjU1NWY5NzUxMTc0MWQ3JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.-kEa8yIYBy-3J5RpnWqtfKXUVLkzPPyR9mxK1zIElIk)

  
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

![image](https://private-user-images.githubusercontent.com/180915097/368465860-6823cd36-9556-482a-a6ec-b11cfe0dfb60.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjY2NTE3MTAsIm5iZiI6MTcyNjY1MTQxMCwicGF0aCI6Ii8xODA5MTUwOTcvMzY4NDY1ODYwLTY4MjNjZDM2LTk1NTYtNDgyYS1hNmVjLWIxMWNmZTBkZmI2MC5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwOTE4JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDkxOFQwOTIzMzBaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0yNmNkMGJmZjQ5NjkzMGQ4YWJhNzM1YWRkOTExYTA0ODU5ODZiZGQxYTY5ZTQ2NDQ3YzAyNDA3ODJjNjk3M2U3JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.caGBXLa6lerNb8S98l9p4mMFqN6jXt2XWyQmJR4oYxU)


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
  
![image](https://private-user-images.githubusercontent.com/180915097/368468099-854a6dde-8aa1-446a-817b-c590f63a4ea5.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjY2NTE3MTAsIm5iZiI6MTcyNjY1MTQxMCwicGF0aCI6Ii8xODA5MTUwOTcvMzY4NDY4MDk5LTg1NGE2ZGRlLThhYTEtNDQ2YS04MTdiLWM1OTBmNjNhNGVhNS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwOTE4JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDkxOFQwOTIzMzBaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0wOTZiYTdmNmRkYzMyNjMzYWU3ZWU3OWM3MGU2MjMzMWQzODlmMmI0NjhmZmQ1YjU4MzIzY2FhNjAxYmZjZDRiJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.AXVao9wzhlelh9_Whhtj5__ew0QxRNR6_bLjqnYuyKw)


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

![image](https://private-user-images.githubusercontent.com/180915097/368468622-f21c5f8d-d8a4-4297-8707-a6ac8370b120.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjY2NTE3MTAsIm5iZiI6MTcyNjY1MTQxMCwicGF0aCI6Ii8xODA5MTUwOTcvMzY4NDY4NjIyLWYyMWM1ZjhkLWQ4YTQtNDI5Ny04NzA3LWE2YWM4MzcwYjEyMC5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwOTE4JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDkxOFQwOTIzMzBaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT04YTRlZDgyODg4NDY2NWI2MGExMGY0OTQxZGQzZWE3MTkwZmZjMDA2Y2MwMDE3MGRiMzdkZTkzNTFhMDZlMjE5JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.CJJ6ddoG7CCrENRLLBiu0z4T8Z4WyXteQqAa4OzMi2k)

  
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

![image](https://private-user-images.githubusercontent.com/180915097/368468933-85b11bf6-97a5-47fe-af7a-d816a5f352d5.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjY2NTE3MTAsIm5iZiI6MTcyNjY1MTQxMCwicGF0aCI6Ii8xODA5MTUwOTcvMzY4NDY4OTMzLTg1YjExYmY2LTk3YTUtNDdmZS1hZjdhLWQ4MTZhNWYzNTJkNS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwOTE4JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDkxOFQwOTIzMzBaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1lOTIxMTYzM2ZiYjcxNzhlMjYzNzkzMmU0YTZmYWY4MWQwMWY4Y2IwYzAxNWI1YWM0Y2M5ODJhNjk2M2VhYjc1JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.ufPVpuX2Eg6X9UGhhcqGGGpUgbueCMK2HafBRr8p2Rc)

  
#### Penjelasan tambahan:
- **where**: Filter data berdasarkan kondisi tertentu, di sini `day_of_week` harus bernilai `"Monday"`.
- **order_by**: Mengurutkan hasil query berdasarkan `start_time` secara ascending.

Dengan cara ini, kamu bisa melakukan query yang mirip dengan `join` dalam SQL dengan mengambil data dari tabel-tabel terkait di GraphQL.  
  
