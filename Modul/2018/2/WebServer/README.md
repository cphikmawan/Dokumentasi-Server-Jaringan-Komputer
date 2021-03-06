# Web Server

## A. Persyaratan Tambahan Mengikuti Sesilab
1. Record A dan PTR pada klampis.com mengarah ke IP Pucang

## B. Penting Untuk Dibaca
1. Pastikan Semua UML bisa connect ke internet baik dapat melakukan koneksi keluar maupun dapat ping dari luar (Khusus DMZ).
2. Pastikan Pucang dan Klampis sudah memiliki memory 256M
3. Ketika mengalami kendala/error **cek syntax dan samakan seperti modul** terlebih dahulu **sebelum** angkat tangan dan berkata **"Mas/Mbak ini kok gak bisa ya?"**


## C. Dasar Teori

### 1. Web Server
Web Server adalah perangkat yang menyediakan layanan akses kepada pengguna melalui protokol HTTP atau HTTPS melalui aplikasi web.

### 2. Apache Web Server
Apache adalah sebuah nama web server yang bertanggung jawab pada request-response HTTP dan logging informasi secara detail

## D. Instalasi Apache

**STEP 1** - Buka uml **Pucang** dan jalankan perintah

    apt-get install apache2

![](gambar/1.PNG)
    
**STEP 2** - Buka browser laptop/komputer masing-masing dan buka web **IP Pucang Masing-Masing Kelompok** sampai muncul halaman Apache

![](gambar/2.PNG)

## E. Instalasi PHP

**STEP 1** - Buka uml **Pucang** dan jalankan perintah

    apt-get install php
    
![](gambar/3.PNG)

**STEP 2** - Test apakah **php** sudah terinstall dengan menjalankan perintah

    php -v
    
![](gambar/4.PNG)

## F. Mengenal Apache

Webserver Apache memiliki folder untuk konfigurasi yang berada di **/etc/apache2**

![](gambar/5.PNG)

Pada folder **/etc/apache2** terdapat berbagai file dan folder untuk konfigurasi

Penting untuk diketahui:
* ***Tentang File*** :

Nama File | Pengertian
------------ | -------------
**apache2.conf** | file konfigurasi utama apache2.
**ports.conf** | file konfigurasi port yang digunakan untuk webserver.
**sites-available** | folder tempat konfigurasi website (virtual host) yang tersedia.
**sites-enabled** | folder tempat konfigurasi website (virtual host) yang tersedia dan sudah aktif.
**mods-available** | folder tempat modul-modul apache2 yang tersedia.
**mods-enabled** | folder tempat modul-modul apache2 yang tersedia dan sudah aktif.

* ***Tentang Command***

Command | Arti
------------ | -------------
a2ensite | Untuk memasukkan atau ENABLE config yang telah dibuat
a2dissite | Untuk menonaktifkan atau DISABLE config yang telah dibuat
a2enmod | Untuk ENABLE spesifik modul ke dalam konfigurasi apache2
a2dismod | Untuk DISABLE spesifik modul ke dalam konfigurasi apache2

## G. Konfigurasi Apache Sederhana
### G.1. Penggunaan Sederhana

**STEP 1** - Pindah ke folder **/etc/apache2/sites-available**

![](gambar/6.PNG)
    
Pada folder **/etc/apache2/sites-availabke** terdapat dua buah file.

- **000-default.conf** adalah file konfigurasi website default apache untuk http.

- **default-ssl.conf** adalah file konfigurasi website default apache untuk https.

**STEP 2** - Buka file **000-default.conf**

![](gambar/7.PNG)

**STEP 3** - Pada file **000-default.conf** berisi contoh konfigurasi,

a. Port berapa yang digunakan.
    
    <VirtualHost *:80> # Menggunakan port 80
        
b. Domain
    
    # ServerName www.example.com
        
c. Folder tempat website
    
    DocumentRoot /var/www/html
     
**STEP 4** - Pindah ke folder tempat website pada file konfigurasi default yaitu **/var/www/html** dan buat file **index.php** yang berisi

    <?php
        phpinfo();
    ?>

