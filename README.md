# RS485 Haberleşmeli Röle Kontrol Devresi Tasarımı

Bu projede, endüstriyel haberleşme protokollerinden biri olan RS485 protokolü üzerinden kontrol edilebilen bir I/O (giriş/çıkış) devresi tasarlanmıştır. Devre, STM32 serisi
mikrodenetleyici tabanlı olup, opto-izolasyon ve elektromanyetik girişim (EMI) koruması gibi endüstriyel güvenlik önlemleriyle desteklenmiştir. Tasarlanan sistem, uzak bir noktadan gelen RS485 sinyalleri ile röle çıkışlarını kontrol etme, sistem durumunu LED ile geri bildirim
sağlama ve yüksek güvenilirlikle çalışmayı hedeflemektedir.

## Sistem Bileşenleri ve Görevleri

### 1. Mikrodenetleyici (STM32F103C8T6)

Projenin temel kontrol birimi olan STM32F103C8T6, ARM Cortex-M3 mimarisi üzerine kurulu bir mikrodenetleyicidir. Bu MCU, RS485 haberleşmesini yönetmek, gelen komutlara
göre röleleri tetiklemek ve sistem durumunu izlemekle sorumludur. 72 MHz çalışma frekansı ve gelişmiş çevresel birimleri ile gerçek zamanlı kontrol uygulamaları için uygundur.

### 2. Röle Sürme Devresi (SRD-05VDC-SL-C)

Kullanılan röle, 5V DC ile çalışan ve AC/DC yükleri anahtarlamak için kullanılan elektromekanik bir anahtardır. Mikrodenetleyiciden gelen lojik seviyeli sinyal, bir transistör vasıtasıyla bu röleyi tetikler. Röle çıkışları, örneğin lambalar, motorlar veya başka bir yüksek güçlü sistemin anahtarlanmasında kullanılabilir.

### 3. Opto İzolatör (140356145000)

Mikrodenetleyici ve röle devresi arasındaki elektriksel izolasyonu sağlamak için opto izolatör kullanılmıştır. Bu bileşen, lojik sinyalleri optik olarak ileterek elektriksel girişimlerin (EMI)
mikrodenetleyiciye zarar vermesini önler ve sistemin endüstriyel ortamlarda daha güvenli çalışmasını sağlar.

### 4. RS485 Haberleşme Entegresi (MAX485CSA+T)

RS485, uzun mesafelerde güvenilir haberleşme sağlayan diferansiyel bir iletişim protokolüdür. MAX485 entegresi, mikrodenetleyicinin UART çıkışlarını RS485 sinyallerine dönüştürerek haberleşmeyi mümkün kılar. Bu entegre hem alıcı hem verici modda çalışabilir.

### 5. RGB LED (150044M155260)

Sistemin durumunu kullanıcıya görsel olarak bildirmek için kullanılan RGB LED, örneğin;

- Kırmızı: Hata durumu
- Yeşil: Röle aktif
- Mavi: Girişe enerji verildiğinde gibi amaçlarla kullanılabilecek. Her bir renk mikrodenetici tarafından PWM yöntemiyle kontrol edilebilir.

### 6. Kristal Osilatör (ECS-80-18-33-JGN-TR)

Mikrodenetleyicinin doğru ve kararlı bir saat sinyali ile çalışması için 8 MHz frekanslı kristal osilatör kullanılmıştır. Kristal, çevresine bağlanan 33pF seramik kapasitörler ile rezonans devresi oluşturur.

### 7. Pasif Bileşenler

 - **Kapasitörler**: 100nF (besleme bypass), 33pF (kristal devresi), haberleşme hatları ve röle bobini gibi noktalarda gürültü filtrelemesi amacıyla kullanılır.
 - **Dirençler**: 10K (pull-up/down), 1K (baz dirençleri), 120 Ohm (RS485 sonlandırma direnci), 2.2 Ohm ve 22K gibi değerler sinyal şekillendirme, akım sınırlama ve referans oluşturma amacıyla kullanılmıştır.

### 8. Diyotlar

 - **1N4007**: Röle bobin uçlarına ters paralel bağlanarak **flyback** voltajlarını bastırır ve transistörün zarar görmesini önler.
 - **TVS Diyot (PESD5V0L1BA)**: RS485 hatlarına bağlanarak, ani voltaj sıçramalarına karşı koruma sağlar (ESD koruması).

### 9. Transistör (2N2222A)

