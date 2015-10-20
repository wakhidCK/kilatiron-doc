# Java Amazon S3 integrasi

[Amazon S3] (http://aws.amazon.com/s3/) adalah Storage-as-a-Service solusi. Ini menyediakan antarmuka layanan web sederhana yang dapat digunakan untuk menyimpan dan mengambil data dari mana saja di web.

## Amazon S3 SDK

Untuk Java Anda dapat memilih antara SDK yang berbeda untuk Amazon S3:
* [Amazon S3 Java SDK] (http://aws.amazon.com/sdkforjava/)
* [JetS3t] (http://jets3t.s3.amazonaws.com/index.html)
* [S3lib] (http://code.google.com/p/s3lib/)
* [Jclouds] (http://www.jclouds.org/)

## Persiapan

Untuk menggunakan resmi Amazon S3 SDK dalam proyek Anda, hanya menentukan ketergantungan Maven tambahan dalam `pom.xml` Anda:

~~~ Xml
<Ketergantungan>
<GroupId> com.amazonaws </ groupId>
<ArtifactId> aws-java-sdk </ artifactId>
<Versi> 1.3.27 </ version>
</ Ketergantungan>
~~~

Ikuti [Amazon Panduan] (http://docs.aws.amazon.com/AWSSdkDocsJava/latest/DeveloperGuide/java-dg-setup.html) untuk membuat akun dan mendapatkan Anda [AWS akses kredensial] (http: // aws.amazon.com/security-credentials).

## Contoh penggunaan:

Cara yang disarankan untuk memberikan mandat AWS untuk aplikasi Anda adalah melalui variabel lingkungan. Untuk melakukan hal ini, gunakan [Config Add-on] (/ Add-on Dokumentasi / Deployment / Kustom Config.md):

~~~ Pesta
$ Ironcliapp APP_NAME / default config.add AWS_SECRET_KEY = [YOUR_SECRET_KEY] AWS_ACCESS_KEY = [YOUR_ACCESS_KEY]
~~~

Sekarang mari kita menunjukkan beberapa operasi pada ember dan benda-benda:

~~~ Java

// Dapatkan kredensial akses dari variabel lingkungan
AWSCredentials creds = AWSCredentials baru () {
    @ Override
    public String getAWSAccessKeyId () {
        kembali System.getenv ("AWS_ACCESS_KEY");
    }

    @ Override
    public String getAWSSecretKey () {
        kembali System.getenv ("AWS_SECRET_KEY");
    }
};

// S3 koneksi klien
AmazonS3 s3 = AmazonS3Client baru (creds);
String akhir EMBER = "testbucket" + UUID.randomUUID ();
KEY final String = "kunci";

// Buat ember
s3.createBucket (BUCKET);

// Daftar ember
Daftar <Bucket> ember = s3.listBuckets ();
System.out.println ("Ember:" + ember);

// Objek Masukan
s3.putObject (baru PutObjectRequest (EMBER, KEY, Berkas baru ("tmp.txt")));

// Baca objek
Objek S3Object = s3.getObject (baru GetObjectRequest (EMBER, KEY));
String contentType = object.getObjectMetadata () getContentType ().;
S3ObjectInputStream objectStream = object.getObjectContent ();
String content = getContent (objectStream);
System.out.println ("Jenis:" + contentType + "\ nContent:" + isi);

// Hapus objek
s3.deleteObject (EMBER, KEY);

// Hapus ember
s3.deleteBucket (baru DeleteBucketRequest (EMBER));
~~~

Kami menggunakan fungsi ini helper sederhana untuk membaca isi dari objek S3:

~~~ Java
private static String getContent (S3ObjectInputStream fin) throws IOException {
    int ch;
    Pembangun StringBuilder = new StringBuilder ();
    sementara ((ch = fin.read ())! = -1) {
        builder.append ((char) ch);
    }
    kembali builder.toString ();
}
~~~
