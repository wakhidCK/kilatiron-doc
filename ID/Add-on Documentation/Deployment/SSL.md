# SSL Add-on

Ikhtisar:

 * Add-on ini Menyediakan dukungan SSL untuk domain kustom.
 * Anda harus log-memiliki RSA Swasta Key, sertifikat SSL dan Rantai Sertifikat.
 * Tambahkan add-on untuk penyebaran melalui CLI kami dengan [perintah addon.add] (# Menambahkan-the-ssl-add-on).

Secure Socket Layer (SSL) enkripsi yang tersedia untuk Peningkatan Ketika keamanan
Transmisi password dan data sensitif --Lain.

Sebagai bagian dari `Asalkan subdomain .kilatiron.net`, semua penyebaran-memiliki
Akses ke kuda-kudaan SSL menggunakan `* sertifikat wildcard .kilatiron.net`.
Untuk menggunakan ini, hanya Arahkan browser Anda ke:
* `Https: // APP_NAME.kilatiron.net` untuk penyebaran standar
* `Https: // DEP_NAME-APP_NAME.kilatiron.net` untuk penyebaran non-default

    Harap Catatan dasbor ** ** entre DEP_NAME dan APP_NAME.

Dukungan SSL untuk domain kustom tersedia melalui SSL add-on.

## Kustom Domain Sertifikat

Untuk mengaktifkan dukungan SSL untuk domain kustom seperti `emas www.example.com`
`Secure.example.com`, Anda perlu SSL add-on.

Silakan pergi melalui langkah-langkah berikut, qui yang Dijelaskan dalam mendatang
bagian, untuk menambahkan dukungan SSL untuk penyebaran Anda:

 * Memperoleh sertifikat yang ditandatangani dari otoritas sertifikat Anda kepercayaan.
 * Tambahkan SSL add-on yang menyediakan baik sertifikat, kunci pribadi dan
   baris sertifikat-rantai.
 * Atur entri DNS untuk DNS Anda to-point SSL Domain.

Catatan: Mohon tunggu hingga satu jam untuk DNS untuk menyebarkan pertukaran Sebelum Mereka mengambil
efek. Akar atau domain telanjang seperti `example.com` tanpa Arent subdomain
didukung.

### Mendapatkan Sertifikat SSL

Ada berbagai macam Otoritas Sertifikat (CA) qui Berbeda biaya dan
Proses Mendapatkan sertifikat SSL.
[SSLShopper] (http://www.sslshopper.com/certificate-authority-reviews.html)
menawarkan cara mudah untuk membandingkan CA. Bahkan Beberapa menawarkan masa percobaan gratis. Di
Kasus PALING, Anda perlu melakukan langkah-langkah berikut.

Catatan: Untuk tujuan pengujian Anda dapat selalu menggunakan sertifikat qui ditandatangani sendiri
adalah gratis dan Tidak memerlukan akan melalui proses pendaftaran
penyedia individu.

#### Menghasilkan kunci pribadi

Sebagai Disebutkan Sebelumnya, Anda memerlukan kunci pribadi, sertifikat dan
rantai sertifikat untuk mengaktifkan dukungan SSL. Untuk proses itu Anda akan memerlukan
`Qui peut être openssl` toolkit diinstal dengan satu Dari metode berikut
Tergantung pada platform Anda:

| Landasan | metode Instal |
| ------- |: ------------- |
| Mac OS X ([Homebrew] (http://brew.sh/)) | `minuman menginstal openssl` |
| Windows | [menginstal Windows paket] (http://gnuwin32.sourceforge.net/packages/openssl.htm) |
| Ubuntu GNU / Linux | `apt-get install openssl` |


Setelah Anda selesai dengan instalasi, menggunakan baris perintah alat `openssl` untuk
Membangkitkan melanjutkan dengan kunci RSA pribadi Anda:
 ~~~
 $ Openssl genrsa -des3 -out server.key.org 2048
 # Masukkan dan konfirmasi passphrase
 ~~~

#### Menghapus passphrase

Kuncinya dihasilkan dilindungi oleh qui passphrase perlu dihapus sehingga
Yang dapat dimuat oleh server web.
 ~~~
 $ Openssl rsa -in server.key -out server.key.org
 ~~~

Kunci pribadi Anda digunakan untuk proses ini sekarang disimpan dalam file `server.key`

#### Menghasilkan (Request Penandatanganan Sertifikat) CSR

Untuk Mendapatkan sertifikat SSL, Anda perlu Menyediakan CA Anda dengan CSR
(Sertifikat Penandatanganan Permintaan). Ini dapat digunakan untuk aussi Membuat ditandatangani sendiri
sertifikat. CSR berisi semua informasi mengenai perusahaan Anda atau
organisasi, mendorong Anda untuk memasukkan DEMIKIAN Mereka:
 ~~~
 $ Openssl req -new -Key server.key -out server.csr
 Negara Nama (2 huruf kode) [AU]: DE
 Negara atau Nama Provinsi (nama lengkap) [Beberapa-Negara]:
 Nama wilayah (misalnya, kota) []:
 Nama organisasi (misalnya, perusahaan) [Internet Widgits Pty Ltd]:
 Organisasi Nama Unit (misalnya, bagian) []: Teknologi Informasi
 Nama umum (misalnya, nama Anda atau nama host server Anda) []: www.example.com
 Alamat Email []:
 Masukkan Setelah atribut 'ekstra'
 untuk dikirim dengan permintaan sertifikat Anda
 Sandi Tantangan []:
 Sebuah nama perusahaan opsional []:
 ~~~

File yang dibuat bernama Setelah proses `server.csr` ini.

Catatan: Harap memperhatikan ke ladang Negara Nama dan Nama umum. Negara
Nama keharusan contenir kode dua huruf negara Anda selon yang
[ISO 3166-1](http://www.iso.org/iso/country_codes/iso_3166_code_lists/country_names_and_code_elements.htm)
Format. Kedua dan PALING penting adalah Nama Umum. Ini keharusan yang Mencerminkan
tuangkan qui domain Anda ingin mengeluarkan sertifikat. Sebagai Disebutkan Sebelumnya, ini
tidak bisa menjadi domain akar tetapi perlu-memiliki ukuran seperti `www.example.com`.

#### Penerbitan Sertifikat

Setelah memilih CA Anda, Anda-harus melalui proses mereka dari Issuing
sertifikat. Untuk ini, Anda akan memerlukan file CSR, qui Diciptakan di
langkah sebelumnya. Cukup Sering kali Anda akan perlu aussi menentukan server web Anda
akan menggunakan. Dalam hal ini Anda memilih keharusan server web Nginx, dan jika ini
bukanlah pilihan Lalu Apache 2.x keharusan aussi OK.

Pada akhirnya, AC Anda akan Memberikan Anda Dengan Beberapa file Termasuk SSL
sertifikat dan rantai sertifikat. Sertifikat file memiliki keharusan Entah
`` Emas memiliki .crt` ekstensi .pem`. Layanan kami membutuhkan sertifikat berada di
Format PEM, sehingga jika tidak, Anda bisa mengubahnya dengan Perintah Berikut:
 ~~~
 $ Openssl x509 -dalam www_example_com.crt -inform PEM -out www_example_com.pem
 ~~~

Isi dari keharusan berkas sertifikat SSL terlihat seperti ini:
 ~~~
 ----- BEGIN CERTIFICATE -----
 ...
 ----- END SERTIFIKAT -----
 ~~~

Rantai sertifikat rantai kepercayaan qui Membuktikan Itu sertifikat Anda
Yang dikeluarkan oleh penyedia dipercaya resmi oleh CA. Akar Sertifikat root CA
Disimpan dalam semua browser modern dan ini adalah bagaimana browser Anda dapat diandalkan untuk
Itu memverifikasi sebuah situs web adalah aman. Dalam Setiap kotak lain, Anda akan recevoir peringatan
seperti ini:

[Firefox warning](https://s3-eu-west-1.amazonaws.com/cctrl-www-production/custom_assets/attachments/000/000/038/original/ffssl.png)

Anda juga memiliki file keharusan qui est qui bundel sertifikat Sukses Saling:
 ~~~
 ----- BEGIN CERTIFICATE -----
 ...
 ----- END SERTIFIKAT -----
 ----- BEGIN CERTIFICATE -----
 ...
 ----- END SERTIFIKAT -----
 ~~~

Catatan: Jika Anda tidak-memiliki serangkaian tujuan sertifikat bundel `file .crt`, Anda
untuk-memiliki 'em up dalam urutan yang benar mulai dari menengah
sertifikat dan berakhir dengan sertifikat root. Silakan membuat asam Itu Mereka Apakah
dalam format PEM.

### Menambahkan SSL Add-on

Untuk menambahkan SSL add-on, hanya Memberikan jalan ke file yang disediakan oleh
otoritas sertifikat menggunakan parameter masing-masing perintah addon.add.
 ~~~
 $ APP_NAME ironapp / DEP_NAME addon.add ssl.host --cert path / ke / cert_file --key path / ke / key_file [path --chain / ke / CHAIN_FILE]
 ~~~

Untuk memeriksa status add-on, Anda dapat melakukan hal berikut.
 ~~~
 $ APP_NAME ironapp / DEP_NAME addon ssl.host
 Addon: ssl.host

 Pengaturan
   SSLDEV_CERT_EXPIRES: 2016/01/01 10:00:00
   SSLDEV_DNS_DOMAIN: snip.app.exo.com
   SSLDEV_CERT_INCEPTS: 2013/01/01 10:00:00
 ~~~

Jika Anda ingin menukar sertifikat Anda (misalnya Karena itu akan berakhir)
Anda dapat meng-upload sertifikat baru dengan Perintah Berikut:
 ~~~
 $ APP_NAME ironapp / DEP_NAME addon.upgrade ssl.host ssl.host --cert path / ke / NEW_CERT_FILE --key path / ke / key_file [path --chain / ke / CHAIN_FILE]
 ~~~

Catatan: Jika rantai sertifikat baru Memiliki baris yang sama, Anda masih harus lulus lagi.
