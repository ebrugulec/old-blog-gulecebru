---
layout: post
title: Background Processing with Rails, Redis and Sidekiq
date:   2018-01-22 20:00:19
excerpt: En basit ifadesi ile redis, key-value şeklinde çalışan
tags: [rails, redis, sidekiq, active job]
comments: true
img: reis-redis.png
---

**Redis**

  En basit ifadesi ile redis, key-value şeklinde çalışan **data structure** deposudur. Kısmen memcache yapısına benzediğini de söyleyebiliriz. Memcache giriş-çıkış yapısını destekler. Yani veriler Ram de işlem görür ve işi biter veriler silinir. Diske herhangi bir veri giriş-çıkışı yapmaz. Yani işlem yapılan verilerimiz kaydedilmez. Ancak redis'in memecache den farkı client dan aldığı verileri ram'e depo eder ve istenildiğinde disk'e kaydeder. Redis'in avantajı verileri bellekte tuttuğu için istenildiği anda çabucak alınıp işlenilmesini sağlar. Bu da projelerde performans açısından büyük yarar sağlar. Redis'in verileri şifreleyerek depolamaması ve bağlantı güvenliği için ek sistemlere ihtiyaç duyması dezavantajları olarak söylenebilir.

**Sidekiq**

  Sidekiq Ruby için geliştirilen bir background processing framework'ü dür. Amaçlarından birisi de Rails'a basit bir şekilde entegre olup yüksek performansı çözümler üretebilmektir.

*Rails İle Kullanımı*

  1) Gemfile dosyasına sidekiq'i eklenir.
  {% highlight ruby %}
    gem 'sidekiq'
  {% endhighlight %}

  2) app/workers'ın içerisine job ları eşzamansız işlemek için bir worker oluşturulur.

  {% highlight ruby %}
    rails g sidekiq:worker Hard # will create app/workers/hard_worker.rb
  {% endhighlight %}

  {% highlight ruby %}
    class HardWorker
      include Sidekiq::Worker
      def perform(name, count)
        # do something
      end
    end
  {% endhighlight %}

  3) Bir job oluşturulur

  {% highlight ruby %}
    HardWorker.perform_async('bob', 5)
  {% endhighlight %}

  Not: *perform* bir instance metodudur, buna karşılık *perform_async* bir *class* çağırır.
  => Gelecekte çalışması için de job'lar yaratabiliriz.

  {% highlight ruby %}
    HardWorker.perform_in(5.minutes, 'bob', 5)
  {% endhighlight %}

  4) Sidekiq'i başlatırız.

  {% highlight ruby %}
    bundle exec sidekiq
  {% endhighlight %}

  Sidekiq arka planda çalışarak uygulamalarımızı ölçeklendirmemize olanak tanır. Bunun için üç kısım gereklidir:

  1) *Client*
  Sidekiq client herhangi bir ruby process'inde çalışır ve daha sonra çalıması için job lar oluşturabilmeye izin verir. Uygulamalarımızda iki şekilde job oluşturabiliriz.

  {% highlight ruby %}
  MyWorker.perform_async(1, 2, 3)
  Sidekiq::Client.push('class' => MyWorker, 'args' => [1, 2, 3])  # Lower-level generic API
  {% endhighlight %}

  Bu iki metod birbirine eşittir ve job'ları temsil eden bir hash oluşturur. Bu hash JSON string hale dönüştürülür ve Redis kuyruğuna String olarak eklenir. Bunun anlamı worker'ın argümanlarının basit JSON veri türleri(numbers, strings, boolean, array, hash) olması gerektiğini ifade eder. Complex ruby objeleri (Date, Time, ActiveRecord models) uygun şekilde serialize edilemez.

  2) *Redis*
  Sidekiq, tüm job ve operasyonel verilerini depolamak için Redis'i kullanır.

  3) *Server*
  Her Sidekiq server gerçekleştirilecek jobları, Redis'deki kuyruktan çeker ve işler.

  => Sidekiq [Idempotence](http://www.wikiwand.com/en/Idempotence) 'dır. Yani varolan jobları birçok defa aynı şekilde çalıştırabiliriz. Örneğin job'a bir işlem tahsis ettiğimizde yarısını gerçekleştirirken hata fırlattı. Sonrasında işlem başarılı olana kadar tekrar ve tekrar çalıtırılır.

  => Sidekiq işlemlerimizi paralel bir şekilde çalıştırmak için tasarlanmıştır.

  **Scheduled Jobs**
  Sidekiq, bir job'ın ne zaman yürüteceğini zamanlamamıza izin verir. *perform_async(*args)* yerine *perform_in(interval, *args)* ya da *perform_at(timestamp, *args)* kullanılabilir.

  {% highlight ruby %}
  MyWorker.perform_in(3.hours, 'mike', 1)
  MyWorker.perform_at(3.hours.from_now, 'mike', 1)
  {% endhighlight %}

  **Active Job**
  Active Job, job tanımlama ve onları server tarafonda çeşitli kuyruklarda çalıştırmak için kullanılan bir framework'dür.

  Bir job oluşturmak için;
  {% highlight ruby %}
    rails generate job Example
  {% endhighlight %}

  Bu komut ile /app/jobs/example_job.rb adında bir dosya oluşturulur.
  <script src="http://gist.github.com/ebrugulec/0b91999f848e4d59e37aeb341aa11027.js"></script>

  *Kullanımı*

  Job'lar job kuyruğunun herhangi bir yerine eklenebilirler. Jobları işemlerni gerçekletirebilme için kuyruğa eklemek;

  {% highlight ruby %}
    ExampleJob.perform_later args
  {% endhighlight %}

  Bu noktada Sidekiq jobları bizim için çalıştırır. Herhangi bir nedenden ötürü job'ın çalışmasında hata ile karşılaşışırsa Sidekiw tekrardan işlemi gerçekleştirmek için hata olan job'ı çalıştıracaktır.

  *Error Handling Özelleştirme*
  ActiveJob Sidekiq'in yeniden deneme özelliklerinin tüm zenginliğini desteklemez. Bunun yerine, özel istisnalarla karşılaşıldığında yeniden denemeleri kodlamak için basit bir soyutlamaya sahiptir.

  <script src="http://gist.github.com/ebrugulec/9b65875e1d2c865151019e7da067789c.js"></script>

  * ActiveJob ile jobları yeniden denemekle, çok fazla Sidekiq işlevselliğini kaybederiz.

  Kaynaklar
  * [Sidekiq](http://github.com/mperham/sidekiq)
  * [Active Job](http://edgeguides.rubyonrails.org/active_job_basics.html)
  * [Redis](http://redis.io/)
