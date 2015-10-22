# Aplikasi Tornado

[Tornado] adalah versi open source dari non-blocking web server yang
digunakan untuk FriendFeed dan ditulis dengan Python.

Dalam tutorial ini kita akan menunjukkan cara untuk men-deploy aplikasi
Tornado sederhana pada [KilatIron].

## Contoh Aplikasi

### Dapatkan Aplikasi
Pertama, mari kita mengkloning contoh kode dari Github.
~~~bash
$ git clone https://github.com/cloudControl/python-tornado-example-app.git
$ cd python-tornado-example-app
~~~

Contoh kode dari repositori sudah siap untuk digunakan. Tetapi mari kita
menelusuri file-file yang ada untuk mengetahui kegunaan dari file-file tersebut.

### Melacak Ketergantungan

[Python buildpack] melacak ketergantungan melalui pip dan file `requirements.txt`.
File tersebut perlu ditempatkan di direktori root dari repositori Anda.
Contoh aplikasi hanya menentukan Tornado sebagai dependensi dan terlihat seperti ini:
~~~pip
tornado==2.4.1
~~~

### Proses Type Definition
KilatIron menggunakan [Procfile] untuk mengetahui bagaimana cara memulai proses Anda.

Pada contoh kode terdapat sebuah file yang bernama `Procfile` di tingkat atas repositori Anda. File tersebut terlihat seperti ini:

~~~
web: python app.py --port=$PORT --logging=error
~~~

Kolom paling kiri **diperlukan** untuk mengetahui jenis proses, pada contoh ini dinamai `web` diikuti dengan
 perintah untuk memulai aplikasi dan environment variable `PORT` untuk menentukan pada port berapa aplikasi akan melayani.

### Kode Aplikasi Tornado

Contoh kode cukup sederhana. Dimulai dengan import modul-modul yang dibutuhkan dan menyediakan
satu handler, lalu menyiapkan aplikasi untuk melayani permintaan dengan metode asynchronous dan
me-render template yang ditunjuk.

Lalu, mendefinisikan parameter command line untuk menentukan port seperti yang didefinisikan
pada `Procfile` di atas, membuat instance dari HTTP server, mendengarkan pada port yang diinginkan,
dan mulai fork proses sejumlah CPU yang tersedia.

~~~python
from tornado.options import define, options
import tornado.ioloop
import tornado.web
import tornado.httpserver


class MainHandler(tornado.web.RequestHandler):
    @tornado.web.asynchronous
    def get(self):
        self._async_callback()

    def _async_callback(self):
        self.write("Hello, world!")
        self.finish()

app = tornado.web.Application([
    (r"/", MainHandler),
])

define("port", default="5555", help="Port to listen on")

if __name__ == "__main__":
    tornado.options.parse_command_line()
    server = tornado.httpserver.HTTPServer(app)
    server.bind(options.port)
    # autodetect cpu cores and fork one process per core
    try:
        server.start(0)
        tornado.ioloop.IOLoop.instance().start()
    except KeyboardInterrupt:
        tornado.ioloop.IOLoop.instance().stop()
~~~

## Push dan Deploy Aplikasi

Pilih nama yang unik untuk menggantikan `APP_NAME` untuk aplikasi Anda dan membuatnya pada platform KilatIron:

~~~bash
$ ironapp APP_NAME create python
~~~

Push kode Anda ke repositori aplikasi, yang memicu proses pembuatan image container:
~~~bash
$ ironapp APP_NAME/default push
Counting objects: 7, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (7/7), 1.01 KiB, done.
Total 7 (delta 0), reused 7 (delta 0)

-----> Receiving push
-----> No runtime.txt provided; assuming python-2.7.3.
-----> Using Python runtime (python-2.7.3)
-----> Installing dependencies using Pip (1.3.1)
       Downloading/unpacking tornado==2.4.1 (from -r requirements.txt (line 1))
       ...
       Successfully installed tornado
       Cleaning up...
-----> Building image
-----> Uploading image (25M)

To ssh://APP_NAME@kilatiron.net/repository.git
 + [new branch] master -> master
~~~

Yang terakhir harus dilakukan adalah menyebarkan versi terbaru dari aplikasi dengan perintah ironapp deploy:
~~~bash
$ ironapp APP_NAME/default deploy
~~~

Selamat, Anda sekarang dapat melihat aplikasi Tornado Anda berjalan di `http[s]://APP_NAME.kilatiron.net`.

[Tornado]: http://www.tornadoweb.org
[KilatIron]: http://www.cloudkilat.com/
[Python buildpack]: https://github.com/cloudControl/buildpack-python
[Procfile]: /Platform%20Documentation.md/#buildpacks-and-the-procfile
