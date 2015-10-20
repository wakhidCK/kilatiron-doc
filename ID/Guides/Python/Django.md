# Menyebarkan Aplikasi Django

Dalam tutorial ini kita akan menunjukkan kepada Anda bagaimana untuk menggunakan aplikasi Django pada [CloudKilat]. Anda dapat menemukan [kode sumber di Github] [contoh-aplikasi] dan memeriksa [Python buildpack] [python buildpack] untuk fitur didukung. Aplikasi mengikuti resmi [Django tutorial] dan memungkinkan Anda untuk membuat, menggunakan dan mengelola jajak pendapat sederhana.

## The Django Aplikasi Dijelaskan

### Dapatkan App

Pertama, mengkloning aplikasi Django dari repositori kami pada Github:

~~~ Pesta
$ Git clone https://github.com/cloudControl/python-django-example-app.git
$ Cd python-Django-contoh-aplikasi
~~~

### Ketergantungan Tracking

Python buildpack melacak dependensi melalui [pip] dan file `requirements.txt`. Ini perlu ditempatkan di direktori root dari repositori Anda. Contoh aplikasi menspesifikasikan [Django] [Django], [sopir MySQL] [mysql-driver] dan [gunicorn] sebagai dependensi. Yang Anda kloning sebagai bagian dari contoh aplikasi terlihat seperti ini:

~~~
Django == 1.4.3
gunicorn == 0.17.2
MySQL-python == 1.2.4
~~~

### Server Produksi

Dalam lingkungan produksi Anda biasanya tidak ingin menggunakan server pengembangan. Kami telah memutuskan untuk menggunakan gunicorn untuk tujuan ini. Untuk melakukannya kami harus memasukkannya dalam daftar aplikasi yang terinstal (`INSTALLED_APPS` di` mysite / settings.py`):

~~~ Python
INSTALLED_APPS = (
    ...
    'Gunicorn'
    ...
)
~~~

### Proses Type Definition

CloudKilat menggunakan [Procfile] tahu bagaimana untuk memulai proses Anda. Contoh kode sudah termasuk file bernama Procfile di tingkat atas repositori Anda. Ini terlihat seperti ini:

~~~
web: python manage.py run_gunicorn -b 0.0.0.0:$PORT
~~~

Kiri dari usus besar kita ditentukan ** diperlukan ** Jenis proses yang disebut `web` diikuti dengan perintah yang dimulai aplikasi dan mendengarkan pada port yang ditentukan oleh variabel lingkungan` $ PORT`.

### Database Produksi

Aplikasi tutorial aslinya menggunakan SQLite sebagai database di semua lingkungan, bahkan produksi satu. Hal ini tidak mungkin untuk menggunakan database SQLite pada CloudKilat karena filesystem ini [tidak terus-menerus] [filesystem]. Untuk menggunakan database, Anda harus memilih penyimpanan data Add-on dari [addons tersedia] [penyimpanan data-addons].

Dalam tutorial ini kita menggunakan [Bersama MySQL Add-on] [mysqls]. Silahkan lihat pada `mysite / settings.py` sehingga Anda dapat mengetahui bagaimana [mendapatkan mandat MySQL] [get-conf] disediakan oleh MySQLs Add-on:

~~~ Python
# Django Pengaturan untuk Proyek mysite.

os impor
impor json

PROJECT_ROOT = os.path.dirname (os.path.dirname (os.path.abspath (__ file__)))

coba:
    # Pengaturan produksi
    f = os.environ ['CRED_FILE']
    db_data = json.load (terbuka (f)) ['MYSQLS']

    db_config = {
        'ENGINE': 'django.db.backends.mysql',
        'NAMA': db_data ['MYSQLS_DATABASE'],
        'PENGGUNA': db_data ['MYSQLS_USERNAME'],
        'PASSWORD': db_data ['MYSQLS_PASSWORD'],
        'HOST': db_data ['MYSQLS_HOSTNAME'],
        'PORT': db_data ['MYSQLS_PORT'],
    }
kecuali KeyError, IOError:
    # Pengaturan pengembangan / test:
    db_config = {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAMA': '{0} /mysite.sqlite3'.format (PROJECT_ROOT),
    }
