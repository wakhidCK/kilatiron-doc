# Dokumentasi CloudKilat 

## Landasan Akses

**TL;DR:**

 * Baris perintah klien adalah antarmuka utama.
 * Kami juga menawarkan konsol web.
 * Untuk kontrol penuh dan integrasi memungkinkan untuk berbicara langsung dengan RESTful API.

Untuk mengontrol platform kami menawarkan beberapa antarmuka yang berbeda. Cara utama untuk mengendalikan aplikasi dan penyebaran adalah melalui [antarmuka baris perintah](http://en.wikipedia.org/wiki/Command-line_interface) (CLI) yaitu *ironapp* dan *ironuser*. Selain itu kami juga menawarkan [web konsol]. Kedua CLI serta konsol web namun hanyalah frontends untuk RESTful API kami. Untuk integrasi yang lebih dalam ke aplikasi Anda, Anda dapat menggunakan salah satu dari yang tersedia [API perpustakaan] kami.

Sepanjang dokumentasi ini kita akan menggunakan CLI sebagai cara utama untuk mengendalikan platform CloudKilat. CLI terdiri dari 2 bagian: *ironapp* dan *ironuser*. Untuk mendapatkan bantuan untuk klien baris perintah, hanya menambahkan --help atau -h ke salah satu perintah tersebut.

Instalasi klien baris perintah mudah dan berjalan pada Mac/Linux serta pada Windows.

#### Instalasi pada Windows

Untuk Windows kami menawarkan installer. Silahkan download [versi terbaru] installer dari S3. File tersebut bernama cctrl-x.x-setup.exe.

#### Instalasi pada Linux/Mac

Pada Linux dan Mac OS kami sarankan menginstal dan memperbarui klien baris perintah melalui pip. aplikasi ini membutuhkan [Python 2.6+].

~~~
$ sudo pip install -U ironcli
~~~

Jika Anda tidak memiliki pip Anda dapat menginstal pip melalui easy_install (di Linux biasanya merupakan bagian dari paket python-setuptools):

~~~
$ sudo easy_install pip
$ sudo pip install -U ironcli
~~~

## Akun Pengguna

Akun pengguna dapat dibuat di  [www.cloudkilat.com](http://www.cloudkilat.com/).

## Aplikasi dan Penyebaran

**TL;DR:**

 * Aplikasi (apps) memiliki repositori dan penyebaran.
 * Repositori adalah tempat dimana kode Anda berada, yang diselenggarakan di cabang.
 * Penyebaran adalah versi menjalankan aplikasi Anda, berdasarkan cabang dengan nama yang sama. Pengecualian: penyebaran standar didasarkan pada cabang master.

CloudKilat PaaS menggunakan satu set konvensi penamaan yang berbeda. Untuk dapat bekerja efektif dengan platform , penting untuk memahami beberapa konsep dasar berikut .

### Aplikasi 

Sebuah aplikasi terdiri dari repositori (dengan cabang) dan penyebaran.

Membuat sebuah aplikasi sangatlah mudah. Hanya menentukan nama dan tipe yang diinginkan untuk menentukan [buildpack](#buildpacks-and-the-procfile) untuk digunakan.

~~~
$ ironapp APP_NAME create php
~~~

Anda selalu bisa melisting aplikasi yang ada dengan menggunakan klien baris perintah .

~~~
$ ironapp -l
Apps
 Nr  Name                           Type
   1 myfirstapp                     php
   2 nextbigthing                   php
   [...]
~~~

### Keys Pengguna

Untuk mengamankan akses ke repositori aplikasi, masing-masing pengembang perlu mengotentikasi
melalui otentikasi kunci pribadi/publik. Silakan lihat artikel GitHub di
[Menghasilkan kunci SSH] untuk rincian tentang cara membuat kunci. Anda bisa menambahkan
kunci default untuk account pengguna Anda menggunakan *web konsol* atau perintah
klien baris. Jika tidak ada kunci yang dapat ditemukan, ironapp akan menawarkan untuk membuat satu.

~~~
$ ironuser key.add
~~~

Anda juga bisa menampilkan id kunci yang tersedia dan menghapus kunci yang ada menggunakan id kunci.

~~~
$ ironuser key
Keys
 Dohyoonuf7

$ ironuser key Dohyoonuf7
ssh-rsa AAA[...]

$ ironuser key.remove Dohyoonuf7
~~~

### Penyebaran

Penyebaran adalah menjalankan salah satu versi pada cabang Anda yang dapat diakses melalui [subdomain yang tersedia](#provided-subdomains-and-custom-domains).
Hal ini didasarkan pada cabang dengan nama yang sama. Pengecualian: penyebaran standar didasarkan pada cabang master.

Penyebaran berjalan secara terpisah satu sama lain, termasuk lingkungan runtime terpisah, penyimpanan file system dan tambahan (misalnya database).
Hal ini memungkinkan untuk memiliki versi yang berbeda dari aplikasi Anda yang berjalan pada waktu yang sama tanpa mengganggu satu sama lain.
Silakan lihat bagian tentang [pembangunan, pementasan dan lingkungan produksi](#development-staging-and-production-environments)untuk memahami mengapa hal ini adalah ide yang baik.

Anda bisa menampilkan semua penyebaran dengan *details* perintah.

~~~
$ ironapp APP_NAME details
App
 Name: APP_NAME                       Type: php        Owner: EMAIL_ADDRESS
 Repository: ssh://APP_NAME@kilatiron.net/repository.git

 [...]

 Deployments
   APP_NAME/default
   APP_NAME/dev
   APP_NAME/stage
~~~


## Kontrol Versi & Citra

**TL;DR:**

 * Git adalah VCS yang didukung.
 * Ketika Anda mendorong untuk memperbarui cabang, citra kode Anda akan dibangun, lalu siap untuk digunakan.
 * Ukuran citra terbatas sampai 200MB (terkompresi). Gunakanlah file `.cctrlignore` untuk mengecualikan aset.

### Membangun Citra

Setiap kali Anda mendorong untuk memperbarui cabang, citra penyebaran dibangun secara otomatis.
Citra ini kemudian dapat digunakan dengan perintah *deploy* untuk penyebaran
sesuai dengan nama cabang. Isi dari citra dihasilkan oleh
[buildpack](#buildpacks-and-the-procfile) termasuk kode aplikasi Anda dalam
bentuk siap untuk dijalankan dengan semua dependensi yang ada.

Anda bisa menggunakan perintah mendorong ironapp atau mendorong dengan perintah git . Mohon Untuk
mengingat bahwa penyebaran dan nama cabang harus sesuai. Jadi untuk mendorong ke penyebaran dev gunakanlah perintah berikut. Juga perhatikan, keduanya
membutuhkan keberadaan cabang dev.

~~~
# Dengan ironapp:
$ ironapp APP_NAME/dev push

# Mendapatkan REPO_URL dari output dari ironapp APP_NAME details

# Dengan git:
$ git remote add exo REPO_URL 
$ git push exo dev
~~~

Repositori mendukung semua operasi jarak jauh lainnya seperti menarik dan kloning juga.

Ukuran citra dikompresi terbatas sampai dengan 200MB. Citra yang lebih kecil dapat digunakan
lebih cepat, jadi kami sarankan untuk menjaga ukuran di bawah 50MB. Ukuran citra dicetak pada akhir proses membangun; jika citra melebihi batas tersebut, dorongan akan ditolak.

Anda dapat mengurangi ukuran citra Anda dengan memastikan bahwa tidak ada file yang tidak dibutuhkan (misalnya
cache, log, file backup) yang terlacak dalam repositori Anda. File yang perlu
dilacak tetapi tidak diperlukan dalam citra (misalnya aset pembangunan atau sumber
file kode dalam bahasa yang sudah dikompilasi), dapat ditambahkan ke file `.cctrlignore` di dalam
direktori root projeknya. Formatnya mirip dengan `.gitignore`, tetapi tanpa
operator  `!`. Berikut ini adalah contoh `.cctrlignore`:

~~~
*.psd
*.pdf
test
spec
~~~
#### Buildpacks dan Procfile

Selagi proses mendorong dijalankan buildpack langsung berjalan. Sebuah buildpack adalah satu set
script yang menentukan bagaimana sebuah aplikasi dalam bahasa tertentu atau kerangka harus
disiapkan untuk penyebaran pada landasan CloudKilat. Dengan buildpacks kustom,
dukungan untuk bahasa pemrograman baru dapat ditambahkan atau runtime kustom
lingkungan dapat dibangun. Untuk mendukung banyak PaaS dengan satu buildpack, kami
merekomendasikan mengikuti [Heroku buildpack API] yang kompatibel dengan
CloudKilat dan platform lainnya.

Bagian dari script buildpack juga dapat menarik dependensi menurut
bahasa atau kerangka cara asli. Misalnya pip dan requirements.txt untuk Python,
Maven untuk Java, NPM untuk Node.js, Komposer untuk PHP dan sebagainya. Hal ini memungkinkan Anda untuk
sepenuhnya mengontrol perpustakaan dan versi yang tersedia untuk aplikasi Anda di 
lingkungan runtime final.

Buildpack yang akan digunakan ditentukan oleh jenis aplikasi set
saat membuat aplikasi.

Sebuah bagian yang diperlukan dari citra adalah file yang bernama `Procfile` di direktori root.
Hal ini digunakan untuk menentukan bagaimana untuk memulai aplikasi yang sebenarnya dalam wadah.
Beberapa buildpacks dapat memberikan standar Procfile. Tapi dianjurkan untuk
eksplisit menentukan Procfile dalam aplikasi Anda untuk menyesuaikan 
persyaratan pribadi Anda yang lebih baik. Untuk wadah agar dapat menerima permintaan dari
lapis Routing dibutuhkan setidaknya konten berikut:

~~~
web: COMMAND_TO_START_THE_APP_AND_LISTEN_ON_A_PORT --port $PORT
~~~

Untuk contoh yang lebih spesifik dari `Procfile` silakan lihat bahasa dan kerangka [panduan].

Pada akhir proses buildpack, citra siap untuk digunakan.


## Menyebarkan Versi Baru

Platform CloudKilat mendukung nol downtime menyebarkan untuk semua penyebaran. Untuk menyebarkan penggunaan versi baru baik *web konsol* atau perintah `deploy`.

~~~
$ ironapp APP_NAME/DEP_NAME deploy
~~~

Untuk menyebarkan versi tertentu, tambahkan versi pengenal sistem kontrol Anda (komit penuh-SHA1).
Jika tidak ditentukan, versi yang akan dikerahkan adalah default untuk gambar terbaru yang tersedia (yang dibangun selama push sukses terakhir).

Untuk setiap menyebarkan, citra mengunduh banyak node platform seperti yang dipersyaratkan oleh [--containers setting](#scaling)dan mulai sesuai dengan default buildpack atau[Procfile](#buildpacks-and-the-procfile).
Setelah wadah baru berjalan dan tingkat load balancing berhenti mengirim permintaan ke kontainer lama dan mengirim mereka ke versi baru.
Sebuah pesan log di [deploy log](#deploy-log) muncul ketika proses ini selesai.

### Kontainer Idling

Penyebaran pada wadah web tunggal dengan satu unit memori (128MB/h) secara otomatis malas ketika mereka tidak menerima permintaan HTTP selama 1 jam atau lebih. Ini
hasil dalam penghentian sementara wadah di mana aplikasi ini
berjalan. Ini tidak mempengaruhi Pengaya atau pekerja yang terkait dengan penyebaran ini.

Setelah permintaan HTTP baru dikirim ke penyebaran ini, aplikasi secara otomatis kembali terlibat. Proses ini menyebabkan sedikit keterlambatan sampai
permintaan pertama dilayani. Semua permintaan berikut akan tampil normal.

Anda dapat melihat keadaan aplikasi Anda dengan perintah berikut:
~~~
$ ironapp APP_NAME/DEP_NAME details
Deployment
 name: APP_NAME/DEP_NAME
 [...]
 current state: idle
 [...]
~~~

Scaling penyebaran Anda akan mencegah pemalasan, yang sangat direkomendasikan untuk
sistem produksi.

## Darurat Rollback

Jika versi terbaru Anda rusak tiba-tiba, Anda dapat menggunakan perintah rollback untuk kembali ke versi sebelumnya dalam hitungan detik:

~~~
$ Ironcliapp APP_NAME/DEP_NAME rollback
~~~

Hal ini juga memungkinkan untuk menyebarkan versi lain sebelumnya. Untuk menemukan identifier versi yang Anda butuhkan, cukup centang [menyebarkan log](#deploy-log) untuk versi sebelumnya, atau mendapatkannya langsung dari sistem kontrol versi. Anda dapat memindahkan versi ini menggunakan perintah menyebarkan:

~~~
$ ironapp APP_NAME/DEP_NAME deploy THE_LAST_WORKING_VERSION_HASH
~~~


## Sistemfile Tidak Persisten

**TL;DR:**

 * Setiap wadah memiliki filesystem sendiri.
 * Filesystem ini tidak terus-menerus.
 * Jangan menyimpan unggahan pada filesystem.

Penyebaran pada platform CloudKilat memiliki akses ke filesystem yang dapat ditulis. 
Filesystem ini namun tidak persisten. Data ditulis mungkin atau mungkin tidak dapat diakses
lagi di masa depan apabila ada permintaan, tergantung pada bagaimana [routing tier](#routing-tier)
rute permintaan di wadah yang tersedia, dan dihapus setelah setiap menyebarkan.
Hal ini termasuk dengan Anda menyebarkan secara manual, tetapi juga menyebarkan ulang yang dilakukan oleh
Platform itu sendiri selama operasi normal.

Untuk upload pelanggan (misalnya gambar profil pengguna) kami merekomendasikan toko objek seperti Amazon S3 atau sejenisnya.


## Pembangunan, Pementasan dan Produksi Lingkungan

**TL;DR:**

 * Beberapa pengaruh penyebaran untuk mendukung penerapan siklus hidup lengkap.
 * Setiap penyebaran memiliki seperangkat variabel lingkungan untuk membantu Anda mengkonfigurasi aplikasi Anda.
 * Berbagai file konfigurasi tersedia untuk menyesuaikan pengaturan runtime.

### Pembangunan, Pementasan dan Produksi: Siklus hidup Aplikasi

Kebanyakan aplikasi berbagi aplikasi siklus hidup, yang terdiri dari pembangunan,
pementasan dan tahap produksi. Landasan CloudKilat dirancang dari
bawah keatas untuk mendukung ini. Seperti yang kita jelaskan sebelumnya, setiap aplikasi dapat memiliki beberapa
penyebaran. Mereka menyebar sesuai dengan cabang di kontrol versi
sistem. Alasan untuk ini sangat sederhana. Untuk bekerja pada fitur baru dianjurkan untuk membuat cabang baru. 
Versi baru ini kemudian dapat digunakan sebagai penyebarannya sendiri memastikan pengembangan fitur baru tidak mengganggu penyebaran yang sudah ada.
Yang lebih penting adalah, pementasan penyebaran juga membantu memastikan bahwa kode baru akan bekerja dalam sistem produksi
karena setiap penyebaran menggunakan [tumpukan](#stacks) yang sama memiliki runtime lingkungan hidup yang sama.

### Variabel Lingkungan

Kadang-kadang Anda memiliki konfigurasi khusus lingkungan, misalnya mengaktifkan debugging output dalam penyebaran pembangunan tetapi menonaktifkannya dalam penyebaran produksi. Hal ini dapat dilakukan dengan menggunakan variabel lingkungan yang setiap penyebaran menyediakan untuk aplikasi. Variabel lingkungan yang tersedia:

 * **TMPDIR**: Lokasi ke direktori tmp.
 * **CRED_FILE**: Lokasi file creds.json yang berisi Add-on kredensial.
 * **DEP_VERSION**: Versi git untuk membangun citra.
 * **DEP_NAME**: Nama penyebaran dalam format yang sama seperti yang digunakan oleh klien baris perintah. Misalnya myapp/default. Yang satu ini tetap sama bahkan ketika undeploying dan menciptakan penyebaran baru dengan nama yang sama.
 * **DEP_ID**: ID penyebaran internal. Yang satu ini tetap sama untuk penyebaran seumur hidup tapi berubah ketika undeploying dan menciptakan penyebaran baru dengan nama yang sama.
 * **WRK_ID**: ID pekerja internal. Hanya mengatur untuk kontainer pekerja.


## Pengaya

**TL;DR:**

 * Pengaya memberikan akses ke layanan tambahan seperti database.
 * Setiap penyebaran perlu mengatur sendiri Pengaya.
 * Kredensial Pengaya tersedia untuk aplikasi Anda melalui JSON diformat `$CRED_FILE` (dan melalui variabel lingkungan tergantung pada bahasa pemrogramannya).

### Mengelola Pengaya

Pengaya menambahkan layanan tambahan untuk penyebaran Anda. [Add-on marketplace]
menawarkan berbagai pilihan Pengaya. Anggap saja sebagai sebuah toko aplikasi
didedikasikan untuk para pengembang. Pengaya berkisar dari persembahan database untuk jasa pengolahan data
atau solusi penyebaran.

Setiap penyebaran memiliki set Pengaya sendiri. Jika aplikasi Anda membutuhkan database MySQL
dan Anda memiliki produksi, pengembangan dan lingkungan pementasan, ketiganya
harus memiliki sendiri MySQL Pengaya mereka. Setiap Pengaya dilengkapi dengan rencana yang berbeda
memungkinkan Anda untuk memilih database yang lebih kuat untuk lalu lintas tinggi 
penyebaran produksi anda dan yang lebih kecil untuk pengembangan atau pementasan
lingkungan.

Anda dapat melihat rencana Pengaya tersedia di pasar Pengaya situs web atau dengan perintah `ironapp addon.list`.

~~~
$ ironapp APP_NAME/DEP_NAME addon.list
[...]
~~~

Menambahkan Pengaya sama mudahnya dengan perintah.
~~~
$ ironapp APP_NAME/DEP_NAME addon.add ADDON_NAME.ADDON_OPTION
~~~

Seperti biasa mengganti penampung ditulis dalam huruf besar dengan nilai masing-masing.

Untuk mendapatkan daftar Pengaya untuk penyebaran gunakan perintah addon.
~~~
$ ironapp APP_NAME/DEP_NAME addon
Addon                    : alias.free

Addon                    : config.free
[...]

Addon                    : cron.hourly
[...]

Addon                    : mysqls.free
[...]
~~~

Untuk meng-upgrade atau downgrade Pengaya menggunakan perintah berikut diikuti oleh rencana Pengaya Anda meng-upgrade dari dan rencana Pengaya Anda meng-upgrade ke.

~~~
# upgrade
$ ironapp APP_NAME/DEP_NAME addon.upgrade FROM_SMALL_ADDON TO_BIG_ADDON
# downgrade
$ ironapp APP_NAME/DEP_NAME addon.downgrade FROM_BIG_ADDON TO_SMALL_ADDON
~~~
**Ingat:** Seperti dalam semua contoh dalam dokumentasi ini, gantilah semua penampung huruf besar dengan nilai masing-masing.

### Kredensial Pengaya
Bagi banyak Pengaya Anda memerlukan kredensial untuk menyambung ke layanan mereka. Kredensial yang diekspor ke penyebaran di
file konfigurasi dengan format JSON. Path ke file dapat ditemukan dalam variabel lingkungan `CRED_FILE`. Jangan pernah
hardcode kredensial tersebut dalam aplikasi Anda, karena mereka berbeda selama penyebaran dan dapat berubah setelah redeploy setiap
tanpa pemberitahuan.

Kami menyediakan contoh kode rinci bagaimana menggunakan file konfigurasi di bagian pemandu kami.

#### Mengaktifkan / menonaktifkan kredensial variabel lingkungan
Kami merekomendasikan menggunakan kredensial untuk alasan keamanan, tetapi kredensial juga dapat diakses melalui variabel lingkungan.
Ini dinonaktifkan secara default untuk aplikasi PHP dan Python.
Mengakses lingkungan lebih nyaman dalam banyak bahasa, tetapi beberapa alat pelaporan atau pengaturan keamanan yang salah dalam
aplikasi Anda dapat mencetak variabel lingkungan untuk layanan eksternal atau bahkan pengguna Anda. Hal ini juga berlaku untuk setiap proses anak
dari aplikasi Anda jika mereka mewarisi lingkungan (yang merupakan default). Jika ragu, nonaktifkan fitur ini dan gunakan
file kredensial.

Set variabel `SET_ENV_VARS` menggunakan [Custom Config Add-on] baik `false` atau `true` secara eksplisit mengaktifkan atau menonaktifkan
fitur ini.

Panduan bagian memiliki contoh rinci tentang bagaimana untuk mendapatkan kredensial dalam
bahasa yang berbeda
([Ruby](/Guides/Ruby/Add-on credentials.md),
[Python](/Guides/Python/Add-on credentials.md),
[Node.js](/Guides/NodeJS/Add-on credentials.md),
[Java](/Guides/Java/Add-on credentials.md),
[PHP](/Guides/PHP/Add-on credentials.md)).
Untuk melihat format dan isi kredensial lokal, gunakan perintah `addon.creds`.

~~~
$ ironapp APP_NAME/DEP_NAME addon.creds
{
    "CONFIG": {
        "CONFIG_VARS": {
            "FOO": "BAR"
        }
    },
    "MYSQLS": {
        "MYSQLS_DATABASE": "SOME_DB_NAME",
        "MYSQLS_HOSTNAME": "SOME_HOST.eu-west-1.rds.amazonaws.com",
        "MYSQLS_PASSWORD": "SOME_SECRET_PASSWORD",
        "MYSQLS_PORT": "3306",
        "MYSQLS_USERNAME": "SOME_USERNAME"
    }
}
~~~

## Logging 

**TL;DR:**

 * Ada empat jenis log yang berbeda (akses, kesalahan, pekerja dan menyebarkan) yang tersedia.

Untuk melihat output log dalam gaya `tail -f` mode gunakan perintah log ironapp. Perintah log awalnya menunjukkan 500 pesan log dan kemudian menambahkan pesan baru saat mereka tiba.

~~~
$ ironapp APP_NAME/DEP_NAME log [access,error,worker,deploy]
[...]
~~~

### Akses Masuk

The `log access` menunjukkan setiap permintaan untuk aplikasi Anda dalam format log Apache kompatibel.

### Error Log

The `log error` menunjukkan semua keluaran aplikasi Anda cetak ke stdout, stderr dan syslog. Log ini mungkin adalah tempat terbaik untuk melihat ketika aplikasi Anda tidak melakukan dengan baik. Kami juga menunjukkan penyebaran baru di sini untuk memberikan Anda lebih banyak konteks tetapi Anda selalu dapat merujuk pada [menyebarkan log] (# menyebarkan-log) untuk informasi rinci tentang menyebarkan.

### Pekerja Log

Pekerja lama menjalankan proses latar belakang. Mereka tidak dapat diakses melalui http dari luar. Untuk membuat output pekerja terlihat oleh Anda, stdout, stderr dan syslog output ditangkap dalam log ini. Log pekerja berisi timestamp dari acara tersebut, * wrk_id * pekerja serta garis log yang sebenarnya.

### Deploy Log

The `log deploy` memberikan informasi rinci tentang proses menyebarkan. Ini menunjukkan berapa banyak node penyebaran Anda berjalan dengan informasi tambahan tentang node, kali startup dan ketika loadbalancers mulai mengirimkan lalu lintas ke [versi baru] (# penggelaran-baru-versi).

### Menyesuaikan penebangan

The [Custom Config Add-on] dapat digunakan untuk meneruskan kesalahan dan pekerja log untuk layanan logging eksternal.

#### Menambahkan syslog kustom logging dengan Kustom Config Add-on

Custom Config Add-on dapat digunakan untuk menentukan titik akhir tambahan untuk menerima kesalahan dan pekerja log.
Hal ini dilakukan dengan menetapkan variabel config "RSYSLOG_REMOTE". Konten harus mengandung berlaku [rsyslog] konfigurasi dan dapat span beberapa baris.

Misalnya untuk meneruskan log ke syslog kustom jauh lebih [TLS] koneksi, membuat file sementara dengan konten berikut:
~~~
$ DefaultNetstreamDriverCAFile / app / CUSTOM_CERTIFICATE_PATH
$ ActionSendStreamDriver GTLS
$ ActionSendStreamDriverMode 1
$ ActionSendStreamDriverAuthMode x509 / nama
$ Template CustomFormat, "% syslogtag %% msg% \ n"
. * *@@SERVER_ADDRESS: PORT; CustomFormat
~~~
Di mana "SERVER_ADDRESS" dan "PORT" harus diganti dengan nilai-nilai konkrit dan "CUSTOM_CERTIFICATE_PATH" harus jalan ke file sertifikat untuk syslog kustom terpencil di repositori Anda.

Gunakan nama file (misalnya `custom_remote.cfg`) sebagai nilai untuk" RSYSLOG_REMOTE "variabel config:
~~~
$ Ironcliapp APP_NAME / DEP_NAME config.add RSYSLOG_REMOTE = custom_remote.cfg
~~~

Mulai sekarang semua log baru akan terlihat di remote syslog kustom.


## Subdomain Asalkan dan Custom Domain

** TL; DR: **

 * Setiap penyebaran disediakan dengan baik `* .kilatiron.net` dan` * .kilatiron.com` subdomain.
 * Custom domain didukung melalui Alias ​​Add-on.

Setiap penyebaran disediakan per bawaan dengan baik `* .kilatiron.net` dan` * .kilatiron.com` subdomain. The `APP_NAME.kilatiron.net` atau` APP_NAME.kilatiron.com` akan menunjuk ke `penyebaran default` sementara setiap penyebaran tambahan dapat diakses dengan subdomain diawali:` DEP_NAME-APP_NAME.kilatiron.net` atau `DEP_NAME-APP_NAME .kilatiron.com`.

Anda juga dapat menggunakan domain kustom untuk mengakses penyebaran Anda. Untuk menambahkan domain seperti `www.example.com`,` app.example.com` atau `secure.example.com` ke salah satu penyebaran Anda, cukup tambahkan masing-masing sebagai alias dan menambahkan CNAME untuk setiap menunjuk ke Anda subdomain penyebaran ini. Jadi untuk menunjuk `www.example.com` untuk penyebaran default dari aplikasi yang disebut * awesomeapp *, menambahkan CNAME untuk` www.example.com` menunjuk ke `awesomeapp.kilatiron.net` atau` awesomeapp.kilatiron.com` . The [Alias ​​Add-on] juga mendukung domain pemetaan wildcard seperti `* .example.com` ke salah satu penyebaran Anda.

Semua domain kustom perlu diverifikasi sebelum mereka mulai bekerja. Untuk memverifikasi domain, diperlukan juga menambahkan kode verifikasi CloudKilat sebagai catatan TXT.

Perubahan DNS dapat memakan waktu hingga 24 jam sampai mereka memiliki efek. Silakan merujuk ke Alias ​​Add-on Dokumentasi untuk petunjuk rinci tentang cara untuk setup CNAME dan TXT catatan.

### Akar Domain

Domain root (misalnya "example.com") juga dapat ditambahkan tetapi tidak secara langsung
didukung. Meskipun Anda secara teoritis dapat menambahkan data CNAME untuk root Anda
domain, Anda harus menyadari bahwa tidak ada catatan lain untuk domain ini bisa
diatur kemudian. ("A CNAME tidak diperbolehkan untuk hidup berdampingan dengan yang lain
Data ", http://tools.ietf.org/html/rfc1912). Dari titik Anda menetapkan
CNAME, semua DNS server standar-compliant akan mengabaikan entri lain yang Anda
mungkin ditetapkan untuk zona Anda (misalnya SOA, NS atau MX record).

Anda dapat menghindari keterbatasan ini dengan menggunakan penyedia DNS yang menyediakan
Fungsi CNAME seperti untuk domain akar, sering disebut aName atau ALIAS.

Sebuah alternatif adalah dengan menggunakan layanan redirection untuk mengirim pengguna dari
akar ke subdomain dikonfigurasi (misalnya example.org -> www.example.org).


## Tier Routing

** TL; DR: **

 * Semua permintaan HTTP diarahkan melalui routing yang tingkat kami.
 * Dalam routing lapis, Anda dapat memilih untuk rute permintaan melalui `* .kilatiron.net` atau` * .kilatiron.com` subdomain.
 * The `* .kilatiron.net` subdomain memberikan dukungan WebSocket.
 * The `* .kilatiron.com` subdomain menyediakan dukungan untuk HTTP caching melalui Varnish.
 * Permintaan diarahkan berdasarkan `sundulan Host`.
 * Gunakan `X-Forwarded-For` sundulan untuk mendapatkan IP client.

Semua permintaan HTTP dibuat untuk aplikasi pada platform yang diarahkan melalui routing yang tingkat kami. Routing tingkat dirancang sebagai cluster loadbalancers reverse proxy yang mengatur penyampaian permintaan pengguna untuk aplikasi Anda. Dibutuhkan perawatan routing permintaan ke salah satu wadah aplikasi berdasarkan pencocokan `Host` sundulan terhadap daftar alias penyebaran ini. Hal ini dilakukan melalui `* .kilatiron.net` atau` * .kilatiron.com` subdomain.

Routing tingkat dirancang untuk menjadi kuat terhadap node tunggal dan kegagalan datacenter bahkan lengkap sambil tetap latency menambahkan serendah mungkin.

### Alamat elastis

Karena sifat elastis dari routing tier, daftar alamat lapis routing dapat berubah sewaktu-waktu. Oleh karena itu sangat dianjurkan untuk menunjuk kustom domain langsung ke salah alamat IP lapis routing. Silakan gunakan CNAME gantinya. Rujuk ke [bagian custom domain] (# disediakan-subdomain-dan-custom-domain) untuk rincian lebih lanjut tentang konfigurasi DNS yang benar.

### Jauh Alamat

Mengingat bahwa permintaan klien tidak memukul aplikasi Anda secara langsung, tapi akan diteruskan melalui routing lapis, Anda tidak dapat mengakses IP klien dengan membaca alamat remote. Alamat remote akan selalu menjadi IP internal salah satu node routing. Untuk membuat asal-usul alamat remote yang tersedia, routing lapis menetapkan `X-Forwarded-For` header IP klien asli.

### Terbalik timeout Proxy

Kami Routing lapis menggunakan cluster loadbalancers reverse proxy untuk mengelola penerimaan dan penyampaian permintaan pengguna untuk aplikasi Anda. Untuk melakukan hal ini dalam cara yang efisien, kami mengatur timeout ketat untuk operasi baca / tulis. Nilai-nilai sedikit berbeda antara `* .kilatiron.net` dan` * .kilatiron.com` subdomain. Anda dapat menemukan mereka di bawah ini.

 * __Connect Timeout__ - waktu dalam koneksi ke aplikasi Anda telah yang akan didirikan. Jika kontainer Anda, tapi tergantung, maka batas waktu ini tidak akan berlaku sebagai koneksi ke titik akhir telah dibuat.
 * __Read Timeout__ - waktu untuk mengambil respon dari aplikasi Anda. Ini menentukan berapa lama routing tingkat akan menunggu untuk mendapatkan tanggapan atas permintaan. Timeout didirikan bukan untuk seluruh respon, tapi hanya antara dua operasi membaca.
 * __Send Timeout__ - waktu maksimum antara dua operasi tulis dari permintaan. Jika aplikasi Anda tidak mengambil data baru dalam waktu ini, routing tingkat akan menutup koneksi.

#### Timeout untuk `* .kilatiron.net` subdomain:

| Parameter | Nilai [s] |
|: --------- |: ----------: |
| Connect batas waktu | 20 |
| Kirim batas waktu | 55 |
| Baca batas waktu | 55 |

#### Timeout untuk `* .kilatiron.com` subdomain:

| Parameter | Nilai [s] |
|: --------- |: ----------: |
| Connect batas waktu | 60 |
| Kirim batas waktu | 60 |
| Baca batas waktu | 120 |

### Permintaan distribusi

Kami pintar [DNS] (https://en.wikipedia.org/wiki/Domain_Name_System) menyediakan cepat dan handal layanan menyelesaikan nama domain dalam mode robin round. Semua node merata ke tiga ketersediaan zona yang berbeda namun permintaan dapat rute ke wadah apapun dalam setiap zona ketersediaan lainnya. Untuk menjaga latency rendah, routing lapis mencoba untuk permintaan rute ke kontainer di zona ketersediaan yang sama kecuali tidak tersedia. Penyebaran berjalan pada --containers 1 (lihat [bagian skala] (# skala) untuk rincian) hanya berjalan pada satu wadah dan oleh karena itu hanya host dalam satu zona ketersediaan.

### Ketersediaan Tinggi

Routing lapis menyediakan dua mekanisme untuk memastikan ketersediaan tinggi, tergantung pada subdomain yang disediakan. Ini adalah Kesehatan Checker (untuk `* .kilatiron.net` subdomain) dan Failover (untuk` * .kilatiron.com` subdomain). Karena mekanisme ini tergantung pada memiliki beberapa kontainer yang tersedia untuk permintaan rute, hanya penggelaran dengan lebih dari satu kontainer berjalan (lihat [bagian skala] (# skala) untuk rincian) dapat mengambil keuntungan dari ketersediaan tinggi.

Dalam hal node atau wadah kegagalan tunggal, platform akan memulai sebuah wadah pengganti. Penyebaran berjalan pada --containers 1 akan tersedia selama beberapa menit sementara platform mulai penggantian. Untuk menghindari bahkan downtime pendek, mengatur pilihan --containers untuk setidaknya 2.

#### `* .kilatiron.net` Subdomain

Untuk `* .kilatiron.net` subdomain, gagal permintaan akan menyebabkan pesan kesalahan akan kembali ke pengguna sekali, tetapi" tidak sehat "kontainer akan aktif dipantau oleh pemeriksa kesehatan. Ini sinyal routing lapis untuk menghapus sementara wadah yang tidak sehat dari daftar kontainer menerima permintaan. Permintaan berikutnya diarahkan ke wadah yang tersedia dari penyebaran. Setelah pemberitahuan checker kesehatan yang wadah telah pulih, wadah akan dimasukkan kembali-dalam daftar untuk menerima permintaan.

Karena checker kesehatan secara aktif memonitor wadah di mana aplikasi berjalan ke timeout atau kembali [http kode kesalahan] (http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.5) `501`, `502` atau` 503` lebih besar, Anda mungkin melihat permintaan untuk `/ CloudHealthCheck` datang dari` agen cloudControl-HealthCheck`.

#### `* .kilatiron.com` Subdomain

Untuk `* .kilatiron.com` subdomain, gagal permintaan secara otomatis kembali dialihkan ke wadah alternatif melalui mekanisme failover. Permintaan akan dicoba dengan wadah yang berbeda dalam set timeout. Hal ini juga akan memastikan permintaan berikutnya tidak dikirim ke lambat / kontainer rusak untuk jumlah waktu tertentu.


## Scaling

** TL; DR: **

 * Anda dapat skala naik atau turun setiap saat dengan menambahkan wadah lebih (skala horisontal) atau mengubah ukuran kontainer (skala vertikal).
 * Gunakan pemantauan kinerja dan pengujian beban untuk menentukan pengaturan skala optimal untuk aplikasi Anda.

Ketika skala aplikasi Anda, Anda memiliki dua pilihan. Anda juga dapat skala horizontal dengan menambahkan wadah lebih, atau skala vertikal dengan mengubah ukuran kontainer. Ketika Anda skala horizontal, loadbalancing CloudKilat dan [Routing lapis] (# Routing-tier) memastikan distribusi yang efisien dari permintaan yang masuk seberang semua kontainer yang tersedia.

### Scaling Horizontal

Skala horisontal dikendalikan oleh parameter --containers.
Ini menentukan jumlah kontainer yang telah berjalan.
Budidaya --containers juga meningkatkan ketersediaan dalam kasus kegagalan node.
Penyebaran dengan --containers 1 (default) tidak tersedia selama beberapa menit dalam hal kegagalan node sampai proses failover selesai. Set nilai --containers setidaknya 2 jika Anda ingin menghindari downtime dalam situasi seperti itu.

### Vertikal Scaling

Selain mengontrol jumlah kontainer Anda juga dapat menentukan ukuran memori dari sebuah wadah. Ukuran kontainer yang ditentukan dengan menggunakan parameter --memory, menjadi mungkin untuk memilih dari 128MB ke 1024MB.


## Kinerja & Caching

** TL; DR: **

 * Mengurangi jumlah permintaan yang membentuk tampilan halaman.
 * Cache jauh dari database Anda mungkin.
 * Cobalah untuk mengandalkan pemutus Cache bukannya pembilasan.

### Mengurangi Jumlah Permintaan

Dirasakan kinerja aplikasi web sebagian besar dipengaruhi oleh frontend. Ini sangat umum bahwa potensi optimasi tertinggi terletak dalam mengurangi jumlah keseluruhan permintaan per tampilan halaman. Salah satu teknik umum untuk mencapai hal ini adalah menggabungkan dan meminimalkan javascript dan file css ke dalam satu file masing-masing dan menggunakan sprite untuk gambar.

### Caching Awal

Setelah Anda telah mengurangi jumlah permintaan, itu dianjurkan untuk cache jauh dari database Anda mungkin. Menggunakan jauh-masa `header expires` menghindari bahwa browser meminta sumber daya sama sekali. Cara terbaik berikutnya untuk mengurangi jumlah permintaan yang melanda kontainer Anda untuk cache tanggapan lengkap dalam loadbalancer tersebut. Untuk ini kami menawarkan caching langsung di routing tier.

#### Caching Proxy

Routing lapis yang ada di depan semua penyebaran termasuk [Varnish] caching proxy. Untuk menggunakan fitur ini, perlu untuk menggunakan `* .kilatiron.com` subdomain. Untuk memiliki permintaan Anda cache secara langsung di Varnish dan mempercepat waktu respon melalui ini, pastikan Anda telah mengatur benar [header cache control] (http://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html) (` Cache-Control`, `Expires`,` Age`) untuk permintaan. Juga, memastikan bahwa permintaan tidak termasuk cookie. Cookie sering digunakan untuk menjaga negara di seluruh permintaan (misalnya jika pengguna login). Untuk menghindari tanggapan caching untuk log-in pengguna dan mengembalikan mereka ke pengguna lain, Varnish dikonfigurasi untuk tidak pernah permintaan tembolok dengan cookie.

Untuk dapat permintaan cache Varnish untuk aplikasi yang mengandalkan cookie, kami sarankan menggunakan [domain cookieless] (http://www.ravelrumba.com/blog/static-cookieless-domain/). Dalam hal ini, Anda harus mendaftarkan domain baru dan mengkonfigurasi DNS database Anda dengan `rekor CNAME` yang menunjuk ke APP_NAME.kilatiron.com` subdomain` `A` catatan Anda. Kemudian Anda dapat memperbarui konfigurasi aplikasi web Anda untuk melayani sumber daya statis dari domain baru Anda.

Anda dapat memeriksa apakah permintaan itu cache di Varnish dengan memeriksa respon yang * X-pernis-Cache * sundulan. Nilai HIT berarti respon dijawab langsung dari cache, dan LEWATKAN berarti itu tidak.

### Breakers Cache

Ketika caching permintaan di sisi klien atau di proxy caching, URL biasanya digunakan sebagai mengidentifikasi Cache. Selama URL tetap sama dan respon cache Belum berakhir, permintaan tersebut Dijawab dari cache. Sebagai bagian dari setiap penyebaran, semua kontainer dimulai dari citra bersih. Ini Memastikan Itu Semua kontainer-memiliki kode aplikasi Termasuk template terbaru, css, javascript dan file gambar. Namun, Bila menggunakan header jauh-masa telah merekomendasikan expires` `atas, ini Apakah tidak mengubah apa pun jika respon cache Apakah di tingkat loadbalancer pelanggan emas. Untuk mendapatkan pelanggan penjamin terbaru dan terbesar versi itu merekomendasikan untuk memasukkan parameter berubah menjadi URL. Hal ini sering disebut sebagai penutup breaker.

The [variabel Lingkungan] (variabel # lingkungan) dari lingkungan deployment runtime contenir yang DEP_VERSION app. Jika Anda ingin memaksa refresh Cache Ketika versi baru Dikerahkan Anda dapat menggunakan DEP_VERSION untuk Dicapai ini.

### Caching di kilatiron.net subdomain

Permintaan melalui `* .kilatiron.net` subdomain tidak dapat di-cache di routing tier. Namun, itu masih layak untuk Memberikan caching untuk aset statis dengan Memanfaatkan domain cookieless terpisah sebagai CNAME dari `* .kilatiron.com`subdomain. Misalnya, Anda dapat melayani permintaan dinamis aplikasi Anda melalui www.example.com (CNAME UNTUK `example.kilatiron.net`) dan melayani aset statis seperti CSS, JS dan gambar melalui static.example.com`` ( `example.kilatiron.com` untuk CNAME).


## WebSockets

** TL; DR: **

 * WebSockets didukung melalui `* .kilatiron.net` subdomain.
 * WebSockets memungkinkan real-time, pelanggan entre komunikasi dua arah dan server
 * Langkah-langkah tambahan yang diperlukan untuk mengamankan koneksi WebSocket
 * Hal ini sangat dianjurkan untuk menggunakan aman `wss: // protocol`` Daripada ws tidak aman: // '.

WebSockets memungkinkan Anda untuk mengaktifkan real-time, pelanggan saluran komunikasi dua arah entre dan server. Koneksi WebSocket menggunakan port HTTP standar (80 dan 443) seperti browser normal. Dalam rangka untuk MEMBANGUN koneksi WebSocket pada platform kami, pelanggan Memiliki Untuk eksplisit mengatur `` connection` Upgrade` dan [hop-by-hop] (http://tools.ietf.org/html/rfc2616#section-13.5. 1) header dalam permintaan. Mereka header menginstruksikan reverse-proxy untuk meng-upgrade protokol dari HTTP ke WebSockets kami. Setelah upgrade protokol jabat tangan selesai, frame data dapat dikirim entre les pelanggan dan server dalam modus full-duplex.

Semua timeout permintaan Dijelaskan atas aussi berlaku untuk koneksi WebSocket, minum dengan efek yang berbeda:

| Parameter | Nilai [s] | Keterangan |
|: -------- |: -------- |: ---------: |
| Kirim batas waktu | 55 | Timeout antara dua potongan berturut-turut dari data yang dikirim oleh pelanggan menjadi putih |
| Baca batas waktu | 55 | Timeout antara dua potongan berturut-turut data menjadi putih terasa kembali ke pelanggan |

Mengatasi keterbatasan Setiap habis, Anda Secara eksplisit dapat Melaksanakan WebSocket mekanisme [control Ping-Pong] (http://tools.ietf.org/html/rfc6455#page-36), qui terus koneksi hidup. Namun demikian, banyak dari klien WebSocket perpustakaan emas Diimplementasikan dalam banyak bahasa sudah menawarkan fitur ini di luar kotak.

### Aman WebSockets

WebSockets konvensional tidak menawarkan jenis protokol otentikasi tertentu atau enkripsi data. Anda Didorong untuk menggunakan standar HTTP Authentication Mekanisme seperti cookies, dasar / diggest atau TLS. Hal yang sama berlaku untuk enkripsi data SSL mana pilihan yang jelas Anda. Sementara koneksi WebSocket konvensional Didirikan melalui HTTP, HTTPS menggunakan satu dilindungi. Perbedaan ini didasarkan pada skema URI:

~~~
Koneksi normal: ws: // {tuan}: {} pelabuhan / {path ke server}
Koneksi aman: wss: // {tuan}: {} pelabuhan / {path ke server}
~~~

Harap Catatan itu WebSockets koneksi aman hanya dapat menggunakan `* Didirikan .kilatiron.net` subdomain, bukan yang kustom. Hal ini sangat dianjurkan untuk menggunakan 'em, tidak hanya untuk keamanan data Alasan. WebSockets aman adalah 100% transparent proxy, qui menempatkan kontainer Anda dalam kontrol penuh dari WebSocket meng-upgrade handshake` `di dalam kotak some of the proxy tidak menanganinya dengan benar.


## Pekerjaan Terjadwal dan Pekerja Latar Belakang

** TL; DR: **

 * Permintaan Web dikenai batas waktu 55.
 * Pekerjaan Dijadwalkan didukung melalui berbagai add-ons.
 * Pekerja Latar belakang adalah cara yang direkomendasikan menangani lama berjalan atau asynchronous tugas.

Sejak permintaan web Mengambil bersama dari 55 dibunuh oleh routing lapis, berjalan bersama-memiliki tugas yang harus ditangani asyncronously.

Cron ###

Untuk tugas itu dijamin untuk menyelesaikan dans le batas waktu, [Cron add-on] solusi sederhana adalah untuk memanggil URL yang telah ditetapkan per jam dan harian emas telah disebut kadaluarsa berkala tugas itu. Untuk dokumentasi yang lebih rinci tentang add-on Cron, silakan Rujuk ke [Cron dokumentasi Add-on].

### Pekerja

Tugas itu akan membawa serta dari 55 untuk mengeksekusi, atau itu dipicu oleh permintaan pengguna dan keharusan tidak ditangani asyncronously untuk menjaga pengguna tunggu, sebaiknya ditangani oleh [Pekerja add-on]. Pekerja proses berjalan lama dimulai pada wadah. Sama seperti proses web bertujuan Mereka Apakah tidak mendengarkan pada port dan The Oleh karena itu tidak recevoir permintaan http. Anda dapat menggunakan pekerja, misalnya, untuk polling antrian dan melaksanakan tugas-tugas di latar belakang emas menangani lama berjalan perhitungan periodik. Rincian lebih lanjut tentang skenario penggunaan antrian dan tersedia add-ons yang tersedia sebagai tangan [Pekerja Dokumentasi Add-on].


## Secure Shell (SSH)

Sifat terdistribusi CloudKilat dari platform means itu tidak layak untuk SSH
ke server yang sebenarnya. Sebaliknya, kami menawarkan perintah run, yang 'Memungkinkan Anda untuk
meluncurkan sebuah wadah baru dan terhubung melalui SSH Untuk itu.

Wadah emas identiques untuk pekerja web dimulai tujuan yang SSH wadah
daemon BUKAN dari salah satu perintah Procfile. Ini didasarkan pada tumpukan yang sama
picture and picture dan penyebaran Apakah aussi Menyediakan Add-on kredensial.

Template ###

Untuk memulai shell (misalnya bash) menggunakan perintah `run`.

~~~
$ APP_NAME ironapp / DEP_NAME run pesta
Menghubungkan ...
Peringatan: Secara ditambahkan '[10.62.45.100]: 25832' (RSA) ke daftar Dikenal host.
u25832 DEP_ID-25832 @: ~ / www $ echo "perintah interaktif bekerja dengan baik"
perintah interaktif bekerja dengan baik
u25832 DEP_ID-25832 @: ~ / www $ exit
Keluar
Koneksi ke 10.62.45.100 ditutup.
Koneksi ke sshforwarder.kilatiron.net ditutup.
~~~

Ini mungkin untuk aussi langsung untuk mengeksekusi perintah dan mitra kontainer-memiliki setelah perintah shutdown selesai. Hal ini sangat berguna untuk --Lain migrasi database dan tugas satu kali.

Misalnya, melewati `" env | semacam "` perintah akan daftar variabel lingkungan. Catatan penggunaan que le dari penilaian diperlukan untuk perintah Itu termasuk ruang.
~~~
$ APP_NAME ironapp / DEP_NAME run "env | sort"
Menghubungkan ...
Peringatan: Secara ditambahkan '[10250134126]: 10346' (RSA) ke daftar Dikenal host.
CRED_FILE = / srv / creds / creds.json
DEP_ID = DEP_ID
DEP_NAME = APP_NAME / DEP_NAME
DEP_VERSION = 9d5ada800eff9fc57849b3102a2f27ff43ec141f
DOMAIN = kilatiron.net
GEM_PATH = vendor / bundel / ruby ​​/ 1.9.1
RUMAH = / srv
HOSTNAME = DEP_ID-10346
LANG = en_US.UTF-8
LOGNAME = u10346
MAIL = / var / mail / u10346
OLDPWD = / srv
PAAS_VENDOR = CloudKilat
PATH = bin: vendor / bundel / ruby ​​/ 1.9.1 / bin: / usr / local / bin: / usr / bin: / bin
PORT = 10346
PWD = / srv / www
RACK_ENV = Produksi
RAILS_ENV = Produksi
SHELL = / bin / sh
SSH_CLIENT = 10.32.47.197 59378 10346
SSH_CONNECTION = 10.32.47.197 59378 10.250.134.126 10346
SSH_TTY = / dev / pts / 0
TERM = xterm
TMP_DIR = / srv / tmp
TMPDIR = / srv / tmp
PENGGUNA = u10346
WRK_ID = WRK_ID
Koneksi ke 10250134126 ditutup.
Koneksi ke sshforwarder.kilatiron.net ditutup.
~~~

## Stacks

** TL; DR: **

 * Tumpukan menentukan lingkungan runtime umum.
 * Mereka didasarkan pada Ubuntu dan Ubuntu nama tumpukan sesuai huruf pertama rilis itu.
 * Pinky adalah tumpukan saat ini dan mendukung beberapa bahasa selon yang tersedia [buildpacks] (# buildpacks-dan-the-procfile).

Tumpukan olefin lingkungan runtime umum untuk semua penyebaran menggunakannya.
Ini Jaminan Itu Semua penyebaran Anda semua versi yang sama dari OS
komponen serta semua perpustakaan pra-instal.

Tumpukan didasarkan pada Ubuntu dan rilis-memiliki huruf pertama yang sama sebagai
Mereka melepaskan didasarkan pada. Setiap stack bernama Setelah A pahlawan sidekick yang super. Kita
mencoba untuk menjaga 'em sebagai dekat dengan rilis Ubuntu mungkin, jangan membuat keuntungan
Ketika Diperlukan pertukaran untuk keamanan atau kinerja Alasan untuk mengoptimalkan
ik stack untuk tujuan tertentu adalah platform kami.

### Stacks Tersedia

 * ** ** Pinky berdasarkan [Ubuntu 12.04 LTS Precise Pangolin]

Rincian tentang stack saat ini tersedia melalui antarmuka baris perintah `ironapp`.~~~
$ ironapp APP_NAME/DEP_NAME details
 name: APP_NAME/DEP_NAME
 stack: pinky
 [...]
~~~

[generating SSH keys]: https://help.github.com/articles/generating-ssh-keys
[Custom Config Add-on]: /Add-on%20Documentation/Deployment/Custom%20Config.md
[web console]: http://www.cloudkilat.com/
[API libraries]: https://github.com/cloudControl
[the latest version]: https://www.cloudcontrol.com/download/win
[Python 2.6+]: http://python.org/download/
[quick Git tutorial]: http://rogerdudler.github.com/git-guide/
[Heroku buildpack API]: https://devcenter.heroku.com/articles/buildpack-api
[guides]: /Guides
[Add-on marketplace]: http://www.cloudkilat.com/
[rsyslog]: http://www.rsyslog.com/
[TLS]: http://en.wikipedia.org/wiki/Transport_Layer_Security
[Alias Add-on]: /Add-on%20Documentation/Deployment/Alias.md
[Varnish]: https://www.varnish-cache.org/
[Cron Add-on]: /Add-on%20Documentation/Deployment/Cron.md
[Cron Add-on documentation]: /Add-on%20Documentation/Deployment/Cron.md
[Worker Add-on]: /Add-on%20Documentation/Data%20Processing/Worker.md
[Worker Add-on documentation]: /Add-on%20Documentation/Data%20Processing/Worker.md
[Ubuntu 12.04 LTS Precise Pangolin]: http://releases.ubuntu.com/precise/
