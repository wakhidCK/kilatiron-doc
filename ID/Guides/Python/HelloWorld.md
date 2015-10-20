# Menyebarkan aplikasi Flask
[Flask] adalah microframework untuk Python berdasarkan Schieberegler, Jinja 2 dan baik
niat.

Dalam tutorial ini kita akan menunjukkan cara untuk menyebarkan Flask
aplikasi pada [CloudKilat]. Anda dapat menemukan [kode sumber di Github] [example_app] dan memeriksa [Python buildpack] untuk
fitur yang didukung.

## The Flask App Dijelaskan

### Dapatkan App
Pertama, mari kita mengkloning Flask App dari repositori kami pada Github:

~~~ Pesta
$ Git clone https://github.com/cloudControl/python-flask-example-app.git
$ Cd python-labu-contoh-aplikasi
~~~

Sekarang Anda memiliki aplikasi Flask kecil tapi berfungsi penuh.

### Ketergantungan Tracking
Python buildpack melacak dependensi melalui pip dan file `requirements.txt`. Ini perlu ditempatkan di direktori root dari repositori Anda. Contoh aplikasi hanya menentukan Flask dirinya sebagai ketergantungan dan terlihat seperti ini:

~~~ Pip
Flask == 0,9
~~~

### Proses Type Definition
CloudKilat menggunakan [Procfile] tahu bagaimana untuk memulai proses Anda.

Contoh kode sudah termasuk sebuah file yang bernama `Procfile` di tingkat atas repositori Anda. Ini terlihat seperti ini:

~~~
web: server.py python
~~~

Kiri dari usus besar kita ditentukan ** diperlukan ** Jenis proses yang disebut `web` diikuti dengan perintah yang dimulai aplikasi dan mendengarkan pada port yang ditentukan oleh variabel lingkungan` $ PORT`.

## Mendorong dan Menyebarkan App
Pilih nama yang unik untuk menggantikan `APP_NAME` tempat untuk aplikasi Anda dan membuatnya pada platform CloudKilat:

~~~ Pesta
$ Ironcliapp APP_NAME membuat python
~~~

Mendorong kode Anda ke repositori aplikasi, yang memicu penyebaran gambar proses build:

~~~ Pesta
$ Ironcliapp APP_NAME / dorongan bawaan
Menghitung benda: 16, dilakukan.
Delta kompresi menggunakan sampai 4 benang.
Mengompresi objek: 100% (10/10), dilakukan.
Menulis objek: 100% (16/16), 258,30 KiB, dilakukan.
Total 16 (delta 2), kembali 16 (delta 2)
       
-----> Mendorong Menerima
-----> Ada runtime.txt tersedia; asumsi python-2.7.3.
-----> Mempersiapkan runtime Python (python-2.7.3)
-----> Instalasi Distribusikan (0.6.36)
-----> Instalasi Pip (1.3.1)
-----> Dependensi Instalasi menggunakan Pip (1.3.1)
       Download / membongkar Flask == 0,9 (dari requirements.txt r (baris 1))
       ...
       Berhasil diinstal Flask Schieberegler Jinja2 markupsafe
       Membersihkan ...
-----> Gambar Building
-----> Gambar Mengunggah (25M)
       
Untuk ssh: //APP_NAME@kilatiron.net/repository.git
 * [Cabang baru] Master -> Master

~~~

Terakhir namun tidak sedikit menyebarkan versi terbaru dari aplikasi dengan ironapp yang menyebarkan perintah:

~~~ Pesta
$ Ironcliapp APP_NAME / default menyebarkan
~~~

Selamat, Anda sekarang dapat melihat aplikasi Flask Anda berjalan pada `http [s]: // APP_NAME.kilatiron.net`.

[Flask]: http://flask.pocoo.org/
[CloudKilat]: http://www.cloudkilat.com/
[Python buildpack]: https://github.com/cloudControl/buildpack-python
[Procfile]: /Platform%20Documentation.md/#buildpacks-and-the-procfile
[Example_app]: https://github.com/cloudControl/python-flask-example-app.git
