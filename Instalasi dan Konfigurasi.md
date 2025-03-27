# Instalasi dan Konfigurasi MySQL: Panduan Lengkap

## 1. Latar Belakang
MySQL merupakan sistem manajemen basis data relasional (RDBMS) yang banyak digunakan dalam berbagai jenis aplikasi, mulai dari situs web hingga sistem berskala enterprise. Untuk memaksimalkan kinerja MySQL, pemahaman mengenai proses instalasi dan konfigurasi sangatlah penting agar sesuai dengan kebutuhan dan karakteristik sistem yang digunakan.

## 2. Problem
Beberapa tantangan yang sering muncul saat menginstal dan mengonfigurasi MySQL:
- Kesalahan instalasi akibat dependensi yang tidak terpenuhi.
- Penggunaan port default (3306) yang mungkin konflik dengan layanan lain.
- Konfigurasi yang tidak optimal, seperti ukuran buffer yang terlalu kecil.
- Keamanan MySQL yang lemah jika password root tidak diperbarui.

## 3. Solusi/Skenario Aktivitas
Berikut langkah-langkah untuk menginstal dan mengonfigurasi MySQL secara optimal:

### a. Instalasi MySQL
1. Buka website MySQL lalu pilih menu unduhan.
2. Pilih MySQL Community lalu klik.
3. Pilih MySQL Installer for Windows.
4. Pilih yang paling bawah lalu klik download.
5. Klik just start my download.
6. Setelah terdownload akan muncul pada file download.
7. Buka File.
8. Lalu MySQL akan otomatis terbuka lalu pilih server only dan klik next.
9. Klik execute maka product akan terdownload setelah itu klik next terus sampai muncul form untuk mengisi password.
10. Masukkan password lalu next terus.
11. Klik execute lalu finish, next lagi lalu finish lagi maka mysql sudah terinstall.
12. Search mysql 8.0 command line client untuk mensetting.
13. Masukkan password yang sudah dimasukkan tadi setelah itu kita sudah masuk ke dalam root mysql.

### b. Mengubah Port MySQL dari 3306 ke 3309 dan Mengubah `innodb_buffer_pool_size`
1. Membuka MySQL Command Line Client, kemudian login dengan password **“your_password”**.
2. Cek default port dan innodb_buffer_pool_size
   ```sql
   SHOW VARIABLES WHERE Variable_name IN ('port','innodb_buffer_pool_size');
   ```
3. Cari file `my.ini` menggunakan `%PROGRAMDATA%`.
4. Buka file `my.ini` dan ubah nilai `port` dari `3306` ke `3309`.
5. Ubah dari `128M` ke `4096M`.
6. Stop MySQL sebelum menyimpan perubahan.
7. Restart MySQL dan cek kembali port dengan perintah yang sama.

### c. Mengubah Password Root MySQL
1. Lanjutkan dengan mengubah password:
   ```sh
   SELECT user, host, authentication_string FROM mysql.user WHERE user = 'root';
   ```
2. Ubah password dengan perintah:
   ```sql
   ALTER USER 'root'@'localhost' IDENTIFIED BY 'root';
   FLUSH PRIVILEGES;
   ```
3. Verifikasi perubahan dengan login ulang.

### d. Membuat Database Baru
1. Buat database dengan format `kelompok_AB_nama_mhs`:
   ```sql
   CREATE DATABASE 06_A_Veri_mhs;
   ```
2. Cek apakah database berhasil dibuat:
   ```sql
   SHOW DATABASES;
   ```

## 4. Pembahasan
Proses instalasi MySQL perlu diikuti dengan konfigurasi yang tepat agar kinerjanya dapat berjalan secara optimal. Penyesuaian port dapat mencegah terjadinya konflik dengan layanan lain, sementara peningkatan nilai innodb_buffer_pool_size berperan penting dalam mengoptimalkan performa query. Di samping itu, penggantian password root merupakan langkah krusial untuk memastikan keamanan basis data.

## 5. Kesimpulan
Penerapan instalasi dan konfigurasi MySQL yang tepat menjadi langkah awal penting dalam menjaga performa dan keamanan sistem. Penyesuaian parameter seperti port dan alokasi memori perlu dilakukan sesuai kebutuhan. Selain itu, pengaturan keamanan serta dokumentasi setiap perubahan konfigurasi berperan penting dalam mendukung pemeliharaan dan troubleshooting di masa mendatang.

## 6. Bukti Dukung Gambar
Konfigurasi pada MySQL
a.   port dari default 3306 menjadi 3309
−    Masuk ke mysql menggunakan command prompt
 ![1](https://github.com/user-attachments/assets/20fbd5c4-c540-46a9-8e60-5dee0951794f)
 
−    Cek port menggunakan show variables like ‘port’; (before)
  ![2](https://github.com/user-attachments/assets/10922aba-44ce-4ab0-b92d-89b46606a087)
−    Letak my.ini <br>
 ![3](https://github.com/user-attachments/assets/0b789dc6-2d36-4daf-a87a-fd902257d9e5)
 ![4](https://github.com/user-attachments/assets/cc725e18-cf02-431a-af92-3e137771b343)
−    Buka my.ini lalu cari port 3306
![5](https://github.com/user-attachments/assets/92f3843c-f991-4e66-afec-729c44660893)
−    Sebelum ganti port menjadi 3309, stop mysql terlebih dahulu di Terminal
Admin
![6](https://github.com/user-attachments/assets/9bd5a205-0040-4436-82d6-e904b70b40a8)
- Setelah itu ubah port <br>
![7](https://github.com/user-attachments/assets/9a4ff273-7774-4838-9e57-3b53f20a1cae)
-    Setelah port diubah, jalan kan lagi mysql di Terminal Admin
![8](https://github.com/user-attachments/assets/48893f4b-9ae9-4784-b81e-bdeba06f0060)
−    Kemudian jalankan lagi untuk cek port yang telah diubah (After)
![9](https://github.com/user-attachments/assets/91f199f8-6ffa-492c-be65-782ba3173c20)
b.   innodb_buffer_pool_size dr default 16M (menjadi 25% dari RAM )
−    Cara untuk mengubah innodb sama seperti mengubah port, jadi cari innodb
 ![10](https://github.com/user-attachments/assets/fa944ad9-f4c0-4fae-95a3-502b8997bbe3)

-  Kemudian sesuai dengan kapasitas RAM Laptop masing masing
   ![11](https://github.com/user-attachments/assets/b2615da0-1d2f-42c6-bfaf-bc1eaf4064f4)

-	Cek innodb dengan SHOW VARIABLES LIKE 'innodb_buffer_pool_size'; di command prompt (Before)
![12](https://github.com/user-attachments/assets/e7ec9c2b-0526-4b91-bbf0-146eb5e86605)
−    Cek innodb setelah innodb diubah (Ater)
![13](https://github.com/user-attachments/assets/d6cee0c2-9e75-411c-86ae-d99c8c14420d)

6.   lakukan perubahan terhadap password root.
![14](https://github.com/user-attachments/assets/6256e2dc-5a58-44d4-bc9c-95054d333fec)

7.   Buat database dengan nama: kelompok_AB_nama_mhs
![16](https://github.com/user-attachments/assets/2a302a12-7ee9-4f9a-bd82-406581c599da)

## 7. Sumber Referensi
- [MySQL Documentation](https://dev.mysql.com/doc/)
- [Panduan Praktikum Sistem Manajemen Basis Data](https://drive.google.com/file/d/1E1SBJXj0sZxMpt6FOjliDfrdSAc26y47/view?usp=sharing)
