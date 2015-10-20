Integrasi # Python Amazon S3

[Amazon S3] (http://aws.amazon.com/s3/) adalah Storage-as-a-Service solusi. Ini menyediakan antarmuka layanan web sederhana yang dapat digunakan untuk menyimpan dan mengambil data dari mana saja di web.

## Amazon S3 SDK

Untuk Python Anda dapat memilih antara SDK yang berbeda untuk Amazon S3:
* [Amazon S3 Python SDK / boto] (http://aws.amazon.com/sdkforpython/)
* [Python-s3] (https://github.com/nephics/python-s3)
* [Py-mini-s3] (http://code.google.com/p/pts-mini-gpl/source/browse/#svn/trunk/py-mini-s3)

## Persiapan

Ikuti [Amazon Panduan] (http://docs.aws.amazon.com/AmazonS3/latest/gsg/GetStartedWithS3.html) untuk membuat akun dan mendapatkan Anda [AWS akses kredensial] (http: //aws.amazon. com / keamanan kredensial).

Untuk menggunakan `penyimpanan AWS S3` dalam aplikasi Anda hanya menentukan ketergantungan tambahan dalam` requirements.txt` Anda:

~~~
boto == 2.9.8
~~~

## Contoh penggunaan:

Cara yang disarankan untuk memberikan mandat AWS untuk aplikasi Anda adalah melalui variabel lingkungan. Untuk melakukan hal ini, gunakan [Config Add-on] (/ Add-on% 20Documentation / Deployment / Kustom% 20Config.md):

~~~ Pesta
$ Ironcliapp APP_NAME / default config.add AWS_SECRET_KEY = [YOUR_SECRET_KEY] AWS_ACCESS_KEY = [YOUR_ACCESS_KEY]
~~~

Sekarang mari kita menunjukkan beberapa operasi pada ember dan benda-benda:

~~~ Python

impor uuid
impor boto
dari boto.s3.key impor Key

jika __name__ == "__main__":

    # Koneksi klien S3 - AWS mandat secara default dibaca
    # Dari env varaiables: AWS_ACCESS_KEY dan AWS_SECRET_KEY
    conn = boto.connect_s3 ()

    # Buat ember
    ember = conn.create_bucket ('testbucket {0}'. Format (str (uuid.uuid4 ())))

    # Daftar ember
    ember = conn.get_all_buckets ()
    print 'Ember:', ember

    # Objek Masukan
    k = Key (bucket)
    k.key = 'kunci'
    nama_file = 'testfile {0}'. Format (str (uuid.uuid4 ()))
    file = open (nama_file, 'w')
    file.write ('Ini adalah file tes yang akan mendarat di S3')
    file.close ()
    k.set_contents_from_filename (nama_file)

    # Baca objek
    key = bucket.get_key ('key')

    print key.get_contents_as_string ()

    # Objek Delete
    bucket.delete_key ('key')

    # Hapus ember
    conn.delete_bucket (bucket.name)
~~~
