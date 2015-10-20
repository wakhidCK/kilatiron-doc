# Alias ​​Add-on

Menambahkan domain untuk penyebaran kustom didukung melalui add-on Alias. Proses ini membutuhkan pengetahuan dasar tentang bagaimana sistem DNS bekerja. Untuk menambahkan domain kustom mengikuti The Mengikuti langkah mudah ** ** Untuk Setiap domain.

## Menambahkan Alias

 1. Dapatkan kode verifikasi.
 Kode verifikasi hanya untuk pemilik app. Untuk mendapatkannya hanya menggunakan perintah alias.

 ~~~
 $ APP_NAME ironapp / default alias APP_NAME.kilatiron.net
 ~~~

 Kode verifikasi adalah kasus sensitif dan termasuk mitra spasi setelah usus besar. Silakan penjamin, itu membuat sintaks yang sama persis dalam teks catatan TXT atau alias tidak akan diverifikasi.

 1. Tambahkan sebagai catatan TXT untuk domain akar Anda.

 Silakan menggunakan antarmuka penyedia DNS Anda untuk menambahkan [TXT record] (http://de.wikipedia.org/wiki/TXT_Resource_Record) ke domain akar Anda. Harap Perhatikan bagaimana catatan TXT diatur untuk `tujuan example.com` digunakan untuk verify` www.example.com`.

 ~~~
 example.com. 3600 IN TXT "CloudControl-verifikasi:
 68b676e063eadb350876ae291e9ae43748d6e51c85ecd3c4cc026c869acc9d2d "
 ~~~

 Karena kita akan menggunakan CNAME untuk titik domain ke kustom `Asalkan .kilatiron.net` subdomain catatan tambahan segala macam akan diabaikan. Ini diperlukan untuk mengatur untuk itu catatan TXT pada domain akar. Memiliki menambahkan manfaat ini, bahwa 'jika Anda dapat verifiy beberapa domain seperti misalnya `dan` www.example.com` secure.example.com` catatan TXT dengan hanya satu set untuk` example.com`.

 1. Tambahkan menunjuk CNAME ke `Diberikan .kilatiron.net` subdomain.

 Selain catatan TXT, maju dan menambahkan CNAME aussi menunjuk ke subdomain .kilatiron.net` aplikasi Anda '. Gunakan perintah alias baris pelanggan perintah untuk menunjukkan satu khusus untuk penyebaran Anda.

 ~~~
 # Untuk penyebaran standar
  $ APP_NAME ironapp / default alias
 Alias
 Nama default diverifikasi
 APP_NAME.kilatiron.net 1 Januari
 # Untuk Setiap penyebaran tambahan
 $ APP_NAME ironapp / alias DEP_NAME
 Alias
 Nama default diverifikasi
 DEP_NAME.APP_NAME.kilatiron.net 1 Januari
 ~~~

 Yang dihasilkan dan CNAME keharusan melihat sesuatu seperti contoh ini.

 ~~~
 www.example.com. 1593 IN CNAME APP_NAME.kilatiron.net.
 ~~~

 1. Tambahkan satu per domain alias untuk penyebaran Anda.

 Selanjutnya tambahkan domain sebagai alias untuk penyebaran Anda menggunakan perintah alias.add.

 ~~~
 $ APP_NAME ironapp / DEP_NAME alias.add www.example.com
 ~~~

 Sekarang Anda keharusan melihat domain Anda dalam daftar penyebaran ini alias.

 ~~~
 $ APP_NAME ironapp / alias DEP_NAME
 Alias
 Nama default diverifikasi
 www.example.com 0 0
 [...]
 ~~~

 Verifikasi memakan waktu setidaknya 30 menit, tergantung pada tujuan Anda TTL konfigurasi DNS.

 1. Tunggu DNS untuk menyebarkan pertukaran.

 Begitu pertukaran-telah disebarkan melalui DNS alias akan diverifikasi dan penyebaran akan mulai menjawab permintaan Untuk domain itu otomatis.

 ~~~
 $ APP_NAME ironapp / alias DEP_NAME
 Alias
 Nama default diverifikasi
 www.example.com 0 1
 [...]
 ~~~

## Menghapus Alias

Untuk menghapus alias, cukup gunakan perintah alias.remove.

~~~
$ APP_NAME ironapp / DEP_NAME alias.remove www.example.com
~~~

## Kasus Khusus: Wildcard Domain

Alias ​​add-on Media Apakah domain wildcard. Sebuah domain wildcard seperti `* Memungkinkan Anda untuk .example.com`-memiliki jawaban untuk subdomain penyebaran sewenang-wenang Anda Like`` something.example.com` somethingelse.example.com` dan tanpa Perlu untuk menambahkan setiap salah satu dari mereka sebagai alias di muka .

Untuk menggunakan fitur ini pertama meng-upgrade alias Anda add-on dari bebas untuk wildcard.

~~~
$ APP_NAME ironapp / DEP_NAME addon.upgrade alias.free alias.wildcard
~~~

Kemudian tambahkan wildcard domain Hakikat sebagai alias.

~~~
$ APP_NAME ironapp / DEP_NAME alias.add * .example.com
~~~

TXT persyaratan rekor aussi Berlaku untuk domain wildcard, jadi silakan ikuti langkah-langkah di atas sesuai.
