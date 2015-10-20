# Caracepat memulai CloudKilat 

Untuk memulai dengan CloudKilat sangatlah mudah. Ikuti 5 menit caracepat ini untuk mendapatkan
Aplikasi pertama Anda yang berjalan di CloudKilat PaaS.

**Catatan:** Semua contoh dimulai dengan $ seharusnya dijalankan dalam terminal.
Untuk Windows sebaiknya menggunakan Git bash, yang dibundel dengan Windows
Instalasi Git. Sepanjang caracepat ini dan seluruh dokumentasi
penampung ditandai dengan ditulis huruf besar semua.

## Pasang Perangkatlunak yang Diperlukan

### Persyaratan

* Sistem kontrol versi git
* Perintah ironuser/ironapp baris klien

### Pasang git

Instal Git dari [situs resmi](http://git-scm.com/) atau dari paket
repositori pilihan Anda. Untuk Windows dianjurkan untuk menggunakan installer resmi dan Git bash. Silahkan instal aplikasi tersebut.

### Instal baris perintah klien

**Linux/Mac OS X:** Kami sarankan menginstal klien baris perintah melalui pip.

~~~bash
# Jika Anda belum memiliki pip
$ sudo easy_install pip
$ sudo pip install ironcli
~~~

**Windows:** Silahkan download yang disediakan [installer].

## Buat Akun Pengguna (jika Anda belum memilikinya)

Anda dapat mendaftar di [www.cloudkilat.com](http://www.cloudkilat.com/).

## Tambahkan Public Key

~~~ bash
$ ironuser key.add
Email: EMAIL
Password: PASSWORD
~~~

Baris perintah klien akan menentukan apakah Anda sudah memiliki kunci publik dan menguploadnya atau menawarkan untuk membuatnya.

## Membuat Aplikasi Pertama di CloudKilat

Membuat aplikasi baru pada platform CloudKilat dengan memberikan nama  yang unik `APP_NAME` (nama yang akan digunakan sebagai subdomain `.kilatiron.net`) dan pilihlah tipenya
`TYPE`.

~~~bash
$ ironapp APP_NAME create [java, php, python, ruby, nodejs]
~~~

Jika `APP_NAME` sudah digunakan, silakan memilih nama yang lain.

Ubah ke direktori kerja di mana Anda ingin menyimpan sumberkode Anda.

~~~bash
$ Cd PATH_TO/YOUR_WORKDIR
~~~

Mengkloning salah satu contoh aplikasi dalam bahasa pemrograman pilihan Anda dan mendorongnya ke platform CloudKilat.

~~~bash
# untuk Java
$ git clone https://github.com/cloudControl/java-jetty-jsp-example-app.git
$ cd java-jetty-jsp-example-app

# untuk PHP
$ git clone https://github.com/cloudControl/php-silex-example-app.git
$ cd php-silex-example-app

# untuk Python
$ git clone https://github.com/cloudControl/python-flask-example-app.git
$ cd python-flask-example-app

# untuk Ruby
$ git clone https://github.com/cloudControl/ruby-sinatra-example-app.git
$ cd ruby-sinatra-example-app

# untuk Node.js
$ git clone https://github.com/cloudControl/nodejs-express-example-app.git
$ cd nodejs-express-example-app

# Lalu kirim ke repositori
$ ironapp APP_NAME push
~~~

Kiriman anda akan membuat persiapan untuk aplikasi yang akan dipasang seperti 
mengunduh persyaratan dan banyak lagi. Anda bisa melihat prosesnya di terminal.

## Pasang Aplikasi Anda di CloudKilat

Pasang aplikasi anda dengan

~~~bash
$ ironapp APP_NAME deploy
~~~

**Selamat, aplikasi anda sudah berjalan dan dapat digunakan.**

~~~bash
http[s]://APP_NAME.kilatiron.net
~~~

## Contekan

Unduh  [our cheatsheet (PDF)](/cloudkilat_cheatsheet.pdf) untuk memiliki baris perintah yang penting kapan saja.

## Dokumentasi

Untuk mempelajari lebih lanjut tentang semua fitur platform dan bagaimana cara untuk mengintegrasikannya
ke dalam siklus hidup pengembangan silahkan lihat ekstensif
[Dokumentasi platform](/Platform Documentation.md).

[installer]: https://www.cloudcontrol.com/download/win
