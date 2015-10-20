#Deploying Kohana 3.2.0

! [Deployment Sukses] (/ statis / apps / gambar / Kohana-homepage.png)

Jika Anda sedang mencari yang sangat cepat, ringan, sangat dapat dikonfigurasi dan efektif Kerangka PHP untuk proyek-proyek Anda, tidak terlihat lagi dari [Kohana] (http://kohanaframework.org/). Sekarang di [versi 3.2.0] (http://dev.kohanaframework.org/attachments/download/1670/kohana-3.2.0.zip) datang dengan berbagai fitur untuk mempercepat pengembangan aplikasi Anda, termasuk:

 * Excellent debugging dan alat profiling
 * Sebuah lisensi distribusi yang fleksibel
 * Sebuah komunitas yang sangat aktif
 * Satu set inti perpustakaan
 * Kemampuan untuk dengan mudah menimpa dan memperluas perpustakaan inti
 * Kemampuan untuk menambahkan di perpustakaan pihak ke-3, seperti Zend Framework
 * Kaya [HMVC] (http://en.wikipedia.org/wiki/Hierarchical_model%E2%80%93view%E2%80%93controller) dukungan

Dalam tutorial ini, kita akan membawa Anda melalui penggelaran Kohana 3.2.0 untuk [platform CloudKilat] (http://www.cloudkilat.com/). Jika Anda membutuhkan informasi lebih lanjut tentang Kohana, periksa [panduan user online] (http://kohanaframework.org/documentation) atau melompat ke [saluran IRC] (irc: //irc.freenode.net/kohana). Jika tidak, mari kita mulai.

## Prasyarat

Anda akan hanya perlu beberapa hal untuk mengikuti bersama dengan tutorial ini. Ini adalah:

 * A [Git klien] (http://git-scm.com/), apakah baris perintah atau GUI.
 * Seorang klien MySQL, apakah baris perintah atau GUI, seperti [MySQL Workbench] (http://dev.mysql.com/downloads/workbench/) atau alat baris perintah.

## 1. Ambil Copy kode Kohana.

Sekarang bahwa Anda memiliki prasyarat di tempat, men-download salinan terbaru, stabil, rilis, 3.2.0 pada saat atau penerbitan. Anda dapat menemukannya di: [http://dev.kohanaframework.org/attachments/download/1670/kohana-3.2.0.zip](http://dev.kohanaframework.org/attachments/download/1670/kohana-3.2.0.zip). Setelah itu, ekstrak ke sistem file lokal Anda.

[Sumber file] (/ statis / apps / gambar / Kohana-files.png)

## 2. Mengubah Kode

Seperti yang saya sebutkan sebelumnya, beberapa perubahan perlu dibuat dengan konfigurasi default Kohana. Perubahan ini adalah sebagai berikut:

 1. Sesi Simpan dalam Database
 2. Auto-Ajaib Menentukan Lingkungan dan Set Konfigurasi

### 2.1 Toko Sesi di Database

Kita perlu melakukan ini karena Kohana, secara default, menyimpan file sesi pada filesystem. Namun, pendekatan ini tidak dianjurkan pada platform CloudKilat.

Terlebih lagi, menyimpan file dalam lingkungan multi-server dapat menyebabkan sulit untuk debug masalah. Jadi apa yang kita akan lakukan adalah untuk menyimpannya dalam database MySQL.

Untungnya, Kohana ditulis dalam cara yang sangat lurus ke depan dan dikonfigurasi, jadi ini tidak terlalu sulit untuk dilakukan. Terlebih lagi, masyarakat sekitar sangat sehat, sehingga ada banyak pilihan dan dukungan yang tersedia bila diperlukan.

### 2.2 Auto-Ajaib Menentukan Lingkungan dan Set Konfigurasi

Karena setiap lingkungan akan, mungkin, memiliki pengaturan konfigurasi yang berbeda, kami juga harus mampu membedakan antara mereka. Kohana tidak melakukan hal ini di luar kotak, tapi itu dilakukan dengan menggunakan file bootstrap yang berbeda, seperti ** index.php **, ** indeks-test.php ** dan sebagainya.

Pada CloudKilat, sebuah aplikasi harus pemrograman tahu di mana itu dan mengatur pilihan konfigurasi yang sesuai. Dengan cara itu, kode Anda akan berjalan di setiap lingkungan. Jadi kita akan membuat penambahan kode sehingga hal ini terjadi auto-ajaib.

## 3. Masukan Control Kode bawah Git

Ok, sekarang mari kita mulai membuat perubahan ini untuk aplikasi. Kita akan mulai dengan menempatkan [bawah kontrol Git] (http://git-scm.com/). Jadi jalankan perintah berikut untuk melakukannya:

    cd <directory Kohana Anda>

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

### 3.1 Kohana Auto-Terdeteksi

Ketika Anda melakukan ini, Anda akan melihat output yang mirip dengan berikut:

    $ Ironcliapp APP_NAME / dorongan bawaan
    Menghitung objek: 9, dilakukan.
    Delta kompresi menggunakan sampai 2 benang.
    Mengompresi objek: 100% (5/5), dilakukan.
    Menulis objek: 100% (5/5), 489 byte, dilakukan.
    Total 5 (delta 3), kembali 0 (delta 0)

    >> Mendorong Menerima
    remote: Tidak ada pemetaan submodule ditemukan di .gitmodules untuk jalur 'modul / Kohana-Cache'
    >> Kompilasi PHP
         INFO: Kohana Kerangka terdeteksi
         INFO: direktori yang diperlukan hilang, menciptakan 'aplikasi / cache'.
    >> Bangunan gambar
    >> Gambar Mengunggah (772K)

    Untuk ssh: //APP_NAME@kilatiron.net/repository.git
       Master f98a87c..a685cd6 -> Master

Perhatikan baris berikut:

    INFO: Kohana Kerangka terdeteksi
    INFO: direktori yang diperlukan hilang, menciptakan 'aplikasi / cache'.


## 4. Menginisialisasinya Addons Diperlukan

Sekarang itu selesai, kita perlu mengkonfigurasi dua addons, konfigurasi dan mysqls. Addon konfigurasi ini diperlukan untuk menentukan lingkungan aktif dan mysqls untuk menyimpan informasi sesi kami. Untuk menginisialisasi ini, jalankan perintah berikut dan membuat catatan dari output:

    // Menginisialisasinya addon mysqls.free untuk penyebaran standar
    ironapp APP_NAME / default addon.add mysqls.free

    // Menginisialisasinya addon mysqls.free untuk penyebaran pengujian
    ironapp APP_NAME / pengujian addon.add mysqls.free

Sekarang kita perlu mengkonfigurasi addon config dan menyimpan lingkungan masing-masing pengaturan di dalamnya. Jadi jalankan perintah berikut untuk melakukan hal ini:

    // Tambahkan variabel APPLICATION_ENV produksi
    ironapp APP_NAME / default config.add APPLICATION_ENV = produksi

    // Tambahkan variabel APPLICATION_ENV untuk pengujian
    ironapp APP_NAME / pengujian config.add APPLICATION_ENV = pengujian

### 4.1 Periksa Add-on Konfigurasi

Sekarang mari kita pastikan bahwa segala sesuatu adalah dalam rangka dengan memiliki melihat output konfigurasi add-on, dalam hal ini untuk pengujian. Untuk melakukan itu, jalankan perintah di bawah ini:

    // Ambil pengaturan
    ironapp APP_NAME / pengujian addon mysqls.free

Output dari perintah akan mirip dengan yang di bawah ini:

    Addon: alias.free

    Addon: mysqls.free

     Pengaturan
       MYSQLS_DATABASE: <database_name>
       MYSQLS_PASSWORD: <database_password>
       MYSQLS_PORT: 3306
       MYSQLS_HOSTNAME: mysqlsdb.co8hm2var4k9.eu-west-1.rds.amazonaws.com
       MYSQLS_USERNAME: <database_username>

    Addon: config.free

     Pengaturan
       CONFIG_VARS: {u'APPLICATION_ENV ': u'testing'}


Sekarang ini dilakukan, kita siap untuk membuat beberapa perubahan pada kode kita untuk menggunakan konfigurasi baru.

## 5. Konfigurasi lingkungan

Dalam `` aplikasi / bootstrap.php``, mencari baris berikut:

    if (isset ($ _ SERVER ['KOHANA_ENV']))
    {
        Kohana :: $ lingkungan = konstan ('Kohana ::' strtoupper ($ _ SERVER ['KOHANA_ENV']).);
    }

Setelah Anda telah menemukan itu, menggantinya dengan berikut ini. Aku akan pergi melalui kode sesudahnya.

    $ Env = Kohana :: PEMBANGUNAN;

    if (! empty ($ _ SERVER ['HTTP_HOST']) && strpos ($ _ SERVER ['HTTP_HOST'], 'localdomain') === FALSE) {
        // Parse file json dengan addons kredensial
        $ String = file_get_contents ($ _ ENV ['CRED_FILE'], false);

        if ($ string == false) {
            die ('FATAL: Tidak dapat membaca berkas kredensial');
        }

        $ Creds = json_decode ($ string, true);

        // Sekarang getenv ('APPLICATION_ENV') harus bekerja:
        $ Lingkungan = $ creds ['CONFIG'] ['CONFIG_VARS'] ['APPLICATION_ENV'];

        switch ($ lingkungan)
        {
            kasus ('menguji'):
               $ Env = Kohana :: PENGUJIAN;
            break;

            kasus ('pementasan'):
               $ Env = Kohana :: STAGING;
            break;

            kasus ('produksi'):
            default:
                $ Env = Kohana :: PRODUKSI;
        }
    }

    Kohana :: $ lingkungan = $ env;

Apa bahwa kode itu selesai untuk mengganti konfigurasi lingkungan asli dengan salah satu yang didasarkan pada melihat pengaturan yang terkandung dalam mandat CloudKilat mengajukan pengaturan, * APPLICATION_ENV *, yang kita tetapkan sebelumnya.

Anda akan melihat bahwa kita menggunakan konstanta lingkungan Kohana, yang dapat Anda temukan di `` / sistem / kelas / Kohana / core.php``. Dengan cara ini, kode dapat tetap konsisten sepanjang dan kita tidak menambahkan pada setiap kompleksitas yang tidak perlu atau menciptakan kembali roda.

Apa yang akan terjadi adalah bahwa jika kita berada dalam lingkungan pengembangan, ditentukan oleh "* localdomain *" berada di url, maka kita akan default ke pengaturan pembangunan. Jika tidak, kami akan mengambil * APPLICATION_ENV * nilai dan mencoba untuk mencocokkan terhadap konfigurasi lingkungan Kohana. Sekarang saya akan menunjukkan bagaimana kita menggunakan ini.

### 5.1 Inti Pengaturan Konfigurasi

Secara default, dalam aplikasi / bootstrap.php, Kohana memiliki konfigurasi sebagai berikut:

    Kohana :: modul (array (
    // 'Auth' => MODPATH.'auth ', // otentikasi dasar
    // 'Cache' => MODPATH.'cache ', // Caching dengan beberapa backends
    // 'Codebench' => MODPATH.'codebench ', // alat Benchmarking
    // 'Database' => MODPATH.'database ', // akses database
    // 'Image' => MODPATH.'image ', // manipulasi Gambar
    // 'Orm' => MODPATH.'orm ', // Pemetaan Obyek Hubungan
    // 'Unittest' => MODPATH.'unittest ', // Unit pengujian
    // 'Userguide' => MODPATH.'userguide ', // Buku petunjuk dan dokumentasi API
    ));

Pada dasarnya, apa artinya ini adalah bahwa tidak ada modul di atas dapat digunakan. Jadi kita akan perlu mengubah ini. Jadi dalam file bootstrap.php Anda, mengubahnya sebagai berikut:

    Kohana :: modul (array (
    // 'Auth' => MODPATH.'auth ', // otentikasi dasar
    'Cache' => MODPATH.'cache ', // Caching dengan beberapa backends
    // 'Codebench' => MODPATH.'codebench ', // alat Benchmarking
    'Database' => MODPATH.'database ', // akses database
    // 'Image' => MODPATH.'image ', // manipulasi Gambar
    // 'Orm' => MODPATH.'orm ', // Pemetaan Obyek Hubungan
    // 'Unittest' => MODPATH.'unittest ', // Unit pengujian
    // 'Userguide' => MODPATH.'userguide ', // Buku petunjuk dan dokumentasi API
    ));

Sekarang kita akan memiliki kedua cache dan modul database yang tersedia. Sebagai contoh aplikasi kami sederhana, ini semua kita harus mengaktifkan. Tinggalkan segala sesuatu yang lain dalam file seperti itu dan mari kita beralih ke caching.

### 5.2 Konfigurasi Caching

Buat file baru di bawah `` aplikasi / config`` disebut `` cache.php``. Dalam file tersebut, tambahkan kode berikut:

    <? Php didefinisikan ('SYSPATH') atau mati ("Tidak ada akses skrip langsung. ');

    return array
    (
        // Override konfigurasi default
        'Apc' => array
        (
            'Driver' => 'apc',
            'Default_expire' => 3600,
        ),
    );

Apa yang dilakukan adalah untuk memberitahu Kohana bahwa cache akan menggunakan APC sebagai backend, yang CloudKilat memberikan keluar dari kotak dan menetapkan periode berakhirnya standar menjadi ** 3600 ** detik, atau 60 menit ** **.

### 5.3 Konfigurasi Koneksi database

Buat file baru di bawah `` aplikasi / config`` disebut `` database.php``. Dalam file tersebut, tambahkan kode berikut:

    <? Php

    // Menimpa pengaturan inti jika kita tidak dalam lingkungan pembangunan daerah
    if (! empty ($ _ SERVER ['HTTP_HOST']) && strpos ($ _ SERVER ['HTTP_HOST'], 'localdomain') === FALSE) {

        // Membaca kredensial berkas
        $ String = file_get_contents ($ _ ENV ['CRED_FILE'], false);
        if ($ string == false) {
            die ('FATAL: Tidak dapat membaca berkas kredensial');
        }

        // File berisi string JSON, decode dan mengembalikan array asosiatif
        $ Creds = json_decode ($ string, true);

        return array
        (
            'Default' => array
            (
                'Jenis' => 'mysql',
                'Koneksi' => array (
                    'Hostname' => $ creds ["MYSQLS"] ["MYSQLS_HOSTNAME"],
                    'Username' => $ creds ["MYSQLS"] ["MYSQLS_USERNAME"],
                    'Password' => $ creds ["MYSQLS"] ["MYSQLS_PASSWORD"],
                    'Gigih' => SALAH,
                    'Database' => $ creds ["MYSQLS"] ["MYSQLS_DATABASE"],
                ),
                'Table_prefix' => '',
                'Charset' => 'utf8',
                'Profiling' => TRUE,
            ),
        );
    } Else {
        return array
        (
            'Default' => array
            (
                'Jenis' => 'mysql',
                'Koneksi' => array (
                    'Hostname' => 'localhost',
                    'Username' => 'cc_dev',
                    'Password' => 'cc_dev',
                    'Gigih' => SALAH,
                    'Database' => 'CloudKilat_kohana',
                ),
                'Table_prefix' => '',
                'Charset' => 'utf8',
                'Profiling' => TRUE,
            ),
        );
    }

Ketika kita mengkonfigurasi add ons sebelumnya (* mysqls * dan * config *) pengaturan secara otomatis bertahan dengan lingkungan server berjalan. Jadi kita sekarang dapat mengambil pengaturan ini, ketika kita tidak dalam lingkungan pembangunan daerah, dan mengkonfigurasi koneksi database kami untuk menggunakannya. Ini benar-benar berguna karena kita tidak perlu melakukan terlalu banyak untuk memanfaatkan opsi.

### 5.4 Konfigurasi Session

Buat file baru di bawah `` aplikasi / config`` disebut `` session.php``. Dalam file tersebut, tambahkan kode berikut:

    <? Php

        // Mengkonfigurasi sistem untuk menyimpan sesi dalam database
        kembali array (
            'Database' => array (
                'Nama' => 'test_cookie',
                'Dienkripsi' => TRUE,
                'Seumur hidup' => 43200,
                'Kelompok' => 'default',
                'Table' => 'sesi',
                'Kolom' => array (
                    'Session_id' => 'session_id',
                    'Last_active' => 'last_active',
                    'Isi' => 'isi'
                ),
                'Gc' => 500,
            ),
        );

Apa yang dilakukan adalah untuk mengatakan bahwa informasi sesi akan disimpan dalam database. Ini akan disimpan dalam sesi tabel yang disebut dan memiliki seumur hidup ** 43.200 detik **. Anda dapat membaca tentang pengaturan lain di [dokumentasi sesi secara online] (http://kohanaframework.org/3.0/guide/kohana/sessions).

Jadi `` direktori aplikasi / config`` Anda akan terlihat seperti itu di bawah:

! [Deployment Sukses] (/ statis / apps / gambar / Kohana aplikasi-config dir.png)

## 6. Database Schema

Ok, setelah semua ini dilakukan, kita perlu memuat skema database di setiap lingkungan kita yang kita setup sebelumnya di add-ons. Skema yang kita akan memuat bawah:

    DROP TABLE IF EXISTS `sessions`;
    ! / * 40101 SETsaved_cs_client =@@character_set_client * /;
    ! / * 40101 SET character_set_client = utf8 * /;
    CREATE TABLE `sessions` (
      `Session_id` varchar (24) NOT NULL,
      `Last_active` int (10) unsigned NOT NULL,
      `Teks contents` NOT NULL,
      PRIMARY KEY (`session_id`),
      KEY `last_active` (` last_active`)
    ) ENGINE = MyISAM DEFAULT CHARSET = latin1;
    ! / * 40101 SET character_set_client =saved_cs_client * /;

    -
    - Dumping data untuk tabel `sessions`
    -

    LOCK TABLES `MENULIS sessions`;
    ! / * 40000 ALTER TABLE `sessions` DISABLE KEYS * /;
    ! / * 40000 ALTER TABLE `sessions` ENABLE KEYS * /;
    UNLOCK TABLES;

    -
    - Struktur tabel untuk tabel `tblUsers`
    -

    DROP TABLE IF EXISTS `tblUsers`;
    ! / * 40101 SETsaved_cs_client =@@character_set_client * /;
    ! / * 40101 SET character_set_client = utf8 * /;
    CREATE TABLE `tblUsers` (
      `Id` int (11) unsigned NOT NULL AUTO_INCREMENT,
      `FirstName` varchar (100) NOT NULL,
      `LastName` varchar (100) NOT NULL,
      `EmailAddress` varchar (200) NOT NULL,
      PRIMARY KEY (`id`)
    ) ENGINE = MyISAM AUTO_INCREMENT = 5 DEFAULT CHARSET = latin1;
    ! / * 40101 SET character_set_client =saved_cs_client * /;

    -
    - Dumping data untuk tabel `tblUsers`
    -

    LOCK TABLES `tblUsers` MENULIS;
    ! / * 40000 ALTER TABLE `tblUsers` DISABLE KEYS * /;
    INSERT INTO `NILAI tblUsers` (1,'matthew','setter','ms@example.com'),(2,'don','bradman','db@example.com'),(3,'alan','border','ab@example.com');
    ! / * 40000 ALTER TABLE `tblUsers` ENABLE KEYS * /;
    UNLOCK TABLES;

Apa yang kita miliki adalah skema MySQL sederhana yang menciptakan dua meja, satu untuk menyimpan informasi sesi ** ** dan satu untuk menyimpan pengguna ** **, yang kita akan menggunakan di sederhana contoh aplikasi pengendali dan tampilan berikutnya.

Kami juga memuat dalam beberapa pengguna ke meja pengguna seperti yang kita tidak akan membuat dan bentuk untuk mengelola informasi di sana, tetapi ingin memiliki sesuatu untuk melihat untuk mengkonfirmasi itu bekerja. Jadi menyimpan skema dalam file bernama `` kohana_CloudKilat_init.sql``.

Sekarang, di shell, kita akan memuat skema ke dalam mysql contoh remote yang kita buat sebelumnya dengan mysqls add-on. Untuk melakukannya, jalankan perintah berikut, mengubah pilihan masing-masing dengan pengaturan konfigurasi Anda:

    mysql -u <database_username> p \
        h mysqlsdb.co8hm2var4k9.eu-west-1.rds.amazonaws.com \
        --ssl-ca = mysql-ssl-ca-cert.pem <database_name> <kohana_CloudKilat_init.sql

Pada perintah di atas, Anda dapat melihat referensi ke pem ** berkas **.. Ini dapat didownload dari: [http://s3.amazonaws.com/rds-downloads/mysql-ssl-ca-cert.pem](http://s3.amazonaws.com/rds-downloads/mysql-ssl-ca-cert.pem). Semua yang baik, perintah akan selesai diam-diam, memuat data. Anda dapat memeriksa bahwa semua sudah pergi baik dengan perintah berikut:

    mysql -u <database_username> p \
        h mysqlsdb.co8hm2var4k9.eu-west-1.rds.amazonaws.com \
        --ssl-ca = mysql-ssl-ca-cert.pem <database_name>

    menampilkan tabel;

Ini harus menunjukkan output yang Anda mirip dengan di bawah:

    Ketik 'bantuan;' atau '\ h' untuk membantu. Ketik '\ c' untuk menghapus pernyataan masukan saat ini.

    mysql> show tables;
    + ----------------------- +
    | Tables_in_dep5jrx8a9a |
    + ----------------------- +
    | Sesi |
    | TblUsers |
    + ----------------------- +
    2 rows in set (0.06 sec)


## Komit dan Push Kode

Setelah semua perubahan ini dilakukan, kita perlu kemudian melakukan mereka di cabang master dan menggabungkan perubahan ke cabang percobaan, kita buat sebelumnya. Untuk tetap sederhana, Anda dapat menjalankan perintah di bawah ini:

    git add aplikasi / config / database.php
    git add aplikasi / config / cache.php
    git add aplikasi / config / session.php
    git add aplikasi / bootstrap.php
    git komit -m "Diperbarui mengaktifkan cache, basis data sesi dan auto-menentukan lingkungan"
    pengujian checkout git
    Master menggabungkan git

    // Mendorong kode untuk default (produksi) cabang
    ironapp APP_NAME / dorongan bawaan
    ironapp APP_NAME / default menyebarkan

    // Mendorong kode untuk cabang pengujian
    ironapp APP_NAME / pengujian dorongan
    ironapp APP_NAME / pengujian menyebarkan

## A Aplikasi Sederhana

Sekarang kita akan membangun sebuah aplikasi yang sangat sederhana untuk menguji konfigurasi baru dan penyebaran. Ini akan hanya memiliki satu kontroler dan melihat. Dalam controller kita akan:

 * Dapatkan pegangan pada database kami dan melakukan beberapa SQL dasar
 * Membuat, menyimpan dan memanipulasi objek sederhana dalam cache

Di bawah `` aplikasi / kelas / controller`` membuat file kontroler baru yang disebut `` hello.php``, yang berisi kode berikut:

    <? Php didefinisikan ('SYSPATH') ATAU mati ('Tidak Langsung Script Access');

    Kelas Controller_Hello meluas Controller_Template
    {
        $ publik Template = 'situs';

        fungsi publik action_index ()
        {
            $ This-> template-> Pesan = 'hello, world!';

            // Mengubah driver Cache default memcache
            Cache :: $ default = 'apc';

            // Buat contoh baru cache menggunakan grup standar
            $ Cache = Cache :: contoh ();

            // Buat objek cachable
            $ Object = stdClass baru;

            // Set properti
            $ Object> foo = 'bar';

            // Cache objek menggunakan grup standar (antarmuka cepat) dengan waktu default (3600 detik)
            Cache :: contoh () -> set ('foo', $ object);

            mencetak "item disimpan dalam cache";

            // Jika tombol cache yang tersedia (dengan nilai default diatur ke FALSE)
            if ($ object = Cache :: contoh () -> get ('foo', FALSE)) {
                 print "<br /> Diperoleh item dari cache";
            } Else {
                 print "<br /> Tidak mengambil item dari cache";
            }

            // Jika entri cache untuk 'foo' dihapus
            if (Cache :: misalnya () -> menghapus ('foo')) {
                print '<br /> Dihapus modul';
            }

            $ Hasil = DB :: pilih ('id', 'EmailAddress') -> dari ('tblUsers') -> execute ();
            $ This-> template-> pengguna = $ hasil-> as_array ();
        }
    }

Apa yang dilakukan adalah untuk memberitahu controller yang kita akan menggunakan tampilan template, yang disebut `` site.php`` yang akan kita buat berikutnya. Kami kemudian mendapatkan pegangan pada konfigurasi database, berdasarkan lingkungan kita secara otomatis ditentukan dan menggunakan cache dikonfigurasi.

Setelah itu, kita membuat sebuah objek yang kita gunakan untuk memanipulasi cache dan mencetak output yang menunjukkan bagaimana ini bekerja, atau tidak. Setelah ini, kita pilih `` `` id`` dan emailAddress`` dari tabel: `` tblUsers`` dan kembali informasi sebagai array sederhana dan menetapkan nilai ke variabel template yang disebut pengguna.

## The View Template

Sekarang membuat file bernama `` `` site.php`` bawah aplikasi / views / ``. Di dalamnya, tambahkan kode berikut:

    <Html>
        <Head>
            <Title> Kami punya pesan untuk Anda! </ Title>
            <Style type = "text / css">
                body {font-family: Georgia;}
                h1 {font-style: italic;}

            </ Style>
        </ Head>
        <Body>
            <H1> Selamat Datang di Kohana di CloudKilat </ h1>
            ? <P> <? Php echo $ pesan; ?> </ P>
            <Table cellspacing = "2" cellpadding = "4" border = "2" width = "100%">
                <Tr>
                    <Th> Pengguna ID </ th>
                    <Th> User Email </ th>
                </ Tr>
            <? Php foreach ($ pengguna sebagai $ user):?>
                <Tr>
                    ? <Td> <php print $ user ['id']; ?> </ Td>
                    <Td> <php print $ user ['EmailAddress']; ?> </ Td>
                </ Tr>
            ? <Endforeach php; ?>
            </ Table>
        </ Body>
    </ Html>

Dalam pandangan file ini, kami output beberapa HTML sederhana dan kemudian iterate nilai pengguna yang kita diambil di controller sebelumnya. Arahkan browser Anda ke `APP_NAME.kilatiron.net / hello.php` untuk melihat hasilnya.

## 7. Tinjau Deployment yang

Setelah ini, menambahkan file ke git dan komit mereka dan mendorong / menyebarkan perubahan untuk kedua lingkungan. Dari sana Anda dapat meninjau pengujian dan produksi penyebaran untuk memastikan bahwa mereka bekerja juga.

Dengan itu, Anda harus bangun dan berjalan, siap untuk membuat berikutnya, menakjubkan, aplikasi PHP web Anda, menggunakan Kohana dan CloudKilat. Jika Anda memiliki masalah apapun, jangan ragu untuk email [support@cloudkilat.com] (mailto: support@cloudkilat.com).
