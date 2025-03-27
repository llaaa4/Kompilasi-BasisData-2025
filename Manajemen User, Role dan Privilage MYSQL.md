# Manajemen User, Role, dan Privilege dalam MySQL

## 1. Latar Belakang
Manajemen pengguna pada MySQL memiliki peran yang krusial dalam memastikan keamanan serta pengendalian akses terhadap basis data. Melalui pengelolaan yang sistematis, administrator dapat menetapkan otorisasi akses bagi setiap pengguna, menentukan hak akses yang dimiliki, serta melakukan pemantauan terhadap aktivitas yang dilakukan oleh pengguna dalam sistem.

## 2. Problem
Beberapa tantangan dalam manajemen pengguna di MySQL antara lain:
- **Pengelolaan akun pengguna**: Meliputi pembuatan, penghapusan, dan pengamanan akun.
- **Pemberian hak akses**: Menyesuaikan privilege sesuai kebutuhan pengguna.
- **Manajemen Role**: Mengelompokkan hak akses agar lebih mudah dikelola.
- **Pemantauan aktivitas**: Melacak aktivitas pengguna untuk menjaga keamanan sistem.

## 3. Solusi/Skenario Aktivitas
### a. Pembuatan dan Penghapusan User
- Membuat user baru:
  ```sql
  CREATE USER 'della'@'localhost' IDENTIFIED BY 'della';
CREATE USER 'alivia'@'localhost' IDENTIFIED BY 'alivia';
  ```
- Menghapus user:
  ```sql
 CREATE USER 'veri'@'localhost' IDENTIFIED BY 'veri';
DROP USER ‘veri’@’localhost’;
  ```

### b. Manajemen Hak Akses dan Privilege
- Memberikan hak akses kepada user:
  ```sql
  GRANT SELECT, INSERT ON database_example.* TO 'user_example'@'localhost';
  ```
- Menampilkan hak akses user:
  ```sql
  SHOW GRANTS FOR 'user_example'@'localhost';
  ```

### c. Manajemen Role
Sejak MySQL 8.0, fitur **role** diperkenalkan untuk mempermudah pengelolaan hak akses:
- Membuat role:
  ```sql
 CREATE ROLE 'role_della_select_insert';
  ```
- Memberikan privilege ke role:
  ```sql
  GRANT SELECT, INSERT ON 06_a_veri_mhs.* TO
'role_della_select_insert';
  ```
- Memberikan role kepada user:
  ```sql
  GRANT ‘role_della_select_insert’ TO ‘della’@’localhost’;
  ```
- Menghapus role dari user:
  ```sql
  REVOKE 'role_della_select_insert' FROM 'della'@'localhost';
  ```

### d. Monitoring Akses dan Aktivitas Pengguna
Monitoring dilakukan untuk memeriksa aktivitas pengguna:
- Mengaktifkan general log:
  ```sql
  SET global general_log = 1;
  SET global log_output = 'table';
  ```
- Melihat log aktivitas pengguna:
  ```sql
  SELECT * FROM mysql.general_log;
  ```

## 4. Pembahasan
Dengan penerapan manajemen pengguna, peran (role), dan hak akses (privilege) yang tepat, basis data dapat terjaga keamanannya serta memiliki struktur pengelolaan yang lebih tertata. Administrator juga dapat mengendalikan akses dengan lebih efektif, mencegah perubahan data yang tidak sah, serta memantau aktivitas yang berpotensi mencurigakan.

## 5. Kesimpulan
Keamanan database dapat ditingkatkan dengan pemberian role yang sesuai bagi setiap pengguna agar hanya memiliki akses yang diperlukan. Monitoring aktivitas juga penting untuk mendeteksi tindakan mencurigakan dan menjaga integritas data. Oleh karena itu, manajemen hak akses menjadi dasar utama dalam pengelolaan sistem basis data yang aman dan efisien di lingkungan produksi.

## 6. Bukti Dukung Gambar
1.   Lakukan proses pembuatan username sebanyak jumlah kelompok anda. Tuliskan script
dan tampilkan hasilnya.
![image](https://github.com/user-attachments/assets/86be45d0-8c6f-4706-92e7-ae1ce3ed1cb8)

2.   Lakukan penghapusan username terhadap user yang sudah dibuat. Tuliskan script dan tampilkan hasilnya
![image](https://github.com/user-attachments/assets/f8181c55-20b3-4979-8374-7fd18558b1fe)

3.   Buat role dengan “role_nama_anda_insert_select” → role_andi_select_insert.
 ![image](https://github.com/user-attachments/assets/fa316714-fa6f-4951-86bd-37d69e5e4103)
 
4.   Berikan privilege select, insert kedalam role diatas.
![image](https://github.com/user-attachments/assets/c88f194f-3f72-4726-b8af-e520543710ec)

5.   Buat role dengan “role_nama_anda_create_drop” → role_andi_create_drop.
![image](https://github.com/user-attachments/assets/8b1b63b1-7299-4777-b195-ed056824dedc)

6.   Berikan privilege create, drop kedalam role diatas.
![image](https://github.com/user-attachments/assets/1e083851-26dd-4231-aa31-d83ad13c3b66)

7.   Berikan 2 user kedalam masing-masing role diatas.
![image](https://github.com/user-attachments/assets/885789b3-9a51-490d-b55c-b723d79f2078)

8.   Lakukan pengujian sebelum dan sesudah user diberikan role.
- BEFORE : <br>
![image](https://github.com/user-attachments/assets/9a27874a-a3a0-42a7-a6ca-30492e0e248a)

- AFTER : <br>
![image](https://github.com/user-attachments/assets/fd6a9c95-6c73-4d97-845b-172045f928c1)

9.   Lepas role dari user diatas. Sehingga user menjadi tidak memiliki role.
 ![image](https://github.com/user-attachments/assets/bb4cbfcb-b4e0-4e03-abe2-8dd19e2a069b)

10.  Lakukan  konfigurasi  untuk  proses  monitoring  proses  seperti  contoh  diatas,  dan  lakukan beberapa kali proses query. Kemudian lihat di log nya dan tampilkan hasilnya.  
- TABEL DI USER IRFAN <br>
![image](https://github.com/user-attachments/assets/e854b476-5817-49d0-aaec-02c6d909e944)

- TABEL DI USER ROOT <br>
![image](https://github.com/user-attachments/assets/063d6efb-fb94-45bd-a949-ae2d7bfea6e5)


## 7. Sumber Referensi
- [MySQL Documentation](https://dev.mysql.com/doc/)
- [Panduan Praktikum Sistem Manajemen Basis Data](https://drive.google.com/file/d/11hwQd_RRfSMjA7BCzRSyalOoSSAro3Wa/view?usp=sharing)
