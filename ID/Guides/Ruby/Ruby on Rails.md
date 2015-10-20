# Menyebarkan Ruby on Rails Aplikasi

Dalam tutorial ini kita akan menunjukkan cara untuk menyebarkan [Ruby on Rails] aplikasi di [CloudKilat]. Anda dapat menemukan [kode sumber di Github] [contoh-aplikasi] dan memeriksa [Ruby buildpack] [ruby buildpack] fitur untuk didukung. Aplikasi ini garpu dari Michael Hartl ini [Rails tutorial] sampel aplikasi yang merupakan tiruan Twitter.

## The Rails Aplikasi Dijelaskan

### Dapatkan App

Pertama, mengkloning aplikasi Rails dari repositori kami pada Github:

~~~ Pesta
$ Git clone https://github.com/cloudControl/ruby-rails-example-app.git
$ Cd ruby-rel-contoh-aplikasi; git checkout mysql;
~~~

### Ketergantungan Tracking

Ruby buildpack trek ketergantungan dengan [Bundler]. Mereka didefinisikan dalam `Gemfile` yang ditempatkan di direktori root dari repositori Anda. Dalam contoh ini app berisi:

~~~ Ruby
source 'http://rubygems.org'

permata 'rel', '3.1.10'
permata 'gravatar_image_tag', '1.0.0.pre2'
permata 'will_paginate', '3.0.4'

Kelompok: pembangunan yang
  permata 'membubuhi keterangan', '2.4.0'
  permata 'faker', '0.3.1'
akhir

Kelompok: tes dilakukan
  permata 'webrat', '0.7.1'
  permata 'spork', '0.9.0.rc8'
  permata 'factory_girl_rails', '1.0'
akhir

Kelompok: pengembangan,: tes dilakukan
  permata 'rspec-rel', '2.6.1'
  permata 'sqlite3', '1.3.4'
akhir

Kelompok: produksi dilakukan
  permata 'mysql2'
  permata 'cloudcontrol-rel', '0.0.5'
akhir

Kelompok: aset lakukan
  permata 'sass-rel'
  permata 'kopi-rel'
  permata 'uglifier'
akhir

permata 'jquery-rel'
~~~

### Pengujian

Aplikasi ini memiliki set lengkap tes. Periksa semua tes melewati lokal:

~~~ Pesta
$ Bundel instalasi
$ Bundel exec rake db: migrate
$ Bundel exec rake db: test: mempersiapkan
$ Bundel exec rspec spek /
~~~

Sekarang bahwa aplikasi ini bekerja, mari kita lihat perubahan yang telah kita buat untuk menyebarkan pada CloudKilat.

### Proses Type Definition

CloudKilat menggunakan [Procfile] tahu bagaimana untuk memulai proses Anda. Contoh kode sudah termasuk file bernama Procfile di root repositori Anda. Ini terlihat seperti ini:

~~~
web: exec rel bundel s p $ PORT
~~~

Kiri dari usus kita ditentukan jenis proses yang diperlukan disebut `web` diikuti dengan perintah yang dimulai aplikasi dan mendengarkan pada port yang ditentukan oleh variabel lingkungan` $ PORT`.

### Konfigurasi Asset Pipeline

Kami telah menambahkan kode ke `Application` kelas didefinisikan dalam` config / application.rb` berikut:

~~~ Ruby
modul SampleApp
  Aplikasi kelas <Rails :: Aplikasi

    ...

    # Jangan menginisialisasi pada precompile dalam proses membangun
    # Karena ini bisa gagal, misalnya jika database sedang diakses dalam proses
    # Dan tidak ada manfaat dalam melakukannya dalam proses membangun pula
    config.assets.initialize_on_precompile = false jika ENV ['BUILDPACK_RUNNING']

  akhir
akhir
~~~

### Database Produksi

Secara default, Rails 3 menggunakan SQLite untuk semua lingkungan. Namun, tidak dianjurkan untuk menggunakan SQLite pada CloudKilat karena filesystem ini [tidak terus-menerus] [filesystem].

Dalam tutorial ini kita menggunakan MySQL dengan [MySQL Bersama Add-on]. Ini sebabnya kami telah memodifikasi `Gemfile` dengan memindahkan` garis sqlite3` ke ": pengembangan,: test" blok dan menambahkan baru ": produksi" kelompok dengan "mysql2" dan ["cloudcontrol-rel"] [permata itu sendiri ] permata.

Selain itu kami telah mengubah "produksi" dari `config / database.yml` menggunakan adaptor mysql:
~~~
Produksi:
  adaptor: mysql2
  encoding: utf8
  pool: 5
