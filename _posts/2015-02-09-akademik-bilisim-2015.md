---
layout: post
title: Akademik Bilişim 2015
date:   2015-02-09 15:07:19
excerpt: "Akademik Bilişim Konferansları 2015"
tags: [ab2015, Gnu/Linux]
modified: 2015-02-09
comments: true
---

Merhaba arkadaşlar geçenlerde Akademik Bilişim 2015 Konferansları düzenlendi. 4 gün boyunca farklı birçok alanda eğitimler verildi. Bende Linux'a Giriş eğitimine katıldım. Bu yazımda orada öğrendiklerimi sizlerle paylaşacağım. Şunu da belirtmeliyim kesinlikle kaçırılmaması gereken bir organizasyondu. Eğitmenimiz de Levent Emmungil di. Neyse fazla uzatmadan konuya gireyim.

İlk gün daha çok teorik bilgiler üzerinden gittik. İşletim sistemi nedir, aralarındaki farklar gibi. Öğrendiklerim ile ilgili sizlere bir döküman hazırladım ona buradan ulaşabilirsiniz. Ardından bir Linux dağıtımı olan Ubuntuyu VirtualBox üzerinden kurduk. Kurulumun nasıl yapılacağını buradan  öğrenebilirsiniz. Eğer benim gibi bir windows kullanıcı iseniz ve diski elle bölümlendirecekseniz dikkat etmenizz gereken 2 nokta var. Birincisi swap(takas) alanı. Ram yetersiz kaldığında bilgisayar bu alana başvurur ve gereklidir. Dosya sistemi yoktur. İkincisi bağlanma noktası. Linux'un çalışma prensibi ile ilgilidir.

Daha sonra komut satırında basit düzeyde komutlar öğrendik. Ki komut satırında herşeyi yapabiliriz bir kısıtlamamız yok. Eğer hata mesajı almıyorsak o işlem tamamlanmış demektir. Öğrendiğimiz komutlar;

  {% highlight text %}
  /$ = Normal kullanıcı
  /# = Ana yönetici(Super user)
  mkdir = Dizin oluşturur.(Linux da Klasör ve klasör birbirinden farklı şeylerdir.)
  ls = List'in kısaltmasıdır. Klasörleri listeler.
  ls -l = Ayrıntılı olarak listeler.( -l bir parametredir.)
  Klasör ise başında 'd' vardır. Dosya ise '-' vardır.
  sudo = Super user gibi yap. Root komutu.
  apt-get = Paket yükleme, güncelleme kaldırmada ullanırız.
  sudo apt-get install gparted = Programı yükle.
  sudo apt-get remove gparted = Programı kaldır.
  top = Sistemin kullanımını listeler.
  shutdown = Sistemi kapatmak reset atmak için kullanılır. 
  Parametreleriyle kullanılırsa belirli bir zaman sonrada o işlem gerçekleştirilebilir.
  cp = Kopyalama.
  cron = Tekrarlanan işlemlerde kullanılır. Örneğin sunucu yedekleme gibi.
  crontab -e = Tekrarlanacak işlemin belirtildiği kısım.
  /* * * * * Çalıştırılacak komut
  /1. * = Dakika
  /2. * = Saat
  /3. * = Ayın günleri
  /4. * = Ay
  /5. * = Haftanın günleri
  Örnek = 16 15 7 5 3 cp calisma/* yedek
  touch = Dizin oluşturur.
  chown = Dosyanın haklarını değiştirir.
  rsync = Benim tabirimle süper üper kopyala, yedekleme.
  Not: Komutun tamamını bilmiyorsak Tab tuşuna basarız. 2 defa basarsak alternatifleri gösterir.

  {% endhighlight %}

Yukarıdaki komutları kullanarak apache web sunucu, wordpress ve kişisel bulut sunucu owncloud 'u yükleyip kurduk.
Daha fazla komuta buradan ulaşabilirsiniz.

Kurs bitiminde kısacık dört günün nasıl geçtiğini anlayamadım. Çok güzel şeyler öğrendim, geleceğime yön verecek insanlarla tanıştım, vay be adamlar neler yapıyor dedim ve özgür yazılım ile tanışma fırsatı buldum. Artık bende bir penguenim :) Linux'u ara vermeden öğrenmeye devam edeceğim. Bunun için internette güzel kaynakar var. E mail listeleri, özgür yazılım dergileri gibi. Öğrendiklerimide sizlerle paylaşacağım umarım faydalı olabilirim.

{% highlight text %}
http://www.sourceforge.net
{% endhighlight %}

-Açık kaynaklı yazılımların toplandığı yer.

{% highlight text %}
http://www.distrowatch.com
{% endhighlight %}

-Linux dağıtımlarının tutulduğu yer.
