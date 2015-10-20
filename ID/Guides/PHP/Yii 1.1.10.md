#Deploying Yii 1.1.10 ke CloudKilat

! [Deployment Sukses] (/ statis / apps / gambar / yii-kerangka-logo.png)

Jika Anda sedang mencari kilat cepat, ringan dan Kerangka PHP efektif untuk proyek-proyek Anda, satu tanpa banyak cruft dan warisan overhead beberapa lainnya kerangka PHP populer yang tersedia saat ini, Anda tidak dapat melewati [Yii Kerangka] (http://www.yiiframework.com/). Sekarang di [versi 1.1.11] (http://yii.googlecode.com/files/yii-1.1.11.58da45.tar.gz) datang dengan berbagai fitur untuk mempercepat pengembangan aplikasi Anda, termasuk:

 * Baked di Keamanan
 * Pendekatan Batal MVC
 * Sebuah besar, berkembang, masyarakat
 * Banyak plugin dan add-ons
 * Mudah untuk membaca dokumentasi

Dalam tutorial ini, kita akan membawa Anda melalui penggelaran v1.1.11 Yii Framework [platform CloudKilat] (http://www.cloudkilat.com/).

## Prasyarat

Anda akan hanya perlu beberapa hal untuk mengikuti bersama dengan tutorial ini. Ini adalah:

 * A [Git klien] (http://git-scm.com/), apakah baris perintah atau GUI.
 * Seorang klien MySQL, apakah baris perintah atau GUI, seperti [MySQL Workbench] (http://dev.mysql.com/downloads/workbench/) atau alat baris perintah.

## 1. Ambil Copy Kerangka Yii

Jadi sekarang bahwa Anda memiliki prasyarat di tempat, men-download salinan terbaru, stabil, rilis. Anda dapat menemukannya di: [http://yii.googlecode.com/files/yii-1.1.11.58da45.tar.gz](http://yii.googlecode.com/files/yii-1.1.11.58da45.tar.gz). Setelah itu, ekstrak file sytem lokal.

[Sumber file] (/ statis / apps / gambar / yii-kerangka-source.png)


## Buat Aplikasi Dasar

Ok, hal pertama yang pertama, [mengikuti tutorial secara online] (http://www.yiiframework.com/doc/guide/1.1/en/quickstart.installation) di situs Yii dan membuat aplikasi sederhana dalam lingkungan pengembangan lokal Anda. Kemudian, setelah Anda melakukan itu, kita akan membuat satu set perubahan sederhana dan Anda akan siap untuk menyebarkan aplikasi pertama Anda untuk CloudKilat.

## 2. Mengubah Kode

Ok, sekarang Anda memiliki aplikasi pengujian Anda dibuat dan berjalan, kita perlu mengubah beberapa bagian dari kode. Perubahan ini adalah sebagai berikut:

 * Informasi sesi Simpan dalam APC
 * Log pesan dalam database, bukan pada filesystem
 * Auto-ajaib menentukan lingkungan dan mengatur konfigurasi

### 2.1 Toko Sesi di Cache File log di Database, Bukan pada Filesystem yang

Kita perlu melakukan ini karena Yii Framework, secara default, log ke dan menyimpan file sesi pada filesystem. Namun, pendekatan ini tidak dianjurkan pada platform CloudKilat.

Terlebih lagi, menyimpan file dalam lingkungan multi-server dapat menyebabkan sulit untuk debug masalah. Jadi apa yang kita akan lakukan adalah untuk menyimpan baik sesi dan log file dalam cache dua tingkat, yang terdiri dari MySQL dan APC.

Untungnya, Yii Framework ditulis dalam cara yang sangat lurus ke depan dan dikonfigurasi, jadi ini tidak terlalu sulit untuk dilakukan. Terlebih lagi, masyarakat sekitar sangat sehat, sehingga ada banyak pilihan dan dukungan yang tersedia.

### 2.2 Auto-ajaib Tentukan Lingkungan dan Set Konfigurasi

Karena setiap lingkungan akan, mungkin, memiliki pengaturan konfigurasi yang berbeda, kami juga harus mampu membedakan antara mereka. Yii Kerangka melakukan hal ini di luar kotak, tapi itu dilakukan dengan menggunakan file bootstrap yang berbeda, seperti `` index.php``, `` indeks-test.php`` dan sebagainya.

Pada CloudKilat, sebuah aplikasi harus pemrograman tahu di mana itu dan mengatur pilihan konfigurasi yang sesuai. Dengan cara itu, kode Anda akan berjalan di setiap lingkungan. Jadi kita akan membuat penambahan kode sehingga hal ini terjadi auto-ajaib.

## 3. Masukan Control Kode bawah Git

Ok, sekarang mari kita mulai membuat perubahan ini dan menggunakan aplikasi. Kita akan mulai dengan meletakkan di bawah kontrol Git. Jadi jalankan perintah berikut untuk melakukannya:

    cd <directory Yii Kerangka Anda>

    git init.

    git add -A

    git commit -m "Selain Pertama sumber file"

Sekarang bahwa kode ini di bawah kontrol versi, kita akan membuat cabang pengujian juga, sehingga kita memiliki satu untuk menguji dengan dan satu untuk produksi. Jalankan perintah berikut dan itu akan dilakukan:

    git checkout pengujian -b

Jika Anda tidak terbiasa dengan Git, perintah sebelumnya akan checkout salinan cabang kami yang ada, menjadi cabang baru, yang disebut * pengujian *. Anda dapat mengkonfirmasi bahwa Anda sekarang memiliki dua cabang, dengan menjalankan perintah berikut:

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

Anda harus melihat output seperti di bawah ini:

    $ Ironcliapp APP_NAME / dorongan bawaan
    Menghitung objek: 2257, dilakukan.
    Delta kompresi menggunakan sampai 2 benang.
    Mengompresi objek: 100% (2131/2131), dilakukan.
    Menulis objek: 100% (2257/2257), 5.42 MiB | 117 KiB / s, dilakukan.
    Total 2257 (delta 735), kembali 0 (delta 0)

    >> Mendorong Menerima
    >> Kompilasi PHP
         INFO: Yii Kerangka terdeteksi
         INFO: ada '.ccconfig.yaml' ditemukan, pengaturan konten web untuk '/ testdrive'.
         INFO: direktori yang diperlukan hilang, menciptakan 'testdrive / protected / runtime'.
    >> Bangunan gambar
    >> Gambar Mengunggah (4,1 juta)

    Untuk ssh: //APP_NAME@kilatiron.net/repository.git
     * [Cabang baru] Master -> Master

## 4. Menginisialisasinya Diperlukan Add-ons

Sekarang itu selesai, kita perlu mengkonfigurasi dua add-ons, config dan mysqls. Config Add-on diperlukan untuk menentukan lingkungan aktif dan mysqls untuk menyimpan sesi dan login informasi.

### 4.1 Periksa Add-on Konfigurasi

Sekarang mari kita pastikan bahwa segala sesuatu adalah dalam rangka dengan memiliki melihat output konfigurasi add-on, dalam hal ini untuk pengujian. Untuk melakukan itu, jalankan perintah di bawah ini:

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

Sekarang kita perlu mengkonfigurasi config add-on dan menyimpan lingkungan masing-masing pengaturan di dalamnya. Jadi jalankan perintah berikut untuk melakukan hal ini:

    // Mengatur pengaturan lingkungan default
    ironapp APP_NAME / default config.add APPLICATION_ENV = utama

    // Mengatur pengaturan lingkungan pengujian
    ironapp APP_NAME / pengujian config.add APPLICATION_ENV = pengujian

Sekarang ini dilakukan, kita siap untuk membuat beberapa perubahan pada kode kita untuk menggunakan konfigurasi baru.

## 5. Konfigurasi lingkungan

### 5.1 Bootstrap

Secara default, Yii datang dengan file konfigurasi 3 bootstrap. Terletak di `` <yourapp> / protected / config`` file disebut `` console.php``, `` main.php`` dan `` test.php``. Mereka adalah konsol * *, * produksi * dan * pengujian * lingkungan konfigurasi, masing-masing.

Kita perlu mengubah mereka sedikit sehingga mereka akan melakukan apa yang kita butuhkan. Silahkan lihat pada kode di bawah ini yang akan menunjukkan, secara spesifik, apa yang kita perlu mengubah.

#### 5.1.1 Load Settings dari Lingkungan

Kami akan melihat secara khusus main.php di sini, tapi Anda dapat mengubah dua lainnya file juga. Kami menambahkan kode berikut ke atas file sehingga kita dapat mengakses `` variabel CRED_FILE``.

Ketika kami dikonfigurasi add ons sebelumnya (mysqls dan config) pengaturan yang secara otomatis bertahan dengan lingkungan server berjalan. Jadi kita sekarang dapat mengambil pengaturan ini dan mengkonfigurasi koneksi database kami untuk menggunakannya. Ini benar-benar berguna karena kita tidak perlu melakukan terlalu banyak untuk memanfaatkan opsi.

    if (! empty ($ _ SERVER ['HTTP_HOST']) && strpos ($ _ SERVER ['HTTP_HOST'], 'localdomain') === FALSE) {
        // Parse file json dengan addons kredensial
        $ String = file_get_contents ($ _ ENV ['CRED_FILE'], false);

        if ($ string == false) {
            die ('FATAL: Tidak dapat membaca berkas kredensial');
        }

        $ Creds = json_decode ($ string, true);
    }

#### 5.1.2 Mengaktifkan Konfigurasi Database MySQL

Dengan `` informasi CRED_FILE`` tersedia, kami menetapkan nama database, username, password dan nama host secara otomatis, seperti di bawah ini:

'Db' ​​=> array (
'ConnectionString' => 'mysql: host ='. $ Creds ["MYSQLS"] ["MYSQLS_HOSTNAME"]. '; Dbname ='. $ Creds ["MYSQLS"] ["MYSQLS_DATABASE"],
'EmulatePrepare' => benar,
'Username' => $ creds ["MYSQLS"] ["MYSQLS_USERNAME"],
'Password' => $ creds ["MYSQLS"] ["MYSQLS_PASSWORD"],
'Charset' => 'utf8',
),

#### 5.1.3 Aktifkan database Logging

Sekarang database dikonfigurasi, kita mengaktifkan logging, dengan kelas CDbLogRoute, menentukan connectionID bahwa dari database kami, 'db'. Sekarang, kami memiliki database logging diaktifkan dan siap.

Kami juga mengaktifkan `` autoCreateLogTable``, yang akan membuat tabel database penebangan secara otomatis bagi kita, jika kita tidak menginisialisasinya dengan skema SQL nanti. Kita akan, tapi cara ini, Anda melihat bahwa Anda tidak harus serta melihat skema tabel. Jadi cara mana Anda pergi, Anda tertutup.

Kami bertahan dengan tingkat standar penebangan kesalahan dan peringatan. Jadi jika Anda ingin memiliki tingkat lebih diaktifkan, ubah ini sesuai dengan kebutuhan Anda.

'Log' => array (
'Kelas' => 'CLogRouter',
'Rute' => array (
array (
'Kelas' => 'CDbLogRoute',
'AutoCreateLogTable' => 1,
'ConnectionID' => 'db',
'Tingkat' => 'kesalahan, peringatan',
),
),
),

Anda dapat mengetahui lebih lanjut tentang kelas di [dokumentasi online] (http://www.yiiframework.com/doc/api/1.1/CDbLogRoute/).

#### 5.1.4 Mengaktifkan Caching dengan APC

Untuk mengaktifkan caching aplikasi kami, kami kemudian mengaktifkan modul 'tembolok' menggunakan `` kelas system.caching.CApcCache``. Dengan melakukan ini, kami sudah siap untuk pergi.

'Cache' => array (
        'Kelas' => 'system.caching.CApcCache',
    ),

Anda dapat mengetahui lebih lanjut tentang kelas di [dokumentasi online] (http://www.yiiframework.com/doc/api/1.1/CApcCache).

### 5.3 index.php

Awalnya, `` index.php`` akan menggunakan `file konfigurasi main.php``` sebagai begitu:

    $ Config = dirname (__ FILE__). '/protected/config/main.php';

Tapi kita perlu mengubah bahwa berdasarkan lingkungan kita berada di, sebagaimana ditentukan oleh * APPLICATION_ENV * nilai yang kita set dengan konfigurasi add-on. Jadi perubahan `` index.php`` menjadi sebagai berikut:

    if (! empty ($ _ SERVER ['HTTP_HOST']) && strpos ($ _ SERVER ['HTTP_HOST'], 'localdomain') === FALSE) {
        // Parse file json dengan addons kredensial
        $ String = file_get_contents ($ _ ENV ['CRED_FILE'], false);

        if ($ string == false) {
            die ('FATAL: Tidak dapat membaca berkas kredensial');
        }

        $ Creds = json_decode ($ string, true);

        // Sekarang getenv ('APPLICATION_ENV') harus bekerja:
        $ EntryScript = $ creds ['CONFIG'] ['CONFIG_VARS'] ['APPLICATION_ENV'];

    } Else {
        $ EntryScript = 'pembangunan';
    }

    $ Config = dirname (__ FILE__). '/ Protected / config /'. $ EntryScript. 'Php';

Sekarang, `` index.php`` akan memuat file konfigurasi berdasarkan * APPLICATION_ENV * otomatis bagi kita.

## 6. Database Schema

Ok, kita perlu membuat skema database dasar untuk menyimpan baik sesi dan informasi log. Untuk menghemat waktu, tambahkan berikut ke file SQL yang disebut `` Yii Framework_CloudKilat_init.sql``, siap digunakan untuk menginisialisasi database berikutnya.

    -
    - Struktur tabel untuk tabel `YiiLog`
    -

    DROP TABLE IF EXISTS `YiiLog`;
    ! / * 40101 SETsaved_cs_client =@@character_set_client * /;
    ! / * 40101 SET character_set_client = utf8 * /;
    CREATE TABLE `YiiLog` (
      `Id` int (11) NOT NULL AUTO_INCREMENT,
      `Level` varchar (128) DEFAULT NULL,
      `Category` varchar (128) DEFAULT NULL,
      `Logtime` int (11) DEFAULT NULL,
      `Teks message`,
      PRIMARY KEY (`id`)
    ) ENGINE = MyISAM AUTO_INCREMENT = 2 DEFAULT CHARSET = latin1;
    ! / * 40101 SET character_set_client =saved_cs_client * /;

    -
    - Struktur tabel untuk tabel `tbl_user`
    -

    DROP TABLE IF EXISTS `tbl_user`;
    ! / * 40101 SETsaved_cs_client =@@character_set_client * /;
    ! / * 40101 SET character_set_client = utf8 * /;
    CREATE TABLE `tbl_user` (
      `Id` int (11) NOT NULL AUTO_INCREMENT,
      `Username` varchar (128) NOT NULL,
      `Password` varchar (128) NOT NULL,
      `Email` varchar (128) NOT NULL,
      PRIMARY KEY (`id`)
    ) ENGINE = MyISAM AUTO_INCREMENT = 22 DEFAULT CHARSET = latin1;
    ! / * 40101 SET character_set_client =saved_cs_client * /;

Sekarang, di shell, kita akan memuat data ke dalam mysql contoh remote yang kita buat sebelumnya. Untuk melakukannya, jalankan perintah berikut, mengubah pilihan masing-masing dengan pengaturan konfigurasi Anda, melakukan hal ini untuk kedua standar dan pengujian:

    mysql -u <database_username> p \
        h mysqlsdb.co8hm2var4k9.eu-west-1.rds.amazonaws.com \
        --ssl-ca = mysql-ssl-ca-cert.pem <database_name> <Yii Framework_CloudKilat_init.sql

Pada perintah di atas, Anda dapat melihat referensi ke pem ** berkas **.. Ini dapat didownload dari: [http://s3.amazonaws.com/rds-downloads/mysql-ssl-ca-cert.pem](http://s3.amazonaws.com/rds-downloads/mysql-ssl-ca-cert.pem). Semua yang baik, perintah akan selesai diam-diam, memuat data. Anda dapat memeriksa bahwa semua sudah pergi baik dengan perintah berikut:

    mysql -u <database_username> p \
        h mysqlsdb.co8hm2var4k9.eu-west-1.rds.amazonaws.com \
        --ssl-ca = mysql-ssl-ca-cert.pem <database_name>

    menampilkan tabel;

Ini akan menunjukkan tabel dari file SQL.

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

## 7. Tinjau Deployment yang

Dengan itu selesai, maka kita lihat baik penyebaran Anda untuk memastikan bahwa mereka bekerja.
Anda harus melihat output mirip dengan yang di bawah ini, pada gambar 2.

! [Deployment Sukses] (/ statis / apps / gambar / yii-kerangka-running.png)

### 7.1 Masalah Deployment

Jika Anda memiliki masalah penggelaran aplikasi Yii Framework, maka silakan baca file log. Ada, saat ini, dua tersedia, ini adalah ** menyebarkan ** dan ** ** kesalahan. Sebagai nama menyarankan, menyebarkan memberikan gambaran tentang proses penyebaran dan error menunjukkan kesalahan PHP setiap dan semua ke memperpanjang diperbolehkan oleh tingkat penebangan Anda saat ini.

Untuk melihat informasi ini, jalankan perintah berikut masing-masing:

#### 7.1.1 Deployment

    ironapp APP_NAME / default log menyebarkan

#### 7.1.1 Kesalahan

    ironapp APP_NAME / default error log

Perintah output informasi dalam [UNIX ekor] (http://en.wikipedia.org/wiki/Tail_%28Unix%29) seperti fashion. Jadi hanya memanggil mereka dan membatalkan memuji ketika Anda tidak lagi tertarik pada output.

## 8. Semua selesai

Dengan itu, Anda harus bangun dan berjalan, siap untuk membuat berikutnya, menakjubkan, aplikasi PHP web Anda, menggunakan Yii Kerangka. Jika Anda ingin menyelamatkan diri beberapa waktu, Anda dapat mengkloning salinan dimodifikasi sumber Yii Framework dari repositori CloudKilat Github. Jika Anda memiliki masalah apapun, jangan ragu untuk email [support@cloudkilat.com] (mailto: support@cloudkilat.com).

## Links

 * [Yii Kerangka] (http://www.yiiframework.com/)
 * [Yii di Wikipedia] (http://en.wikipedia.org/wiki/Yii)
 * [Yii di Twitter] (https://twitter.com/yiiframework)
 * [Belajar Yii dari Larry Ullman] (http://www.larryullman.com/series/learning-the-yii-framework/)
