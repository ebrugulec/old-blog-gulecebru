---
layout: post
title: Web Servisleri Dersi Client-Server Ödevi
date:   2017-11-30 20:00:19
excerpt: Ödevin nasıl olması gerektiğini
modified: 2017-11-30
tags: [netbeans, client-server, glassfish, apache2, php, jsoup, php-soap]
comments: true
---

Ödevin nasıl olması gerektiğini [şu](http://github.com/ebrugulec/Web-Services/blob/master/Week8/homework/server-client.pdf) adresten öğrenebilirsiniz.
Not: Bilgisayarınızda netbeans, apache2, php ve php-soap'ın kurulu olduğunu farzederek anlatmaya başlıyorum.

Öncelikle netbeans ile yeni bir Java Web Application'ı oluşturmamız gerekiyor.

![Smithsonian Image]({{ site.url }}/assets/img/web-services-homework.png)
{: .pull-left}

Ardından oluşturduğumuz projeye bir web servise yaratıyoruz.

![Smithsonian Image]({{ site.url }}/assets/img/web-services-homework2.png)
{: .pull-left}

Yarattığımız web servis'in içeriği ilk etapta şu şekilde olacaktır.

![Smithsonian Image]({{ site.url }}/assets/img/web-services-homework3.png)
{: .pull-left}

Oluşturduğumuz servis'in çalışıp çalışmadığını **Test Web Service** yapısı ile kontrol edebiliriz.

![Smithsonian Image]({{ site.url }}/assets/img/web-services-homework4.png)
{: .pull-left}

Serviste varsayılan olarak gelen **Hello** fonksiyonu verilen değeri **hello** string'i ile birleştirip geri göndürmektedir.

![Smithsonian Image]({{ site.url }}/assets/img/web-services-homework5.png)
{: .pull-left}

Şimdi web servis için gerekli olan **Jsoup** kütüphanesini projeye dahil edelim.

![Smithsonian Image]({{ site.url }}/assets/img/web-services-homework6.png)
{: .pull-left}

Ardından twitter dan profil resmini çekmek için gerekli olan kodları yazalım.

<script src="http://gist.github.com/ebrugulec/902e9c80a9f1483d0bceebc7e6483693.js"></script>

Burada fonksiyon tek parametre alıyor **username** adında. Ve gönderilen veriyi **http://www.twitter.com/** adresi ile birleştiriyor. Ardından Jsoup kütüphanesini kullanarak verilen adres ile bağlantı kuruyor. Dönen değeri parse ederek **doc** değişkenine atıyor. Bu değişkende sayfa kaynağımız mevcut. Ancak biz burada yanlızca Twitter'ın profil resmi için tanımladığı etikete ulaşmak istiyoruz. Yani aşağıdaki resimde gördüğünüz **ProfileAvatar-image** ifadesine.

![Smithsonian Image]({{ site.url }}/assets/img/web-services-homework7.png)
{: .pull-left}

Bu yüzden Document türündeki veri'nin içinde image etiketini ve class bilgisini seçiyoruz. Ve gelen ilk elementin "src" bilgisini tanımladığımız **userImage** değişkenine atıyoruz. Eğer uygulama herhangi bir hata ile karşılaşırsa diye try-catch ibaresini de eklememiz gerekiyor. Web servis kısmı buraya kadardı şimdi client kısmını php ile yazmaya bakalım. Bu arada projenizi deploy etmeyi ve glassfish server'ınızın açık olup olmadığını kontrol etmeyi unutmayın.

Client kısmından istek atabilmemiz için projenin localimizde tanımlı **wsdl** adresini bilmemiz gerekli. Bu yüzden glassfish'in çalıştığı **4848** portuna bağlanıyoruz. Ve buradan oluşturuğumuz uygulamayı seçiyoruz.

![Smithsonian Image]({{ site.url }}/assets/img/web-services-homework8.png)
{: .pull-left}

Açılan pencerede **Module and Components** kısmında oluşturduğumuz servis'in endpoint'ini görüntülüyoruz.(View Endpoint)

![Smithsonian Image]({{ site.url }}/assets/img/web-services-homework9.png)
{: .pull-left}

Açılan pencerede farklı portlara ait server adresleri var. Bunlardan herhangi birini seçebiliriz. Ben **8080** portunu seçiyorum.

![Smithsonian Image]({{ site.url }}/assets/img/web-services-homework10.png)
{: .pull-left}

Burada yazdığımız web servis'e istek atabilmemiz için **wsdl_url** yeni bir Soap client'ı oluşturuyoruz. Ve web servis tarafında tanımladığımız **getTwitPic** metoduna kullanıcı adını gönderiyoruz. Gelen değeri de **img** etiketi içine ekliyoruz. Kodları aşağıdaki gibi.

![Smithsonian Image]({{ site.url }}/assets/img/web-services-homework11.png)
{: .pull-left}

_
<script src="http://gist.github.com/ebrugulec/8e98ff954860731afe6ff04da1d7377e.js"></script>

Ardında **apache server**'ı başlatıyoruz.

![Smithsonian Image]({{ site.url }}/assets/img/web-services-homework12.png)
{: .pull-left}

Son olarak da localhost'tan projenin çalışıp çalışmadığını kontrol ediyoruz. Php dosyalarımı tuttuğum path şu şekilde. **/var/www/html/WebServices**

Bu sebeple tarayıcıyı açmak için kullanacağımız adres: **http://localhost/WebServices/test.php** olmalı. Hemen bu adrese gidelim.

![Smithsonian Image]({{ site.url }}/assets/img/web-services-homework13.png)
{: .pull-left}

Ve gördüğünüz gibi oluşturduğumuz client-server uygulamamız doğru bir şekilde çalışıyor. Benzer işlemi instagram için de yapalım. Burada php kısmında yalnızca çağırdığımız metod adı ve istersek username ifadesi değişecek. Java web servis metodu da twitter'a benzer olarak şu şekilde yazılmalı.

<script src="http://gist.github.com/ebrugulec/97fbda02c6e3321fac4d3a99c7b8c72b.js"></script>

Instagram etiketleri farklı şekilde tuttuğu için burada yanlızca parse etme işlemimizde ulaşmak istediğimiz etiketleri değişiyoruz.

![Smithsonian Image]({{ site.url }}/assets/img/web-services-homework14.png)
{: .pull-left}

Tarayıcıyı yenilediğimizde kodlarımızın doğru bir şekilde çalıştığını görebiliriz.

![Smithsonian Image]({{ site.url }}/assets/img/web-services-homework15.png)
{: .pull-left}


Anlatacaklarım burada bitti. Eğer yazıda bir hata ya da eksik olduğunu düşünüyorsan yorum olarak belirtmeni çok isterim.<br />

Sevgiler

Teşekkürler<br />
Yavuz Kuru
