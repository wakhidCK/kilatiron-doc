# Aplikasi Sinatra
[Sinatra] [sinatra] adalah DSL untuk membuat aplikasi web di Ruby dengan cepat dan sedikit usaha.

Dalam tutorial ini kita akan menunjukkan kepada Anda bagaimana cara menggunakan aplikasi Sinatra di
[KilatIron]. Anda dapat menemukan [kode sumber di Github] [contoh-aplikasi] dan memeriksa [Ruby buildpack]
untuk mengetahui fitur-fitur yang didukung.


## Aplikasi Sinatra

### Dapatkan Aplikasi
Pertama, mari kita mengkloning aplikasi Sinatra dari repositori kami pada Github:
~~~bash
$ git clone https://github.com/cloudControl/ruby-sinatra-example-app.git
$ cd ruby-sinatra-example-app
~~~

Sekarang Anda memiliki aplikasi Sinatra sederhana.

### Melacak Ketergantungan
Sinatra melacak ketergantungan melalui [Bundler]. Persyaratan dibaca dari `Gemfile` (dan `Gemfile.lock`)
di direktori root repositori Anda.  Aplikasi sederhana kami hanya tergantung pada Sinatra:

~~~ruby
source 'https://rubygems.org'
gem 'sinatra'
~~~

Perhatikan bahwa ada juga `Gemfile.lock`. Ketika Anda mengubah ketergantungan,
Anda harus menjalankan `bundle install` perintah untuk memperbarui` Gemfile.lock`. File ini harus dalam repositori Anda
dan memastikan bahwa semua pengembang selalu menggunakan versi yang sama dari semua gem.

### Proses Type Definition

KilatIron menggunakan [Procfile] untuk mengetahui bagaimana memulai proses Anda.

Contoh kode sudah termasuk sebuah file yang bernama `Procfile` di tingkat atas repositori Anda. File tersebut terlihat seperti ini:
~~~
web: bundle exec ruby server.rb -e production -p $PORT
~~~

Kolom paling kiri **diperlukan** untuk mengetahui jenis proses, pada contoh ini dinamai `web` diikuti dengan perintah
untuk menjalankan aplikasi dan mendengarkan pada port yang ditentukan oleh environment variable `$PORT`.

## Push dan Deploy Aplikasi

Pilih nama yang unik untuk menggantikan `APP_NAME` untuk aplikasi Anda dan membuatnya pada platform KilatIron:

~~~bash
$ ironapp APP_NAME create ruby
~~~

Push kode Anda ke repositori aplikasi, yang memicu proses pembuatan image container:

~~~bash
$ ironapp APP_NAME/default push
Counting objects: 14, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (10/10), done.
Writing objects: 100% (14/14), 258.14 KiB, done.
Total 14 (delta 0), reused 14 (delta 0)

-----> Receiving push
-----> Using Ruby version: ruby-2.0.0
-----> Installing dependencies using Bundler version 1.3.2
       Running: bundle install --without development:test --path vendor/bundle --binstubs vendor/bundle/bin --deployment
       Fetching gem metadata from https://rubygems.org/.........
       Fetching gem metadata from https://rubygems.org/..
       Installing rack (1.5.2)
       Installing rack-protection (1.5.0)
       Installing tilt (1.4.1)
       Installing sinatra (1.4.3)
       Using bundler (1.3.2)
       Your bundle is complete! It was installed into ./vendor/bundle
       Cleaning up the bundler cache.
-----> WARNINGS:
       You have not declared a Ruby version in your Gemfile.
       To set your Ruby version add this line to your Gemfile:
       ruby '2.0.0'
-----> Building image
-----> Uploading image (31M)

To ssh://APP_NAME@kilatiron.net/repository.git
 * [new branch]      master -> master
~~~

Yang terakhir harus dilakukan adalah menyebarkan versi terbaru dari aplikasi dengan perintah ironapp deploy:

~~~bash
$ ironapp APP_NAME/default deploy
~~~

Selamat, Anda sekarang dapat melihat aplikasi Sinatra Anda berjalan di `http[s]://APP_NAME.kilatiron.net`.

[Sinatra]: http://www.sinatrarb.com/
[KilatIron]: http://www.cloudkilat.com/
[CloudKilat-doc-user]: /Platform%20Documentation.md/#user-accounts
[CloudKilat-doc-cmdline]: /Platform%20Documentation.md/#command-line-client-web-console-and-api
[Ruby buildpack]: https://github.com/cloudControl/buildpack-ruby
[Procfile]: /Platform%20Documentation.md/#buildpacks-and-the-procfile
[Git]: https://help.github.com/articles/set-up-git
[Bundler]: http://gembundler.com/
[Contoh-aplikasi]: https://github.com/cloudControl/ruby-sinatra-example-app
