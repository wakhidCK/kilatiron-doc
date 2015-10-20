# Menyebarkan Java / Jetty Aplikasi

Jika Anda sedang mencari cepat dan ringan Java web server / Servlet wadah untuk proyek Anda, Anda pasti harus mencoba [Jetty].

Dalam tutorial ini kita akan menunjukkan kepada Anda bagaimana untuk menggunakan aplikasi Jetty pada [CloudKilat]. Anda dapat menemukan [kode sumber dari Github] (https://github.com/cloudControl/java-jetty-jsp-example-app.git) dan memeriksa [Java buildpack] fitur untuk didukung.


## The Jetty Aplikasi Dijelaskan
### Dapatkan App
Pertama, mengkloning aplikasi hello world dari repositori kami:

~~~ Pesta
$ Git https://github.com/cloudControl/java-jetty-jsp-example-app.git
$ Cd java-dermaga-jsp-contoh-aplikasi
~~~

Sekarang Anda memiliki aplikasi Java / Jetty kecil tapi berfungsi penuh.


### Ketergantungan Tracking
Untuk membuat aplikasi ini kita harus menyediakan kerangka kerja Spring dan Maven Log4j sebagai dependensi dalam `pom.xml`.
~~~ Xml
<Ketergantungan>
    <GroupId> org.springframework </ groupId>
    <ArtifactId> semi-core </ artifactId>
    <Version> $ {org.springframework.version} </ version>
</ Ketergantungan>
<Ketergantungan>
    <GroupId> org.springframework </ groupId>
    <ArtifactId> semi-webmvc </ artifactId>
    <Version> $ {org.springframework.version} </ version>
</ Ketergantungan>
<Ketergantungan>
    <GroupId> org.springframework </ groupId>
    <ArtifactId> semi-konteks </ artifactId>
    <Version> $ {org.springframework.version} </ version>
</ Ketergantungan>
<Ketergantungan>
    <GroupId> org.springframework </ groupId>
    <ArtifactId> semi-biji </ artifactId>
    <Version> $ {org.springframework.version} </ version>
</ Ketergantungan>
<Ketergantungan>
    <GroupId> log4j </ groupId>
    <ArtifactId> log4j </ artifactId>
    <Versi> 1.2.17 </ version>
</ Ketergantungan>
<Ketergantungan>
    <GroupId> org.slf4j </ groupId>
    <ArtifactId> slf4j-log4j13 </ artifactId>
    <Versi> 1.0.1 </ version>
</ Ketergantungan>
~~~

[Maven ketergantungan Plugin] juga diperlukan dalam `pom.xml` untuk menyalin semua dependensi ke direktori target untuk membuat mereka tersedia di classpath. Sebuah repositori Maven lokal tidak termasuk dalam membangun citra.

~~~ Xml
<Plugin>
    <GroupId> org.apache.maven.plugins </ groupId>
    <ArtifactId> maven-ketergantungan-plugin </ artifactId>
    <Versi> 2.4 </ version>
    <Eksekusi>
        <Eksekusi>
            <Id> copy-dependensi </ id>
            <Fase> paket </ fase>
            <Tujuan> <tujuan> copy-dependensi </ tujuan> </ tujuan>
        </ Eksekusi>
    </ Eksekusi>
</ Plugin>
~~~

### Proses Type Definition
CloudKilat menggunakan [Procfile] tahu bagaimana untuk memulai proses Anda.

Contoh kode yang sudah termasuk `Procfile` di tingkat atas repositori Anda. Ini terlihat seperti ini:

~~~
web: java $ JAVA_OPTS jar sasaran / ketergantungan / jetty-runner.jar --port sasaran $ PORT / java-dermaga-jsp-contoh-aplikasi 0.0.1-SNAPSHOT.war
~~~

The `tipe proses web` diperlukan dan menentukan perintah yang akan dijalankan ketika aplikasi ini digunakan.
Perintah java memulai 'com.exo.sample.jetty.App' dengan classpath ditetapkan untuk kelas Java dikompilasi dan dependensi.

## Mendorong dan Menyebarkan App Anda
Pilih nama yang unik untuk menggantikan `APP_NAME` tempat untuk aplikasi Anda dan membuatnya pada platform CloudKilat:

~~~ Pesta
$ Ironcliapp APP_NAME buat java
~~~

Mendorong kode Anda ke repositori aplikasi, yang memicu penyebaran gambar proses build:


~~~ Pesta
$ Ironcliapp APP_NAME / dorongan bawaan

-----> Mendorong Menerima
-----> Instalasi OpenJDK 1,7 (openjdk7.b32.tar.gz) ... dilakukan
-----> Instalasi Maven (maven_3_1_with_cache_1.tar.gz) ... dilakukan
-----> Instalasi settings.xml ... dilakukan
-----> Mengeksekusi /srv/tmp/buildpack-cache/.maven/bin/mvn -B -Duser.home = / srv / tmp / builddir -Dmaven.repo.local = / srv / tmp / buildpack-Cache /.m2/repository -s /srv/tmp/buildpack-cache/.m2/settings.xml -DskipTests = true instalasi yang bersih
       [INFO] Scanning untuk proyek-proyek ...
       [INFO]
       [INFO] ----------------------------------------------- ---------------
       [INFO] Bangunan APP_NAME 1.0-SNAPSHOT
       [INFO] ----------------------------------------------- ---------------
       ...
       [INFO] ----------------------------------------------- ---------------
       [INFO] MEMBANGUN SUKSES
       [INFO] ----------------------------------------------- ---------------
       [INFO] Total waktu: 5: 57.950s
       [INFO] Selesai di: Fri 11 Juli 14:09:05 UTC 2013
       [INFO] Akhir Memory: 10M / 56m
-----> Gambar Building
-----> Gambar Mengunggah (39m)

Untuk ssh: //APP_NAME@kilatiron.net/repository.git
   54b0da2..d247825 Master -> Master
~~~

Terakhir namun tidak sedikit menyebarkan versi terbaru dari aplikasi dengan ironapp yang menyebarkan perintah:

~~~ Pesta
$ Ironcliapp APP_NAME / default menyebarkan
~~~

Selamat, Anda sekarang dapat melihat aplikasi Jetty Anda berjalan pada `http [s]: // APP_NAME.kilatiron.net`.

[Jetty]: http://jetty.codehaus.org/jetty/
[CloudKilat]: http://www.cloudkilat.com/
[Java buildpack]: https://github.com/cloudControl/buildpack-java
[CloudKilat-baris perintah-client]: /Platform%20Documentaion.md/#command-line-client-web-console-and-api
[Git klien]: http://git-scm.com/
[Maven ketergantungan Plugin]: http://maven.apache.org/plugins/maven-dependency-plugin/
[Procfile]: /Platform%20Documentaion.md/#buildpacks-and-the-procfile
