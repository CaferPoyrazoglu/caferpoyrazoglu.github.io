---
layout: post
title:  "SQL SERVER Veri Tipleri Nedir ?"
subtitle: "SQL SERVER Veri Tipleri"
date:   2021-08-05 11:44:14 +0300
category: "blog"
permalink: :title/
---

# SQL SERVER VERİ TİPLERİ <br/>


## 1.) Metinsel Veri Tipleri
- **char :**  8000 karaktere kadar değer alabilirler. char(n) şeklinde kullanılır ve verilen değerden az karakter girilse dahi giriş yapılan boyut kadar yer kaplar. Unicode desteklemez.

- **nchar :** Maksimum 4000 karaktere kadar değer alabilir. Unicode destekler.

- **varchar :** Girilen değer kadar boyut kaplar. 8000 karaktere kadar depolama yapar. Unicode desteklemez. 

- **nvarchar :** Verinin boyutu kadar yer kaplar. 4000 karaktere kadar değer alabilir.

- **text :** Belirtilen değer kadar yer kaplar. Unicode desteklemez.

- **ntext :** Karakter boyutu kadar yer kaplar. Unicode destekler.

## 2.) Binary (İkilik) Veri Tipleri

- **binary :** 1 ve 0 ları temsil eden ikilik taban veri tipidir. Sabit uzunluklu veri tiplerinde kullanılırlar. 8000 byte'a kadar depolama yapabilir.

- **varbinary :** Binary tipinden farklı olarak girilen karakter kadar yer kaplar. Uzunlukların değişken olduğu durumlarda tercih edilir.

- **image :** Resim dosyalarını saklamak için kullanılır. **image** veri tipi yerine **varbinary(MAX)** önerilir.

## 3.) Sayısal Veri Tipleri
- **bit :** Bir byte uzunluğunda tam sayı veri tipidir. Evet / Hayır - 1 / 0 şeklinde mantıksal bilgileri saklamak için kullanılır.

- **int :** 4 byte büyüklüğünde, - 2 milyar / + 2 milyar arasında veri tutabilen tam sayı veri tipidir.

- **biging :** 8 byte büyüklüğünde, - 2⁶³ / + 2⁶³ arasında değer tutabilen tam sayı veri tipidir.

- **smallint :** 2 byte büyüklüğünde, - 32.768 / + 32.768 arası değer alabilen tam sayı veri tipidir.

- **tinyint :** 1 byte büyüklüğünde, 0 / 255 arası değer alabilen tam sayı veri tipidir.

- **decimal, numeric :** İkisinin de kullanımı aynıdır. - 38 / + 38 basamak arası veri depolayabilir. - 10³⁸ / + 10³⁸ arası ondalık ve tam sayı türünde veri saklayabilir.

## 4.) Parasal Veri Tipleri

- **money :** 8 byte büyüklüğünde,  - 2⁶⁴ / + 2⁶⁴ arasındaki parasal değerleri tutmak için kullanılır.

- **smallmoney :** 4 byte büyüklüğünde, - 214.000 / + 214.000 arası parasal değerleri tutmak için kullanılır.

## 5.) Tarih - Zaman Veri Tipleri

- **date :** 3 byte uzunluğunda, tarihleri ***YYYY-AA-GG*** formatında saklar.

- **smalldatetime :** 4 byte uzunluğunda, tarih ve zaman verilerini ***yıl-ay-gün-saat-dakika-saniye-salise*** şeklinde saklar. 

- **datetime :** 8 byte uzunluğunda, tarih bilgilerini ***YYYY-AA-GG*** şeklinde saklar. ***1 Ocak 1753 - 31 Aralık 9999*** arası veriler için kullanılır.

- **datetime2 :** Veri boyutu 6 - 8 byte arası değişir. ***1 Ocak 0001 - 31 Aralık 9999*** tarihleri arası verileri tutar, ayrıca ekstra olarak salise hassasiyeti daha yüksektir.

- **time :** Boyutu kullanıcı tarafından değiştirilebilen 3 - 5 byte arası yer kaplayan tiptir. Sadece saat verilerini ***saat-dakika-salise*** olarak saklar.

- **datetimeoffset :** Ülkelere göre değişen zaman farkını hesaplamak için kullanılan veri tipidir.

## 6.) Diğer Veri Türleri

- **sql-variant :** Sayı, metin, binary gibi veri tiplerini saklamak / depolamak için kullanılır.

- **xml :** XML türünde veri saklamak için kullanılır. Kapasitesi 2GB boyutundadır.

- **geometry :** Geometrik şekillerin ***en-boy-yükseklik*** verilerini saklar.

- **timestamp :** Tabloya kayıt eklendiğinde ya da güncellendiğinde binary türünde özel değer alan veri tipidir.

- **uniqueidentifier :** 16 byte uzunluğunda benzersiz GUID tipinde veri tutar.

- **hierarchyid :** Ağaç veri modeli veya hiyerarşik olarak sınıflandırmılmış verileri saklamak için kullanılır.

- **geography :** Coğrafi koordinat ve GPS verilerini tutmak için kullanılır.

> Kaynakça : [Link](https://medium.com/@berkaykosak8/sql-server-veri-tipleri-2a612ebc9f68)
