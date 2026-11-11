---
tags:
  - projekt/pc-steuerung
  - typ/index
aliases:
  - PCSteuerung
  - PC-Steuerung
  - Projekt PC Steuerung
status: aktiv
stand: 2026-11-10
---

# PC Steuerung

> Android-App zur Steuerung eines Windows-PCs im Heimnetz – Einschalten, Herunterfahren, Ruhezustand, Energieplan und Status.

## Überblick
- **PC-Seite:** Rust-Agent als einzelne EXE (`PC Agent Rust/`), Rust-only
- **Handy-Seite:** Kotlin/Jetpack Compose App
- **Kommunikation:** HTTP im lokalen Netz, Basic Auth
- **Wake-on-LAN:** Magic Packet direkt vom Handy (optional, MAC aus QR oder automatisch)
- **Sicherheit:** Simulation als Standard, Echtmodus nur bewusst
- **Archiv:** Python-Version eingestellt: https://github.com/ege16tan/pc-steuerung-python

## Notizen
- [[Android App]] – Aufbau, Funktionen, Build, Versionshistorie
- [[PC Agent (Rust)]] – Agent starten/tray/debug, Datenordner, Endpunkte
- [[PC Agent (Python)]] – archivierter Vorgänger (Referenz)
- [[API]] – Endpunkt-Referenz
- [[Sicherheit]] – Auth, Simulation, Veröffentlichung
- [[Rust-Migration]] – abgeschlossene Migration
- [[Roadmap und Entscheidungen]] – Stand und Gründe
- [[Git und Workflow]] – Repo, Branches, ADB, Push-Schutz
- [[Routinen]] – geplantes Wenn-Dann-Feature (ersetzt den Zeitplan)
- [[UI und Design]] – Design-Entscheidungen und UI-Backlog

## Schnellstart
1. Agent starten: `start-agent.bat` (Agent + Tray im Hintergrund) – Debug-Konsole: `start-agent-debug.bat`
2. Web-Oberfläche `http://localhost:8000/` öffnen und den Einrichtungs-Assistenten durchgehen (PC-Name, optional WoL, Zugangsdaten)
3. App bauen/installieren ([[Android App#Bauen]])
4. In der App beim Erststart „Mit QR-Code einrichten“ wählen und den Code scannen (oder manuell IP, Port, Zugangsdaten eintragen)

## Status
- **App v0.9.5** (versionCode 16): Widget entfernt, Setup verlassbar, Markt+ aufgewertet, Energieplan-Animation, weicher Theme-Wechsel, Texte in `strings.xml`
- **Agent:** Rust-only, einzelne EXE, **kein GUI-Fenster**; Tray startet automatisch mit, Debug-Konsole verfügbar
- **Modus:** Echt ist Standard, Simulation ist die bewusste Ausnahme (Banner)
- Datenordner: `%LOCALAPPDATA%\PC Steuerung`; alte `PC App/`-Daten werden übernommen
- Offen: Live-Test Scanner/WoL auf dem Handy, [[Routinen]], Rust-HTTP-Tests + CI

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