**STEP 5** - Buka browser dan akses alamat **http://[IP Pucang]/index.php**

![](gambar/8.PNG)
    
* **Catatan**: <br>Apabila tampilan web tidak muncul seperti gambar diatas dan hanya muncul plain text isi file **index.php**, silahkan install **libapache2-mod-php7.0** dengan menjalankan perintah 
        
    `apt-get install libapache2-mod-php7.0`
    
    dan restart apache dengan perintah
    
    `service apache2 restart`
        
### G.2. Membuat Konfigurasi Website Menggunakan Port 8080

**STEP 1** - Pindah ke folder **/etc/apache2/sites-available** dan copy file **000-default.conf** menjadi file **default-8080.conf**.

![](gambar/9.PNG)
    
**STEP 2** - Buka file **default-8080.conf**, kemudian ubah ubah port yang digunakan yang awalnya **80 **menjadi **8080 **dan ubah tempat menaruh file/folder web yang awalnya **/var/www/html** menjadi **/var/www/web-8080**.

![](gambar/10.PNG)

**STEP 3** - Tambahkan **port 8080** pada file **ports.conf**

![](gambar/11.PNG)

**STEP 4** - Untuk mengaktifkan konfigurasi **default-8080.conf** jalankan perintah
`a2ensite` dan ketik nama **file konfigurasi tanpa .conf**
    
![](gambar/12.PNG)
    
kemudian tekan enter.

Atau bisa ketikkan langsung `a2ensite (namafile)`

**STEP 5** - Restart apache menggunakan perintah `service apache2 restart`

**STEP 6** - Pindah ke folder **/var/www** dan buat folder baru dengan nama **web-8080**

**STEP 7** - Masuk ke folder **web-8080** dan buat file **index.php** yang berisi
    
    <?php
        echo "Hello ini port 8080";
    ?>
    
![](gambar/13.PNG)
        
**STEP 8** - Buka browser dan akses alamat http://[IP Pucang]:8080

![](gambar/14.PNG)

## H. Mari Berimajinasi
### H.1. Setting Domain Pada Apache
Nia adalah seorang mahasiswi Departemen Infomatika yang sedang ingin membuat website dengan domain **klampis.com**. Dia memiliki **teman** yang bernama Udin kebetulan mempunyai server yang bisa digunakan untuk tempat host websitenya.

Maka yang harus dilakukan Udin untuk menyenangkan hati Nia adalah:

**STEP 1** - Pindah ke folder **/etc/apache2/sites-available** dan copy file **000-default.conf** menjadi **klampis.com.conf**

![](gambar/15.PNG)
    
**STEP 2** - Buka file **klampis.com.conf**, kemudian
    
***2.1*** - Uncomment **ServerName** dan ganti **www.example.com** menjadi **klampis.com**.

***2.2*** - Tambahkan

    ServerAlias www.klampis.com
agar dapat mengakses **www.klampis.com**
         
***2.3*** - Ganti tempat **DocumentRoot** yang awalnya **/var/www/html** menjadi **/var/www/klampis.com**
    
![](gambar/16.PNG)
    
**STEP 3** - Aktifkan konfigurasi **klampis.com.conf** dengan menjalankan `a2ensite klampis.com`
 
![](gambar/17.PNG)
 
**STEP 4** - Restart apache dengan menjalankan `service apache2 restart`

**STEP 5** - Pindah ke folder **/var/www** dan buat folder baru dengan nama **klampis.com**

**STEP 6** - Buat file **index.php** dengan isi file

    <?php
        echo "Hello ini klampis.com";
    ?>

![](gambar/18.PNG)

**STEP 7** - Ganti DNS laptop/komputer sesuai **IP Klampis** masing-masing.

**STEP 8** - Buka browser dan akses **klampis.com**

![](gambar/19.PNG)

### H.2. Directory Listing

Di dalam folder **/var/www/klampis.com** terdapat folder sebagai berikut

        /var/www/klampis.com
                            /data
                            /download
                                    /lagu
                            /assets
                                    /javascript

