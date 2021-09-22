---
layout: post
title: Hello Scala!
date:   2016-05-17 11:07:19
excerpt: "Hello Scala!"
tags: [Scala]
modified: 2016-05-17
comments: true
---

Merhabalar kısa bir süre önce Scala'yı öğrenmeye başladım. Türkiye'de ne yazık ki yaygın olarak kullanılmıyor. Bu sebeple bende yabancı kaynaklardan öğrenmeye çalışıyorum(Coursera gibi) Ne kadar yeterli oluyor ne kadar eksik kalıyor o kısım şuan benim için soru işareti ancak zamanla üzerinde çalışarak geliştirilebileceğimi düşünüyorum. Bu yazıyı yazmamın amacı(muhtemelen bir yazı dizisi şeklinde olacak) benim gibi yeni öğrenenlere ya da öğrenmek isteyenlere yardımcı olmak. Burada şunu da belirtmem gerek sanırım; fonksiyonel programlama dünyasına yeni yeni giriş  yaptığım için yazılarımda eksiklikler, hatalar ya da yanlış ifade edilmiş kısımlar olabilir. Lütfen farkettiklerinizi benimle iletişime geçerek düzeltmeme yardımcı olun. Diyerek kısa bir giriş yaptıktan sonra kısaca Scala'nın özelliklerinden bahsedeyim. 

-Hem nesne yönelimli hemde fonksiyonel programlama yapılabilen bir dil.

-Scala da herşey objedir.

-Fonksiyonları bir diğer fonksiyona parametre olarak gönderebiliriz ya da iç içe fonksiyonlar yazabiliriz.

-Hem derlenebilen hemde yorumlanabilen bir dil. 

-Scala'nın kendi derleyicileri olmasına rağmen istenildiğinde Java Byte-Code üretebiliyor.(Bu sayede java kütüphanelerini, framework’lerini, tool’larını rahatlıkla kullanabiliyoruz)

-Scala’nın REPL (Read-Evaluate-Print Loop) denilen bir yorumlayıcısı var.  Bu yourmlayıcı  geliştirciye, yazdığı kodu hızlı bir şekilde çalıştırıp, çabuk bir şekilde geriş dönüş almayı sağlıyor.

-CLI(command-line interpreter) arayüzü mevcut.

⌘⌘ Scala'yı Vim, Emacs, Sublime Text gibi editörlerle yazabileceğiniz gibi Java IDEleri Netbeans, Eclipse, Intellij ile de yazabilirsiniz. 

⌘⌘ Scala programcıları ilk zamanlar Maven kullansalarda, sonraları Simple Build Tool (aka SBT) isimli bir yapılandırma aracı kullanılmaya başlamışlar. Sbt ortamında yazdığınız kodları derlemenin ve çalıştırmanın yanında 'console' diyerek Scalanın etileşimli kabuğunda çabuk ve pratik bir şekilde işlemler yapabilirsiniz.

Şimdilik Scalaya dair bildiklerim bu kadar, yeni bilgiler öğrendikçe bu yazıyı güncellemeye çalışacağım. Yazıyı burada noktalamadan önce çoğu programcının yaptığı gibi Scala'ya ilk etapta "Hello World" ile başlayalım.

<script src="http://gist.github.com/GlcEbru/884d2cd3e2758d10b6697f7be256b59a.js"></script>


