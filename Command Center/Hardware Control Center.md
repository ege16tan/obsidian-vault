---
tags:
  - projekt/command-center
  - thema/dotnet
  - typ/doku
aliases:
  - HCC
  - Hardware Control Center
status: aktiv
stand: 2026-11-06
---

# Hardware Control Center

> Windows-Desktop-App für Live-Hardware-Monitoring, Sensordiagnose und stufenweise Lüftersteuerung.

## Stack
- C# / .NET 10, WPF, MVVM Toolkit
- LibreHardwareMonitor 0.9.6
- PawnIO 2.0+ (signierter Kernel-Treiber)
- LiveCharts2

## Highlights
- Live CPU/GPU/RAM/Temperaturen/Takte/Spannungen/Leistung/Lüfter
- Hardware- und Sensor-Inventar inkl. Super-I/O
- Dashboard mit lokalem Verlauf
- Benannte Aufzeichnungssitzungen
- Monitoring-Profile
- Lüfterdiagnose + Kurveneditor (Profile immer inaktiv)
- Temperaturwarnungen
- Dark/Light Theme
- PawnIO-Diagnose

## Zielsystem (verifiziert)
- MSI H87-G43 (MS-7816)
- Nuvoton NCT6779D, 29 Sensoren
- Intel Core i7-4770
- NVIDIA GeForce GTX 1060 6GB
- Windows 11 x64, HVCI

## Voraussetzungen
- Windows 10/11 x64
- .NET 10 SDK (global.json: 10.0.403)
- PawnIO 2.0+
- Administratorrechte

## Lokale Daten
`%LOCALAPPDATA%/HardwareControlCenter`:
- settings.json, alerts.json, theme.json
- history.jsonl (max. 7200 Einträge)
- fan-curves.json
- sessions/ (max. 20 Sitzungen)

## Projektstruktur
- src/HCC.App – WPF, ViewModels, Theme, DI
- src/HCC.Core – Modelle/Interfaces
- src/HCC.Hardware – LHM, Fan-Discovery, PawnIO
- src/HCC.Infrastructure – Sampling, Fan-Service, Config
- tests/HCC.Tests – xUnit, Fakes
- docs/ – Verifikation, Treiber, UI

## Sicherheit
- Lüftersteuerung standardmäßig **gesperrt**
- Lesen → Verifizieren → Steuern
- BIOS-Automatik als Sicherheitsnetz
- Keine Abschwächung von Secure Boot/HVCI/Driver-Blocklist