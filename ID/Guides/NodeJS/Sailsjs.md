# Menyebarkan Aplikasi Sails.js

Dalam panduan ini kita akan menunjukkan cara untuk menyebarkan [Sails.js] aplikasi pada [CloudKilat]. Sails.js adalah real-time [Node.js] kerangka MVC, yang dirancang untuk meniru pola kerangka seperti [Ruby on Rails]. Hal ini memungkinkan Anda untuk dengan mudah membuat aplikasi dengan Node.js menggunakan pola MVC untuk mengatur kode Anda sehingga lebih mudah untuk mempertahankan.

Jika Anda baru untuk Sails.js, pertama, memeriksa [Sails mendapatkan mulai halaman] untuk info lebih lanjut tentang cara menginstal Sails.

## The Sails.js App Dijelaskan

### Dapatkan App

Pertama, mengkloning aplikasi Sails.js dari repositori kami:

~~~ Pesta
$ Git clone https://github.com/cloudControl/nodejs-sails-example-app.git
$ Cd nodejs-layar-contoh-aplikasi
~~~

Sekarang Anda memiliki aplikasi Sails.js kecil, tapi berfungsi penuh.

### Ketergantungan Tracking

Dependensi dilacak menggunakan [NPM] dan ditentukan dalam `package.json`-file dalam direktori root proyek Anda.
Yang Anda kloning sebagai bagian dari contoh aplikasi terlihat seperti ini:

~~~ Json
{
    "Nama": "layar-todomvc",
    "Pribadi": true,
    "Versi": "0.0.0",
    "Description": "aplikasi Sails",
    "Dependensi": {
        "Layar": "0.9.7",
        "Mendengus": "0.4.1",
        "Layar-disk": "~ 0.9.0",
        "Ejs": "0.8.4",
        "Optimis": "0.3.4",
        "Layar-mysql": "0.9.5"
    },
    "Script": {
        "Mulai": "simpul app.js",
        "Debug": "simpul men-debug app.js"
    },
    "Main": "app.js",
    "Repositori": "",
    "Penulis": "",
    "Lisensi": ""
}
~~~

### Proses Type Definition
CloudKilat menggunakan [Procfile] untuk memulai proses aplikasi. The `Procfile` dapat ditemukan di tingkat akar repositori Anda.

Untuk memulai server layar, Anda perlu menggunakan `layar lift` perintah. Perintah ini termasuk dalam definisi procfile seperti berikut:

~~~
web: ekspor NODE_ENV = produksi; angkat layar
~~~

Kiri dari usus besar kita ditentukan ** diperlukan ** Jenis proses yang disebut `web` untuk aplikasi web dan diikuti dengan perintah yang dimulai server Sails.

### Menghubungkan Aplikasi Sails.js ke Database
Sails.js adalah database yang agnostik. Ini menyediakan akses data lapisan sederhana yang bekerja, tidak peduli apa database yang Anda gunakan. Yang harus Anda lakukan adalah steker di salah satu adapter untuk database Anda. Di sini, kami tunjukkan cara untuk menghubungkan aplikasi Sails.js Anda ke database MySQL menggunakan CloudKilat [Bersama MySQL Add-on].

Silahkan lihat di `config / file adapter.js` sehingga Anda dapat mengetahui bagaimana [mendapatkan mandat MySQL] disediakan oleh MySQLs Add-on:

~~~ Javascript
module.exports.adapters = {

    'Default': process.env.NODE_ENV || 'pembangunan',

    pembangunan: {
        Modul: 'layar-mysql',
        host: 'localhost',
        pengguna: 'todouser',
        password: 'todopass',
        Database: 'todomvc',
        Kolam renang: benar,
        connectionLimit: 2,
        waitForConnections: true
    },

    Produksi: {
        Modul: 'layar-mysql',
        tuan rumah: process.env.MYSQLS_HOSTNAME,
        pengguna: process.env.MYSQLS_USERNAME,
        password: process.env.MYSQLS_PASSWORD,
        Database: process.env.MYSQLS_DATABASE,
        Kolam renang: benar,
        connectionLimit: 2,
        waitForConnections: true
    }
};
~~~

### Socket.io dan Dukungan WebSocket

Dalam Sails.js, komunikasi client-backend dilakukan dengan menggunakan [WebSockets]. Untuk lebih jelasnya, kita lihat di [CloudKilat WebSockets dokumentasi].

## Mendorong dan Menyebarkan Anda Sails.js App
Untuk menyebarkan aplikasi Sails.js Anda, pilih nama yang unik untuk menggantikan `APP_NAME` tempat untuk aplikasi Anda dan membuatnya pada platform CloudKilat:

~~~ Pesta
$ Ironcliapp APP_NAME membuat nodejs
~~~

Mendorong kode Anda ke repositori aplikasi, yang memicu penyebaran gambar proses build:

~~~ Pesta
$ Ironcliapp APP_NAME / dorongan bawaan
Menghitung benda: 73, dilakukan.
Delta kompresi menggunakan hingga 8 benang.
Mengompresi objek: 100% (35/35), dilakukan.
Menulis objek: 100% (73/73), 267,28 KiB | 0 byte / s, dilakukan.
Total 73 (delta 30), kembali 73 (delta 30)

-----> Mendorong Menerima
-----> Menyelesaikan versi mesin

       Menggunakan versi Node.js: 0.10.15
       Menggunakan versi NPM: 1.3.5
-----> Mengambil Node.js binari
-----> Dependensi Instalasi dengan NPM
       ...
       Dependensi diinstal
-----> Bangunan lingkungan runtime
-----> Gambar Building
-----> Gambar Mengunggah (17M)

Untuk ssh: //APP_NAME@kilatiron.net/repository.git
 * [Cabang baru] Master -> Master
~~~

Tambahkan [Bersama MySQL Add-on]:
~~~ Pesta
$ Ironcliapp APP_NAME / default addon.add mysqls.free
~~~

Akhirnya, menyebarkan aplikasi Sails.js:
~~~ Pesta
$ Ironcliapp APP_NAME / default menyebarkan
~~~

Selamat, Anda sekarang dapat melihat aplikasi Sails.js Anda berjalan pada
`Http: // APP_NAME.kilatiron.net`.

[Node.js]: http://nodejs.org/
[Sails.js]: http://sailsjs.org/
[Sails mendapatkan halaman mulai]: http://sailsjs.org/#!getStarted
[Ruby on Rails]: http://rubyonrails.org/
[NPM]: https://npmjs.org/
[CloudKilat]: http://www.cloudkilat.com/
[Procfile]: /Platform%20Documentation.md/#buildpacks-and-the-procfile
[Mendapatkan mandat MySQL]: /Guides/NodeJS/Add-on%20credentials.md
[WebSockets]: http://socket.io/
[CloudKilat WebSockets dokumentasi]: /Platform%20Documentation.md/#websockets
[Bersama MySQL Add-on]: /Add-on%20Documentation/Data%20Storage/MySQLs.md
