---
id: pertemuan-5
title: Performing Advanced Queries
sidebar_label: Pertemuan 5
sidebar_position: 6
slug: /pertemuan-5
---

> **Topik:** *Performing Advanced Queries*  
> **Platform:** Laragon (MySQL/MariaDB) + HeidiSQL/phpMyAdmin  
> **DBMS** — diselenggarakan oleh Fakultas Teknologi Informasi dan Sains Data Universitas Sebelas Maret, Semester Ganjil 2026/2027

## Agenda
- Advance Queries
- String Function
- Aggregate & Math Function
- Date Function

Pada materi kali ini menggunakan schema ini 

![sql](/img/p5/7.png)

:::info
Download file schema di Classroom masing-masing kelas
:::

## Advance Queries

![sql](/img/p5/1.png)

### INNER JOIN

**INNER JOIN** adalah salah satu jenis operasi JOIN dalam SQL yang digunakan untuk menggabungkan baris dari dua atau lebih tabel berdasarkan kondisi tertentu. Operasi ini hanya mengembalikan baris-baris di mana terdapat kecocokan (match) antara kolom pada tabel yang digabungkan.

**INNER JOIN** merupakan DEFAULT dari perintah JOIN di MySQL/MariaDB.

Ciri utama INNER JOIN:
- Mengembalikan hanya data yang cocok di kedua tabel.
- Berguna untuk mengekstrak hubungan antar-tabel yang memiliki referensi satu sama lain, seperti relasi antara tabel pelanggan dengan pesanan.

![sql](/img/p5/2.png)

```sql
SELECT
select_list
FROM
T1
INNER JOIN T2 ON join_predicate;

=================================================================

SELECT
*
FROM
SONG s
INNER JOIN
ALBUM a
ON
s.AlbumID = a.ID;
```

### LEFT JOIN

**LEFT JOIN** adalah jenis JOIN dalam SQL yang digunakan untuk menggabungkan data dari dua tabel. 

Dalam **LEFT JOIN**, semua baris dari tabel kiri (left table) akan ditampilkan dalam hasil, meskipun tidak ada data yang cocok di tabel kanan (right table). 

Jika tidak ada kecocokan, kolom dari tabel kanan akan diisi dengan nilai NULL.

![sql](/img/p5/3.png)

```sql
SELECT
select_list
FROM
T1
LEFT JOIN T2 ON join_predicate;

=================================================================

SELECT
*
FROM
SONG s
LEFT JOIN
ALBUM a
ON
s.AlbumID = a.ID;
```

### RIGHT JOIN

**RIGHT JOIN** adalah jenis JOIN dalam SQL yang digunakan untuk menggabungkan data dari dua tabel. 

Dalam **RIGHT JOIN**, semua baris dari tabel kanan (right table) akan ditampilkan dalam hasil, meskipun tidak ada data yang cocok di tabel kiri (left table). 

Jika tidak ada kecocokan, kolom dari tabel kiri akan diisi dengan nilai NULL.

![sql](/img/p5/4.png)

```sql
SELECT
select_list
FROM
T1
RIGHT JOIN T2 ON join_predicate;

=================================================================

SELECT
*
FROM
SONG s
RIGHT JOIN
ALBUM a
ON
s.AlbumID = a.ID;
```

### FULL OUTER JOIN (Simulasi di MySQL)

**FULL OUTER JOIN** mengembalikan semua baris dari tabel kanan dan baris dari tabel kiri. Jika tidak ditemukan baris yang cocok, maka nilai kolom akan diisi dengan NULL.

:::note
MySQL/MariaDB **tidak mendukung** perintah `FULL OUTER JOIN` secara langsung. Sebagai gantinya, kita bisa mensimulasikannya dengan menggabungkan hasil dari `LEFT JOIN` dan `RIGHT JOIN` menggunakan klausa `UNION`.
:::

![sql](/img/p5/5.png)

```sql
SELECT * FROM T1
LEFT JOIN T2 ON join_predicate
UNION
SELECT * FROM T1
RIGHT JOIN T2 ON join_predicate;

=================================================================

SELECT * FROM SONG s
LEFT JOIN ALBUM a ON s.AlbumID = a.ID
UNION
SELECT * FROM SONG s
RIGHT JOIN ALBUM a ON s.AlbumID = a.ID;
```

### CROSS JOIN

**CROSS JOIN** menghasilkan *cartesian product* antara dua tabel, yang berarti setiap baris dari tabel pertama akan digabungkan dengan setiap baris dari tabel kedua.

**CROSS JOIN** biasanya digunakan secara hati-hati karena dapat menghasilkan dataset yang sangat besar.

