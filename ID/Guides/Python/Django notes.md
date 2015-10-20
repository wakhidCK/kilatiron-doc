# Catatan untuk Django Pengembang
Dokumen ini berisi informasi untuk Django programmer menyebarkan aplikasi mereka di [CloudKilat].

## Mengelola Dependensi
The [python buildpack] menggunakan [pip] untuk mengelola dependensi. Tentukan dependensi Anda dalam sebuah file bernama `requirements.txt` di direktori root proyek.

## Menentukan Jenis Proses
CloudKilat menggunakan [Procfile] [procfile] tahu bagaimana untuk memulai proses Anda. File ini menentukan perintah _web_ yang akan dijalankan untuk memulai server setelah aplikasi ini digunakan. Hal opsional juga menetapkan [pekerja] jenis yang dapat digunakan untuk menjalankan tugas-tugas berjalan lama.

The `Procfile` untuk aplikasi Django menggunakan gunicorn sebagai web server dapat terlihat seperti ini:
~~~
web: python manage.py run_gunicorn --config gunicorn_config.py -b 0.0.0.0:$PORT
mengelola: python manage.py
~~~

## Pelaksana Tugas Manajemen
Gunakan perintah `run` untuk melaksanakan tugas-tugas seperti` syncdb`. Ini dimulai sebuah [SSH-sesi] interaktif.
~~~ Pesta
ironapp APP_NAME / DEP_NAME run "python manage.py syncdb"
~~~

Database ##
Untuk menggunakan database, kita lihat di [Bersama MySQL Add-on] [Bersama MySQL Add-on]. Untuk mendapatkan mandat dari database Anda, lihat artikel [Add-on kredensial] [add-on-kredensial].

[SSH-sesi]: /Platform%20Documentation.md/#secure-shell-ssh
[Python buildpack]: https://github.com/cloudControl/buildpack-python
[Pip]: http://www.pip-installer.org/
[Procfile]: /Platform%20Documentation.md/#buildpacks-and-the-procfile
[Bersama MySQL Add-on]: /Add-on%20Documentation/Data%20Storage/MySQLs.md
[Add-on-kredensial]: /Guides/Python/Add-on%20credentials.md
[CloudKilat]: http://www.cloudkilat.com/
[Pekerja]: /Add-on%20Documentation/Data%20Processing/Worker.md
