# ⚡ Endüstriyel Siber-Fiziksel Güvenlik ve Emniyet Mimarisi: Trafo Kurutma Fırını (IDT Serisi)

[![IEC 62443](https://img.shields.io/badge/Standart-IEC%2062443-blue.svg)](https://www.isa.org/iec-62443)
[![Cyber Resilience Act](https://img.shields.io/badge/Mevzuat-EU%20CRA-orange.svg)](https://digital-strategy.ec.europa.eu/en/policies/cyber-resilience-act)
[![Safety Integrity](https://img.shields.io/badge/Emniyet-Fonksiyonel%20Emniyet-critical.svg)]()

Bu repository, **Sistem Teknik IDT Serisi Trafo Kurutma Fırını** için geliştirilmiş kapsamlı bir siber-fiziksel güvenlik değerlendirmesini, risk matrisini, güvenli mimari tasarımını ve olay müdahale iş akışını içermektedir.

Proje, endüstriyel ortamlardaki kritik emniyet-güvenlik etkileşimlerini ele alarak **OT (Operasyonel Teknoloji) Siber Güvenliği** ile **Fonksiyonel Emniyet (IEC 61508 / IEC 62061)** disiplinlerini bir araya getirmektedir.

---

## 🏭 Sistem Genel Bakışı ve Hedef Makine
* **Makine:** Sistem Teknik IDT Serisi Trafo Kurutma Fırını (Yüksek sıcaklık odası + Vakum sistemi + PID Isıtma Eşanjörü).
* **Seçilen Kritik Senaryo (B Seçeneği):** 
  * *Tehdit:* Varsayılan/zayıf kimlik bilgileriyle (`admin/admin`) internete açık uzak bakım modülleri üzerinden gerçekleştirilen yetkisiz uzaktan erişim.
  * *Saldırı Vektörü:* PLC dahili **PID ısıtma rampa parametrelerinin ve set-point değerlerinin** manipüle edilerek fırın sıcaklığının güvenli sınırların üzerine çıkarılması ($350\text{ °C}+$).
  * *Fiziksel Sonuç ($S=3$):* Trafo yalıtım malzemelerinin termal bozunmaya (pyrolysis) uğraması, zehirli gaz üretimi, egzoz/vent vanalarının zorla kapatılması ve basınç/buhar patlaması neticesinde ölümcül termal yanıklar ile akut solunum yetmezliği.

---

## 📁 Repository Yapısı ve Laboratuvar Modülleri

| Modül | Odak Alanı | Temel Çıktı / Deliverable |
| :--- | :--- | :--- |
| **Lab 1** | Emniyet Mimarisi (Safety Architecture) | Emniyet Fonksiyon Haritası (`Emergency Shutdown`, `STO`, Bağımsız Termokupllar) |
| **Lab 2** | IEC 62443 Mini Atölyesi | Bölge ve Geçiş (Zone & Conduit) Diyagramı (Zone 0/1 OT ile Zone 2 IT İzolasyonu) |
| **Lab 3** | Tehdit ve Risk Matrisi | Risk Sicili (Termal-toksik tehlikelere odaklanan 18 puanlık Kritik Senaryo) |
| **Lab 4** | Güvenli Uzaktan Erişim | Güvenli Mimari (Jump Server, MFA, VPN, Kişisel Hesaplar, Varsayılan Kapalı Portlar) |
| **Lab 5** | SBOM, CVE ve Yama Yönetimi | Zafiyet Kararı (Termal şoka yol açmadan CVSS 9.8 açıklarının güvenli yönetimi) |
| **Lab 6** | CRA Olay Simülasyonu | Olay Müdahale İş Akışı (EP 13: 15 dk triyaj, 2 saat izolasyon, 24s/72s AB CRA raporlaması) |
| **Lab 7** | Yapay Zekâ Güvenlik Sınavı | YZ Fail-Safe Kararları (`Human Override`, `Plausibility Check`, `Fallback`) |

---

## 🔒 Temel Mühendislik Vurguları

### 1. Sorumlulukların Ayrılması (Emniyet vs. Kontrol)
* Standart otomasyon mantığı (PLC proses kontrolü), **Safety PLC** ve donanımsal acil durdurma (trip) hatlarından katı bir şekilde ayrılmıştır.
* Uzaktan bakım üzerinden gelen komutların, donanımsal sıcaklık limit switch'lerini doğrudan override etmesi engellenmiştir.

### 2. Güvenli Uzaktan Erişim Mimarisi (5 İlke)
* **Doğrudan İnternet Yok:** Uzak bakım modülleri güçlendirilmiş bir **Jump Server** arkasında gizlenmiştir.
* **Kimlik Doğrulama ve Şifreleme:** Zorunlu **MFA**, şifreli **VPN tünelleri** ve ortak `admin/admin` hesapları yerine bireysel kullanıcı hesapları.
* **Kayıt ve Denetim:** Tam oturum kaydı (audit trails) ve **varsayılan kapalı** port yönetimi (yalnızca yetkili bakım pencerelerinde açılır).

### 3. Siber Direnç Yasası (CRA) Olay Müdahalesi (EP 13)
* **İlk 15 Dakika:** Kontrolör yeniden başlatılmaz (RAM adli bilişim logları korunur); PID döngüleri doğrulanır ve zehirli gaz güvenliği için personel tahliye edilir.
* **24s / 72s / 14 Gün Kilometre Taşları:** Yapılandırılmış erken uyarı, kök neden analizi (güvensiz uzak ağ geçidi), mitigasyon devreye alma ve adli bilişim kanıt arşivi.

---

## 🛠️ Teknik Altyapı ve Standartlar
* **Endüstriyel Protokoller ve Standartlar:** IEC 62443 (Endüstriyel Otomasyon ve Kontrol Sistemleri Güvenliği), IEC 61508 (Fonksiyonel Emniyet).
* **Otomasyon ve Kontrol:** Siemens/Rockwell PLC Mantığı, PID Sıcaklık Kontrolörleri, SCADA/HMI Arayüzleri.
* **Güvenlik Araçları:** VPN, Jump Host Mimarisi, Syslog/Denetim Günlükleri, Dijital İkiz simülasyon ortamları.

---

## 📌 Yazar
* **İrem Tüfekçi**  
* *Yazılım Mühendisliği 4. Sınıf Öğrencisi*  
* *Fırat Üniversitesi Teknoloji Fakültesi*  
* [GitHub Profili](https://github.com/iremtufekci)
