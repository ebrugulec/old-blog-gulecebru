---
layout: post
title: Rails Polymorphic Associations
date:   2017-11-13 20:00:19
modified: 2017-10-13
excerpt: Polimorfizm kelime anlamı olarak "birçok şekil" demektir.
tags: [rails, polymorphic associations]
comments: true
---

Polimorfizm kelime anlamı olarak "birçok şekil" demektir. Programlama dillerinde ise genel anlamı itibari ile bir yapının başka yapılar tarafında kullanılabilmesini sağlayan özelliktir. Railsda da durum aynıdır. Rails da polymorphic ilişkiler sayesinde tek bir ilişki ile bir model birden fazla başka modele ait olabilir.
Örneğin bir picture modeli oluşturalım ve bu model hem employee hem de product modeline ait olsun.

{% highlight ruby %}
rails g model employee name:string
{% endhighlight %}

{% highlight ruby %}
rails g model product title:string
{% endhighlight %}

{% highlight ruby %}
rails g model picture title:string imageable_id:integer imageable_type:string
{% endhighlight %}

<script src="http://gist.github.com/ebrugulec/5da044ca8efd3797196fff35c6b540db.js"></script>
<script src="http://gist.github.com/ebrugulec/5277a238f19f8c02ed49f3be852ea63a.js"></script>
<script src="http://gist.github.com/ebrugulec/c4a4bc5a5b5ca12707b9794893b3d412.js"></script>

Ardından oluşturduğumuz modellere ilişkileri tanımlıyoruz.

<script src="http://gist.github.com/ebrugulec/935b43d02a28d446cfb323209611df2d.js"></script>
<script src="http://gist.github.com/ebrugulec/9472e33ab6a00ec1171ff70668e2bd97.js"></script>
<script src="http://gist.github.com/ebrugulec/1e5d7cf2f4b134171a411efb8bb69f9b.js"></script>

employee ve product modellerinin birden fazla picture modelini kullanabilir. Bu sebeple has_many ilişkisi oluşturduk. picture modeli ise imageable'ı referans alarak hem employee hem de product modeline ait olabilir. Burada t.references :imageable, polymorphic: true ifadesi ile polymorphic türünü ve id'sini tutabiliyoruz. Polymorphic ilişki tanımlamamız bu kadardı şimdi de bunu test edelim.

Öncelikle rails console'a gidip bir employee oluşturmayı denemelim.

Ardından da employee'ye ait bir picture oluşturalım.
![poli image 1]({{ site.url }}/assets/img/poli1.png)

Şimdi de oluşturduğumuz employee için bir picture oluşturalım.
![poli image 2]({{ site.url }}/assets/img/poli2.png)

Son olarak da employee modelimize ait bütün pictures'lara ulaşmayı deneyelim.
![poli image 3]({{ site.url }}/assets/img/poli3.png)

Gördüğünüz gibi sağlıklı bir şekilde polymorphic ilişkimizi oluşturduk. Burada eğer önce picture'ı oluşturup daha sonra model'e bağlamak istiyorsak imageable_type ve imageable_id kısımlarını da doldurmamız gerekiyor.

İşlemlerimiz burada bitti. Eğer yazıda bir hata ya da eksik olduğunu düşünüyorsan yorum olarak belirtmeni çok isterim.

Sevgiler

Kaynaklar:
[rubyonrails.org](http://guides.rubyonrails.org/association_basics.html#polymorphic-associations)
