# Kustom Config Add-on

Custom Pengaturan Add-on Memungkinkan Anda untuk menambahkan mandat kustom untuk standar
File creds.json Disediakan Untuk Setiap penyebaran Anda. Ini yang Membuat Hal ini dapat
bagi Anda untuk menjaga kode Anda di terpisah isinya cabang, masing-masing dengan konfigurasi pengaturan Sendiri.

## Menambahkan Pengaturan Konfigurasi

Untuk menambahkan pengaturan konfigurasi, hanya memanggil perintah config dengan add
pilihan, dan menambahkan key` Diinginkan `/` rekan value`.
~~~ Bash
$ APP_NAME ironapp / DEP_NAME CONFIG.ADD KEY = NILAI
~~~

Ini secara otomatis akan menambahkan Config add-on untuk penyebaran Anda.

Ganti APP_NAME, DEP_NAME, KUNCI dan NILAI dengan nilai-nilai yang diinginkan dan Mereka Will
ditambahkan ke penyebaran Anda cred.json antrian.

Untuk mengatur beberapa pengaturan sekaligus, hanya menambahkan lebih dari satu key` `/` pasangan value`.
~~~ Bash
$ APP_NAME ironapp / DEP_NAME CONFIG.ADD VALUE1 key2 key1 = = VALUE2 [...]
~~~

Parameter setup dapat diatur menggunakan layar ditampilkan di kolom pertama Dari tabel berikut. Mereka disimpan dalam format JSON Kemudian, seperti yang ditunjukkan pada kolom kedua. Argumen multiline dapat diatur menggunakan `\ n` melarikan diri karakter.

Parameter CLI | JSON representasi
--- | ---
key = value | {"key": "value"}
key = "multiline \ nvalue" | {"key" "multiline \\\\ nvalue"}
key = path_to_file.txt | {"kunci", "isi \ nof \ nfile \ n"}
kunci | {"key": true}

Catatan: Disarankan untuk menggunakan tanda kutip ganda `" `untuk pengaturan multispace emas
nilai multiline untuk membuat asam Mereka Apakah disimpan dengan benar.

## Pengaturan Daftar Konfigurasi

Anda bisa daftar pengaturan konfigurasi dari set yang ada dengan Meminjam konfigurasi
perintah:
~~~ Bash
Ironcliapp $ APP_NAME / config DEP_NAME
Key1 = VALUE1
Key2 = VALUE2
~~~

Untuk menunjukkan nilai kunci tertentu, hanya menambahkan nama kunci yang diinginkan:
~~~ Bash
Ironcliapp $ APP_NAME / config DEP_NAME KEY
NILAI
~~~

## Memperbarui Pengaturan Konfigurasi

Untuk menambah atau menghapus pengaturan kustom untuk konfigurasi Anda, cukup gunakan `add` emas
Opsi `Remove` dari perintah config dan menambahkan parameter yang Anda butuhkan.
~~~ Bash
$ APP_NAME ironapp / DEP_NAME CONFIG.ADD [-f | force] NEW_PARAM NEW_VALUE = [...]
$ APP_NAME ironapp / DEP_NAME config.remove param1 param2 [...]
~~~

Memperbarui pengaturan yang ada est mungkin menggunakan perintah `add`. Ini
akan memerlukan konfirmasi Anda KECUALI Anda menggunakan `` -f` emas Setelah bendera --force`
perintah add.

## Melepaskan Config Add-on

Menghapus semua pengaturan konfigurasi yang ada dari penyebaran bisa dilakukan oleh
Menghapus add-on.
~~~ Bash
$ APP_NAME ironapp / DEP_NAME addon.remove config.free
~~~

Ini akan menghapus semua konfigurasi pengaturan kustom.
