# Mendapatkan Add-on Kredensial yang

Setiap penyebaran mendapat mandat yang berbeda untuk masing-masing Add-on. Penyedia dapat mengubah mandat ini setiap saat, sehingga mereka tidak harus keras-kode dalam kode sumber. Jika kredensial tidak dalam kode sumber, mereka juga tidak akan muncul di kontrol versi dan menyebabkan masalah keamanan potensial.

Ada dua cara untuk mendapatkan [Add-on kredensial] dalam aplikasi Node.js.

## Membaca Kredensial dari Variabel Lingkungan

Secara default, setiap Add-on mengekspos identitasnya di lingkungan. Anda dapat melihat lingkungan individu nama variabel di masing Add-on dokumentasi. Untuk menggunakan variabel lingkungan tertentu, Anda dapat merujuk ke menggunakan `process.env.ENVIRONMENT_VARIABLE_NAME` dalam kode Anda. Beberapa contoh untuk database Add-ons dapat dilihat di bagian terakhir.

Jika Anda tidak ingin mengekspos identitasnya tersebut di lingkungan, Anda dapat menonaktifkan mereka dengan menjalankan:

~~~ Pesta
$ Ironcliapp APP_NAME / DEP_NAME config.add SET_ENV_VARS = false
~~~

Add-on kredensial masih bisa dibaca dari mandat mengajukan, seperti yang dijelaskan di bagian selanjutnya.

Perhatikan bahwa ada beberapa [variabel lingkungan] menarik lain yang tersedia dalam wadah penyebaran Anda, seperti jalan ke kredensial berkas.

## Membaca Kredensial dari Kredensial Berkas

Semua [Add-on kredensial] dapat ditemukan di sebuah tersedia JSON file juga, jalan mana yang terkena di
yang `variabel lingkungan CRED_FILE`. Anda dapat melihat format file lokal dengan perintah:

~~~ Pesta
$ Ironcliapp addon.creds APP_NAME / DEP_NAME
~~~

Anda dapat menggunakan kode berikut di mana pun Anda ingin mendapatkan mandat dalam aplikasi Node.js Anda:

~~~ Javascript
fs var = require ('fs');

creds var = JSON.parse (
    fs.readFileSync (process.env.CRED_FILE)
);

var param1 = creds.ADDON_NAME.ADDON_NAME_PARAMETER1;
var param2 = creds.ADDON_NAME.ADDON_NAME_PARAMETER2;
var param3 = creds.ADDON_NAME.ADDON_NAME_PARAMETER3;
// Misal untuk MYSQLS: var hostname = creds.MYSQLS.MYSQLS_HOSTNAME
~~~

# Contoh

CloudKilat menawarkan sejumlah solusi penyimpanan data melalui [Add-on Marketplace]. Di bawah ini Anda dapat melihat bagaimana mengakses Add-on kredensial untuk MySQL.

## MySQL
Untuk menambahkan database MySQL, gunakan [MySQL Bersama Add-on].

Berikut adalah potongan Node.js yang membaca pengaturan database dari mandat berkas:

~~~ Javascript
fs var = require ('fs');

creds var = JSON.parse (
    fs.readFileSync (process.env.CRED_FILE)
);

var host = creds.MYSQLS.MYSQLS_HOSTNAME;
Database var = creds.MYSQLS.MYSQLS_DATABASE;
var user = creds.MYSQLS.MYSQLS_USERNAME;
sandi var = creds.MYSQLS.MYSQLS_PASSWORD;
pelabuhan var = creds.MYSQLS.MYSQLS_PORT;

~~~

Ingat, Anda selalu dapat merujuk pada `perintah addon.creds` untuk melihat nama-nama variabel yang sebenarnya dan nilai-nilai.

[Add-on Marketplace]: http://www.cloudkilat.com/
[Variabel lingkungan]: /Platform%20Documentation.md/#environment-variables
[MySQL Bersama Add-on]: /Add-on%20Documentation/Data%20Storage/MySQLs.md
[Add-on kredensial]: /Platform%20Documentation.md/#add-on-credentials
