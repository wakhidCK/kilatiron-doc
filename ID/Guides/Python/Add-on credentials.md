# Mendapatkan Add-on Kredensial yang

Setiap penyebaran mendapat mandat yang berbeda untuk masing-masing Add-on. Penyedia bisa
mengubah mandat ini setiap saat, sehingga mereka tidak harus keras-kode dalam
kode sumber. Jika kredensial tidak dalam kode sumber, mereka juga tidak akan
muncul di kontrol versi dan menyebabkan masalah keamanan potensial.

Ada dua cara untuk mendapatkan [Add-on kredensial] dalam aplikasi Python.

## Membaca Kredensial dari Kredensial Berkas

Secara default, semua Add-on kredensial dapat ditemukan di sebuah disediakan JSON berkas,
jalan mana yang terkena di `variabel lingkungan CRED_FILE`. Anda dapat melihat
format file lokal dengan perintah:
~~~ Pesta
$ Ironcliapp addon.creds APP_NAME / DEP_NAME
~~~

Anda dapat menggunakan kode berikut di mana pun Anda ingin mendapatkan mandat dalam Anda
Python app:
~~~ Python
os impor
impor json

coba:
  cred_file = open (os.environ ['CRED_FILE'])
  creds = json.load (cred_file)

  config = {
    'Var1_name': creds ['ADDON_NAME'] ['ADDON_NAME_PARAMETER1'],
    'Var2_name': creds ['ADDON_NAME'] ['ADDON_NAME_PARAMETER2'],
    'Var3_name': creds ['ADDON_NAME'] ['ADDON_NAME_PARAMETER3']
    # Mis untuk MYSQLS: 'hostname': creds ['MYSQLS'] ['MYSQLS_HOSTNAME']
  }
kecuali IOError:
  print 'Tidak dapat membuka berkas creds.json'
~~~

Beberapa contoh untuk database Add-ons dapat dilihat di bagian terakhir.

## Membaca Kredensial dari Variabel Lingkungan

Default untuk Python adalah untuk tidak mengekspos Add-on kredensial sebagai lingkungan
variabel. Untuk menimpa default menggunakan perintah berikut:
~~~ Pesta
$ Ironcliapp APP_NAME / DEP_NAME config.add SET_ENV_VARS
~~~

Anda dapat melihat lingkungan individu nama variabel di masing-masing
Add-on dokumentasi. Untuk membacanya, cukup mengakses `os.environ` Python
kamus.

Perhatikan bahwa ada beberapa [variabel lingkungan] menarik lainnya [env-vars]
tersedia dalam wadah penyebaran Anda.

# Contoh

CloudKilat menawarkan sejumlah solusi penyimpanan data melalui [Add-on Marketplace].
Di bawah ini Anda dapat melihat bagaimana mengakses Add-on kredensial untuk MySQL.

## MySQL

Untuk menambahkan database MySQL, gunakan [MySQL Bersama Add-on].

Berikut adalah potongan Python yang membaca pengaturan database dari mandat
File dan menyimpannya di `kamus db_config`:
~~~ Python
os impor
impor json

cred_file = open (os.environ ['CRED_FILE'])
creds = json.load (cred_file)

db_config = {
    'Database': creds ['MYSQLS'] ['MYSQLS_DATABASE'],
    'Host': creds ['MYSQLS'] ['MYSQLS_HOST'],
    'Pelabuhan': creds ['MYSQLS'] ['MYSQLS_PORT'],
    'Username': creds ['MYSQLS'] ['MYSQLS_USER'],
    'Password': creds ['MYSQLS'] ['MYSQLS_PASSWORD']
}
~~~

Ingat, Anda selalu dapat merujuk pada `perintah addon.creds` untuk melihat variabel yang sebenarnya
nama dan nilai-nilai.

[Env-vars]: /Platform%20Documentation.md/#environment-variables
[Add-on kredensial]: /Platform%20Documentation.md/#add-on-credentials
[Add-on Marketplace]: http://www.cloudkilat.com/
[Custom Config Add-on]: /Add-on%20Documentation/Deployment/Custom%20Config.md
[MySQL Bersama Add-on]: /Add-on%20Documentation/Data%20Storage/MySQLs.md