~~~
Permata 'cloudcontrol-rel' akan memberikan mandat basis data.


## Mendorong dan Menyebarkan App Anda

Pilih nama yang unik untuk menggantikan `APP_NAME` tempat untuk aplikasi Anda dan membuatnya pada platform CloudKilat:

~~~ Pesta
$ Ironcliapp APP_NAME membuat ruby
~~~

Mendorong kode Anda ke repositori aplikasi, yang memicu membangun proses gambar penyebaran (melakukannya dengan `nama penyebaran mysql` karena kita menggunakan cabang yang sama dalam aplikasi repo):

~~~ Pesta
$ Ironcliapp APP_NAME / dorongan mysql
Menghitung benda: 62, dilakukan.
Delta kompresi menggunakan sampai 4 benang.
Mengompresi objek: 100% (51/51), dilakukan.
Menulis objek: 100% (62/62), 15.14 KiB, dilakukan.
Total 62 (delta 2), kembali 0 (delta 0)

-----> Mendorong Menerima
-----> Menggunakan versi Ruby: ruby-2.0.0
-----> Dependensi Instalasi menggunakan Bundler versi 1.3.2
       Menjalankan: bundel instalasi --without pengembangan: tes --path vendor / bundel --binstubs vendor / bundel / bin --deployment
       Metadata permata Mengambil dari https: //rubygems.org / ..........
       Metadata permata Mengambil dari https://rubygems.org/ ..
       Instalasi rake (10.0.3)
       ...
       Instalasi rel (3.1.10)
       ...
       Instalasi uglifier (1.3.0)

       Bundel Anda selesai! Itu diinstal ke ./vendor/bundle
       Post-install pesan dari rdoc:
       Tergantung pada versi ruby, Anda mungkin perlu menginstal ruby ​​rdoc / data ri:
       <= 1.8.6: tidak didukung
       = 1.8.7: gem install rdoc-data; rdoc-data yang --install
       = 1.9.1: gem install rdoc-data; rdoc-data yang --install
       > = 1.9.2: tidak ada hubungannya! Hore!

       Membersihkan cache bundler.
-----> Mempersiapkan aplikasi untuk pipa aset Rails
       Menjalankan: aset menyapu: precompile
       Precompilation aset selesai (6.34s)
       Membersihkan aset
-----> PERINGATAN:
       Anda belum menyatakan versi Ruby di Gemfile Anda.
       Untuk mengatur versi Ruby Anda tambahkan baris ini ke Gemfile Anda:
       ruby '2.0.0'
-----> Gambar Building
-----> Gambar Mengunggah (34m)

Untuk ssh: //APP_NAME@kilatiron.net/repository.git
 * [Cabang baru] mysql -> mysql
~~~

Tambahkan MySQLs Add-on dengan `rencana free` penyebaran dan menyebarkan:

~~~ Pesta
$ Ironcliapp APP_NAME / mysql addon.add mysqls.free
$ Ironcliapp APP_NAME / mysql menyebarkan
~~~

Akhirnya, menyiapkan database dengan menjalankan migrasi menggunakan [perintah Run] [menjalankan perintah]:

~~~ Pesta
$ Ironcliapp APP_NAME / mysql run "rake db: bermigrasi"
~~~

Selamat, Anda sekarang dapat mengakses aplikasi di http://mysql-APP_NAME.kilatiron.net.

Untuk informasi tambahan lihat [Ruby on Rails catatan] [rel-catatan] dan
lainnya [-ruby spesifik dokumen] [ruby-panduan].

[Ruby on Rails]: http://rubyonrails.org/
[CloudKilat]: http://www.cloudkilat.com/
[Contoh-aplikasi]: https://github.com/cloudControl/ruby-rails-example-app
[Ruby buildpack]: https://github.com/cloudControl/buildpack-ruby
[Rails tutorial]: http://ruby.railstutorial.org/
[Bundler]: http://bundler.io/
[Procfile]: /Platform%20Documentation.md/#buildpacks-and-the-procfile
[Filesystem]: /Platform%20Documentation.md/#non-persistent-filesystem
[Menjalankan perintah]: /Guides/Ruby/RunCommand.md
[Rel-catatan]: /Guides/Ruby/RubyNotes.md
[Ruby-panduan]: / Guides / Ruby
[Permata itu sendiri]: http://rubygems.org/gems/cloudcontrol-rails
[MySQL Bersama Add-on]: /Add-on%20Documentation/Data%20Storage/MySQLs.md
