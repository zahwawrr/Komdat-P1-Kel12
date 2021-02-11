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

