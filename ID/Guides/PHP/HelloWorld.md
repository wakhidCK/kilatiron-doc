# Aplikasi Silex pada KilatIron

[Silex] adalah microframework PHP untuk PHP 5.3. Framework ini terinspirasi oleh sinatra dan
dibangun dengan menggunakan Symfony2 dan Pimple.

Dalam tutorial ini kita akan menunjukkan kepada Anda bagaimana cara menggunakan aplikasi Silex di
[KilatIron]. Anda dapat menemukan [kode sumber di Github] [contoh-aplikasi] dan memeriksa [PHP buildpack]
untuk mengetahui fitur-fitur yang didukung.


## Silex

### Dapatkan Aplikasi
Pertama, mari kita mengkloning Silex App dari repositori kami pada Github:
~~~bash
$ git clone https://github.com/cloudControl/php-silex-example-app.git
$ cd php-silex-example-app
~~~

Sekarang Anda memiliki aplikasi Silex sederhana.


### Melacak Ketergantungan
PHP buildpack melacak ketergantungan melalui [Composer]. Persyaratan dibaca
dari `composer.json` di direktori root proyek. Untuk aplikasi sederhana ini
persyaratan adalah:

~~~json
{
    "minimum-stability": "dev",
    "require": {
        "silex/silex": "*",
        "twig/twig": "*",
        "mheap/Silex-Assetic": "*",
        "natxet/CssMin": "*"
    }
}
~~~

Perhatikan bahwa ada file `composer.lock`. Ketika Anda mengubah ketergantungan,
Anda harus menjalankan `composer.phar update` perintah untuk memperbarui
`composer.lock`. File ini harus dalam repositori Anda dan memastikan bahwa semua
pengembang selalu menggunakan versi yang sama dari semua library. Hal ini juga membuat
perubahan terlihat di git. Dan ingat bahwa `.gitignore` Anda harus berisi
`vendor` seperti yang diusulkan dalam
[dokumentasi Composer] (http://getcomposer.org/doc/01-basic-usage.md#installing-dependencies),
karena Anda tidak membutuhkan semua kode tersebut dalam repositori Anda.


## Document Root Definition

PHP buildpack memungkinkan pengguna untuk menimpa root dokumen default. Hal ini dilakukan melalui
file konfigurasi di direktori `.buildpack/apache/conf/`. File harus menggunakan
ekstensi `.conf`. Dalam tutorial ini file bernama `documentroot.conf` dan memiliki
konten sebagai berikut:
~~~conf
DocumentRoot /app/www/web
<Directory /app/www/web>
    AllowOverride All
    Options SymlinksIfOwnerMatch
    Order Deny,Allow
    Allow from All
    DirectoryIndex index.php index.html index.htm
</Directory>
~~~

Untuk informasi lebih lanjut lihat [dokumentasi buildpack] [PHP buildpack].

## Push dan Deploy Aplikasi

Pilih nama yang unik untuk menggantikan `APP_NAME` untuk aplikasi Anda dan membuatnya pada platform KilatIron:

~~~bash
$ ironapp APP_NAME create php
~~~

Push kode Anda ke repositori aplikasi, yang memicu proses pembuatan image container:
~~~
$ ironapp APP_NAME/default push
Counting objects: 29, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (21/21), done.
Writing objects: 100% (29/29), 459.72 KiB | 720 KiB/s, done.
Total 29 (delta 5), reused 18 (delta 0)

-----> Receiving push
       Loading composer repositories with package information
       Installing dependencies (including require-dev) from lock file
         - Installing symfony/process (dev-master 998d489)
           Cloning 998d489806011e1d790db5fc0284e6083cc8ea8b

         - Installing kriswallsmith/assetic (dev-master f9f754d)
           Cloning f9f754dc7524acd6daf0bf510d22c055b4967e08

         - Installing symfony/finder (dev-master 57b6772)
           Cloning 57b67729f863be8b950441a739b82678b91accde

           ...

       Generating autoload files
-----> Building image
-----> Uploading image (14M)

To ssh://APP_NAME@kilatiron.net/repository.git
* [new branch]      master -> master
~~~

Yang terakhir harus dilakukan adalah menyebarkan versi terbaru dari aplikasi dengan perintah ironapp deploy:
~~~bash
$ ironapp APP_NAME/default deploy
~~~

Selamat, Anda sekarang dapat melihat aplikasi Silex Anda berjalan di `http[s]://APP_NAME.kilatiron.net`.

[Silex]: http://silex.sensiolabs.org/
[KilatIron]: http://www.cloudkilat.com/
[CloudKilat-doc-user]: /Platform%20Documentation.md/#user-accounts
[CloudKilat-doc-cmdline]: /Platform%20Documentation.md/#command-line-client-web-console-and-api
[PHP buildpack]: https://github.com/cloudControl/buildpack-php
[Procfile]: /Platform%20Documentation.md/#buildpacks-and-the-procfile
[Git]: https://help.github.com/articles/set-up-git
[Composer]: http://getcomposer.org/
[Contoh-aplikasi]: https://github.com/cloudControl/php-silex-example-app
