# Membuat Aplikasi Ruby on Rails

Dalam tutorial ini kita akan menunjukkan cara untuk membuat aplikasi [Ruby on Rails] sederhana di [KilatIron]. Anda dapat menemukan [source code di Github] [contoh-aplikasi] dan memeriksa fitur-fitur [Ruby buildpack] [ruby buildpack] untuk didukung. Aplikasi ini adalah fork dari Michael Hartl [Rails tutorial], sampel aplikasi yang merupakan Twitter clone.

## Aplikasi Rails

### Dapatkan Aplikasi

Pertama, mengkloning aplikasi Rails dari repositori kami pada Github:

~~~bash
$ git clone https://github.com/cloudControl/ruby-rails-example-app.git
$ cd ruby-rails-example-app; git checkout mysql;
~~~

### Melacak Ketergantungan

Ruby buildpack melacak ketergantungan dengan [Bundler]. Mereka didefinisikan dalam `Gemfile` yang ditempatkan di direktori root dari repositori Anda. Dalam contoh ini app berisi:

~~~ruby
source 'http://rubygems.org'

gem 'rails', '3.1.10'
gem 'gravatar_image_tag', '1.0.0.pre2'
gem 'will_paginate', '3.0.4'

group :development do
  gem 'annotate', '2.4.0'
  gem 'faker', '0.3.1'
end

group :test do
  gem 'webrat', '0.7.1'
  gem 'spork', '0.9.0.rc8'
  gem 'factory_girl_rails', '1.0'
end

group :development, :test do
  gem 'rspec-rails', '2.6.1'
  gem 'sqlite3', '1.3.4'
end

group :production do
  gem 'mysql2'
  gem 'cloudcontrol-rails', '0.0.5'
end

group :assets do
  gem 'sass-rails'
  gem 'coffee-rails'
  gem 'uglifier'
end

gem 'jquery-rails'
~~~

### Pengujian

Aplikasi ini memiliki set lengkap tes. Periksa semua tes melewati lokal:

~~~bash
$ bundle install
$ bundle exec rake db:migrate
$ bundle exec rake db:test:prepare
$ bundle exec rspec spec/
~~~

Sekarang, setelah yakin bahwa aplikasi ini berjalan dengan baik, mari kita lihat perubahan yang telah kita buat untuk menyebarkan pada KilatIron.

### Proses Type Definition

KilatIron menggunakan [Procfile] untuk mengetahui bagaimana memulai proses Anda.

Contoh kode sudah termasuk file bernama Procfile di root repositori Anda. Ini terlihat seperti ini:

~~~
web: bundle exec rails -p $PORT
~~~

Kolom paling kiri **diperlukan** untuk mengetahui jenis proses, pada contoh ini dinamai `web` diikuti dengan perintah yang dimulai aplikasi dan mendengarkan pada port yang ditentukan oleh environment variable `$PORT`.

### Konfigurasi Asset Pipeline

Kami telah menambahkan kode ke kelas `Application` didefinisikan dalam `config/application.rb` berikut:

~~~ruby
module SampleApp
  class Application < Rails::Application

    ...

    # Do not initialize on precompile in the build process
    # as this can fail, e.g. if database is being accessed in the process
    # and there is no benefit in doing it in build process anyway
    config.assets.initialize_on_precompile = false if ENV['BUILDPACK_RUNNING']

  end
end
~~~

### Database

Secara default, Rails 3 menggunakan SQLite untuk semua lingkungan. Namun, tidak dianjurkan untuk menggunakan SQLite pada KilatIron karena filesystem ini [persisten] [filesystem].

Dalam tutorial ini kita menggunakan MySQL dengan [shared MySQL Add-on]. Ini sebabnya kami telah memodifikasi `Gemfile` dengan memindahkan` garis sqlite3` ke blok ":development,: test" dan menambahkan grup baru ":production" dengan "mysql2" dan gem ["cloudcontrol-rails"] .

