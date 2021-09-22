---
layout: post
title: JavaScript'de Web Workers, Service Workers ve Worklets
date:   2019-03-11 10:56:19
modified:  2019-03-11
comments: true
img: workers.jpg
---

Web workers, service workers ve worklets, JavaScript Workers olarak adlandırılır. Çalışma şekilleri benzerlik gösterse de kullanım yerleri nispeten farklıdır. JavaScript kodları browserda ki ui elementleri ile beraber aynı thread de çalışmaktadır. Eğer uzun süreli çalışan bir javascript görevimiz varsa, bu görev tamamlanana kadar, browser kullanıcı işlemlerine cevapsız kalmaktadır. Bundan dolayı da bir işlem tamamlana kadar kullanıcı hiçbir şey yapamamaktadır. JavaScript Workerlar javascript kodunun browser kullanıcı arayüzünün çalıştığı threaddan farklı bir thread de çalıştırmayı sağlarlar. Bu sayede uzun süreli scriptler kullanıcı ile browser etkileşimini etkilemeden çalıştırılabilirler. Ayrıca, Worker' lar ayrı thread de çalıştığından, DOM'a yada diğer UI foksiyonlarına erişemezler.

## Web Workers

En genel amaçlı worker türü web workerlardır. service worker ve worklet'lerin aksine ana thread'den ayrı çalışması dışında sipesifik use case'leri yoktur. Özet olarak web worker'ları ana thread'e yüklenmiş aşırı yükü azaltmak için kullanılırlar.

![Workers]({{ site.url }}/assets/img/worker1.jpg)

Bir örnek ile açıklayacak olursak, mesaj gönderme işlemini web worker kullanarak yapalım.

````javascript
/* main.js */
const myWorker = new Worker('worker.js');
```

Burada tanımladığımız 'worker.js' dosyamızdan yeni bir worker yarattık. Şimdi de bu worker ı kullanalım.

<script src="https://gist.github.com/ebrugulec/a3209727f4c2c774b4cacf409196ee9a.js"></script>

worker.js, main.js den gelen mesajları dinler ve response döner.

<script src="https://gist.github.com/ebrugulec/fa1290b20eea1bbd49d2f53142a204c9.js"></script>

## Service Workers

Service Worker; tarayıcının arka planında, web sayfasından ayrı olarak çalıştırdığı bir komuttur. Tarayıcı ve network/ cache arasında proxy görevi görür. Service Workers, web sayfası veya kullanıcı etkileşimi gerektirmeyen özelliklere açılan bir kapı niteliğindedir. Bugün bu özellikler arasında push bildirimler, arka planda senkronizasyon vb. bulunmaktadır.

![Workers]({{ site.url }}/assets/img/worker2.jpg)

Web Workers gibi Service Workers da ayrı bir js dosyasında tutulur ve işlemi gerçekleştirilecek olan ana js dosyasının içinde kullanılır.

```javascript
/* main.js */
navigator.serviceWorker.register('/service-worker.js');
```

Web Workersların sahip olduğu özellikler dışında Services Workersların proxy işlemlerini yerine getirebilmeleri için bazı ek özelliklere sahiptirler. Kurulduktan ve etkinleştirildikten sonra, Service Workers ana belgeden yapılan tüm ağ isteklerini engelleyebilir.

<script src="https://gist.github.com/ebrugulec/b1cc1c679ba9bd189a52da7cac0768a1.js"></script>

Yapılan requestlerde ağa gitmek yerine cache'den veri döndürerek web uygulamalarının çevrimdışı çalışmasına izin verir.

<script src="https://gist.github.com/ebrugulec/9154ce9fa01d913fc69c4c012dcc7486.js"></script>

## Worklets
Web sayfamız renderlanırken, tarayıcımız [birkaç adımdan](https://bitsofco.de/understanding-the-critical-rendering-path/) geçer.

![Workers]({{ site.url }}/assets/img/worker3.jpg)

Örneğin Paint kısmında tarayıcımız her bir elemente varolan stillerini uygular. Bu oluşturma aşamasına kanca görevi gören ve işlemin tamamlanmasını sağlayan Paint Worklet dir.

Paint worklet CSS in görüntü beklediği herhangi bir yere uygulanacak özel görüntüler oluşturmamızı sağlar. Örneğin background-image in değeri gibi.

Diğer workers'larda olduğu gibi worklets'de main dosyamızın içerisinde kullanılır.

```javascript
/* main.js */
CSS.paintWorklet.addModule('myWorklet.js');
```

myWorklet dosyamızın içerisinde bir custom image oluşturabiliriz. paint metodu canvas api ile benzer çalışır. Worklet ile basit bir gradient şöyle oluşturulabilir.

<script src="https://gist.github.com/ebrugulec/fce1d8a7d622db47cfe61c51f0d0c240.js"></script>

Diğer arka plan resimlerinde olduğu gibi worklet ile özelleştirdiğimiz arka plan resmini istediğimiz elemette kullanabiliriz.

```bash
div {
    background-image: paint(myGradient);
}
```

Paint Worklet e ek olarak, rendering işleminde diğer stagelere bağlanan worklets çeşitleri de var. Örneğin Animation Worklet Composite stage e bağlanır, Layout Worklet Layout stage e bağlanır gibi.
Kısaca Workletleri özetleyecek olursak; Wrokletler geliştiricilere rendering işleminin low-level kısımlarına müdahale etme izni veren Web Workerların hafif bir sürümüdür.

Kaynaklar:

[Worklet Mozilla](https://developer.mozilla.org/en-US/docs/Web/API/Worklet)

[HTML5 Web Workers](https://kodcu.com/2013/04/html5-web-worker/)

[web and service workers](https://tech.ovoenergy.com/web-workers-vs-service-workers/)

[web and service workers](https://bitsofco.de/web-workers-vs-service-workers-vs-worklets/)
