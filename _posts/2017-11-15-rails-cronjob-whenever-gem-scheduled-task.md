---
layout: post
title: Rails da cron job ve whenever gem kullanımı
date:   2017-11-15 20:00:19
modified: 2017-11-15
excerpt: Cron'u ilk geçen yıl aldığım işletim sistemleri dersinde öğrenmiştim
tags: [rails, cron job, whenever gem]
comments: true
---

Cron'u ilk geçen yıl aldığım işletim sistemleri dersinde öğrenmiştim sonra biraz araştırınca hatırlayabildim.
Cron tekrar eden görevlerin zamanlanmasına yardımcı olmak için Unix, Linux işletim sistemleri tarafından kullanılan görev planlayıcısıdır. Sistem tarafından tekrar eden görevleri belirlediğimiz zaman aralıklarında çalışmasını sağlar.

Rails uygulamalarında da belirli görevleri planlamak için cron'u kullanabiliriz. Örneğin istenilen zamanlarda mail atma ya da veritabanından hergün oluşan artık kayıtları silme vs. Bu tür işlemleri gerçekleştirebilmek için cron job ve whenever gem'ini birlikte kullanabiliriz. Bu yazıda daha çok rails tarafını anlatacağım ama cron'un nasıl kullanıldığını öğrenmek için [şuraya](http://www.wikiwand.com/en/Cron) bakabilirsin.

**Whenever Gem**

[whenever gem](http://github.com/javan/whenever) insanların okuyabileceği formatta cron jobları yazmamıza olanak sağlar.


**1) Kurulum** <br />
Gem file dosyamızın içerisine whenever gem'ini ekliyoruz.

{% highlight ruby %}
gem 'whenever', require: false
{% endhighlight %}

Ardından
{% highlight ruby %}
bundle install
{% endhighlight %}

komutunu koşuyoruz.

**2) Kullanımı** <br />
Proje dosyamızın içerisinde whenever'ı Initialise ediyoruz.

{% highlight ruby %}
wheneverize .
{% endhighlight %}

Yukarıdaki komutu çalıştırdığımızda rails projemizin içinde `config/schedule.rb` adında bir dosya oluşturuyor. Hangi işlemlerin ve ne sıklıkla gerçekleştirileceğini bu dosyanın içerisine belirtiyoruz. Örneğin bu örnekte hergün saat 01:00 da veritabanımızdaki bütün veri customerları silmek istiyoruz. Bunu aşağıdaki şekilde belirtiriz. (Buradaki set :output ifadesi işlemlerin doğru çalışıp çalışmadığını kontrol etmek için belirtildi.)
<br />
<script src="http://gist.github.com/ebrugulec/a16e7e1edd30c4b139cc14a858899d73.js"></script>

Şimdi ise yapılacak işlemi tanımlamaya geçelim.

**3) Rake Task Oluşturma** <br />
Cron tasklerini, whenever gem'ini kullanarak otomatik olarak çalışacak şekilde ayarlamanın birkaç yolu var. Fakat burada yanlızca kolay bir yöntem olan rake task oluşturmadan bahsedeceğim. Öncelikle projemizin içerisine bir rake dosyası oluşturalım. `lib/tasks/delete_customers.rake` Ve içeriğini şu şekilde dolduralım.

<script src="http://gist.github.com/ebrugulec/4481ad080d25f35fce5b936c37af49a2.js"></script>

Yukarıdaki kodlarda rake'e özgü birkaç anahtar keyword kullanılmakta. Buradaki namespace:task kombinasyonu ile rake taks'ini nasıl çağıracağımızı belirtiriz. Bunu rails db:migrate örneği ile bağdaştıracak olursak "db" 'namespace' alanına "migrate" de 'task' alanına karşılık gelmektedir. Şimdi rake'i yazdığımıza göre artık console da kullanmayı deneyebiliriz.

{% highlight ruby %}
rake delete:customers
{% endhighlight %}

Artık her saat 01:00 da bütün müşterileri silen bir cron jon'umuz var. Bunun doğru çalışıp çalışmadığını kontrol etmek için testini de yazabiliriz. Normalde unit testinin yazılması gerekiyor ama ben request testlerinin içerisine yazıyorum. Öncelikle
`spec/requests/customers_request_spec.rb`
 dosyamızın içerisine gidip bir describe blogu tanımlayalım. Ve içeriği şu şekilde olsun.

<script src="http://gist.github.com/ebrugulec/37f2ce5bc16f5f255e00098df66751ef.js"></script>

Burada `before do` blogunun içerisinde işlem gerçekleştirilmeden önce yapılacak olayları belirtiyoruz. Öncelikle 9 tane customer oluşturduk ve `Timecop.freeze` ile zamanı sabitledik. (Timecop rails uygulamalarında zamanı yönetebilme imkanı sağlıyor. Detaylı bilgi için [şurada](http://github.com/travisjeffery/timecop) bakabilirsiniz.) Ardından lib klasörün içerisine yazdığımız rake dosyamızı testlerimizin içerisine yüklüyoruz. Ve bir environment tanımlıyoruz. `it blogunun` içerisinde ise yapılacak işi ve sonucunu kontrol ediyoruz. Blog içinde belirttiğiz "task" ifadesine yarattığımız rake'in task'ini atıyoruz. expect blogunda yapılan işlemi tanımlıyoruz. Burada oluşturduğumuz task'i çağırdık ve bu task çalıştığında oluşturduğumuz 9 customer'ın silinip silinmediğini kontrol ettik. Son olarak ise zamanı serbest bıraktık.

İşlemlerimiz burada bitti. Eğer yazıda bir hata ya da eksik olduğunu düşünüyorsan yorum olarak belirtmeni çok isterim.

Sevgiler

Kaynaklar<br />
[http://eewang.github.io/](http://eewang.github.io/blog/2013/03/12/how-to-schedule-tasks-using-whenever/)<br />
[http://jameshuynh.com/](http://jameshuynh.com/2016/02/16/using-cron-job-and-whenever-gem-to-run-scheduled-task-in-rails/)
