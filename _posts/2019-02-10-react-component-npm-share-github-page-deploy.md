---
layout: post
title: React İle Component Oluşturup npm de Paylaşma ve github-pages de Deploy Etme
date:   2019-02-10 19:07:19
modified:  2019-02-10
comments: true
img: react-component.png
---

Yaptığınız React component’ı community ile paylaşarak başka insanların da faydalanmasını sağlayabilirsiniz. Bu yazıda kendi yaptığım component üzerinde bunu nasıl yapacağımızdan bahsedeceğim.

Yazıyı 3 kısma ayırdım. Bunlar şöyle;

1. Webpack kullanarak localde çalışan bir component oluşturma
2. Component’ı npm de paylaşma
3. github-pages’e deploy etme

**1) Component Oluşturma**

Öncelikle bir folder oluşturmalı ve npm’i başlatmalıyız. Burada dikkat edilmesi gereken en önemli nokta paket adınızın npm’deki diğer paket adlarından farklı olması. Benim kullandığım folder adı ‘react-rate-component’

<script src="https://gist.github.com/ebrugulec/2589ceaf394309e6edcbc592eaef2470.js"></script>

npm init dediğinizde varsayılan soruları kabul edip ilerleyebilirsiniz. Ya da

**npm init -y**

diyerek bütün soruları geçebilirsiniz.

Component’ı oluşturabilmemiz için başta react ve react-dom olmak üzere çeşitli paketlerimize ihtiyacımız var. Bu paketleri hepsini birden indiriyoruz.

<script src="https://gist.github.com/ebrugulec/dd010422f936bd83c1da90f31d71f3e9.js"></script>

İhtiyacımız olan paketleri indirmemiz için bir başka yöntem de benim package.json dosyamı koplayıp düzenledikten sonra npm install demeniz.

<script src="https://gist.github.com/ebrugulec/8bc064c2f3807f45b7f61561821948c3.js"></script>

Projeyi webpack ile başlatabilmemiz için package.json’ a start komutunu eklemeliyiz.

```javacript
"start": "webpack-dev-server --mode development"
```

Şimdi root folder’ımızın içine src ve examples/src adında folder’ları oluşturuyoruz.

```javascript
src\   <- component'ımızın kaynak kodları ve style dosyalarımız
examples\src\  <- oluşturacağımız demo için
```

src folder’ın içine index.js adında bir dosya daha oluşturup component’ın kaynak kodlarını içine yerleştiriyoruz. Varsa eğer style’larımız için de bir dosya oluşturup onları da index.js’e import ediyoruz.

<script src="https://gist.github.com/ebrugulec/8c4e4c738e5d73db09e031e4052db462.js"></script>

Component’ın iskeletini oluşturduğumuza göre şimdi de demo için bir iskelet oluşturalım.

<script src="https://gist.github.com/ebrugulec/c3aa721310390a706c96cc1d80d5feb5.js"></script>

Demo için component’ımızı ../../src ‘ den import etmeliyiz.

<script src="https://gist.github.com/ebrugulec/987ff9e21cec813f1ace0034b5685c32.js"></script>

Şimdi ise component’ın bel kemiğini oluşturmaya geldi. Yani webpack ayarlarımızı yapmamız gerekli öncelikle. Bunun için root folder’a webpack.config.js adında bir dosya oluşturun. Ve aşağıdaki kodları bu dosyanın içine kopyalayın.

<script src="https://gist.github.com/ebrugulec/d231b11703eef79cf85e2023117f77d9.js"></script>

Daha fazla browser’da kodumuzu çalıştırabilmek için babel ayarlarımızı yapmamız gerekli. Babel component’ımızı es5'e transpile ediyor. Babel ayarlarını yapmak için root folder da .babelrc adında bir dosya oluşturuyoruz.

<script src="https://gist.github.com/ebrugulec/c1bbb7ad114e6682d3a406dac8ed530e.js"></script>

Şimdi oluşturduğumuz component’ın demosunu

**npm start**

diyarek localhost:3001 adresinde görebiliriz. Yaptığınız değişikliklerin otomatik olarak güncelleneceğini unutmayın lütfen.

**2) Component’ı npm de Paylaşma**

