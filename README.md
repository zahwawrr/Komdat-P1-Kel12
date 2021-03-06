# Aplikasi Web Openfire
<p align="center">
  <img width="300" src="https://chatsdk.co/wp-content/uploads/2017/12/openfire-1.png">
</p>

[Sekilas Tentang](#Sekilas-Tentang) | [Instalasi](#Instalasi) | [Konfigurasi](#Konfigurasi) | [Maintenance](#Maintenance) | [Otomatisasi](#otomatisasi) | [Cara Pemakaian](#cara-pemakaian) | [Pembahasan](#pembahasan) | [Referensi](#referensi)
:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:

## Anggota
| Nama              | NIM        |
| -------------     | -----------|
| Lathifah Kurnia K | G64180023  |
| Ulfainil Aisyah   | G64180045  |
| Hana Salsabila    | G64180051  |
| Zahwa Wahyu Riana | G64180070  |
## Sekilas Tentang
[`^ kembali ke atas ^`](#)

**Openfire** merupakan sebuah server *Real Time Collaboration* (RTC) yang berlisensi Open Source Apache. Aplikasi ini bersifat *open protocol* (dapat digunakan semua orang secara gratis dan bebas) untuk mengirimkan pesan instan. Aplikasi berjenis *Extensible Messaging and Presence Protocol* (XMPP) ini sangat mudah untuk di-*install* dan digunakan, namun dapat memberikan layanan keamanan dan performa yang baik. Openfire biasanya digunakan pada skala enterprise untuk komunikasi internal.

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

### Setting Port Forwarding di VM
Sebelum mengakses VM dari Host, harus dilakukan *port forwarding* terlebih dahulu.
* Pada VirtualBox, pilih pengaturan *Settings* -> *Network* -> *Advanced* -> *Port Forwarding*
* Tambahkan tiga rule *port forwarding*, yaitu ```http```, ```ssh``` dan ```Openfire``` (untuk aplikasi)
![port](https://user-images.githubusercontent.com/60166539/111415215-0c46e300-8714-11eb-9bb8-f08b6288819c.png)
* Klik *OK*, kemudian lanjutkan pengaturan dengan menjalankan VM Ubuntu
* Buka terminal pada VM, lalu pastikan seluruh paket sistem adalah versi yang terbaru
  ```
  $ sudo apt update
  ```
* Install server OpenSSH
  ```
  $ sudo apt-get install openssh-server
  ```
* Aktifkan OpenSSH dan pastikan sudah aktif
   ```
   $ sudo systemctl enable ssh
   $ sudo systemctl start ssh
   $ sudo systemctl status ssh
   ```
  
### Proses Instalasi:
#### 1. Instalasi Kebutuhan Sistem
* Pada *Command Prompt* Host, akses VM dengan *username* dan *password* VM
  ```
  $ ssh username@localhost -p 2200
  ```
* Pastikan seluruh paket sistem adalah versi yang terbaru
  ```
  $ sudo apt update
  ```
* Jika terdapat paket yang bukan merupakan versi terbarunya, lakukan upgrade paket
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
  $ sudo mysql -uopenfireuser -pOpenFirePWD
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
![Spark](https://opensourcesolution.com.br/wp-content/upload/2016/11/logo_spark.png)

Agar client dapat terhubung ke Openfire, maka dibutuhkan software client Spark untuk di install pada sistem. **Spark** adalah aplikasi instant messaging client yang bersifat open-source. Spark juga dirilis oleh igniterealtime yang merupakan vendor dari OpenFire. Spark memiliki interface yang dapat mengelola obrolan secara real-time dalam contact window yang dapat diintegrasikan dengan OpenFire server. Spark yang diintegrasikan dengan OpenFire dapat menjadi alternatif untuk menggunakan jaringan public Instant Messaging yang tidak aman.
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

  ![spark0 (2)](https://user-images.githubusercontent.com/60083962/111339966-c6a20000-86aa-11eb-8309-19a947ef511b.PNG)
  
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
  ![Screenshot (106)](https://user-images.githubusercontent.com/60083962/111340241-0a950500-86ab-11eb-9475-d44cd939eadf.png)
  
  Ketika server chat sudah digunakan, admin dapat melihat aktivitas yang terjadi pada server meliputi **pengguna dan percakapan yang sedang aktif**, **packets per minute**, **trafik server**, dan lain-lain.
  ![Screenshot (110)](https://user-images.githubusercontent.com/60083962/111337364-9bb6ac80-86a8-11eb-9966-78e8a892b805.png)
  
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
  
    ![spark1FIX (2)](https://user-images.githubusercontent.com/60083962/111338207-547ceb80-86a9-11eb-8b8f-8e613667298d.PNG)
  * Pada opsi **Advanced**, masukkan alamat domain Openfire, dan tinggalkan port nya default
  
    ![spark2 (2)](https://user-images.githubusercontent.com/60083962/111338257-5e9eea00-86a9-11eb-90a3-ae945326df63.PNG)
  * Klik login dan login pun berhasil dilakukan
  
    ![spark3 (2)](https://user-images.githubusercontent.com/60083962/111338268-6068ad80-86a9-11eb-94c1-234e2382e09b.PNG)

#### 4. Komunikasi chat dengan client lain
  * Supaya antar client dapat berkomunikasi, kita perlu menambahkan kontak dari client lain yang dibuat di Openfire
  
    ![spark_addcontact1 (2)](https://user-images.githubusercontent.com/60083962/111338460-89893e00-86a9-11eb-96ef-655106df2b7b.PNG)
  * Isi username Client lain. Disini saya akan menambahkan kontak client Ulfa, lalu pilih group nya dan terakhir klik Add
 
    ![1_ulfa](https://user-images.githubusercontent.com/60083962/111328772-1ed40480-86a1-11eb-955e-3e660d04b5fc.PNG)
  * Setelah itu Client Ulfa akan mendapat notifikasi permintaan pertemanan oleh Hana seperti berikut. Jika Client Ulfa memilih accept, maka Client Hana juga akan menerima notifikasi permintaan pertemanan seperti Client Zahwa.
  
    ![1_askask](https://user-images.githubusercontent.com/60083962/111331088-25fc1200-86a3-11eb-8c44-2baebb463ef6.PNG)
  * Setelah saling menerima permintaan pertemanan, kedua user akan memiliki kontak satu sama lain. Untuk memulai Chat, klik kanan pada nama kontak yang ingin diajak chat, lalu klik start Chat
  
    ![12231](https://user-images.githubusercontent.com/60083962/111339740-93f80780-86aa-11eb-9fe5-9f830323f969.PNG)
  * Kemudian chatting antar user client sudah dapat dilakukan.
  
    ![1_chatFIX](https://user-images.githubusercontent.com/60083962/111339168-21872780-86aa-11eb-836e-14432da654ee.PNG)

    
## Pembahasan
[`^ kembali ke atas ^`](#)
### 1. Pendapat tentang aplikasi OpenFire
  * **Kelebihan**
    * Berbasis Java, bisa digunakan pada berbagai OS
    * Lebih stabil dan matang dalam pengembangan
    * Proses instalasi, konfigurasi, administrasi, manajemen dan penggunaan yang mudah
    * Gratis, opensource dan tersedia banyak plugin untuk pengembangan fitur
    * Cukup satu server dengan spesifikasi sesuai kebutuhan jumlah client
    * Banyak tersedia aplikasi client karena menggunakan protokol standar
    * Mendukung client/user berskala besar
    * Mendukung beberapa jenis Database
    * Mendukung komunikasi internal yang aman (grup komunikasi)
  
  * **Kekurangan**
    * Server harus berada dalam semua jangkauan yang membutuhkan layanan
    * Database tidak bisa diubah setelah terinstal jika menggunakan embedded database dari OpenFire
    * Dibutuhkan reboot agar tidak terjadi crash
    * Tidak mendukung panggilan suara dan video
    
### 2. Perbandingan dengan aplikasi eJabberd
  * **Kelebihan:**
    * Openfire lebih fleksibel untuk kebutuhan *gateway* ke aplikasi *Instant Messaging* lainnya seperti Yahoo, MSN, AIM, dll.
    * Openfire lebih mudah dalam segi instalasi dan pemeliharaan
    * Openfire dibangun menggunakan bahasa Java, sedangkan eJabberd menggunakan bahasa Erlang yang belum familiar digunakan masyarakat.
    * Openfire lebih didukung oleh database seperti SQL, dll.

  * **Kekurangan**
    * Ejabberd sudah memiliki fitur clustering dan pengaturannya lebih mudah, sedangkan Openfire belum tersedia fitur *clustering*.
    * Di sisi lain bahasa Erlang juga dapat mempermudah pemakaian karena tidak memerlukan database maupun web eksternal karena sudah termasuk ke dalam paket Erlang.
    * Konfigurasi dapat dilakukan selagi proses berjalan, sehingga tidak diperlukan *temporary server shutdown*.

## Referensi
[`^ kembali ke atas ^`](#)

1. [How To Install OpenFire Chat on Ubuntu 20 19 18 LTS](https://www.youtube.com/watch?v=Ph0UWgKUruY)

2. [How to Install Spark IM 2.9.4 – Instant Messaging Client on Linux](https://linuxhint.com/spark-im-client-2-8-2-messaging-linux/)

3. [Ignite Realtime: Openfire Server](http://www.igniterealtime.org/projects/openfire/)

4. [Openfire Reviews & Product Details](https://www.g2.com/products/openfire/reviews#survey-response-4587040)

5. [Openfire vs eJabberd](https://discourse.igniterealtime.org/t/openfire-vs-ejabberd/48904/7)

6. [XMPP server : ejabberd vs openfire vs prosody](https://stackoverflow.com/questions/33596842/xmpp-server-ejabberd-vs-openfire-vs-prosody)
