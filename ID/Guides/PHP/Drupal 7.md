#Deploying Drupal 7
! [Deployment Sukses] (/ statis / apps / gambar / drupal7.png)

Jika Anda sedang mencari sebuah platform manajemen konten yang fleksibel, ramah dan kuat, dibangun di PHP, untuk proyek-proyek Anda, Anda benar-benar tidak dapat melewati [Drupal] (http://drupal.org/). Setelah secara konsisten tumbuh dan berkembang sejak pertama kali diciptakan oleh Dries Buytaert pada Januari 2001, Drupal 7 adalah iterasi terbaru dan hadir dengan berbagai fitur untuk mempercepat pengembangan aplikasi Anda, termasuk:

 * Sejumlah inti dan modul pihak ketiga
 * Localised dalam 55 bahasa
 * Sebuah besar, berkembang, masyarakat
 * Pemberitahuan Pembaruan
 * Mudah untuk membaca dokumentasi

Dalam tutorial ini, kita akan membawa Anda melalui penggelaran Drupal 7 ke [platform CloudKilat] (http://www.CloudKilat.ch).

## Prasyarat

Anda akan hanya perlu beberapa hal untuk mengikuti bersama dengan tutorial ini. Ini adalah:

 * A [Git klien] (http://git-scm.com/), apakah baris perintah atau GUI.
 * Seorang klien MySQL, apakah baris perintah atau GUI, seperti [MySQL Workbench] (http://dev.mysql.com/downloads/workbench/) atau alat baris perintah.

## 1. Ambil Copy Drupal 7.

Jadi sekarang bahwa Anda memiliki prasyarat di tempat, men-download salinan terbaru, stabil, rilis, 7.14 pada saat atau penerbitan. Anda dapat menemukannya di: [http://ftp.drupal.org/files/projects/drupal-7.14.tar.gz](http://ftp.drupal.org/files/projects/drupal-7.14.tar.gz). Setelah itu, ekstrak ke sistem file lokal Anda.

[Sumber file] (/ statis / apps / gambar / drupal7-files.png)

Setelah ini, dalam lingkungan pengembangan lokal Anda, melakukan instalasi standar Drupal, seperti dibahas dalam [dokumentasi instalasi secara online] (http://drupal.org/documentation/install).

## 2. Mengubah Kode

Beberapa perubahan perlu dibuat ke default Drupal 7 konfigurasi. Perubahan ini adalah sebagai berikut:

### 2.1 Auto-Ajaib Menentukan Lingkungan dan Konfigurasi Aplikasi

Seperti cukup umum dalam pengembangan aplikasi modern, kami menyebarkan ke beberapa lingkungan, seperti pengujian, pementasan dan produksi. Dan masing-masing lingkungan ini akan, mungkin, memiliki pengaturan konfigurasi yang berbeda untuk * Database *, * caching * dll, jadi kita harus mampu membedakan antara mereka. Drupal tidak secara otomatis melakukan hal ini di luar kotak, tapi itu tidak sulit untuk menyesuaikan untuk mewujudkannya.

Dan untungnya bagi kita, Drupal 7, secara default, toko kebanyakan penebangan dan sesi informasi secara otomatis dalam database. Jadi kita tidak perlu melakukan banyak perubahan konfigurasi sana untuk memastikan bahwa ia bekerja.

Apa yang tidak sedang dibahas dalam tutorial ini adalah menghubungkan instalasi ke filesystem ditulisi, seperti [Amazon S3] (http://aws.amazon.com/s3/). Yang akan dilakukan dalam tindak lanjut untuk tutorial ini.

## 3. Masukan Control Kode bawah Git

Ok, sekarang mari kita mulai membuat perubahan ini dan menggunakan aplikasi. Kita akan mulai dengan meletakkan di bawah kontrol Git. Jadi jalankan perintah berikut untuk melakukannya:

    cd <directory Drupal 7 Anda>
    
    git init.
    
    git add -A
    
    git commit -m "Selain Pertama sumber file"
    
Sekarang bahwa kode ini di bawah kontrol versi, kita akan membuat cabang pengujian juga, sehingga kita memiliki satu untuk menguji dengan dan satu untuk produksi. Jalankan perintah berikut dan itu akan dilakukan:

    git checkout pengujian -b
    
Jika Anda tidak terbiasa dengan Git, perintah sebelumnya akan checkout salinan cabang kami yang ada, menjadi cabang baru, yang disebut ** pengujian **. Anda dapat mengkonfirmasi bahwa Anda sekarang memiliki dua cabang, dengan menjalankan perintah berikut:

    git branch
    
Itu akan menunjukkan output yang mirip dengan di bawah:

    $ Git branch
        menguasai
        * Pengujian

Pilih nama yang unik untuk menggantikan `APP_NAME` tempat untuk aplikasi Anda dan membuatnya pada platform CloudKilat. Sekarang, kita perlu membuat penyebaran pertama kami kedua cabang ke platform CloudKilat. Untuk melakukan ini kita checkout cabang master, membuat aplikasi di akun CloudKilat dan * mendorong * dan * menggunakan * baik penyebaran. Dengan menjalankan perintah berikut, ini semua akan dilakukan:

    // Beralih ke cabang master
    Master checkout git
    
    // Membuat aplikasi, menunjukkan itu berbasis PHP
    ironapp APP_NAME membuat php
    
    // Menyebarkan cabang default
    ironapp APP_NAME / dorongan bawaan
    ironapp APP_NAME / default menyebarkan
    
    // Menyebarkan cabang pengujian
    ironapp APP_NAME / pengujian dorongan
    ironapp APP_NAME / pengujian menyebarkan

## 4. Menginisialisasinya Addons Diperlukan

Sekarang itu selesai, kita perlu mengkonfigurasi dua add-ons, config dan mysqls. Config add-on diperlukan untuk menentukan lingkungan aktif dan mysqls untuk menyimpan sesi dan login informasi.

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
       MYSQLS_DATABASE: <database_name>
       MYSQLS_PASSWORD: <database_password>
       MYSQLS_PORT: 3306
       MYSQLS_HOSTNAME: mysqlsdb.co8hm2var4k9.eu-west-1.rds.amazonaws.com
       MYSQLS_USERNAME: <database_username>

### 4.2 Mengawali config

Sekarang kita perlu mengkonfigurasi addon config dan menyimpan lingkungan masing-masing pengaturan di dalamnya. Jadi jalankan perintah berikut untuk melakukan hal ini:

    // Mengatur pengaturan lingkungan default
    ironapp APP_NAME / default config.add APPLICATION_ENV = produksi

    // Mengatur pengaturan lingkungan pengujian
    ironapp APP_NAME / pengujian config.add APPLICATION_ENV = pengujian

Sekarang ini dilakukan, kita siap untuk membuat beberapa perubahan pada kode kita untuk menggunakan konfigurasi baru.

## 5. Konfigurasi lingkungan

Sekarang kita akan memperpanjang proses bootstrap Drupal 7 untuk dapat menentukan lingkungan yang sedang digunakan, yang kita bicarakan sebelumnya. Untuk melakukan itu, membuka `` situs / default / settings.php`` dan menambahkan kode di bawah ini tepat di akhir file.

Silahkan lihat pada itu dan kami akan pergi melalui bersama-sama.

    $ Env = 'produksi';
    
    if (! empty ($ _ SERVER ['HTTP_HOST']) && strpos ($ _ SERVER ['HTTP_HOST'], 'localdomain')! == FALSE) {
       $ Env = 'pembangunan';
    }
    
    if (! empty ($ _ SERVER ['HTTP_HOST']) && strpos ($ _ SERVER ['HTTP_HOST'], 'localdomain') === FALSE) {
        // Parse file json dengan addons kredensial
        $ String = file_get_contents ($ _ ENV ['CRED_FILE'], false);
    
        if ($ string == false) {
            die ('FATAL: Tidak dapat membaca berkas kredensial');
        }
    
        $ Creds = json_decode ($ string, true);
    
        // Sekarang getenv ('APPLICATION_ENV') harus bekerja:
        $ Env = $ creds ['CONFIG'] ['CONFIG_VARS'] ['APPLICATION_ENV'];
    }
    
    
    $ Local_settings = __DIR__. "/settings.{$env}.inc";
    termasuk ($ local_settings);

Pertama, kita mengatur lingkungan ke default produksi. Kemudian, jika kita berada dalam lingkungan pembangunan daerah, sebagaimana ditentukan, bukan hanya, dengan memiliki `` localdomain`` dalam URL, maka kita mengatur lingkungan untuk pembangunan.

Jika tidak, kami akan mengambil setting yang terkandung dalam CloudKilat pengaturan kredensial berkas, ** APPLICATION_ENV **, yang kita tetapkan sebelumnya dengan addon konfigurasi, yang harus menjadi salah satu '** ** produksi' atau '** pengujian **' .

Dengan kode ini di tempat, kita sekarang dapat bootstrap beberapa lingkungan. Setelah ini, kita perlu mengkonfigurasi database ,.

## 6. Konfigurasi Database

Ketika kami dikonfigurasi add ons sebelumnya (mysqls dan config) pengaturan yang secara otomatis bertahan dengan lingkungan server berjalan. Jadi kita sekarang dapat mengambil pengaturan ini, ketika kita tidak dalam lingkungan pembangunan daerah, dan mengkonfigurasi koneksi database kami untuk menggunakannya secara otomatis.

Ini benar-benar berguna karena kita tidak perlu melakukan terlalu banyak untuk memanfaatkan opsi.

Di bawah ** situs / default ** membuat tiga file baru:

 * `` Settings.development.inc``
 * `` Settings.testing.inc``
 * `` Settings.production.inc``

Di sana, melewati pengaturan database masing-masing untuk lingkungan yang berbeda Anda bahwa Anda dapat mengambil dari database add-on konfigurasi atau lingkungan pengembangan lokal Anda.

Dua contoh yang disediakan di bawah:

### 6.1 Produksi

    <? Php
    
    // Membaca kredensial berkas
    $ String = file_get_contents ($ _ ENV ['CRED_FILE'], false);
    if ($ string == false) {
        die ('FATAL: Tidak dapat membaca berkas kredensial');
    }
    
    // File berisi string JSON, decode dan mengembalikan array asosiatif
    $ Creds = json_decode ($ string, true);
    
    $ Database = array (
      'Default' => array (
        'Default' => array (
          'Database' => $ creds ["MYSQLS"] ["MYSQLS_DATABASE"],
          'Username' => $ creds ["MYSQLS"] ["MYSQLS_USERNAME"],
          'Password' => $ creds ["MYSQLS"] ["MYSQLS_PASSWORD"],
          'Host' => $ creds ["MYSQLS"] ["MYSQLS_HOSTNAME"],
          'Pelabuhan' => '',
          'Driver' => 'mysql',
          'Awalan' => '',
        ),
      ),
    );

### 6.2 Pengujian

    <? Php
    
    // Membaca kredensial berkas
    $ String = file_get_contents ($ _ ENV ['CRED_FILE'], false);
    if ($ string == false) {
        die ('FATAL: Tidak dapat membaca berkas kredensial');
    }
    
    // File berisi string JSON, decode dan mengembalikan array asosiatif
    $ Creds = json_decode ($ string, true);
    
    $ Database = array (
      'Default' => array (
        'Default' => array (
          'Database' => $ creds ["MYSQLS"] ["MYSQLS_DATABASE"],
          'Username' => $ creds ["MYSQLS"] ["MYSQLS_USERNAME"],
          'Password' => $ creds ["MYSQLS"] ["MYSQLS_PASSWORD"],
          'Host' => $ creds ["MYSQLS"] ["MYSQLS_HOSTNAME"],
          'Pelabuhan' => '',
          'Driver' => 'mysql',
          'Awalan' => '',
        ),
      ),
    );

### 6.3 Database Schema

Ok, kita perlu membuat skema database dasar untuk menyimpan sesi dan
log informasi serta pengaturan konfigurasi dan data pengguna lain yang
Drupal toko. Unduh [file] (/ statis / apps / drupal_CloudKilat_init.sql), siap digunakan untuk menginisialisasi database.

Sekarang, di shell, kita akan memuat data ke dalam mysql contoh remote yang kita buat sebelumnya. Untuk melakukannya, jalankan perintah berikut, mengubah pilihan masing-masing dengan pengaturan konfigurasi Anda, melakukan hal ini untuk kedua ** bawaan ** dan ** pengujian **:

    mysql -u <database_username> p \
        h mysqlsdb.co8hm2var4k9.eu-west-1.rds.amazonaws.com \
        --ssl-ca = mysql-ssl-ca-cert.pem <database_name> <drupal_CloudKilat_init.sql

Pada perintah di atas, Anda dapat melihat referensi ke pem ** berkas **.. Ini dapat didownload dari: [http://s3.amazonaws.com/rds-downloads/mysql-ssl-ca-cert.pem](http://s3.amazonaws.com/rds-downloads/mysql-ssl-ca-cert.pem). Semua yang baik, perintah akan selesai diam-diam, memuat data. Anda dapat memeriksa bahwa semua sudah pergi baik dengan perintah berikut:

    mysql -u <database_username> p \
        h mysqlsdb.co8hm2var4k9.eu-west-1.rds.amazonaws.com \
        --ssl-ca = mysql-ssl-ca-cert.pem <database_name>
    
    menampilkan tabel;
    
Ini harus menunjukkan output yang Anda mirip dengan di bawah:

Ketik 'bantuan;' atau '\ h' untuk membantu. Ketik '\ c' untuk menghapus pernyataan masukan saat ini.

    mysql> show tables;
    + ----------------------------- +
    | Tables_in_dep29a6hxt9 |
    + ----------------------------- +
    | Tindakan |
    | Authmap |
    | Bets |
    | Blok |
    | Block_custom |
    ...
    | Taxonomy_vocabulary |
    | Url_alias |
    | Pengguna |
    | Users_roles |
    | Variabel |
    | Pengawas |
    + ----------------------------- +
    74 rows in set (0.06 sec)
    
    mysql>

## 7. Komit Kode Perubahan

Sekarang itu selesai, melakukan perubahan yang kami buat sebelumnya dan mendorong dan menyebarkan kedua lingkungan lagi sehingga informasi baru akan digunakan. Hal ini dapat dilakukan dengan cepat dengan perintah berikut:

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

## 8. Tinjau Deployment yang

Dengan itu selesai, maka Anda akan dapat melihat di kedua penyebaran Anda untuk memastikan bahwa mereka bekerja.

Dengan itu, Anda harus bangun dan berjalan, siap untuk membuat berikutnya, menakjubkan, aplikasi PHP web Anda, menggunakan Drupal 7. Jika Anda memiliki masalah apapun, jangan ragu untuk email [support@cloudkilat.com] (mailto: dukungan @ cloudkilat. com).