Component’ımızı oluşturduktan sonra sıra geldi npm’de paylaşma kısmına. Aslında burası en kolay kısmı yanlızca birkaç ayar yaptıktan sonra kolayca paylaşabiliriz.

İlk adım için babel -cli paketimizi indiriyoruz.

<script src="https://gist.github.com/ebrugulec/9bc8ed031f647a8a7aefef65d36409bd.js"></script>

Ve dosyalarımızı transpile etmek için package.json dosyamıza bu komutu ekliyoruz. Ardından root folder’da dist adında bir dosya yaratıp main endpointi değiştiriyoruz.

<script src="https://gist.github.com/ebrugulec/6647a95a9651ea9d4af31fb671f477a9.js"></script>

**npm run transpile**

diyerek dosyalarımızın transpile edilmiş halini dist/index.js dosyamızda görebiliriz.

Her seferinde bu transpile işlemini yapmamak ve değişiklikleri en güncel halde olması için script dosyamıza prepublishOnly komutumuzu ekliyoruz package.json dosyamızın içine.

<script src="https://gist.github.com/ebrugulec/dfc6934e3a4b5c5f00386721fb0874f7.js"></script>

Sondan bir önceki adım olarak bir .npmignore dosyası oluşturacağız root klsörümüzün içine. Çünkü gereksiz dosyaların component’ı kullanan insanlara ulaşmasını ve component file’ını şişirmek istemeyiz.

<script src="https://gist.github.com/ebrugulec/6617514698cd6ba732039315aa962b0b.js"></script>

Şimdi component’ımızı paylaşabiliriz.

```javascript
npm publish
```

Burada dikkat edilmesi gereken bir diğer husus da npm ‘ecommand line’dan giriş yapmış olmalısınız. Onun için de;

```javascript
npm login
```

Şimdi npm hesabınızdan paketinizi kontrol edebilirsiniz. Benimki [şurada](https://www.npmjs.com/package/react-rate-component).

**3)github-pages’a Deploy Etme**

github-pages kullanarak projelerimizi deploy etmek için webpack olmazsa olmaz. Github projeleri github-pages branch’i üzerinden yapıyor. En hızlı yoldan deploy almak için şu paketi projemize indiriyoruz.

**npm i gh-pages -D**

Ve pakage.json’a şu komutları yazıyoruz.

<script src="https://gist.github.com/ebrugulec/cf7dda00e569aeaa547fd40b627eef11.js"></script>

build komutu dosyalarımızı production için minified etmeye yarıyor. Ancak webpack’de nereye build edeceğini belirtmemiz gerekli. Bunun için şu komutu webpack.config.js dosyamıza ekliyoruz.

<script src="https://gist.github.com/ebrugulec/94f0f114f548d7fa21c8ba9154276385.js"></script>

**npm run build**

ile production için build edilmiş dosyalarımızı **examples/dist** altında de bulabiliriz.

Şimdi bir .gitignore dosyası oluşturacağız. Yine gereksiz dosyalarımızı göndermemek için.

<script src="https://gist.github.com/ebrugulec/b6101ac28b092953a04babeced56a77a.js"></script>

Artık bir github repository oluşturup local dosyalarımızı bu repositorye gönderebiliriz.

Son adım olarak artık deploy alabiliriz. \o/

**npm run deploy**

Şimdi github’da projenizin setting ayarlarından github-pages ayarına bakarsanız component’ın oraya deploy edilmiş halini bulabilirsiniz. Benimki de [şurada](http://gulecebru.com/react-rate-component/).

Component da herhangi bir değişiklikte paylaşma ve deploy yapma işini tek bir komutta toplamak için;

**npm run publish-demo**

⚠️ Bu ve diğer yazılarımı [medium](https://medium.com/@ebrugulec/) üzerinden de okuyabilirsiniz.

⚠️ Component için PropTypes kullanmayı ve diğer developerların projenizi rahatlıkla anlayabilmesi için README yazmayı unutmayın.

⚠️ Yazıda herhangi bir hata bulursanız ya da eksik olduğunu düşündüğünüz bir yer olursa lütfen bana bildirin. Bu hem benim yanlış öğrendiğim bir şeyi düzeltir hem de bu yazıyı okuyanları yanlış bilgilendirmiş olmam. Şimdiden teşekkürler.

Sevgiler.



