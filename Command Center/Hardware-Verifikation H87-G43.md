---
tags:
  - projekt/command-center
  - thema/hardware
  - typ/bericht
aliases:
  - Verifikation H87-G43
  - Phase B
  - Hardware-Verifikation
status: bestanden
stand: 2026-11-06
---

# Hardware-Verifikation MSI H87-G43

- **Datum:** 2026-11-02
- **Board:** MSI H87-G43 (MS-7816)
- **CPU:** Intel Core i7-4770
- **GPU:** NVIDIA GeForce GTX 1060 6GB
- **Tool:** LHM 0.9.6, .NET 10.0.403, PawnIO 2.2.0

## Ergebnis
- Phase A (Lesen): bestanden
- Phase B (Schreiben): bestanden für Fan #2 + #4

## Messung
| Gerät | Typ | Sensoren |
|---|---|---|
| MSI H87-G43 | Motherboard | 0 (am Super-I/O) |
| Nuvoton NCT6779D | SuperIO | 29 |
| Intel i7-4770 | CPU | 35 |
| Virtual Memory | Memory | 3 |
| Total Memory | Memory | 3 |
| GTX 1060 | GpuNvidia | 30 |

## Lüfterkanäle
| Kanal | RPM | Steuerwert | Status |
|---|---|---|---|
| Fan #1 | 0 | 84,7 % | unverifiziert |
| Fan #2 | 2116 | 80,0 % | verifiziert |
| Fan #3 | 0 | 60,0 % | unverifiziert |
| Fan #4 | 1133 | 100,0 % | verifiziert |
| Fan #5 | 0 | 60,0 % | unverifiziert |
| GPU (NVAPI) | 1454 | 59,0 % | Kandidat |

## Phase B (40 %)
- Fan #2: 1802 → 1261–1546 RPM → 1847 RPM (Rückfall)
- Fan #4: 1086 → 616–912 RPM → 875 RPM (Rückfall)
- Fan #1/#3/#5: keine Reaktion
- CPU Package: 54–60 °C

## Offene Punkte
- Anschlüsse zuordnen (Fan #2 = CPUFAN?)
- Fan #1/#3/#5 klären
- `Temperature #6` (91 °C) unplausibel → keine Alarmquelle
- Absturzverhalten messen