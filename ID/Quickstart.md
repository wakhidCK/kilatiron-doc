## Cara Cepat Menggunakan Kilat Iron di CloudKilat

Sangat mudah untuk memulai dengan Kilat Iron. Ikuti cara cepat 5 menit agar aplikasi pertama Anda berjalan di PaaS CloudKilat.

**Catatan:** Semua contoh dimulai dengan $ harus dijalankan di terminal. Untuk Windows kami sarankan menggunakan Git bash, yang sudah bundling dengan instaler Windows Git. Didalam panduan cara cepat ini dan semua dokumentasi ditandai dengan huruf kapital. 

## Instalasi Software yang dibutuhkan
### Kebutuhan

* git version control system
* ironuser/ironapp command line client

### Instal git

Instal git dari situs official http://git-scm.com/ atau dari dari paket repository yang Anda pilih. Untuk Windows kami rekomendasikan untuk menggunakan instaler official dan Git bash. Lanjutkan ke langkah selanjutnya apabila software yang dibutuhkan sudah diinstal semuanya.

### Instal ironcli

**Linux/Mac OS X:** Kami sarankan melakukan instalasi ironcli melalui pip

~~~bash
# Jika pip belum terinstall
$ sudo easy_install pip
$ sudo pip install ironcli
~~~

**Windows:** Mohon untuk mengunduh instaler yang disediakan di https://downcloud.cloudcontrolled.net/windows

## Membuat Akun User (Jika belum ada)

Silahkan mendaftar pada website kami (www.cloudkilat.com).

## Menambah Public Key

~~~bash
$ ironuser key.add
Email   : EMAIL
Password: PASSWORD
~~~

Command line client akan menentukan apakah Anda sudah memiliki public key dan mengunggahnya atau menawarkan untuk membuatnya.

## Membuat Aplikasi Pertama di CloudKilat

Membuat aplikasi baru pada platform CloudKilat dengan memberikan 'APP_NAME' yang unik (Nama biasanya seperti subdomain '.kilatiron.net') dan pilih 'TYPE' aplikasi.

~~~bash
$ ironapp APP_NAME create [java, php, python, ruby, nodejs]
~~~

Jika 'APP_NAME' sudah ada sebelumnya, maka pilih nama yang lain.

Mengganti direktori untuk menyimpan source code.

~~~bash
$ cd PATH_TO/YOUR_WORKDIR
~~~

Menggandakan salah satu contoh aplikasi sesuai dengan bahasa pemrograman yang dipilih lalu menjalankannya di platform CloudKilat.

~~~bash
# untuk Java
$ git clone https://github.com/cloudControl/java-jetty-example-app.git
$ cd java-jetty-example-app

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

# Melakukan push
$ ironapp APP_NAME push
~~~

Dengan push menjadikan pancingan untuk mempersiapkan aplikasi Anda ke tahap deployment seperti mengumpulkan kebutuhan aplikasi dan lainnya. Anda dapat melihat hasil dari proses build pada terminal Anda.

## Deploy Aplikasi Anda pada CloudKilat

Deploy aplikasi Anda dengan cara

~~~bash
$ ironapp APP_NAME deploy
~~~

**Selamat, aplikasi Anda sudah bisa diakses.**

~~~bash
http[s]://APP_NAME.kilatiron.net
~~~

## Rangkuman

Dapatkan Rangkuman kami versi PDF pada tautan berikut : https://github.com/CloudKilat/kilatiron-doc/blob/master/Cheat%20Sheet%20Kilat%20Iron.pdf untuk mengetahui command line penting dan dapat membantu Anda.

## Dokumentasi

Untuk mengetahui lebih lanjut dari fitur platform dan bagaimana untuk mengintegrasikannya menjadi siklus hidup development silahkan merujuk ke dokumentasi platform berikut : platform documentation](https://github.com/CloudKilat/kilatiron-doc/blob/master/Platform-Documentation.md