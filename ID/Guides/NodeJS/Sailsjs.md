# Deploy Aplikasi Sails.js

Dalam panduan ini kami akan menunjukkan bagaimana cara men-Deploy aplikasi [Sails.js] di [CloudKilat]. Sails.js adalah real-time [Node.js] MVC framework, yang dirancang untuk meniru pola dari framework seperti [Ruby on Rails]. Hal ini memudahkan anda untuk membuat aplikasi dengan Node.js menggunakan MVC framework untuk mengatur kode Anda sehingga lebih mudah untuk dipelihara.

Jika Anda seorang pemula pada Sails.js. Pertama, kunjungi [halaman permulaan Sails] untuk info lebih lanjut tentang cara menginstal Sails.

## Penjabaran Aplikasi Sails.js

### Mendapatkan Aplikasi

Pertama, mengkloning aplikasi Sails.js dari repositori kami:

~~~bash
$ git clone https://github.com/cloudControl/nodejs-sails-example-app.git
$ cd nodejs-sails-example-app
~~~

Sekarang Anda memiliki aplikasi Sails.js sederhana, tapi berfungsi penuh.

### Dependensi Tracking

Dependensi dilacak menggunakan [NPM] dan ditentukan didalam file `package.json` di direktori root proyek Anda.
Yang Anda kloning sebagai bagian dari contoh aplikasi terlihat seperti ini:

~~~json
{
    "name": "sails-todomvc",
    "private": true,
    "version": "0.0.0",
    "description": "a Sails application",
    "dependencies": {
        "sails": "0.9.7",
        "grunt": "0.4.1",
        "sails-disk": "~0.9.0",
        "ejs": "0.8.4",
        "optimist": "0.3.4",
        "sails-mysql": "0.9.5"
    },
    "scripts": {
        "start": "node app.js",
        "debug": "node debug app.js"
    },
    "main": "app.js",
    "repository": "",
    "author": "",
    "license": ""
}
~~~

### Proses Type Definition
CloudKilat menggunakan [Procfile] untuk memulai proses aplikasi. `Procfile` dapat ditemukan di level root repositori Anda.

Untuk menjalankan server sails, gunakan perintah `sails lift`. Perintah ini termasuk dalam definisi procfile, seperti yang ditunjukkan di bawah ini:

~~~
web:  export NODE_ENV=production; sails lift
~~~

Keluar dari clon yang kita tentukan **diperlukan** Jenis proses yang disebut `web` untuk aplikasi web dan diikuti dengan perintah menjalankan server Sails.

### Menghubungkan Aplikasi Sails.js ke Database
Sails.js adalah database agnostik. Dia bekerja menyediakan akses data lapisan sederhana, tidak peduli apa database yang Anda gunakan. Yang harus Anda lakukan adalah memasangnya di salah satu adapter untuk database Anda. Di sini, kami tunjukkan cara untuk menghubungkan aplikasi Sails.js Anda ke database MySQL menggunakan [Shared MySQL Add-on] CloudKilat.

Silahkan lihat di file `config / file adapter.js` sehingga Anda dapat mengetahui bagaimana [mendapatkan MySQL credentials] yang disediakan oleh MySQLs Add-on:

~~~javascript
module.exports.adapters = {

    'default': process.env.NODE_ENV || 'development',

    development: {
        module: 'sails-mysql',
        host: 'localhost',
        user: 'todouser',
        password: 'todopass',
        database: 'todomvc',
        pool: true,
        connectionLimit: 2,
        waitForConnections: true
    },

    production: {
        module: 'sails-mysql',
        host: process.env.MYSQLS_HOSTNAME,
        user: process.env.MYSQLS_USERNAME,
        password: process.env.MYSQLS_PASSWORD,
        database: process.env.MYSQLS_DATABASE,
        pool: true,
        connectionLimit: 2,
        waitForConnections: true
    }
};
~~~

### Socket.io dan Dukungan WebSocket

Dalam Sails.js, komunikasi client-backend dilakukan dengan menggunakan [WebSockets]. Untuk lebih jelasnya, kita lihat di [dokumentasi CloudKilat WebSockets].

## Push dan Deploy Aplikasi Sails.js Anda
Untuk men-Deploy aplikasi Sails.js Anda, pilih nama yang unik untuk menggantikan `APP_NAME` aplikasi Anda dan membuatnya di platform CloudKilat:

~~~bash
$ ironapp APP_NAME create nodejs
~~~

Push kode Anda ke repositori aplikasi, yang memicu proses build deployment image:

~~~bash
$ ironapp APP_NAME/default push
Counting objects: 73, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (35/35), done.
Writing objects: 100% (73/73), 267.28 KiB | 0 bytes/s, done.
Total 73 (delta 30), reused 73 (delta 30)

-----> Receiving push
-----> Resolving engine versions

       Using Node.js version: 0.10.15
       Using npm version: 1.3.5
-----> Fetching Node.js binaries
-----> Installing dependencies with npm
       ...
       Dependencies installed
-----> Building runtime environment
-----> Building image
-----> Uploading image (17M)

To ssh://APP_NAME@kilatiron.net/repository.git
 * [new branch]      master -> master
~~~

Tambahkan [Shared MySQL Add-on]:
~~~bash
$ ironapp APP_NAME/default addon.add mysqls.free
~~~

Terakhir, deploy aplikasi Sails.js:
~~~bash
$ ironapp APP_NAME/default deploy
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
