# Aplikasi Node.js
[Node.js] adalah platform yang dibangun di atas runtime JavaScript milik Chrome untuk membangun aplikasi
jaringan yang cepat dan terukur. Event yang bermodel non-blocking I/O membuat framework ringan dan efisien
untuk membangun aplikasi real-time di Cloud dengan perubahan data yang intensif.

Tutorial ini menunjukkan bagaimana cara membangun aplikasi Hello World Node.js sederhana pada [KilatIron].
Anda dapat memeriksa [Node.js buildpack] untuk mengetahui fitur-fitur untuk didukung.

## Aplikasi Node.js

### Dapatkan Aplikasi
Pertama, mengkloning aplikasi Node.js dari repositori kami pada Github:

~~~bash
$ git clone https://github.com/cloudControl/nodejs-express-example-app.git
$ cd nodejs-express-example-app
~~~

Sekarang Anda memiliki aplikasi Node.js sederhana.

### Melacak Ketergantungan
Node.js buildpack melacak ketergantungan dengan menggunakan [NPM]. Ketergantungan
ditetapkan dalam file `package.json` yang diletakkan pada direktory root repositori Anda.
Untuk aplikasi Hello World, satu-satunya ketergantungan adalah Express. `package.json` yang
Anda kloning sebagai bagian dari aplikasi contoh terlihat seperti ini:

~~~json
{
  "Nama": "nodejs-express-contoh-aplikasi",
  "Versi": "0.0.1",
  "Dependensi": {
    "Mengungkapkan": "~ 3.3.4"
  },
  "Mesin": {
    "Node": "0.10.13",
    "NPM": "1.3.2"
  }
}
~~~

Anda harus menentukan versi dependensi Anda jika Anda ingin membangun aplikasi yang dapat
direproduksi dan untuk mencegah kesalahan tak terduga yang disebabkan oleh perubahan versi.

### Proses Type Definition
[Procfile] diperlukan untuk memulai proses pada platform KilatIron. Harus terdapat sebuah
file yang bernama `Procfile` pada directory root repositori Anda. Dalam contoh kode,
file tersebut berisi:

~~~
web: node web.js
~~~

Kolom paling kiri **diperlukan** untuk mengetahui jenis proses, pada contoh ini dinamai
`web` diikuti dengan perintah untuk menjalankan aplikasi.

## Push dan Deploy Aplikasi

Pilih nama yang unik untuk menggantikan `APP_NAME` untuk aplikasi Anda dan membuatnya pada platform KilatIron:

~~~bash
$ ironapp APP_NAME create nodejs
~~~

Push kode Anda ke repositori aplikasi, yang memicu proses pembuatan image container:

~~~bash
$ ironapp APP_NAME push
Counting objects: 344, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (294/294), done.
Writing objects: 100% (344/344), 461.55 KiB | 412.00 KiB/s, done.
Total 344 (delta 24), reused 337 (delta 20)

-----> Receiving push
-----> Requested node range:  0.10.13
-----> Resolved node version: 0.10.13
-----> Downloading and installing node
-----> Installing dependencies
       […]
       express@4.10.2 node_modules/express
       +-- utils-merge@1.0.0
       +-- merge-descriptors@0.0.2
       +-- fresh@0.2.4
       +-- escape-html@1.0.1
       +-- cookie@0.1.2
       +-- range-parser@1.0.2
       +-- cookie-signature@1.0.5
       +-- finalhandler@0.3.2
       +-- vary@1.0.0
       +-- media-typer@0.3.0
       +-- methods@1.1.0
       +-- parseurl@1.3.0
       +-- serve-static@1.7.1
       +-- content-disposition@0.5.0
       +-- path-to-regexp@0.1.3
       +-- depd@1.0.0
       +-- qs@2.3.2
       +-- debug@2.1.0 (ms@0.6.2)
       +-- on-finished@2.1.1 (ee-first@1.1.0)
       +-- proxy-addr@1.0.4 (forwarded@0.1.0, ipaddr.js@0.1.5)
       +-- etag@1.5.1 (crc@3.2.1)
       +-- send@0.10.1 (destroy@1.0.3, ms@0.6.2, mime@1.2.11)
       +-- type-is@1.5.3 (mime-types@2.0.3)
       +-- accepts@1.1.3 (negotiator@0.4.9, mime-types@2.0.3)
-----> Caching node_modules directory for future builds
-----> Cleaning up node-gyp and npm artifacts
-----> Building runtime environment
-----> Building image
-----> Uploading image (5.9 MB)


To ssh://APP_NAME@kilatiron.net/repository.git
 * [new branch]      master -> master
~~~

Yang terakhir harus dilakukan adalah menyebarkan versi terbaru dari aplikasi dengan perintah ironapp deploy:

~~~bash
$ ironapp APP_NAME/default deploy
~~~

Selamat, Anda sekarang dapat melihat aplikasi Node.js Anda berjalan di `http[s]://APP_NAME.kilatiron.net`.

## Langkah Selanjutnya
Jika Anda ingin membangun aplikasi Node.js dengan menggunakan database MongoDB, silahkan baca [tutorial Express]. Dan baca [dokumentasi Platform] untuk lebih lanjut mempelajari konsep-konsep yang akan Anda butuhkan untuk menulis, mengeset konfigurasi, deploy, dan menjalankan aplikasi Node.js pada KilatIron


[Node.js]: http://nodejs.org/
[NPM]: https://npmjs.org/
[KilatIron]: http://www.cloudkilat.com/
[Node.js buildpack]: https://github.com/cloudControl/buildpack-nodejs
[Procfile]: /Platform%20Documentation.md/#buildpacks-and-the-procfile
[dokumentasi Platform]: /Platform%20Documentation.md
[tutorial Express]: /Guides/NodeJS/express.md
