# Mendapatkan Add-on Kredensial yang

Setiap penyebaran mendapat mandat yang berbeda untuk masing-masing Add-on. Penyedia bisa
mengubah mandat ini setiap saat, sehingga mereka tidak harus keras-kode dalam
kode sumber. Jika kredensial tidak dalam kode sumber, mereka juga tidak akan
muncul di kontrol versi dan menyebabkan masalah keamanan potensial.

Ada dua cara untuk mendapatkan [Add-on kredensial] dalam aplikasi Java.

## Membaca Kredensial dari Variabel Lingkungan

Secara default, setiap Add-on mengekspos identitasnya di lingkungan. Kamu bisa
mencari lingkungan individu nama variabel di masing Add-on
dokumentasi. Untuk membacanya, cukup gunakan System.getenv () metode `` dalam kode Anda.
Beberapa contoh untuk database Add-ons dapat dilihat di bagian terakhir.

Jika Anda tidak ingin mengekspos identitasnya tersebut di lingkungan, Anda bisa
menonaktifkan mereka dengan menjalankan:
~~~ Pesta
$ Ironcliapp APP_NAME / DEP_NAME config.add SET_ENV_VARS = false
~~~

Add-on kredensial masih bisa dibaca dari mandat mengajukan, seperti yang dijelaskan di bagian selanjutnya.

Perhatikan bahwa ada beberapa [variabel lingkungan] menarik lainnya
tersedia dalam wadah penyebaran Anda.

## Membaca Kredensial dari Kredensial Berkas

Semua [Add-on kredensial] dapat ditemukan di sebuah tersedia JSON file juga, yang jalan
terkena di `variabel lingkungan CRED_FILE`. Anda dapat melihat format file yang lokal:

~~~ Pesta
$ Ironcliapp addon.creds APP_NAME / DEP_NAME
~~~

Kami menyediakan kecil [kredensial CloudKilat penolong kelas] untuk mendapatkan Add-on kredensial dari file tersebut.
Hal ini membutuhkan [json-sederhana], Java toolkit sederhana untuk mengkodekan atau decode JSON teks dengan mudah.
Untuk menggunakannya dalam proyek Anda, menambahkannya sebagai ketergantungan maven:
~~~ Xml
<Dependensi>
    <Ketergantungan>
        <GroupId> com.googlecode.json-sederhana </ groupId>
        <ArtifactId> json-sederhana </ artifactId>
        <Versi> 1.1 </ version>
    </ Ketergantungan>
</ Dependensi>
~~~

Sekarang Anda bisa mendapatkan mandat seperti ini:
~~~ Java
// Misal untuk MySQLs
Kredensial cr = Credentials.getInstance ();
String Database = (String) cr.getCredential ("MYSQLS_DATABASE", "MYSQLS");
~~~

# Contoh

CloudKilat menawarkan sejumlah solusi penyimpanan data melalui [Add-on Marketplace].
Di bawah ini Anda dapat menemukan contoh tentang cara untuk mengakses Add-on
mandat untuk MySQL.

## MySQL
Untuk menambahkan database MySQL, gunakan [MySQL Bersama Add-on].

Berikut adalah potongan Java yang membaca pengaturan database dari variabel lingkungan:
~~~ Java
String Database = System.getenv ("MYSQLS_DATABASE");
String host = System.getenv ("MYSQLS_HOSTNAME");
int port = Integer.valueOf (System.getenv ("MYSQLS_PORT"));
String username = System.getenv ("MYSQLS_USERNAME");
Sandi String = System.getenv ("MYSQLS_PASSWORD");
~~~
Ingat, Anda selalu dapat merujuk pada addon.creds perintah untuk melihat nama-nama variabel yang sebenarnya dan nilai-nilai.

[Aplikasi Java dengan MySQL]: https://github.com/cloudControl/java-mysql-example-app
[Add-on Marketplace]: http://www.cloudkilat.com/
[Variabel lingkungan]: /Platform%20Documentation.md/#environment-variables
[Add-on kredensial]: /Platform%20Documentation.md/#add-on-credentials
[Cred-env-vars]: /Platform%20Documentation.md/#enablingdisabling-credentials-environment-variables
[Json-sederhana]: http://code.google.com/p/json-simple/
[CloudKilat kredensial helper class]: https://gist.github.com/b350762c61fcc069b427
[MySQL Bersama Add-on]: /Add-on%20Documentation/Data%20Storage/MySQLs.md
