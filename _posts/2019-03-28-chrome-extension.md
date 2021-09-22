---
layout: post
title: React ile Chrome Eklentisi Oluşturma
date:   2019-03-28 10:56:19
modified:  2019-03-28
comments: true
img: endoplazmik.jpg
---

Bazı browser eklentileri cidden hayat kurtarıyor. Daha önceden hiç bakmamıştım nasıl yapıldığına. Bu sıra da çok sık görünce 'Ben acaba bunu React ile yapabilir miyim?' diye bakayım dedim. Başta da sanıyorum ki şimdi bin tane konfigürasyon yapacağım, bundle vs kim uğraşacak diye üşeniyorum. Sonra bir baktım 5 dakikalık işmiş. Hazır fırsatını bulmuşken de hemen öğrenmek için basit bir şey yapayım dedim. Baktım çalışıyor, dur ben bunu bir de anlatayım üzerine bu yazıyı yazıyorum. Hikaye faslımız bittiğine göre ufaktan başlayalım.

## Uygumala Oluşturma

En pratik React projesi oluşturmak için create-react-app'i çok sık kullanıyorum. Siz isterseniz webpack vs de deneyebilirsiniz. Hiç önemli değil. Projeyi oluşturmak için sırayla aşağıdaki adımları takip edebilirsiniz.

<script src="https://gist.github.com/ebrugulec/b116524a8170660fc6557fdc287f58bf.js"></script>

Dipnot: Neden uygulamanın adı 'endoplazmik-retikulum' derseniz. Bazı kelimelere aşırı sempatim var. Bu da  onlardan biri.

Uygulamayı nasıl yapacağınız sizin isteğinize kalmış. Burayı anlatmıyorum o yüzden. Ben [codepen](
https://codepen.io/pbmasigla/pen/aMRwXV)'de çok güzel bir proje görmüştüm. Burada onu kullandım.

## Eklenti Ayarlarını Yapma

Yaptığımız uygulama şuan local'de çalışıyor. Bunun extension olarak çalışması için bazı ayarlamalar yapmamız lazım.

### manifest.json Ayarları

Uygulamamızın publicklasöründe bir manifest.jsondosyamız var pwa ya da extension'ların konfigürasyonlarını yapmak için kullandığımız. Burada birkaç değişiklik yapacağız.

<script src="https://gist.github.com/ebrugulec/99985e2f45ed1005e21233cc2edf47c7.js"></script>

Buradaki ayarları açıklayacak olursam;

- version: Eklentimizin versiyonunun belirtildiği için. Her değişiklik yaptığımız yeni versiyonda burayı değiştirmemiz gerekli.

- incognito: Açılan gizli sekmelerde kendi gizli işlemlerinin çalışmasını istediğiniz için "split" seçeneğini seçin.

- icons: Eklentiyi farklı case'lere göre kullanabilmemiz için farklı icon boyunlarına ihtiyacımız var. Örneğin Chrome Web Store'da yayınlarken 128x128'i kullanıyor.

- chrome_url_overrides: Chrome da yeni sekmeyi açtığımızda varolan HTML sayfasını ovirred atmek için index.html 'i kullanıyoruz.

Diğer ayarlara [şuradan](https://developer.chrome.com/extensions/manifest) bakabilirsiniz. Bizim için bunlar yeterli.

### INLINE_RUNTIME_CHUNK=false

Create React App index.html içerisine küçük bir runtime scripti gömüyor bazı sebeplerden dolayı. Uygulamayı yayınladığımızda da şöyle bir hata alıyoruz.

**Refused to execute inline script because it violates the following Content Security Policy directive: "script-src 'self' blob: filesystem: chrome-extension-resource:"**

Hatayı gidermek için root klasörde bir .env dosyası oluşturmanız ve içerisine şu ifadeyi koymanız gerekiyor.

```javascript
INLINE_RUNTIME_CHUNK=false
```

### Başlangıç için placeholder ekleme

Uygulamamız render edilmeden önce react boş bir html sayfası döndürür. Bu da hoş olmayan kullanıcı deneyimlerine sebep olabiliyor. 

Bunu [react-snapshot](https://www.npmjs.com/package/react-snapshot) gibi bir uygulama ile çözebiliriz ama biz biraz daha basite kaçalım ve uygulama render edilmeden bir resim gösterelim kullanıcıya.

<script src="https://gist.github.com/ebrugulec/062f56797cce044c523f30be6f9a5ca8.js"></script>

laucher için ben şu resmi seçtim.

![Ne diyo]({{ site.url }}/assets/img/ne-diyo.jpeg)

Benim javascript dosyalarımın boyutu çok küçük olduğu için bu resim saliselik görünecek ve okunmayacaktır. Ama olsun ben ve siz biliyorsunuz artık bence bu yeterli 😅

### local'de Test Etme

Öncelikle;

```javascript
npm run build
```

komutunu koşuyoruz. Ardından eklentiyi test etmek içi bütün ihtiyacımız olan kodlar build klasöründe artık. Hemen **chrome://extensions/** adresini açalım.

![Extensions]({{ site.url }}/assets/img/extension.png)

Burada dikkat etmemiz gereken ilk kısım 'Developer mode' açık olmalı.

manifes.json, index.html ve iconların bulunduğu build klasörünü **'Load unpacked'** ile buraya yüklüyoruz. Açılan yeni sekmenin üzerinde başka bir eklentinin değişiklik yapmadığına emin olun lütfen.
Şimdi yeni sekmeyi açtığınızda eklentinizin çalışmış olması gerekli. \o/

Uygulamada bir şeyleri değiştirdikten sonra **npm run build** komutunu tekrardan koşun ve 'Refresh' butonuna tıklayın. Otomatikmen yenilenecektir.

İşte bu kadar artık React ile oluşturduğumuz bir chrome eklentimiz var. Bu eklentiyi Chrome Web Store'da yayınlamak için [şu dokümantasyona](https://developer.chrome.com/webstore/publish) bakabilirsiniz. O da 5 dakikalık bir iş. Benim yaptığım uygulamaya ise [şuraya](https://chrome.google.com/webstore/detail/endoplazmik-retikulum/ldamfimjkoemakilfgmhoigbcnmalnfl/related?hl=en-GB) tıklayarak ulaşabilir ve kolayca deneyebilirsiniz. Her tarayıcıda ve her tarayıcı versionunda çalışmadığını hatırlatmak isterim. [Github](https://github.com/ebrugulec/endoplazmik-retikulum)'a da ekledim kodları. Buraya kadar okuduğun için çok teşekkürler.

![Extensions]({{ site.url }}/assets/img/chrome-extension.png)

Şimdilik Hoşçakal. Sevgiler.

Kaynaklar:

[Building Chrome Ex.](https://medium.com/@gilfink/building-a-chrome-extension-using-react-c5bfe45aaf36)

[gitconnected](https://levelup.gitconnected.com/how-to-use-react-js-to-create-chrome-extension-in-5-minutes-2ddb11899815)

