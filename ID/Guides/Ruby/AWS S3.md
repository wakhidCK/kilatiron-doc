# Integrasi dengan Amazon S3

[Amazon S3] (http://aws.amazon.com/s3/) adalah solusi Storage-as-a-Service. Layanan ini menyediakan antarmuka web sederhana yang dapat digunakan untuk menyimpan dan mengambil data dari mana saja di web.


## Amazon S3 SDK

Untuk Ruby Anda dapat memilih beberapa SDK yang berbeda untuk Amazon S3:
* [Amazon S3 Ruby SDK]
* [Fog]
* [AWS-S3]
* [RightAWS]
* [S3]


## Persiapan

Untuk menggunakan Amazon S3 SDK dalam proyek Anda, tambahkan gem pada [Gemfile].

~~~ruby
source 'https://rubygems.org'
gem 'aws-sdk'
~~~

Ikuti [panduan Amazon] (http://docs.aws.amazon.com/AWSSdkDocsJava/latest/DeveloperGuide/java-dg-setup.html) untuk membuat akun dan mendapatkan akses [kredensial AWS] (http://aws.amazon.com/security-credentials).

## Contoh penggunaan:

Cara yang disarankan untuk memberikan kredensial AWS untuk aplikasi Anda adalah melalui environment variable. Untuk melakukan hal ini, gunakan [Config Add-on] (https://community.CloudKilat.ch/tutorial/custom-config-add-on/):

~~~bash
$ ironapp APP_NAME/default config.add AWS_ACCESS_KEY_ID=[YOUR_SECRET_KEY] AWS_SECRET_ACCESS_KEY=[YOUR_ACCESS_KEY] AWS_REGION = 'eu-west1'
~~~

Sekarang mari kita mencoba beberapa operasi pada bucket dan file:

~~~ruby
require 'aws-sdk'
require 'securerandom'

s3 = AWS::S3.new
bucket = s3.buckets.create('testbucket' + SecureRandom.uuid)

# List buckets
s3.buckets.each do |bucket|
    puts bucket.name
end

# Put object
bucket.objects['key'].write(Pathname.new('tmp.txt'))

# Read object
puts bucket.objects['key'].read


# Delete object
bucket.objects['key'].delete

# Delete bucket
bucket.delete
~~~


[Amazon S3 Ruby SDK]: https://aws.amazon.com/sdkforruby/
[Fog]: https://github.com/fog/fog
[AWS-S3]: https://rubygems.org/gems/aws-s3
[RightAWS]: https://rubygems.org/gems/right_aws
[S3]: https://github.com/qoobaa/s3
[Gemfile]: http://bundler.io/v1.3/gemfile.html
