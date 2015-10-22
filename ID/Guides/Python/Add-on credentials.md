# Mendapatkan Kredensial Add-on

Setiap deployment mendapatkan kredensial yang berbeda untuk masing-masing Add-on. Penyedia bisa
mengubah kredensial ini setiap saat, sehingga tidak harus di-hard-code dalam
source code. Jika kredensial tidak dalam source code, mereka juga tidak akan
muncul di kontrol versi dan menimbulkan potensi masalah keamanan.

Ada dua cara untuk mendapatkan [Kredensial Add-on] dalam aplikasi Python.

## Membaca Kredensial dari Environment

Secara default, semua Add-on kredensial dapat ditemukan di sebuah disediakan JSON berkas,
jalan mana yang terkena di `variabel lingkungan CRED_FILE`. Anda dapat melihat
format file lokal dengan perintah:
~~~bash
$ ironapp addon.creds APP_NAME/DEP_NAME
~~~

Anda dapat menggunakan kode berikut untuk mendapatkan kredensial dalam aplikasi Python Anda:
~~~python
import os
import json

try:
  cred_file = open(os.environ['CRED_FILE'])
  creds = json.load(cred_file)

  config = {
    'var1_name': creds['ADDON_NAME']['ADDON_NAME_PARAMETER1'],
    'var2_name': creds['ADDON_NAME']['ADDON_NAME_PARAMETER2'],
    'var3_name': creds['ADDON_NAME']['ADDON_NAME_PARAMETER3']
    # e.g. for MYSQLS: 'hostname': creds['MYSQLS']['MYSQLS_HOSTNAME']
  }
except IOError:
  print 'Could not open the creds.json file'
~~~

Beberapa contoh untuk database Add-ons dapat dilihat di bagian terakhir.

## Membaca Kredensial dari Environtment Variable

Konfigurasi default untuk Python adalah tidak mengekspos kredensial Add-on di environment
variable. Untuk menimpa konfigurasi default menggunakan perintah berikut:
~~~bash
$ ironapp APP_NAME/DEP_NAME config.add SET_ENV_VARS
~~~

Anda dapat mencari nama-nama variabel di masing-masing dokumentasi Add-on. Untuk membacanya,
cukup mengakses dictionary `os.environ`.

Perhatikan bahwa ada beberapa [environment variable] menarik lainnya
yang tersedia dalam deployment Anda.

# Contoh

KilatIron menawarkan sejumlah solusi penyimpanan data melalui [Add-on Marketplace].
Di bawah ini Anda dapat melihat bagaimana mengakses kredensial Add-on untuk MySQL.

## MySQL

Untuk menambahkan database MySQL, gunakan [MySQL Shared Add-on].

Berikut adalah potongan kode Python yang membaca pengaturan database dari file
kredensial dan menyimpannya di dictionary `db_config`:
~~~python
import os
import json

cred_file = open(os.environ['CRED_FILE'])
creds = json.load(cred_file)

db_config = {
    'database': creds['MYSQLD']['MYSQLD_DATABASE'],
    'host': creds['MYSQLD']['MYSQLD_HOST'],
    'port': creds['MYSQLD']['MYSQLD_PORT'],
    'username': creds['MYSQLD']['MYSQLD_USER'],
    'password': creds['MYSQLD']['MYSQLD_PASSWORD']
}
~~~

Anda dapat merujuk pada perintah `addon.creds` untuk melihat nama-nama variabel yang sebenarnya dan nilai-nilainya.

[Env-vars]: /Platform%20Documentation.md/#environment-variables
[Add-on kredensial]: /Platform%20Documentation.md/#add-on-credentials
[Add-on Marketplace]: http://www.cloudkilat.com/
[Custom Config Add-on]: /Add-on%20Documentation/Deployment/Custom%20Config.md
[MySQL Bersama Add-on]: /Add-on%20Documentation/Data%20Storage/MySQLs.md
