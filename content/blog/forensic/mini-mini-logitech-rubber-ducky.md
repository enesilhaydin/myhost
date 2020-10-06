---
title: Mini-mini Logitech Rubber Ducky
date: 2019-03-10 13:07:01
category: forensic
thumbnail: 'logitech.png'
draft: false
---

**Mini-mini Logitech Rubber Ducky**

Rubber Ducky vb. zararlı donanımları üzerinde inceleme / üretme ve anti-forensic çalışmalarım bitmedi, bitecek gibi de durmuyor :)

![](https://cdn-images-1.medium.com/max/1114/1*mJIhyv65VQFViHdhgC2g4A.png)

Bugün ise çok farklı bir çalışma olan Logitech Unifying Receiverları kullanarak MouseJacking konusuna değineceğim. Öncelikle bu özelliğin **CVE-2019–13052 **numaralı açığı sömürerek çalıştığını söylemek isterim. Güncel Logitech ürünleri rastlanıldığı üzere Unifying özelliği barındırmaktadır. Bu özellik sayesinde Mouse/Klavye receiver’i bozulması veya kaybolması durumunda Unifying Receiver sayesinde 6 adet eşleştirme ( pairing ) işlemi yapılmasına izin vermektedir. Aslında açıktan ziyede olay yapılan işlemde bitiyor. Nordic nRF52840 model RF IoT cihazımızı unifying cihazımıza tıpkı bir Mouse gibi tanımlatıp aradaki AES Key Pairing işlemini gerçekleştiriyoruz. İşin özü; bir mouse kullanımında olduğu gibi ileri,geri sürme işleminde bilgisayar tarafına HID komutları gönderilerek işlem yapılmasını sağlanıyor. Ben ise RF donanımını kullanarak çok daha farklı HID komutları göndererek Logitech Unifying Receiver’in Rubber Ducky görevini gerçekleştirmesini sağlayacağım. Gönderici olarakta nRF52840'ı kullanacağım.

Bu işlem için kullanılan yazılım ve donanımlar şu şekilde;

**Donanımlar:**

1 — Nordic nRF52840 Dongle — [https://www.robiz.net/nRF52840dongle](https://www.robiz.net/nRF52840dongle)

2 — Logitech Unifying Receiver C-U0012 ( Birçok model barındırmaktadır. U0012 olduğuna dikkat edin. )

3 — Eğer kullanılmak istenirse telefonunuza uygun OTG

**Yazılımlar:**

1 — Munifying — [https://github.com/mame82/munifying](https://github.com/mame82/munifying)

2 — LOGITracker — [https://github.com/mame82/LOGITacker](https://github.com/mame82/LOGITacker)

3 — Tercihen Debian bir sistem.

4 — nRF Connect for Desktop — [https://www.nordicsemi.com/Software-and-Tools/Development-Tools/nRF-Connect-for-desktop](https://www.nordicsemi.com/Software-and-Tools/Development-Tools/nRF-Connect-for-desktop)

5- Android cihazda serial bağlantı için — Another Term Lite uygulaması

İlk olarak yapılması gereken Logitech Unifying donanımımızın firmware’ünü LIGHTSPEED firmware’ü ile değiştirerek flashlamak olacak. ( Güncelledikten sonra donanımımızı Logitech Unifying Software görmeyebilir. Bunun için kullanmadığınız Unifying donanımını kullanmakta fayda var. )

İlgili firmware’i [Logitech’in Github](https://github.com/Logitech/fw_updates/blob/update2019-08-27/RQR39/RQR39.06/RQR39.06_B0040.shex) adresinden indirebilirsiniz. LIGHTSPEED firmware’ü kullanmamızın nedeni gizli kanal kullanıp AES Key’i elde etmemizi sağlamak ve bununda yanında hızdaki performans yükselişleridir.

İndirdiğimiz munifying aracını linux makinada build yapıyoruz. Ardından Logitech Unifying Receiver aracının bağlantısını gerçekleştirip ./munifying info komutuyla donanım hakkında bilgilere ulaşıyoruz.

![](https://cdn-images-1.medium.com/max/1874/1*5HszOSF4lpawo2R2GDv0tw.png)

Görüldüğü üzere Unifying cihazımız doğru bir şekilde çalışmaktadır. ( İlgili bilgiler güncelleme gerçekleştirildikten sonraki bilgilerdir. )

![Firmware Flash komutu.](https://cdn-images-1.medium.com/max/1124/1*CHuU6c6LGRl0JQCXo2GBag.png)

Görseldeki komutu kullanarak Unifying cihazımıza indirildiğimiz firmware dosyasını flash ediyoruz. İşlem bitene kadar kesinlikle Unifying donanımının bağlantısını kesmiyoruz.

Nordic nRF52840 donanımı default yazılımla gelmektedir. Biz LOGITracker kullanacağımız için bu yazılımı nRF52840'a flashlamalıyız. Yüklediğimiz nRF Connect aracını açıp Dongle’i bilgisayara bağlıyoruz.

![](https://cdn-images-1.medium.com/max/1332/1*gvPdH20CyTplKFXOIpEFmw.png)

Ardından listeden Programmer aracını bulup yüklü değilse yükleyip açıyoruz.

![](https://cdn-images-1.medium.com/max/378/1*n-Na3EGdu_kcaKzSoU_aeQ.png)

Üst bölümden cihazı seçiyoruz.

![](https://cdn-images-1.medium.com/max/394/1*t1iyMPIcU2Z_ZOZlUdJRmw.png)

Sağdaki File bölümünden Add Hex file’a tıklayıp Github’dan indirdiğimiz LOGITracker klasörünün altındaki **/build/logitacker_pca10059.hex** dosyasını seçiyoruz. Ardından sağ altta bulunan Write butonuna tıklayıp firmware’i flashlıyoruz. İşlem bittikten sonra cihaz LOGITracker olarak çalışacaktır. Test için Putty aracıyla bağlantı gerçekleştirelim.

![](https://cdn-images-1.medium.com/max/574/1*d2D1TJUInncTjk4fn9XADQ.png)

![MAC adresleri gizlenmiştir.](https://cdn-images-1.medium.com/max/1322/1*kFuHAR7hRe8fs04baHn4tw.png)

Bağlantı gerçekleştirdiğimizde ilk olarak bizi discover modu karşılacayacaktır. Bu mod dongle’nin etraftaki yakaladığı bütün rf 2.4 paketleri sniff yapmaktadır.

Dongle default olarak Unifying receiver modunda çalışmaktadır. Fakat biz LIGHTSPEED firmware flashladığımız için dongle’i bu moda getirmeliyiz. Bunun için;

![](https://cdn-images-1.medium.com/max/992/1*IUQcURMpydyJW-6p580SCw.png)

Şimdi AES Pairing işlemini gerçekleştirmemiz gerekiyor. Unifying cihazımız Linux makinada takılı iken **./munifying pair **komutu ile eşleştirme moduna alıyoruz. Ardından COM4 serial arayüzünden ;

![](https://cdn-images-1.medium.com/max/670/1*rm6hjSk4VlsYISEmg2FpTA.png)

Komutunu kullanarak eşleştirme işlemini gerçekleştiriyoruz. Eşleştirme işlemini doğrulamak için **./munifying info** komutunu kullanabiliriz. Çıktı aşağıdaki gibi olacaktır.

![](https://cdn-images-1.medium.com/max/2292/1*3PACh0-ZSM4vz1a5np4qZg.png)

Connected devices : 1 olup bilgileri aşağı bölümde eklenmiştir. RF adress’i not etmemiz gerekiyor. Bu adres injection yaparken kullanacağımız adrestir. Bu adresi sürekli elle girmememiz için **_devices storage load XX:XX:XX:XX:XX _**ilgili yere receiverınızın rf adresini girmeniz gerekiyor. Benim kullandığım RF adresi görüldüğü üzere **3F:D9:95:AE:0C**

# **Scripting İşlemi**

LOGITracker scripting dili olarak Hak5’in ürettiği Rubber Ducky ile aynı dili kullanmaktadır. Bu sayede internette birçok script bulup import edebilirsiniz. Örnek küçük bir script yazıp bunu cihazımıza kaydedelim. ( Dongle’in bağlantısı söküldükten sonra bellekteki script silinmektedir. Bu yüzden kalıcı bir şekilde depolamamız gerekiyor. )

**script show** komutu ile şuanda bellekte olan scripti görebiliriz. Scripting syntax’ini bir kaç örnekle açıklamak istiyorum.

![](https://cdn-images-1.medium.com/max/600/1*q8bNGiiWvovPzOO-SvmGew.png)

**script press** : Tuş vuruşu gerçekleştirir. Bu tek bir SHIFT, ALT veya CTRL tuşu olabileceği gibi 2 veya 3 lü tuş vuruşuda olabilir. WIN + R gibi veya CTRL + SHIFT + ENTER vb.

**script string **: Yazma işlemini gerçekleştirir. “enes” , “notepad” veya “PowerShell” vb.

**script delay** : Bekleme işlemidir. Bilgisayarin herhangi bir HID komutuna vereceği tepki süresi değişiklik göstereceği için farklı konutlarda test ederek farklı farklı delaylar kullanmalıyız. Örneğin WIN + R tuşuna bastıktan sonra 500 ms ( 0.5 saniye ) beklemesi gibi.

Logitech receiver’ın bilgisayara bağlantısı gerçekleştikten sonra. PowerShell’i admin yetkisinde açıp ekrana bir kaç şey yazan script yazalım. ( Bir çok gelişmiş scripti diğer yazımda paylaşacağım.)

Dikkat edilmesi gereken bir nokta var. Script yazılırken keybord layout olarak ingilizce klavye kullanılıyor. Suanlik firmware’nin Türkçe Layout desteği bulunmamakta. Umarım ilerde bununda desteğini vermiş olurum.

Script ilk olarak WIN tuşuna basarak arama kutucuğunu açıyor. Gecikmelerden dolayı 1 saniye bekliyorum. Kutucuğa **Powershell** yazıp **CTRL+SHIFT+ENTER** yapıyorum. Bu kısayolun amacı uygulamayı yönetici olarak çalıştırmayı sağlıyor. 500 ms bekleyip **ALT+Y** tuşuna basıyor. Bu tuş işe gelen UAC mesajını yes olarak cevaplıyor. Bizi karşılayan ekran ise Administrator yetkili Powershell ekranı oluyor. Bundan sonra hedef bilgisayarın tespi süresine göre delay süresi eklenip istenilen hertürlü işlem yaptırılabilir. İlk paragraflarda söylediğim gibi bir çok değişken script’i ilerleyen zamanlarda paylaşacağım. ( Script’i yazarken silme tuşu çalışmaz. Bunun için CTRL+H tuşunu kullanmanızı öneriyorum. )

![Örnek bir script.](https://cdn-images-1.medium.com/max/650/1*QkVOXho-TWDJLDVwtPUi2Q.png)

i yerine ‘ , — yerine = kullanmamızın nedeni English — Turkish layout dönüşümünden gelmektedir. Şimdi bu kodu cihazımıza **script store “script_adı” **olarak kaydedelim.

![](https://cdn-images-1.medium.com/max/1288/1*x8YokCogXoBcauR9gji1CA.png)

Görselde de görüldüğü üzere istenildiği zaman **script load “script_adı” **komutu ile tekrar hafızaya alabiliyoruz. Unutulan script isimleri için **script list **komutunu kullanabilirsiniz.

# İnjection İşlemi

Unifying cihazına 6 adet tanımlama yapabileceğimiz için ilk olarak hedef cihazı göstermeliyiz. Bunun için önceden device store komutunu kullanmıştır. Aynı şekilde hedef adreside bu şekilde tanımlayabiliriz.

**injection target** yapıp TAB tuşuna basıldığında **DISCOVER** modunda yakalanan tüm RF adresleri gözükmektedir. load yaptığımız adresi seçip enterliyoruz. Sonunda kodumuz **injection target 3F:D9:95:AE:0C **oluyor. Ardından Unifying cihazının kurban bilgisayarda takılı olduğuna emin olup. injection execute yaparak çalıştırıyoruz. ( Eğitim amaclı -Window Hide veya EncodedCommand kullanılmamıştır. )

<iframe
                width="854"
                height="480"
                src="https://cdn.embedly.com/widgets/media.html?src=https%3A%2F%2Fwww.youtube.com%2Fembed%2Ff7V9Yf7YEuw&url=http%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3Df7V9Yf7YEuw&image=http%3A%2F%2Fi.ytimg.com%2Fvi%2Ff7V9Yf7YEuw%2Fhqdefault.jpg&key=a19fcc184b9711e1b4764040d3dc5c07&type=text%2Fhtml&schema=youtube"
                frameborder="0"
                allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
                allowfullscreen
              ></iframe>

# Mobil Serial Bağlantı aracılığıyla Injection

Another Term Lite uygulamasını açıyoruz. OTG yardımı ile mobil cihaza dongle’i bağlıyoruz. Ardından görseldeki gibi UART modunu seçip baudrate hızını 115200 olarak ayarlıyoruz.

![](https://cdn-images-1.medium.com/max/2152/1*azkGLHHTKqFIVs7I5rJcSw.jpeg)

![](https://cdn-images-1.medium.com/max/2154/1*xkHbiWvZX8PnzieJDhYNZg.jpeg)

![](https://cdn-images-1.medium.com/max/2146/1*Jh12K59j19w8ttmB5-pdSw.jpeg)

> Değerli zamanlarınızı ayırıp bu makaleyi okuduğunuz için çok teşekkür ederim. Öneri veya hatalarıımı herzaman iletebilirsiniz.

Kaynakça :

> [**USBSamurai: how to make a remote controlled USB HID injecting cable for less than 10\$ | So Long, and Thanks for All the Fish**](https://www.andreafortuna.org/2019/08/21/usbsamurai-how-to-make-a-remote-controlled-usb-hid-injecting-cable-for-less-than-10/)
>
> <small>An interesting article by Luca Bongiorni explains how to create a remote controlled HID injector cable using some simple hardware components easily purchased on online stores (with less then 10\$) Remove the top black case without breaking the dongle; Push out the PCB & the orange plastic holder; Pop-open an USB Cable and solder the Vcc& GND to their related pins on the CU-0007.</small>

> [**mame82/LOGITacker**](https://github.com/mame82/LOGITacker)
>
> <small>LOGITacker is a hardware tool to enumerate and test vulnerabilities of Logitech Wireless Input devices via RF. In contrast to available tooling, it is designed as stand-alone tool. This means not only the low level RF part, but also the application part is running on dedicated hardware, which could provides Command Line Interface (CLI) via USB serial connection.</small>

> [**mame82/munifying**](https://github.com/mame82/munifying)
>
> <small>The tool munifying could be used to interact with Logitech receivers from USB side (not RF). This tool was developed during vulnerability research and is provided as-is.</small>

**Bu içerik tamamen eğitim amaçlı hazırlanmıştır.**

