---
layout: post
title: "React.createClass ve extends React.Component Arasındaki Farklar"
excerpt: React da bir componet’ı oluşturmanın birden farklı yolu vardır. Temelde React.createClass ve extends React.Component aynı
tags: [React, React.createClass, extends React.Component]
comments: true
---

React da bir componet’ı oluşturmanın birden farklı yolu vardır. Temelde React.createClass ve extends React.Component aynı işlemi farklı şekillerde yaparlar.  


React da geleneksel olarak bir component oluşturmak için React.createClass methodu kullanılır. ES6 sınıf yapısı ile de component oluşturulabilir.


Bir component oluşturmak için uygulanan bu iki yöntemde bazı farklılıklar mevcuttur. Bunlardan bazıları;

### Syntax Farklılıkları:

#### React.createClass

Burada const keyword’ü ile bir React sınıfı oluşturulmuş. Tipik base component tanımlamalarında render fonksiyonu önemli bir role sahiptir.

İlk olarak aşağıdaki iki kod örneği arasındaki syntax farklılıklarını inceleyebilirsiniz.

<script src="http://gist.github.com/GlcEbru/dbf756c419567b67161e6390debbd8b4.js"></script>

#### React.Component

Yukarıdaki React.createClass tanımlamasını ele alıp, onu ES6 sınıfını kullanarak yeniden düzenleyebiliriz.

<script src="http://gist.github.com/GlcEbru/e775ecc855df55720a08e220509145c0.js"></script>

JavaScript açısından bakıldığında şuanda ES6 sınıflarını kullanıyoruz. Henüz bütün tarayıcıların ES6’yı desteklememeleri sebebiyle yazdığımız kodların ES6 dan ES5’ e babel gibi bir compiler aracılığıyla dönüştürülmesi gerekiyor.

### State Farklılıkları:

#### React.createClass

React da props bileşenlerindeki değişimleri izlemek için state adında bir nesne bulunmaktadır. Bu state'in ilk değerini vermek için getInitialState adında bir metod yazmamız gerekli.

<script src="http://gist.github.com/GlcEbru/ba825b6c6fd674dc09dbbcb9d7d33a62.js"></script>

#### React.Component

React.Component'ın constructor özelliğini kullanarak kolay bir şekilde bütün stateleri başlatabiliriz.

<script src="http://gist.github.com/GlcEbru/2f97982528bb0a00fc6c28155c5dd7d5.js"></script>

### “this” Farklılıkları:

#### React.createClass

React.createClass otomatikmen this keywordüne kendi instance'ını bağlar. ES6 class yapısı ile yapılırsa bu işlemi manuel yapmak gereklidir.

<script src="http://gist.github.com/GlcEbru/589891e3dd0fd40458826809d1fad917.js"></script>

#### React.Component

Buradaki this null dur çünkü component’ın instance’ı bağlanmamıştır.

<script src="http://gist.github.com/GlcEbru/24052d69c9424a803f2225f5e7303c93.js"></script>

Doğru içeriği bağlamak için birkaç farklı yöntem kullanabiliriz. Aşağıda inline olarak nasıl bağlayabileceğimizi görebilirsiniz.

<script src="http://gist.github.com/GlcEbru/31e3cee7734b42d9f84fea57a5fdd1fe.js"></script>

Alternatif olarak constructor’ın içerrisindeki this.handleClick bağlamını değiştirebiliriz.

<script src="http://gist.github.com/GlcEbru/53f606ea225277c94f193c6a60b211d6.js"></script>

### propTypes ve getDefaultProps:

Component’a property bağlamak için kullanılır. Default değerler ile gönderilmek istendiğinde getDefaultProps()  kullanılır.

#### React.createClass

React.createClass bir fonksiyondur. Çeşitli parametreler alır. Bu parametrelere fonksiyonun prop type'ını, mixins'ini verebiliriz. Ancak render’ı kesinlikle vermemiz gerekli. Diğer parametreleri istersek verebiliriz. Sonunda bu parametrelere bağlı olarak component classında bir instance döndürür.

<script src="http://gist.github.com/GlcEbru/6df95b3e01022778d6264bf38ed3187d.js"></script>

#### React.Component

Eğer ES6 ile component oluşturma işlemini gerçekleştirirsek parametre vermek yerine sınıfa sınıfınismi.propertismi şeklinde property ekleyebiliriz.

<script src="http://gist.github.com/GlcEbru/2d918d35de9d13232b54524c6a662107.js"></script>

### Mixins:

Componentlara dışarıdan fonksiyon, property gibi özellikleri bağlamak için kullanılır.

#### React.createClass

<script src="http://gist.github.com/GlcEbru/65cf3d740c31f261826b3c23f60ff7c9.js"></script>

#### React.Component

ES6 sınıfları mixins’i desteklememektedir.

[Kaynak](http://toddmotto.com/react-create-class-versus-component/#reactcomponent)


Yardım ve desteklerinden dolayı [Mesut Yiğit' e](http://mesutyigit.weebly.com) çok teşekkürler.
