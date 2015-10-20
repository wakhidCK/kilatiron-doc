# Mendapatkan Add-on Kredensial yang

Setiap penyebaran mendapat mandat yang berbeda untuk masing-masing Add-on. Penyedia bisa
mengubah mandat ini setiap saat, sehingga mereka tidak harus keras-kode dalam
kode sumber. Jika kredensial tidak dalam kode sumber, mereka juga tidak akan
muncul di kontrol versi dan menyebabkan masalah keamanan potensial.

## Membaca Kredensial dari Kredensial Berkas

Semua [Add-on kredensial] dapat ditemukan dalam JSON file yang disediakan, yang jalan
terkena di `variabel lingkungan CRED_FILE`. Anda dapat melihat format file lokal dengan perintah:
~~~ Pesta
$ Ironcliapp addon.creds APP_NAME / DEP_NAME
~~~

Anda dapat menggunakan kode berikut di mana pun Anda ingin mendapatkan mandat dalam Anda
PHP aplikasi:
~~~ Php
# Membaca kredensial mengajukan
$ Creds_string = file_get_contents ($ _ ENV ['CRED_FILE'], false);
if ($ creds_string == false) {
    die ('FATAL: Tidak dapat membaca berkas kredensial');
}

# File berisi string JSON, decode dan mengembalikan array asosiatif
$ Creds = json_decode ($ creds_string, true);

# Sekarang menggunakan array $ creds untuk mengkonfigurasi aplikasi Anda
$ Var1_name = $ creds ['ADDON_NAME'] ['ADDON_NAME_PARAMETER1'];
$ Var2_name = $ creds ['ADDON_NAME'] ['ADDON_NAME_PARAMETER2'];
$ Var3_name = $ creds ['ADDON_NAME'] ['ADDON_NAME_PARAMETER3'];

# Mis untuk MySQLs $ hostname = $ creds ['MYSQLS'] ['MYSQLS_HOSTNAME'];
~~~

# Contoh

CloudKilat menawarkan sejumlah solusi penyimpanan data melalui [Add-on Marketplace].
Di bawah ini Anda dapat melihat bagaimana mengakses Add-on kredensial untuk MySQL.

## MySQL

Untuk menambahkan database MySQL, gunakan [MySQL Bersama Add-on].

Berikut adalah potongan PHP yang membaca pengaturan database dari mandat berkas:
~~~ Php
$ Creds_string = file_get_contents ($ _ ENV ['CRED_FILE'], false);
if ($ creds_string == false) {
    die ('FATAL: Tidak dapat membaca berkas kredensial');
}
$ Creds = json_decode ($ creds_string, true);
$ Database = $ creds ['MYSQLS'] ['MYSQLS_DATABASE'];
$ Host = $ creds ['MYSQLS'] ['MYSQLS_HOSTNAME'];
$ Port = $ creds ['MYSQLS'] ['MYSQLS_PORT'];
$ Username = $ creds ['MYSQLS'] ['MYSQLS_USERNAME'];
$ Password = $ creds ['MYSQLS'] ['MYSQLS_PASSWORD'];
~~~

Ingat, Anda selalu dapat merujuk pada `perintah addon.creds` untuk melihat nama-nama variabel yang sebenarnya dan nilai-nilai.

[Env-vars]: /Platform%20Documentation.md/#environment-variables
[Add-on kredensial]: /Platform%20Documentation.md/#add-on-credentials
[Add-on Marketplace]: http://www.cloudkilat.com/
[MySQL Bersama Add-on]: /Add-on%20Documentation/Data%20Storage/MySQLs.md
