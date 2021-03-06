# 2. Web Server
Web Server adalah perangkat yang menyediakan layanan akses kepada pengguna melalui protokol HTTP atau HTTPS melalui aplikasi web.

### 2.1 Instalasi apache2
Install Web Server Apache pada PUCANG yang sebelumnya sudah diupdate dengan mengetikkan __apt-get install apache2__. Setelah proses instalasi selesai, coba akses __IP PUCANG TIAP KELOMPOK__ pada browser maka akan muncul seperti gambar di bawah:

![Pucang1](images/01.png)

### 2.2 Instalasi PHP pada PUCANG
Supaya web server dapat menjalankan perintah PHP, maka kita perlu menginstall PHP pada PUCANG dengan mengetikkan __apt-get install php5__ atau __apt-get install php__

![Pucang2](images/02.png)

Setelah proses instalasi selesai, untuk mengetahui apakah instalasi PHP berhasil atau tidak, silahkan membuat file PHP yang diletakkan di __/var/www/html__

Jalankan perintah untuk membuat file __ini.php__:
```bash
echo "<?php phpinfo(); ?>" > /var/www/html/ini.php
```

Setelah selesai membuat file __ini.php__, untuk mencoba membukanya akses dengan `http://IP_PUCANG_TIAP_KELOMPOK/ini.php` pada browser laptop masing-masing maka akan muncul seperti gambar di bawah:

![Pucang3](images/03.png)

Untuk membuat sebuah halaman atau website, kita dapat menambahkannya pada direktori __/var/www/html__. Langkah-langkahnya sama seperti saat membuat halaman __ini.php__.

