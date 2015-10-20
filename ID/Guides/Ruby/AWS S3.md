# Ruby Amazon S3 integrasi

[Amazon S3] (http://aws.amazon.com/s3/) adalah Storage-as-a-Service solusi. Ini menyediakan antarmuka layanan web sederhana yang dapat digunakan untuk menyimpan dan mengambil data dari mana saja di web.


## Amazon S3 SDK

Untuk Ruby Anda dapat memilih antara SDK yang berbeda untuk Amazon S3:
* [Amazon S3 Ruby SDK]
* [Kabut]
* [AWS-S3]
* [RightAWS]
* [S3]


## Persiapan

Untuk menggunakan resmi Amazon S3 SDK dalam proyek Anda, tambahkan permata untuk Anda [Gemfile].

~~~ Ruby
source 'https://rubygems.org'
permata 'aws-sdk'
~~~

Ikuti [Amazon Panduan] (http://docs.aws.amazon.com/AWSSdkDocsJava/latest/DeveloperGuide/java-dg-setup.html) untuk membuat akun dan mendapatkan Anda [AWS akses kredensial] (http: // aws.amazon.com/security-credentials).

## Contoh penggunaan:

Cara yang disarankan untuk memberikan mandat AWS untuk aplikasi Anda adalah melalui variabel lingkungan. Untuk melakukan hal ini, gunakan [Config Add-on] (https://community.CloudKilat.ch/tutorial/custom-config-add-on/):

~~~ Pesta
$ Ironcliapp APP_NAME / default config.add AWS_ACCESS_KEY_ID = [YOUR_SECRET_KEY] AWS_SECRET_ACCESS_KEY = [YOUR_ACCESS_KEY] AWS_REGION = 'eu-barat 1'
~~~

Sekarang mari kita menunjukkan beberapa operasi pada ember dan benda-benda:

~~~ Ruby
membutuhkan 'aws-sdk'
membutuhkan 'SecureRandom'

s3 = AWS :: S3.new
ember = s3.buckets.create ('testbucket' + SecureRandom.uuid)

# Daftar ember
s3.buckets.each melakukan | ember |
    menempatkan bucket.name
akhir

# Objek Masukan
bucket.objects ['key']. menulis (Pathname.new ('tmp.txt'))

# Baca objek
puts bucket.objects ['key']. baca


# Objek Delete
bucket.objects ['key']. delete

# Hapus ember
bucket.delete
~~~


[Amazon S3 Ruby SDK]: https://aws.amazon.com/sdkforruby/
[Kabut]: https://github.com/fog/fog
[AWS-S3]: https://rubygems.org/gems/aws-s3
[RightAWS]: https://rubygems.org/gems/right_aws
[S3]: https://github.com/qoobaa/s3
[Gemfile]: http://bundler.io/v1.3/gemfile.html
