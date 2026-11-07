---
tags:
  - projekt/pc-steuerung
  - typ/index
aliases:
  - PCSteuerung
  - PC-Steuerung
  - Projekt PC Steuerung
status: aktiv
stand: 2026-11-06
---

# PC Steuerung

> Android-App zur Steuerung eines Windows-PCs im Heimnetz – Einschalten, Herunterfahren, Ruhezustand, Energieplan und Status.

## Überblick
- **PC-Seite:** FastAPI-Agent (Python, Prototyp); Rust-Migration geplant
- **Handy-Seite:** Kotlin/Jetpack Compose App
- **Kommunikation:** HTTP im lokalen Netz, Basic Auth
- **Wake-on-LAN:** Magic Packet direkt vom Handy
- **Sicherheit:** Simulation als Standard, Echtmodus nur bewusst

## Notizen
- [[Android App]] – Aufbau, Funktionen, Build
- [[PC Agent (Python)]] – Agent, Endpunkte, Tray, Autostart
- [[API]] – Endpunkt-Referenz
- [[Sicherheit]] – Auth, Simulation, Veröffentlichung
- [[Rust-Migration]] – Plan für den Rust-Agenten
- [[Roadmap und Entscheidungen]] – Stand und Gründe
- [[Git und Workflow]] – Repo, Branches, ADB, Push-Schutz

## Schnellstart
1. Agent starten ([[PC Agent (Python)#Starten]])
2. Web-Oberfläche `http://localhost:8000/` öffnen und Zugangsdaten anlegen
3. App bauen/installieren ([[Android App#Bauen]])
4. In der App IP, Port, Benutzername, Passwort, MAC eintragen

## Status
- Android-App: funktional weit, UI überarbeitet
- Agent: Python-Prototyp stabil, Rust auf Branch `rust`
- Offen: App-Icon, Hardware-Tests (WoL/Shutdown), Rust-Umsetzung

## Suchen
- Alle Projektnotizen: `tag:#projekt/pc-steuerung`
- Android: `tag:#thema/android`
- Agent/Python: `tag:#thema/python`
- API: `tag:#thema/api`
- Sicherheit: `tag:#thema/sicherheit`
- Rust: `tag:#thema/rust`
- Planung: `tag:#typ/plan`
- Workflow: `tag:#thema/workflow`
- Rückblick: `tag:#typ/rueckblick`

## Weitere Notizen
- [[Hardware und Netzwerk]] – WoL, Firewall, feste IP
- [[Test-Checkliste]] – Agent, Auth, App, Hardware
- [[Ideen und Backlog]] – Ideen und offene Punkte

## Rückblick
- [[Rückblick]] – was am 2026-11-06 entstanden ist, inklusive Echtmodus-Vorfall und Lehren