Sekarang bayangkan ketika kita ingin mengakses sebuah halaman web kita harus mengetikkan alamat IPnya terlebih dahulu. Ribet kan? Maka kita akan membuatnya menjadi mudah, silahkan lakukan konfigurasi sehingga ketika kita mengakses __klampis.com__ akan diarahkan ke halaman web yang sudah kita buat. Dengan cara mengarahkan nameserver klampis.com ke IP PUCANG. Jika lupa caranya, silahkan buka lagi modul sebelumnya yang membahas tentang [DNS](https://github.com/rohanaq/jarkom-modul-2/tree/master/DNS).

Tambah nameserver dengan ip KLAMPIS pada PC kita pada OS Linux menggunakan 
```
sudo nano /etc/resolv.conf
```
![Pucang23](images/23.png)

Lalu buat file jarkom.php pada /var/www/html pada PUCANG. Ketika diakses hasilnya seperti ini.
```
echo "<?php echo 'Semangat JARKOM 2018'; ?>" > /var/www/html/jarkom.php
```
![Pucang4](images/04.png)

### 2.3 Mengubah Direktori Default
Webserver akan memproses file sesuai URL yang diminta oleh client. Sebagai contoh, ketika client mengakses `http://klampis.com:8080/index.php`.

Sebagai | Contoh
------------ | -------------
fullpath URL | `http://klampis.com:8080/index.php`
hostname | `klampis.com`
port | `8080`
URL | `/index.php`

Secara _default_, apache2 menggunakan __/var/www/html__ sebagai direktori default. Ketika mengakses `http://klampis.com/ini.php`, sebenarnya webserver sedang mengakses file __/var/www/html/ini.php__.

Dalam kasus tertentu kita ingin mengganti default direktori menjadi direktori lain. Sebagai contoh, apabila direktori default dari apache2 diganti menjadi __/home/rohanaq/mysite__, maka ketika mengakses `http://klampis.com/pasar/index.php`, webserver akan mengakses file __/home/rohanaq/mysite/pasar/index.php__.


Default direktori pada apache2 ditandai dengan _syntax_ `DocumentRoot`. Contohnya
```
    DocumentRoot /var/www/html
```

Sekarang, coba ubah direktori default menjadi __/var/www/__, seperti dibawah ini:

![Pucang6](images/06.png)

Setelah itu, coba buatlah file __coba.php__ pada direktori __/var/www__ dengan perintah
```
echo "<?php echo 'Hello, ini coba.php'; ?>" > /var/www/coba.php
```

Setelah itu restart apache2 dengan perintah `service apache2 restart` dan coba akses kembali halaman __coba.php__ pada browser kalian.

### 2.4 Konfigurasi VirtualHost
Sekarang kita akan mengkonfigurasi __VirtualHost__ untuk bisa mengakses web server. Pada PUCANG lakukan copy file terhadap file konfigurasi bawaan dari apache2. Setelah menyalin file konfigurasi bukalah dengan perintah __nano__
```bash
cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/default2.conf
nano sites-available/default2.conf
```

Lalu silahkan ganti port pada __VirtualHost__ menjadi __8080__, dan ubah __DocumentRoot__ menjadi __/var/www/8080__ dan tambahkan sintaks seperti di bawah ini:
```
<VirtualHost *:8080>
```

```
ServerAdmin webmaster@localhost
DocumentRoot /var/www/8080

<Directory /var/www/8080>
AllowOverride All
</Directory>
```

![Pucang7](images/07.png)

Lalu tambahkan port 8080 dengan mengedit file __ports.conf__ yang ada pada __/etc/apache2__. Tambahkan dengan sintaks `listen 8080`

![Pucang8](images/08.png)

Kemudian pada folder __/var/www__ buatlah folder 8080 dan setelah itu buatlah file index.php pada folder tersebut
```
cd /var/www
mkdir 8080
nano 8080/index.php
```

Lalu pada file index.php tersebut isikan tulisan seperti di bawah ini:
```html
<!DOCTYPE html>
<html>
<head>
    <title>JARKOM 2018</title>
</head>
<body>
    <h3>INI PORT 8080</h3>
</body>
</html>
```

Lalu untuk menjalankannya ketikkan `a2ensite default2.conf` dan kemudian restart apache2 dengan perintah `service apache2 restart`. Setelah itu pada browser silahkan akses website kalian dengan menggunakan __IP PUCANG TIAP KELOMPOK__ atau dengan DNS yang sudah dibuat, lalu tambahkan port 8080 pada akhir penulisannya. Contoh -> __10.151.73.235:8080__ atau __pucang.com:8080__. Maka akan muncul seperti gambar di bawah:

![Pucang9](images/09.png)

__Keterangan [Klik Disini](#30-keterangan)__
### 2.5 Alias Directory
Alias directory adalah membuat direktori lebih mudah diakses tetapi tetap di bawah DocumentRoot. Silahkan masuk ke dalam __/etc/apache2/sites-available__ kemudian buka file __default2.conf__ yang sebelumnya sudah dikonfigurasi untuk poin sebelumnya. Edit file tersebut dan tambahkan seperti gambar di bawah ini:

![Pucang10](images/10.png)

Kemudian restart apache2 dengan perintah `service apache2 restart`. Setelah itu buatlah direktori __/var/www/asset/javascript/__ dengan mengetikkan
```
mkdir /var/www/asset
mkdir /var/www/asset/javascript
```

Kemudian pada direktori __/var/www/alias/test/jarkom__ buatlah sebuah file bernama __coba.js__.
```
touch /var/www/asset/javascript/coba.js
```

Sekarang coba buka pada browser __IP PUCANG TIAP KELOMPOK/asset/js__ atau __klampis.com/asset/js__ (sesuai dengan DNS yang sudah dibuat masing-masing kelompok)

![Pucang11](images/11.png)

Lalu apa perbedaannya ketika kita mengakses __/asset/javascript__?
![Pucang12](images/12.png)

### 2.6 Directory Listing
Silahkan masuk ke dalam __/etc/apache2/sites-available__ kemudian buka file __default2.conf__ yang sebelumnya sudah dikonfigurasi untuk poin sebelumnya. Edit file tersebut dan tambahkan seperti gambar di bawah ini:

![Pucang13](images/13.png)

Kemudian coba akses website masing-masing maka akan muncul gambar seperti ini:

![Pucang14](images/14.png)

Sekarang coba buat directory download dengan mengetikkan
```
mkdir /var/www/download
```

Lalu buat file blabla.tar.gz dengan mengetikkan
```
touch blabla.tar.gz
```

Lalu coba akses klampis.com/download
![Pucang14](images/24.png)

Lalu bagaimana cara agar kita dapat melihat isi dari direktori download tersebut? Buka kembali __default2.conf__ dan ganti seperti gambar di bawah:

![Pucang15](images/15.png)

Setelah itu restart apache2 dan coba akses kembali website masing-masing maka kita dapat melihat isi file dari direktori seperti gambar di bawah:

![Pucang16](images/16.png)

### 2.7 Allow dan Deny
Dalam web server dibutuhkan adanya proteksi dan penjagaan terhadap web server agar tetap aman. Dalam web server, kita bisa membatasi hak akses siapa saja yang boleh mengakses web server tersebut.

Silahkan masuk ke dalam __/etc/apache2/sites-available__ kemudian buka file __default2.conf__ yang sebelumnya sudah dikonfigurasi untuk poin sebelumnya. Edit file tersebut dan tambahkan seperti gambar di bawah ini:

![Pucang17](images/17.png)

Restart apache2 kemudian coba akses website masing-masing maka direktori __html__ akan menghilang:

![Pucang18](images/18.png)

### 2.8 Basic Authentication
Basic Authentication digunakan untuk mebatasi akses terhadap suatu halaman dengan memberikan suatu autentikasi untuk user-user tertentu dengan password yang sudah ditentukan.

Silahkan masuk ke dalam __/etc/apache2/sites-available__ kemudian buka file __default2.conf__ yang sebelumnya sudah dikonfigurasi untuk poin sebelumnya. Edit file tersebut dan tambahkan seperti gambar di bawah ini:

![Pucang19](images/19.png)

Kemudian buat folder __/var/www/private__ dengan perintah `mkdir /var/www/private` dan buatlah file index.html di dalamnya. Pada file cobaindex.html isikan apa saja, sebagai contoh:
```html
<html>
<head>
    <title>JARKOM 2018</title>
</head>
<body>
    <h3>Basic Authentication</h3>
</body>
</html>
```

Setelah itu ketikkan sintaks berikut:
```
htpasswd -c /var/www/private/.htpasswd basicauth
```

__Keterangan__:
* __htpasswd__
Untuk menambahkan autentikasi basic untuk sebuah user
* __/var/www/private/.htpasswd__
File yang menyimpan data user dan password
* __basicauth__
Nama user
* __DirectoryIndex__
Digunakan untuk menentukan file mana yang akan menjadi index pada directory

Kemudian akan muncul permintaan password untuk user __basicauth__, isi permintaan password tersebut sesuai dengan keinginan kalian.

Untuk mencobanya, akses pada browser kalian dengan mengetikkan __IP PUCANG TIAP KELOMPOK/private__ atau __klampis.com/private__ (sesuai dengan DNS yang sudah dibuat masing-masing kelompok)

![Pucang20](images/20.png)

Masukkan user dan password yang sudah dibuat sebelumnya maka halaman cobaindex.html akan ditampilkan:
![Pucang21](images/21.png)

Jika user dan password tidak dimasukkan maka akan muncul halaman __401 Unauthorized__ seperti gambar di bawah:

![Pucang22](images/22.png)

### 2.9 Digest Authentication


### 3.0 Keterangan
Catatan
Syntax | Arti
------------ | -------------
a2ensite | Untuk memasukkan atau ENABLE config yang telah dibuat
a2dissite | Untuk menonaktifkan atau DISABLE config yang telah dibuat
a2enmod | Untuk ENABLE spesifik modul  ke dalam konfigurasi apache2