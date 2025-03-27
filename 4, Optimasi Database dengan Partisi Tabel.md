
# **Optimasi Database dengan Partisi Tabel di MySQL**  

## **Latar Belakang**  
Optimalisasi kinerja query merupakan tantangan penting dalam pengelolaan basis data berskala besar, terutama ketika jumlah data terus meningkat. Salah satu pendekatan efektif untuk meningkatkan efisiensi penyimpanan dan pengambilan data adalah penerapan **partisi tabel**. Teknik partisi membagi data ke dalam bagian-bagian yang lebih kecil dan terstruktur, sehingga mempercepat proses akses serta meningkatkan keteraturan penyimpanan data.

## **Problem**  
Seiring pertumbuhan data dalam tabel yang besar, beberapa masalah umum yang sering muncul antara lain:  
1. **Query Lambat** – Saat jumlah data meningkat, pencarian berdasarkan kondisi tertentu menjadi lebih lambat.  
2. **Pengelolaan Data Sulit** – Pemeliharaan dan backup data menjadi lebih kompleks.  
3. **Beban Server Tinggi** – Karena semua data disimpan dalam satu tabel besar, penggunaan sumber daya server meningkat drastis.  

## **Pembahasan**  
Dengan menerapkan partisi tabel, kita bisa mendapatkan keuntungan berikut:  
- **Peningkatan Performa Query** – Query yang mengakses hanya satu partisi akan lebih cepat dibandingkan mengakses keseluruhan tabel.
- **Efisiensi Penyimpanan** – Data dapat disimpan lebih terorganisir, misalnya dalam storage yang berbeda.  
- **Manajemen Data Lebih Mudah** – Partisi dapat dihapus atau ditambahkan secara independen tanpa mempengaruhi seluruh tabel.  

Namun, ada beberapa hal yang perlu diperhatikan dalam penggunaan partisi:  
- **Tidak Semua Query Mendukung Partisi** – Beberapa operasi seperti `JOIN` dan `FOREIGN KEY` bisa menjadi lebih kompleks.
- **Overhead Administrasi** – Memerlukan perencanaan awal yang matang untuk menentukan strategi partisi yang optimal.  

## **Kesimpulan**  
Penggunaan partisi pada tabel tr_penjualan_partisi menunjukkan peningkatan performa yang signifikan ketika query menggunakan filter berdasarkan tgl_transaksi. Hal ini terjadi karena MySQL hanya membaca bagian tabel yang relevan, menghindari pemindaian seluruh tabel (full table scan). Dengan demikian, partisi sangat efektif untuk skenario yang sering melakukan pencarian data berdasarkan kolom yang dijadikan dasar partisi.

Namun, saat query menggunakan filter pada kolom lain seperti kode_cabang, performa justru bisa lebih lambat dibandingkan tabel tanpa partisi. MySQL tetap harus membaca semua partisi untuk menemukan data yang sesuai, sehingga keuntungan partisi menjadi kurang optimal. Oleh karena itu, penggunaan partisi harus disesuaikan dengan pola query yang paling sering digunakan agar memberikan manfaat maksimal dalam pengolahan data.

