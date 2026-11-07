---
tags:
  - projekt/command-center
  - thema/todo
  - typ/plan
aliases:
  - HCC TODO
  - Command Center TODO
status: offen
stand: 2026-11-06
---

# Command Center TODO

## P0 – Sicherheit/Hardware (nur mit Aufsicht)
- [ ] Phase C: Schreibpfad mit Allowlist (Fan #2/#4)
- [ ] Absturzverhalten messen
- [ ] Anschlüsse zuordnen
- [ ] Fan #1/#3/#5 klären
- [ ] Unplausible Sensoren aus Alarmen nehmen (Temperature #6)

## P1 – Produktfunktionen
- [ ] Logging/Fehlerdiagnose (DiagnosticLog + Rotation + Export)
- [ ] Aufzeichnung entkoppeln (Hintergrund-Queue)
- [ ] Alarm-Hysterese
- [ ] Autostart (HKCU Run)
- [ ] GPU-Auslastung über Windows-GPU-Engine-Zähler

## P2 – Qualität
- [ ] Sensorliste in-place aktualisieren (ItemsControl)
- [ ] FanControlService verdrahten oder entfernen
- [ ] Aufzeichnung umbenennen
- [ ] Flaky Test entschärfen
- [ ] Dateigrößen begrenzen (50 MB)
- [ ] Build-Hygiene (NU1701, TreatWarningsAsErrors)
- [ ] CI erweitern (Coverage)
- [ ] Barrierefreiheit

## P3 – Doku
- [ ] Claude-Handoff/ archivieren
- [ ] DEEPSEEK_IMPLEMENTATION_PLAN.md archivieren
- [ ] .OLD/README.md prüfen
- [ ] UI_DESIGN_GUIDE ergänzen
- [ ] Widerspruch im Verifikationsbericht glätten
- [ ] README aufwerten (Screenshot + Release Notes)

## Empfohlene Reihenfolge
1. Logging
2. Aufzeichnungen verwalten/entkoppeln
3. Sensorliste in-place
4. Temperaturfilter + unplausible Sensoren
5. Phase C (mit Aufsicht)