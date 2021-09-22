---
layout: post
title: React Uygulamalarını Lazy-loading ve Suspense Kullanarak İyileştirme
date:   2019-02-21 10:56:19
modified:  2019-02-21
comments: true
img: lazy-suspense.jpg
---

Geliştirdiğimiz uygulamaların performansları uygulama büyüdükçe daha çok önem kazanır. Son kullanıcının düşük internet bağlantısında bile yüksek kalitede ulaşılabilirlik sağlamalıyız. Son kullanıcı uygulamayı açtığında içeriğin çoğu kullanıcı tarafından etkileşime girmeyebilir veya görünmez. Kaynakların verimli kullanılması ve kullanıcının problem yaşamaması için uygulamamızın bazı kısımlarını yüklenme sırasında erteleyebiliriz.
Server Side Rendering açılış hızı için daha fazla performans ve tutarlı SEO problemlerini çözer. SSR'de(Php ya da Rails kullandığımızı düşünelim)her link farklı bir HTML sayfasını işaret eder ve onu yükler. Örneğin;

```bash
our-app/
 - index.html
 - about.html
 - products.html
 - settings.html
```

Ancak genellikle Javascript frameworklerinde import edilen her modül tek bir javascript dosyası haline getirilir(bundle) ardından DOM aracılığı ile web sayfasında görüntülenir. Örneğin React de farklı componentlarımız olsun ve bunu görüntülemek isteyelim;

<script src="https://gist.github.com/ebrugulec/232e73923662af068cc0c023537a0800.js"></script>

Bütün dosyalarımız bunde edildikten sonra uygulamamız;

```bash
our-react-app/
 dist/
  - index.html
  - index.js
```

about, products ve settings componentlarımız tek bir dosya haline getirildi.(index.js) Şimdi eğer index.html dosyamızı yüklemeye çalıştığımızda index.js dosyamızın içerisindeki componentlarımız da yüklenecektir. Örneğin her bir dosyanın ayrı ayrı yüklenme süresi şu şekilde olsun;

- index.js 4ms
- about.js 10ms
- products.js 35ms
- settings.js 10ms

Bundle edilmiş dosyamızın yüklenme hızı da şu şekilde olacaktır;

- index.js 59ms

Bu durumda sayfamızın açılma işlemi tam olarak 59ms sürecektir. Ancak React ile dosyaları ayırabilir ve istek üzerine yükleyebiliriz. Böylece tüm kodu indirmek yerine küçük parçalara bölebilir ve çalışma zamanında dinamik olarak yükleyebiliriz. Reactde code-split işlemi için birden farklı metod benimsenebilir. Bunların bazılarından aşağıda bahsedeceğim.

# 🔻 Dynamic import proposal

Dinamik import işlemi ES Module'lerinin yeni özelliği olup, kod bağımlılıklarını asenkron bir şekilde küçük parçalara bölüp talep üzerine yüklememize müsaade eder. Bunu kullanmak için kodumuzda birkaç küçük değişiklik yapmamız yeterlidir.

<script src="https://gist.github.com/ebrugulec/0c8bcb3cf50bf9643d9daa24e78aff78.js"></script>

Yaptığımız işlemi şöyle ifade edecek olursak. Uygulamamıza Description'a hemen ihtiyaç duymuyorum, ihtiyacım olduğu bir zamanda yüklenmesi için bekleyebiliriz demiş oluyoruz. Burada bundler yaptığımız lazly loaded işlemi için ayrı bir javascript dosyası oluşturacak. Buna da kısaca code-splitting diyoruz.

# 😴Lazy Loading React Components

Yukarıda çözümlenmeyi bekleyen bir promise oluşturduk. Bu işlem bittiğinde artık component'ımızı kullanabiliriz. Ancak bu lazy-loading yöntemi biraz hataya açık. Şimdi onu tekrardan düzenleyelim.

<script src="https://gist.github.com/ebrugulec/82889d3fc9750c49931dce8e4c3920a7.js"></script>

Bu işlemi e paket kullanarak (react-loadable) daha pratik bir şekide gerçekleştirebiliriz.

<script src="https://gist.github.com/ebrugulec/fe00bdb697390565184d6e6c8f0c1f92.js"></script>

