---
layout: post
title: Todo Da Ne Ki?
date:   2017-06-22 20:00:19
excerpt:
tags: Todo, Rails]
modified: 2016-09-06
comments: true
---

Merhabalar :wave: Giriş kısımlarını bir türlü beceremediğim için hemen konuya giriyorum. Bu blog postunu yazmamın amacı size rails da bir todo uygulaması nasıl yapılır onu anlatmak ancak gelin önce rails maceramdan ve neden bu işe kalkıştığımdan bahsedeyim.

Bir süredir Ruby ve Ruby on Rails ile ilgileniyorum. Neden Ruby, neden Rails derseniz tonlarca mantıklı sebep sıralanabilir. Benim de bu mantıklı sebeplerin haricinde bir nedenim daha vardı. Artık kendimi tam olarak ifade edebildiğim, kullanırken mutlu olduğum ve süpervistik projeler yapabileceğim bir şey bulma isteğimden kaynaklanıyordu. Aramızda kalsın ama yeni şeyler denemeye, sürekli farklı şeyler öğrenmeye inanılmaz açık biriyim. Şurda şu mu çıkmış hemen bende öğrenmeliyim. Yok efendim bak bunun yeni bir özelliği mi varmış; hani nerde?, ooo Ebru bak bunu kesin denemelisin; dur dur hemen geliyorum gibi gibi önünü alamadığım tonlarca isteğim var. Bu anlattıklarımı daha rahat anlayabilmeniz için şu [tedx konuşmasını](http://youtu.be/4sZdcB6bjI8) dinlemenizi tavsiye ederim. Ha eğer dinlemek istemezseniz de bu durumu en net şekilde “maymun iştahlılık” olarak anlatabilirim. Bunu kendimde farketmem uzun zamanıma mal oldu. Sürekli deneyip, çalışıp işe yarar bir şey ortaya çıkaramayınca delirdim. Kendimi hırpaladım, öğrenmeye çalıştığım dillere, çatılara çamur attım. Sonra farkettim ki sıkıntı bendeydi. Ben kararlı olup ilerleyemiyordum. Böyle olunca da kendimi inanılmaz kötü hissediyordum. Ve bir karar verip kendimi en rahat ifade edebileceğim, aklımdaki projeleri yapabileceğim uygun dili, çatıyı aramaya başladım. Şaka şaka başlamadım ne olduğunu zaten biliyordum. O ruby idi :innocent:

Ruby ile ilk tanışmam bir konferans da dinlediğim [Serdar abi](http://serdardogruyol.com/) sayesinde olmuştu.(Serdar abi ve ruby camiasını övmeyi bir başka yazıya bırakıyorum yoksa içinden çıkamayacağım) Neyse efenim o zamanlar çok takmadım tabi çünkü kafa kimbilir nerelerdeydi. Ancak sonrasında yazılıma ve sektöre dair bir şeyler bende oturunca fark ettim ve araştırmaya koyuldum. Araştırdıkça rubye dedim ki; bu ne güzelliktir yarabbi... seni öğrenmek istiyorum [yiğidim](http://imgim.com/620x190-kopya.jpg)... :sweat_smile: Şuan bir teknik yazıda geyiğin dibine vuruyorum yaa. İnanılmaz keyifli, durun azıcık daha devam edicem yemişim teknikliği :satisfied: Aşağıdaki paragraftan devam ediyorum.

Yavaş yavaş ruby, rails öğreniyorum, çabalıyorum felan sonra kendimde bir şey daha fark ettim. Ben öğrenirken ilk aşamada tutorial'dan ilerleyemiyorum. Bildiğiniz tutorial okuma özürlüsüyüm.(Bu ara bunu aşmaya çalışıyorum) İlla süreci başından sonuna kadar gözlemlemem gerekiyor ya da birinden dinlemem. Ardından bunu uygulayarak ilerleyebiliyorum. Şurası şöyle olsaydı ne olurdu derken bir bakıyorum zaten öğrenmişim. Kendimde bunu çözdükten sonra internette rails ile ilgili videolu dersleri araştırdım. Çoğunluğu giriş aşamasındaydı ve benim işime yaramazdı. Ardından Mackenzie Child'ın youtubedeki [şu](http://www.youtube.com/channel/UCfWZwsP8trUy5uHJg8gcGIQ) play listine denk geldim. Ve inanılmaz mutlu oldum. Tam istediğim şekilde uygulayarak öğretiyordu. Hemen kolları sıvayıp tek tek videodaki uygulamaları yapmaya başladım. Yaparken eğlendim, öğrendim çoğunlukla da yok artık bu nasıl bu kadar kolay olabilir ya dedim. Gerçekten de rails ile bir şeyler yapmak inanılmaz kolaydı. Hatta çoğunlukla kendimi fazlalık gibi hissettim. Sanki o arkada her işi yapıyor da beni ayıp olmasın diye başında bekletiyor gibiydi. Mackenzie uygulamaları Rails 4 ile yaparken ben 5 ile yaptım. Bazı noktalarda problem oldu tabi. örneğin onun kullandığı gemlerin rails 5 desteği yoktu ya da yapıları değişmişti. Bir şekilde bunları çözmeyi denedim. Vurdum, kırdım, bozdum, patlattım derken kafasını gözünü yara yara rails yazdım. Sonra düşündüm, rails konusundaki Türkçe kaynak eksiği bir hayli fazlaydı ve dünyanın bilmem neresindeki bir insan hiçbir karşılığı olmadan benim bir şeyler öğrenmeme sebep oluyorsa ben de bir başkasının öğrenmesine vesile olmalıydım diyerek anlatmaya karar verdim. Belki bunun gereksiz olduğunu düşünebilirsin. Adam sonuçta gayet muhteşem bir şekilde anlatmış sana ne gerek, sen niye yazıyorsun diyebilirsin. Belki de haklısındırda. Mamafih bende bunu kendime sordum. Ve birkaç makul yanıt aldım. Öncelikle belki bir başkası benim gibi görerek değil de okuyarak daha iyi öğreniyordur ya da ingilizcesi çok iyi değildir. Yani netice de anlatmak istedim. Neden videoya çemedin diye de sorabilirsin, bu konuda da bir açıklamam var. Kendimi yazarak daha rahat ifade edebiliyordum. Bu sebeple ben de yazmaya karar verdim. Ancak burada da bir problem vardı. Sonuçta ben Mackenzie'nin yaptıklarını ufak tefek değişiklikler olsa da aynılarını anlatacaktım. O üretmek için o kadar emek verirken benim onu kopyalamam (kaynak göstersem bile) çok doğru gelmedi. Ben de ona sorup izin almak istedim ve inanılmaz ciddi bir mail attım. Mailin sonuna da githubdaki repolarını ve blogumun adresini ekledim. Mail cevabı ise hiç beklemediğim bir pozitiflikteydi.

![mail answer]({{ site.url }}/assets/img/email_answer.png)

Ve bütün engeller ortadan kalktığına göre artık anlatmalıydım ve hemen işe koyuldum. (Hemen yapmadı)

Başlıyoruz..<br/>
![todo image 1]({{ site.url }}/assets/img/todo7.jpg)

<h2>Rails ile Todo App</h2>(Asıl uygulamanın anlatıldığı kısım)
	Uygulamayı anlatırken ruby ve rails'ın nasıl kurulduğundan bahsetmeyeceğim çünkü google da küçük bir arama ile bunu öğrenebilirsiniz. Onun yerine direk uygulama kısmına geçiyorum.<br/>
	Bu arada benim kullandığım ruby ve rails versiyonu;<br/>
	ruby: ruby 2.2.2p95<br/>
	rails: Rails 5.0.4<br/>
:small_red_triangle: Rails da yeni bir uygulama başlatmak için terminalimizden

{% highlight ruby %}
$ rails new uygulamaadi(Todo-App)
{% endhighlight %}

komutumuzu çalıştırıyoruz. Böylece rails gerekli olan yapıları oluşturuyor. Uygulamamızı tarayıcıda görüntüleyebilmek için

{% highlight ruby %}
$ cd  Todo-App
$ rails server
{% endhighlight %}

komutunu çalıştırıyoruz.

*http://localhost:3000/* adresine giderek uygulamamızın çalışıp çalışmadığını kontrol edebiliriz.

![todo image 1]({{ site.url }}/assets/img/todo1.png)

Tebrikler artık çalışan bir rails uyuglamasına sahipsin ancak işin daha başında olduğunu unutma :)
Hadi devam edelim

Rails da scaffold adında kullanması inanılmaz kolay bir yapı var. Şöyle ki biz scaffold oluşturmak istediğimiz zaman rails bizim için model, view, controller hepsini otomatik olarak oluşturuyor. Bu da işimizi bir hayli hızlandırıyor. Bu uygulamada da öncelikle bir yapılacaklar listemize ihtiyacımız var yani todo listimiz olmalı. Scaffold ile todo list oluşturmak için;

{% highlight ruby %}
$ rails g scaffold todo_list title:string description:text
{% endhighlight %}

Hemen ardında yapılan değişiklikleri veritabanına aktarmalıyız.

{% highlight ruby %}
$ rails db:migrate
{% endhighlight %}

Oluşturduğumuz yapıyı görüntülemek için tarayıcımızdan *http://localhost:3000/todo_lists* adresine gidersek şöyle bir ekran ile karşılaşırız.

![todo image 1]({{ site.url }}/assets/img/todo2.png)

Şimdilik her şey yolunda gibi görünüyor. Yeni bir todo list ekleyip onu görüntülemeyi deneyebilirsin.

![todo image 1]({{ site.url }}/assets/img/todo3.png)

Artık bütün todoları görüntüleyebileceğimiz bir index sayfamız var ancak bu sayfayı direk *http://localhost:3000* adresinden görmemiz için root.rb dosyamızın içerisine

{% highlight ruby %}
root "todo_lists#index"
{% endhighlight %}

ifadesini eklemeliyiz.

Yapılacaklar listemizi(todo list) oluşturduktan sonra sıra geldi içerisine görevleri ekleyebileceğimiz todo item kısmını oluşturmaya Bunun için öncelikle bir model oluşturmalıyız.

{% highlight ruby %}
$ rails g model todo_item content:string todo_list:references
$ rails db:migrate
{% endhighlight %}

Buradaki **todo_list:references**
ifadesi modeller arasında ilişkilendirme yapmak için kullanılıyor.
Eğer **app/models/todo_item.rb** dosyasını kontrol ederseniz orada **belongs_to :todo_list** ibaresini görebilirsiniz. Bu ilişkiyi tam anlamı ile oluşturmak için todo list modelimize todo_item'ın varlığından haberdar etmeliyiz. Yani Todo list'in items ları olabilir. Bunu belirtmek için **app/models/todo_list.rb** dosyamıza gidip

{% highlight ruby %}
  has_many :todo_items
{% endhighlight %}

ifadesini eklemeliyiz.
Modelimiz oluşturduktan sonra sıra geldi todo item controllerını oluşturmaya. Bunun için;

{% highlight ruby %}
rails g controller todo_list
{% endhighlight %}
dememiz yeterli. Ardından da controller'ımızın actionlarını oluşturalım.

<script src="http://gist.github.com/ebrugulec/0acd7b44e891e165a48d360f54832fc8.js"></script>

Kısaca actionları açıklamak gerekirse;
**set_todo_list** metodu ile itemları ekleyaceğimiz todo list'i buluyoruz. **create** metodu ile de bulduğumuz todo list'e yeni bir item oluşturuyoruz. **destroy** metodu ise tahmin edeceğiniz üzere oluşturduğumuz todo item'ı silmemize yarıyor. Son olarak **complete** metodu ise o görevin yapılıp yapılmadığını kontrol ederken kullanıyoruz.

Metodların yazım diline çok yakın oldukları için ilk baktığınızda çoğunlukla ne işe yaradıkları anlaşılıyor o yüzden üzerinde fazla durmayacağım. Ancak burada **before_action**'ları kullanmamız belki kafanızı karıştırabilir. Bunun sebebi tekrarı önlemek için. Örneğin todo item oluştururken de, yok ederken de elimizde bir todo list'imizin olması gerekli. Sürekli
<b style="color: purple">@todo_list = TodoList.find(params[:todo_list_id])</b> ifadesini çağırmak yerine bunu bir fonksiyonun içerisine koyup istenilen her fonksiyondan önce çağırmak için before_action'ı kullanırız.

Todo item controller ve modelimizi oluşturduktan sonra sıra geldi MVC yapımızın son katmanı olan view kısmını oluşturmaya. Bunun için **app/views/todo_items** klasörünün içerisine
 **_form.html.erb** ve **_todo_item.html.erb** dosyamızı oluşturuyoruz.

**_form.html.erb** içeriği;

<script src="http://gist.github.com/ebrugulec/0abbf53c0b445839fa6e342e3d5ae88d.js"></script>

**_todo_item.html.erb** içeriği;

<script src="http://gist.github.com/ebrugulec/57efa7ae97dd57af86bd3225b72e6b1c.js"></script>

Artık todo itemların view kısımları hazır. Son adım olarak bunları halihazırda scaffold ile oluşturulan todo list'imizin içerisinde gösterme kısmı kalıyor. Bunun için **app/views/todo_lists/show.html.erb** dosyamızın içerisine todo item'ın view larını gömmeliyiz. O dosyamız da şu şekilde;

<script src="http://gist.github.com/ebrugulec/ba8519df17345804a078395b9c63ea9d.js"></script>

Bu işlemi yaptıktan sonra **config/routes.rb** dosyamızda biraz değişiklik yapmamız gerekli. Çünkü yeni bir item oluşturduğumuzda bu item seçilen todo list'in altında olmalı, oradan ulaşabilmeliyiz. Bu yüzden içi içe resources tanımladık.

<script src="http://gist.github.com/ebrugulec/7621da2229ea5838ede81c32754fe1dd.js"></script>

En son yaptığımız eklemeler ile uygulamamız şu şekilde görünmeli.

![todo image 1]({{ site.url }}/assets/img/todo4.png)

Evettt geldik en son kısma. Şuana kadar yeni bir görev ekleyip, silebiliyoruz. Hadi bu görevlerin bir de tamamlanıp tamamlanmadığını belirten bir ibare koyalım. Bunun için hemen consolumuza dönelim ve bir migration oluşturalım.

{% highlight ruby %}
$ rails g migration add_completed_at_to_todo_items completed_at:datetime
$ rails db:migrate
{% endhighlight %}

Controller'ımızda hali hazırda **complete** metodumuz bulunmaktaydı. Bu metod çalıştırıldığında todo item'ın tamamlanmış olduğunu anlıyoruz. Metodu çalıştırmak için **routes.rb** dosyamıza gidip complete action'ının hangi http isteği karşılığında çalıştıracağını belirtiyoruz. **routes.rb** dosyamızın son hali.

<script src="http://gist.github.com/ebrugulec/a5288e62feadc8ba88328409257e0cd6.js"></script>

Tamamlanma işlemini view da göstermek için; **app/views/todo_items/_todo_item.html.erb**
dosyamızın içerisine

{% highlight ruby %}
<%= link_to "Mark as Complete", complete_todo_list_todo_item_path(@todo_list, todo_item.id), method: :patch %>
{% endhighlight %}

ifadesini ekliyoruz. Dosyanın en son hali;
<script src="http://gist.github.com/ebrugulec/76ba338670077e75644b2ae2d49f1573.js"></script>

:heavy_exclamation_mark: path yapısını terminalimizde görüntülemek için
{% highlight ruby %}
$ rails routes
{% endhighlight %}
komutunu kullanabiliriz.

![todo image 1]({{ site.url }}/assets/img/todo3.png)

Buraya kadar geldiysen tebrikler artık çok basitte olsa bir todo app'in var. :tada: :tada:
Style kısmını sana bırakıyorum. O yaratıcı ruhun ile neler yapabileceğini görmek isterim :sweat_smile:
Bu arada uygulamanın çalışır halini [şu adresten](http://nameless-stream-59526.herokuapp.com/) görebilirsin. Github reposu da [şurada](http://github.com/ebrugulec/Todo-App). Herhangi bir problem ile karşılaşırsan ya da fikirlerini belirtmek istersen bana sosyal medyadan veyahut *“ebru[at]gulecebru.com”* adresinde ulaşabileceğini sakın unutma. Kapanışı da yine postun yazılma amacına uygun olarak Ebruca yapıyorum :dizzy:

Turgut Uyar'ın da dediği gibi;

*ben bazen eksilirim biraz*<br/>
*aslında hepimiz eksilirmişiz biraz*<br/>
*bunu sonradan öğrendim*<br/><br/>
*ben aslında her şeyi sonradan öğrendim*<br/>
*herkes herkesi sonradan öğrenirmiş*<br/>
*bunu da sonradan öğrendim*<br/>

Şimdilik hoşçakal :raised_hands:
