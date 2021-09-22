---
layout: post
title: Scala Higher Order ve Anonymous Fonksiyonlar, Currying
date:   2016-06-02 15:07:19
excerpt: "Scala Higher Order ve Anonymous Fonksiyonlar, Currying"
tags: [Scala, Higher Order Functions, Anonymous Functions, Currying]
modified: 2016-06-02
comments: true
---

### Higer-Order Functions:
Scala Higer-Order Fonksiyon tanımlamaya izin veir. Fonksiyonları diğer fonksiyonlara parametre olarak gönderebiliriz ya da işlemin sonunda elde ettiğimiz sonuç bir fonksiondur.

Örneğin; iki tam sayı arasındaki sayıların kübünü bulmak istiyoruz.

<script src="http://gist.github.com/GlcEbru/b5cb8dd12580c3fd865faffe9a3483c2.js"></script>

Burada sumCubes'ün içerisinde cube fonksiyonunu çağırmak yerine fonksiyonu direk parametre olarak gönderebiliriz. İfadeyi şu şekilde tanımlarsak;

<script src="http://gist.github.com/GlcEbru/2af8d8eae6bb70039e1fa9396e57e30c.js"></script>

Bu tanımladığımız ifadeyi küp ve faktoriyel bulma ile örneklendirebiliriz.

<script src="http://gist.github.com/GlcEbru/1f41ddf1b949e891156a74554c7fb298.js"></script>

Burada cube ve fact;

<script src="http://gist.github.com/GlcEbru/c4efe1a52f8aabb8033df56eebb8afb8.js"></script>

ifadeleri ile eşleşir.

Fonksiyon Tipi: a => b ifadesi bize “a” giriş değerini “b” ise sonuç değerini belirtir.

Bu ifadelerde dikkatinizi birşey çekmiş olmalı. Neden birden çok yardımcı fonksiyon tanımladık. Bu tanımlamaları yapmadanda işlemimizi gerçekleştirebilir miyiz? İşte bu kısımda devreye Anonymous Fonksiyonlar giriyor.

### Anonymous Functions:

Fonksiyonları bir diğer fonksiyona gönderirken, parametre olarak geçen birçok küçük fonksiyonun oluşmasına sebep olur.
Bu fonksiyonları def kullanarak tanımlamak bazen program yazan kişiye angarya yük olabilir.  
Örneğin ekrana “abc” ifadesini yazmak istiyoruz. Bunu iki şekilde yapabiliriz. Birincisi def kullanarak;

{% highlight javascript %}
def str = “abc”;
println(str)
{% endhighlight %}

İkinci yol ise direk ifadeyi yazarak.

{% highlight javascript %}
println(“abc”)
{% endhighlight %}

Bu örnekde karakter dizileri değişmez literallerdir. Benzer olarak fonksiyonlarda literaller gibidir isimsiz fonksiyon yazmamıza izin verir. Bu fonksiyonlara Anonymous Functions adı verilir. 

Anonymous Functions Syntax:
{% highlight javascript %}
(x: Int) => x*x*x
{% endhighlight %}

Burada (x: Int)  ifademizin parametresini , x*x*x ise esas kısmını oluşturuyor.

Yukarıdaki küp örneğini anonymous fonksiyon şeklinde tekrardan düzenlersek.

<script src="http://gist.github.com/GlcEbru/b40013ea5c1000c84a7da22d7e087d07.js"></script>

Peki biz bu ifadeyi daha sade bir hale getirebilirmiyiz. Tabiki evet.

### Currying:

Amaci bir fonksiyona luzum dahilinde ihtiyaci oldugundan daha az parametre gonderebilmektir.

<script src="http://gist.github.com/GlcEbru/0bef36bb09af3729fafd6713171c28f8.js"></script>

Çoklu Parametre Listesinin Açılımı:

Kullandığımız fonksiyonlar birden çok parametre içeriyorsa bunu currying mantığı kullanarak daha sade hale getirebiliriz.
{% highlight javascript %}
def f(args1)....(argsn) = E 
{% endhighlight %}
Genel olarak fonksiyonara argüman gönderme işlemi bu şekilde yapılır. Eğer n değeri 1 den büyük ise yeni bir fonksiyon tanımlamalıyız;
{% highlight javascript %}
def f(args1)...(argsn-1) = {def g(argsn) = E;g}
{% endhighlight %}
Daha sade bir şekilde ifadeyi Anonymous Function şekline getirisek;
{% highlight javascript %}
def f(args1)....(argsn-1) = (argsn) => E
{% endhighlight %}
İşlemimiz n kez devam ediyor ise;
{% highlight javascript %}
def(args1)...(argsn-1)(agsn) = E
{% endhighlight %}
Bütün parametreleri nested ve anonymous fonksiyon şeklinde yazarız.
{% highlight javascript %}
def f = (args1 => (args2 => ...(argsn => E)...))
{% endhighlight %}
Bu tanımlama şekli Currying olarak isimlendirilir.

Hemen ufak bir örnek yapalım. 
Bir fonksiyon tanımlayalım. Bu fonksiyon a ve b arasındaki sayıların karelerini çarpsın.

<script src="http://gist.github.com/GlcEbru/9163ffa5da1f251d91cee4db2c369c1a.js"></script>

Şimdide fact adlı bir fonksiyon tanımlayalım. Bu fonksiyon sayının faktöriyelini alsın ancak product fonksiyonunu kullanarak bunu gerçekleştirsin.

<script src="http://gist.github.com/GlcEbru/33fce929181690f5d16ad950815d58b6.js"></script>
