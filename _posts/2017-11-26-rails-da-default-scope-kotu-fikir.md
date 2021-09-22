---
layout: post
title: Rails da default_scope kullanmak neden kötü fikir?
date:   2017-11-26 20:00:19
excerpt: Rails da activerecord, model üzerindeki bütün işlemler için varsayınlan
tags: [rails, default_scope]
comments: true
---
Rails da Active Record, model üzerindeki bütün işlemler için varsayınlan bir kapsamı default_scope aracılığı ile belirler. Bu yazıda neden default_scope kullanmanın kötü bir fikir olduğundan bahsedeceğim.

Öncelikle bir `Post` modelimizin olduğunu varsayalım. Ve onun için bir `default_scope` belirleyelim.

{% highlight ruby %}
class Post
  default_scope where(published: true).order("created_at desc")
end
{% endhighlight %}

Modelimize default_scope'u tanımladıktan sonra bazı beklemediğimiz davranışlar otomatik olarak eklenecektir.

**1) default_scope'u geçersiz kılamayız**

<script src="http://gist.github.com/ebrugulec/85c91f93d4628419fb0b9d576b7a0d4e.js"></script>

Yukarıdaki sorguda ilk 10 adet post'a ulaşmak istiyoruz ancak varsayılan olarak default_scope tanımlı olduğu için postları created_at field'ını dikkate alarak listeler.

Eğer `Post`'ları sıralamak için **created_at** değil de **updated_at** kullanmak istersek;

<script src="http://gist.github.com/ebrugulec/19890f8e66757e33684470e29a23b958.js"></script>

yukarıdaki sorguda gördüğünüz gibi hem created_at hem de updated_at'e göre sıralama yapacaktır. Çünkü default_scope üzerine yazılamaz. İşlemlerimizde default_scope'u devre dışı bırakmak için `unscoped`'ı kullanabiliriz.

<script src="http://gist.github.com/ebrugulec/ad914454b49b72c37032330b23829d81.js"></script>

**2) default_scope model initialization'ı etkiler**

Örneğin yeni bir `Post` oluşturmak istediğimizde, modelimizin içerisinde default_scope tanımlı olduğu için oluşturma işlemini doğrudan etkileyecek ve yeni oluşturacağımız post'un published field'ını true olarak verecektir.

<script src="http://gist.github.com/ebrugulec/cfe4389f8f7668e952807762ccb19fb0.js"></script>

Birçok developer default_scope ile yalnızca sorgularının etkilendiğini düşünmektedir. Ama ne yazık ki hiç istemediğimiz sonuçlar doğabilir. Bu yüzden uygulamalarımızda default_scope yerine normal scope'u kullanmak sonrası için daha kullanışlı çözümler sağlamaktadır.

Kaynaklar<br />
[rails-bestpractices](http://rails-bestpractices.com/posts/2013/06/15/default_scope-is-evil)

Anlatacaklarım burada bitti. Eğer yazıda bir hata ya da eksik olduğunu düşünüyorsan yorum olarak belirtmeni çok isterim.<br />

Sevgiler
