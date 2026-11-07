---
tags:
  - projekt/command-center
  - thema/luefter
  - typ/plan
aliases:
  - Lüftersteuerung
  - Fan Control
status: phase-b
stand: 2026-11-06
---

# Lüftersteuerung

## Grundsatz
1. **Lesen**
2. **Verifizieren** (am echten Board)
3. **Steuern** (nur verifizierte Kanäle)

Solange Phase B nicht bestanden ist, bleibt die Steuerung gesperrt.

## Phasen
- **A – Erkennung:** FanDiscoveryService, Diagnosebericht (NCT6779D, 29 Sensoren, 5 Kanäle)
- **B – Verifikation:** Schreibtest am Board mit Aufsicht
- **C – Steuerung:** Allowlist, LhmFanController, Rückfall, Tests
- **D – UI:** Karten pro Kanal, Statusbadges, Kurvenauswahl

## Verifizierte Kanäle
- Fan #2: reagiert (40 % → 1261–1546 RPM), Rückfall bestätigt
- Fan #4: reagiert (40 % → 616–912 RPM), Rückfall bestätigt
- Fan #1/#3/#5: keine Reaktion, bleiben unverifiziert

## Sicherheitsregeln
- Mindestwert 30 %
- Maximal 100 %
- Ungültige Werte ablehnen
- Fehlender Temperatursensor → Rückfall
- 3 Fehler → Rückfall
- Dispose/try-finally setzt zurück
- Start immer inaktiv
- Hysterese 2 °C / 3 s
- Kein Schreiben im UI-/Sampler-Thread
- Keine Adminrechte ohne Notwendigkeit

## Zustandsmaschine
Unbekannt → Nur überwacht → Steuerbar (inaktiv) → Aktiv → Rückfall (BIOS)

## Offen (Phase C)
- Allowlist für Fan #2/#4
- Schreibpfad in LhmFanController
- Regel 9 (eigener Hintergrundpfad)
- Absturzverhalten messen