![sql](/img/p5/6.png)

```sql
SELECT
select_list
FROM
T1
CROSS JOIN T2;

=================================================================

SELECT
Customers.CustomerName, Orders.OrderID
FROM
Customers
CROSS JOIN
Orders;
```

### GROUP BY

Klausa GROUP BY di SQL digunakan untuk mengelompokkan baris data berdasarkan nilai dalam satu atau lebih kolom. 

Setelah data dikelompokkan, fungsi agregat seperti `COUNT, SUM, AVG, MAX, atau MIN` sering digunakan untuk melakukan perhitungan pada setiap kelompok tersebut.

```sql
SELECT
select_list
FROM
T1
INNER JOIN T2 ON join_predicate
GROUP BY params;

=======================================================================

SELECT
brand_id,
MAX(list_price) AS MaxPrice
FROM
PRODUCTS
GROUP BY
brand_id;
```

### HAVING

HAVING digunakan untuk memfilter hasil dari sebuah query yang melibatkan fungsi agregasi.

HAVING biasanya digunakan bersama dengan GROUP BY.

```sql
SELECT
B.brand_name,
AVG(P.list_price) AS avg_price
FROM
PRODUCTS P
JOIN
BRANDS B
ON
P.brand_id = B.brand_id
GROUP BY
B.brand_name
HAVING
AVG(P.list_price) > 1000;
```

### SUBQUERY

Subquery adalah sebuah perintah SQL yang digunakan untuk menanamkan (embed) suatu kueri di dalam kueri lain. 

Subquery sering digunakan untuk melakukan pemrosesan data yang lebih kompleks dengan cara membangun kueri dalam lapisan bertingkat. 

Subquery biasanya ditulis di dalam tanda kurung, dan hasil dari subquery akan digunakan oleh kueri utama untuk memproses data lebih lanjut.

SUBQUERY bisa berada di bagian SELECT, FROM, WHERE, atau HAVING dari query utama dan digunakan untuk menghasilkan nilai sementara yang bisa digunakan dalam query utama.

```sql
SELECT product_name,
model_year,
brand_id,
list_price
FROM PRODUCTS
WHERE list_price < (
    SELECT AVG(list_price)
    FROM PRODUCTS
);
```

## STRING FUNCTION

String function adalah built-in functions pada MySQL/MariaDB yang memungkinkan users untuk memanipulasi data karakter.

String function memungkinkan users untuk melakukan berbagai operasi seperti menggabungkan, memotong, mengganti, mencari, atau mengubah format teks dalam suatu kolom atau nilai string.

