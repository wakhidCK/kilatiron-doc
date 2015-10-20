# Run Command Contoh untuk Ruby

Jalankan perintah benar-benar berguna bagi programmer ruby. Berikut adalah beberapa contoh bagaimana hal itu dapat digunakan untuk tugas-tugas sehari-hari seperti ruby ​​menjalankan migrasi atau menggunakan rel konsol.

Untuk bermigrasi database:

~~~ Pesta
$ Ironcliapp APP_NAME / DEP_NAME run "rake db: bermigrasi"
~~~

Untuk menjalankan rel konsol:
~~~ Pesta
$ Ironcliapp APP_NAME / DEP_NAME run "rel c"
~~~

Berikut ini adalah contoh lengkap di mana beberapa perintah dijalankan di sesi pesta remote:

~~~
$ Ironcliapp APP_NAME / DEP_NAME run pesta
Menghubungkan ...
Peringatan: Secara ditambahkan '[XXXX]: <PORT>' (RSA) ke daftar host dikenal.
u <PORT> @ <DEP_ID> - <PORT>: ~ / www $ rel judul g perancah Posting: isi string: teks
      memanggil active_record
Menghubungkan ke database yang ditentukan oleh database.yml
      membuat db / bermigrasi / 20121029153226_create_posts.rb
      membuat app / model / post.rb
      memanggil test_unit
      membuat tes / unit / post_test.rb
      membuat tes / perlengkapan / posts.yml
      memanggil resource_route
       sumber dengan rute: posting
      memanggil scaffold_controller
      membuat app / controllers / posts_controller.rb
      memanggil Erb
      membuat app / views / posts
      membuat app / views / posts / index.html.erb
      membuat app / views / posts / edit.html.erb
      membuat app / views / posts / show.html.erb
      membuat app / views / posts / new.html.erb
      membuat app / views / posts / _form.html.erb
      memanggil test_unit
      membuat tes / fungsional / posts_controller_test.rb
      memanggil pembantu
      membuat app / pembantu / posts_helper.rb
      memanggil test_unit
      membuat tes / unit / pembantu / posts_helper_test.rb
      memanggil aset
      memanggil js
      membuat app / aset / javascripts / posts.js
      memanggil css
      membuat app / aset / stylesheet / posts.css
      memanggil css
      membuat app / aset / stylesheet / scaffold.css
u <PORT> @ <DEP_ID> - <PORT>: ~ / www $ rake db: migrate
Menghubungkan ke database yang ditentukan oleh database.yml
Migrasi ke CreatePosts (20121029153226)
== CreatePosts: migrasi ============================================= =======
- Create_table (: posting)
   -> 0.0370s
== CreatePosts: bermigrasi (0.0371s) ========================================= ==

u <PORT> @ <DEP_ID> - <PORT>: ~ / www $ rel c
Menghubungkan ke database yang ditentukan oleh database.yml
Memuat lingkungan produksi (Rails 3.2.8)
irb (main): 001: 0> Post.all
  Posting Beban (1.1ms) SELECT `posts`. * FROM` posts`
=> []
irb (main): 002: 0> p = Post.new judul: "judul saya", isi: "saya isi"
=> # <Posting id: nihil, judul: "judul saya", isi: "saya isi", created_at: nihil, updated_at: nihil>
irb (main): 003: 0> p.save
   (1.1ms) BEGIN
  SQL (1.2ms) INSERT INTO `posts` (` content`, `created_at`,` title`, `updated_at`) VALUES ('konten saya', '2012/10/29 15:33:42', 'judul saya ',' 2012/10/29 15:33:42 ')
   (16.7ms) COMMIT
=> True
irb (main): 004: 0> Post.all
  Posting Beban (1.3ms) SELECT `posts`. * FROM` posts`
=> [# <Posting id: 1, judul: "judul saya", isi: "saya isi", created_at: "2012/10/29 15:33:42", updated_at: "2012/10/29 15:33 : 42 ">]
irb (main): 005: 0> exit
u <PORT> @ <DEP_ID> - <PORT>: ~ / www $ exit
Koneksi ke X.X.X.X ditutup.
Koneksi ke sshforwarder.kilatiron.net ditutup.
~~~

Hal yang sama dapat dicapai jika beberapa perintah individu dirantai:

~~~
$ Ironcliapp APP_NAME / DEPLOYMENT run "rel judul g perancah Posting: isi string: teks && rake db: bermigrasi && rel c"
~~~

Contoh sebelumnya cukup buatan dan itu kegunaan di dunia nyata akan dipertanyakan. Perubahan ke database dipertahankan, namun semua file yang dihasilkan hilang. Namun demikian hal ini menunjukkan penggunaan yang lebih kompleks dari perintah run dan memberikan sedikit wawasan dalam itu kekuasaan.