Karena folder **download** terdapat file-file yang bisa didownload oleh pengunjung website **klampis.com**, Nia ingin folder tersebut dapat menampilkan list file yang ada. Tetapi untuk folder **assets**, Nia tidak ingin ada yang tahu apa isi folder tersebut ketika diakses oleh pengunjung websitenya.

Karena udin sangat suka kepada Nia.

Maka yang harus dilakukan Udin adalah:

**STEP 1** - Buat folder **download**, **private** ,**assets**, **data** dan **assets/javascript** pada **/var/www/klampis.com** dengan menjalankan perintah berikut

        mkdir -p /var/www/klampis.com/data
        mkdir -p /var/www/klampis.com/download
        mkdir -p /var/www/klampis.com/download/lagu
        mkdir -p /var/www/klampis.com/assets
        mkdir -p /var/www/klampis.com/assets/javascript

**STEP 2** - Mengaktifkan Directory Listing

***2.1*** - Pindah ke folder **/etc/apache2/sites-available** kemudian buka file **klampis.com** dan tambahkan
    
    <Directory /var/www/klampis.com/download>
        Options +Indexes #Untuk mengaktifkan directory listing
    </Directory>
    
agar folder **download** menampilkan isi folder.
    
![](gambar/20.PNG)
    
***2.2*** - Simpan dan restart apache `service apache2 restart`

***2.3*** - Buka Browser dan akses **http://klampis.com/download**

![](gambar/21.PNG)


**Keterangan**:
  1. Untuk mengatur folder pada sebuah web, menggunakan
  
  `<Directory /x> ... </Directory>`
  
Contoh untuk mengatur /var/www/klampis.com/download
    
    <Directory /var/www/klampis.com/download>
        
    </Directory>

**STEP 3** - Mematikan Directory Listing

***3.1*** - Pindah ke folder **/etc/apache2/sites-available** kemudian buka file **klampis.com** dan tambahkan
    
    <Directory /var/www/klampis.com/assets>
        Options -Indexes #Untuk mematikan directory listing
    </Directory>

agar folder **assets** tidak menampilkan isi folder.

![](gambar/22.PNG)
    
***3.2*** - Simpan dan restart apache `service apache2 restart`

***3.3*** - Buka Browser dan akses **http://klampis.com/assets**

![](gambar/23.PNG)
  
### H.3 Directory Alias

Karena dirasa **http://[IP Klampis]/assets/javascript** terlalu panjang url-nya, maka Udin mencoba membuat directory alias menjadi **http://[IP Klampis]/assets/js** agar Nia tidak capek mengetik.

Maka yang dilakukan Udin adalah

**STEP 1** - Pindah ke folder **/etc/apache2/sites-available** kemudian buka file **klampis.com** dan tambahkan

    Alias "/assets/js" "/var/www/klampis.com/assets/javascript"
            
    <Directory /var/www/klampis.com/assets/javascript>
        Require all granted # Mengizinkan akses ke semua pengguna
        Options +Indexes
    </Directory>

![](gambar/24.PNG)
        
**STEP 2** - Restart apache2 `service apache2 restart`

**STEP 3** - Pindah ke folder **/var/www/klampis.com/assets/javascript** dan buat file **app.js** dengan perintah `touch app.js`

![](gambar/25.PNG)

**STEP 4** - Buka browser dan akses **http://klampis.com/assets/js**

![](gambar/26.PNG)

### H.4. Module Rewrite

* ### Mengaktifkan Module Rewrite

Setelah dipikir-pikir ternyata **http://klampis.com/index.php** ternyata kurang cantik untuk penulisan url. Maka Udin berinisiatif untuk mengaktifkan module rewrite agar ketika mengakses file php tidak usah menambahkan ekstensi .php.

Maka yang dilakukan Udin adalah

**STEP 1** - Menjalankan perintah `a2enmod` dan menuliskan **rewrite** untuk mengaktikan module rewrite.

![](gambar/27.PNG)

**STEP 2** - Restart apache `service apache2 restart`

* ### Membuat file .htaccess

Biasanya semua konfigurasi terhadap sebuah website diatur pada file di folder **/etc/apache2/sites-available**. Namun terkadang ada sebuah kasus bahwa kita tidak memiliki hak akses root untuk edit file konfigurasi yang berada di folder **/etc/apache2/sites-available** atau kita tidak ingin user lain untuk mengedit file konfigurasi yang berada di folder **/etc/apache2/sites-available**.

