# Menyebarkan Aplikasi Node.js
[Node.js] adalah platform yang dibangun di atas JavaScript runtime Chrome untuk membangun aplikasi jaringan yang cepat dan terukur. -Event yang, non-blocking I / O Model membuat kerangka ringan dan efisien untuk membangun real-time aplikasi cloud data-intensif.

Tutorial ini menunjukkan bagaimana untuk membangun dan menyebarkan aplikasi Hello World Node.js sederhana pada [CloudKilat]. Check out [Node.js buildpack] fitur untuk didukung.

## The Node.js App Dijelaskan

### Dapatkan App
Pertama, mengkloning Node.js App dari repositori kami pada Github:

~~~ Pesta
$ Git clone https://github.com/cloudControl/nodejs-express-example-app.git
$ Cd nodejs-express-contoh-aplikasi
~~~

Sekarang Anda memiliki aplikasi Node.js kecil, tapi berfungsi penuh.

### Ketergantungan Tracking
The Node.js buildpack melacak dependensi menggunakan [NPM]. Ketergantungan
persyaratan yang ditetapkan dalam file `package.json` yang perlu berada di
akar repositori Anda. Untuk aplikasi Hello World, satu-satunya
persyaratan adalah Express. The `package.json` Anda kloning sebagai bagian dari contoh
app terlihat seperti ini:

~~~ Json
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

Anda harus selalu menentukan versi dependensi Anda jika Anda ingin membangun menjadi direproduksi dan untuk mencegah kesalahan tak terduga yang disebabkan oleh perubahan versi.

### Proses Type Definition
A [Procfile] diperlukan untuk memulai proses pada platform CloudKilat. Harus ada sebuah file yang bernama `Procfile` pada akar repositori Anda. Dalam kode contoh Anda sudah kloning itu terlihat seperti ini:

~~~
web.js simpul: web
~~~

Kiri dari usus besar, kami menetapkan ** diperlukan ** Jenis proses yang disebut `web` diikuti dengan perintah yang dimulai app.

## Mendorong dan Menyebarkan App Anda
Pilih nama yang unik untuk menggantikan `APP_NAME` tempat untuk aplikasi Anda
dan menciptakannya pada platform CloudKilat:

~~~ Pesta
$ Ironcliapp APP_NAME membuat nodejs
~~~

Mendorong kode Anda ke repositori aplikasi, yang memicu penyebaran gambar proses build:

~~~ Pesta
$ Ironcliapp APP_NAME / dorongan bawaan
Menghitung objek: 307, dilakukan.
Delta kompresi menggunakan hingga 8 benang.
Mengompresi objek: 100% (261/261), dilakukan.
Menulis objek: 100% (307/307), 202,14 KiB | 0 byte / s, dilakukan.
Total 307 (delta 18), kembali 307 (delta 18)

-----> Mendorong Menerima
-----> Menyelesaikan versi mesin
       Menggunakan versi Node.js: 0.10.13
       Menggunakan versi NPM: 1.3.2
-----> Mengambil Node.js binari
-----> Vendoring simpul ke siput
-----> Dependensi Instalasi dengan NPM
       [...]
       express@3.3.8 node_modules / ekspres
       ├── methods@0.0.1
       ├── range-parser@0.0.4
       ├── cookie-signature@1.0.1
       ├── fresh@0.2.0
       ├── buffer-crc32@0.2.1
       ├── cookie@0.1.0
       ├── debug@0.7.2
       ├── mkdirp@0.3.5
       ├── commander@1.2.0 (keypress@0.1.0)
       ├── send@0.1.4 (mime@1.2.11)
       └── connect@2.8.8 (uid2@0.0.2, pause@0.0.1, qs@0.6.5,
       [...]
       Dependensi diinstal
-----> Bangunan lingkungan runtime
-----> Gambar Building
-----> Gambar Mengunggah (4.3m)

Untuk ssh: //APP_NAME@kilatiron.net/repository.git
 * [Cabang baru] Master -> Master
~~~

Last but not least, menyebarkan versi terbaru dari aplikasi dengan ironapp yang menyebarkan perintah:

~~~ Pesta
$ Ironcliapp APP_NAME / default menyebarkan
~~~

Selamat, Anda sekarang dapat melihat aplikasi Node.js Anda berjalan pada
`Http [s]: // APP_NAME.kilatiron.net`.


[Node.js]: http://nodejs.org/
[NPM]: https://npmjs.org/
[CloudKilat]: http://www.cloudkilat.com/
[Node.js buildpack]: https://github.com/cloudControl/buildpack-nodejs
[Procfile]: /Platform%20Documentation.md/#buildpacks-and-the-procfile
[Platform dokumentasi]: /Platform%20Documentation.md
