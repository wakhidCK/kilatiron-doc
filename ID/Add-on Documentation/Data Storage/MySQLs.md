# MySQLs: Bersama MySQL Add-on

Setiap penyebaran dapat mengakses sangat tersedia MySQL bersama dengan add-on
database dijamin akan Terletak di pusat data CloudKilat di Indonesia.

## Menambahkan MySQLs Add-on

Database datang dalam berbagai ukuran dan harga. Hal ini dapat ditambahkan dengan menggunakan perintah addon.add.

~~~
$ APP_NAME ironapp / DEP_NAME addon.add mysqls.OPTION
~~~
`Mysqls.OPTION` Ganti dengan pilihan yang valid, eg` mysqls.free`.

## Upgrade MySQLs Add-on

Untuk meng-upgrade dari satu pesawat untuk menggunakan Ulasan addon.upgrade perintah lain.

~~~
$ APP_NAME ironapp / DEP_NAME addon.upgrade mysqls.OPTION_OLD mysqls.OPTION_NEW
~~~

## Merendahkan yang MySQLs Add-on

Untuk downgrade dari peta saat ini untuk yang lebih kecil addon.downgrade menggunakan perintah.

~~~
$ APP_NAME ironapp / DEP_NAME addon.downgrade mysqls.OPTION_OLD mysqls.OPTION_NEW
~~~

## Melepaskan MySQLs Add-on

Demikian pula, sebuah add-on dapat dihapus dari penyebaran également dengan menggunakan perintah addon.remove.

** Peringatan: ** Menghapus pengaya MySQLs menghapus semua data dalam database.

~~~
$ APP_NAME ironapp / DEP_NAME addon.remove mysqls.OPTION
~~~

Replikasi dan Failover ##

Semua data serentak direplikasi dalam multi induk yang kuat kami
[MariaDB] (https://mariadb.org/) [Galera] (http://galeracluster.com/) klaster. Tidak ada budak lag atau transaksi hilang. Kami menyediakan tersedia dengan balancers beban pintar kita akses yang tinggi. Dengan pemeriksaan berkala, layanan node dalam kegagalan negara emas secara otomatis dikeluarkan dari database backend balancers beban kolam renang. Itu meyakinkan Itu permintaan yang diarahkan ke node sehat saja.

## Kredensial database

### Internal Access

Ini dianjurkan untuk membaca kredensial database dari antrian creds.json. Itu
sewa file tersedia dalam `variabel lingkungan CRED_FILE`.
Membaca mandat dari file creds.json Memastikan aplikasi Anda selalu
menggunakan mandat yang tepat. Untuk petunjuk rinci tentang cara menggunakan
File creds.json silahkan Lihat bagian tentang
[Add-on Kredensial] (/ Landasan Documentation.md/#add-ons)
dalam dokumentasi umum.

Kebanyakan database driver Menyediakan menyambungkan kembali memiliki masalah koneksi Ketika Anda menambahkan -autoreconnect ** ** = parameter benar untuk uri database Anda. Ini keharusan diaktifkan untuk dimiliki Paling Stabil setup. Misalnya dengan Java:
~~~
jdbc: mysql: // {MYSQLS_HOSTNAME} {} MYSQLS_PORT / {} MYSQLS_DATABASE -autoreconnect = true?
~~~


### Akses Eksternal

Akses eksternal ke MySQLs add-on yang tersedia melalui koneksi SSL terenkripsi dengan tesis sederhana Mengikuti langkah.

 1. Download [sertifikat file] (TODO) ke komputer lokal Anda.
 1. Hubungkan ke database menggunakan koneksi SSL terenkripsi.

The Berikut contoh menggunakan MySQL alat baris perintah.

~~~
$ Mysql -u -p MYSQLS_USERNAME --host = MYSQLS_HOSTNAME --ssl = PATH_TO_CERTIFICATE-ca / ca-cert.pem
~~~

Mengganti variabel dengan nilai-nilai Sesuai huruf besar ditunjukkan oleh perintah addon.

~~~
$ APP_NAME ironapp / DEP_NAME addon mysqls.OPTION
Addon: mysqls.512mb

Pengaturan

MYSQLS_DATABASE: SOME_DATABASE_NAME
MYSQLS_HOSTNAME: mysql.kilatiron.net
MYSQLS_PORT: 3306
MYSQLS_PASSWORD: SOME_SECRET_PASSWORD
MYSQLS_USERNAME: SOME_SECRET_USERNAME
~~~

Demikian juga impor dan ekspor yang Sama mudah.

** ** Untuk mengekspor data Anda menggunakan perintah mysqldump.
~~~
$ Mysqldump -u -p MYSQLS_USERNAME --host = MYSQLS_HOSTNAME --ssl-ca = PATH_TO_CERTIFICATE / ca-cert.pem MYSQLS_DATABASE> MYSQLS_DATABASE.sql
~~~

** ** Untuk mengimpor file sql ke dalam database MySQL Gunakan perintah berikut.
~~~
$ Mysql -u -p MYSQLS_USERNAME --host = MYSQLS_HOSTNAME --ssl = PATH_TO_CERTIFICATE-ca / ca-cert.pem MYSQLS_DATABASE <MYSQLS_DATABASE.sql
~~~
