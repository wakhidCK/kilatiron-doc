# Menyebarkan aplikasi Flask
[Flask] adalah microframework untuk Python berdasarkan Werkzeug dan Jinja 2.

Dalam tutorial ini kita akan mempelajari cara untuk men-deploy aplikasi Flask
pada [KilatIron]. Anda dapat menemukan [kode sumber di Github] [Contoh-aplikasi] dan
memeriksa [Python buildpack] untuk mengetahui fitur-fitur yang didukung.

## Aplikasi Flask

### Dapatkan Aplikasi
Pertama, mari kita mengkloning Flask App dari repositori kami pada Github:

~~~bash
$ git clone https://github.com/cloudControl/python-flask-example-app.git
$ cd python-flask-example-app
~~~

Sekarang Anda sudah memiliki aplikasi Flask sederhana.

### Melacak Ketergantungan
Python buildpack melacak ketergantungan melalui pip dan file `requirements.txt`.
File tersebut perlu ditempatkan di direktori root dari repositori Anda.
Contoh aplikasi hanya menentukan Flask sebagai dependensi dan terlihat seperti ini:

~~~pip
Flask==0.10.1
~~~

### Proses Type Definition
KilatIron menggunakan [Procfile] untuk mengetahui bagaimana cara memulai proses Anda.

Pada contoh kode terdapat sebuah file yang bernama `Procfile` di tingkat atas repositori Anda. File tersebut terlihat seperti ini:

~~~
web: server.py python
~~~

Kolom paling kiri **diperlukan** untuk mengetahui jenis proses, pada contoh ini dinamai `web` diikuti dengan perintah untuk memulai aplikasi.

## Kode Aplikasi Flask

Contoh kode cukup sederhana. Dimulai dari import modul-modul yang dibutuhkan dan membuat instance
dari class Flask. Lalu set route untuk fungsi hello() yang akan me-render template jinja.

~~~python
import os
from flask import Flask, render_template

app = Flask(__name__)


@app.route('/')
def hello():
    return render_template('hello.jinja', domain=os.environ['DOMAIN'])

app.debug = True
app.run(host='0.0.0.0', port=int(os.environ['PORT']))
~~~

## Push dan Deploy Aplikasi

Pilih nama yang unik untuk menggantikan `APP_NAME` untuk aplikasi Anda dan membuatnya pada platform KilatIron:

~~~bash
$ ironapp APP_NAME create python
~~~

Push kode Anda ke repositori aplikasi, yang memicu proses pembuatan image container:
~~~bash
$ ironapp APP_NAME/default push
Counting objects: 3, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 283 bytes | 0 bytes/s, done.
Total 3 (delta 1), reused 0 (delta 0)

-----> Receiving push
-----> No runtime.txt provided; assuming python-2.7.8.
-----> Preparing Python runtime (python-2.7.8)
-----> Installing Distribute (0.6.36)
-----> Installing Pip (1.3.1)
-----> Installing dependencies using Pip (1.3.1)
       Downloading/unpacking Flask==0.10.1 (from -r requirements.txt (line 1))
        ...
       Successfully installed Flask Werkzeug Jinja2 itsdangerous MarkupSafe
       Cleaning up...
-----> Building image
-----> Uploading image (25.1 MB)

To ssh://APP_NAME@kilatiron.net/repository.git
 * [new branch]      master -> master
~~~

Yang terakhir harus dilakukan adalah menyebarkan versi terbaru dari aplikasi dengan perintah ironapp deploy:

~~~bash
$ ironapp APP_NAME/default deploy
~~~

Selamat, Anda sekarang dapat melihat aplikasi Flask Anda berjalan pada `http[s]://APP_NAME.kilatiron.net`.

[Flask]: http://flask.pocoo.org/
[KilatIron]: http://www.cloudkilat.com/
[Python buildpack]: https://github.com/cloudControl/buildpack-python
[Procfile]: /Platform%20Documentation.md/#buildpacks-and-the-procfile
[Contoh-aplikasi]: https://github.com/cloudControl/python-flask-example-app.git
