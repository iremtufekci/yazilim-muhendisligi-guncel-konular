# ⚡ IDT Series Transformer Drying Oven - Safety 4.0 & OT Security Portfolio

Bu depo, endüstriyel otomasyon sistemleri, fonksiyonel emniyet (**Functional Safety - IEC 61508 / IEC 62061**) ve Operasyonel Teknoloji (OT) siber güvenliği (**IEC 62443**) standartlarına uygun olarak hazırlanmış kapsamlı bir laboratuvar ve siber-fiziksel risk analizi portfolyosudur. Projede temel çalışma hedefi olarak **Sistem Teknik Trafo Kurutma Fırınları (IDT Serisi)** ele alınmıştır.

---

## 🏭 Sistem Mimarisi ve Fiziksel Risk Profili

Güç trafolarının üretim ve izolasyon süreçlerinde kullanılan **Trafo Kurutma Fırınları (IDT Serisi)**; devasa kamara yapıları, yüksek sıcaklık kapasiteleri ($200\text{ °C}$), derin vakum sistemleri ve tonlarca ağırlıktaki hidrolik kapı/araba mekanizmaları nedeniyle yüksek risk ($S=3$) barındıran kritik endüstriyel varlıklardır. 

Bu sistemlerde yaşanabilecek bir siber saldırı veya otomasyon manipülasyonu, verisel kayıplarla değil; doğrudan **ölümcül termal yanıklar, basınç patlamaları ve ağır iş kazalarıyla** sonuçlanmaktadır. Bu depo, siber dünya ile fiziksel emniyet arasındaki bu kritik köprüyü incelemektedir.

---

## 📂 Depo Klasör Yapısı

```text
├── trafo-kurutma/                 # ⚡ Sistem Teknik Trafo Kurutma Fırını (IDT Serisi)
│   ├── LAB1.docx                  # Safety Architecture & Emniyet Fonksiyon Haritası
│   ├── LAB2.docx                  # IEC 62443 Zone & Conduit Mimarisi / Uzak Erişim Kararları
│   ├── LAB3.docx                  # Threat & Risk Assessment (L x S x E Risk Matrisi)
│   ├── LAB4.docx                  # Secure Remote Access ve Functional Safety Köprüsü
│   └── LAB5.docx                  # SBOM, CVE Analizi ve Yama Yönetim Kararları
│
├── siseleme/                      # 🍼 Monoblok Dolum ve Kapatma Hattı
│   └── (İlgili Laboratuvar Föyleri ve Analizler)
│
└── paketleme/                     # 📦 PaketPro 3000 Paketleme Hattı
    └── (İlgili Laboratuvar Föyleri ve Analizler)
