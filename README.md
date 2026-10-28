# ⚡ Yenilenebilir Enerji Dağıtım ve Akıllı Şebeke Yönetim Sistemi

Bu proje, farklı parametrelere ve üretim kapasitelerine sahip yenilenebilir enerji kaynaklarından (Güneş, Rüzgar vb.) elde edilen elektriği; öncelik sıralarına ve maliyet kriterlerine göre tüketici düğümlerine aktaran, **Nesneye Yönelik Programlama (OOP)** prensiplerine ve **SOLID** ilkelerine tam uyumlu bir simülasyon yazılımıdır.

---

## 📌 İçindekiler
- [Mimari ve Tasarım Kalıpları](#-mimari-ve-tasarım-kalıpları)
- [Proje Sınıf Yapısı ve Detayları](#-proje-sınıf-yapısı-ve-detayları)
- [UML Mimari Mantığı ve İlişkiler](#-uml-mimari-mantığı-ve-ilişkiler)
- [Kurulum ve Çalıştırma](#-kurulum-ve-çalıştırma)
- [Birim Testleri (Unit Tests)](#-birim-testleri-unit-tests)
- [Ekip Görev Dağılımı](#-ekip-görev-dağılımı)

---

## 🏗️ Mimari ve Tasarım Kalıpları

Proje kurgulanırken kod tekrarını önlemek, modülerliği artırmak ve bakımı kolaylaştırmak adına aşağıdaki tasarım kalıplarından (Design Patterns) yararlanılmıştır:

- **Strategy Pattern:** Dağıtım algoritmalarını (`PriorityDistributionStrategy`, `CostOptimizedDistributionStrategy`) ana sistemden ayırarak çalışma zamanında (runtime) dinamik olarak değiştirilebilir hale getirir.
- **Factory Pattern:** Enerji kaynaklarının ve tüketici nesnelerinin esnek bir şekilde üretilmesini sağlar.
- **Abstraction & Inheritance:** `EnergySource` soyut sınıfı (abstract class) üzerinden polimorfik bir kaynak hiyerarşisi kurgulanmıştır.

---

## 🧩 Proje Sınıf Yapısı ve Detayları

### 1. Veri ve Model Katmanı
* **ConsumerNode:** Şebekedeki enerji talep noktalarını (konut, fabrika vb.) temsil eder. İki ana değişken grubu barındırır: Kimlik verileri (`id`, `name`) ve şebeke yönetim verileri (`requiredEnergy`, `priority`). kapsülleme (encapsulation) prensibine uygun olarak değişkenler private tanımlanmış, dış erişim getter/setter metotları ile sağlanmıştır.
* **EnergySource (Abstract Class):** Tüm enerji kaynaklarının ata sınıfıdır. Ortak özellikleri (`id`, `name`, `capacity`) barındırır ve türetilen tüm sınıfların uygulamak zorunda olduğu `calculateEnergyOutput()` soyut metodunu tanımlar.
* **SolarPanel:** `EnergySource` sınıfından türetilmiştir. Güneş paneli verimlilik katsayısına (`efficiency`) göre anlık enerji üretimini hesaplar.
* **WindTurbine:** `EnergySource` sınıfından türetilmiştir. Anlık rüzgar hızına (`windSpeed`) göre enerji üretimini hesaplar.

### 2. Strateji Katmanı (Strategy Pattern)
* **IDistributionStrategy (Interface):** Tüm dağıtım algoritmalarının uymak zorunda olduğu standart `distribute(sources, consumers)` metot imzasını barındıran arayüzdür.
* **PriorityDistributionStrategy:** Enerjiyi tüketicilerin şebekedeki öncelik derecelerine (`priority`) göre dağıtan algoritmayı çalıştırır.
* **CostOptimizedDistributionStrategy:** İletim hattı mesafelerini ve birim maliyetleri dikkate alarak hat kayıplarını en aza indirecek şekilde optimizasyon yapar.

### 3. Yönetim ve Veri Kalıcılığı Katmanı
* **ConsoleUI:** Kullanıcı arayüzünü başlatan, girdi alan, kaynak ve tüketici listelerini bünyesinde tutarak dağıtım süreçlerini tetikleyen ana yönetim sınıfıdır.
* **DataManager:** Tek Sorumluluk İlkesine (Single Responsibility Principle) uygun olarak tasarlanmıştır. Dağıtım simülasyon sonuçlarını ve sistem loglarını dosyaya kaydeder (`saveLog`) ve ihtiyaç duyulduğunda okur (`readLog`).

---

## 📐 UML Mimari Mantığı ve İlişkiler

Projenin UML diyagramındaki sınıf ilişkileri ve bağlantı yönleri şu kurallara göre kurgulanmıştır:

* **Kalıtım (Inheritance / Extends) İlişkisi:** `SolarPanel` ve `WindTurbine` sınıfları, soyut `EnergySource` sınıfına düz çizgi ve içi boş üçgen ok ucu ile bağlıdır. Bu durum iki sınıfın da `EnergySource`'tan türediğini ifade eder.
* **Arayüz Uygulama (Implementation / Implements) İlişkisi:** `PriorityDistributionStrategy` ve `CostOptimizedDistributionStrategy` sınıfları, `IDistributionStrategy` arayüzüne kesikli çizgi ve içi boş üçgen ok ucu ile bağlıdır.
* **Kullanım ve Bağımlılık (Association / Use) İlişkisi:** `ConsoleUI` sınıfı; `ConsumerNode`, `EnergySource`, `IDistributionStrategy` ve `DataManager` sınıflarına düz çizgi ve ince ok ucu ile bağlıdır. Bu durum arayüz sınıfının tüm bu yapıyı orkestre ettiğini gösterir.

*(Hazırlanan görsel UML Sınıf Diyagramı dosyası proje klasöründeki `docs/uml_diagram.png` yolunda yer almaktadır.)*

---

## 🚀 Kurulum ve Çalıştırma

### Gereksinimler
- **Java JDK:** 11 veya üzeri (Önerilen: JDK 17 / 21)
- **IDE:** IntelliJ IDEA / Eclipse / Dev-C++ / VS Code

### Çalıştırılma Adımları
1. Projeyi bilgisayarınıza klonlayın veya indirin:
   ```bash
   git clone [https://github.com/kullanici-adi/proje-repo-adi.git](https://github.com/kullanici-adi/proje-repo-adi.git)
