---
layout: post
title:  "SOLID Prensipleri Tanıyalım"
subtitle: "Nedir bu SOLID prensipleri ?"
date:   2021-07-19 13:23:21 +0300
category: "blog"
permalink: :title/
---

Öncelike yazıya **SOLID** yazılımın ne olduğunu açıklamakla başlayayım.

**SOLID** belirli prensiplerden oluşan, ***OOP*** nesne tabanlı yazılım geliştirilirken kullanılan, kod tekrarını önleyen, sürdürülebilir ve anlaşılabilir olmasını sağlayan **Robert C. Martin** tarafında öne sürülen prensiplere diyoruz.

# Peki nedir bu prensipler ?

- **S** - Single Responsibility Principle (SRP)
- **O** - Open Closes Principle (OGP)
- **L** -  Liskov Substitution Principle (LSP)
- **I** - Interface Segregation Principle (ISP)
- **D** - Dependency Injection Principle (DIP)

*Prensiplerin baş harfleri gördüğümüz üzere **SOLID** kelimesinin oluşumunu sağlıyor.*

## Single Responsibility Principle (SRP)

Bir sınıf oluşturduğumuz zaman o sınıf üzerine belirli görevler ve özellikler yükleriz. Bu sınıfa projede kullanılacak tüm özellikler, fonksiyonlar eklenirse bir süre sonra karman çorman bir hâle gelecektir. Hatta program sınıfı açarken bile bazen kasacaktır. Bu prensibe göre her sınıfın kendine has özelliği olmalıdır. Tüm projeyi tek başına üstlenmemeli. Kısa ve öz şekilde sınıfın amacı belirlenmeli, fonksiyonlar ve özellikler sınıf amacına uygun hazırlanmalı.

## Open Closed Principle (OGP)

Daha önceden hazırlanmış sınıfın özellikleri korunmalı ve değişime izin verilmemelidir. Sınıf amacını değiştirmiyor olmalı ama gelişime her zaman açık olmalı. 

## Liskov Subtition Principle (LSP)

Bir sınıfta bulunun özellike ve fonksiyonlar, kendisinden kalıtım alan sınıflarda kullanılmayacaksa **SOLID** prensiplerine aykırı bir durum açığa çıkar. Peki nasıl ? Kalıtım alınan sınıfın içindeki özellikler kalıtımı alan sınıfa uygun bir şekilde kullanılmalıdır. Yürüyenler adındaki bir sınıfa insan adındaki bir sınıfı kalıtım alabilirsiniz fakat yılanı alamazsınız. Böyle bir durumda yılan için sürünenler adında bir sınıf oluşturmanız gerekecektir.

## Interface Segregation Principle (ISP)

Sınıflarda da olduğu gibi bu prensipte de görevlerin hepsini tek bir arayüzde (interface) toplamak yerine, kısa ve öz olan, amaçlarına uygun özelleştirilmiş birden fazla arayüz (interface) oluşturmalıyız. Bu sayede karmaşıklığın önüne geçmiş oluruz.

## Dependency Inversion Principle (DIP)

***Prensibe göre;***

- Üst seviye sınıflar alt seviye sınıflara bağlı olmamalıdır. İlişki abstraction veya interface kullanarak sağlanmalıdır.
- Abstraction detaylara bağlı olmamalıdır, tam tersi detaylar abstraction'lara bağlı olmalıdır.

Kısacası geliştirilen uygulamalarda üst seviyeli işlem yapan bir metotta yapılacak en ufak değişiklik, bağlı olduğu tüm metodların değişmesini gerektirecektir. Sınıflar arası bağımlılıkar olabildiğince az olmalıdır. Özellikle üst seviye sınıflar alt seviye sınıflara bağımlı olmamalıdır. Nesne temelli bir dil kullanıyor olabilirsiniz fakat yazılan kodların her zaman nesne temelli olacağı anlamına gelmiyor. 

**SOLID** prensiplerini en basit şekilde anlatmaya çalıştım. Umarım faydalı olmuştur :)

Bir sonraki postta görüşmek üzere,
Kendinize iyi bakın,
Öğrenmeyi bırakmayın,
Hoşça kalın... :))