NPN tipi 2N2222A transistörü, mikrodenetleyici çıkışlarının akım kapasitesi yetersiz olduğunda, röleleri sürmek amacıyla kullanılır. Transistör, düşük akımlı giriş sinyalini yükselterek röle bobinine gerekli akımı sağlar.

<img src="https://github.com/user-attachments/assets/793494d9-8774-4710-a566-37e44325b76b" width="480">

*Şekil 1 RS485 Haberleşmeli Röle Kontrol Devresi Şematik*

## Devre Tasarımı ve Çalışma Prensibi

Devre, RS485 üzerinden gelen komutları UART protokolü ile alarak, bu komutlara bağlı olarak röleleri tetiklemektedir. Mikro denetici, gelen sinyalleri işler ve ilgili transistörü ile röleyi aktif hâle getirir. Röle çıkışı ile harici bir yük kontrol edilebilir. Sistemde opto izolatör ile izolasyon sağlanarak hem kontrol hem yük tarafında oluşabilecek elektriksel farklılıkların zarar vermesi engellenmiştir.

RGB LED sayesinde sistemin çalışma durumu görsel olarak izlenebilir. RS485 sonlandırma direnci olarak 120 Ohm’luk direnç kullanılmıştır. Diyotlar, rölelerde oluşan ani gerilim
yükselmelerine karşı koruma sağlar. Devrenin tüm bileşenleri mikro denetici etrafında sistematik bir şekilde yerleştirilmiş ve sinyal bütünlüğü dikkate alınarak yerleşim yapılmıştır.

<img src="https://github.com/user-attachments/assets/23984489-ceb2-4b02-a4dc-ac20c8fea491" width="480">

*Şekil 2 RS485 Haberleşmeli Röle Kontrol Devresi 2D Gösterimi*

## Sonuç ve Değerlendirme

Tasarlanan bu RS485 kontrollü röle sistemi, düşük maliyetli, güvenilir ve genişletilebilir bir endüstriyel otomasyon modülüdür. Sistemin, çeşitli sensör ve aktüatörlerle genişletilmesi
mümkündür. Bu yapı, özellikle bina otomasyonu, uzaktan kumanda sistemleri ve endüstriyel
kontrol uygulamalarında kullanılabilir niteliktedir. RS485 protokolü sayesinde birden fazla cihaz aynı hat üzerinde bağlanabilir ve modbus gibi üst protokollerle entegre çalışabilir.

<img src="https://github.com/user-attachments/assets/f7b78409-cfe0-473e-ad8b-6af18810d4a9" width="480">

*Şekil 3 RS485 Haberleşmeli Röle Kontrol Devresi 3D Gösterimi*

<img src="https://github.com/user-attachments/assets/7c45e899-5fe2-4bd5-aabd-e77e7805474b" width="480">

*Şekil 4 RS485 Haberleşmeli Röle Kontrol Devresi 3D Gösterimi*

Devreye enerji verildiğinde, sistemin çalıştığını göstermek amacıyla RGB LED’in mavi rengi aktif edilmektedir. Kullanıcı, röle çıkışına bağlı butona bastığında, röle tetiklenerek bağlı yüke enerji gönderilir ve bu durum RGB LED’in yeşil rengi ile görsel olarak kullanıcıya bildirilir. Eğer sistemde bir arıza meydana gelir ya da güç kaynağında bir kesinti yaşanırsa, RGB LED kırmızı renkte yanarak hata durumunu belirtir.

Tüm kontrol ve durum izleme işlemleri, RS485 haberleşme protokolü üzerinden gerçekleştirilmektedir. Bu işlemler, STM32F103C8T6 mikrodenetleyici ile geliştirilen gömülü yazılım sayesinde kontrol edilmektedir. Yazılım geliştirme süreci, STM32CubeIDE platformu kullanılarak tamamlanmıştır. RGB LED kontrolü, GPIO pinleri üzerinden PWM sinyalleri ile; röle sürme işlemi ise transistör kontrollü dijital çıkış ile sağlanmıştır. RS485 haberleşme ise UART modülü ile MAX485 entegresi üzerinden yönetilmektedir.

## Yazılım Geliştirme Süreci

<img src="https://github.com/user-attachments/assets/74c0ca3d-76e7-40f2-bd5c-f41a54d126b7" width="480">

*Şekil 5 RS485 Haberleşmeli Röle Kontrol Devresi Pin-Clock Ayarlama*

<img src="https://github.com/user-attachments/assets/eb562fb1-ce02-432b-bfb3-589312101d9c" width="480">

*Şekil 6 RS485 Haberleşmeli Röle Kontrol Devresi Yazılımı*

