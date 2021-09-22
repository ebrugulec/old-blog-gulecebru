---
layout: post
title: React Hooks Nedir ve Nasıl Kullanılır?
date:   2019-02-12 19:07:19
modified:  2019-02-12
comments: true
img: hooks.png
---

Başlamadan önce yazının daha rahat anlaşılması için iki kısma ayırdım;

1. React Hooks hakkında teknik bilgiler ve örnekler
2. Örnek bir projeyi hooks ile tekrar yazma

React ekosistemini ciddi anlamda çok seviyorum. Birçok problemi çözmesinin yanında mükemmel değil elbette. Biraz React ile uğraştıysanız componentların bir ağaç gibi tek kökten birbirine geçtiğini ve onlarca/yüzlerce component’ı birbirini sarmaladığını farkedersiniz. Buna kısaca “wrapper hell” deniliyor. Component’ları tekrar kullanabilmek içim React higher order component gibi çözümler sunuyor ancak neticede kodun takip edilmesini ciddi anlamda hantallaşıyor. Hooks ise component hiyerarşisini değiştirmeden stateful logic’i tekrar tekrar kullanabilmemize olanak veriyor. React’ın bir diğer problemi ise life-cycle metodlarının side effectleri. React de api çağrılarını ve listenerları componentDidMount() ve componentDidUpdate fonksiyonlarını kullanarak gerçekleştiriebiliyoruz. Bu da birbiriyle alakasız işlemlerin bir yerde toplanmasına sebep oluyor ve kodun kalitesini ciddi oranda azaltıyor. Genelde bu tarz stateful componentları bölmek problemli olduğu için Redux gibi third party kütüphaneler kullanılıyor. Redux’da ise abstraction olayında çığır açan Redux kodlarını anlamak için birbirinde farklı dosyaların içerisinde kayboluyoruz. Hooks bu ve bunun gibi problemleri çözmek için varolan component’ı yaptığı işe göre küçük fonksiyonlara bölebiliyor. Ve artık class yazmadan react’ın özelliklerini kullanabiliyoruz.

React’ ilk öğrendiğimde sanki bütün işi “this” keyword’ü yapıyor da diğer fonksiyonlar eşantiyon gibi duyurdur. Her yerdeydi “this” Ve çok sinir bozucuydu. Hooks ile artık state’i tanımlarken, güncellerken ya da fonksiyonu çağırırken this kullanmamıza gerek kalmıyor.

<script src="https://gist.github.com/ebrugulec/b69a86751a61a9691b2a9814a1e22c79.js"></script>

Hemen küçük bir örnek ile hooks’un nasıl çalıştığına bakabiliriz.

<script src="https://gist.github.com/ebrugulec/bd16cd625bc665853622058cbbfc7d02.js"></script>

Burada class yapısı kullanarak basit bir counter yaptık. Şimdi aynı işlemleri bir de hooks kullanarak yapalım.

<script src="https://gist.github.com/ebrugulec/d4b26a58ae2d3b14bb8f191ae4fb8c8e.js"></script>

Ta daa artık hooks’u kolayca fonksiyonlar ile birlikte kullanmış olduk. ‘useState’ fonksiyonu arguman olarak inital state kabul edebilir ve array destructing kullanarak iki değişken döndürür. İlk değişken kullanabileceğimiz state, ikinci değişken ise state’i güncellememiz için kullanacağımız fonksiyonu temsil eder.

**Effect Hook**

<script src="https://gist.github.com/ebrugulec/3713bbe0800d8116c1e8ade857c4c4e3.js"></script>

Burada class yapısında api çağrıları ve SyntheticEvent’lar için componenDidMount() ve componentWillUnmount’u kullandık. Ancak hooks yapısında aynı listener’ı useEffect kullanarak tanımlayabilir ve kaldırabiliriz.

useEffect her renderda tekrardan çağırılır. Daha kolay anlaşılabilmesi için useEffect’i 3 farklı işlemi aynı anda yapan tek bir fonksiyon olarak düşünebiliriz.

Eğer useEffect’in her renderda çalışmasına müdahale etmek istiyorsak ikinci argüman olarak array verebiliriz.

