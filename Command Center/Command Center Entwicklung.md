---
tags:
  - projekt/command-center
  - thema/workflow
  - typ/doku
aliases:
  - HCC Entwicklung
  - Entwicklung
status: aktiv
stand: 2026-11-06
---

# Command Center Entwicklung

## Voraussetzungen
- Windows 10/11 x64
- .NET 10 SDK (global.json: 10.0.403)
- PawnIO 2.0+ (für Board-/CPU-Sensoren)
- Adminrechte für die App

## Build & Test
```powershell
dotnet restore ./HardwareControlCenter.sln
dotnet build ./HardwareControlCenter.sln
dotnet test ./HardwareControlCenter.sln
```

## Start
```powershell
dotnet run --project ./src/HCC.App/HCC.App.csproj
```

## Regeln
- Topic-Branch + PR gegen main
- Conventional Commits: feat/fix/docs/test/chore/refactor
- PR fokussiert halten
- Doku aktualisieren bei Verhaltens-/Verifikationsänderungen
- Keine Secrets, lokalen Pfade oder persönliche Daten

## Checkliste
- [ ] dotnet build
- [ ] dotnet test
- [ ] Kein Schreibpfad ohne Board-Test
- [ ] Doku aktualisiert