Untuk mengatasi masalah tersebut kita dapat membuat file **.htaccess** pada folder dimana kita ingin atur.

Contoh kasus diatas kita ingin mengatur mod rewrite dari **http://klampis.com** agar saat mengakses file php tanpa ekstensi file. 

Maka yang dilakukan adalah

**STEP 1** - Pindah ke folder **/var/www/klampis.com** dan buat file **.htaccess** dengan isi file

    RewriteEngine On
    RewriteCond %{SCRIPT_FILENAME} !-d #Aturan tidak akan jalan ketika yang diakses adalah folder
    RewriteRule ^([^.]+)$ $1.php [NC,L]
    
![](gambar/28.PNG)

**Keterangan** :

    * RewriteEngine On = Untuk flag bahwa menggunakan module rewrite
    * RewriteCond %{SCRIPT_FILENAME} !-d = aturan tidak akan jalan ketika yang diakses adalah folder (d)
    * RewriteRule ^([^.]+)$ $1.php [NC,L] = $1 adalah parameter input yang akan dicari oleh webserver
    * Info cek [Klik Disini](https://httpd.apache.org/docs/2.4/rewrite/flags.html)
    
**STEP 2** - Buat file aboutus.php dengan isi

    <?php
        echo "ini halaman About Us";
    ?>
    
![](gambar/29.PNG)

**STEP 3** - Pindah ke folder **/etc/apache2/sites-available** kemudian buka file **klampis.com** dan tambahkan

    <Directory /var/www/klampis.com>
        AllowOverride All
    </Directory>

![](gambar/30.PNG)

**Keterangan** :

* `AllowOverride All` ditambahkan agar konfigurasi **.htaccess** dapat berjalan.

**STEP 4** - Restart apache `service apache2 restart`

**STEP 5** - Buka browser dan akses **http://klampis.com/aboutus**

![](gambar/31.PNG)

### H.5 Otorisasi

Pada web **http://klampis.com** terdapat path **/data** yang tidak boleh dibuka sembarang orang. Nia ingin **/data** hanya boleh di akses oleh pengguna yang memiliki ip **10.151.252.0/255.255.252.0**.

Maka yang dilakukan Udin agar Nia tetap aman adalah

**STEP 1** - Pindah ke folder **/etc/apache2/sites-available** kemudian buka file **klampis.com** dan tambahkan

    <Directory /var/www/klampis.com/data>
        Options +Indexes
        Order deny,allow
        Deny from all
        Allow from 10.151.252.0/255.255.252.0
    </Directory>

![](gambar/32.PNG)

**Keterangan** :

* `Order deny,allow` merupakan urutan hak akses. Terdapat dua jenis tipe order yaitu:

1. deny,allow : Bagian *Deny* harus dideclare terlebih dahulu sebelum _Allow_
2. allow, deny : Bagian *Allow* harus dideclare terlebih dahulu sebelum _Deny_

* `Deny from all` berarti semua pengguna ditolak

* `Allow from 10.151.252.0/255.255.252.0` berarti apabila pengguna memiliki ip nid 10.151.252.0/22 diperbolehkan mengakses halaman.

* Info lebih lanjut [Klik Disini](https://httpd.apache.org/docs/2.4/mod/mod_access_compat.html)

**STEP 2** - Restart apache `service apache2 restart`

**STEP 3** - Buka browser dan akses **http://klampis.com/data**

![](gambar/33.PNG)

Gambar diatas ketika pengguna **tidak memiliki ip nid 10.151.252.0/22**


![](gambar/34.png)

Gambar diatas ketika pengguna **memiliki ip nid 10.151.252.0/22**

## SOAL LATIHAN
[Klik Disini](https://github.com/rohanaq/jarkom-modul-2/blob/masterSoal%20Latihan%20Web%20Server.md)

<br>

## <center>MODUL DNS DAN WEB SERVER SELESAI</center>
## <center>Selamat Menunggu Soal Shift :)</center>