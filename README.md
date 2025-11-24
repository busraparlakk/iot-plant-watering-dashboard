# IoT Bitki Sulama Sistemi
ESP32, Firebase Realtime Database ve web tabanlı bir dashboard kullanılarak geliştirilmiş gerçek zamanlı bir bitki sulama ve izleme sistemi.

Proje tamamen simülasyon ortamında çalışmaktadır. Donanım kısmı Wokwi üzerinde, yazılım kısmı ise HTML, CSS, JavaScript ve Firebase servisleri ile gerçekleştirilmiştir.

---

## 1. Projenin Amacı
Bu projenin amacı, sensör verilerini bulut sistemine aktaran, uzaktan kontrol edilebilen ve verileri web üzerinden anlık gösteren bir IoT uygulaması geliştirmektir.  
Proje, gerçek bir akıllı sulama sisteminin temel mantığını sanal ortamda uygulamalı olarak göstermektedir.

---

## 2. Kullanılan Teknolojiler
- ESP32 (Wokwi simülasyonu)
- Firebase Realtime Database
- Firebase Authentication (Email/Password + Email doğrulama)
- HTML, CSS, JavaScript (Web dashboard)
- REST üzerinden veri gönderme (ESP32 → Firebase)
- Realtime listener ile veri okuma (Firebase → Web dashboard)
- Git / GitHub

---

## 3. Sistem Mimarisi

### Genel Veri Akışı:
1. ESP32, toprak nem değerini sensörden okur.
2. ESP32 bu veriyi JSON formatında Firebase Realtime Database'e gönderir.
3. Web dashboard Firebase'i sürekli dinler ve verileri anlık olarak günceller.
4. Dashboard üzerinde kullanıcı bir komut gönderdiğinde bu komut Firebase'e yazılır.
5. ESP32 periyodik olarak bu komutu okur ve görevi yerine getirir (sulama aç/kapat).

Sistem iki yönlü çalışmaktadır:
- ESP32 → Firebase → Dashboard  
- Dashboard → Firebase → ESP32

---

## 4. Mevcut Özellikler

### 4.1 Gerçek Zamanlı Nem Takibi
- ESP32 her veri gönderdiğinde Firebase veritabanı güncellenir.
- Dashboard üzerindeki gösterge, nem oranını canlı olarak gösterir.
- Nem seviyesine göre durum metni ve bitki simgesi değiştirilmektedir.

### 4.2 Uzaktan Sulama Kontrolü
- Dashboard üzerinden sulama aç/kapat komutu gönderilebilir.
- Komut Firebase üzerinde "controls/pump" alanına yazılır.
- ESP32 bu değeri belirli aralıklarla okuyarak pompa durumunu değiştirir.
- Dashboard üzerinde “pompa açık/kapalı” bilgisi canlı olarak görüntülenir.

### 4.3 Kullanıcı Girişi (Authentication)
- Firebase Authentication ile kullanıcı kayıt, giriş ve şifre sıfırlama işlemleri desteklenmektedir.
- Email/Password yöntemi kullanılmaktadır.
- E-posta doğrulaması zorunlu tutulmuştur.
- Doğrulanmamış kullanıcı dashboard'a erişemez.
- Login olmayan kullanıcılar otomatik olarak login sayfasına yönlendirilir.

### 4.4 Korunan Dashboard
- index.html sayfası yalnızca giriş yapmış ve e-postası doğrulanmış kullanıcılar tarafından görüntülenebilir.
- Çıkış yap butonu ile oturum kapatılabilir.

---

## 5. Proje Dosya Yapısı

iot-plant-watering-dashboard/
│
├── index.html → Ana dashboard
│
├── auth/
│ ├── login.html → Kullanıcı girişi
│ ├── register.html → Kayıt ekranı (email doğrulama içerir)
│ ├── reset.html → Şifre sıfırlama ekranı
│
├── script.js (yok) → Tüm scriptler sayfa içinde module olarak yazılmaktadır
└── README.md

---

## 6. ESP32 – Firebase Haberleşme

### ESP32 veri gönderme:
- ESP32, HTTP POST veya PUT isteği ile Firebase'e JSON gönderir.
- Gönderilen örnek veri:
```json
{
  "moisture": 45,
  "autoWater": false,
  "pumpOn": false,
  "timestamp": 1737737500000
}
``` 
### ESP32 komut okuma:
    ESP32, Firebase'deki "controls/pump" alanını GET isteği ile okur.

Eğer değer true ise pompa açılır.

false ise kapatılır.

7. Dashboard Özellikleri

Dashboard aşağıdaki unsurları içerir:

Daire şeklinde nem göstergesi

Güncel nem yüzdesi

Bitkinin durumunu gösteren simgesel ifade

Oto-sulama ve pompa durumunun bildirimi

Son güncelleme zamanı

Sulama aç/kapat butonu

Kullanıcı adı ve çıkış yap butonu

Dashboard tamamen HTML, vanilla CSS ve vanilla JS ile yazılmıştır.

8. Güvenlik Yapısı

Email doğrulaması zorunludur.

Giriş yapılmadan index.html’e erişim engellenmiştir.

Firebase Security Rules proje geliştirme aşamasında test modundadır.

Proje tamamlandığında yazma yetkisi yalnızca cihazlar ve yetkilendirilmiş kullanıcılarla sınırlandırılacaktır.

9. Kurulum ve Çalıştırma
1. Firebase Ayarları

Firebase Console'da bir proje oluşturulur.

Realtime Database aktif edilir.

Email/Password Authentication etkinleştirilir.

Web App oluşturulur ve config bilgileri projeye eklenir.

2. Web Dashboard’ı Çalıştırmak

Terminalden: npx serve
Ardından : http://localhost:3000
ile dashboard çalışır.
3. ESP32 (Wokwi)

Wokwi üzerinde ESP32 devresi açılır.

Kod içine Firebase URL'i ve endpoint eklenir.

Simülasyon çalıştırıldığında dashboard ile bağlantı başlar.
10. Gelecek Geliştirmeler


Nem geçmişi kaydı (history) oluşturma


Grafiksel veri gösterimi


Birden fazla bitki destekleme


Kullanıcıya özel bitki profili


Mobil uyumluluğun geliştirilmesi


Dashboard için tema seçenekleri



11. Sonuç
Bu çalışma, bir IoT sisteminin uçtan uca mimarisini oluşturarak sensör okuma, bulut iletişimi, uzaktan kontrol ve kullanıcı yetkilendirme gibi konuları tek projede bir araya getirmektedir.
Sanal ortamda gerçekleştirilmiş olsa da gerçek bir akıllı sulama cihazının çalışma mantığını birebir göstermektedir.

---
