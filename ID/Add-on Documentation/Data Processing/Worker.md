Pekerja # Add-on

Pekerja lama menjalankan proses latar belakang. Mereka Biasanya digunakan untuk apa pun dari mengirim email ke menjalankan perhitungan berat atau membangun kembali cache di latar belakang.

Setiap pekerja mulai melalui Pekerja add-on berjalan dalam wadah terisolasi terpisah. Wadah-memiliki tepat lingkungan runtime yang sama yang didefinisikan oleh stack dan buildpack Dipilih dan digunakan-memiliki akses yang sama ke semua penyebaran add-ons.

## Menambahkan Pekerja Add-on

Sebelum Anda dapat mulai pekerja, tambahkan add-on dengan perintah addon.add.

~~~
$ APP_NAME ironapp / DEP_NAME addon.add worker.single
~~~

## Mulai Pekerja a

Pekerja dapat dimulai melalui perintah worker.add baris pelanggan perintah ini.

Untuk Tentukan bagaimana memulai pekerja menambahkan baris baru untuk aplikasi Anda `Procfile` Dan Kemudian gunakan dari` Itu memang WORKER_NAME`.

~~~
$ APP_NAME ironapp / DEP_NAME worker.add WORKER_NAME [WORKER_PARAMS]
~~~

Menyertakan beberapa WORKER_PARAMS dalam tanda kutip ganda.

~~~
$ APP_NAME ironapp / DEP_NAME worker.add WORKER_NAME "param1 param2 PARAM3"
~~~

## Daftar Menjalankan Pekerja

Untuk mendapatkan daftar berjalan pekerja Saat menggunakan pekerja perintah.

~~~
$ APP_NAME ironapp / pekerja DEP_NAME
Pekerja
 nr. wrk_id
   1 WRK_ID
~~~

Anda bisa mendapatkan semua rincian aussi pekerja dengan menambahkan perintah WRK_ID untuk pekerja.

~~~
$ APP_NAME ironapp / DEP_NAME pekerja WRK_ID
Pekerja
wrk_id: WRK_ID
perintah: WORKER_NAME
params "param1 param2 PARAM3"
~~~

## Menghentikan Pekerja

Pekerja dapat dihentikan Entah melalui baris perintah atau dengan keluar proses pelanggan dengan kode nol keluar.

### Via Command Line

Untuk berhenti berjalan pekerja melalui baris perintah menggunakan perintah worker.remove.

~~~
$ APP_NAME ironapp / DEP_NAME worker.remove WRK_ID
~~~

Untuk mendapatkan WRK_ID Lihat bagian atas pekerja daftar.

### Via Exit Codes

Untuk menghentikan pekerja programatik menggunakan UNIX gaya kode keluar. Ada tiga kode keluar yang berbeda tersedia.

 * Exit (0); // Semuanya baik-baik saja. Pekerja akan dihentikan.
 * Exit (1); // Kesalahan. Pekerja Akan Ulang.
 * Exit (2); // Kesalahan. Pekerja akan dihentikan.

Untuk lebih jelasnya Rujuk ke [PHP contoh] (# php-pekerja-contoh) di bawah ini.

Log Pekerja ##

Seperti yang sudah dijelaskan di [Logging] bagian (/ Landasan Documentation.md/#logging) stdout dan stderr keluaran semua pekerja yang Dialihkan ke log pekerja. Untuk melihat output dalam mode seperti perintah tail -f menggunakan log.

~~~
$ APP_NAME ironapp / log pekerja DEP_NAME
[Fri 17 Desember 2010 01:39:41] WRK_ID Dimulai Pekerja (perintah: parameter 'WORKER_NAME' 'param1 param2 PARAM3')
[Fri 17 Desember 2010 01:39:42] WRK_ID Hello param1 param2 PARAM3
[...]
~~~

## Melepaskan Pekerja Add-on

Untuk menghapus Pekerja add-on menggunakan perintah addon.remove.

~~~
$ APP_NAME ironapp / DEP_NAME addon.remove worker.single
~~~

## Pekerja PHP Contoh

The Berikut contoh menunjukkan bagaimana menggunakan kode keluar untuk menghentikan atau memulai kembali pekerja.

~~~ Php
// Parameter Baca keluar kode
$ ExitCode = isset ($ argv [1]) && (int) $ argv [1]> 0? (Int) $ argv [1]: 0;
Langkah = $ 5;

$ Counter = 1;
sementara (benar) {
    mencetak "langkah". ($ Counter). PHP_EOL;
    if ($ == langkah $ counter) {
        if ($ ExitCode == 0) {
            mencetak "Semua beres Keluar." . PHP_EOL;
        } Lain jika ($ ExitCode == 2) {
            mencetak "Terjadi kesalahan. Keluar." . PHP_EOL;
        } Lain {
            mencetak "Terjadi kesalahan. Restart." . PHP_EOL;
        }
        mencetak "ExitCode". $ ExitCode. PHP_EOL. PHP_EOL;
        exit ($ ExitCode);
    }
    tidur (1);
    $ Kontra ++;
}
~~~

Menjalankan pekerja ini dengan kode keluar set ke 2 di Mengikuti The Akan menghasilkan output dan pekerja berhenti Hakikat.

~~~
$ APP_NAME ironapp / DEP_NAME worker.add WORKER_NAME 2
$ APP_NAME ironapp / log pekerja DEP_NAME
[Tue Apr 12, 2011 09:15:54] WRK_ID Dimulai Pekerja (perintah: 'WORKER_NAME' parameter '2')
[Tue Apr 12, 2011 09:15:54] WRK_ID langkah: 1
[Tue Apr 12, 2011 09:15:55] WRK_ID langkah: 2
[Tue Apr 12, 2011 09:15:56] WRK_ID langkah: 3
[Tue 12 April 09:15:57 2011] WRK_ID langkah: 4
[Tue Apr 12, 2011 09:15:58] WRK_ID langkah: 5
[Tue Apr 12, 2011 09:15:58] WRK_ID Terjadi kesalahan. Keluar.
[...]
~~~
