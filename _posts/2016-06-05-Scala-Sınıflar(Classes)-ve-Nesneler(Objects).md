---
layout: post
title: Scala Sınıflar(Classes) ve Nesneler(Objects)
date:   2016-06-05 11:07:19
excerpt: "Scala Sınıflar(Classes) ve Nesneler(Objects)"
tags: [Scala, Classes, Objects]
modified: 2016-06-05
comments: true
---

Scala fonksiyonel bir dil olmasına rağmen Object Oriented mantığınada izin verir. 
Yani scala da sınıflar oluşturabilir ve bu sınıflardan yeni nesneler türetebiliriz. 
Örneğin x/y şeklindeki bir rasyonel ifadeyi saklamak istediğimizi varsayalım. 
Bunu bir sınıf yardımı ile yapabiliriz.

<script src="http://gist.github.com/GlcEbru/b2dd32b27c49cf2e0b0b9a09908005e6.js"></script>

Scala tür isimlerini ve değerlerini farklı isim uzaylarında saklar. 
Bu sebepden aralarında çakışma olmaz.

Class veri türünden örneklenen değerler Obje olarak isimlendirilir. 
Scalada yeni bir obje üretmek için new ön ekini kullanırız.

{% highlight javascript %}
new Rational(1, 2)
{% endhighlight %}

Böylece Raional veri tipinde yeni bir eleman elde etmiş oluruz. İlk parametre Rational sınıfındaki 
numer = x değerine karşılık gelirken ikinci parametre ise denom = y değerine karşılık gelir.

Oluşturduğumuz nesneyi bir değişkene atayalım ve örneklediğimiz nesnenin verilerine bu değişken
aracılığı ile ulaşalım.

{% highlight javascript %}
val x = new Rational(1, 2)
x.numer     >>1
x.denom     >>2
{% endhighlight %}

x Rational tipinde bir değişken. Yani numer ve denom adlı fonksiyonları içeriyor. 
Nokta(.) ifadesi x' in içerisindeki fonksiyonlara ulaşmamızı sağlıyor. 

Oluşturduğumuz sınıf ile ilgili birkaç örnek yapalım. Örneğin iki rasyonel ifadeyi toplayalım.
Bunun için;

<script src="http://gist.github.com/GlcEbru/766a4bfbac8a590f0193d334378a4030.js"></script>

addRational adlı fonsiyonu yazabiliriz. Bu fonksiyon r ve s adında iki parametre alır.
Yanlız burada şöyle bir problemimiz var. Yazdığımız bu fonksiyon bize dönen değeri x/y 
şeklinde vermeyecektir. İfadeyi rasyonel sayı formatına getirmek için yeni bir fonksiyon tanımlayalım.

<script src="http://gist.github.com/GlcEbru/2c2aa4551d3bf4317ba7bf0837afc201.js"></script>

Ve ifadeyi test edelim.

{% highlight javascript %}
makeString(addRational(new Rational(1,2), new Rational(2,3)))
{% endhighlight %}

işlemin sonunda bize 7/6 cevabını verecektir.

Sınıfın içerisindeki fonksiyonlar method olarak isimlendirilir. 
Biraz önceki iki rasyonel sayıyı toplama işlemini bir sınıf içerisinde yapabiliriz. 
Bu programımıza daha derli toplu bir görüntü sağayacaktır. Hemde Scala mantığı 
ile hareket etmiş olacağız.

<script src="http://gist.github.com/GlcEbru/170ef611814f9fe2cb9a7bc48160a8db.js"></script>

Burada dikkatinizi birşey çekmiş olmalı. Bizim Rational sınıfımız iki değer içeriyor ancak 
add adlı methodda tek parametre var. Peki diğer parametremiz nerde? Diğer arametremiz programımızın
“object” kısmından gönderiliyor. Add fonksiyonunun sol kısmındaki ki “x” bizim gerçek rasyonel sayımız.

<script src="http://gist.github.com/GlcEbru/3276f97cff9740a907a712c8f1081339.js"></script>

