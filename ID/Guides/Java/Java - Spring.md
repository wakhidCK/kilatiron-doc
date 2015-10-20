#Deploying Aplikasi Musim Semi

Dalam tutorial ini kita akan menunjukkan kepada Anda bagaimana untuk menggunakan Spring aplikasi / MVC / Hibernate pada [CloudKilat]. Contoh aplikasi adalah siap untuk menyebarkan proyek berdasarkan [musim semi Roo petclinic] contoh.

## Aplikasi musim semi Dijelaskan

### Dapatkan App


Pertama, mengkloning aplikasi semi dari repositori kami:

~~~ Pesta
$ Git clone https://github.com/cloudControl/java-spring-hibernate-example-app
$ Cd java-semi-hibernasi-contoh-aplikasi
~~~


### Server Produksi

The [Jetty Runner] menyediakan cara yang mudah dan cepat untuk menjalankan aplikasi Anda di server aplikasi. Kami telah menambahkan ketergantungan ke bagian membangun plugin di `pom.xml`:

~~~ Xml
...
        <Plugin>
            <GroupId> org.apache.maven.plugins </ groupId>
            <ArtifactId> maven-ketergantungan-plugin </ artifactId>
            <Versi> 2.3 </ version>
            <Eksekusi>
                <Eksekusi>
                    <Fase> paket </ fase>
                    <Tujuan>
                        <Tujuan> copy </ tujuan>
                    </ Tujuan>
                    <Configuration>
                        <ArtifactItems>
                            <ArtifactItem>
                                <GroupId> org.mortbay.jetty </ groupId>
                                <ArtifactId> jetty-pelari </ artifactId>
                                <Version> 7.4.5.v20110725 </ version>
                                <DestFileName> jetty-runner.jar </ destFileName>
                            </ ArtifactItem>
                        </ ArtifactItems>
                    </ Configuration>
                </ Eksekusi>
            </ Eksekusi>
        </ Plugin>
    </ Plugin>
</ Membangun>
~~~



### Database Produksi

Dalam tutorial ini kita menggunakan [Bersama MySQL Add-on]. Kami telah mengubah `src / main / sumber / META-INF / semi / applicationContext.xml` membaca [kredensial database] disediakan oleh MySQLs Add-on:

~~~ Xml
<Property name = "url" value = "jdbc: mysql: // $ {} MYSQLS_HOSTNAME: $ {} MYSQLS_PORT / $ {} MYSQLS_DATABASE" />
<Properti name = "username" value = "$ {} MYSQLS_USERNAME" />
<Properti name = "password" value = "$ {} MYSQLS_PASSWORD" />
~~~

### Sesuaikan Logger Konfigurasi

Logging ke file tidak dianjurkan karena [file system] wadah ini tidak terus-menerus.
Konfigurasi default logger - `src / main / sumber / log4j.properties` dimodifikasi untuk login ke` stdout / stderr`.
Kemudian CloudKilat dapat mengambil semua pesan dan memberikan mereka kepada Anda melalui [log perintah]. Ini adalah bagaimana file terlihat sekarang:
~~~ Xml
og4j.rootLogger = DEBUG, stdout
log4j.appender.stdout = org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout = org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern =% p [% t] (% c) - m%% n%
~~~

### Proses Type Definition

CloudKilat menggunakan `Procfile` untuk memulai aplikasi. The `Procfile` di root proyek karena menentukan perintah yang mengeksekusi Jetty Runner:

~~~
perang java $ JAVA_OPTS jar sasaran / ketergantungan / jetty-runner.jar --port sasaran $ PORT / *: web.
~~~


## Mendorong dan Menyebarkan App Anda

Pilih nama yang unik (dari sekarang disebut APP_NAME) untuk aplikasi Anda dan membuatnya pada platform CloudKilat:

~~~ Pesta
$ Ironcliapp APP_NAME buat java
~~~

Mendorong kode Anda ke repositori aplikasi:

