# 24 V / 2 A (48 W) Flyback Converter – Design Calculations and Prototype

[Türkçe](README.tr.md)

Isolated, DCM, peak-current-mode flyback converter. The design calculations follow TI SSZTCW6 (DCM flyback, Eq. 1–27), SSZTCV6 (RCD snubber) and SLUP127 (transformer sizing). The design was built as a hand-wound prototype and measured; calculated and measured results are compared below.

## Prototype vs. Calculation

| Quantity | Calculated | Measured |
|---|---|---|
| Efficiency (full load) | 88.8 – 89.0 % | 88 % |
| Peak Vds | 641 V | 582 V |
| Peak primary current (Ipk) | 1.58 A | 1.6 A |

## Design Inputs

| Parameter | Value |
|---|---|
| Output | 24 V / 2 A (48 W) |
| DC bus | ≈ 235 – 375 V |
| Switching frequency | 100 kHz (fixed) |
| Mode | DCM, peak current mode |
| Target efficiency | 89.3 % |

## Calculation Results

| Quantity | Value |
|---|---|
| Np/Ns | 5.91 → implemented 41 : 7 |
| Lpri | 429 µH (max. 472 µH, ±10 %) |
| Ipk / Irms (primary) | 1.58 A / 0.50 A |
| Isec,pk / Isec,rms | 9.35 A / 3.69 A |
| Duty cycle D | 29.6 % (Vin,min) – 18.4 % (Vin,max) |
| Current-sense resistor Rs | 311 mΩ (limit 2.12 A) |
| Flat-top Vds / peak with snubber | 520 V / 641 V (800 V FET) |
| Diode VPIV | 87 V (×1.3 = 114 V, Vrrm 200 V) |
| Cout / Cin | ≥ 62.2 µF / ≥ 46.8 µF |

The DCM boundary is preserved in the worst case (t3 = 20 % of T).

## Transformer

| Parameter | Value |
|---|---|
| Turns | Np = 41, Ns = 7 (Np,min = 38 @ Bmax 0.35 T) |
| Bpk | 0.32 T (at current limit), 0.22 T (nominal) |
| AL / air gap | 255 nH/turn² / ≈ 0.37 mm |
| Wire diameter | Primary 0.33 mm, secondary 0.90 mm (6 A/mm²) |
| Window utilization | 70 % (Ap = 0.67 cm⁴) |
| Core | ETD29 |

**Core selection.** About 70 % of the available window is used. A smaller core such as EF25 might fit, but this was not tested. It would likely need a larger air gap, which increases fringing flux and can raise leakage inductance and worsen EMI. A hand-wound EF25 would also be much harder to build, so ETD29 was chosen. EMI was considered in the design, but its emission performance has not been measured.

## RCD Snubber

| Parameter | Value |
|---|---|
| Vreset / Vclamp | 145 V / 266 V (k = 1.83) |
| Lleak (assumed) | 2 % of Lpri = 8.6 µH |
| Psn | 2.63 W (5.5 % of Pout) |
| Rsn / Csn | 27 kΩ (≥ 5.3 W) / ≥ 2.2 nF |
| Clamp diode | ≥ 770 V, fast recovery |

The snubber is the largest single loss item. Reducing leakage inductance (winding order) or increasing k lowers the loss, but raises the Vds peak.

**Leakage inductance in practice.** Lower leakage requires tighter winding and the best possible use of the winding area, which in turn needs a suitable bobbin. Such bobbins are hard to source in Turkey. My first winding had about 20 µH of leakage inductance. After re-winding it more tightly, I reduced it to about 8 µH (≈ 2 % of Lpri). Hand winding is demanding.

## Loss Budget and Efficiency

| Item | Vin,min | Vin,max |
|---|---|---|
| FET (conduction + switching + Coss) | 1.33 W | 1.31 W |
| Output diode | 1.20 W | 1.20 W |
| RCD snubber | 2.46 W | 2.46 W |
| Transformer (copper + core) | 0.82 W | 0.73 W |
| Rs, Cout ESR, bias | 0.27 W | 0.24 W |
| **Total loss** | **6.08 W** | **5.93 W** |
| **Efficiency** | **88.8 %** | **89.0 %** |

## Limitations

Leakage inductance and core loss are entered values. The air-gap calculation neglects fringing. Isolation distances and EMI are not covered by these calculations.

## Repository Structure

- `Calculations/` – design calculations
- `Simulation/` – circuit simulation
- `Schematic and PCB/` – schematic and PCB design files
- `Hardware and prototype/` – prototype photos and measurements
- `components datasheet/` – datasheets of the selected components

## References

TI SSZTCW6, TI SSZTCV6, TI SLUP127 (Unitrode Magnetics Design Handbook, §5)
