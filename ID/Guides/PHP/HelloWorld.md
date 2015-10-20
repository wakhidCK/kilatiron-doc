#Deploying Aplikasi Silex pada CloudKilat

[Silex] adalah microframework PHP untuk PHP 5.3. Hal ini terinspirasi oleh sinatra dan
dibangun di pundak Symfony2 dan Jerawat.

Dalam tutorial ini kita akan menunjukkan kepada Anda bagaimana untuk menggunakan aplikasi Silex pada
[CloudKilat]. Anda dapat menemukan [kode sumber di Github] [contoh-aplikasi] dan cek
keluar [php buildpack] untuk fitur didukung.


## The Silex App Dijelaskan

### Dapatkan App
Pertama, mari kita mengkloning Silex App dari repositori kami pada Github:
~~~ Pesta
$ Git clone https://github.com/cloudControl/php-silex-example-app.git
$ Cd php-silex-contoh-aplikasi
~~~

Sekarang Anda memiliki aplikasi Silex kecil tapi berfungsi penuh.


### Ketergantungan Tracking
PHP buildpack melacak dependensi melalui [Komposer]. Persyaratan dibaca
dari `composer.json` di direktori root proyek. Untuk aplikasi sederhana ini
persyaratan adalah:

~~~ Json
{
    "Minimum stabilitas": "dev",
    "Membutuhkan": {
        "Silex / silex": "*",
        "Ranting / ranting": "*",
        "Mheap / Silex-Assetic": "*",
        "Natxet / CssMin": "*"
    }
}
~~~

Perhatikan bahwa ada juga `composer.lock`. Ketika Anda mengubah ketergantungan,
Anda harus menjalankan `composer.phar update` perintah untuk memperbarui
`Composer.lock`. File ini harus dalam repositori Anda dan memastikan bahwa semua
pengembang selalu menggunakan versi yang sama dari semua perpustakaan. Hal ini juga membuat
perubahan terlihat di git. Juga mencatat bahwa `.gitignore` Anda harus berisi
`Vendor` seperti yang diusulkan dalam
[Komposer dokumentasi] (http://getcomposer.org/doc/01-basic-usage.md#installing-dependencies),
karena Anda tidak perlu semua bahwa kode dalam repositori Anda.


## Document Root Definition

PHP buildpack memungkinkan pengguna untuk menimpa akar dokumen default. Hal ini dilakukan melalui
file konfigurasi di `.buildpack / apache / conf /` direktori. File harus menggunakan
`Ekstensi .conf`. Dalam tutorial ini file yang bernama `documentroot.conf` dan memiliki
konten berikut:
~~~ Conf
DocumentRoot / app / www / web
<Directory / app / www / web>
    AllowOverride Semua
    Pilihan SymLinksIfOwnerMatch
    Agar Deny, Allow
    Izinkan dari Semua
    DirectoryIndex index.php index.html index.htm
</ Directory>
~~~

Untuk informasi lebih lanjut lihat [dokumentasi buildpack] [php buildpack].

## Mendorong dan Menyebarkan App
Pilih nama yang unik untuk menggantikan `APP_NAME` tempat untuk aplikasi Anda dan membuatnya pada platform CloudKilat:
~~~ Pesta
$ Ironcliapp APP_NAME membuat php
~~~

Mendorong kode Anda ke repositori aplikasi, yang memicu penyebaran gambar proses build:
~~~
$ Ironcliapp APP_NAME / dorongan bawaan
Menghitung benda: 29, dilakukan.
Delta kompresi menggunakan sampai 4 benang.
Mengompresi objek: 100% (21/21), dilakukan.
Menulis objek: 100% (29/29), 459,72 KiB | 720 KiB / s, dilakukan.
Total 29 (delta 5), ​​kembali 18 (delta 0)

-----> Mendorong Menerima
       Repositori Memuat komposer dengan informasi paket
       Instalasi dependensi (termasuk membutuhkan-dev) dari file kunci
         - Instalasi symfony / proses (dev-master 998d489)
           Kloning 998d489806011e1d790db5fc0284e6083cc8ea8b

         - Instalasi kriswallsmith / assetic (dev-master f9f754d)
           Kloning f9f754dc7524acd6daf0bf510d22c055b4967e08

         - Instalasi symfony / finder (dev-master 57b6772)
           Kloning 57b67729f863be8b950441a739b82678b91accde

           ...

       Menghasilkan file autoload
-----> Gambar Building
-----> Gambar Mengunggah (14M)

Untuk ssh: //APP_NAME@kilatiron.net/repository.git
* [Cabang baru] Master -> Master
~~~

Terakhir namun tidak sedikit menyebarkan versi terbaru dari aplikasi dengan ironapp yang menyebarkan perintah:
~~~ Pesta
$ Ironcliapp APP_NAME / default menyebarkan
~~~

Selamat, Anda sekarang dapat melihat aplikasi Silex Anda berjalan pada `http [s]: // APP_NAME.kilatiron.net`.


[Silex]: http://silex.sensiolabs.org/
[CloudKilat]: http://www.cloudkilat.com/
[CloudKilat-doc-user]: / Landasan% 20Documentation / # user-account
[CloudKilat-doc-cmdline]: / Landasan% 20Documentation / # baris perintah-client-web-konsol-dan-api "dokumentasi CloudKilat-baris perintah-client"
[Php buildpack]: https://github.com/cloudControl/buildpack-php
[Procfile]: / Landasan% 20Documentation / # buildpacks-dan-the-procfile
[Git]: https://help.github.com/articles/set-up-git
[Komposer]: http://getcomposer.org/
[Contoh-aplikasi]: https://github.com/cloudControl/php-silex-example-app
