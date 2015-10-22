# Catatan untuk Django
Dokumen ini berisi informasi untuk programmer Django cara menyebarkan aplikasi mereka di [KilatIron].

## Mengelola Dependensi
The [python buildpack] menggunakan [pip] untuk mengelola dependensi. Tentukan dependensi Anda dalam sebuah file bernama `requirements.txt` di direktori root proyek.

## Menentukan Jenis Proses
KilatIron menggunakan [Procfile] [procfile] untuk mengetahui cara memulai proses Anda. Pada file ini harus terdapat perintah _web_ yang akan dijalankan untuk memulai server setelah aplikasi ini digunakan. Anda juga bisa mendefinisikan [worker] yang dapat digunakan untuk menjalankan tugas-tugas yang lain.

The `Procfile` untuk aplikasi Django menggunakan gunicorn sebagai web server dapat terlihat seperti ini:
~~~
web: python manage.py run_gunicorn --config gunicorn_config.py -b 0.0.0.0:$PORT
manage: python manage.py
~~~

## Melakukan Maintenance
Gunakan perintah `run` untuk melaksanakan tugas-tugas seperti `syncdb`. Ini dimulai sebuah [sesi SSH].
~~~bash
ironapp APP_NAME/DEP_NAME run "python manage.py syncdb"
~~~

## Database
Untuk menggunakan database dapat dilihat di [MySQL Shared Add-on] [MySQL Shared Add-on]. Untuk mendapatkan kredensial dari database Anda, lihat artikel [Kredensial Add-on] [add-on-credentials].

[sesi SSH]: /Platform%20Documentation.md/#secure-shell-ssh
[Python buildpack]: https://github.com/cloudControl/buildpack-python
[Pip]: http://www.pip-installer.org/
[Procfile]: /Platform%20Documentation.md/#buildpacks-and-the-procfile
[MySQL Shared Add-on]: /Add-on%20Documentation/Data%20Storage/MySQLs.md
[Add-on-credentials]: /Guides/Python/Add-on%20credentials.md
[KilatIron]: http://www.cloudkilat.com/
[worker]: /Add-on%20Documentation/Data%20Processing/Worker.md
