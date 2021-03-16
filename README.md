# Aplikasi Web Openfire
<p align="center">
  <img width="300" src="https://chatsdk.co/wp-content/uploads/2017/12/openfire-1.png">
</p>

[Sekilas Tentang](#Sekilas-Tentang) | [Instalasi](#Instalasi) | [Konfigurasi](#Konfigurasi) | [Maintenance](#Maintenance) | [Otomatisasi](#otomatisasi) | [Cara Pemakaian](#cara-pemakaian) | [Pembahasan](#pembahasan) | [Referensi](#referensi)
:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:

## Sekilas Tentang
[`^ kembali ke atas ^`](#)

**Openfire** merupakan sebuah server *Real Time Collaboration* (RTC) yang berlisensi Open Source Apache. Aplikasi ini bersifat *open protocol* (dapat digunakan semua orang secara gratis dan bebas) untuk mengirimkan pesan instan. Aplikasi berjenis *Extensible Messaging and Presence Protocol* (XMPP) ini sangat mudah untuk di-*install* dan digunakan, namun dapat memberikan layanan keamanan dan performa yang baik.

## Instalasi
[`^ kembali ke atas ^`](#)

### **Kebutuhan Sistem**:
**Software**
- Database dengan JDBC driver, atau embedded pure Java database
- Java 5 (JRE 1.5+)
- OpenJDK (8+)
- Windows, Linux, Solaris, Unix atau MacOS
- MySQL (5.x+)
- Apache Web Server (1.3+)
- Spark Client (Optional)

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
  
  Jika terdapat paket belum versi terbaru, lakukan upgrade paket
  ```
  $ sudo apt upgrade -y
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
![Screenshot from 2021-02-12 01-20-53](https://user-images.githubusercontent.com/60166539/111327573-13340e00-86a0-11eb-8d18-0965264696a0.png)
* **Pengaturan server** 
  * Server akan mengisi kolom persyaratan secara *default* sehingga kolom dapat dibiarkan seperti itu jika memang tidak ingin melakukan perubahan.
![Screenshot from 2021-02-12 01-25-12](https://user-images.githubusercontent.com/60166539/111327606-1a5b1c00-86a0-11eb-805e-e18385cd953f.png)
* **Pengaturan database**   
  * Kolom **Database Driver Presets** diisi dengan preset **MySQL**
  * Setelah preset mengisi kolom-kolom sebagai *default*, pada kolom **Database URL** ganti **HOSTNAME** dengan **localhost** dan **DATABASE NAME** dengan **openfiredb**.
  * Kolom **Username** dan **Password** diisi sesuai dengan database yang dibuat saat instalasi.
  * **Minimum Connections**, **Maximum Connections** dan **Connection Timeout** dapat dibiarkan *default* atau diatur sesuai dengan kebutuhan.
![Screenshot from 2021-02-12 01-29-11](https://user-images.githubusercontent.com/60166539/111327729-365ebd80-86a0-11eb-84f2-9369b251dc86.png)
* **Pengaturan profile**
  * Pilih sistem ***user*** dan ***group*** sesuai dengan kebutuhan. Pada instalasi kali ini, dibiarkan *default*.
![Screenshot from 2021-02-12 01-29-26](https://user-images.githubusercontent.com/60166539/111327768-3f4f8f00-86a0-11eb-90c3-b9823880c42a.png)
* **Pengaturan akun admin**
  * Kolom **Admin Email Address** diisikan dengan email admin yang akan mempunyai kontrol terhadap server.
  * Kolom **New Password** dan **Confirm Password** diisi dengan kata sandi sesuai keinginan admin.
![Screenshot from 2021-02-12 01-30-09](https://user-images.githubusercontent.com/60166539/111327797-45de0680-86a0-11eb-88b8-41fb41109624.png)
* **Server siap digunakan**
![Screenshot from 2021-02-12 01-31-00](https://user-images.githubusercontent.com/60166539/111327823-4c6c7e00-86a0-11eb-94ba-a678f913a254.png)
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
  ![spark0](https://user-images.githubusercontent.com/60083962/111077352-02aa5900-8523-11eb-90d9-d9f8dad545fd.PNG)
  
## Konfigurasi
[`^ kembali ke atas ^`](#)

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
[`^ kembali ke atas ^`](#)

Setting tambahan untuk maintenance secara periodik, misalnya:

- buat backup database tiap pekan
- hapus direktori sampah tiap hari
- dll

## Otomatisasi
[`^ kembali ke atas ^`](#)

Skrip shell untuk otomatisasi instalasi, konfigurasi, dan maintenance.

## Cara Pemakaian
[`^ kembali ke atas ^`](#)

#### 1. Login pada halaman admin server OpenFire.
  * Kolom **Username** diisikan dengan **Admin**
  * Kolom **Password** diisikan dengan kata sandi yang telah dibuat pada proses instalasi.
![Screenshot from 2021-02-12 01-31-32](https://user-images.githubusercontent.com/60166539/110609354-b6c48080-81bf-11eb-97cd-d8ff37592375.png)

#### 2. Create user pada OpenFire 
  * Pilih bagian **Users/Groups**, lalu klik **Create New User**
    ![cara pemakaian 1](https://user-images.githubusercontent.com/60083962/111075022-8b6fc780-8518-11eb-9abf-f5b6c6108526.PNG)
  * Kita dapat menambahkan lebih dari 1 user sesuai dengan kebutuhan
    ![carapemakaian3](https://user-images.githubusercontent.com/60083962/111075161-46986080-8519-11eb-9460-ff03f1331dbb.PNG)
    ![carapemakaian4](https://user-images.githubusercontent.com/60083962/111075164-4ef09b80-8519-11eb-9c7a-e467f30ecfa0.PNG)

#### 3. Login user pada aplikasi client Spark yang telah di install sebelumnya
  * Isi username, password dan domain sesuai dengan user yang telah dibuat sebelumnya di Openfire
    ![spark1FIX](https://user-images.githubusercontent.com/60083962/111076243-1b644000-851e-11eb-8dfe-c4e82bd5592b.PNG)
  * Pada opsi **Advanced**, masukkan alamat domain Openfire, dan tinggalkan port nya default
    ![spark2](https://user-images.githubusercontent.com/60083962/111076248-1f905d80-851e-11eb-8cbf-6d088970636a.PNG)
  * Klik login dan login pun berhasil dilakukan
    ![spark3](https://user-images.githubusercontent.com/60083962/111076303-60887200-851e-11eb-915c-97d58f209d94.PNG)

#### 4. Komunikasi chat dengan client lain
  * Supaya antar client dapat berkomunikasi, kita perlu menambahkan kontak dari client lain yang dibuat di Openfire
    ![spark_addcontact1](https://user-images.githubusercontent.com/60083962/111078228-0213c180-8527-11eb-85a8-fa46f71c59f6.PNG)
  * Isi username Client lain. Disini saya akan menambahkan kontak client zahwa, lalu pilih group nya dan terakhir klik Add
    ![spark_addcontact2ZAHWA](https://user-images.githubusercontent.com/60083962/111078230-03dd8500-8527-11eb-8027-b636799b8949.PNG)
  * Setelah itu Client Zahwa akan mendapat notifikasi permintaan pertemanan oleh Hana seperi berikut. Jika Client Zahwa memilih accept, maka Client Hana juga akan menerima notifikasi permintaan pertemanan seperti Client Zahwa.
  * Setelah saling menerima permintaan pertemanan, kedua user akan memiliki kontak satu sama lain. Untuk memulai Chat, klik kanan pada nama kontak yang ingin diajak chat,lalu klik start Chat
  * Lalu setelah keduanya memasuki room chat, chatting sudah dapat dilakukan.
    
## Pembahasan
[`^ kembali ke atas ^`](#)
#### 1. Pendapat tentang aplikasi OpenFire
  * Kelebihan
    * Berbasis Java, bisa digunakan pada berbagai OS
    * Lebih stabil dan matang dalam pengembangan
    * Proses instalasi, konfigurasi, administrasi, manajemen dan penggunaan yang mudah
    * Gratis, opensource dan tersedia banyak plugin untuk pengembangan fitur
    * Cukup satu server dengan dengan spesifikasi sesuai kebutuhan jumlah client
    * Banyak tersedia aplikasi client karena menggunakan protokol standar
    * Mendukung client/user berskala besar
    * Mendukung beberapa jenis Database
    * Mendukung komunikasi internal yang aman (grup komunikasi)
  
  * Kekurangan
    * Server harus berada dalam semua jangkauan yang membutuhkan layanan
    * Database tidak bisa diubah setelah terinstal jika menggunakan embedded database dari OpenFire
    * Dibutuhkan reboot agar tidak terjadi crash
    * Tidak mendukung panggilan suara dan video


#### 2. Perbandingan dengan aplikasi web lain yang sejenis

## Referensi
[`^ kembali ke atas ^`](#)

1. [How To Install OpenFire Chat on Ubuntu 20 19 18 LTS](https://www.youtube.com/watch?v=Ph0UWgKUruY)

2. [How to Install Spark IM 2.9.4 – Instant Messaging Client on Linux](https://linuxhint.com/spark-im-client-2-8-2-messaging-linux/)

3. [Ignite Realtime: Openfire Server](http://www.igniterealtime.org/projects/openfire/)

4. [Openfire Reviews & Product Details](https://www.g2.com/products/openfire/reviews#survey-response-4587040)