<script src="https://gist.github.com/ebrugulec/e4ffeca95406a9b7cd951145cd5d0161.js"></script>

useEffect’i component initial render ettiğinde çalışacak. Eğer value değişirse tekrardan useEffect çalışacak. Güncellenmezse çalışmayacak.

Yanlızca useEffect’i component yüklendiğinde(mount) ve işi bittiğinde(unmount) çalıştırmak için boş bir array göndermemiz yeterli.

<script src="https://gist.github.com/ebrugulec/c0f72e1bd349180fb94a2616f78384b4.js"></script>

Bazen component yüklendiğinde küçük bir değişiklik yapmak isteyebiliriz.

<script src="https://gist.github.com/ebrugulec/aa173daf7d2519579323dcebd1832ebe.js"></script>

Bu örnekte olduğu gibi useRef kullanarak yanlızca component yüklendiğinde bir defaya mahsus olarak useEffect çalışacaktır.Ve inputRef güncellenmediği için tekrar çalışmaz.

useEffect’i componentDidMount gibi lifecyle metodları yerine kullanıyoruz demiştik. Ve bu metodlar genelde Fetch Data yaptığımız fonksiyonlardır. Aynı şekilde useEffect’i de Fetch Data işlemlerini yapmak için kullanabiliriz.

<script src="https://gist.github.com/ebrugulec/b8309eb3c6d7c6203de87598dc35a350.js"></script>

Yukarıdaki component da api çağrısı yaptıktan sonra posts’ları güncelliyoruz. Ancak burada bir prolem var. useEffect her render’a aynı çağrıyı tekrar yapacaktır. Bunu önlemek için ikinci argüman olarak setPosts göndermemiz gerekir. Böylece yanlızca post güncellendiğinde useEffect tekrar çalışacak.

**Effect Hook ile Re-fetch**

<script src="https://gist.github.com/ebrugulec/4da3e58d7525c5ad8f4b04f8793f7b08.js"></script>

Burada inputValue ve subreddit’i tutan iki state’imiz var. input element’e girilen veri değiştiğinde Reddit component’ında birkaç değişiklik olması ve api çağrısını yinelemesi gerekli.

<script src="https://gist.github.com/ebrugulec/ccb1faf491978469fb1a5c792a491884.js"></script>

Bu component da useEffect de dikkat edilmesi gereken nokta subreddit ve setPosts güncellendiğinde ancak useEffect çalışabilecek.

⚠️ Şuan hata yakalamak için herhangi bir işlem yapmadık ama eğer fetch ve re-fetch işlemleri başarısız olursa onun için ayrıca bir işlem yapmamız gerektiğini unutmayalım.

**2) Örnek Proje**

