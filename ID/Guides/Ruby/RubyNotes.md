# Ruby Catatan


## Procfile

Platform CloudKilat menggunakan file bernama `Procfile` untuk menentukan bagaimana menjalankan Anda
aplikasi. Ini `Procfile` menggunakan format YAML untuk menentukan diinginkan
konfigurasi.

Perintah yang ditentukan di bawah `masuk web` akan digunakan untuk memulai web
Server.

Jika Anda memiliki `Procfile` dengan konten berikut:
~~~
web: ruby ​​my_server.rb
~~~
yang `ruby my_server.rb` akan dieksekusi pada penyebaran web baru
kontainer.

Misalnya `Procfiles` yang dapat digunakan untuk memulai aplikasi Rails dapat ditemukan
[Nanti dalam dokumen ini] [rel-procfile].

Untuk konteks yang lebih, kunjungi [Landasan dokumentasi] [procfile].


## Versi Ruby

Versi Ruby dapat ditentukan dalam Gemfile. Jika tidak ada versi yang ditentukan,
yang standar akan digunakan. Saat ini, versi standar adalah 2.0.0.

Untuk menentukan versi, menempatkan `direktif ruby` sebagai baris pertama di
`Gemfile`, mis .:
~~~
ruby "1.9.3"
~~~

Pada dorongan berikutnya, versi Ruby yang diinginkan akan digunakan.

Untuk melihat semua versi yang didukung, periksa [Ruby buildpack] [ruby-buildpack]
dokumentasi.


# Rails Catatan


## Rails Procfile

Untuk menjalankan server Rails, membuat file bernama `Procfile` dengan konten berikut:

~~~
web: exec rel bundel s p $ PORT
~~~


## Aset Pipeline

Jika pipa aset yang digunakan, `config / file yang application.rb` harus berisi baris berikut:

~~~ Ruby
config.assets.initialize_on_precompile = false jika ENV ['BUILDPACK_RUNNING']
~~~

Menonaktifkan intialization pada precompile hanya selama proses membangun (sementara di buildpack), tetapi tidak mempengaruhi eksekusi kode normal, misalnya menjalankan server web atau perintah dijalankan.


## Database

Untuk menggunakan database dalam aplikasi Rails, yang `config / file database.yml` perlu dimodifikasi. Kredensial dan informasi database terkait lainnya harus tertanam dalam potongan ERB. Namun, ekstensi file harus tetap `.yml`.

Berikut adalah contoh dari file `database.yml` yang akan digunakan dengan lingkungan produksi database MySQL.

~~~ Erb
pengembangan:
  adaptor: sqlite3
  Database: db / development.sqlite3
  pool: 5
  batas waktu: 5000

Tes:
  adaptor: sqlite3
  Database: db / test.sqlite3
  pool: 5
  batas waktu: 5000

Produksi:
  adaptor: mysql2
  encoding: utf8
  pool: 5
  Database: <% = "'# {ENV [' MYSQLS_DATABASE ']}'"%>
  host: <% = "'# {ENV [' MYSQLS_HOSTNAME ']}'"%>
  port: <% = ENV ["MYSQLS_PORT"]%>
  username: <% = "'# {ENV [' MYSQLS_USERNAME ']}'"%>
  password: <% = "'# {ENV [' MYSQLS_PASSWORD ']}'"%>
~~~

CATATAN: Sebagai YAML karakter markup dapat digunakan dalam password, string dalam potongan ruby ​​tertanam diapit tanda kutip tunggal. Karena port diperlukan untuk menjadi integer, itu tidak tertutup dalam tanda kutip di sini.

Atau Anda dapat menggunakan [cloudcontrol-rel] permata.


Lingkungan ##

Rails server dapat dijalankan dalam lingkungan yang berbeda. Produksi adalah default, tetapi Anda dapat mengubahnya dengan menetapkan `` RAILS_ENV` dan lingkungan RAKE_ENV` variabel dengan [Custom Config addon] (https://community.CloudKilat.ch/tutorial/custom-config-add-on/ ). Sebagai contoh:

~~~
ironapp APP_NAME / DEP_NAME config.add RACK_ENV = some_env RAILS_ENV = some_env
~~~

CATATAN: Gems dalam pengembangan dan pengujian lingkungan dikecualikan dari bundel proses instalasi.



[Cloudcontrol-rel]: https://rubygems.org/gems/cloudcontrol-rails
[Procfile]: /Platform%20Documentation.md/#version-control-images
[Rel-procfile]: # rel-procfile
[Ruby-buildpack]: https://github.com/cloudControl/buildpack-ruby