~~~ Pesta
$ Ironcliapp APP_NAME / dorongan bawaan
Menghitung objek: 223, dilakukan.
Delta kompresi menggunakan sampai 4 benang.
Mengompresi objek: 100% (212/212), dilakukan.
Menulis objek: 100% (223/223), 99,59 KiB, dilakukan.
Total 223 (delta 107), kembali 0 (delta 0)

-----> Mendorong Menerima
-----> Instalasi OpenJDK 1,7 (openjdk7.b32.tar.gz) ... dilakukan
-----> Instalasi Maven (maven_3_1_with_cache_1.tar.gz) ... dilakukan
-----> Instalasi settings.xml ... dilakukan
-----> Mengeksekusi /srv/tmp/buildpack-cache/.maven/bin/mvn -B -Duser.home = / srv / tmp / builddir -Dmaven.repo.local = / srv / tmp / buildpack-Cache /.m2/repository -s /srv/tmp/buildpack-cache/.m2/settings.xml -DskipTests = true instalasi yang bersih
       [INFO] Scanning untuk proyek-proyek ...
       [INFO]
       [INFO] ----------------------------------------------- ----------------
       [INFO] Bangunan petclinic 0.1.0-SNAPSHOT
       [INFO] ----------------------------------------------- ----------------
       ...
       [INFO] Kemasan webapp
       [INFO] Perakitan webapp [petclinic] di [/srv/tmp/builddir/target/petclinic-0.1.0.BUILD-SNAPSHOT]
       [INFO] proyek perang Pengolahan
       [INFO] sumber Menyalin webapp [/ srv / tmp / builddir / src / main / webapp]
       [INFO] webapp dirakit di [365 msecs]
       Perang [INFO] Bangunan: /srv/tmp/builddir/target/petclinic-0.1.0.BUILD-SNAPSHOT.war
       [INFO] WEB-INF / web.xml sudah ditambahkan, melompat-lompat
       [INFO]
       [INFO] --- maven-ketergantungan-plugin: 2.3: copy (default) @ petclinic ---
       ...
       [INFO] ----------------------------------------------- ----------------
       [INFO] MEMBANGUN SUKSES
       [INFO] ----------------------------------------------- ----------------
       [INFO] Total waktu: 3: 38.174s
       [INFO] Selesai di: Thu Juli 20 11:23:02 UTC 2013
       [INFO] Akhir Memory: 20M / 229M
       [INFO] ----------------------------------------------- ----------------
-----> Gambar Building
-----> Gambar Mengunggah (84m)

Untuk ssh: //APP_NAME@kilatiron.net/repository.git
 * [Cabang baru] Master -> Master
~~~

Tambahkan MySQLs Add-on dengan rencana bebas untuk penyebaran dan menyebarkan:

~~~ Pesta
$ Ironcliapp APP_NAME / default addon.add mysqls.free
$ Ironcliapp APP_NAME / default menyebarkan --memory = 768MB
~~~

The `--memory = 768MB` argumen meningkatkan ukuran wadah untuk memenuhi konsumsi memori tinggi dari kerangka Spring. Harap dicatat: meningkatkan ukuran datang dengan biaya tambahan.

Et voila, app sekarang dan berjalan di `http [s]: // APP_NAME.kilatiron.net`.


[Musim semi Roo petclinic]: http://static.springsource.org/spring-roo/reference/html/intro.html#intro-exploring-sample
[Database kredensial]: /Guides/Java/Add-on%20credentials.md
[Jetty Runner]: http://wiki.eclipse.org/Jetty/Howto/Using_Jetty_Runner
[CloudKilat]: http://www.cloudkilat.com/
[Sistem file]: /Platform%20Documentation.md/#non-persistent-filesystem
[Log perintah]: /Platform%20Documentation.md//#logging
[Bersama MySQL Add-on]: /Add-on%20Documentation/Data%20Storage/MySQLs.md
