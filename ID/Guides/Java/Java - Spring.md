# Aplikasi Spring

Dalam tutorial ini kita akan menunjukkan kepada Anda bagaimana cara menggunakan aplikasi Spring/MVC/Hibernate pada [KilatIron]. Contoh aplikasi siap untuk digunakan berdasarkan contoh [Spring Roo petclinic].

## Aplikasi Spring

### Dapatkan Aplikasi

Pertama, mengkloning aplikasi Spring dari repositori kami:

~~~bash
$ git clone https://github.com/cloudControl/java-spring-hibernate-example-app
$ cd java-spring-hibernate-example-app
~~~


### Server Produksi

The [Jetty Runner] menyediakan cara yang mudah dan cepat untuk menjalankan aplikasi Anda di aplikasi server. Kami telah menambahkan ketergantungan ke bagian membangun plugin di `pom.xml`:

~~~xml
...
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-dependency-plugin</artifactId>
            <version>2.3</version>
            <executions>
                <execution>
                    <phase>package</phase>
                    <goals>
                        <goal>copy</goal>
                    </goals>
                    <configuration>
                        <artifactItems>
                            <artifactItem>
                                <groupId>org.mortbay.jetty</groupId>
                                <artifactId>jetty-runner</artifactId>
                                <version>7.4.5.v20110725</version>
                                <destFileName>jetty-runner.jar</destFileName>
                            </artifactItem>
                        </artifactItems>
                    </configuration>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
~~~



### Database

Dalam tutorial ini kita akan menggunakan [MySQL Shared Add-on]. Kami telah mengubah `src/main/resources/META-INF/spring/applicationContext.xml` untuk membaca [kredensial database] yang disediakan oleh Add-on MySQLs:

~~~xml
<property name="url" value="jdbc:mysql://${MYSQLS_HOSTNAME}:${MYSQLS_PORT}/${MYSQLS_DATABASE}"/>
<property name="username" value="${MYSQLS_USERNAME}"/>
<property name="password" value="${MYSQLS_PASSWORD}"/>
~~~

### Sesuaikan Konfigurasi Logger

Logging ke file tidak dianjurkan karena [file system] pada container ini tidak bersifat persistent.
Konfigurasi default logger - `src/main/resources/log4j.properties` diubah untuk log ke`stdout/stderr`.
Kemudian KilatIron dapat mengambil semua pesan dan memberikan kepada Anda melalui [perintah log]. Ini adalah konten dari file tersebut:

~~~xml
log4j.rootLogger=DEBUG, stdout
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%p [%t] (%c) - %m%n%
~~~

### Proses Type Definition

KilatIron menggunakan [Procfile] untuk mengetahui bagaimana cara memulai proses aplikasi Anda.

Pada contoh kode terdapat `Procfile` di tingkat atas repositori Anda yang berisi command untuk menjalankan Jetty Runner.

~~~
web: java $JAVA_OPTS -jar target/dependency/jetty-runner.jar --port $PORT target/*.war
~~~


## Push dan Deploy Aplikasi

Pilih nama yang unik untuk menggantikan `APP_NAME` untuk aplikasi Anda dan membuatnya pada platform KilatIron:

~~~bash
$ ironapp APP_NAME create java
~~~

Push kode Anda ke repositori aplikasi, yang memicu proses pembuatan image container:

~~~bash
$ ironcliapp APP_NAME/default push
Counting objects: 223, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (212/212), done.
Writing objects: 100% (223/223), 99.59 KiB, done.
Total 223 (delta 107), reused 0 (delta 0)

-----> Receiving push
-----> Installing OpenJDK 1.7(openjdk7.b32.tar.gz)... done
-----> Installing Maven (maven_3_1_with_cache_1.tar.gz)... done
-----> Installing settings.xml... done
-----> executing /srv/tmp/buildpack-cache/.maven/bin/mvn -B -Duser.home=/srv/tmp/builddir -Dmaven.repo.local=/srv/tmp/buildpack-cache/.m2/repository -s /srv/tmp/buildpack-cache/.m2/settings.xml -DskipTests=true clean install
       [INFO] Scanning for projects...
       [INFO]
       [INFO] ---------------------------------------------------------------
       [INFO] Building petclinic 0.1.0-SNAPSHOT
       [INFO] ---------------------------------------------------------------
       ...
       [INFO] Packaging webapp
       [INFO] Assembling webapp [petclinic] in [/srv/tmp/builddir/target/petclinic-0.1.0.BUILD-SNAPSHOT]
       [INFO] Processing war project
       [INFO] Copying webapp resources [/srv/tmp/builddir/src/main/webapp]
       [INFO] Webapp assembled in [365 msecs]
       [INFO] Building war: /srv/tmp/builddir/target/petclinic-0.1.0.BUILD-SNAPSHOT.war
       [INFO] WEB-INF/web.xml already added, skipping
       [INFO]
       [INFO] --- maven-dependency-plugin:2.3:copy (default) @ petclinic ---
       ...
       [INFO] ---------------------------------------------------------------
       [INFO] BUILD SUCCESS
       [INFO] ---------------------------------------------------------------
       [INFO] Total time: 3:38.174s
       [INFO] Finished at: Thu Juli 20 11:23:02 UTC 2013
       [INFO] Final Memory: 20M/229M
       [INFO] ---------------------------------------------------------------
-----> Building image
-----> Uploading image (84M)

To ssh://APP_NAME@kilatiron.net/repository.git
 * [new branch]      master -> master
~~~

Tambahkan Add-on MySQLs untuk deployment dan deploy Add-on tersebut:

~~~bash
$ ironcliapp APP_NAME/default addon.add mysqls.free
$ ironcliapp APP_NAME/default deploy --memory=768MB
~~~

Argumen `--memory = 768MB` meningkatkan RAM dari container untuk memenuhi kebutuhan konsumsi memori tinggi dari framework Spring. Catatan: Dengan meningkatkan ukuran, aplikasi Anda akan membutuhkan biaya tambahan.

Selamat, Anda sekarang dapat melihat aplikasi Spring Anda berjalan di `http[s]://APP_NAME.kilatiron.net`.


[Spring Roo petclinic]: http://static.springsource.org/spring-roo/reference/html/intro.html#intro-exploring-sample
[kredensial Database]: /Guides/Java/Add-on%20credentials.md
[Jetty Runner]: http://wiki.eclipse.org/Jetty/Howto/Using_Jetty_Runner
[KilatIron]: http://www.cloudkilat.com/
[file system]: /Platform%20Documentation.md/#non-persistent-filesystem
[perintah log]: /Platform%20Documentation.md//#logging
[MySQL Shared Add-on]: /Add-on%20Documentation/Data%20Storage/MySQLs.md
