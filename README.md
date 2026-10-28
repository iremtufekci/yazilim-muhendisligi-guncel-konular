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

| No | Siber Emniyet Başlığı | İlgili Lab | Projedeki Uygulama / Karşılığı |
| :---: | :--- | :---: | :--- |
| **01** | **Asset Inventory** *(Varlık Envanteri)* | **Lab 2** | Fırın içi PLC, HMI panelleri, vakum pompaları ve PID ısıtma eşanjörlerinin envanteri. |
| **02** | **Network Architecture** *(Ağ Mimarisi)* | **Lab 2 & 4** | OT (Operasyonel Teknoloji) saha ağının topoloji ve haberleşme hatları şeması. |
| **03** | **Safety Functions** *(Emniyet Fonksiyonları)* | **Lab 1** | Acil durdurma (`Emergency Shutdown`), `STO` ve bağımsız emniyet termokupl hatları. |
| **04** | **Cyber Risk Assessment** *(Siber Risk Analizi)* | **Lab 3 & 4** | $S=3$ ölümcül risk kriterine göre optimize edilmiş 18 puanlık Kritik Risk Sicili. |
| **05** | **Zone & Conduit** *(Bölge ve Geçişler)* | **Lab 2 & 4** | Saha seviyesi (Zone 0/1 OT) ile dış dünya (Zone 2 IT) arası segmentasyon. |
| **06** | **Remote Access Procedure** *(Uzak Erişim)* | **Lab 4** | Şifreli VPN, Jump Server, MFA ve varsayılan kapalı port kuralları. |
| **07** | **User / Role Matrix** *(Kullanıcı Rol Matrisi)* | **Lab 4** | Operatör, servis mühendisi ve sistem yöneticisi yetki sınırlandırmaları. |
| **08** | **Software / Firmware Inv.** *(Yazılım Env.)* | **Lab 5** | PLC Real-Time OS ve gömülü protokol yığımlarının sürüm envanteri. |
| **09** | **SBOM** *(Yazılım Malzeme Listesi)* | **Lab 5** | 7 sütunlu standartlara tam uyumlu (Bileşen, Sürüm, Tedarikçi vb.) SBOM tablosu. |
| **10** | **Vulnerability Records** *(Zafiyet Kayıtları)* | **Lab 5** | `Component X` v2.4 üzerindeki CVE / CVSS 9.8 kritik zafiyet kayıtları. |
| **11** | **Patch / Update Records** *(Yama Kayıtları)* | **Lab 5** | Canlı sistemde termal şoka yol açmamak için **Dijital İkiz** ortamında yapılan yama testleri. |
| **12** | **Backup & Restore** *(Yedekleme & Geri Yükleme)*| **Lab 6** | PLC parametrelerinin, PID set-point değerlerinin ve sistem imajlarının yedekleme planı. |
| **13** | **Incident Response** *(Olay Müdahale)* | **Lab 6** | *EP 13 İş Akışı:* 15 dk kritik triyaj (RAM log koruma), 2s izolasyon ve 24s/72s CRA bildirimleri. |
| **14** | **Change Management** *(Değişim Yönetimi)* | **Lab 6** | Fırın PID rampa ve sıcaklık set-point değişikliklerinde onay ve kayıt prosedürleri. |
| **15** | **Verification / Test** *(Doğrulama ve Test)* | **Lab 7** | YZ tabanlı süreçlerde *Human Override*, *Plausibility Check* ve emniyet doğrulama testleri. |

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
