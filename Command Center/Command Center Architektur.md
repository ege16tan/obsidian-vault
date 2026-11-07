---
tags:
  - projekt/command-center
  - thema/architektur
  - typ/doku
aliases:
  - HCC Architektur
  - Architektur
status: aktiv
stand: 2026-11-06
---

# Command Center Architektur

## Schichten
- **HCC.Core:** Modelle + Interfaces, keine Abhängigkeiten
- **HCC.Hardware:** einziger Layer mit LibreHardwareMonitor
- **HCC.Infrastructure:** Sampling, Fan-Curve, Config, Fan-Control-State-Machine
- **HCC.App:** WPF Views + ViewModels, keine Hardwarezugriffe
- **tests/HCC.Tests:** xUnit mit Fakes

## Regeln
- Kein Hardware-Zugriff aus Views/ViewModels
- Kein Polling außerhalb des gemeinsamen MetricsSampler
- Alle LHM-Schreibaufrufe nur in LhmFanController
- Kleinstmögliche Lösung, bestehende Muster (MVVM, DI, JSON)
- Tests ohne echte Hardware

## Hardware-Safety
- Kanal erst nach dokumentiertem Phase-B-Test als verifiziert markieren
- Nie standardmäßig schreiben
- Ungültige Werte nie an Write
- Reset in try/finally und Dispose
- Keine Abschwächung von Secure Boot/HVCI/Blocklist

## Build
```powershell
dotnet restore ./HardwareControlCenter.sln
dotnet build ./HardwareControlCenter.sln
dotnet test ./HardwareControlCenter.sln
```