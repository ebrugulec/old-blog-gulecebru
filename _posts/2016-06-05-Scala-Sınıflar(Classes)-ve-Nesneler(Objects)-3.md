---
layout: post
title: Scala Sınıflar(Classes) ve Nesneler(Objects)-3
date:   2016-06-06 11:07:19
excerpt: "Scala Sınıflar(Classes) ve Nesneler(Objects)-3"
tags: [Scala, Classes, Objects]
modified: 2016-06-06
comments: true
---

--Sınıfları ve objeleri karışıklık olmadan kullanmamızın yolu onları organize etmek yani onları bir “package” içerisinde saklamaktır. Yazdığımız kodların en başına hangi paketi kullanacağımızı belirtiriz. 

package progfun.examples 
object Hello { ... }

Burada Hello objesini ima ettiğimizde aslında nitelendirilmiş adı olan progfun.examples.Hello ifadesinden bahsediyoruz. Örneğin Hello programını command line ile çalıştırdığımızda çalıştırdığımızda;
> scala  progfun.examples.Hello
>>Bir “package” içerisinde bir başkasını kullanabiliriz. Örneğin elimizde stcratch adlı bir objemiz var. Ve biz bu objemizin içerisinde b,r önceki yazımda bahsettiğim rasyonel sayılar objesini kullanmak istiyoruz. Bunu kullanabilmek için Rational sınıfının hangi “package içerisinde olduğunu bilmeliyiz ve kullanmak istediğimiz zaman “week3.Rational” şeklinde belirtmeliyiz.

<script src="http://gist.github.com/GlcEbru/0fa233b4ca7665e6fe396c0d6d4dfcf7.js"></script>

Burada şöyle bir can sıkıcı durum var. Biz her “Rational” sınıfını kullanmak istediğimizde sınıfımızın önüne “week3” ifadesini getirmek zorundayız. Bu tekrar eden durumu önlemek için “Rational” sınıfının bulunduğun package' ı import ederiz.

<script src="http://gist.github.com/GlcEbru/d5e79383274c7d446b009d1c4d1401cd.js"></script>

Paketleri import yani kullandığımız programa dahil etmenin birkaç yolu vardır. Yukarıdaki “Import week3.Rational” ifadesi sadece “Rational” sınıfını, kullanılan programa dahil etmiştir. Bütün “week3” paketini  import etmek istersek “_” ifadesini kulanırız.

<script src="http://gist.github.com/GlcEbru/21ffb3379c06dfa45962aee4b6d4ce28.js"></script>

Herhangi bir Scala programında otomatik olarak import edilen bazı elemanlar vaardır. Bunlar;
▶Scala package'ın büyün üyeleri
▶ java.lang package'ın büyün üyeleri
▶ scala.Predef. Objesinin tekil bütün üyeleri

Bazı tip ve fonksiyonların tam olarak nitelenmiş isimleri:
Int              scala.Int 
Boolean      scala.Boolean 
Object            java.lang.Object 
require         scala.Predef.require 
assert           scala.Predef.assert

http://www.scala-lang.org/files/archive/api/current/

adresini kullanarak standart Scala kütüphaneleriini öğrenebilirsiniz.

## Traits
Trait içinde sadece kendisinden türeyen sınıfların içini doldurmak zorunda olduğu içi boş metod tanımlarının yapıldığı bir yapıdır. Kısacası kendisini kullanacak sınıflar için yerine getirilmesi gereken metodları belirtir.
-Scala Javada olduğu gibi yanlızca bir “superclass” bulundurabilir. 
-Abstract class gibidir ancak trait tanımlarken, tanımlamamızın önüne “trait” anahtar kelimesini getiririz.

<script src="http://gist.github.com/GlcEbru/590a7f55d6309c53eb233da71eef2744.js"></script>

-Traitlerin içindeki metodlar sadece metod imzasından ibaret olabilirler. ayrıca trait constructorları parametre almazlar. 
-Traitlerin içindeki metodlar metod implementasyonunu da içerebilirler. bu yönüyle abstract classlara benzer.
-Sınıflar, nesneler ve traitler en çok bir sınıftan miras alabilir fakat isteğe bağlı olarak birden fazla trait alabilir.

class Square extends Shape with Planar with Movable ...

-Traitler Javadaki interfacelere benzerler fakat daha güçlülerdir çünkü fields ve concrete metodlarını içerirler. İnterfaceler yanlızla abstract metodlarını içerir. 
-Kısaca özetlemiş olursak traitler geliştirdiğimiz yazılımda aynı kavramın birden farklı şekilde uygulandığında bu kavramı soyutlayarak kodun esnekliğini,okunulabilirliğini arttırmak ve değişimin etkisini en aza indirmek için kullanılan yapılardır.
