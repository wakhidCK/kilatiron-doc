# Mendapatkan Add-on Kredensial yang

Setiap penyebaran mendapat mandat yang berbeda untuk masing-masing Add-on. Penyedia bisa
mengubah mandat ini setiap saat, sehingga mereka tidak harus keras-kode dalam
kode sumber. Jika kredensial tidak dalam kode sumber, mereka juga tidak akan
muncul di kontrol versi dan menyebabkan masalah keamanan potensial.

Ada dua cara untuk mendapatkan [Add-on kredensial] dalam aplikasi Ruby.


## Membaca Kredensial dari Variabel Lingkungan

Secara default, setiap Add-on mengekspos identitasnya di lingkungan. Kamu bisa
mencari lingkungan individu nama variabel di masing Add-on
dokumentasi. Untuk membacanya, cukup mengakses `hash ENV` Ruby. Beberapa contoh untuk
Database Pengaya dapat dilihat di bagian terakhir.

Jika Anda tidak ingin mengekspos identitasnya tersebut di lingkungan, Anda bisa
menonaktifkan mereka dengan menjalankan:
~~~ Pesta
$ Ironcliapp APP_NAME / DEP_NAME config.add SET_ENV_VARS = false
~~~

Add-on kredensial masih bisa dibaca dari mandat mengajukan, seperti yang dijelaskan di bagian selanjutnya.

Perhatikan bahwa ada beberapa [variabel lingkungan] menarik lainnya
tersedia dalam wadah penyebaran Anda.


## Membaca Kredensial dari Kredensial Berkas

Semua [Add-on kredensial] dapat ditemukan di sebuah tersedia JSON file juga, jalan mana yang terkena di
yang `variabel lingkungan CRED_FILE`. Anda dapat melihat format file lokal dengan perintah:
~~~ Pesta
$ Ironcliapp addon.creds APP_NAME / DEP_NAME
~~~

Anda dapat menggunakan kode berikut di mana pun Anda ingin mendapatkan mandat dalam Anda
Ruby app:
~~~ Ruby
membutuhkan 'json'

mulai
  cred_file = File.open (ENV ["CRED_FILE"]). baca
  creds = JSON.parse (cred_file) ["ADDON_NAME"]
  config = {
    : Var1_name => creds ["ADDON_NAME_PARAMETER1"],
    : Var2_name => creds ["ADDON_NAME_PARAMETER2"],
    : Var3_name => creds ["ADDON_NAME_PARAMETER3"]
    # Mis untuk MYSQLS:: hostname => creds [MYSQLS_HOSTNAME]
  }
penyelamatan
  menempatkan "Tidak dapat membuka file creds.json"
akhir
~~~


# Contoh

CloudKilat menawarkan sejumlah solusi penyimpanan data melalui [Add-on Marketplace].
Di bawah ini Anda dapat melihat bagaimana mengakses Add-on kredensial untuk MySQL.

## MySQL

Untuk menambahkan database MySQL, gunakan [MySQL Bersama Add-on].

Berikut adalah potongan Ruby yang membaca pengaturan database dan menyimpannya dalam
`Hash db_config`:
~~~ Ruby
db_config = {
  Database: ENV ["MYSQLS_DATABASE"],
  host: ENV ["MYSQLS_HOST"],
  port: ENV ["MYSQLS_PORT"],
  username: ENV ["MYSQLS_USER"],
  password: ENV ["MYSQLS_PASSWORD"]
}
~~~

Ingat, Anda selalu dapat merujuk pada `perintah addon.creds` untuk melihat nama-nama variabel yang sebenarnya dan nilai-nilai.

[Add-on kredensial]: /Platform%20Documentation.md/#add-on-credentials
[Variabel lingkungan]: /Platform%20Documentation.md/#environment-variables
[Add-on Marketplace]: http://www.cloudkilat.com/
[MySQL Bersama Add-on]: /Add-on%20Documentation/Data%20Storage/MySQLs.md
