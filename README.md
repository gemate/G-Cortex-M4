# G-Cortex M4 CNC Motion Controller & GMach V2 Software Integration

![Version](https://img.shields.io/badge/Version-1.0-blue.svg)
![Status](https://img.shields.io/badge/Status-Production-brightgreen.svg)
![Hardware](https://img.shields.io/badge/Board-G--Cortex%20M4-orange.svg)

**G-Cortex M4**, CNC Router, plazma/lazer kesim ve portal (gantry) tipi otomasyon sistemleri için tasarlanmış, **GMach V2** yazılımı ile PC Ethernet üzerinden haberleşen, 3 eksen (X, Y, Z) mimarisine sahip endüstriyel hareket kontrol kartıdır. Portal sistemlerde çift motor kullanımı için Y eksenine senkronize 2. çıkış (Y2) barındırır.

---

## 🛠 Temel Özellikler

- **Donanım Mimarisi:** 3 Eksen (X, Y, Z) + Y2 Senkron Çıkışı (Toplam 4 x Pulse/Dir Çıkışı)
- **Haberleşme:** PC Ethernet (GMach V2 Yazılımı ile doğrudan haberleşme)
- **Besleme Gerilimi:** 12 - 24V DC (Önerilen: 24V DC / 2A)
- **Dijital Girişler:** 20 Kanal PNP Dijital Giriş (24V)
- **NPN Çıkışlar:** 8 Kanal Röle/Yardımcı Çıkış (Spindle Enable, Water, Air, Light vb.)
- **Pulse/Dir Çıkışları:** 8 Kanal 5V D/P (4 Bağımsız Eksen Sinyali)
- **Analog Girişler:** 2 Kanal (Spindle Hız ve Feedrate Potansiyometreleri için 0-10V / 5V)
- **Manuel Kontrol:** 5V / 100 Pulse Harici El Çarkı (Handwheel MPG) desteği (X/Y/Z Mod & %1/%10/%100 Hız Çarpanı)

---

## 📐 Kart Ölçüleri

- **Genişlik (X):** 150 mm
- **Yükseklik (Y):** 120 mm
- **Derinlik (Z):** 30 mm

---

## 📌 Pin & Terminal Atamaları

### Dijital Girişler (INPUT PNP)

| Terminal | Sinyal Adı | Açıklama |
| :--- | :--- | :--- |
| **X0** | X+ LIMIT | X Ekseni + Yönü Limit Sensörü |
| **X1** | X- LIMIT | X Ekseni - Yönü Limit Sensörü |
| **X2** | Y+ LIMIT | Y Ekseni + Yönü Limit Sensörü |
| **X3** | Y- LIMIT | Y Ekseni - Yönü Limit Sensörü |
| **X4** | Z+ LIMIT | Z Ekseni + Yönü Limit Sensörü |
| **X5** | Y2+ LIMIT | Gantry Y2 Ekseni + Limit Sensörü |
| **X6** | Y2- LIMIT | Gantry Y2 Ekseni - Limit Sensörü |
| **X7** | Takım Sıfırla | Takım Ucu Referans / Sıfırlama Sensörü |
| **X8** | START | Program Başlat Butonu |
| **X9** | STOP | Program Durdur Butonu |
| **X10** | APP/BOOT | Bootloader Seçimi & Acil Stop (E-Stop) / Reset Girişi |
| **X11** | ALARM | Servo / Step Sürücü Alarm Girişi |
| **X12 - X14** | %1 / %10 / %100 | El Çarkı Hız Çarpanı Seçicileri |
| **X15 - X17** | X / Y / Z MOD | El Çarkı Eksen Seçim Girişleri |
| **X18 / X19** | ENCODER A / B | El Çarkı Encoder Sinyal Girişleri (5V) |
| **A0 / A1** | Analog Spindle / Feedrate | Spindle ve İlerleme Hızı Potansiyometre Girişleri |

### Dijital Çıkışlar (OUTPUT)

| Terminal | Sinyal Adı | Açıklama |
| :--- | :--- | :--- |
| **Y0** | SPINDLE EN | Spindle Enable Röle Çıkışı |
| **Y1** | WATER | Soğutma Suyu Pompası Çıkışı |
| **Y2** | AIR | Hava Valfi Çıkışı |
| **Y3** | LIGHT | Aydınlatma Lambası Çıkışı |
| **Y8 / Y9** | X PULSE+ / X DIR+ | X Ekseni Sürücü Sinyalleri (5V) |
| **Y10 / Y11**| Y PULSE+ / Y DIR+ | Y Ekseni Sürücü Sinyalleri (5V) |
| **Y12 / Y13**| Z PULSE+ / Z DIR+ | Z Ekseni Sürücü Sinyalleri (5V) |
| **Y14 / Y15**| Y2 PULSE+ / Y2 DIR+ | Y2 Senkron Gantry Sürücü Sinyalleri (5V) |

---

## ⚡ Bağlantı Şemaları & Kablolama

### 1. Motor Sürücü (Step / Servo) Bağlantısı

| Kart Terminali | Sinyal | Sürücü Terminali |
| :--- | :--- | :--- |
| **X PULSE +** | Darbe Pozitif | PUL+ (veya CP+) |
| **X DIR +** | Yön Pozitif | DIR+ (veya CW+) |
| **GND** | Ortak Dönüş | PUL- / DIR- (veya CP- / CW-) |

> **Uyarı:** Y, Z ve Y2 eksenleri de aynı mantıkla kendi terminal çiftleri üzerinden bağlanır.

### 2. Acil Stop (E-Stop) Bağlantısı

Acil stop butonu **APP/BOOT (X10)** terminaline **Normalde Kapalı (NC)** kontak olarak bağlanmalıdır. Basıldığında devreyi keserek tüm hareketleri anında durdurur.

### 3. Endüstriyel Kablolama Uyarısı ⚠️

Step/servo motor güç hatları ve Spindle beslemeleri yüksek frekanslı gürültü oluşturabilir. Sinyal hatlarının (Pulse/Dir, Enkoder, Potansiyometre) etkilenmemesi için **SHIELDED (Blendajlı)** kablo kullanılmalı ve kablo zırhı pano topraklamasına bağlanmalıdır.

---

## 🖥 GMach V2 Yazılım Entegrasyonu

G-Cortex M4, **GMach V2** yazılımı ile Ethernet üzerinden anlık haberleşir.

- **3D Toolpath Simülasyonu:** G-kodlarını işleme koymadan önce 3 boyutlu ortamda önizleme.
- **Eksen & Hatve Kalibrasyonu:** Setup menüsünden X, Y, Z için mm başına düşen pulse sayıları ve maksimum hız/ivme değerleri yapılandırılabilir.
- **Backlash Compensation:** X ve Y eksenleri için mekanik boşluk telafisi desteği.
- **Gantry / Y2 Senkronizasyonu:** Çift motorlu portal sistemlerde tam senkron çalışma.

---

## 📞 Teknik Destek & İletişim

**GEMATE Machine and Robotic Systems**  
- **Web:** [www.gemate.com](https://www.gemate.com)  
- **E-posta:** info@gemate.com  
- **Doküman Sürümü:** 2026 / v1.0