Hemen bir örnek daha yapacak olursak. Sınıf içerisinde “neg” adlı bir fonksiyon tanımlayalım.
Bu fonksiyon bize verilen rasyonel sayının negatifini döndürsün.
Ve verdiğimiz x,y,z, değerleri ile x-y-z işlemini gerçekleştirsin.

<script src="http://gist.github.com/GlcEbru/a05f1b8544c2a4d4dff233cd26b7851b.js"></script>

Programı çalıştırdığımızda sonuç = -79/42 dir.

Yukarıdaki rasyonel sayıları toplamak için yazdığımız kodda eğer y.add(y) ifadesini ile işlem yaparsak bize “70/49” değerini dönderecektir. Ama bizim gerçek sonucumuz “10/7” olacaktır. Verilen değerler ile doğru sonuç elde etmek için ek bir fonksiyona ihtiyaç duyarız. Bu fonksiyonun amacı ortak bölenleri bulup sonucu en sade hale getirmektir.

<script src="http://gist.github.com/GlcEbru/b0c9069ce30ea342826fc89921ac23a5.js"></script>

Bir sınıfın veri veya fonksiyonlarınıdan oluşan üyelerine erişimi kısıtlayabiliriz. Eğer bir kısıtlama işlemi yapmak istiyorsak fonksiyonların ya da değişkenlerin başına “Private” ifadesini ekleriz.
Böylece istediğimiz verilerin istenilmeyen değerler almasını önlemiş oluruz. 

Gcd fonksiyonunu private tanımladık çünkü sadeleştirme işlemini yanlızca sınıf içerisinde yapmak istiyoruz. Böylece dışarıdan gönderilen değerler bu fonksiyondan etkilenmiyor.

Gcd fonksiyonunu bir değişkene atamak zorunda değildik. Yani işlemi şu şekilde de yapabilirdik.

{% highlight javascript %}
class Rational(x:Int, y:Int)
{

private def gcd(a: Int, b:Int): Int = if (b == 0) a else gcd(b, a%b)
	def numer = x / gcd(x,y)
	def denom = y / gcd(x,y)
...
}
{% endhighlight %}

ancak bu tanımalada her işlem yaptığımızda gcd fonksiyonu tekrar çalışacak ve aynı hesaplamaları tekrar yapacak. Bu da işlemciyi gereksiz yere yormak demek bu sebeple bir değişkene gcd den dönen değeri atadık ve istenildiği zaman kullandık.

Sınıf çerisindeki fonksiyonlar ile ilgili daha fazla örnek yapabiliriz. Mesela iki rasyonel sayının hangisinin maksimum ya da hangisinin minimum olduğunu bulabiliriz.

<script src="http://gist.github.com/GlcEbru/188b3e5b37d049c31aa460dab8f7ec02.js"></script>


{% highlight javascript %}
x.less(y) 
{% endhighlight %}

şeklinde main içerisinden çağırısak bize “true” değerine verecektir.

Burada maksimum değerini bulmak için less fonksiyonunu kullanmak istiyoruz. Bunu yapmamızın yolu çoğu Nesne Yönelimli Programlama dillerinde olduğu gibi “this” ya da “self” önekini kullanmak. This bizim şu anki rasyonel sayımızı temsil eder. 
<script src="http://gist.github.com/GlcEbru/5b2bdcefc757519362e4561e03e289cf.js"></script>

less fonksiyonunu “this” kullanarak yazarsak;

{% highlight javascript %}
def less(that: Rational) = 
	this.numer * that.denom < that.numer * this.denom
{% endhighlight %}

Dipçe: Diğer programa dillerinde tanımlamalar alfanümerik karakterler ile başlamalı. Ancak Scala da alternatif olarak semboller ile de tanımlanabilir. İki tanımlamanın da arasında sonuç bazında hiçbir fark yoktur.

<script src="http://gist.github.com/GlcEbru/a2ae6adfb1c5f5fe9ecd9c778ce2e0d9.js"></script>


