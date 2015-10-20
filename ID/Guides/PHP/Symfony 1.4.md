#Deploying Symfony 1.4 untuk CloudKilat

! [Deployment Sukses] (/ statis / apps / gambar / symfony1.4-homepage.png)

Jika Anda sedang mencari kaya fitur, open source, PHP Framework untuk proyek-proyek Anda lengkap dengan masyarakat yang kuat, berbagai macam plugin dan add-ons dan sejarah yang kuat pengembangan aktif, Anda tidak dapat melewati [Symfony 1 ] (http://symfony.com/). Muncul dengan berbagai fitur untuk mempercepat pengembangan aplikasi Anda, termasuk:

 * Independen mesin database
 * Sangat dikonfigurasi
 * Perusahaan siap
 * Mudah untuk memperpanjang
 * Built-in internasionalisasi
 * Pabrik, plug-in, dan mixin
 * Unit Built-in dan kerangka pengujian fungsional

Dalam tutorial ini, kita akan membawa Anda melalui penggelaran CakePHP v2.2.1 untuk [platform CloudKilat] (http://www.cloudkilat.com/).

## Prasyarat

Anda akan hanya perlu beberapa hal untuk mengikuti bersama dengan tutorial ini. Ini adalah:

 * A [Git klien] (http://git-scm.com/), apakah baris perintah atau GUI.
 * Seorang klien MySQL, apakah baris perintah atau GUI, seperti [MySQL Workbench] (http://dev.mysql.com/downloads/workbench/) atau alat baris perintah.

## 1. Ambil Copy Symfony

Sekarang bahwa Anda memiliki prasyarat di tempat, men-download salinan terbaru, stabil, rilis Symfony, ** versi 1.4 ** pada saat atau penerbitan. Anda dapat menemukannya di: [http://www.symfony-project.org/installation/1_4](http://www.symfony-project.org/installation/1_4). Setelah itu, ekstrak ke sistem file lokal Anda.

[Sumber file] (/ statis / apps / gambar / symfony1-source.png)

## 2. Mengubah Kode

Seperti yang saya sebutkan sebelumnya, beberapa perubahan perlu dibuat ke default konfigurasi Symfony. Perubahan ini adalah sebagai berikut:

 * Ubah default [Cross-Site Request Pemalsuan] (https://www.owasp.org/index.php/Cross-Site_Request_Forgery_ (CSRF)) (CSRF) kunci rahasia
 * Aktifkan Propel bukan Ajaran
 * Sesi Store dan log file dalam database
 * Auto-ajaib menentukan lingkungan dan mengatur konfigurasi

### 2.1 Toko Sesi dalam Database & Nonaktifkan Logging

Kita perlu melakukan ini karena Symfony, secara default, log ke dan menyimpan file sesi pada filesystem. Namun, pendekatan ini dianjurkan pada platform CloudKilat.

Terlebih lagi, menyimpan file dalam lingkungan multi-server dapat menyebabkan sulit untuk debug masalah. Jadi apa yang kita akan lakukan adalah untuk menyimpan sesi dalam database dan menonaktifkan logging.

Untungnya, Symfony ditulis dalam cara yang sangat lurus ke depan dan dikonfigurasi, jadi ini tidak terlalu sulit untuk dilakukan. Terlebih lagi, masyarakat sekitar sangat sehat, sehingga ada banyak pilihan dan dukungan yang tersedia.

### 2.2 Auto-ajaib menentukan lingkungan dan mengatur konfigurasi

Karena setiap lingkungan akan, mungkin, memiliki pengaturan konfigurasi yang berbeda, kami juga harus mampu membedakan antara mereka. Untuk tetap sederhana, apa yang perlu kita lakukan adalah untuk memiliki satu file bootstrap, di beberapa penyebaran, dan untuk itu untuk pemrograman tahu di mana itu dan mengatur pilihan konfigurasi yang sesuai. Jadi kita akan membuat penambahan kode sehingga hal ini terjadi.

## 3. Masukan Kode di bawah kontrol Git

Ok, sekarang mari kita mulai membuat perubahan ini dan menggunakan aplikasi. Kita akan mulai dengan meletakkan di bawah kontrol Git. Jadi jalankan perintah berikut untuk melakukannya:

    cd <directory Symfony Anda>

    git init.

    git add -A

    git commit -m "Selain Pertama sumber file"

Sekarang bahwa kode ini di bawah kontrol versi, kita akan membuat cabang pengujian juga, sehingga kita memiliki satu untuk menguji dengan dan satu untuk produksi. Jalankan perintah berikut dan itu akan dilakukan:

    git checkout pengujian -b

Jika Anda tidak terbiasa dengan Git, perintah sebelumnya akan checkout salinan cabang kami yang ada, menjadi cabang baru, yang disebut `` testing``. Anda dapat mengkonfirmasi bahwa Anda sekarang memiliki dua cabang, dengan menjalankan perintah berikut:

    git branch

Itu akan menunjukkan output yang mirip dengan di bawah:

    $ Git branch
        menguasai
        * Pengujian

Pilih nama yang unik untuk menggantikan `APP_NAME` tempat untuk aplikasi Anda dan membuatnya pada platform CloudKilat. Sekarang, kita perlu membuat penyebaran pertama kami kedua cabang ke platform CloudKilat. Untuk melakukan ini kita checkout cabang master, membuat aplikasi di akun CloudKilat kami dan mendorong dan menyebarkan kedua penyebaran. Dengan menjalankan perintah berikut, ini semua akan dilakukan:

    // Beralih ke cabang master
    Master checkout git

    // Membuat aplikasi
    ironapp APP_NAME membuat php

    // Menyebarkan cabang default
    ironapp APP_NAME / dorongan bawaan
    ironapp APP_NAME / default menyebarkan

    // Menyebarkan cabang pengujian
    ironapp APP_NAME / pengujian dorongan
    ironapp APP_NAME / pengujian menyebarkan

### 3.1 Symfony Auto-Terdeteksi

Ketika Anda melakukan ini, Anda akan melihat output yang mirip dengan berikut:

    $ Ironcliapp APP_NAME / dorongan bawaan
    Menghitung benda: 15, dilakukan.
    Delta kompresi menggunakan sampai 2 benang.
    Mengompresi objek: 100% (7/7), dilakukan.
    Menulis objek: 100% (8/8), 1,23 KiB, dilakukan.
    Total 8 (delta 4), kembali 0 (delta 0)

    >> Mendorong Menerima
    >> Kompilasi PHP
         INFO: Symfony 1.x terdeteksi
         INFO: ada '.ccconfig.yaml' ditemukan, pengaturan konten web untuk '/ Web.
    >> Bangunan gambar
    >> Gambar Mengunggah (3.0m)

    Untuk ssh: //APP_NAME@kilatiron.net/repository.git
       d90506c..4078c78 Master -> Master

Perhatikan baris berikut:

    INFO: Symfony 1.x terdeteksi
    INFO: ada '.ccconfig.yaml' ditemukan, pengaturan konten web untuk '/ Web.

Dalam versi sebelumnya dari platform CloudKilat, Anda akan harus menggunakan file-platform tertentu config disebut: `` .ccconfig.yaml`` dan di dalamnya mengatur sebagai berikut:

    BaseConfig:
      Webcontent: / web


## 4. Menginisialisasinya Diperlukan Add-ons

Sekarang itu selesai, kita perlu mengkonfigurasi dua add-ons, config dan mysqls. Config Add-on diperlukan untuk menentukan lingkungan aktif dan mysqls untuk menyimpan sesi dan login informasi.

### 4.1 Mengawali mysqls

Untuk menginisialisasi mysqls, jalankan perintah berikut dan membuat catatan dari output:

    // Menginisialisasinya addon mysqls.free untuk penyebaran standar
    ironapp APP_NAME / default addon.add mysqls.free

    // Ambil pengaturan
    ironapp APP_NAME / default addon mysqls.free

    // Menginisialisasinya addon mysqls.free untuk penyebaran pengujian
    ironapp APP_NAME / pengujian addon.add mysqls.free

    // Ambil pengaturan
    ironapp APP_NAME / pengujian addon mysqls.free

Output dari perintah akan mirip dengan yang di bawah ini:

    Addon: mysqls.free

     Pengaturan
       MYSQLS_DATABASE: <database_username>
       MYSQLS_PASSWORD: <database_password>
       MYSQLS_PORT: 3306
       MYSQLS_HOSTNAME: mysqlsdb.co8hm2var4k9.eu-west-1.rds.amazonaws.com
       MYSQLS_USERNAME: <database_name>

### 4.2 Mengawali config

Sekarang kita perlu mengkonfigurasi addon config dan menyimpan lingkungan masing-masing pengaturan di dalamnya. Jadi jalankan perintah berikut untuk melakukan hal ini:

    // Mengatur pengaturan lingkungan default
    ironapp APP_NAME / default config.add APPLICATION_ENV = produksi

    // Mengatur pengaturan lingkungan pengujian
    ironapp APP_NAME / pengujian config.add APPLICATION_ENV = pengujian

Sekarang ini dilakukan, kita siap untuk membuat beberapa perubahan pada kode kita untuk menggunakan konfigurasi baru.

## 5. Konfigurasi lingkungan

### 5.1 Mengubah Permintaan standar Cross-Site Pemalsuan (CSRF) Kunci Rahasia

Dalam `` apps / frontend / config / settings.yml``, menemukan konfigurasi berikut:

    # Formulir rahasia keamanan (perlindungan CSRF)
    csrf_secret: 83d9c79e8609e738e148c8ccd41113d5c5ddf301

Jika yang tersisa di tempat, Symfony akan melempar kesalahan. Jadi kita perlu mengubahnya ke nilai yang unik untuk lingkungan kita. Aku sudah berubah untuk tujuan contoh sederhana sebagai berikut:

    csrf_secret: 664914c3be95fc798d87a12ce4ea1610e2104d92

### 5.2 Aktifkan Propel bukan Ajaran

Sekarang Anda tidak perlu melakukan hal ini. Aku telah melakukannya karena aku lebih nyaman dengan itu. Jika Anda seorang guru Doktrin, atau hanya lebih nyaman dengan itu, silakan melewati bagian ini.

Dalam `` config / properties.ini``, mencari baris berikut:

    orm = Doktrin

Mengubahnya ke:

    orm = Propel

### 5.3 Toko Sesi di Database

Di bawah `` apps / frontend / config`` membuka file `` factories.yaml``. Dalam file itu, kita perlu menambahkan di konfigurasi sesi untuk kedua prod dan uji. Ini diidentifikasi oleh `` prod: `` dan `` uji: `` masing-masing. Untuk masing-masing, tambahkan konfigurasi yang sama dengan yang di bawah, mengubah rincian masing di mana diperlukan:

      Penyimpanan:
        kelas: sfCacheSessionStorage
        param:
          session_name: symfony1_CloudKilat
          session_cookie_path: /
          session_cookie_domain: APP_NAME.kilatiron.net
          session_cookie_lifetime: +30 hari
          session_cookie_secure: palsu
          session_cookie_http_only: true
          cache:
            kelas: sfAPCCache
            param: ~

Apa yang telah dilakukan adalah untuk memberitahu Symfony menggunakan [sfCacheSessionStorage] (http://www.symfony-project.org/api/1_4/sfCacheSessionStorage) untuk mengelola penyimpanan sesi dengan [sfAPCCache] (http: // www. symfony-project.org/api/1_4/sfAPCCache) kelas untuk fisik menyimpan sesi.

### 5.4 Nonaktifkan Logging

Seperti yang kita hanya melakukan untuk konfigurasi sesi, di factories.yaml, menambahkan konfigurasi berikut, untuk prod dan uji, untuk menonaktifkan logging:

      logger:
        kelas: sfNoLogger
        param:
          Tingkat: err
          penebang: ~

Apa yang telah dilakukan adalah untuk memberitahu Symfony menggunakan [sfNoLogger] (http://www.symfony-project.org/api/1_4/sfNoLogger) untuk mengelola logging, secara efektif membuang log, seperti `` / dev / null `` di Linux / BSD.

### 5.5 Auto-Ajaib Menentukan Lingkungan dan Set Konfigurasi

Dalam file `` web / index.php``, menemukan garis yang mirip dengan di bawah ini:

    $ Konfigurasi = ProjectConfiguration :: getApplicationConfiguration (
        'Frontend', 'prod', false
    );

Dan kemudian menggantinya dengan yang berikut

    $ Lingkungan = 'prod';
    $ Debug = false;

    if (! empty ($ _ SERVER ['HTTP_HOST']) && strpos ($ _ SERVER ['HTTP_HOST'], 'localdomain') === FALSE) {
        // Parse file json dengan addons kredensial
        $ String = file_get_contents ($ _ ENV ['CRED_FILE'], false);

        if ($ string == false) {
            die ('FATAL: Tidak dapat membaca berkas kredensial');
        }

        $ Creds = json_decode ($ string, true);
        $ Lingkungan = $ creds ['CONFIG'] ['CONFIG_VARS'] ['CAKE_ENV'];
    } Else {
        $ Lingkungan = 'pembangunan';
    }

    if ($ lingkungan == 'pembangunan') {
        $ Debug = true;
    }

    $ Konfigurasi = ProjectConfiguration :: getApplicationConfiguration (
        'Frontend', $ lingkungan, $ men-debug
    );

Apa yang akan dilakukan adalah, jika kita tidak menggunakan lingkungan pembangunan daerah, ditentukan dengan memiliki `` localdomain`` di url, maka kita akan mengambil salinan mandat file dari pengaturan lingkungan, yang datang dengan setiap aplikasi CloudKilat penyebaran secara default.

Di sana, kita akan melihat apa nilai ** APPLICATION_ENV ** config var. Jika itu diatur, maka kita mengatur lingkungan untuk itu. Jika tidak diatur, maka kita akan mengatur lingkungan untuk `` prod``, sehingga kita memiliki standar aman dan waras setiap saat.

Konfigurasi 5.6 database ###

Dalam `` config / databases.yml`` secara default, Symfony akan memiliki pengaturan berikut ini:

    semua:
      doktrin:
        kelas: sfDoctrineDatabase

Apa yang kita akan lakukan adalah mengubahnya untuk memiliki nilai-nilai dari mysqls.free dijalankan kami add-on. Jadi mengambil pengaturan yang Anda mencatat sebelumnya dan mengubah file sehingga, dengan pengaturan Anda, terlihat mirip dengan konfigurasi di bawah ini:

    dev:
      mendorong:
        kelas: sfPropelDatabase
        param:
          classname: DebugPDO
          men-debug: {realmemoryusage: true, rincian: {waktu: {diaktifkan: true}, lambat: {diaktifkan: true, ambang batas: 0,1}, mem: {diaktifkan: true}, mempeak: {diaktifkan: true}, memdelta: {diaktifkan : true}}}
          dsn: 'mysql: host = localhost; dbname = CloudKilat_symfony1'
          username: cc_dev
          password: cc_dev
          encoding: utf8
          gigih: true
          pooling: true

    Tes:
      mendorong:
        kelas: sfPropelDatabase
         param:
          classname: PropelPDO
          men-debug: {realmemoryusage: true, rincian: {waktu: {diaktifkan: true}, lambat: {diaktifkan: true, ambang batas: 0,1}, mem: {diaktifkan: true}, mempeak: {diaktifkan: true}, memdelta: {diaktifkan : true}}}
           dsn: 'mysql: host = mysqlsdb.co8hm2var4k9.eu-west-1.rds.amazonaws.com; dbname = <database_name>'
           username: <database_username>
           password: <database_password>
          encoding: utf8
          gigih: true
          pooling: true

    prod:
      mendorong:
        kelas: sfPropelDatabase
        param:
          classname: PropelPDO
          men-debug: {realmemoryusage: true, rincian: {waktu: {diaktifkan: true}, lambat: {diaktifkan: true, ambang batas: 0,1}, mem: {diaktifkan: true}, mempeak: {diaktifkan: true}, memdelta: {diaktifkan : true}}}
          dsn: 'mysql: host = mysqlsdb.co8hm2var4k9.eu-west-1.rds.amazonaws.com; dbname = <database_name>'
          username: <database_username>
          password: <database_password>
          encoding: utf8
          gigih: true
          pooling: true

    semua:

Perlu diketahui bahwa jika Anda menggunakan Doktrin, meninggalkan `` sfDoctrineDatabase`` di tempat bukan menggantinya dengan `` sfPropelDatabase``.

Setelah ini, tahap semua file dalam Git dan komit mereka dengan pesan komit sesuai. Kemudian melakukan perubahan dan mendorong dan menyebarkan kedua lingkungan lagi sehingga informasi baru akan digunakan. Hal ini dapat dilakukan dengan cepat dengan perintah berikut:

    // Melakukan perubahan
    git komit -m "berubah untuk menyimpan log dan sesi di mysql dan auto-menentukan lingkungan"

    // Menyebarkan cabang default
    ironapp APP_NAME / dorongan bawaan
    ironapp APP_NAME / default menyebarkan

    pengujian checkout git
    Master menggabungkan git

    // Menyebarkan cabang pengujian
    ironapp APP_NAME / pengujian dorongan
    ironapp APP_NAME / pengujian menyebarkan


## 7. Tinjau Deployment yang

Dengan itu selesai, maka kita lihat baik penyebaran Anda untuk memastikan bahwa mereka bekerja.

Dengan itu, Anda harus bangun dan berjalan, siap untuk membuat berikutnya, menakjubkan, aplikasi PHP web Anda, menggunakan Symfony. Jika Anda memiliki masalah apapun, jangan ragu untuk email [support@CloudKilat.ch] (mailto: support@CloudKilat.ch).

## Links

 * [http://www.designdisclosure.com/2009/11/symfony-apc-cache-and-memcache-session-storage/](http://www.designdisclosure.com/2009/11/symfony-apc-cache-and-memcache-session-storage/)
