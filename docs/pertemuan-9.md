---
id: pertemuan-9
title: Database Administration
sidebar_label: Pertemuan 9
sidebar_position: 10
slug: /pertemuan-9
---

> **Topik:** *Database Administration*  
> **Platform:** Laragon (MySQL/MariaDB) + HeidiSQL/phpMyAdmin  
> **DBMS** — diselenggarakan oleh Fakultas Teknologi Informasi dan Sains Data Universitas Sebelas Maret, Semester Ganjil 2026/2027

## Agenda

- Database Administration

## Database Administration

### Pengertian

Database administration pada MySQL/MariaDB merujuk pada serangkaian tugas dan tanggung jawab yang terkait dengan pengelolaan dan pemeliharaan sistem basis data. Ini mencakup berbagai aspek teknis dan administratif yang bertujuan untuk memastikan bahwa basis data berfungsi dengan baik, aman, dan efisien.

:::note
Berbeda dengan SQL Server yang memisahkan konsep `LOGIN` (akses server) dan `USER` (akses database), MySQL/MariaDB menggabungkan keduanya. Sebuah `USER` di MySQL sudah mencakup kredensial login sekaligus entitas yang diberi hak akses ke dalam database.
:::

### CREATE USER

User adalah entitas/akun yang digunakan untuk mengotentikasi pengguna saat mengakses MySQL/MariaDB. Format pembuatan user di MySQL biasanya menyertakan host asal koneksi (misalnya `localhost`).

Contoh **CREATE USER**
```sql
CREATE USER 'username'@'host' IDENTIFIED BY 'password';

-- Contoh nyata:
CREATE USER 'bob'@'localhost' IDENTIFIED BY 'bobganteng';
```

Contoh **ALTER USER** (Mengubah password)
```sql
ALTER USER 'username'@'host' IDENTIFIED BY 'new_password';

-- Contoh nyata:
ALTER USER 'bob'@'localhost' IDENTIFIED BY 'rahasia123';
```

Contoh **DROP USER**
```sql
DROP USER 'username'@'host';

-- Contoh nyata:
DROP USER 'bob'@'localhost';
```

### GRANT PERMISSION

Grant Permission adalah proses memberikan izin/hak akses spesifik kepada USER atau ROLE untuk melakukan tindakan tertentu pada objek database seperti tabel, prosedur, atau keseluruhan database.

Format dasar:
```sql
GRANT jenis_permissions
ON nama_database.nama_tabel TO 'nama_user'@'host';
```

Contoh memberikan akses penuh pada semua tabel di suatu database:
```sql
GRANT ALL PRIVILEGES 
ON db_kampus.* TO 'bob'@'localhost';
```

Contoh memberikan akses spesifik:
```sql
GRANT SELECT, UPDATE, INSERT, DELETE
ON db_kampus.ARTIKEL TO 'bob'@'localhost';
```

### REVOKE

Revoke adalah proses mencabut/menghapus izin spesifik yang sebelumnya telah diberikan kepada USER atau ROLE pada objek database.

Format dasar:
```sql
REVOKE jenis_permissions
ON nama_database.nama_tabel FROM 'nama_user'@'host';
```

Contoh mencabut hak hapus data (DELETE):
```sql
REVOKE DELETE
ON db_kampus.ARTIKEL
FROM 'bob'@'localhost';
```

### ROLE

ROLE adalah sebuah grup atau kumpulan yang digunakan untuk mengelompokkan sekumpulan hak akses (permissions). ROLE memungkinkan administrator database untuk mengelola izin secara efisien dengan memberikan izin ke sebuah ROLE, lalu memasukkan satu atau beberapa user ke dalam ROLE tersebut.

Contoh **CREATE ROLE**
```sql
CREATE ROLE 'nama_role';

-- Contoh nyata:
CREATE ROLE 'penulis';
```

Contoh **Memberikan izin (GRANT) kepada ROLE**
```sql
-- Memberikan akses ke role penulis
GRANT SELECT, INSERT, UPDATE ON db_kampus.ARTIKEL TO 'penulis';
```

Contoh **Menambahkan User ke dalam ROLE**
Di MySQL/MariaDB, memasukkan user ke dalam role dilakukan dengan perintah `GRANT`.
```sql
GRANT 'nama_role' TO 'nama_user'@'host';

-- Contoh nyata:
GRANT 'penulis' TO 'bob'@'localhost';
```

Contoh **Menghapus User dari ROLE**
```sql
REVOKE 'nama_role' FROM 'nama_user'@'host';

-- Contoh nyata:
REVOKE 'penulis' FROM 'bob'@'localhost';
```

Contoh **Menghapus ROLE**
```sql
DROP ROLE IF EXISTS 'nama_role';

-- Contoh nyata:
DROP ROLE IF EXISTS 'penulis';
```

## Kontributor

- Rizky Fajar Triwibowo
- Nadhifal Azharudia Atmaja
- Aqila Ramdhan Fuady Latief

:::warning
## Credits

Tutorial ini dikembangkan oleh Asisten Praktikum DBMS 2026. Segala tutorial serta instruksi yang dicantumkan pada repositori ini dirancang sedemikian rupa sehingga mahasiswa yang sedang mengambil mata kuliah Basis Data dapat menyelesaikan tutorial saat sesi lab berlangsung.
:::