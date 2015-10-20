# Mendapatkan Kredensial Add-on

Setiap deployment mendapatkan kredensial yang berbeda untuk masing-masing Add-on. Penyedia bisa
mengubah kredensial ini setiap saat, sehingga tidak harus di-hard-code dalam
source code. Jika kredensial tidak dalam source code, mereka juga tidak akan
muncul di kontrol versi dan menimbulkan potensi masalah keamanan.

Ada dua cara untuk mendapatkan [Add-on kredensial] dalam aplikasi Java.

## Membaca Kredensial dari Environtment Variable 

Secara default, setiap Add-on mengekspos identitasnya di environment. Anda bisa
mencari nama variabel di masing-masing dokumentasi Add-on. Untuk membacanya, cukup gunakan method `System.getenv()` dalam kode Anda.
Beberapa contoh untuk database Add-ons dapat dilihat di bagian terakhir.

Jika Anda tidak ingin mengekspos identitasnya tersebut di lingkungan, Anda bisa
menonaktifkan mereka dengan menjalankan:
~~~ Bash
$ ironapp APP_NAME/DEP_NAME config.add SET_ENV_VARS = false
~~~

Kredensial Add-on masih bisa dibaca dari file kredensial, seperti yang dijelaskan di bagian selanjutnya.

Perhatikan bahwa ada beberapa [environment variable] menarik lainnya
tersedia dalam deployment Anda.

## Membaca Kredensial dari File Kredensial

Semua [Kredensial Add-on] dapat ditemukan di sebuah tersedia JSON file juga, jalan mana yang terkena di
yang environtment variable `CRED_FILE`. Anda dapat melihat format file dengan perintah:
~~~ Bash
$ ironapp addon.creds APP_NAME/DEP_NAME
~~~

Kami menyediakan [helper class KilatIron credentials] sederhana untuk mendapatkan kredensial Add-on dari file tersebut.
Hal ini membutuhkan [json-simple], Java toolkit sederhana untuk encode atau decode JSON teks dengan mudah.
Untuk menggunakannya dalam proyek Anda, tambahkan pada maven:
~~~ Xml
<dependencies>
    <dependency>
        <groupId>com.googlecode.json-simple</groupId>
        <artifactId>json-simple</artifactId>
        <version>1.1</version>
    </dependency>
</dependencies>
~~~

Sekarang Anda bisa mendapatkan kredensial seperti ini:
~~~ Java
// e.g. untuk MySQLs
Credentials cr = Credentials.getInstance();
String database = (String)cr.getCredential("MYSQLS_DATABASE", "MYSQLS");
~~~

# Contoh

KilatIron menawarkan sejumlah solusi penyimpanan data melalui [Add-on Marketplace].
Di bawah ini Anda dapat menemukan contoh cara mengakses kredensial Add-on untuk MySQL.

## MySQL
Untuk menambahkan database MySQL, gunakan [MySQL Shared Add-on].

Berikut adalah potongan kode Java yang membaca pengaturan database dari environment variable:
~~~ Java
String database = System.getenv("MYSQLS_DATABASE");
String host     = System.getenv("MYSQLS_HOSTNAME");
int port        = Integer.valueOf(System.getenv("MYSQLS_PORT"));
String username = System.getenv("MYSQLS_USERNAME");
String password = System.getenv("MYSQLS_PASSWORD");
~~~
Ingat, Anda selalu dapat merujuk pada addon.creds perintah untuk melihat nama-nama variabel yang sebenarnya dan nilai-nilai.

[Aplikasi Java dengan MySQL]: https://github.com/cloudControl/java-mysql-example-app
[Add-on Marketplace]: http://www.cloudkilat.com/
[environment variable]: /Platform%20Documentation.md/#environment-variables
[Kredensial Add-on]: /Platform%20Documentation.md/#add-on-credentials
[Cred-env-vars]: /Platform%20Documentation.md/#enablingdisabling-credentials-environment-variables
[Json-simple]: http://code.google.com/p/json-simple/
[helper class KilatIron credentials]: https://gist.github.com/b350762c61fcc069b427
[MySQL Shared Add-on]: /Add-on%20Documentation/Data%20Storage/MySQLs.md
