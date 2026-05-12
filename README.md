# 💻 YakutDoğan - 2. El PC Donanım ve Aksesuar Pazar Yeri

## 📌 Proje Özeti
YakutDoğan, kullanıcıların ikinci el bilgisayar parçalarını (Ekran kartları, Anakartlar, RAM vb.) ve aksesuarlarını güvenle alıp satabildiği **C2C (Tüketiciden Tüketiciye) tabanlı dinamik bir e-ticaret platformudur.** Türkiye'nin önde gelen pazar yerlerinden (Sahibinden, Letgo) ilham alınarak, özellikle "donanım tutkunları" için niş bir topluluk ve güvenli bir alım-satım ortamı yaratmak amacıyla sıfırdan geliştirilmiştir.

## 🚀 Sistemin Temel Modülleri ve Teknik Detaylar

Sistem, kullanıcı deneyimini kesintiye uğratmayan modern bir arayüz (Bootstrap 5) ve arka planda güvenli bir PHP/MySQL mimarisi ile inşa edilmiştir.

### 🔐 1. Gelişmiş Kimlik Doğrulama ve Güvenlik (Auth & Security)
* **Kriptografik Şifreleme:** Kullanıcı şifreleri veritabanına düz metin olarak değil, PHP'nin `password_hash()` fonksiyonu ile şifrelenerek kaydedilmektedir.
* **Token Bazlı Şifre Sıfırlama:** Şifresini unutan kullanıcılar için `bin2hex(random_bytes(16))` ile benzersiz ve tek kullanımlık güvenlik token'ları üretilir. Bu token'lar veritabanında eşleştirilerek güvenli şifre sıfırlama akışı sağlanır.
* **XSS ve SQL Injection Koruması:** Kullanıcıdan alınan tüm veriler `htmlspecialchars()` ile temizlenmiş ve veritabanı sorguları **PDO Prepared Statements** kullanılarak tamamen güvenli hale getirilmiştir.

### 🛒 2. Kalıcı Sepet Sistemi (Persistent Cart)
Sepet işlemleri geçici oturum (session) değişkenleri yerine, doğrudan veritabanındaki `cart` tablosu üzerinden ilişkisel olarak yönetilmektedir. Bu sayede kullanıcılar farklı cihazlardan giriş yapsalar dahi sepetlerindeki (ekledikleri, sildikleri) ürünleri kaybetmezler ve anlık ara toplam hesaplamalarını görebilirler.

### 📦 3. İlan ve Envanter Yönetimi
* **Görsel Yükleme ve İşleme:** Kullanıcılar ilanlarına ürün görselleri ekleyebilirler. Yüklenen dosyalar kontrol edilerek sunucudaki `Parcalar/` dizinine dinamik olarak kaydedilir.
* **Durum Yönetimi:** Satılan bir ürün tamamen silinmek yerine durum tabanlı güncellenerek (`IsSold = 1`) katalogda "Tükendi/Satıldı" etiketiyle listelenmeye devam eder. Bu durum, veri kaybını önler ve platformun istatistiksel bütünlüğünü korur.
* **Dinamik Filtreleme:** Kategori tabanlı (`categories` tablosu) dinamik filtreleme ve arama motoru entegrasyonu mevcuttur.

### 🛡️ 4. Rol Bazlı Yetkilendirme (Admin Dashboard)
Standart kullanıcıların aksine, sistem yöneticileri (Admin) için özel bir kontrol paneli tasarlanmıştır. Adminler, platformdaki tüm ilanları, ürün durumlarını (Satışta/Satıldı) ve sistem istatistiklerini tek bir ekranda görüp, kural ihlali yapan ilanları kalıcı olarak sistemden uzaklaştırma yetkisine sahiptir.

## 🗄️ Veritabanı Mimarisi (RDBMS)
Proje, veri tekrarını önleyen Normalize (3NF) edilmiş bir SQL mimarisi kullanmaktadır:
* **`users`:** Kullanıcı bilgileri, şifre hash'leri ve reset token'ları.
* **`categories`:** Ürün ağacı ve donanım sınıflandırmaları.
* **`products`:** İlan detayları, fiyatlar, görsel yolları (`ImagePath`) ve satış durumları.
* **`cart`:** Hangi kullanıcının (`UserID`), hangi ürünü (`ProductID`) sepetine eklediğini tutan köprü tablo.

## 📄 Proje Dökümantasyonu
Projenin ekran senaryoları (Wireframe), kullanım koşulları sözleşmesi, yerel sunucu (XAMPP/MAMP) kurulum adımları ve mimari detayları proje dizininde bulunan **`YakutDoganDokuman.docx`** dosyasında detaylıca anlatılmıştır.

## 👨‍💻 Geliştirici Ekip
* **Ahmet Eren Yakut** - Backend & Veritabanı Mimarisi 
* **Yiğit Doğan** - Frontend & UI Tasarımı
