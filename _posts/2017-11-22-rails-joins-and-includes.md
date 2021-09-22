---
layout: post
title: Rails da Joins ve Includes Kullanımı
date:   2017-11-22 20:00:19
modified: 2017-11-22
excerpt: Includes ve Joins birbirine çok benzeyen iki Active Record methodudur.
tags: [rails, joins, includes]
comments: true
---
Includes ve Joins birbirine çok benzeyen iki Active Record methodudur. Her ikisi de çok kullanışlı metodlar olmasına karşılık yanlış kullanıldığında performansı azaltabilirler.

**Includes ve Joins arasındaki fark?**

Includes işlem yaparken eager loading kullanırken, joins lazy loading kullanır.
Kısaca eager loading nesnenin ihtiyaç anından çok önce yaratılması ve daha sonra kullanmak için hazır halde bekletilmesidir. Bunun tam tersi olarak lazy loading ise nesnenin gerçekten ihtiyaç duyulacağı ana kadar alınmaması, bekletilmesi amacıyla kullanılır.

Ruby on Rails dokumantasyonunda **includes** için yapılan en önemli açıklama şudur;

*"With includes, Active Record ensures that all of the specified associations are loaded using the minimum possible number of queries.""*

Active Record includes ile belirtilen tüm ilişkilerin mümkün olan en az sayıda sorgu ile yüklenmesini sağlar.

Bir başka deyişle bir tabloyu ilişkili bir başka tabloyla sorgularken, her iki tablo da belleğe yüklenir ve bu da ilişkili verileri almak için gereken veritabanı sorgularının miktarını azaltır. Aşağıdaki kodda bütün şirketler(companies) ile ilişkili olan aktif(active) kişilerin(persons) listesini alıyoruz.

<script src="http://gist.github.com/ebrugulec/9335dec19fd457b393d36c326981c7a4.js"></script>

Burada eğer includes kullanmasaydık herbir şirkette çalışan herbir active kişinin adına ulaşmak için her seferinde ayrı bir veritabanı sorgusu atmak zorunda kalacaktık. Ancak includes kullandığımızda ilişkili person tablosu zaten yüklenmiş ve kullanıma hazır olduğu için tek sorgu ile alıp kullanabildik.

Peki ya bütün şirketlerdeki(companies) ilişkili kişi(person) kaydına ulaşmak fakat Person tablosundaki herhangi bir kaydı görüntülemek istemezsek ne olacak? Burada Person tablosundaki herbir kaydı yüklemek biraz anlamsız geliyor. İşte bu noktada **Joins** devreye giriyor.

Yukarıdaki örneği joins'e uyarlayalım.

<script src="http://gist.github.com/ebrugulec/ba8581ad9b9a6c79b917d4a88caad48e.js"></script>
includes ibaresinin yerini joins ile değiştirelim ve yanlıza company name' e ulaşalım. Joins'in lazy load olduğundan daha önce bahsetmiştim. Burada joins ilişkili tabloyu kullanarak veritabanı sorgusunu gerçekleştirir ancak yanlızca Company table'ı memory'e yükler çünkü burada person tablosu gerekli değildir. Joins kullanarak gereksiz verileri gereksizce memory'e yüklemiyoruz. Eğer Person tablosunun verilerini daha sonra kullanmak istersek, daha fazla veritabanı sorgusu gerekecektir.

**İkna olmadım bana istatistiklerle gel!**

Aşağıdaki index action'ı anormal bir şekilde veritabanı sorguları üretiyordu.

<script src="http://gist.github.com/ebrugulec/d744312d4959cbad5f18e56dabb53da0.js"></script>

![Smithsonian Image]({{ site.url }}/assets/img/includes_vs_joins_1.png)
{: .pull-left}

Burada gördüğünüz gibi tablonun her bir satırında, ilişkili diğer tablolara(zones, tiers) ulaşmak için iki adet sorgu atılıyor. Ölçeklendirildiğinde **265.2ms** de bütün sorgular kaynaktan yükleniyor. Uygulamada performansı ve ölçeklenebilirliği koruyabilmek adına index action'ı includes ile tekrardan düzenleyebiliriz.

<script src="http://gist.github.com/ebrugulec/268004fbd15f1bfd5eec288bbbbd2085.js"></script>

Bu düzenlemeyi yaptıktan sonra Active Record'un yüklenme süresi **2.8ms** oldu. Ve sorgu sayısı yanlızca 5'e düştü. Bu da yüklenme süresinde %99'luk bir azalma demektir. Süpersonik değil mi ^^

![Smithsonian Image]({{ site.url }}/assets/img/includes_vs_joins_2.png)
{: .pull-left}

Kaynaklar<br />
[http://tomdallimore.com/](http://tomdallimore.com/blog/includes-vs-joins-in-rails-when-and-where/)

Anlatacaklarım burada bitti. Eğer yazıda bir hata ya da eksik olduğunu düşünüyorsan yorum olarak belirtmeni çok isterim.<br />

Sevgiler
