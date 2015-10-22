# Aplikasi Java / Jetty

Jika Anda sedang mencari web server / Servlet Java yang ringan dan cepat untuk proyek Anda, Anda pasti harus mencoba [Jetty].

Dalam tutorial ini kita akan menunjukkan kepada Anda bagaimana untuk menggunakan aplikasi Jetty pada [KilatIron]. Anda dapat menemukan [kode sumber dari Github] (https://github.com/cloudControl/java-jetty-jsp-example-app.git) dan memeriksa [Java buildpack] mengetahui fitur-fitur untuk didukung.


## Aplikasi Jetty
### Dapatkan Aplikasi

Pertama, mengkloning aplikasi hello world dari repositori kami:

~~~bash
$ git https://github.com/cloudControl/java-jetty-jsp-example-app.git
$ cd java-jetty-jsp-example-app
~~~

Sekarang Anda memiliki aplikasi Java / Jetty sederhana.


### Melacak Ketergantungan 

Untuk membuat aplikasi ini kita harus menyediakan framework Spring dan Log4j sebagai dependensi dalam `pom.xml`.

~~~xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
    <version>${org.springframework.version}</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>${org.springframework.version}</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>${org.springframework.version}</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-beans</artifactId>
    <version>${org.springframework.version}</version>
</dependency>
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-log4j13</artifactId>
    <version>1.0.1</version>
</dependency>
~~~

[Maven Dependency Plugin] juga diperlukan dalam `pom.xml` untuk menyalin semua ketergantungan ke direktori target untuk membuat mereka tersedia di classpath. Repositori Maven lokal tidak termasuk dalam membangun container.

~~~xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-dependency-plugin</artifactId>
    <version>2.4</version>
    <executions>
        <execution>
            <id>copy-dependencies</id>
            <phase>package</phase>
            <goals><goal>copy-dependencies</goal></goals>
        </execution>
    </executions>
</plugin>
~~~

### Proses Type Definition

KilatIron menggunakan [Procfile] untuk mengetahui bagaimana cara memulai proses aplikasi Anda.

Pada contoh kode terdapat `Procfile` di tingkat atas repositori Anda. File tersebut berisi:

~~~
web: java $JAVA_OPTS -jar target/dependency/jetty-runner.jar --port $PORT target/java-jetty-jsp-example-app-0.0.1-SNAPSHOT.war
~~~

Tipe proses `web` diperlukan untuk menentukan perintah yang akan dijalankan ketika aplikasi ini digunakan.
Perintah java memulai 'com.exo.sample.jetty.App' dengan classpath sudah diset untuk kelas Java yang sudah dikompilasi dan dependensi-dependensinya.

## Push dan Deploy Aplikasi

Pilih nama yang unik untuk menggantikan `APP_NAME` untuk aplikasi Anda dan membuatnya pada platform KilatIron:

~~~bash
$ ironapp APP_NAME create java
~~~

Push kode Anda ke repositori aplikasi, yang memicu proses pembuatan image container:


~~~bash
$ ironcliapp APP_NAME/default push

-----> Receiving push
-----> Installing OpenJDK 1.7(openjdk7.b32.tar.gz)... done
-----> Installing Maven (maven_3_1_with_cache_1.tar.gz)... done
-----> Installing settings.xml... done
-----> executing /srv/tmp/buildpack-cache/.maven/bin/mvn -B -Duser.home=/srv/tmp/builddir -Dmaven.repo.local=/srv/tmp/buildpack-cache/.m2/repository -s /srv/tmp/buildpack-cache/.m2/settings.xml -DskipTests=true clean install
       [INFO] Scanning for projects...
       [INFO]
       [INFO] --------------------------------------------------------------
       [INFO] Building APP_NAME 1.0-SNAPSHOT
       [INFO] --------------------------------------------------------------
       ...
       [INFO] --------------------------------------------------------------
       [INFO] BUILD SUCCESS
       [INFO] --------------------------------------------------------------
       [INFO] Total time: 5:57.950s
       [INFO] Finished at: Fri Jul 11 14:09:05 UTC 2013
       [INFO] Final Memory: 10M/56M
-----> Building image
-----> Uploading image (39M)

To ssh://APP_NAME@kilatiron.net/repository.git
   54b0da2..d247825  master -> master
~~~

Yang terakhir harus dilakukan adalah menyebarkan versi terbaru dari aplikasi dengan perintah ironapp deploy:

~~~bash
$ ironapp APP_NAME/default deploy
~~~

Selamat, Anda sekarang dapat melihat aplikasi Jetty Anda berjalan di `http[s]://APP_NAME.kilatiron.net`.

[Jetty]: http://jetty.codehaus.org/jetty/
[KilatIron]: http://www.cloudkilat.com/
[Java buildpack]: https://github.com/cloudControl/buildpack-java
[Maven Dependency Plugin]: http://maven.apache.org/plugins/maven-dependency-plugin/
[Procfile]: /Platform%20Documentaion.md/#buildpacks-and-the-procfile
