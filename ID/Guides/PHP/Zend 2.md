# Menyebarkan Aplikasi Zend2

Dalam tutorial ini kita akan menunjukkan kepada Anda bagaimana untuk menggunakan aplikasi Zend2 pada [CloudKilat].

The [contoh aplikasi] adalah siap untuk menyebarkan garpu dari ZendSkeletonApplication resmi tersedia di [github] (https://github.com/zendframework/ZendSkeletonApplication).

## The Zend2 Aplikasi Dijelaskan

### Dapatkan App

Pertama, mengkloning aplikasi Zend2 dari repositori kami:

~~~ Pesta
$ Git clone https://github.com/cloudControl/php-zend2-example-app.git
$ Cd php-zend2-contoh-aplikasi
~~~

### Opsional: Mulai App Lokal Menggunakan PHP 5.4 Built-in Webserver

Aplikasi ini dapat dijalankan secara lokal dengan PHP 5.4 built-in web server. Cukup memberikan mandat db lokal, menginstal dependensi menggunakan Composer, menginisialisasi tabel sesi dan kemudian mulai PHP 5.4 built-in web server.

Membuat file `config / autoload / local.php` dengan kode berikut. Pastikan untuk mengganti `DATABASE`,` `username` dan penampung password`.

~~~ Php
<? Php

kembali array (
'Db' ​​=> array (
'Driver' => 'PDO',
'Dsn' => 'mysql: dbname = DATABASE; host = localhost',
'Driver_options' => array (
PDO :: MYSQL_ATTR_INIT_COMMAND => 'SET NAMES \' UTF8 \ ''
),
'Username' => USERNAME,
'Password' => PASSWORD,
)
);
~~~

~~~ Pesta
$ Php composer.phar menginstal
$ Php publik / index.php init-sesi-tabel
[SUKSES] Sesi meja dibuat.
$ Cd publik /
$ Php -S localhost: 8888
~~~

Terbuka [localhost: 8888] (http: // localhost: 8888 /) di browser Anda untuk mengunjungi aplikasi lokal.

### Baca Kredensial dari Lingkungan dan Tuliskan Log untuk Syslog

Kode di `config / autoload / global.php` adalah cukup sederhana. Jika variabel lingkungan `CRED_FILE` diatur,` get_credentials () `metode yang digunakan untuk membaca file JSON dan mengembalikan kepercayaan db sebagai bagian dari Zend 2 konfigurasi.

Kami juga mengkonfigurasi logger untuk log ke syslog.

~~~ Php
<? Php

get_credentials function () {
// Membaca kredensial berkas
$ Creds_content = file_get_contents ($ _ ENV ['CRED_FILE'], false);
if ($ creds_content == false) {
melempar Exception baru ('Tidak dapat membaca berkas kredensial');
}
// File berisi string JSON, decode dan mengembalikan array asosiatif
$ Creds = json_decode ($ creds_content, true);

if (! array_key_exists ('MYSQLS', $ creds)) {
melempar Exception baru ('. Tidak ada MySQL identitasnya ditemukan Pastikan Anda telah menambahkan mysqls addon.');
}

$ Database_host = $ creds ["MYSQLS"] ["MYSQLS_HOSTNAME"];
$ Database_name = $ creds ["MYSQLS"] ["MYSQLS_DATABASE"];
$ Database_user = $ creds ["MYSQLS"] ["MYSQLS_USERNAME"];
$ Database_password = $ creds ["MYSQLS"] ["MYSQLS_PASSWORD"];

kembali array (
'Driver' => 'PDO',
'Dsn' => sprintf ('mysql: dbname =% s; host =% s', $ database_name, $ database_host),
'Driver_options' => array (
PDO :: MYSQL_ATTR_INIT_COMMAND => 'SET NAMES \' UTF8 \ ''
),
'Username' => $ database_user,
'Password' => $ database_password,
);
}

$ Config = array ();

// Jika aplikasi berjalan pada CloudKilat PaaS membaca kredensial
// Dari lingkungan. Kredensial db lokal harus dimasukkan ke dalam local.php
if (isset ($ _ ENV ['CRED_FILE'])) {
$ Config ['db'] = get_credentials ();
}

$ Config ['service_manager'] = array (
'Pabrik' => array (
'Zend \ Db \ Adapter \ Adapter' => 'Zend \ Db \ Adapter \ AdapterServiceFactory',
'Zend \ Log \ Logger' => function () {
$ Logger = baru Zend \ Log \ Logger;
$ Penulis = Zend baru \ Log \ Penulis \ Syslog ();
if (! endsWith ($ _ ENV ['DEP_NAME'], '/ default')) {
$ Writer-> addFilter (Zend \ Log \ Logger :: ERR);
}
$ Logger-> addWriter ($ writer);
kembali $ logger;
}
),
'Alias' => array (
'Db' ​​=> 'Zend \ Db \ Adapter \ Adapter'
)
);

kembali $ config;

~~~

### Sesi Simpan dalam Database

Menyimpan sesi pada sistem file lokal tidak bekerja dengan baik pada platform horizontal skala seperti CloudKilat. Selain filesystem di CloudKilat tidak persitent di menyebarkan sehingga semua sesi hilang setelah setiap menyebarkan.

Untuk menghindari hal ini, aplikasi telah dikonfigurasikan untuk menyimpan sesi menggunakan koneksi dikonfigurasi sebelumnya dalam database.

Kode masing tinggal di `modul / Application / Module.php`. Ia menggunakan kredensial database global dan menetapkan sesi Database Zend 2 built-in save handler sebagai default.

~~~ Php
[...]

Modul kelas
{
    fungsi publik onBootstrap (MvcEvent $ e)
    {
    // Mengkonfigurasi sesi untuk menggunakan database
    $ Config = $ e> getApplication () -> getServiceManager () -> mendapatkan ('config');
    $ DbAdapter = baru \ Zend \ Db \ Adapter \ Adapter ($ config ['db']);
    $ SessionOptions = baru \ Zend \ Session \ SaveHandler \ DbTableGatewayOptions ();
    $ SessionTableGateway = baru \ Zend \ Db \ TableGateway \ TableGateway ('sesi', $ dbAdapter);
    $ SaveHandler = baru \ Zend \ Session \ SaveHandler \ DbTableGateway ($ sessionTableGateway, $ sessionOptions);
    $ SessionManager = baru \ Zend \ Session \ SessionManager (NULL, NULL, $ saveHandler);
    Kontainer :: setDefaultManager ($ SessionManager);

[...]
~~~

## Mendorong dan Menyebarkan App Anda
Pilih nama yang unik untuk menggantikan `APP_NAME` tempat untuk aplikasi Anda dan membuatnya pada platform CloudKilat:

~~~ Pesta
$ Ironcliapp APP_NAME membuat php
~~~

Mendorong kode Anda ke repositori aplikasi, yang memicu penyebaran gambar proses build:

~~~ Pesta
$ Ironcliapp APP_NAME / dorongan bawaan
Menghitung objek: 2208, dilakukan.
Delta kompresi menggunakan sampai 4 benang.
Mengompresi objek: 100% (771/771), dilakukan.
Menulis objek: 100% (2208/2208), 869,14 KiB | 180.00 KiB / s, dilakukan.
Total 2208 (delta 1087), kembali 2208 (delta 1087)
       
-----> Mendorong Menerima
       Submodule 'vendor / ZF2' (https://github.com/zendframework/zf2.git) terdaftar untuk jalur 'vendor / ZF2'
       Diinisialisasi repositori Git kosong di /data/applications/APP_NAME/git-push-92157c6dc50dfab545adbda2761e4ef5f2138dd9-sDyGf40f/builddir/vendor/ZF2/.git/
       Submodule jalan 'vendor / ZF2': memeriksa '6022f490695b1c835070d9e5a81b45dc20b4a51c'
       Repositori Memuat komposer dengan informasi paket
       Instalasi dependensi (termasuk membutuhkan-dev)
         - Instalasi ZendFramework / ZendFramework (2.2.1)
           Download: 100%
       ...
       Menulis file kunci
       Menghasilkan file autoload
-----> Zend Framework 2.x terdeteksi
-----> Gambar Building
-----> Gambar Mengunggah (3.1M)
       
Untuk ssh: //APP_NAME@kilatiron.net/repository.git
 * [Cabang baru] Master -> Master
~~~

Terakhir namun tidak sedikit menyebarkan versi terbaru dari aplikasi dengan ironapp yang menyebarkan perintah:

~~~ Pesta
$ Ironcliapp APP_NAME / default menyebarkan
~~~

## Tambahkan Database Diperlukan MySQL Add-on dan Inisialisasi Tabel Sesi

Untuk menyimpan sesi kita perlu menambahkan database Add-on dan menginisialisasi meja.

Kita akan menggunakan [yang MySQLs Add-on ini rencana bebas] (https://community.CloudKilat.ch/tutorial/mysqls-add-on/). Ini menyediakan database bersama gratis untuk pengujian dan pengembangan.

Membuat tabel sesi mudah dengan menjalankan perintah init-sesi-tabel termasuk dalam run-wadah:

~~~ Pesta
# Tambahkan Add-on
$ Ironcliapp APP_NAME / default addon.add mysqls.free
# Menginisialisasi tabel sesi
$ Ironcliapp APP_NAME / default run "kode php / public / index.php init-sesi-table"
Menghubungkan ...
[SUKSES] Sesi meja dibuat.
Koneksi ke sshforwarder.kilatiron.net ditutup.
~~~

Et voila, app sekarang dan berjalan di `http [s]: // APP_NAME.kilatiron.net`.

[PHP buildpack]: https://github.com/cloudControl/buildpack-php
[CloudKilat]: http://www.cloudkilat.com/
[Contoh aplikasi]: https://github.com/cloudControl/php-zend2-example-app.git
