# Add-on Worker

Worker adalah proses yang berjalan lama di background. Mereka bisa digunakan untuk apapun, dari mengirim email ke menjalankan komputasi berat atau membangun kembali cache di background.

Setiap worker dijalankan melalui add-on worker dan berjalan dalam container yang terisolasi. Container memiliki environment runtime yang sama didefinisikan oleh stack dan buildpack yang digunakan dan memiliki akses yang sama ke semua penyebaran add-on.

## Menambahkan Add-on Worker

Sebelum Anda dapat menggunakan worker, tambahkan add-on dengan perintah addon.add.

~~~
$ ironapp APP_NAME/DEP_NAME addon.add worker.single
~~~

## Mulai Worker a

Worker dapat dimulai melalui perintah worker.add baris pelanggan perintah ini.

Tentukan bagaimana memulai worker dengan menambahkan baris baru untuk aplikasi `Procfile` Anda, dan gunakan kolom paling kiri sebagai `WORKER_NAME`.

~~~
$ ironapp APP_NAME/DEP_NAME worker.add WORKER_NAME [WORKER_PARAMS]
~~~

Sertakan beberapa WORKER_PARAMS dalam tanda kutip ganda.

~~~
$ ironapp APP_NAME/DEP_NAME worker.add WORKER_NAME "param1 param2 PARAM3"
~~~

## Daftar Worker

Untuk mendapatkan daftar worker yang berjalan, gunakan perintah.

~~~
$ ironcliapp APP_NAME/DEP_NAME worker
Workers
 nr. wrk_id
   1 WRK_ID
~~~

Anda bisa mendapatkan detail worker dengan menambahkan perintah WRK_ID untuk worker.

~~~
$ ironcliapp APP_NAME/DEP_NAME worker WRK_ID
Worker
wrk_id   : WRK_ID
command  : WORKER_NAME
params   : "PARAM1 PARAM2 PARAM3"
~~~

## Menghentikan Worker

Worker dapat dihentikan dari perintah ironapp atau worker berhenti dengan exit code 0.

### Via Command Line

Untuk menghentikan worker menggunakan perintah worker.remove pada ironapp

~~~
$ ironapp APP_NAME/DEP_NAME worker.remove WRK_ID
~~~

Untuk mendapatkan WRK_ID lihat bagian atas worker daftar.

### Via Exit Codes

Untuk menghentikan pekerja programatik menggunakan UNIX exit code. Ada tiga kode keluar yang berbeda tersedia.

 * Exit(0); // Semuanya baik-baik saja. Worker akan dihentikan.
 * Exit(1); // Kesalahan. Worker akan dijalankan lagi.
 * Exit(2); // Kesalahan. Worker akan dihentikan.

Untuk lebih jelasnya Rujuk ke [contoh PHP] (#contoh-worker-php) di bawah ini.

Log Worker ##

Seperti yang sudah dijelaskan di [Logging] bagian (/Platform Documentation.md/#logging) stdout dan stderr keluaran semua pekerja yang Dialihkan ke log pekerja. Untuk melihat output dalam mode seperti perintah tail -f menggunakan log.

~~~
$ ironapp APP_NAME/DEP_NAME log worker
[Fri Dec 17 13:39:41 2010] WRK_ID Started Worker (command: 'WORKER_NAME', parameter: 'PARAM1 PARAM2 PARAM3')
[Fri Dec 17 13:39:42 2010] WRK_ID Hello PARAM1 PARAM2 PARAM3
[...]
~~~

## Menghapus Add-on Worker

Untuk menghapus add-on worker menggunakan perintah addon.remove.

~~~
$ ironapp APP_NAME/DEP_NAME addon.remove worker.single
~~~

## Contoh Worker PHP

Berikut contoh bagaimana menggunakan kode keluar untuk menghentikan atau memulai kembali pekerja.

~~~ php
// read exit code parameter
$exitCode = isset($argv[1]) && (int)$argv[1] > 0 ? (int)$argv[1] : 0;
$steps = 5;

$counter = 1;
while(true) {
    print "step: " . ($counter) . PHP_EOL;
    if($counter == $steps){
        if($exitCode == 0) {
            print "All O.K. Exiting." . PHP_EOL;
        } else if ($exitCode == 2){
            print "An error occured. Exiting." . PHP_EOL;
        } else {
            print "An error occured. Restarting." .  PHP_EOL;
        }
        print "Exitcode: " . $exitCode . PHP_EOL . PHP_EOL;
        exit($exitCode);
    }
    sleep(1);
    $counter++;
}
~~~

Menjalankan worker ini dengan exit code diset ke 2, worker akan berhenti dan menghasilkan output berikut :

~~~
$ ironapp APP_NAME/DEP_NAME worker.add WORKER_NAME 2
$ ironapp APP_NAME/DEP_NAME log worker
[Tue Apr 12 09:15:54 2011] WRK_ID Started Worker (command: 'WORKER_NAME', parameter: '2')
[Tue Apr 12 09:15:54 2011] WRK_ID step: 1
[Tue Apr 12 09:15:55 2011] WRK_ID step: 2
[Tue Apr 12 09:15:56 2011] WRK_ID step: 3
[Tue Apr 12 09:15:57 2011] WRK_ID step: 4
[Tue Apr 12 09:15:58 2011] WRK_ID step: 5
[Tue Apr 12 09:15:58 2011] WRK_ID An error occured. Exiting.
[...]
~~~
