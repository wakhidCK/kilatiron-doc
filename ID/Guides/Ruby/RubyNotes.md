# Catatan Ruby


## Procfile

Platform KilatIron menggunakan file bernama `Procfile` untuk menentukan bagaimana menjalankan aplikasi Anda. `Procfile` menggunakan format YAML untuk menentukan konfigurasi yang diinginkan.

Perintah yang ditentukan di bawah `masuk web` akan digunakan untuk memulai web
Server.

Jika Anda memiliki `Procfile` dengan konten berikut:
~~~
web: ruby my_server.rb
~~~
`ruby my_server.rb` akan dieksekusi pada saat deployment.

Misalnya `Procfiles` yang dapat digunakan untuk memulai aplikasi Rails dapat ditemukan
[Nanti dalam dokumen ini] [rel-procfile].

Untuk konteks yang lebih, kunjungi [Landasan dokumentasi] [procfile].


## Versi Ruby

Versi Ruby dapat ditentukan dalam Gemfile. Jika tidak ada versi yang ditentukan,
yang standar akan digunakan. Saat ini, versi standar adalah 2.0.0.

Untuk menentukan versi, menempatkan direktif `ruby` sebagai baris pertama di
`Gemfile`, contoh:
~~~
ruby "1.9.3"
~~~

Pada push berikutnya, versi Ruby yang diinginkan akan digunakan.

Untuk melihat semua versi yang didukung, periksa dokumentasi [Ruby buildpack] [ruby-buildpack].


# Catatan Rails


## Rails Procfile

Untuk menjalankan server Rails, membuat file bernama `Procfile` dengan konten berikut:

~~~
web: exec bundle rails s -p $PORT
~~~


## Asset Pipeline

Jika asset pipeline digunakan, file `config/application.rb` harus berisi baris berikut:

~~~ Ruby
config.assets.initialize_on_precompile = false if ENV['BUILDPACK_RUNNING']
~~~

Menonaktifkan intialization pada precompile hanya selama proses membangun (di buildpack), tetapi tidak mempengaruhi eksekusi kode normal, misalnya menjalankan server web atau perintah dijalankan.


## Database

Untuk menggunakan database dalam aplikasi Rails, file `config/database.yml` perlu dimodifikasi. Kredensial dan informasi database lainnya harus tertanam dalam ERB. Namun, ekstensi file harus tetap `.yml`.

Berikut adalah contoh dari file `database.yml` yang akan menggunakan MySQL pada environment production.

~~~ Erb
development:
  adapter: sqlite3
  database: db/development.sqlite3
  pool: 5
  timeout: 5000

test:
  adapter: sqlite3
  database: db/test.sqlite3
  pool: 5
  timeout: 5000

production:
  adapter: mysql2
  encoding: utf8
  pool: 5
  database: <%= "'#{ ENV['MYSQLS_DATABASE'] }'" %>
  host: <%= "'#{ ENV['MYSQLS_HOSTNAME'] }'" %>
  port: <%= ENV["MYSQLS_PORT"] %>
  username: <%= "'#{ ENV['MYSQLS_USERNAME'] }'" %>
  password: <%= "'#{ ENV['MYSQLS_PASSWORD'] }'" %>
~~~

CATATAN: Sebagai YAML karakter markup dapat digunakan dalam password, string dalam ruby diapit tanda kutip tunggal. Karena port diperlukan untuk menjadi integer, variable tersebut tidak diapit dalam tanda kutip.

Atau Anda dapat menggunakan gem [cloudcontrol-rails].


Lingkungan ##

Rails server dapat dijalankan dalam environment yang berbeda. Production adalah default, tetapi Anda dapat mengubahnya dengan menetapkan `RAILS_ENV` dan lingkungan `RAKE_ENV` variabel dengan [Custom Config addon] (https://community.CloudKilat.ch/tutorial/custom-config-add-on/ ). Sebagai contoh:

~~~
ironapp APP_NAME/DEP_NAME config.add RACK_ENV=some_env RAILS_ENV=some_env
~~~

CATATAN: Gems dalam environment development dan test tidak dimasukkan ke proses instalasi bundle.



[Cloudcontrol-rails]: https://rubygems.org/gems/cloudcontrol-rails
[Procfile]: /Platform%20Documentation.md/#version-control-images
[rails-procfile]: #rails-procfile
[Ruby-buildpack]: https://github.com/cloudControl/buildpack-ruby
