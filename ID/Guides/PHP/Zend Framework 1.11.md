#Deploying Zend Framework 1.11 ke CloudKilat

! [Deployment Sukses] (/ statis / apps / gambar / ZendFramework-logo.png)

Jika Anda sedang mencari kaya fitur, Kerangka PHP fleksibel dan mampu untuk proyek-proyek Anda, Anda tidak dapat melewati [Zend Framework] (http://framework.zend.com/). Sekarang di [versi 1.11] (http://framework.zend.com/download/latest) datang dengan berbagai fitur untuk mempercepat pengembangan aplikasi Anda, termasuk:

 * Pendekatan MVC Lurus ke depan
 * Sebuah besar, berkembang, masyarakat
 * Sub-perpustakaan komponen untuk bekerja dengan berbagai layanan, termasuk PayPal dan Google
 * Mudah untuk membaca dokumentasi
 * A super, mengkilap, baru versi 2 ** segera hadir **

Dalam tutorial ini, kita akan membawa Anda melalui penggelaran Zend Framework v1.11 untuk [platform CloudKilat] (http://www.cloudkilat.com/).

## Prasyarat

Anda akan hanya perlu beberapa hal untuk mengikuti bersama dengan tutorial ini. Ini adalah:

 * A [Git klien] (http://git-scm.com/), apakah baris perintah atau GUI.
 * Seorang klien MySQL, apakah baris perintah atau GUI, seperti [MySQL Workbench] (http://dev.mysql.com/downloads/workbench/) atau alat baris perintah.

## 1. Ambil Copy Zend Framework

Jadi sekarang bahwa Anda memiliki prasyarat di tempat, men-download salinan terbaru, stabil, rilis. Anda dapat menemukannya di: [http://framework.zend.com/download/latest](http://framework.zend.com/download/latest). Setelah itu, ekstrak ke sistem file lokal.

[Sumber file] (/ statis / apps / gambar / zf-sumber-files.png)

## Buat Aplikasi Dasar

Setelah itu, membuat aplikasi dasar dengan menggunakan zf.sh (atau bat) sebagai berikut:

    zf.sh membuat proyek uji-aplikasi
    zf.sh memungkinkan tata letak

Dengan itu, Anda akan memiliki aplikasi dasar yang dapat dijalankan di VHost dasar.

## 2. Mengubah Kode

Seperti yang saya sebutkan sebelumnya, beberapa perubahan perlu dibuat untuk konfigurasi aplikasi default dan kode untuk mengakomodasi penyebaran CloudKilat. Perubahan ini adalah sebagai berikut:

 * Sesi Store dan log file dalam database, bukan pada filesystem
 * Auto-ajaib menentukan lingkungan dan mengatur konfigurasi

### 2.1 Toko Session dan Log File di Database, Bukan pada Filesystem yang

Menyimpan file dalam lingkungan multi-server dapat menyebabkan sulit untuk debug masalah. Jadi apa yang kita akan lakukan adalah untuk menyimpan baik sesi dan log file dalam cache dua tingkat, yang terdiri dari MySQL dan APC.

Untungnya, Zend Framework ditulis dalam cara yang sangat lurus ke depan dan dikonfigurasi, jadi ini tidak terlalu sulit untuk dilakukan. Terlebih lagi, masyarakat sekitar sangat sehat, sehingga ada banyak pilihan dan dukungan yang tersedia.

### 2.2 Auto-ajaib Tentukan Lingkungan dan Set Konfigurasi

Jika kita menyebarkan aplikasi secara sederhana, VHost dikonfigurasi, maka kita akan mengatur variabel lingkungan, seperti `` APPLICATION_ENV`` di lingkungan webserver, yang kemudian dibuat tersedia untuk PHP, mirip dengan:

    <VirtualHost *: 80>
        Pengembangan SetEnv APPLICATION_ENV
    </ VirtualHost>

Bila menggunakan CloudKilat, kita perlu melakukan ini dengan cara yang sedikit berbeda. Ini melibatkan, untuk setiap lingkungan, pengaturan variabel lingkungan dengan konfigurasi ironapp add-on, kemudian membaca informasi dari lingkungan saat runtime. Kami akan datang ke ini tak lama.

## 3. Masukan Control Kode bawah Git

Ok, sekarang mari kita mulai membuat perubahan ini dan menggunakan aplikasi. Kita akan mulai dengan meletakkan di bawah kontrol Git. Jadi jalankan perintah berikut untuk melakukannya:

    cd <directory Zend Framework Anda>

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

Anda akan melihat output yang mirip dengan berikut:

    $ Ironcliapp APP_NAME / pengujian dorongan
    Total 0 (delta 0), kembali 0 (delta 0)

    >> Mendorong Menerima
    >> Kompilasi PHP
         INFO: Zend Framework 1.x terdeteksi
    >> Bangunan gambar
    >> Gambar Mengunggah (3.6M)

    Untuk ssh: //APP_NAME@kilatiron.net/repository.git
       pengujian dde253a..7b040e2 -> pengujian

## 4. Menginisialisasinya Diperlukan Add-ons

Sekarang itu selesai, kita perlu mengkonfigurasi dua add-ons, config dan mysqls. Config add-on diperlukan untuk menentukan lingkungan aktif dan mysqls untuk menyimpan sesi dan login informasi.

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
    ironapp APP_NAME / default config.add APPLICATION_ENV = produksi

    // Mengatur pengaturan lingkungan pengujian
    ironapp APP_NAME / pengujian config.add APPLICATION_ENV = pengujian

Sekarang ini dilakukan, kita siap untuk membuat beberapa perubahan pada kode kita untuk menggunakan konfigurasi baru.

## 5. Konfigurasi lingkungan

### 5.1 application.ini

Dalam file ini, di bawah, kita sudah diletakkan konfigurasi inti, yang akan diwarisi oleh semua lingkungan secara default. Anda dapat melihat bahwa params telah dikosongkan. Hal ini karena mereka diperlukan. Tapi Anda akan melihat di sumber daya Plugin, kemudian, bahwa kita mengatur mereka secara tepat.

    [Produksi]
    phpSettings.display_startup_errors = 1
    phpSettings.display_errors = 1
    resources.frontController.params.displayExceptions = 1

#### 5.1.1 database

    ; Mengkonfigurasi pengaturan database
    resources.db.adapter = PDO_MYSQL
    resources.db.isDefaultTableAdapter = true
    resources.db.params.host =
    resources.db.params.username =
    resources.db.params.password =
    resources.db.params.dbname =

#### 5.1.2 Penyimpanan Sesi

Pada bagian bawah, kita sudah dikonfigurasi sesi yang akan disimpan dengan [Zend_Session_SaveHandler_DbTable](http://framework.zend.com/manual/en/zend.session.savehandler.dbtable.html) kelas. Tabel skema akan diberikan segera. Ini menggunakan database adapter default untuk menyambung ke database, sehingga tidak ada konfigurasi lebih lanjut akan diperlukan pada bagian kami untuk membuat pekerjaan ini dengan benar.

    ; Konfigurasi Zend_Session_SaveHandler_DbTable:
    resources.session.savehandler.class = "Zend_Session_SaveHandler_DbTable"
    resources.session.savehandler.options.name = "sesi"
    resources.session.savehandler.options.primary = "id"
    resources.session.savehandler.options.modifiedColumn = "dimodifikasi"
    resources.session.savehandler.options.dataColumn = "data"
    resources.session.savehandler.options.lifetimeColumn = "seumur hidup"

#### 5.1.3 Caching

Pada bagian bawah, kami sudah menyiapkan opsi Cache sederhana yang dapat kita gunakan dalam aplikasi. Ini memiliki seperangkat sederhana pilihan frontend dan menggunakan APC sebagai backend. Sesuai manual, kami juga bisa menyimpan informasi di backend, tetapi untuk keperluan tutorial ini, APC akan bekerja dengan baik.

    ; Mengkonfigurasi opsi frontend inti caching
    resources.cachemanager.general.frontend.name = Inti
    resources.cachemanager.general.frontend.options.caching = true
    resources.cachemanager.general.frontend.options.cache_id_prefix = NULL
    resources.cachemanager.general.frontend.options.lifetime = 3600
    resources.cachemanager.general.frontend.options.logging = false
    resources.cachemanager.general.frontend.options.write_control = true
    resources.cachemanager.general.frontend.options.automatic_serialization = true
    resources.cachemanager.general.frontend.options.automatic_cleaning_factor = 10
    resources.cachemanager.general.frontend.options.ignore_user_abort = false

    ; Mengkonfigurasi APC Cache sederhana
    resources.cachemanager.general.backend.name = Apc

### 5.2 Bootstrap Plugin Sumber Daya

Ok, mari kita mulai melihat ke sumber bootstrap plugin yang akan membantu kita menyelesaikan setup aplikasi kita.

#### 5.2.1 database

Dalam konfigurasi database, jika kita tidak ** ** di lingkungan pembangunan daerah, kita perlu berkonsultasi dengan `` variabel CRED_FILE``, tersedia di semua lingkungan CloudKilat, untuk pilihan untuk mysql.

Ketika kami dikonfigurasi add ons sebelumnya (** mysqls ** dan ** config **) pengaturan yang secara otomatis bertahan dengan lingkungan server berjalan. Jadi kita sekarang dapat mengambil pengaturan ini dan mengkonfigurasi koneksi database kami untuk menggunakan mereka.

Ini benar-benar berguna karena kita tidak perlu melakukan terlalu banyak untuk memanfaatkan opsi. Untuk melakukan hal ini, kita mendapatkan pegangan pada adaptor database yang sebagian-dikonfigurasi dan melengkapi pengaturan dengan memanggil `` metode setParams``. Silahkan lihat melalui kode untuk sumber daya di bawah ini untuk melihat cara kerjanya.

        Fungsi dilindungi _initDb ()
        {
            //
            // Dapatkan pegangan pada sumber daya yang ada Plugin db
            //
            $ DbPluginResource = $ this-> getPluginResource ('db');

            jika (APPLICATION_ENV! == 'pembangunan') {

                //
                // Baca kredensial lingkungan berkas
                //
                $ String = file_get_contents ($ _ ENV ['CRED_FILE'], false);
                if ($ string == false) {
                    die ('FATAL: Tidak dapat membaca berkas kredensial');
                }

                //
                // Decode mereka dari JSON
                //
                $ Creds = json_decode ($ string, true);

                //
                // Mengatur pengaturan database hilang dengan pilihan diambil.
                //
                $ DbPluginResource-> setParams (array (
                    'Dbname' => $ creds ["MYSQLS"] ["MYSQLS_DATABASE"],
                    'Host' => $ creds ["MYSQLS"] ["MYSQLS_HOSTNAME"],
                    'Username' => $ creds ["MYSQLS"] ["MYSQLS_USERNAME"],
                    'Password' => $ creds ["MYSQLS"] ["MYSQLS_PASSWORD"],
                ));
            }

            //
            // Mengatur modus mengambil dan menyimpan sumber daya di registri app
            //
            if (! is_null ($ dbPluginResource)) {
                $ Db = $ this-> getPluginResource ('db') -> getDbAdapter ();
                $ Db-> setFetchMode (Zend_Db :: FETCH_OBJ);
                Zend_Db_Table :: setDefaultAdapter ($ db);
                Zend_Registry :: set ('db', $ db);
                kembali $ db;
            }

            kembali SALAH;
        }

Dengan ini, kita akan memiliki konfigurasi database bekerja.

#### 5.2.2 Logging

Meskipun fleksibilitas Zend Framework application.ini berkas, tidak memiliki dukungan untuk mengkonfigurasi database berbasis logging. Jadi yang perlu kita lakukan dalam sumber daya Plugin. Anda dapat lihat di bawah yang kita dapatkan adaptor aplikasi database dan kemudian menggunakannya ketika inisialisasi baru [Zend_Log_Writer_Db](http://framework.zend.com/manual/en/zend.log.writers.html#zend.log.writers.database) kelas.

        Fungsi dilindungi _initLog ()
        {
            //
            // Dapatkan pegangan pada sumber daya Plugin db
            //
            $ DbPluginResource = $ this-> getPluginResource ('db');

            if (! is_null ($ dbPluginResource)) {
                $ Db = $ this-> getPluginResource ('db') -> getDbAdapter ();

                //
                // Buat penulis Zend_Log_Writer_Db baru
                //
                $ DbWriter = Zend_Log_Writer_Db baru ($ db, 'log_table');

                //
                // Daftar dengan logger
                //
                $ Logger = baru Zend_Log ($ dbWriter);

                //
                // Store yang di registri
                //
                Zend_Registry :: set ('logger', $ logger);
                kembali $ logger;
            }
        }

#### 5.2.3 Caching

Ini lebih dari sebuah metode utilitas, untuk kenyamanan dalam aplikasi. Kami membuat umum `` objek cachemanager`` kita dijalankan dalam file application.ini sebelumnya tersedia melalui sumber daya Plugin.

    Fungsi dilindungi _initCache ()
    {
        try {
            $ BootstrapCacheMgr = $ this-> bootstrap ('cachemanager');
        } Catch (Zend_Application_Bootstrap_Exception $ e) {
            // Log kesalahan ...
        }

        if (! empty ($ bootstrapCacheMgr) && $ bootstrapCacheMgr instanceof
            Zend_Application_Bootstrap_BootstrapAbstract &&
            $ BootstrapCacheMgr-> hasResource ('cachemanager'))
        {

            //
            // Dapatkan pegangan pada manajer cache yang ada
            //
            $ CacheManager = $ bootstrapCacheMgr-> getResource ('cachemanager');
            $ GeneralCache = 'umum';

            if ($ cacheManager-> hasCache ($ generalCache)) {
                $ Cache = $ cacheManager-> getCache ($ generalCache);
                // Hanya mencoba untuk cache metadata jika kita memiliki cache yang tersedia
                if (! empty ($ cache)) {
                    try {
                        Zend_Registry :: set ('Cache', $ cache);
                        kembali $ cache;
                    } Catch (Zend_Db_Table_Exception $ e) {
                        // Log kesalahan ...
                    }
                }
            }
        }
    }

### 5.3 index.php

Kami kemudian harus melakukan penyesuaian ke default `` index.php`` yang datang dengan instalasi Zend Framework standar, seperti yang dibuat oleh `` zf.sh`` (atau kelelawar).

Biasanya terlihat seperti di bawah ini (diformat untuk dibaca):

    // Tentukan path ke direktori aplikasi
    didefinisikan ('APPLICATION_PATH')
        || Define ('APPLICATION_PATH', realpath (dirname (__ FILE__) '/../application').);

    // Tentukan path ke direktori aplikasi
    didefinisikan ('PROJECT_PATH')
        || Define ('PROJECT_PATH', realpath (dirname (__ FILE__) '/../').);

    // Tentukan lingkungan aplikasi
    didefinisikan ('APPLICATION_ENV')
        || Define ('APPLICATION_ENV',
            (Getenv ('APPLICATION_ENV')
            ? getenv ('APPLICATION_ENV'): 'produksi'));

Namun, kita perlu membuat perubahan kecil, seperti yang disorot di bawah:

    // Tentukan path ke direktori aplikasi
    didefinisikan ('APPLICATION_PATH')
        || Define ('APPLICATION_PATH', realpath (dirname (__ FILE__) '/../application').);

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
               define ('APPLICATION_ENV', 'pengujian');
            break;

            kasus ('pementasan'):
               define ('APPLICATION_ENV', 'pementasan');
            break;

            kasus ('produksi'):
            default:
                define ('APPLICATION_ENV', 'produksi');
        }
    } Else {
        define ('APPLICATION_ENV', 'pembangunan');
    }

    // Tentukan lingkungan aplikasi
    didefinisikan ('APPLICATION_ENV') || define ('APPLICATION_ENV', 'produksi');

Apa yang dilakukan adalah dengan menggunakan `` pengaturan CRED_FILE`` yang kita dikonfigurasi sebelumnya untuk membantu kami menentukan lingkungan, `` APPLICATION_ENV``, bahwa aplikasi yang beroperasi dalam.

## 6. Database Schema

Ok, kita perlu membuat skema database dasar untuk menyimpan baik sesi dan informasi log. Untuk menghemat waktu, tambahkan berikut ke file SQL yang disebut `` zendframework_CloudKilat_init.sql``, siap digunakan untuk menginisialisasi database berikutnya.

    - Struktur tabel
    CREATE TABLE `session` (
      `Id` Char (32),
      `Modified` int,
      `Lifetime` int,
      `Data` teks,
      PRIMARY KEY (`id`)
    );

    CREATE TABLE `log_table` (
        `Varchar priority` (50) NULL default,
        `Message` NULL varchar (100) default,
        `Timestamp timestamp` NOT NULL CURRENT_TIMESTAMP default pada pembaruan CURRENT_TIMESTAMP,
        `Varchar priorityName` (40) NULL standar
    ) ENGINE = InnoDB DEFAULT CHARSET = latin1;

    CREATE TABLE `tbl_users` (
        `First` NULL varchar (50) default,
        `Varchar last` (100) NULL default,
        `Username` varchar (40) NULL standar
    ) ENGINE = InnoDB;

    - Menambahkan beberapa catatan
    INSERT INTO tbl_users (pertama, terakhir, nama) VALUES ('matthew', 'setter', 'settermjd');

Sekarang, di shell, kita akan memuat data ke dalam mysql contoh remote yang kita buat sebelumnya. Untuk melakukannya, jalankan perintah berikut, mengubah pilihan masing-masing dengan pengaturan konfigurasi Anda, melakukan hal ini untuk kedua standar dan pengujian:

    mysql -u <database_username> p \
        h mysqlsdb.co8hm2var4k9.eu-west-1.rds.amazonaws.com \
        --ssl-ca = mysql-ssl-ca-cert.pem <database_name> <zendframework_CloudKilat_init.sql

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

! [Deployment Sukses] (/ statis / apps / gambar / zf-deployed.png)

### 7.1 Masalah Deployment

Jika Anda memiliki masalah penggelaran aplikasi Zend Framework, maka silakan baca file log. Ada, saat ini, dua tersedia, ini adalah ** menyebarkan ** dan ** ** kesalahan. Sebagai nama menyarankan, menyebarkan memberikan gambaran tentang proses penyebaran dan error menunjukkan kesalahan PHP setiap dan semua ke memperpanjang diperbolehkan oleh tingkat penebangan Anda saat ini.

Untuk melihat informasi ini, jalankan perintah berikut masing-masing:

#### 7.1.1 Deployment

    ironapp APP_NAME / default log menyebarkan

#### 7.1.1 Kesalahan

    ironapp APP_NAME / default error log

Perintah output informasi dalam [UNIX ekor] (http://en.wikipedia.org/wiki/Tail_%28Unix%29) seperti fashion. Jadi hanya memanggil mereka dan membatalkan memuji ketika Anda tidak lagi tertarik pada output.

## Links

 * [Zend_Log_Writer_Db - Menyesuaikan kolom tabel] (http://www.webeks.net/php/zend-log-writer-db-customising-table-columns.html)
 * [Logging di Zend Framework (Zend_Log & Zend_Log_Writer_Db)] (http://mnshankar.wordpress.com/tag/zend_log_writer_db/)