# 💤React.lazy()

React.lazy() ile dinamik import kullanılarak kolayca component oluşturma ve render etme işlemlerimizi gerçekleştirebiliriz.

<script src="https://gist.github.com/ebrugulec/20fd399ebbf89c3636fafba15ff90649.js"></script>

React.lazy bir call back çağırır ve import ile bir promise işlemini gerçekleştirir. Promise modülün başarılı bir şekilde yüklenip yüklenmediğini ya da bir hata varsa (dosya bulunamadı, path yanlış gibi) bunu çözer.
Kodumuz compile işleminden sonra farklı bundle dosyaları oluşacatır.

```bash
our-react-app
 dist/
  - index.html
  - main.b1234.js (MyComponent ve diğer kodları içerir)
  - othercomponent.bc4567.js (OtherComponent'ı içerir)
```

```html
/** index.html **/
<head>
    <div id="root"></div>
    <script src="main.b1234.js"></script>
</head>
```

Şimdi uygulamamızı çoklu bundle'lara ayırmış olduk. MyComponent render olduğunda, othercomponent.bc4567.js dosyası da yüklenir ve DOM'da bulunan MyComponent görüntülenir.

# 🚦React Suspense

Şimdi otherComponent.bc4567 dosyası yüklendiğinde bir gecikme gerçekleşecek. Görünüşe göre, uygulamamız bir süre donacak. Bu kötü bir kullanıcı deneyimi olur elbette. Kullanıcıya bir şeyin olduğunu veya yüklendiğini bildirmemiz gerekir. Bunu yapmak için React.lazy ile ilişkilendirilmiş yeni bir özellik eklendi. O da Suspense. 

React.lazy() ile component'ımız yüklenirken Suspense ile kullanıcıya bilgilendirmek için çeşitli fallback'ler gösterebiliriz. Suspense lazy component'ı sarmalamalıdır.

<script src="https://gist.github.com/ebrugulec/6462ad28821412e644ab700f5e8a3c1a.js"></script>

İstersek fallback'e props da gönderebiliriz.

<script src="https://gist.github.com/ebrugulec/d938048558ed7eff949c29a47439d30c.js"></script>

Birden fazla lazy component'ı tek bir suspense component'ı içerisine koyabiliriz.

<script src="https://gist.github.com/ebrugulec/029d67055abb1aa0125f6304346c6d27.js"></script>

İç içe suspense tanımlayabiliriz.

<script src="https://gist.github.com/ebrugulec/f18d0b4b860a9d1cb0d16813a1639ad0.js"></script>

Burada <Description /> component'ı yüklendikten sonra, "Sory for our laziness" yazacak. Çünkü AnotherLazyComponent'ın boyutu çok fazla olduğu için o en son yüklenecek.

# 👮 Yüklenme Hatalarını Yakalama

import() fonksiyonunun bir promise döndürdüğünden söz etmiştik. Bazı sebeplerden dolayı(network failure,file not found, file path error) componentlarımız yüklenmezse eğer, bunu kullanıcıya bildirmemiz gerekli. (Catching Loading Errors) Bunu da harici bir component kullanarak yapabiliriz.

<script src="https://gist.github.com/ebrugulec/e879bbcf538d204b214c3e402802185f.js"></script>

# 🌳 Lazy-loading routes

React.lazy () ve Suspense ile route tabanlı code-splitting işlemini herhangi bir harici paket kullanmadan gerçekleştirebiliriz. Route componentlarınımızı kolayca lazy componentlar şeklinde parçalamak için bütün route'u suspense ile sarmalamamız gerekli.

<script src="https://gist.github.com/ebrugulec/c9b82d658835ae1ffe4bb090de736e11.js"></script>

⚠️ Yazıda herhangi bir hata bulursanız ya da eksik olduğunu düşündüğünüz bir yer olursa lütfen bana bildirin. Bu hem benim yanlış öğrendiğim bir şeyi düzeltir hem de bu yazıyı okuyanları yanlış bilgilendirmiş olmam. Şimdiden teşekkürler.

⚠️ Bu ve diğer yazılarımı [medium](https://medium.com/@ebrugulec) üzerinden de okuyabilirsiniz.





