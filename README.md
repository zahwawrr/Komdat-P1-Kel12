# Komdat-P1-Kel12

## Tentang
**Openfire** merupakan sebuah server *Real Time Collaboration* (RTC) yang berlisensi Open Source Apache. Aplikasi ini bersifat *open protocol* (dapat digunakan semua orang secara gratis dan bebas) untuk mengirimkan pesan instan. Aplikasi berjenis *Extensible Messaging and Presence Protocol* (XMPP) ini sangat mudah untuk di-*install* dan digunakan, namun dapat memberikan layanan keamanan dan performa yang baik.

## Instalasi
### **Kebutuhan Sistem**:
**Software**
- Database dengan JDBC driver, atau embedded pure Java database
- Java 5 (JRE 1.5+)
- OpenJDK (8+)
- Windows, Linux, Solaris, Unix atau MacOS
- MySQL (5.x+)
- Apache Web Server (1.3+)

**Hardware**
- 1-500 pengguna: minimal 384MB RAM dan prosesor 1.5GHz
- 501-10.000 pengguna: minimal 768MB RAM dan prosesor 3.0 GHZ
- 10.001-25.000 pengguna: minimal 1.5 GB dan dua prosesor 3 GHz, dan satu modul *connection manager* pada mesin yang sama.
- 25.0001-100.000 pengguna: minimal 2.0Gb RAM, dua prosesor 3 GHz, dan antara 1-4 modul *connection managers* dengan ukuran yang sesuai di setiap mesin.

### Proses Instalasi:
#### 1. Instalasi Kebutuhan Sistem
1.1. Pastikan seluruh paket sistem adalah versi yang terbaru
```
$ sudo apt  update
```
1.2. Install MySQL dan pastikan sudah terinstall
```
$ sudo apt install mysql-server
$ mysql --version
```
1.3. Jika MySQL sudah terinstall, selanjutnya install Apache dan pastikan sudah terinstall
```
$ sudo apt install apache2
$ sudo systemctl status apache2
```
1.4. Jika Apache sudah terinstall, selanjutnya install OpenJDK dan pastikan sudah terinstall
```
$ sudo apt install openjdk-8-jdk -y
$ java -version
```
#### 2. Pembuatan database untuk OpenFire
```
$ sudo mysql -uroot -p
  create database openfiredb;
  create user openfireuser@localhost identified by 'OpenFirePWD';
  grant all on openfiredb.* to openfireuser@localhost;
  flush privileges;
  exit;
```
#### 3. Unduh dan Install OpenFire
3.1. Pindah ke direktori ```/opt``` kemudian unduh OpenFire dan pastikan OpenFire sudah terunduh
```
$ cd /opt
$ sudo wget https://www.igniterealtime.org/downloadServlet?filename=openfire/openfire_4.6.0_all.deb
$ ll
```
3.2. Install OpenFire
```
$ sudo apt install ./downloadServlet?filename=openfire/openfire_4.6.0_all.deb
```
3.3. Mulai dan aktifkan OpenFire service, kemudian pastikan OpenFire sudah aktif
```
$ sudo systemctl start openfire
$ sudo systemctl enable openfire
$ sudo systemctl status openfire
```
#### 4. Import skema database OpenFire
4.1. Login ke MySQL server menggunakan ```openfireuser``` sebagai user-nya
```
$ sudo mysql -uopenfireuser -popenfirePWD
```
4.2. Pindah ke database ```openfiredb``` yang telah dibuat sebelumnya, kemudian import skema database OpenFire. Setelah itu verifikasi bahwa skema telah ter-*import*
```
use openfiredb;
source /usr/share/openfire/resources/database/openfire_mysql.sql;
show tables;
exit;
```
#### 5. Konfigurasi Firewall jika terdapat Firewall pada sistem
5.1. Allow port 9090, 9091, 5222, 7777 pada Firewall
```
$ sudo ufw allow 9090
$ sudo ufw allow 9091
$ sudo ufw allow 5222
$ sudo ufw allow 7777
```
#### 6. Kunjungi alamat IP web server untuk langkah instalasi selanjutnya
* **Pengaturan bahasa**
![Screenshot from 2021-02-12 01-20-53](https://user-images.githubusercontent.com/60166539/110605431-b3c79100-81bb-11eb-86f7-27f8b80e615f.png)
* **Pengaturan server** 
  * Server akan mengisi kolom persyaratan secara *default* sehingga kolom dapat dibiarkan seperti itu jika memang tidak ingin melakukan perubahan.
![Screenshot from 2021-02-12 01-25-12](https://user-images.githubusercontent.com/60166539/110606812-25eca580-81bd-11eb-9e63-7d55308e01d0.png)
* **Pengaturan database**   
  * Kolom **Database Driver Presets** diisi dengan preset **MySQL**
  * Setelah preset mengisi kolom-kolom sebagai *default*, pada kolom **Database URL** ganti **HOSTNAME** dengan **localhost** dan **DATABASE NAME** dengan **openfiredb**.
  * Kolom **Username** dan **Password** diisi sesuai dengan database yang dibuat saat instalasi.
  * **Minimum Connections**, **Maximum Connections** dan **Connection Timeout** dapat dibiarkan *default* atau diatur sesuai dengan kebutuhan.
![Screenshot from 2021-02-12 01-29-08](https://user-images.githubusercontent.com/60166539/110608117-7e707280-81be-11eb-99f7-2f8fc3b2d923.png)
* P**engaturan profile**
  * Pilih sistem ***user*** dan ***group*** sesuai dengan kebutuhan. Pada instalasi kali ini, dibiarkan *default*.
![Screenshot from 2021-02-12 01-29-26](https://user-images.githubusercontent.com/60166539/110608343-bd062d00-81be-11eb-961c-488b6bf596be.png)
* **Pengaturan akun admin**
  * Kolom **Admin Email Address** diisikan dengan email admin yang akan mempunyai kontrol terhadap server.
  * Kolom **New Password** dan **Confirm Password** diisi dengan kata sandi sesuai keinginan admin.
![Screenshot from 2021-02-12 01-30-09](https://user-images.githubusercontent.com/60166539/110608784-238b4b00-81bf-11eb-8e57-acb7a9a8e504.png)
* **Server siap digunakan**
* ![Screenshot from 2021-02-12 01-31-00](https://user-images.githubusercontent.com/60166539/110609050-6a794080-81bf-11eb-9884-8c598ba4bc32.png)

### Konfigurasi

### Maintenance

### Otomatisasi

### Cara Pemakaian
1. Login pada halaman admin server OpenFire.
  * Kolom **Username** diisikan dengan **Admin**
  * Kolom **Password** diisikan dengan kata sandi yang telah dibuat pada proses instalasi.
![Screenshot from 2021-02-12 01-31-32](https://user-images.githubusercontent.com/60166539/110609354-b6c48080-81bf-11eb-97cd-d8ff37592375.png)

### Referensi
[How To Install OpenFire Chat on Ubuntu 20 19 18 LTS](https://www.youtube.com/watch?v=Ph0UWgKUruY)




