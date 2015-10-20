# Menyebarkan Aplikasi Sinatra
[Sinatra] [sinatra] adalah DSL untuk cepat membuat aplikasi web di Ruby dengan sedikit usaha.

Dalam tutorial ini kita akan menunjukkan kepada Anda bagaimana untuk menggunakan aplikasi Sinatra di
[CloudKilat]. Anda dapat menemukan [kode sumber di Github] [contoh-aplikasi] dan memeriksa [Ruby buildpack] fitur untuk didukung.


## The Sinatra App Dijelaskan

### Dapatkan App
Pertama, mari kita mengkloning Sinatra App dari repositori kami pada Github:
~~~ Pesta
$ Git clone https://github.com/cloudControl/ruby-sinatra-example-app.git
$ Cd ruby-sinatra-contoh-aplikasi
~~~

Sekarang Anda memiliki aplikasi Sinatra kecil tapi berfungsi penuh.

### Ketergantungan Tracking
Sinatra melacak dependensi melalui [Bundler]. Persyaratan dibaca dari `Gemfile` (dan` Gemfile.lock`) di direktori root proyek. Aplikasi sederhana kami hanya tergantung pada Sinatra:
~~~ Ruby
source 'https://rubygems.org'
permata 'sinatra'
~~~

Perhatikan bahwa ada juga `Gemfile.lock`. Ketika Anda mengubah ketergantungan,
Anda harus menjalankan `bundel install` perintah untuk memperbarui` Gemfile.lock`. File ini harus dalam repositori Anda dan memastikan bahwa semua pengembang selalu
menggunakan versi yang sama dari semua permata.

### Proses Type Definition

CloudKilat menggunakan [Procfile] tahu bagaimana untuk memulai proses Anda.

Contoh kode sudah termasuk sebuah file yang bernama `Procfile` di tingkat atas repositori Anda. Ini terlihat seperti ini:
~~~
web: bundel exec ruby ​​server.rb -e p produksi $ PORT
~~~

Kiri dari usus besar kita ditentukan ** diperlukan ** Jenis proses yang disebut `web` diikuti dengan perintah yang dimulai aplikasi dan mendengarkan pada port yang ditentukan oleh variabel lingkungan` $ PORT`.

## Mendorong dan Menyebarkan App
Pilih nama yang unik untuk menggantikan `APP_NAME` tempat untuk aplikasi Anda dan membuatnya pada platform CloudKilat:
~~~ Pesta
$ Ironcliapp APP_NAME membuat ruby
~~~

Mendorong kode Anda ke repositori aplikasi, yang memicu penyebaran gambar proses build:
~~~ Pesta
$ Ironcliapp APP_NAME / dorongan bawaan
Menghitung benda: 14, dilakukan.
Delta kompresi menggunakan sampai 4 benang.
Mengompresi objek: 100% (10/10), dilakukan.
Menulis objek: 100% (14/14), 258,14 KiB, dilakukan.
Total 14 (delta 0), kembali 14 (delta 0)
       
-----> Mendorong Menerima
-----> Menggunakan versi Ruby: ruby-2.0.0
-----> Dependensi Instalasi menggunakan Bundler versi 1.3.2
       Menjalankan: bundel instalasi --without pengembangan: tes --path vendor / bundel --binstubs vendor / bundel / bin --deployment
       Metadata permata Mengambil dari https: //rubygems.org / .........
       Metadata permata Mengambil dari https://rubygems.org/ ..
       Instalasi rak (1.5.2)
       Instalasi rak-perlindungan (1.5.0)
       Instalasi tilt (1.4.1)
       Instalasi sinatra (1.4.3)
       Menggunakan bundler (1.3.2)
       Bundel Anda selesai! Itu diinstal ke ./vendor/bundle
       Membersihkan cache bundler.
-----> PERINGATAN:
       Anda belum menyatakan versi Ruby di Gemfile Anda.
       Untuk mengatur versi Ruby Anda tambahkan baris ini ke Gemfile Anda:
       ruby '2.0.0'
-----> Gambar Building
-----> Gambar Mengunggah (31M)

Untuk ssh: //APP_NAME@kilatiron.net/repository.git
 * [Cabang baru] Master -> Master
~~~

Terakhir namun tidak sedikit menyebarkan versi terbaru dari aplikasi dengan ironapp yang menyebarkan perintah:
~~~ Pesta
$ Ironcliapp APP_NAME / default menyebarkan
~~~

Selamat, Anda sekarang dapat melihat Anda berjalan Sinatra App di `http [s]: // APP_NAME.kilatiron.net`.


[Sinatra]: http://www.sinatrarb.com/
[CloudKilat]: http://www.cloudkilat.com/
[CloudKilat-doc-user]: /Platform%20Documentation.md/#user-accounts
[CloudKilat-doc-cmdline]: /Platform%20Documentation.md/#command-line-client-web-console-and-api
[Ruby buildpack]: https://github.com/cloudControl/buildpack-ruby
[Procfile]: /Platform%20Documentation.md/#buildpacks-and-the-procfile
[Git]: https://help.github.com/articles/set-up-git
[Bundler]: http://gembundler.com/
[Contoh-aplikasi]: https://github.com/cloudControl/ruby-sinatra-example-app
