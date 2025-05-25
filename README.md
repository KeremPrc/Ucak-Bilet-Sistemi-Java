# Uçak Rezervasyon Sistemi (Java Console App)

Bu uygulama, konsol tabanlı basit bir uçak rezervasyon sistemidir. Kullanıcılar, mevcut uçuşlar arasında seçim yaparak ad-soyad ve yaş bilgilerini girerek rezervasyon oluşturabilir. Rezervasyonlar `.json` ve `.csv` formatlarında kayıt edilir. Ek olarak rezervasyonları görüntüleme ve silme seçenekleri de mevcuttur.

---

## 🔧 Özellikler

- Uçak, lokasyon ve uçuş nesneleri tanımlanabilir.
- Mevcut uçuşlar listelenir.
- Kullanıcıdan ad, soyad ve yaş bilgisi alınarak rezervasyon oluşturulur.
- Rezervasyonlar `rezervasyonlar.json` ve `rezervasyonlar.csv` dosyalarına kaydedilir.
- Yapılan rezervasyonlar görüntülenebilir veya silinebilir.
- JSON ve CSV formatında dosya kayıt işlemleri yapılabilir.

---

## 📂 Proje Yapısı

```
UcakRezervasyonApp.java      → Tüm sınıflar ve uygulama giriş noktası
gson-2.10.1.jar              → JSON desteği için gerekli kütüphane
rezervasyonlar.json          → JSON formatında rezervasyon verisi
rezervasyonlar.csv           → CSV formatında rezervasyon verisi
```

---

## 🚀 Nasıl Çalıştırılır?

### 1. Gerekli Kurulumlar

- [Java JDK 17+](https://jdk.java.net/) sistemde kurulu olmalıdır.
- [Gson Kütüphanesi (gson-2.10.1.jar)](https://repo1.maven.org/maven2/com/google/code/gson/gson/2.10.1/) indirilmelidir.

### 2. Derleme ve Çalıştırma (Windows)

```bash
javac -cp gson-2.10.1.jar UcakRezervasyonApp.java
java -cp .;gson-2.10.1.jar UcakRezervasyonApp
```

(Linux/macOS'ta `.;` yerine `.:` kullanmalısınız.)

---

## 🧩 Kod Yapısı

### 1. `BaseEntity`
- Ortak özellikler: UUID (benzersiz kimlik) ve aktiflik bilgisi içerir.
- Diğer sınıfların kalıtım aldığı soyut sınıftır.

### 2. `DosyaIslemleri`
- JSON ve CSV'ye kayıt işlemlerini soyutlayan arayüz.

### 3. `Ucak`
- Uçak bilgilerini içerir: model, marka, seri numarası ve koltuk kapasitesi.
- JSON ve CSV'ye yazım işlemleri desteklenir.

### 4. `Lokasyon`
- Uçuşun gerçekleştiği ülke, şehir ve havaalanı bilgilerini içerir.

### 5. `Ucus`
- Bir uçuşun saat, uçak ve lokasyon bilgilerini içerir.
- Koltuk sayısı kontrolü yapılır ve rezervasyon yapılabilir.

### 6. `Rezervasyon`
- Kullanıcı bilgilerini (ad, soyad, yaş) ve uçuş bilgilerini içerir.
- Dosyaya kayıt işlemleri gerçekleştirilir.

### 7. `UcakRezervasyonApp`
- `main` metodu içerir.
- Kullanıcıya:
  - Mevcut uçuşları listeler.
  - Uçuş seçtirir.
  - Rezervasyon yaptırır.
  - Menü döngüsü ile rezervasyonları görüntüleme, silme işlemleri sunar.

---

## 🖥️ Uygulama Akışı

1. Uygulama başlatılır.
2. Uçuş listesi gösterilir.
3. Kullanıcı geçerli bir seçim yapana kadar yönlendirilir.
4. Rezervasyon bilgileri alınır.
5. Dosyaya yazılır.
6. Menüye dönülerek:
   - Yeni rezervasyon yapılabilir.
   - Kayıtlı rezervasyonlar görülebilir.
   - Rezervasyonlar silinebilir.

---

## 📝 Kullanılan Komutlar Açıklaması

- `javac -cp gson-2.10.1.jar UcakRezervasyonApp.java`: Java dosyasını Gson kütüphanesi ile birlikte derler.
- `java -cp .;gson-2.10.1.jar UcakRezervasyonApp`: Derlenen uygulamayı çalıştırır.
- `%JAVA_HOME%`: Sisteminizde Java'nın kurulu olduğu dizin.
- `UUID.randomUUID()`: Her nesneye eşsiz bir kimlik verir.
- `Scanner`: Kullanıcıdan veri almak için kullanılır.

---

## 🧹 Temizleme

Testlerinizden sonra dosyaları sıfırlamak isterseniz:

```bash
del rezervasyonlar.json
del rezervasyonlar.csv
```

---

## 📌 Notlar

- Girişlerin geçerliliği kontrol altındadır. (Hatalı seçimlerde tekrar istenir.)
- `gson-2.10.1.jar` dosyasının proje klasöründe olduğundan emin olun.
- Dosya işlemlerinde `try-with-resources` kullanılmıştır, bu sayede dosyalar otomatik kapanır.

---

## 🛡️ Lisans

Bu proje tamamen eğitim amaçlı hazırlanmıştır. Herkes kullanabilir ve geliştirebilir.