#Deploying Joomla 2.5 ke CloudKilat

! [Deployment Sukses] (/ statis / apps / gambar / joomla-logo.png)

Jika Anda sedang mencari cepat, ringan dan efektif PHP Framework untuk proyek-proyek Anda, Anda tidak dapat melewati [Joomla] (http://www.joomla.org/download.html). Sekarang di [versi 2.5] (http://www.joomla.org/download.html) datang dengan berbagai fitur untuk mempercepat pengembangan aplikasi Anda, termasuk:

 * Baked di Keamanan
 * Pendekatan Batal MVC
 * Sebuah besar, berkembang, masyarakat
 * Banyak plugin dan add-ons
 * Mudah untuk membaca dokumentasi

Dalam tutorial ini, kita akan membawa Anda melalui penggelaran Joomla v2.5 untuk [platform CloudKilat] (http://www.CloudKilat.ch).

## Prasyarat

Anda akan hanya perlu beberapa hal untuk mengikuti bersama dengan tutorial ini. Ini adalah:

 * A [Git klien] (http://git-scm.com/), apakah baris perintah atau GUI.
 * Seorang klien MySQL, apakah baris perintah atau GUI, seperti [MySQL Workbench] (http://dev.mysql.com/downloads/workbench/) atau alat baris perintah.

## 1. Ambil Copy Joomla

Jadi sekarang bahwa Anda memiliki prasyarat di tempat, men-download salinan terbaru, stabil, rilis. Anda dapat menemukannya di: [http://www.joomla.org/download.html](http://www.joomla.org/download.html). Setelah itu, ekstrak file sytem lokal.

! [Deployment Sukses] (/ statis / apps / gambar / joomla-source.png)


## Buat Aplikasi Dasar

Setelah Anda memiliki salinan dari sumber Joomla tersedia secara lokal, setup VHost (atau setara) di server web Anda pilihan dan menginstal salinan itu, menerima pilihan default dan memasukkan rincian Anda sesuai. Jika Anda belum familiar dengan Joomla, pertama kali Anda melihatnya sebagai situs itu akan menjalankan installer.

## 2. Memperbarui Konfigurasi

Beberapa perubahan perlu dibuat ke default konfigurasi Joomla dan kode untuk mengakomodasi penyebaran CloudKilat. Perubahan ini adalah sebagai berikut:

 * Simpan sesi dalam database
 * Informasi Toko Cache di APC
 * Update Kode Konfigurasi

### 2.1 Toko Sesi di Database

Kecuali sesuatu berjalan serba salah, Anda tidak perlu melakukan apa-apa di sini sebagai Joomla harus dikonfigurasi untuk menyimpan informasi sesi dalam database secara default. Tapi beruang double-memeriksa, hanya untuk memastikan. Jadi, dari "Global Configuration" -> "System" di sisi kanan, lakukan hal berikut:

 * Di bawah ** Pengaturan Sesi **:
     * Memastikan ** Session Handler ** diatur ke ** database **
 
Klik Simpan.

### 2.2 Informasi Store Cache di APC

Secara default, caching di Joomla dimatikan. Jadi dari "Global Configuration" -> "System" di sisi kanan, lakukan hal berikut:

 * Di bawah ** Pengaturan Cache **:
     * Mengatur ** Cache ** untuk ** On **
     * Mengatur ** Cache Handler ** untuk ** PHP Alternatif Cache **

Klik ** Simpan & Tutup **.

### 2.3 Update Kode Konfigurasi

File konfigurasi inti Joomla, `` configuration.php``, diperbarui setiap kali rincian yang berubah di panel administrasi seperti yang kita hanya melakukan. Jadi, untuk mengambil informasi dari lingkungan CloudKilat menjadi, sedikit, sedikit rumit.

Apa yang bisa kita lakukan, meskipun solusi kekal jika kita upgrade versi kita Joomla, adalah untuk memperbarui file yang bertanggung jawab untuk menulis file configuration.php, sehingga meskipun konstruktor baru dapat memilih untuk kembali baik informasi asli atau mengambil data database dari lingkungan dan kembali bahwa alih-alih.

Kami melakukan ini dengan memperbarui `` perpustakaan / joomla / registry / Format / php.php``. Silahkan lihat pada versi modifikasi dari file di bawah ini:

    <? Php

    kelas JRegistryFormatPHP meluas JRegistryFormat
    {

    fungsi publik objectToString ($ object, $ params = array ())
    {
    // Membangun variabel objek string
    $ Vars = '';
    foreach (get_object_vars ($ object) sebagai $ k => $ v)
    {
    jika (is_scalar ($ v))
    {
    $ Vars. = "\ Tpublic $". $ K. "=" ". addcslashes ($ v, '\\\' '). "'; \ N";
    }
    elseif (is_array ($ v) || is_object ($ v))
    {
    $ Vars. = "\ Tpublic $". $ K. "=". $ This-> getArrayString ((array) $ v). "; \ N";
    }
    }
    
    $ Str = "<? Php \ nclass". $ Params ['kelas']. "{\ N";
    . $ Str = $ vars;
    
    //
    // Termasuk dalam generasi kelas panggilan ke sihir __get
    Metode //, yang akan membaca pengaturan database dari lingkungan
    // Dan melewati mereka saat dipanggil untuk dalam kode.
    //
    $ Str. = '
            fungsi publik __construct ()
            {
                $ Creds = null;
        
                if (! empty ($ _ SERVER [\ 'HTTP_HOST \']) &&
                    strpos ($ _ SERVER [\ 'HTTP_HOST \'], \ 'localdomain \') === FALSE) {
        
                    $ String = file_get_contents ($ _ ENV [\ 'CRED_FILE \'], false);
                    if ($ string == false) {
                        mati (\ 'FATAL: tidak dapat membaca kredensial mengajukan \');
                    }
                    $ Creds = json_decode ($ string, true);
                    $ This-> host = $ creds ["MYSQLS"] ["MYSQLS_HOSTNAME"];
                    $ This-> user = $ creds ["MYSQLS"] ["MYSQLS_USERNAME"];
                    $ This-> db = $ creds ["MYSQLS"] ["MYSQLS_DATABASE"];
                    $ This-> password = $ creds ["MYSQLS"] ["MYSQLS_PASSWORD"];
                }
            } '. "\ N";
    
    . $ Str = "}";
    
    // Gunakan tag penutup jika tidak diatur ke palsu dalam parameter.
    if (! isset ($ params ['closingtag']) || $ params ['closingtag']! == false)
    {
    . $ Str = "? \ N>";
    }
    
    kembali $ str;
    }
    


## 3. Masukan Control Kode bawah Git

Ok, sekarang mari kita mulai membuat perubahan ini dan menggunakan aplikasi. Kita akan mulai dengan meletakkan di bawah kontrol Git. Jadi jalankan perintah berikut untuk melakukannya:

    cd <directory Joomla Anda>
    
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

## 4. Menginisialisasinya Diperlukan Add-ons

Sekarang itu selesai, kita perlu mengkonfigurasi dua add-ons, config dan mysqls. Config add-on yang diperlukan untuk menentukan lingkungan aktif dan mysqls untuk menyimpan sesi dan login informasi.

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

## 5. Sebuah Catatan Tentang Logging & Temp Direktori

Mana mungkin menjadi menarik adalah jika / ketika Anda mulai menggunakan lebih dari satu klon untuk aplikasi Anda. Kami sudah melihat baik di IRC, Joomla Forum dan dokumentasi dan ada tampaknya tidak menjadi cara sederhana untuk menyimpan informasi log dalam database, meskipun kami percaya [JLog] (http://docs.joomla.org/API16 : JLog) dapat diperpanjang. ** Jadi tolong diingat ini. **

## 6. Database Schema

Sekarang, di shell, kita akan membuang database yang menginstal rutin dibuat dan beban ke dalam mysql contoh remote yang kita buat sebelumnya. Untuk melakukannya, jalankan perintah berikut, mengubah pilihan masing-masing dengan pengaturan konfigurasi Anda, melakukan hal ini untuk kedua standar dan pengujian:

    - Dump database (SQL) File
    mysqldump -u <database_username> p <database_name>> joomla_CloudKilat_init.sql

    - Memuat database dump (SQL) file ke database lingkungan yang jauh
    mysql -u <database_username> p \
        h mysqlsdb.co8hm2var4k9.eu-west-1.rds.amazonaws.com \
        --ssl-ca = mysql-ssl-ca-cert.pem <database_name> <joomla_CloudKilat_init.sql

Pada perintah di atas, Anda dapat melihat referensi ke pem ** berkas **.. Ini dapat didownload dari: [http://s3.amazonaws.com/rds-downloads/mysql-ssl-ca-cert.pem](http://s3.amazonaws.com/rds-downloads/mysql-ssl-ca-cert.pem). Semua yang baik, perintah akan selesai diam-diam, memuat data. Anda dapat memeriksa bahwa semua sudah pergi baik dengan perintah berikut:

    mysql -u <database_username> p \
        h mysqlsdb.co8hm2var4k9.eu-west-1.rds.amazonaws.com \
        --ssl-ca = mysql-ssl-ca-cert.pem <database_name>
    
    menampilkan tabel;
    
Ini akan menunjukkan tabel dari file SQL. Sekarang itu selesai, melakukan perubahan yang kami buat sebelumnya dan mendorong dan menyebarkan kedua lingkungan lagi sehingga informasi baru akan digunakan. Hal ini dapat dilakukan dengan cepat dengan perintah berikut:

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

### 7.1 Masalah Deployment

Jika Anda memiliki masalah penggelaran aplikasi Joomla, maka silakan baca file log. Ada, saat ini, dua tersedia, ini adalah ** menyebarkan ** dan ** ** kesalahan. Sebagai nama menyarankan, menyebarkan memberikan gambaran tentang proses penyebaran dan error menunjukkan kesalahan PHP setiap dan semua ke memperpanjang diperbolehkan oleh tingkat penebangan Anda saat ini.

Untuk melihat informasi ini, jalankan perintah berikut masing-masing:

#### 7.1.1 Deployment

    ironapp APP_NAME / default log menyebarkan

#### 7.1.1 Kesalahan

    ironapp APP_NAME / default error log

Perintah output informasi dalam [UNIX ekor] (http://en.wikipedia.org/wiki/Tail_%28Unix%29) seperti fashion. Jadi hanya memanggil mereka dan membatalkan memuji ketika Anda tidak lagi tertarik pada output.

### 7.2 Pertimbangan Deployment

Seperti yang telah disebutkan sebelumnya, ini bukan solusi yang paling sempurna seperti ketika Anda meng-upgrade Joomla, perubahan yang dibuat untuk php.php akan ditimpa dan JLog masih menulis ke filesystem. Kami sedang bekerja pada solusi yang lebih permanen untuk situasi ini.

## Links
 
 * [Joomla] (http://www.joomla.org/)
