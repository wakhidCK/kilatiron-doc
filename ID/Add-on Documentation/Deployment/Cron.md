Cron # Add-on

## Apa cronjobs?

Pada sistem UNIX [cronjobs] (http://en.wikipedia.org/wiki/Cron) Itu adalah perintah
Berkala Dilaksanakan. Kami CloudKilat Namun, tidak ada satu node Itu
kita dapat menjalankan cronjob tersebut. The Oleh karena itu kami CloudKilat cronjobs panggilan berkala
Menentukan URL untuk Anda.

## Bagaimana cara kerjanya?

The Cron Add-on Memungkinkan Anda untuk memanggil URL dalam interval tertentu, misalnya emas harian
jam. Ketika Anda menambahkan cron per jam di 14:45, panggilan berikutnya akan dijalankan pada
03:45. Untuk Cron harian Ini akan terulang kembali pada hari berikutnya di 14:45. The Cron
Add-on Apakah tidak menjamin URL hanya disebut ounce kadaluarsa per interval.

Cronjobs adalah permintaan rutin terhadap aplikasi Anda dan tunduk pada 55 sama
timelimit.

Jika Anda membutuhkan lebih banyak kontrol atas tugas Kapan dan Seberapa sering dijalankan dan / atau dimiliki
Itu tugas membawa serta dari 55 detik kami sarankan menggunakan
[Pekerja] (/ Add-on Dokumentasi / Pengolahan Data / Worker.md) Add-on.

## Menambahkan Cron Add-on

Sebelum Anda dapat menambahkan pekerjaan Cron, Add-on Hakikat yang Memiliki Untuk ditambahkan:

~~~
$ APP_NAME ironapp / DEP_NAME addon.add cron.OPTION
~~~

Seperti biasa pilihan yang berbeda tercantum pada [Cron Add-on] (/ Dokumentasi Add-on / Deployment / Cron.md) halaman.

## Menambahkan url untuk pekerjaan Cron

Untuk memanggil URL dengan interval tertentu yang Anda tulis sebagai parameter:

~~~
# Untuk penyebaran standar
$ APP_NAME ironapp / default cron.add http [s]: // [user: password @] APP_NAME.kilatiron.net
# Untuk Setiap penyebaran tambahan
$ APP_NAME ironapp / DEP_NAME cron.add http [s]: // [user: password @] DEP_NAME.APP_NAME.kilatiron.net
~~~

Anda hanya dapat menambahkan panggilan pekerjaan cron telah diverifikasi alias penyebaran. Ini
dianjurkan untuk menggunakan https Saat mengirim kredensial.

## Cron Daftar gambaran

Mendapatkan gambaran dari semua pekerjaan Cron Anda:

~~~
$ APP_NAME ironapp / cron DEP_NAME
~~~

## Rincian Cron

Dapatkan rincian pekerjaan Cron tertentu:

~~~
$ APP_NAME ironapp / cron DEP_NAME CRON_ID
Cronjob
 job_id: jobkqy7rdmg
 url: http://APP_NAME.kilatiron.net
 next_run: 2011-05-09 19:39:39
 dibuat: 2011/05/05 19:39:39
 diubah: 2011/05/05 19:39:39
~~~

## Menghapus pekerjaan Cron:

Anda dapat menghapus pekerjaan Cron oleh job_id yang

~~~
$ APP_NAME ironapp / DEP_NAME cron.remove job_id
~~~

## Upgrade / merendahkan Cron addon

Dalam rangka untuk beralih dari harian per jam Cron atau sebaliknya, gunakan up atau
Fungsi downgrade

~~~
$ APP_NAME ironapp / DEP_NAME addon.upgrade cron.free cron.hourly
~~~

emas

~~~
$ APP_NAME ironapp / DEP_NAME addon.downgrade cron.hourly cron.free
~~~

Cron menambahkan dengan gratis add-on akan tinggal dengan harian dan menambahkan cron
jam Add-on akan tetap per jam.

## Melepaskan Cron Add-on

Melepaskan add-on Hakikat dapat dilakukan dengan:

~~~
$ APP_NAME ironapp / DEP_NAME addon.remove cron.OPTION
~~~

Harap dicatat: Melepaskan add-on tidak akan secara otomatis menghapus semua pekerjaan Cron.