...
DATABASES = {
    'Default': db_config,
}
...
~~~

## Mendorong dan Menyebarkan App Anda

Pilih nama yang unik untuk menggantikan `APP_NAME` tempat untuk aplikasi Anda dan membuatnya pada platform CloudKilat:

~~~ Pesta
$ Ironcliapp APP_NAME membuat python
~~~

Mendorong kode Anda ke repositori aplikasi, yang memicu penyebaran gambar proses build:

~~~ Pesta
$ Ironcliapp APP_NAME / dorongan bawaan
Menghitung benda: 31, dilakukan.
Delta kompresi menggunakan hingga 8 benang.
Mengompresi objek: 100% (25/25), dilakukan.
Menulis objek: 100% (31/31), 7.11 KiB, dilakukan.
Total 31 (delta 3), kembali 24 (delta 0)

-----> Mendorong Menerima
-----> Ada runtime.txt tersedia; asumsi python-2.7.3.
-----> Mempersiapkan runtime Python (python-2.7.3)
-----> Instalasi Distribusikan (0.6.36)
-----> Instalasi Pip (1.3.1)
-----> Dependensi Instalasi menggunakan Pip (1.3.1)
       Download / membongkar Django == 1.4.3 (dari requirements.txt r (baris 1))
         Menjalankan egg_info setup.py untuk paket Django
       ...
-----> Gambar Building
-----> Gambar Mengunggah (30M)

Untuk ssh: //APP_NAME@kilatiron.net/repository.git
 * [Cabang baru] Master -> Master
~~~

Tambahkan MySQLs Add-on dengan `rencana free` penyebaran dan menyebarkan:
~~~ Pesta
$ Ironcliapp APP_NAME / default addon.add mysqls.free
$ Ironcliapp APP_NAME / default menyebarkan
~~~

Akhirnya, menyiapkan database menggunakan [perintah Run] [ssh-sesi] (saat diminta membuat user admin):

~~~ Pesta
$ Ironcliapp APP_NAME / default run "python manage.py syncdb"
~~~

Anda dapat login ke konsol admin di `APP_NAME.kilatiron.net / admin`, membuat beberapa jajak pendapat dan melihat mereka di` APP_NAME.kilatiron.net / polls`.

Untuk informasi tambahan lihat [Django Catatan] [Django-catatan] dan lainnya [-python spesifik dokumen] [python-panduan].

[Django]: https://www.djangoproject.com/
[CloudKilat]: http://www.cloudkilat.com/
[CloudKilat]: http://www.cloudkilat.com/
[CloudKilat-doc-user]: /Platform%20Documentation.md/#user-accounts
[CloudKilat-doc-cmdline]: /Platform%20Documentation.md/#command-line-client-web-console-and-api
[Procfile]: /Platform%20Documentation.md/#buildpacks-and-the-procfile
[Git]: https://help.github.com/articles/set-up-git
[Filesystem]: /Platform%20Documentation.md/#non-persistent-filesystem
[Penyimpanan data-addons]: / Add-on% 20Documentation / Data% 20Storage
[Mysqls]: /Add-on%20Documentation/Data%20Storage/MySQLs.md
[Contoh-aplikasi]: https://github.com/cloudControl/python-django-example-app
[Django-catatan]: /Guides/Python/Django%20notes.md
[Get-conf]: /Guides/Python/Add-on%20credentials.md
[Django tutorial]: https://docs.djangoproject.com/en/1.4/intro/tutorial01/
[Python-panduan]: / Guides / Python
[Python buildpack]: https://github.com/cloudControl/buildpack-python
[Pip]: http://www.pip-installer.org/
[Gunicorn]: http://gunicorn.org/
[Pekerja]: /Platform%20Documentation.md/#scheduled-jobs-and-background-workers
[Db-commit]: https://github.com/cloudControl/python-django-example-app/commit/983f45e46ce0707476cec167ea062e19adcb53c9
[Ssh-sesi]: /Platform%20Documentation.md/#secure-shell-ssh
[Mysql-driver]: https://pypi.python.org/pypi/MySQL-python/1.2.4