Selain itu kami telah mengubah "production" dari `config/database.yml` menggunakan adaptor mysql:
~~~
production:
  adapter: mysql2
  encoding: utf8
  pool: 5
~~~
Gem 'cloudcontrol-rails' akan memberikan kredensial database.


## Deployment Aplikasi
Pilih nama yang unik untuk menggantikan `APP_NAME` untuk aplikasi Anda dan membuatnya pada platform KilatIron:

~~~bash
$ ironapp APP_NAME create ruby
~~~

Push kode Anda ke repositori aplikasi (gunakan deployment mysql, karena nama deployment itulah yang kita gunakan pada repository contoh aplikasi), yang memicu proses pembuatan image container:

~~~bash
$ ironapp APP_NAME/mysql push
Counting objects: 62, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (51/51), done.
Writing objects: 100% (62/62), 15.14 KiB, done.
Total 62 (delta 2), reused 0 (delta 0)

-----> Receiving push
-----> Using Ruby version: ruby-2.0.0
-----> Installing dependencies using Bundler version 1.3.2
       Running: bundle install --without development:test --path vendor/bundle --binstubs vendor/bundle/bin --deployment
       Fetching gem metadata from https://rubygems.org/..........
       Fetching gem metadata from https://rubygems.org/..
       Installing rake (10.0.3)
       ...
       Installing rails (3.1.10)
       ...
       Installing uglifier (1.3.0)

       Your bundle is complete! It was installed into ./vendor/bundle
       Post-install message from rdoc:
       Depending on your version of ruby, you may need to install ruby rdoc/ri data:
       <= 1.8.6 : unsupported
       = 1.8.7 : gem install rdoc-data; rdoc-data --install
       = 1.9.1 : gem install rdoc-data; rdoc-data --install
       >= 1.9.2 : nothing to do! Yay!

       Cleaning up the bundler cache.
-----> Preparing app for Rails asset pipeline
       Running: rake assets:precompile
       Asset precompilation completed (6.34s)
       Cleaning assets
-----> WARNINGS:
       You have not declared a Ruby version in your Gemfile.
       To set your Ruby version add this line to your Gemfile:
       ruby '2.0.0'
-----> Building image
-----> Uploading image (34M)

To ssh://APP_NAME@kilatiron.net/repository.git
 * [new branch]      mysql -> mysql
~~~

Tambahkan MySQLs Add-on dengan `plan free` penyebaran dan menyebarkan:

~~~bash
$ ironapp APP_NAME/mysql addon.add mysqls.free
$ ironapp APP_NAME/mysql deploy
~~~

Siapkan database dengan menjalankan migrate menggunakan [perintah Run] [menjalankan perintah]:

~~~bash
$ ironapp APP_NAME/mysql run "rake db:migrate"
~~~

Selamat, Anda sekarang dapat mengakses aplikasi di http://mysql-APP_NAME.kilatiron.net.

Untuk informasi tambahan lihat [Ruby on Rails catatan] [catatan-rails] dan
lainnya [dokumen-spesifik-ruby] [panduan-ruby].

[Ruby on Rails]: http://rubyonrails.org/
[KilatIron]: http://www.cloudkilat.com/
[Contoh-aplikasi]: https://github.com/cloudControl/ruby-rails-example-app
[Ruby buildpack]: https://github.com/cloudControl/buildpack-ruby
[Rails tutorial]: http://ruby.railstutorial.org/
[Bundler]: http://bundler.io/
[Procfile]: /Platform%20Documentation.md/#buildpacks-and-the-procfile
[Filesystem]: /Platform%20Documentation.md/#non-persistent-filesystem
[Menjalankan perintah]: /Guides/Ruby/RunCommand.md
[catatan-rails]: /Guides/Ruby/RubyNotes.md
[panduan-ruby]: /Guides/Ruby
[Gem itu sendiri]: http://rubygems.org/gems/cloudcontrol-rails
[shared MySQL Add-on]: /Add-on%20Documentation/Data%20Storage/MySQLs.md
