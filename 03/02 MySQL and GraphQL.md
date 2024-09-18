# Panduan Menggunakan DBeaver dan Hasura GraphQL untuk Pengelolaan Data

## 1. Instalasi DBeaver

Untuk menambah kolom di database, kalian dapat menggunakan DBeaver sebagai alat bantu. Langkah pertama adalah menginstal DBeaver pada perangkat kalian. Kalian bisa mengunduhnya melalui tautan berikut:  
[Unduh DBeaver](https://dbeaver.io/download/)  

## 2. Membuat Koneksi ke MySQL

Setelah DBeaver terinstal, buat koneksi baru ke MySQL dan masukkan detail setup host atau URL JDBC.   

![image](https://github.com/IhsanYusuf-ADI/Hasura-TS-Learning/blob/main/03/images/DBeaver/01%20MySQL.png)
  
![image](https://github.com/IhsanYusuf-ADI/Hasura-TS-Learning/blob/main/03/images/DBeaver/02%20Create%20Connection.png)
    
Lakukan tes koneksi sebelum mengklik **Finish** untuk memastikan koneksi berhasil. 

![image](https://github.com/IhsanYusuf-ADI/Hasura-TS-Learning/blob/main/03/images/DBeaver/03%20Test%20Connection.png)  

Jika koneksi sudah terhubung, kalian dapat melakukan berbagai tindakan, seperti:

- **Create Table:** Membuat tabel baru.
- **Add Column:** Menambah kolom dan menentukan tipe data.
- **Primary/Unique Key:** Menetapkan primary key atau unique key.
- **Foreign Key:** Membuat foreign key untuk menghubungkan tabel.

Berikut tampilan untuk membuat tabel baru di Bbeaver:  

![image](https://github.com/IhsanYusuf-ADI/Hasura-TS-Learning/blob/main/03/images/DBeaver/04%20Create%20Table.png)
  
Berikut adalah tampilan di DBeaver untuk menambahkan kolom baru pada tabel di database MySQL, sekaligus mengatur tipe data serta menetapkan primary key atau unique key:  

![image](https://github.com/IhsanYusuf-ADI/Hasura-TS-Learning/blob/main/03/images/DBeaver/05%20Create%20Column.png)  

Berikut adalah tampilan di DBeaver untuk menambahkan foreign key pada tabel di database MySQL yang sudah dibuat, guna membentuk relasi antar tabel serta menjaga integritas data:  

![image](https://github.com/IhsanYusuf-ADI/Hasura-TS-Learning/blob/main/03/images/DBeaver/06%20Create%20Foreign%20Key.png)  

## 3. Memasukkan Data Menggunakan GraphQL di Hasura

Setelah tabel dibuat di DBeaver, kalian dapat menggunakan perintah **mutation insert** pada GraphQL di Hasura untuk memasukkan data ke tabel tersebut. Pastikan untuk memeriksa apakah tabel dan relasi foreign key sudah terdeteksi (tracked) di halaman data. Jika belum, kalian harus melakukan tracking terlebih dahulu.

### Contoh Mutation Insert

```graphql
mutation insert_attendance {
  insert_attendance(objects: {
    student_id: 1, 
    class_id: 3, 
    date: "2024-09-11", 
    status: "present"
  }) {
    affected_rows
    returning {
      id
      status
      date
    }
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/IhsanYusuf-ADI/Hasura-TS-Learning/blob/main/03/images/CRUD/07%20Mutation%20Insert.png)  
  
### Mutation Insert Multiple Data  

```graphql
mutation insert_students {
  insert_students(objects: [
    { name: "Charlie Brown", grade: "9", address: "101 Pine Street" },
    { name: "Diana Prince", grade: "9", address: "102 Pine Street" },
    { name: "Evan Taylor", grade: "8", address: "103 Cedar Lane" },
    { name: "Fiona Davis", grade: "8", address: "104 Cedar Lane" },
    { name: "George Miller", grade: "7", address: "105 Birch Road" },
    { name: "Helen Clark", grade: "7", address: "106 Birch Road" },
    { name: "Ian Lee", grade: "6", address: "107 Elm Street" },
    { name: "Julia Roberts", grade: "6", address: "108 Elm Street" }
  ]) {
    affected_rows
    returning {
      id
      name
    }
  }
}
```
  
Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/IhsanYusuf-ADI/Hasura-TS-Learning/blob/main/03/images/CRUD/08%20Mutation%20Insert%20Batch.png)  
    
## 4. Update Data Menggunakan GraphQL
   
Untuk memperbarui data yang sudah diinput, kalian dapat menggunakan perintah mutation update pada GraphQL.  

  ### Contoh Mutation Update

```graphql
mutation update_attendance {
  update_attendance(where: { id: { _eq: 1 } }, _set: { status: "absent" }) {
    affected_rows
    returning {
      id
      status
      date
    }
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/IhsanYusuf-ADI/Hasura-TS-Learning/blob/main/03/images/CRUD/09%20Mutation%20Update.png) 
    
### Mutation Update Multiple Data  

Untuk memperbaiki banyak data yang sudah diinput secara bersamaan dapat menggunakan perintah mutation update GraphQL seperti berikut.

```graphql
mutation {
  update_attendance_1: update_attendance(where: {id: {_eq: 35}}, _set: {id: 5}) {
    affected_rows
  }
  update_attendance_2: update_attendance(where: {id: {_eq: 36}}, _set: {id: 6}) {
    affected_rows
  }
  update_attendance_3: update_attendance(where: {id: {_eq: 37}}, _set: {id: 7}) {
    affected_rows
  }
  update_attendance_4: update_attendance(where: {id: {_eq: 38}}, _set: {id: 8}) {
    affected_rows
  }
  update_attendance_5: update_attendance(where: {id: {_eq: 39}}, _set: {id: 9}) {
    affected_rows
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/IhsanYusuf-ADI/Hasura-TS-Learning/blob/main/03/images/CRUD/10%20Mutation%20Update%20Batch%20V1.png)  
  
Jika kalian ingin memperbaiki banyak data yang sudah diinput secara bersamaan tetapi memiliki set value yang sama, kalian dapat menggunakan perintah mutation update GraphQL seperti berikut.

```graphql
mutation {
  update_attendance(
    where: {
      id: { _in: [5, 6, 7, 8, 9, 10] }
    }
    , _set: {status: "absent"}
  ) {
    affected_rows
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/IhsanYusuf-ADI/Hasura-TS-Learning/blob/main/03/images/CRUD/11%20Mutation%20Update%20Batch%20V2.png)
    
## 5. Menghapus Data Menggunakan GraphQL
   
Untuk menghapus data, gunakan perintah mutation delete. Kalian juga bisa menghapus beberapa data sekaligus.

  ### Contoh Mutation Delete

```graphql
mutation delete_attendance {
  delete_attendance(where: { id: { _eq: 1 } }) {
    affected_rows
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/IhsanYusuf-ADI/Hasura-TS-Learning/blob/main/03/images/CRUD/12%20Mutation%20Delete.png)
    
### Mutation Delete Multiple Data  

```graphql
mutation {
  delete_attendance(
    where: {
      id: { _in: [5, 6, 7, 8, 9, 10] }
    }
  ) {
    affected_rows
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/IhsanYusuf-ADI/Hasura-TS-Learning/blob/main/03/images/CRUD/13%20Mutation%20Delete%20Batch.png)
    

## 6. Mengambil Data Menggunakan GraphQL
   
Untuk membaca atau mengambil data dari database, kalian bisa menggunakan perintah query.

  ### Contoh Query Mengambil Data Berdasarkan Kriteria Tertentu

```graphql
query get_attendance {
  attendance(where: { class_id: { _eq: 3 }, date: { _eq: "2024-09-11" } }) {
    student_id
    status
    date
    student {
      name
    }
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/IhsanYusuf-ADI/Hasura-TS-Learning/blob/main/03/images/CRUD/14%20Query%20Get%20By%20Criteria.png)
    
### Contoh Query Mengambil Semua Data

```graphql
query {
  attendance {
    id
    student_id
    class_id
    date
    status
  }
}
```

Berikut adalah hasil ketika diterapkan pada GraphQL:

![image](https://github.com/IhsanYusuf-ADI/Hasura-TS-Learning/blob/main/03/images/CRUD/15%20Query%20Get%20All.png)
      
Fungsi utama query ini adalah untuk mengambil data dari database, baik itu secara keseluruhan maupun berdasarkan filter tertentu seperti pada contoh di atas. Dalam contoh yang telah disebutkan, data dari tabel attendance diambil untuk ditampilkan kepada pengguna atau aplikasi. Selain itu perintah ini dapat Memfilter Data Berdasarkan Kriteria Tertentu: Dengan menambahkan filter (where), kamu bisa mengambil data yang spesifik, seperti data siswa yang hadir pada hari tertentu atau data kehadiran dalam rentang waktu tertentu. Contoh sebelumnya adalah pengmabilan data menggunakan filter class dan date. 

Setelah Anda melakukan perintah CRUD, pastikan data telah masuk atau terbarui dengan memeriksa data di tab Data pada Hasura, seperti yang terlihat pada gambar berikut:

![image](https://github.com/IhsanYusuf-ADI/Hasura-TS-Learning/blob/main/03/images/CRUD/16%20Data%20Show.png)  
  
Kalian juga dapat memeriksa Foreign Key Relationships yang telah dibuat sebelumnya melalui DBeaver atau di Hasura. Caranya, masuk ke menu Data, kemudian pilih database yang ingin kalian cek relasinya. Setelah itu, masuk ke bagian Foreign Key Relationships untuk melihat foreign key dan relasi antar tabel. Tampilan di Hasura akan terlihat seperti gambar berikut:

![image](https://github.com/IhsanYusuf-ADI/Hasura-TS-Learning/blob/main/03/images/CRUD/17%20Data%20Relationship.png)

Gambar di atas menunjukkan dua jenis relasi yang ada pada database yang dipilih, yaitu:
- Object Relationships (one-to-one)
- Array Relationships (one-to-many)

Pastikan relasi antar tabel yang kalian inginkan atau sudah kalian buat pada DBeaver sebelumnya sudah berada dalam kondisi **"track"**. Jika belum, kalian bisa masuk ke tab **Untrack**, kemudian klik **Track** pada relasi yang ingin diimplementasikan.
  
Kalian juga dapat melihat relasi antar tabel pada DBeaver. Kalian cukup pilih database atau table yang ingin kalian lihat relasinya setelah itu pilih ERD untuk mengakses relasi antar tabel seperti pada gambar berikut.  

![image](https://github.com/user-attachments/assets/696044db-828a-4996-8a60-1171c446f2c9)  
  
