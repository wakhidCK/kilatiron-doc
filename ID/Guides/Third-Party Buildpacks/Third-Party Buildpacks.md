# Custom Buildpacks

[KilatIron] resmi mendukung jenis aplikasi berikut melalui [Pinky Stack] [PinkyStack].

- Berbasis Java (Java dengan Maven, Gradle, Grails, Scala, Play atau Clojure!)
- Ruby
- PHP
- Python
- NodeJS
 
Namun, Anda dapat menggunakan aplikasi yang dikembangkan dengan bahasa dan teknologi yang lain menggunakan custom buildpack.

## Verified Buildpacks

Berikut adalah daftar diverifikasi dan direkomendasikan buildpacks untuk platform KilatIron meliputi bahasa dan teknologi berikut:

| Teknologi | Buildpack URL |
|: --------- |: ----------: |
| Go | [https://github.com/kr/heroku-buildpack-go] [buildpack-go] |
| Erlang | [https://github.com/cloudControl/buildpack-erlang-kernel] [buildpack-erlang] |
| Common Lisp | [https://github.com/mtravers/heroku-buildpack-cl] [buildpack-common-lisp] |
| Lua | [https://github.com/leafo/heroku-buildpack-lua] [buildpack-lua] |
| C | [https://github.com/atris/heroku-buildpack-C] [buildpack-c] |
| Haskell | [https://github.com/pufuwozu/heroku-buildpack-haskell] [buildpack-Haskell] |
| Perl | [https://github.com/fern4lvarez/buildpack-perl] [buildpack-perl] |

## Menggunakan Custom Buildpack

Dalam rangka untuk menciptakan sebuah aplikasi menggunakan custom buildpack Anda harus memilih `custom` jenis aplikasi dan kemudian memberikan URL buildpack yang diinginkan:

~~~ Pesta
$ ironapp APP_NAME create BUILDPACK_URL custom --buildpack
~~~

** Catatan: ** `BUILDPACK_URL` harus menjadi git repositori non-ssh.

Anda dapat menggunakan salah satu buildpacks tersebut, fork buildpack tersebutdan membuat perubahan sesuai dengan kebutuhan Anda atau bahkan membuat sendiri. Perlu diingat bahwa custom buildpack harus mengikuti heroku [Buildpack API] [buildpack-API].

Sebelum menggunakan pihak ketiga buildpack Anda harus memeriksa kode sumber mereka dan melanjutkan dengan hati-hati.

[KilatIron]: http://www.cloudkilat.com/
[PinkyStack]: /Platform%20Documentation.md/#stacks
[Buildpack-java]: https://github.com/cloudControl/buildpack-java
[Buildpack-python]: https://github.com/cloudControl/buildpack-python
[Buildpack-ruby]: https://github.com/cloudControl/buildpack-ruby
[Buildpack-php]: https://github.com/cloudControl/buildpack-php
[Buildpack-nodejs]: https://github.com/heroku/heroku-buildpack-nodejs
[Buildpack-clojure]: https://github.com/heroku/heroku-buildpack-clojure
[Buildpack-gradle]: https://github.com/heroku/heroku-buildpack-gradle
[Buildpack-Grails]: https://github.com/heroku/heroku-buildpack-grails
[Buildpack-scala]: https://github.com/heroku/heroku-buildpack-scala
[Buildpack-play]: https://github.com/heroku/heroku-buildpack-play
[Buildpack-erlang]: https://github.com/cloudControl/buildpack-erlang-kernel
[Buildpack-go]: https://github.com/kr/heroku-buildpack-go
[Buildpack-common-lisp]: https://github.com/mtravers/heroku-buildpack-cl
[Buildpack-lua]: https://github.com/leafo/heroku-buildpack-lua
[Buildpack-c]: https://github.com/atris/heroku-buildpack-C
[Buildpack-Haskell]: https://github.com/pufuwozu/heroku-buildpack-haskell
[Buildpack-perl]: https://github.com/fern4lvarez/buildpack-perl
[Buildpack-API]: https://devcenter.heroku.com/articles/buildpack-api
