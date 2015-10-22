# Mendapatkan Kredensial Add-on

Setiap deployment mendapatkan kredensial yang berbeda untuk masing-masing Add-on. Penyedia bisa
mengubah kredensial ini setiap saat, sehingga tidak harus di-hard-code dalam
source code. Jika kredensial tidak dalam source code, mereka juga tidak akan
muncul di kontrol versi dan menimbulkan potensi masalah keamanan.

Ada dua cara untuk mendapatkan [Kredensial Add-on] dalam aplikasi Ruby.

## Membaca Kredensial dari Environment

Konfigurasi default untuk ruby adalah mengekspos kredensial Add-on di environment variable. Anda dapat
mencari nama-nama variabel di masing-masing dokumentasi Add-on. Untuk membacanya,
cukup mengakses hash `ENV` Ruby. Beberapa contoh untuk Add-on Database dapat dilihat di bagian terakhir.

Jika Anda tidak ingin mengekspos kredensial tersebut di environtment, Anda bisa
menonaktifkan mereka dengan menjalankan:
~~~bash
$ ironapp APP_NAME/DEP_NAME config.add SET_ENV_VARS = false
~~~

Kredensial Add-on juga bisa dibaca dari file kredensial, seperti yang dijelaskan di bagian selanjutnya.

Perhatikan bahwa ada beberapa [environment variable] menarik lainnya
yang tersedia dalam deployment Anda.


## Membaca Kredensial dari File Kredensial

Semua [Kredensial Add-on] dapat ditemukan di sebuah JSON file juga, path ke file tersebut
bisa diketahui dengan membaca environment variable `CRED_FILE`. Anda dapat melihat format file dengan perintah:
~~~bash
$ ironapp addon.creds APP_NAME / DEP_NAME
~~~

Anda dapat menggunakan kode berikut di mana pun Anda ingin mendapatkan kredensial dalam aplikasi Ruby Anda:
~~~ruby
require 'json'

begin
  cred_file = File.open(ENV["CRED_FILE"]).read
  creds = JSON.parse(cred_file)["ADDON_NAME"]
  config = {
    :var1_name => creds["ADDON_NAME_PARAMETER1"],
    :var2_name => creds["ADDON_NAME_PARAMETER2"],
    :var3_name => creds["ADDON_NAME_PARAMETER3"]
    # e.g. for MYSQLS: :hostname => creds[MYSQLS_HOSTNAME]
  }
rescue
  puts "Could not open the creds.json file"
end
~~~


# Contoh

KilatIron menawarkan sejumlah solusi penyimpanan data melalui [Add-on Marketplace].
Di bawah ini Anda dapat melihat bagaimana mengakses kredensial untuk Add-on MySQL.

## MySQL

Untuk menambahkan database MySQL, gunakan [MySQL Shared Add-on].

Berikut adalah potongan kode Ruby yang membaca pengaturan database dan menyimpannya dalam hash `db_config`:
~~~ruby
db_config = {
  Database: ENV ["MYSQLS_DATABASE"],
  host: ENV ["MYSQLS_HOST"],
  port: ENV ["MYSQLS_PORT"],
  username: ENV ["MYSQLS_USER"],
  password: ENV ["MYSQLS_PASSWORD"]
}
~~~

Anda dapat merujuk pada perintah `addon.creds` untuk melihat nama-nama variabel yang sebenarnya dan nilai-nilainya.

[Kredensial Add-on]: /Platform%20Documentation.md/#add-on-credentials
[environment variable]: /Platform%20Documentation.md/#environment-variables
[Add-on Marketplace]: http://www.cloudkilat.com/
[MySQL Shared Add-on]: /Add-on%20Documentation/Data%20Storage/MySQLs.md