Sumber belajar:
[MySQL String Functions](https://www.mysqltutorial.org/mysql-string-functions/)

### CONCAT

Fungsi CONCAT mengembalikan sebuah string hasil dari proses *concatenation* atau penggabungan dari dua atau lebih string.

```sql
CONCAT(string1, string2, ...);

-----------

SELECT CONCAT('John', ' ', 'Doe') AS full_name;

full_name
---------
John Doe
```

### CONCAT_WS

Fungsi CONCAT_WS memungkinkan users untuk melakukan *concatenate* lebih dari 1 string menjadi sebuah string dengan separator tertentu yang dispesifikkan. (WS = With Separator).

```sql
CONCAT_WS(separator, string1, string2, ...);

-----------

SELECT CONCAT_WS('-', 'John', 'Doe') AS full_name;

full_name
---------
John-Doe
```

### DATE_FORMAT

Di MySQL, jika ingin mengubah format tanggal menjadi string (seperti halnya fungsi FORMAT di SQL Server untuk tanggal), kita menggunakan `DATE_FORMAT()`.

```sql
DATE_FORMAT(date, format);

-----------

SELECT DATE_FORMAT('2024-10-11', '%d/%m/%Y') AS Date;

Date
--------------------
11/10/2024
```

### CHAR_LENGTH & LENGTH

Untuk mengukur panjang string di MySQL, gunakan `CHAR_LENGTH` (mengukur jumlah karakter) atau `LENGTH` (mengukur ukuran dalam byte).

```sql
CHAR_LENGTH(string)

-----------

SELECT CHAR_LENGTH('MySQL LENGTH') AS length;

length
-----------
12
```

### REPLACE

Fungsi REPLACE mengganti semua kemunculan nilai string yang ditentukan dengan nilai string lain.

```sql
REPLACE(input_string, substring, new_substring);

-----------

SELECT REPLACE(
'It is a good tea at the famous tea store.',
'tea',
'coffee'
) AS result;

result
-------------
It is a good coffee at the famous coffee store.
```

### SUBSTRING

Digunakan untuk mengambil sebagian string dari sebuah kolom berdasarkan posisi awal dan panjang substring yang diinginkan.

```sql
SUBSTRING(input_string, start, length);

-----------

SELECT SUBSTRING('MySQL SUBSTRING', 7, 9) AS result;

result
------
SUBSTRING
```

## Aggregate & Math Function

Aggregate function adalah fungsi yang melakukan perhitungan pada sekumpulan nilai dan mengembalikan satu nilai tunggal.

Kecuali fungsi COUNT(*), aggregate functions mengabaikan nilai NULL.

Sumber Belajar:
[MySQL Aggregate Functions](https://www.mysqltutorial.org/mysql-aggregate-functions/)

### AVG

AVG merupakan fungsi untuk mencari nilai rata-rata dari suatu kolom/nilai. Fungsi AVG() mengabaikan nilai NULL.

```sql
AVG([DISTINCT] expression)

-----------

SELECT AVG(Area) AS result
FROM country;
```

### COUNT

COUNT adalah fungsi agregasi untuk menghitung jumlah suatu item di dalam suatu kolom.

```sql
COUNT([DISTINCT] expression)

-----------

SELECT Country, COUNT(Name) AS CityCount
FROM city
GROUP BY Country;
```

### MAX & MIN

MAX mengembalikan nilai tertinggi (maksimum), sedangkan MIN mengembalikan nilai terendah (minimum).

```sql
SELECT e.Continent, MAX(c.Area) AS MaxArea, MIN(c.Area) AS MinArea
FROM country c
JOIN encompasses e ON e.Country = c.Code
GROUP BY e.Continent;
```

### SUM

Digunakan untuk menjumlahkan nilai-nilai numerik dalam satu grup.

```sql
SELECT SUM(quantity) AS total_stocks
FROM production.stocks;
```

### ABS

Digunakan untuk menghitung nilai absolut dari suatu bilangan, yaitu menghilangkan tanda negatif dari suatu angka.

```sql
ABS(numeric_expression)

-----------

SELECT ABS(-25.5) AS absolute_value;
```

### ROUND

Digunakan untuk membulatkan angka ke sejumlah digit desimal tertentu.

```sql
ROUND(number, decimals)

-----------

SELECT ROUND(10.4567, 2) AS result;

result
-------
10.46
```

## DATE FUNCTION

Date function adalah fungsi yang yang digunakan untuk mengelola dan memanipulasi data tanggal dan waktu.

Sumber Belajar:
[MySQL Date Functions](https://www.mysqltutorial.org/mysql-date-functions/)

### CURRENT_TIMESTAMP / NOW()

`CURRENT_TIMESTAMP` atau `NOW()` merupakan fungsi untuk mengembalikan waktu saat ini dari sistem server Basis Data.

```sql
SELECT NOW() AS current_date_time;

current_date_time
-----------------------
2024-10-29 07:00:21
```

### MONTHNAME / DAYNAME

Di MySQL, untuk mendapatkan nama bulan atau nama hari dari sebuah tanggal, kita bisa menggunakan `MONTHNAME()` dan `DAYNAME()`.

```sql
SELECT MONTHNAME(NOW()) AS 'Current Month';

Current Month
---------------------------
October
```

### YEAR / MONTH / DAY / EXTRACT

Untuk mendapatkan angka spesifik tahun, bulan, atau hari, MySQL menyediakan fungsi `YEAR()`, `MONTH()`, `DAY()`, atau yang lebih fleksibel menggunakan `EXTRACT()`.

```sql
SELECT YEAR(NOW()) AS 'Current Year';

Current Year
---------------------------
2024
```

### TIMESTAMPDIFF

Untuk mencari selisih waktu atau tanggal di MySQL, kita menggunakan `TIMESTAMPDIFF(unit, waktu_awal, waktu_akhir)`.

```sql
SELECT TIMESTAMPDIFF(YEAR, '2019-12-31 23:59:59', '2024-01-01 00:00:00') AS diff_in_year;

diff_in_year
-----------
4
```

## Kontributor

- Rizky Fajar Triwibowo
- Nadhifal Azharudia Atmaja
- Aqila Ramdhan Fuady Latief

:::warning
## Credits

Tutorial ini dikembangkan oleh Asisten Praktikum DBMS 2026. Segala tutorial serta instruksi yang dicantumkan pada repositori ini dirancang sedemikian rupa sehingga mahasiswa yang sedang mengambil mata kuliah Basis Data dapat menyelesaikan tutorial saat sesi lab berlangsung.
:::