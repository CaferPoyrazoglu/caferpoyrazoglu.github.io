---
layout: post
title:  "Jekyll ile Blog Kurmak"
subtitle: "En basit ve hızlı yoldan blog sayfanızı açacaksınız"
date:   2020-11-11 19:31:14 +0300
category: "blog"
permalink: :title/
---

>Merhabalar bugün size Jekyll ile nasıl hızlıca blog kuracağınızı göstermeye çalışacağım.

<img style="width: 100%" src="https://miro.medium.com/proxy/0*N8RG95bKJnnF-wpL.png">

# Jekyll Nedir
_**Jekyll**_, basit, blog odaklı, statik bir site oluşturucusudur. **_Ruby_** _dilinde kodlanmıştır. Çalışma mantığı kısaca şöyledir: Siz bir yazı/sayfa hazırlarsınız Jekyll hazırladığınız yazıyı/sayfayı _**_static html_**_ sayfasına dönüştürür. Ayrıca eğer sitenizi Jekyll kullanarak oluşturmak isterseniz_ **_GitHub Pages_** _üzerinde ücretsiz host edebilirsiniz._
<br><br>
>Öncelikle bilgisayarınızda <a href="(https://rubyinstaller.org/downloads/)">Ruby</a> kurulu olması gerekmekte bunun için de yukarıdaki Ruby linkine tıklayarak bilgisayarınız için uygun Ruby'i 
kurun.
<br><br>
Daha sonra terminale <code>ruby -v</code> yazın ve çalıştırın. Karşınıza kurduğunuz Ruby sürümü bilgisi gelecek.
<br><br>
Evet Ruby kurulumunu yaptıysak artık _**Jekyll**_ ile sitemizi yapmaya başlayabiliriz.
<br><br>
Konsol ekranımıza geliyoruz ve <code>gem install jekyll bundler</code> yazalım ve çalıştıralım. Bu şekilde **_Jekyll_**  paketini yüklemiş olduk.
<br><br>
Şimdi site dosyalarımızı kurmaya başlayalım <code>jekyll new myBlog</code> bu kodu yazalım ve çalıştıralım.
<br><br>
_**_myBlog_** site dosyalarını tutan bir klasördür. _**Jekyll**_ bunu çok kısa bir sürede hazırlar ve size sunar._
<br><br>
Site dosyalarımız hazırlandı blog sayfamıza göz atmak için klasöre gitmemiz gerek <code>cd myBlog</code> yazın ve çalıştırın. Şu an da site dosyalarının bulunduğu klasöre giriş yaptık.
<br><br>
_Burada önemli olan nokta site dosyalarınızın nerede olduğudur. Klasör dizinine dikkat etmelisiniz!!_
<br><br>
Evet <code>cd myBlog</code> yazdık, çalıştırdık ve klasör dizinine girdik. <code>bundle exec jekyll serve</code> veya <code>jekyll serve</code> kodlarını yazalım ve çalıştıralım. 
<br><br>
<pre>
<code>C:\Users\Hamit\Desktop\myBlog>jekyll serve
Resolving dependencies...
Configuration file: C:/Users/Hamit/Desktop/myBlog/_config.yml
            Source: C:/Users/Hamit/Desktop/myBlog
       Destination: C:/Users/Hamit/Desktop/myBlog/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
       Jekyll Feed: Generating feed for posts
                    done in 9.077 seconds.
 Auto-regeneration: enabled for 'C:/Users/Hamit/Desktop/myBlog'
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.</code>
</pre>
<br><br>
Karşınızda yukarıdaki yazılanları görüyorsanız tebrikler ilk blog sitenizi kurdunuz.
<br><br>
Herhangi bir tarayıcıya girin ve url kısmına http://localhost:4000 yazın.
<br><br>
<img style="width: 100%" src="https://i.hizliresim.com/5Dzmyo.png">
<br><br>
Tarayıcı ekranınızda yukarıdaki site ile karşılaşacaksınız. Özelleştirmek için klasör dizinine gidin ve oradan _config.yml dosyasını bulup açın. Size kod görüntüleme ve yazmanız için <a href="https://www.sublimetext.com/3">Sublime Text</a> programını öneririm. 
<br><br>
_config.yml dosyasından değiştirebileceğiniz ekleri göreceksiniz. Örneğin 'title' için kendi isminizi yazabilirsiniz 'email' için kendi emailinizi yazabilirsiniz. 
<br><br>
Post ekleme işine de gelirsek _**Jekyll**_ markdown formatlı dosyaları okur ve size sunar. Yazı eklemek isterseniz de <a href="https://stackedit.io/">StackEdit</a> öneriyorum. Siteye giriş yapın ve yazılarınızı yazmaya başlayın. Yazımızı kaydetme işleminde ise <code>Yıl-Ay-Gün-(post-ismi).markdown</code> olarak kaydedin. Örneğin <code>2020-04-20-Jekyll-Blog.markdown</code>. Kaydedin ve Post klasörünüze atın bu kadar.
<br><br>
Kendinize dikkat edin,
<br><br>
Hoşça kalın.
<br><br>