Teorik kısımdan bahsettiğimize göre artık Hooks’u aktif bir projede kullanıp öğrendiklerimizi pekiştirelim. Projede kısaca bahsedecek olursam; verilen props değerlerine göre parola oluşturuyoruz ve bu parolanın güçlü olup olmadığını kontrol ediyoruz. Deploy edilmiş haline [şuradan](http://ebrugulec.com/react-password-generator/) ulaşabilirsiniz. İlk etapta fonksiyonları hooks kullanmadan yazmıştım. Component’ın o haline [şuradan](https://github.com/ebrugulec/react-password-generator/blob/c9f6e2d15040aefcb527baa3021e50019b731452/src/components/PasswordGenerator.js) ulaşabilirsiniz. Şimdi aynı işlemleri hooks kullanarak tekrardan yazalım.

**npm install react@16.8.1**
**npm install react-dom@16.8.1**

Hooks’u kullanmak için önce import etmeyi unutmuyoruz.

```javascript
import React, { useState, useEffect } from ‘react’;
```

<script src="https://gist.github.com/ebrugulec/8c87b440cffec2191ee6962881b6697d.js"></script>

Başlamamız gereken ilk yer state tabiki. Şimdi state’imizi useState kullanarak ve değerlerini props’dan alacak şekilde takrardan yazalım.

<script src="https://gist.github.com/ebrugulec/fb69b9c04fb5e3d88c0897fbeebdbbfb.js"></script>

---

Uygulamalarımızın belkide bel kemiği olan component yüklendiğinde ve kaldırıldığında çeşitli işlemler yaptığımız, componentDidMount() ve componentWillUnmount()’i Effect Hook kullanarak tekrar yazalım. Ama önce ne yaptıklarını bir anlatayım. componentDidMount()’da componentımız yüklendiğinde bir password üretecek ve checkPasswordStrength() fonksiyonu da bu password’un sağlamlığını kontrol edecek. Click eventListener’ı ise password’u kopyalamak için kullandığımız bir fonksiyon.

<script src="https://gist.github.com/ebrugulec/3221e0d69ed1b076925c1345344e2f06.js"></script>

useEffect kullanılarak oluşturulmuş hali;

<script src="https://gist.github.com/ebrugulec/a3896497305ebb77babd5ecc028c60bc.js"></script>

İlk useEffect password’ı kopyalamak için kullandığım bir listener. useEffect sonundaki köşeli parantezin anlamından bahsetmiştim. Yanlızca component yüklendiğinde ve kaldırıldığında çalışıyor.

İkinci useEffect is length, digitCount ve symbolCount değerlerinin değişimine bağlı olarak çalışacak.

---

<script src="https://gist.github.com/ebrugulec/08762151ebf64ea423f467459c2e203a.js"></script>

generatePassword fonksiyonunda bizi ilgilendiren kısmını kullandım yanlızca. O da state’i güncelledikten sonra checkPasswordStrength() fonksiyonunu çağırması. Burada bir virgül koyarak bilmeyenler için ufak bir anekdot paylaşmak istiyorum. Biz this.setState dediğimizde state’i güncellemeden diğer satıra geçeceği için doğru state ile bir sonraki fonksiyonun çalışıp çalışmadığından emin olamayız. Çünkü senkronize bir şekilde çalışmıyor. Bu yüzden bir callback oluşturdum. Bazen de güncellediğimiz state’i hemen bir sonraki satırda kullanmak istiyoruz. Böyle durumlarda yine bir callback oluşturup yeni state’i return edebiliriz. this.setState’in ilk parametresi previous state’i ikinci parametresi props değerini tutuyor.

<script src="https://gist.github.com/ebrugulec/7c4df1836d02d9461f5a863cebf67f9b.js"></script>

password değiştikten sonra strength’i kontrol etme işini useEffect’e devrettik.

---

<script src="https://gist.github.com/ebrugulec/e98febeec4f6c4ffe7fcfed6353e444d.js"></script>

checkPasswordStrength ve copyToClipboard fonksiyonunda bizi ilgilendiren yanlızca setState kısmı.

<script src="https://gist.github.com/ebrugulec/45d86b9082647706dc7c675b0ea94d9b.js"></script>

Her range input elementinin onChange özelliğini handleOnChange fonksiyonunda elementine bağladık. Böylece bu elementler değiştiği takdirde biz de state’i güncelleyeceğiz.

<script src="https://gist.github.com/ebrugulec/4f59a1974f71b88eeda54d0a18f69dd7.js"></script>

<script src="https://gist.github.com/ebrugulec/30a162d65f30beb1714201ea262c5222.js"></script>

Ve böylece component’ımızın tamamını hooks ile baştan yazmış olduk. prop-types ve defaultProps’u da yazmayı unutmayın lütfen. Projenin github reposu [şurada](https://github.com/ebrugulec/react-password-generator).

<script src="https://gist.github.com/ebrugulec/fde0e24d686901945767de73282a9fe8.js"></script>

⚠️ Yazıda herhangi bir hata bulursanız ya da eksik olduğunu düşündüğünüz bir yer olursa lütfen bana bildirin. Bu hem benim yanlış öğrendiğim bir şeyi düzeltir hem de bu yazıyı okuyanları yanlış bilgilendirmiş olmam. Şimdiden teşekkürler.

⚠️ Bu ve diğer yazılarımı [medium](https://medium.com/@ebrugulec/) üzerinden de okuyabilirsiniz.

Sevgiler

