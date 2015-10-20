Integrasi # Node.js Amazon S3

[Amazon S3] adalah Storage-as-a-Service solusi. Ini menyediakan antarmuka layanan web sederhana yang dapat digunakan untuk menyimpan dan mengambil data dari mana saja di web.

Panduan ini menunjukkan bagaimana mengintegrasikan Amazon S3 dengan aplikasi Node.js Anda.

## Amazon S3 SDK
Untuk Node.js Anda dapat memilih antara SDK yang berbeda untuk Amazon S3:
* [Amazon S3]
* [Kabut]
* [Knox S3]

## Persiapan
Untuk menggunakan resmi Amazon S3 SDK dalam proyek Anda, Anda harus menginstal AWS SDK untuk Node.js menggunakan [NPM manajer paket].
Untuk menginstal SDK, ketik berikut ke jendela terminal:

~~~ Pesta
NPM menginstal aws-sdk
~~~

Selain AWS SDK, Anda juga perlu memiliki akses AWS kredensial. Jika Anda tidak memilikinya, mengikuti [Amazon Panduan] untuk membuat akun dan mendapatkan Anda [AWS akses kredensial].

## Contoh Penggunaan
S3 membutuhkan mandat AWS Anda untuk akses. Cara yang disarankan untuk memberikan mandat AWS untuk aplikasi Anda adalah melalui variabel lingkungan. Untuk melakukan hal ini, gunakan [Config Add-on]:

~~~ Pesta
ironapp APP_NAME / default config.add
AWS_ACCESS_KEY_ID = [YOUR_SECRET_KEY]
AWS_SECRET_ACCESS_KEY = [YOUR_ACCESS_KEY]
AWS_REGION = 'eu-barat 1'
~~~

Untuk memuat perpustakaan AWS di app Node.js Anda, gunakan memerlukan fungsi seperti berikut:

~~~ Javascript
AWS var = require ('aws-sdk');
var s3 = AWS.S3 baru ();
~~~

Sekarang, mari kita melakukan beberapa operasi pada S3 menggunakan Node.js. Pada contoh di bawah, kita menunjukkan bagaimana untuk membuat ember baru, daftar ember yang ada, tambahkan kunci ke dalam ember, membaca kunci dari ember, menghapus kunci dari ember, dan menghapus ember.

~~~ Javascript
   // Buat myBucket S3 ember bernama
   s3.createBucket ({Bucket: 'myBucket'}, fungsi (err, data) {
    jika (err) melemparkan kesalahan baru (err);
   });
    
   // Daftar yang ada ember S3
   s3.ListBuckets (function (err, data) {
    jika (err) melemparkan kesalahan baru (err);

    ember var = data.Body.ListAllMyBucketsResult.Buckets.Bucket;
    buckets.forEach (function (bucket) {
        console.log ('% s:% s', bucket.CreationDate, bucket.Name);
    });
   });

   // Menambahkan kunci myBucket
   putparams var = {Bucket: 'myBucket', Key: 'MyKey', tubuh: 'Halo'};
   s3.putObject (putparams, fungsi (err, data) {
       jika (err)
           console.log (err)
       lain console.log ("Berhasil upload data ke myBucket / MyKey");
    });

   // Baca kunci dari myBucket
   getparams var = {Bucket: 'myBucket', Key: 'MyKey'};
   s3.getObject (getparams, fungsi (err, url) {
  jika (err)
console.log (err)
lain console.log ("Kuncinya adalah", url);
   });

   // Hapus kunci dari myBucket
   delparams var = {Bucket: 'myBucket', Key: 'MyKey'};
   s3.deleteObject (delparams, fungsi (err, data) {
        console.log (err, data)
   });

   // Hapus ember myBucket
   s3.deleteBucket ({Bucket: ember}, fungsi (err, data) {
   jika (err)
           console.log ("error menghapus ember" + err);
   lain console.log ("menghapus ember" + data);
   });
~~~

## Langkah Berikutnya
Anda dapat membangun kaya Node.js aplikasi menggunakan operasi S3 lebih maju. Untuk mempelajari lebih lanjut, memeriksa Node.js [Amazon Panduan]. Semoga berhasil.

[Amazon S3]: http://aws.amazon.com/s3/
[Kabut]: https://docs.appfog.com/languages/node
[Knox S3]: https://github.com/LearnBoost/knox
[NPM manajer paket]: https://npmjs.org/
[Amazon Panduan]: http://docs.aws.amazon.com/AWSJavaScriptSDK/guide/node-intro.html
[AWS akses kredensial]: http://aws.amazon.com/security-credentials
[Config Add-on]: /Add-on%20Documentation/Deployment/Custom%20Config.md
