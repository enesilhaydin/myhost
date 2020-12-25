---
title: Adli Bilişim Alanında Exif ve Steghide Araçlarının Kullanımı
date: 2020-12-25 19:43:01
category: forensic
thumbnail: 'logitech.png'
draft: false
---


## **Adli Bilişim Alanında Exif ve Steghide Araçlarının Kullanımı**

![](https://cdn-images-1.medium.com/max/2000/1*n9UuKpBkhryF4K8i96subg.png)

## **1.GİRİŞ**

EXIF, “*Japan Electronic Industries Development Association”* (**JEIDA**) tarafından 1998 yılında tasarlanarak çıkarılmıştır. O kadar büyük bir buluştur ki fotoğrafların nerde ve nasıl çekildiğinden tutunda, resimlere koyacağınız etiketlere varıncaya kadar bünyesinde barındırabilmektedir.

Dijital makineyle çekilmiş olan fotoğraf sadece görüntü (imaj) dosyasından ibaret değildir. Fotoğrafın hangi makine, hangi lensle, ne zaman, hangi diyafram ve enstantane hızıyla çekildiği gibi daha onlarca değeri bünyesinde barındıran bir dosyayı da bu görüntü dosyasının içinde barındırır. Bu dosyaya ***“exchangeable image file”*** değiştirilebilir resim formatı **EXIF** denilmektedir. **EXIF **çekilen fotoğrafı okumamızı sağlarken aynı zamanda bize çekime ilişkin ipuçları ve detayları da verebilir. Bunları ise çeşitli yerlerde kullanabiliriz. Hatta geçtiğimiz yıllarda büyük sitelerden birine atılan index üzerinde, hacker kız arkadaşının fotoğrafını koymuştu. Daha sonra fotoğrafın detaylı incelenip lokasyon bilgilerine ulaşılması sonucu hacker yakalanmıştı. Bu yüzden ulaşılabildiği kadar detaya ulaşıp iyi inceleme yapıldığında sonuca varmak o kadar kolay olacaktır.

Steganografi: Eski Yunanca’da “gizlenmiş yazı” anlamına gelir. Amacı var olan bilgi gizlemektir.

## **2.EXIFTOOL**

Çeşitli dosya türlerinde meta bilgileri okumak ve yazmak için kullanılan komut satırı üzerinde çalışan adli bilişimi yazılımıdır. Exiftool perl dili üzerinde geliştirilmiş bir araçtır. Backtrack ve Kali ile birlikte kurulu gelmektedir. Bunun dışında Windows ve Mac içinde sürümleri bulunmaktadır. Meta verileri yazmak veya silmek için etiket değerleri, -TAG = [DEĞER] sözdizimi kullanılarak atanır. Meta verileri kopyalamak veya taşımak için -tagsFromFile özelliği kullanılır. Varsayılan olarak, orijinal dosyalar adlarına _original eklenerek korunur; orijinalleri silmeden önce yeni dosyaların doğruluğundan emin olun. Ek olarak Exiftool aracının Windows için GUI arayüzüde bulunur. Linux tarafında kurulum için apt-get install exiftool yazılarak kurulabilir. Windows tarafında ise [https://www.sno.phy.queensu.ca/~phil/exiftool/](https://www.sno.phy.queensu.ca/~phil/exiftool/) adresinden indirilerek komut satırı üzerinden çalıştırabilir.

![](https://cdn-images-1.medium.com/max/2020/1*1fvkSNHsTJyJdul3Bbq_Fg.png)

### **2.1. Exiftool Kullanımı :**

**exiftool** [*AYARLAR*] [-*ETİKET*…] [ — *ETİKET*…] *DOSYA*…

### **2.2.Etiketler ve Kullanımları**

    -TAG veya --TAG                      Özelleştirilmiş etiketleri gösterir.

    -TAG[+-]=[DEĞER]                  Etiket için yeni değer atar.

    -tagsFromFile SRCFILE            SRCFILE’nin etiket değerlerini kopyalar.

    -x TAG      (-exclude)                 Etiketlerin listeleri gösterilirken belirtilen etiketi listeden çıkarır.

### **2.3.İşlem Kontrolleri**

    -a          (-duplicates)                 Yinelenen etiketlerin çıkarılmasına izin verir

    -ee         (-extractEmbedded)    Gömülü dosyalardaki bilgiyi çıkarır.

    -ext[+] EXT (-extension)          Yalnızca belirtilen uzantılarda işlem yapar.

    -fast[NUM]                               Yavaş cihazlarda hız artırımı sağlar.

    -fileOrder [-]TAG                     Dosyada işlem sırası ayarlar.

    -i DIR      (-ignore)                   Özel klasörü yok sayar.

    -if EXPR                                   Koşullara bağlı olarak dosyaların işlenmesi    için.

    -m       (-ignoreMinorErrors)    Küçük hataları ve uyarıları yok saymak için.

    -o OUTFILE  (-out)                  Çıkan dosyayı veya konumu belirlemek için.

    -P          (-preserve)                  Dosya değişiklik tarihini koru.

    -password PASSWD              Korumalı dosyalar için parola girilmesi.

    -q          (-quiet)                       İşlemleri sessiz modda yapmayı sağlar.

    -r[.]       (-recurse)                    Klasörün alt klasörlerinde de arama yapar.

    -scanForXMP                          XMP aramasında Brute Force kullanır.

    -u          (-unknown)                Bilinmeyen etiketleri çıkarır.

    -wm MODE    (-writeMode)   Etiketlerde yazma/oluşturma yapmak için modu aktive eder.

    -z          (-zip)                         Sıkıştırılmış verilerde Okuma/Yazma

### **2.4.Diğer Ayarlar**

    -k          (-pause)             İşlemleri durdurmayı sağlar.

    -ver                                 Exiftool sürüm numarasını verir.

### **2.5.Araçlar**

    -delete_original[!]              Orijinal yedeği siler.

    -restore_original                 Resmin orijinalini değişmiş olan medyanın yerine getir.

### **2.6.Gelişmiş Ayarlar**

    -common_args                     Ortak argümanları tanımlamak.

    -config CFGFILE                Özel yapılandırma dosyası.

    -execute[NUM]                    Bir satırda birden fazla komut çalıştırmak.

    -srcfile FMT                         Farklı bir kaynak dosyası tanımlar.

### 2.7.Özel Örnek ve Kullanımlar

* **TAG kullanımı**

- Exif verileri listelenirken sadece belirtilen etiketleri gösterir.

-Örnek : exiftool –comment resim.jpg

- **-all **etiketi bütün exif verileri göstermeye yarar.

-Örnek : exiftool –all resim.jpg

**-TAG+-=[VALUE] kullanımı**

**- **Exif verilerindeki belirtilen etiketi değere eşitlemeye yarar. + - ise sayılarda, tarihlerde, saatlerde, gps verilerinde artırma ve azaltma, anahtar kelime işlemleri için kullanılır.

* Örnek: exiftool “-keyword+=deneme” resim.jpg

### ***2.8.Adli Bilişim Alanında Kullanabilecek Kullanışlı Etiketler ( Buradaki bilgilerin tamamı değiştirilebilmektedir. )***

- keyword : Exif verileri için belirtilen kelime listesini ekler.

- comment : Exif verileri için açıklama yazılmasına yarar.

- Photoshop : Photoshop meta verilerinde işlem yapılması için kullanılır

- ThumbnailImage : Tırnak resimde işlem yapmak için kullanılır.

- FileCreateDate : Windows tarafında gözüken dosyanın oluşturma tarihidir.

- FileModifyDate : Windows tarafında gözüken dosyanın düzenlenme tarihidir.

- AllDates : Bütün tarihleri ifade eder.

- DateTimeOriginal : EXIF tarafındaki tarihi ifade eder.

- CreateTime : EXIF tarafında oluşturma zamanıdır.

- ModifyTime : EXIF tarafındaki düzenleme zamanıdır.

- GPSDateStamp : GPS verileri arasında gelen Tarih Damgasıdır.

- GPSTimeStamp : GPS verileri arasında gelen Zaman Damgasıdır.

- GPSLongitude : GPS verileri arasında gelen boylam bilgisidir.

- GPSLatitude : GPS verileri arasında gelen enlem bilgisidir.

- GPSPosition : GPS verileri arasında gelen Enlem ve Boylam bilgileri ile koordinatı daha düzenli gösteren bilgidir.

- GPSAltitude : GPS verileri arasında gelen rakım bilgisidir.

- Flash : Fotoğraf çekimi sırasında Flaş kullanıp, kullanılmadığının bilgisidir.

- Make : Fotoğraf’ı çeken makinanın marka bilgisini içerir.

- Model : Fotoğraf’ı çeken makinanın model bilgisini içerir.

- ShutterSpeed : Fotoğraf çekimindeki deklanşör hızı bilgisini içerir.

* xpcomment : Windows tarafındaki “Açıklama” bilgisidir.

***2.9.Faydalı Örnekler***

- exiftool –all -x comment resim.jpg : Bütün etiketleri göster ama comment’i dışla yani onu gösterme.( exclude tag)

- exiftool –comment= resim.jpg : Resim jpg içinde comment etiketini temizler

- exiftool –all= resim.jpg : Resim.jpg içindeki bütün exif verilerini temizler.

- Exiftool –all= comment=”deneme” resim.jpg : Resim jpg içindeki bütün exif verilerini kaldırır ve açıklama bölümünü deneme değerini ekler.

- exiftool “GPSDateStamp=1997:12:12” resim.jpg : Resim.jpg içindeki GPS Tarih Damgasını 12.12.1997 olarak değiştirir. NOT:Etiketin çift tırnak içinde kullanılmasının nedeni tarih ile bir bütün kullanmak içindir.

- exiftool “GPSLongitude=1:1” resim.jpg : Resim.jpg içindeki GPS Boylamını “1 derece olarak ayarlar.”

- exiftool “-FileCreateDate=1997:12:12 10:10:10” : Resim.jpg’in Windows tarafında oluşturma tarihini 10:10:10 12.12.1997 şeklinde değiştirmeyi sağlar.

- exiftool “FileModifyDate=1997:12:12 10:10:10” : Resim.jpg’in Windows tarafında değiştirme tarihini 10:10:10 12.12.1997 şeklinde değiştirmeyi sağlar.

- exiftool –Make=”Samsung” : Fotoğraf çeken makinasının markasını Samsung olarak değiştirir.

- exiftool –Model=”S3 Mini” : Fotoğraf çeken makinanın model’ini S3 Mini olarak değiştir.

- exiftool -Keywords+=”deneme” -o yeniresimdosyasi.jpg resim.jpg : Resim.jpg’in keyword bölümüne deneme’i ekler. NOT: + — = sayesinde belli bir listeye ekleme ve çıkarma yapabiliyoruz.

- exiftool –tagsFromFile resim.jpg RESİM1.jpg : RESİM1.jpg deki eksik exif verilerini resim.jpg’den ve uygula işlemini gerçekleştirir.

* Örnek: exiftool “-keyword+=deneme” resim.jpg

## **3.STEGHIDE**

Steghide, ses ve görüntü dosyaları içine veri gizleyebilen, GNU/GPL lisansı altında piyasaya sürülen Linux ve Windows için C++ dilinde yazılmış bir steganografi programıdır. Gömülen verileri hem sıkıştırma hemde şifreleme özelliğe de vardır. Renk bakımından numune frekansları değiştirilmez, böylece gömülmeyi birinci mertebe istatistiksel testlere karşı dirençli hale getirir. Varsayılan şifreleme yöntemi olarak 128 Bit AES(Rijndael) kullanılır. Algoritmaların ve çalışma yöntemlerinin tam bir listesini almak için steghide –encinfo komutu kullanılır. Windows için aşağıdaki adresden kolayca indirip, komut satırı üzerinde kolayca çalıştırabilir.

- [https://sourceforge.net/projects/steghide/files/steghide/](https://sourceforge.net/projects/steghide/files/steghide/)
>  Linux içinde **apt-get install steghide** komutu ile kurulum gerçekleştirilebilir.

### **3.1.Genel Özellikleri :**

- Gömülü verilerin sıkıştırılması

- Gömülü verilerin şifrelenmesi

- Veri bütünlüğünün doğrulanması

- JPEG, BMP, WAV ve AU dosyalarını deskteler.

Kullanımı :

steghide komutlar [ argümanlar ]

**embed** : Medya dosyasının içine veri gömmek için kullanılır. Bu komut ile parola girilerek veri gizlenilebilir. Burada iki önemli terim vardır.

1) Cover File : İçine veri gizlenilmek istenilen veri dosyasıdır.

2) Embed File : Gizlenecek veri dosyasıdır.

Ø steghide embed -cf picture.jpg -ef secret.txt

bu komut ile Cover File olarak Picture.jpg ve Embed File olarak secret.txt dosyası seçilerek veri gizleme işlemi yapılacaktır. Komut çalıştırıldığından bizden bir adet parola isteyecektir. ( Varsayılan olarak AES kullanarak veriyi gizliyor.) Parola girildikten sonra işlem tamamlanır. İstebilir –p parametresi ile komut yazılırken parola önceden belirlenebilir.

**extract** : Medya dosyasının içindeki gizli veriyi dışarı çıkarmak için kullanılır. Bu komut parola girilerek gizlenmiş veri gösterilebilir.

### **3.2.Önemli Parametreler:**

-ef (Embed File ) : Gizlenecek veri dosyasını belirtmek için kullanılır

-cf ( Cover File ) : İçine veri gizlenilmek istenilen dosyayı belirtmek için kullanılır.

-sf ( Stego File ) : Steganografi uygulanmış dosyayı belirtmek için kullanılır.

-p ( passphrase ) : Veriyi gizlemek veya çıkarmak için parolayı önceden belirtememizi sağlayan parametredir.

-e ( encryption ) : Şifreleme parametresini önceden ayarlamamızı sağlar.

-z ( compress ) : Veriyi gizlemeden önce sıkıştırılmasını istediğimizde kullanacağınız parametredir.

-Z ( dont compress ) : Veriyi gizlemeden önce sıkıştırılmamasını istediğimizde kullanacağımız parametredir.

-v ( verbose ) : İşlemleri gerçekleştirirken detaylı bilgi vermesini istediğimizde bu parametreyi kullanırız.

### **3.3.Faydalı Örnekler :**

- steghide embed –ef secret.txt –cf resim.jpg : resim.jpg’in içine secret.txt’i gizliyor. ( Bunu yaparken girişte parola isteyecektir.)

- steghide embed –sf steganografiliresim.jpg –ef secret.txt –cf resim.jpg : resim.jpg’in içine gizliyeceği secret.txt’i resim.jpg’in aynısından oluşturup onun içine gizliyor.

- steghide –encinfo : İçine veri gizlenirken hangi şifreleme algoritması kullanacağını seçmek için kullanılır.

- steghide embed –ef secret.txt –cf resim.jpg –p “parola123” –e des : resim.jpg’in içine secret.txt’i des algoritması kullanarak ve parola olarak da “parola123” kullanarak gizleme yapan fonksiyondur.

- Steghide extract –sf steganografiliresim.jpg : Steganografi uygulanmış fotoğrafın içindeki gizlenmiş veriyi parola girerek çıkartma görevini görüyor.

- steghide embed –cf resim.jpg –ef gizli.txt –N –p “15509014” : Tekrar aynı şekilde resim.jpg’in içine gizli.txt’i önceden ayarlanmış parolayı kullanarak gizleme yapar fakat –N parametresi sayesinde extract yaptığımızda gizli.txt diyemez. Çünkü –N parametresi gizlemek istediğimiz dosyanın ismini saklamaz. Bu yüzden

- çıkartırkende –xf parametresini kullanarak isim belirleyerek çıkartmamız gerekir.

* md5sum uygulaması ile oluşan steganografi uygulanmış dosyalar ve çıkartılan dosyaların hash kontrolü yapılabilir.

## 4.Kaynakça

1-[http://www.siberportal.org/red-team/information-gathering-for-penetration-tests/information-gathering-by-using-exiftool-for-penetration-tests/](http://www.siberportal.org/red-team/information-gathering-for-penetration-tests/information-gathering-by-using-exiftool-for-penetration-tests/)

2-[https://sno.phy.queensu.ca/~phil/exiftool/TagNames/index.html](https://sno.phy.queensu.ca/~phil/exiftool/TagNames/index.html)

3-[http://www.sordum.net/30563/exif-verisi-nedir-nasil-goruntulenir-ve-kaydedilir/](http://www.sordum.net/30563/exif-verisi-nedir-nasil-goruntulenir-ve-kaydedilir/)

4- [http://remzidalyan.com/articles/2017-03/steghidekullan%C4%B1m%C4%B1](http://remzidalyan.com/articles/2017-03/steghidekullan%C4%B1m%C4%B1)

[5- http://steghide.sourceforge.net/documentation/manpage.php](http://steghide.sourceforge.net/documentation/manpage.php)

6- [https://github.com/necrose99/stegui/wiki/steghide](https://github.com/necrose99/stegui/wiki/steghide)

