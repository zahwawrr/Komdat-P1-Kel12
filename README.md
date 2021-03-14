# Komdat-P1-Kel12
![openfire-1](https://user-images.githubusercontent.com/60083962/111074355-2666a280-8515-11eb-8b61-6dd9403205fb.png)
[Tentang](#Tentang) | [Instalasi](#Instalasi) | [Konfigurasi](#Konfigurasi) | [Maintenance](#Maintenance) | [Otomatisasi](#otomatisasi) | [Cara Pemakaian](#cara-pemakaian) | [Pembahasan](#pembahasan) | [Referensi](#referensi)
:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:

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
* Pastikan seluruh paket sistem adalah versi yang terbaru
  ```
  $ sudo apt  update
  ```
* Install MySQL dan pastikan sudah terinstall
  ```
  $ sudo apt install mysql-server
  $ mysql --version
  ```
* Jika MySQL sudah terinstall, selanjutnya install Apache dan pastikan sudah terinstall
  ```
  $ sudo apt install apache2
  $ sudo systemctl status apache2
  ```
* Jika Apache sudah terinstall, selanjutnya install OpenJDK dan pastikan sudah terinstall
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
* Pindah ke direktori ```/opt``` kemudian unduh OpenFire dan pastikan OpenFire sudah terunduh
  ```
  $ cd /opt
  $ sudo wget https://www.igniterealtime.org/downloadServlet?filename=openfire/openfire_4.6.0_all.deb
  $ ll
  ```
* Install OpenFire dari official websitenya
  ```
  $ sudo apt install ./downloadServlet?filename=openfire/openfire_4.6.0_all.deb
  ```
* Mulai dan aktifkan OpenFire service, kemudian pastikan OpenFire sudah aktif
  ```
  $ sudo systemctl start openfire
  $ sudo systemctl enable openfire
  $ sudo systemctl status openfire
  ```
#### 4. Import skema database OpenFire
* Login ke MySQL server menggunakan ```openfireuser``` sebagai user-nya
  ```
  $ sudo mysql -uopenfireuser -popenfirePWD
  ```
* Pindah ke database ```openfiredb``` yang telah dibuat sebelumnya, kemudian import skema database OpenFire. Setelah itu verifikasi bahwa skema telah ter-*import*
  ```
  use openfiredb;
  source /usr/share/openfire/resources/database/openfire_mysql.sql;
  show tables;
  exit;
  ```
#### 5. Konfigurasi Firewall jika terdapat Firewall pada sistem
* Allow port 9090, 9091, 5222, 7777 pada Firewall
  ```
  $ sudo ufw allow 9090
  $ sudo ufw allow 9091
  $ sudo ufw allow 5222
  $ sudo ufw allow 7777
  ```
#### 6. Kunjungi alamat IP web server untuk langkah instalasi selanjutnya
OpenFire secara default berjalan pada port 9090, hubungkan server local dengan port 9090 pada browser ```localhost:9090/setup/index.jsp```
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
* **Pengaturan profile**
  * Pilih sistem ***user*** dan ***group*** sesuai dengan kebutuhan. Pada instalasi kali ini, dibiarkan *default*.
![Screenshot from 2021-02-12 01-29-26](https://user-images.githubusercontent.com/60166539/110608343-bd062d00-81be-11eb-961c-488b6bf596be.png)
* **Pengaturan akun admin**
  * Kolom **Admin Email Address** diisikan dengan email admin yang akan mempunyai kontrol terhadap server.
  * Kolom **New Password** dan **Confirm Password** diisi dengan kata sandi sesuai keinginan admin.
![Screenshot from 2021-02-12 01-30-09](https://user-images.githubusercontent.com/60166539/110608784-238b4b00-81bf-11eb-8e57-acb7a9a8e504.png)
* **Server siap digunakan**
* ![Screenshot from 2021-02-12 01-31-00](https://user-images.githubusercontent.com/60166539/110609050-6a794080-81bf-11eb-9884-8c598ba4bc32.png)

#### 7. Instalasi Spark
Agar client dapat terhubung ke Openfire, maka dibutuhkan software client Spark untuk di install pada sistem. 
* Pastikan pada sistem telah terinstall Java, jika belum install terlebih dahulu
  ```
  $ sudo apt install default-jre
  ```
* Jika Java sudah terinstall, selanjutnya download Spark versi terbaru
  ```
  $ wget – O Spark_2_9_4.tar.gz http://igniterealtime.org downloadServlet?filename=spark/spark_2_9_4.tar.gz
  ```
* File Spark yang telah terunduh dapat dilihat pada direktori home. Jalankan perintah ```/opt``` untuk mengekstrak file di direktori tersebut
  ```
  $ sudo tar -zxvf Spark_2_9_4.tar.gz -C /opt/
  ```
* Pindahkan file dari folder "Spark" ke folder baru "spark"
  ```
  $ sudo mv /opt/Spark /opt/spark
  ```
* Ubah direktori
  ```
  $ cd /opt/Spark
  ```
* Buka terminal text editor dan buat file dengan nama ```spark.desktop``` pada direktori ```/usr/share/apps```
  ```
  $ sudo nano /usr/share/applications/spark.desktop
  ```
* Salin teks ke editor GNU Nano
  ```
  [Desktop Entry]

  Name=Spark
  Version=2.8.2.2 Version=2.8.2.2
  GenericName=Spark Spark
  X-GNOME-FullName=Spark
  Comment=ignite realtime Spark IM client
  Type=Application
  Categories=Application;Utility;
  Path=/opt/spark
  Exec=/bin/bash Spark
  Terminal= false
  StartupNotify=true
  Icon=/opt/spark/spark.png/fFLQ0.png
  EnvironmentObjective=Gnome
  ```
* Jalankan aplikasi dari direktori ```/opt/spark/```
  ```
  $ ./Spark
  ```
* Instalasi selesai dan aplikasi dapat dijalankan, gunakan akun yang telah dibuat pada OpenFire untuk login

## Konfigurasi
Terdapat banyak konfigurasi yang dapat dilakukan pada server OpenFire.
![Screenshot from 2021-03-14 14-10-25](https://user-images.githubusercontent.com/60166539/111061697-411a2680-84d7-11eb-8707-1a83923028f2.png)

Beberapa pilihan konfigurasi yang disediakan oleh OpenFire adalah sebagai berikut:

**1. Registration Settings**
  ![Screenshot from 2021-03-14 14-11-48](https://user-images.githubusercontent.com/60166539/111063947-ff43ad00-84e3-11eb-971c-cbd9123a8d55.png)
  Pada pilihan ini, admin dapat mengizinkan *user* untuk **membuat akun sendiri, mengganti password, *login* menggunakan akun anonim, *login* dari server lain selain server lokal, dan mengatur mekanisme SASL.**
  
**2. Offline Messages**
  ![Screenshot from 2021-03-14 14-14-02](https://user-images.githubusercontent.com/60166539/111064218-56964d00-84e5-11eb-8fe4-0d0ef126a040.png)
  Pada pilihan ini, OpenFire memberikan layanan untuk menyimpan pesan yang dikirim dari pengguna yang sedang *online* ke pengguna lainnya yang sedang *offline*. Pesan yang disimpan tersebut tentunya akan memakan memori penyimpanan, maka dari itu disediakan konfigurasinya. Admin dapat melakukan konfigurasi **sifat penyimpanan pesan *offline*, kapasitas penyimpanan pesan *offline* per-*user*, pembersihan pesan *offline*, seberapa lama pesan *offline* tersebut akan disimpan dan seberapa lama pesan *offline* tersebut dapat di-*retrieve***
  
**3. Plugin** <br/>
Jika pilihan konfigurasi tidak disediakan oleh OpenFire, maka admin dapat menambahkan *plugin* sesuai dengan kebutuhan perusahaan. Contohnya **Monitoring Service** untuk mengetahui statistik dan arsip dari chat yang terjadi.
  ![Screenshot from 2021-03-14 11-35-03](https://user-images.githubusercontent.com/60166539/111063615-46c93980-84e2-11eb-990b-eef171bfa443.png)
  ![Screenshot from 2021-03-14 14-05-30](https://user-images.githubusercontent.com/60166539/111063617-4a5cc080-84e2-11eb-91f5-749c6fcd5334.png)
  
## Maintenance

## Otomatisasi

## Cara Pemakaian
#### 1. Login pada halaman admin server OpenFire.
  * Kolom **Username** diisikan dengan **Admin**
  * Kolom **Password** diisikan dengan kata sandi yang telah dibuat pada proses instalasi.
![Screenshot from 2021-02-12 01-31-32](https://user-images.githubusercontent.com/60166539/110609354-b6c48080-81bf-11eb-97cd-d8ff37592375.png)

#### 2. Create user pada OpenFire 
  * Pilih bagian **Users/Groups**

## Pembahasan

## Referensi
[How To Install OpenFire Chat on Ubuntu 20 19 18 LTS](https://www.youtube.com/watch?v=Ph0UWgKUruY)

[How to Install Spark IM 2.9.4 – Instant Messaging Client on Linux](https://linuxhint.com/spark-im-client-2-8-2-messaging-linux/)




