# Menyebarkan Aplikasi Tornado

[Tornado] adalah versi open source dari scalable, non-blocking web server yang
dan alat-alat yang listrik FriendFeed ditulis dengan Python.

Dalam tutorial ini kita akan menunjukkan cara untuk menyebarkan Tornado sederhana berdasarkan
aplikasi pada [CloudKilat].

## The Contoh App Dijelaskan

### Dapatkan App
Pertama, mari kita mengkloning contoh kode dari Github.
~~~ Pesta
$ Git clone https://github.com/cloudControl/python-tornado-example-app.git
$ Cd python-tornado-contoh-aplikasi
~~~

Kode dari contoh repositori siap untuk digunakan. Mari kita masih pergi
melalui file yang berbeda dan tujuan sebenarnya mereka cepat.

### Ketergantungan Tracking

The [Python buildpack] melacak dependensi melalui pip dan `requirements.txt`
mengajukan. Ini perlu ditempatkan di direktori root dari repositori Anda. Itu
contoh aplikasi menspesifikasikan hanya Tornado dirinya sebagai ketergantungan. Yang Anda kloning
sebagai bagian dari contoh aplikasi terlihat seperti ini:
~~~ Pip
tornado == 2.4.1
~~~

### Proses Type Definition
CloudKilat menggunakan [Procfile] tahu bagaimana untuk memulai proses aplikasi.

Contoh kode sudah termasuk sebuah file yang bernama `Procfile` di tingkat atas
repositori Anda. Ini terlihat seperti ini:
~~~
web: python app.py --port = $ PORT --logging = error
~~~

Kiri dari usus besar kita ditentukan ** tipe proses ** diperlukan disebut `web`
diikuti dengan perintah yang dimulai aplikasi dan mendengarkan pada port tertentu
oleh variabel lingkungan `$ PORT`.

### The Aktual Kode Aplikasi

Kode aplikasi yang sebenarnya benar-benar lurus ke depan. Ini menyediakan satu handler
dan set up aplikasi untuk menangani permintaan yang masuk asynchronous dan menjadikan
didefinisikan Template.

Akhirnya, kita mendefinisikan parameter baris perintah untuk menentukan pelabuhan sebagai
terlihat pada `Procfile` atas, instantiate server HTTP, membuatnya mendengarkan pada
port tertentu dan mulai server forking satu proses per CPU core.
~~~ Python
dari tornado.options mengimpor menentukan, pilihan
impor tornado.ioloop
impor tornado.web
impor tornado.httpserver


kelas MainHandler (tornado.web.RequestHandler):
    @ Tornado.web.asynchronous
    def mendapatkan (self):
        self._async_callback ()

    def _async_callback (self):
        self.write ("Hello, world!")
        self.finish ()

app = tornado.web.Application ([
    (R "/", MainHandler),
])

define ("pelabuhan", default = "5555", bantuan = "Pelabuhan untuk mendengarkan pada")

jika __name__ == "__main__":
    tornado.options.parse_command_line ()
    Server = tornado.httpserver.HTTPServer (app)
    server.bind (options.port)
    # Core cpu autodetect dan garpu satu proses per inti
    coba:
        server.start (0)
        tornado.ioloop.IOLoop.instance (). start ()
    kecuali KeyboardInterrupt:
        tornado.ioloop.IOLoop.instance (). stop ()
~~~

## Mendorong dan Menyebarkan App

Pilih nama yang unik untuk menggantikan `APP_NAME` tempat untuk aplikasi Anda
dan menciptakannya pada platform CloudKilat:
~~~ Pesta
$ Ironcliapp APP_NAME membuat python
~~~

Mendorong kode Anda ke repositori aplikasi, yang memicu penyebaran
image membangun proses:
~~~ Pesta
$ Ironcliapp APP_NAME / dorongan bawaan
Menghitung objek: 7, dilakukan.
Delta kompresi menggunakan sampai 4 benang.
Mengompresi objek: 100% (4/4), dilakukan.
Menulis objek: 100% (7/7), 1,01 KiB, dilakukan.
Total 7 (delta 0), kembali 7 (delta 0)
       
-----> Mendorong Menerima
-----> Ada runtime.txt tersedia; asumsi python-2.7.3.
-----> Menggunakan runtime Python (python-2.7.3)
-----> Dependensi Instalasi menggunakan Pip (1.3.1)
       Download / membongkar tornado == 2.4.1 (dari requirements.txt r (baris 1))
       ...
       Tornado berhasil diinstal
       Membersihkan ...
-----> Gambar Building
-----> Gambar Mengunggah (25M)
       
Untuk ssh: //APP_NAME@kilatiron.net/repository.git
 + [Cabang baru] Master -> Master
~~~

Terakhir namun tidak sedikit menyebarkan versi terbaru dari aplikasi dengan ironapp yang
menyebarkan perintah.
~~~ Pesta
$ Ironcliapp APP_NAME / default menyebarkan
~~~

Selamat, Anda sekarang dapat melihat aplikasi berjalan Tornado di `http: // APP_NAME.kilatiron.net`.

[Tornado]: http://www.tornadoweb.org
[CloudKilat]: http://www.cloudkilat.com/
[Python buildpack]: https://github.com/cloudControl/buildpack-python
[Procfile]: /Platform%20Documentation.md/#buildpacks-and-the-procfile