## **Bukti Dukung**  
1. Pada tabel tr_penjualan, lakukan scenario sebagai berikut: 
a. Redesaian tabel tr_penjualan, tambahkan partisi pada tabel tersebut. Sehingga  ada tabel baru tr_penjualan_partisi
```sql
CREATE TABLE tr_penjualan_partisi ( 
 tgl_transaksi DATETIME DEFAULT NULL, 
 kode_cabang VARCHAR(10) DEFAULT NULL, 
 kode_kasir VARCHAR(10) DEFAULT NULL, 
 kode_item VARCHAR(7) DEFAULT NULL, 
 kode_produk VARCHAR(12) DEFAULT NULL, 
 jumlah_pembelian INT(11) DEFAULT NULL,  
 harga INT(6) DEFAULT NULL 
) 
PARTITION BY RANGE (YEAR(tgl_transaksi)) ( 
 PARTITION p0 VALUES LESS THAN (2008), 
 PARTITION p1 VALUES LESS THAN (2009), 
 PARTITION p2 VALUES LESS THAN (2010), 
 PARTITION p3 VALUES LESS THAN (2011), 
 PARTITION p4 VALUES LESS THAN (2012), 
 PARTITION p5 VALUES LESS THAN (2013), 
 PARTITION p6 VALUES LESS THAN (2014), 
 PARTITION p7 VALUES LESS THAN (2015) 
);
```
Hasil <br>
![image](https://github.com/user-attachments/assets/211ec94f-6038-43b7-b4ec-20a9f189c5fa)

Menambahkan data ke table tr_penjualan_partisi dari tahun 2008 - 2014
```sql
INSERT INTO tr_penjualan_partisi (tgl_transaksi, kode_cabang, kode_kasir, kode_item,  kode_produk, jumlah_pembelian, harga) ( 
SELECT tgl_transaksi, kode_cabang, kode_kasir, kode_item, kode_produk, jumlah_pembelian, harga_produk FROM tr_penjualan )
```
Hasil <br>
![image](https://github.com/user-attachments/assets/ce2f82dd-8c33-4200-a154-76f947553c4d)

Query record sesuai partisi akan ditampilkan seperti  berikut:
```sql
SELECT TABLE_NAME, PARTITION_NAME, TABLE_ROWS 
FROM INFORMATION_SCHEMA.PARTITIONS 
WHERE TABLE_NAME = 'tr_penjualan_partisi'; 
```
Hasil <br>
![image](https://github.com/user-attachments/assets/4ad62381-f90b-4f6c-9aa4-85d56593f829)

b. Redesaian tabel tr_penjualan, tambahkan partisi pada tabel tersebut. Sehingga  ada tabel baru tr_penjualan_raw
```sql
CREATE TABLE tr_penjualan_raw ( 
 tgl_transaksi DATETIME DEFAULT NULL, 
 kode_cabang VARCHAR(10) DEFAULT NULL, 
 kode_kasir VARCHAR(10) DEFAULT NULL, 
 kode_item VARCHAR(7) DEFAULT NULL, 
 kode_produk VARCHAR(12) DEFAULT NULL, 
 jumlah_pembelian INT(11) DEFAULT NULL,  
 harga INT(6) DEFAULT NULL 
);
```
Hasil <br>
![image](https://github.com/user-attachments/assets/8f347184-e0f3-4892-be9a-39d0c0b9e312)

Menambahkan data ke table tr_penjualan_raw dari tahun 2008 - 2014
```sql
INSERT INTO tr_penjualan_raw (tgl_transaksi, kode_cabang, kode_kasir, kode_item,  kode_produk, jumlah_pembelian, harga) ( 
SELECT tgl_transaksi, kode_cabang, kode_kasir, kode_item, kode_produk, jumlah_pembelian, harga_produk FROM tr_penjualan )
```
Hasil <br>
![image](https://github.com/user-attachments/assets/5b53145d-7bca-40b0-a209-d79948d57192)

Query record sesuai partisi akan ditampilkan seperti  berikut:
```sql
SELECT TABLE_NAME, PARTITION_NAME, TABLE_ROWS 
FROM INFORMATION_SCHEMA.PARTITIONS 
WHERE TABLE_NAME = 'tr_penjualan_raw'; 
```
Hasil <br>
![image](https://github.com/user-attachments/assets/649f55e8-24c9-44b5-b758-837965028b70)


c. Pengujian tabel 
Jalankan query berikut dengan perulangan 10x. lakukan pencatatan waktu running setiap query. Dan ambil rata-ratanya.

Table Raw
```sql
SELECT * FROM tr_penjualan_raw WHERE tgl_transaksi > DATE('2010-08-01') AND tgl_transaksi < DATE('2011-07-31') 
```
Hasil waktu eksekusi:
| Percobaan | Waktu (s) |
|-----------|----------|
| 1         | 15,632   |
| 2         | 16,339   |
| 3         | 15,377   |
| 4         | 16,221   |
| 5         | 15,221   |
| 6         | 15,398   |
| 7         | 15,117   |
| 8         | 15,022   |
| 9         | 15,037   |
| 10        | 18,866   |
**Rata-rata: 15,823 s**

Table Partisi
```sql
SELECT * FROM tr_penjualan_partisi WHERE tgl_transaksi > DATE('2010-08-01') AND tgl_transaksi < DATE('2011-07-31')
```
Hasil waktu eksekusi:
| Percobaan | Waktu (s) |
|-----------|----------|
| 1         | 11,172   |
| 2         | 10,918   |
| 3         | 10,955   |
| 4         | 11,367   |
| 5         | 10,555   |
| 6         | 10,723   |
| 7         | 10,679   |
| 8         | 11,251   |
| 9         | 10,708   |
| 10        | 10,731   |
**Rata-rata: 10,906 s**


## **Referensi**  
- [Dokumentasi MySQL Partisi Tabel](https://dev.mysql.com/doc/refman/8.0/en/partitioning.html)  
- [Praktikum Sistem Manajemen Basis Data](https://drive.google.com/file/d/1YGY-s6ULwffHcAX69vc4-AAlst_k1uw7/view?usp=sharing)
