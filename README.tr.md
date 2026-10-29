# 24 V / 2 A (48 W) Flyback Dönüştürücü – Tasarım Hesapları ve Prototip

[English](README.md)

İzoleli, DCM, tepe akım modlu flyback dönüştürücü. Tasarım hesapları TI SSZTCW6 (DCM flyback, Eq. 1–27), SSZTCV6 (RCD snubber) ve SLUP127 (trafo boyutlandırma) kaynaklarına dayanır. Tasarım elle sarılmış gerçek bir prototiple hayata geçirilmiş ve ölçülmüştür; hesap ve ölçüm sonuçları aşağıda karşılaştırılmıştır.

## Prototip Karşılaştırması

| Büyüklük | Hesap | Ölçüm |
|---|---|---|
| Verim (tam yük) | %88.8 – 89.0 | %88 |
| Vds tepe | 641 V | 582 V |
| Ipk | 1.58 A | 1.6 A |

## Tasarım Girdileri

| Parametre | Değer |
|---|---|
| Çıkış | 24 V / 2 A (48 W) |
| DC bara | ≈ 235 – 375 V |
| Anahtarlama frekansı | 100 kHz (sabit) |
| Mod | DCM, tepe akım modu |
| Hedef verim | %89.3 |

## Hesap Sonuçları

| Büyüklük | Değer |
|---|---|
| Np/Ns | 5.91 → uygulanan 41 : 7 |
| Lpri | 429 µH (maks. 472 µH, ±%10) |
| Ipk / Irms (primer) | 1.58 A / 0.50 A |
| Isec,pk / Isec,rms | 9.35 A / 3.69 A |
| Duty D | %29.6 (Vin,min) – %18.4 (Vin,max) |
| Akım algılama Rs | 311 mΩ (limit 2.12 A) |
| Düz tepe Vds / snubber ile tepe | 520 V / 641 V (800 V FET) |
| Diyot VPIV | 87 V (×1.3 = 114 V, Vrrm 200 V) |
| Cout / Cin | ≥ 62.2 µF / ≥ 46.8 µF |

DCM sınırı en kötü durumda korunur (t3 = T'nin %20'si).

## Trafo

| Parametre | Değer |
|---|---|
| Sarım | Np = 41, Ns = 7 (Np,min = 38 @ Bmax 0.35 T) |
| Bpk | 0.32 T (limit akımı), 0.22 T (nominal) |
| AL / hava aralığı | 255 nH/sarım² / ≈ 0.37 mm |
| Tel çapı | Primer 0.33 mm, sekonder 0.90 mm (6 A/mm²) |
| Pencere doluluğu | %70 (Ap = 0.67 cm⁴) |
| Çekirdek | ETD29 |

**Çekirdek seçimi.** Kullanılabilir pencere alanının yaklaşık %70'i kullanılmıştır. EF25 gibi daha küçük bir çekirdeğe sığması mümkün olabilir, ancak denenmemiştir. Daha büyük bir hava aralığı gerektirmesi muhtemeldir; bu da saçaklanma akısını (fringing) ve kaçak endüktansı artırıp EMI sonuçlarını kötüleştirebilir. Ayrıca EF25 üzerine elle sarım çok daha zor olacağından ETD29 tercih edilmiştir. Tasarımda EMI göz önünde bulundurulmuştur, ancak emisyon performansı ölçülmemiştir.

## RCD Snubber

| Parametre | Değer |
|---|---|
| Vreset / Vclamp | 145 V / 266 V (k = 1.83) |
| Lleak (varsayım) | %2 Lpri = 8.6 µH |
| Psn | 2.63 W (Pout'un %5.5'i) |
| Rsn / Csn | 27 kΩ (≥ 5.3 W) / ≥ 2.2 nF |
| Klemp diyodu | ≥ 770 V, hızlı toparlanmalı |

Snubber, en büyük tekil kayıp kalemidir. Kaçak endüktansı azaltmak (sargı sıralaması) veya k'yi artırmak kaybı düşürür, ancak Vds tepesini yükseltir.

**Kaçak endüktans pratikte.** Kaçak endüktansı azaltmak için daha sıkı sarmak ve sargı alanını olabildiğince verimli kullanmak gerekir; bu da uygun bir bobin (karkas) gerektirir. Bu karkasları Türkiye'de bulmak oldukça zordur. İlk sarımımda kaçak endüktans yaklaşık 20 µH idi; trafoyu daha sıkı yeniden sararak yaklaşık 8 µH'ye (Lpri'nin ≈ %2'si) düşürdüm. Elle sarım gerçekten zorlayıcıdır.

## Kayıp Bütçesi ve Verim

| Kalem | Vin,min | Vin,max |
|---|---|---|
| FET (iletim + anahtarlama + Coss) | 1.33 W | 1.31 W |
| Çıkış diyodu | 1.20 W | 1.20 W |
| RCD snubber | 2.46 W | 2.46 W |
| Trafo (bakır + çekirdek) | 0.82 W | 0.73 W |
| Rs, Cout ESR, bias | 0.27 W | 0.24 W |
| **Toplam kayıp** | **6.08 W** | **5.93 W** |
| **Verim** | **%88.8** | **%89.0** |

## Sınırlamalar

Kaçak endüktans ve çekirdek kaybı girilen değerlerdir; hava aralığı hesabı saçaklanmayı ihmal eder. İzolasyon mesafeleri ve EMI bu hesaplara dahil değildir.

## Depo Yapısı

- `Calculations/` – tasarım hesapları
- `Simulation/` – devre simülasyonu
- `Schematic and PCB/` – şema ve PCB tasarım dosyaları
- `Hardware and prototype/` – prototip fotoğrafları ve ölçümler
- `components datasheet/` – seçilen komponentlerin datasheet'leri

## Kaynaklar

TI SSZTCW6, TI SSZTCV6, TI SLUP127 (Unitrode Magnetics Design Handbook, §5)
