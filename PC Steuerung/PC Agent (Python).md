---
tags:
  - projekt/pc-steuerung
  - thema/python
  - typ/doku
aliases:
  - PC Agent
  - Python Agent
  - FastAPI Agent
status: archiviert
stand: 2026-11-10
---

# PC Agent (Python)

> **Archiviert am 10.11.2026.** Die Python-Version wird nicht mehr weiterentwickelt. Der Code liegt im Archiv-Repo https://github.com/ege16tan/pc-steuerung-python. Aktiv ist der Rust-Agent in `PC Agent Rust/`.

> FastAPI-Dienst auf dem Windows-PC. Prototyp; Rust-Migration auf Branch `rust`.

## Starten
```powershell
cd "PC App"
python -m pip install -r requirements.txt
start.bat
```
- Port 8000, erreichbar im Heimnetz
- Beim ersten Start: Web-Oberfläche `http://localhost:8000/` → Zugangsdaten anlegen

## Sicherheit
- Basic Auth für LAN-Zugriffe; localhost ohne Login
- Passwort als PBKDF2-Hash in `credentials.json` (gitignored)
- Alter `PC_AGENT_TOKEN` als Fallback
- Simulation standardmäßig an

## Endpunkte
Siehe [[API]].

## Log
- `agent.log` mit Datum/Uhrzeit, Rotation nach `agent.log.1`
- `GET /log?lines=200`
- Web-UI: Ereignisse + Verlauf

## Tray
- `start-tray.bat`
- Linksklick: Quick-Action-Popup (Start/Stop, Herunterfahren, Abbrechen, Web-UI)
- Rechtsklick: Menü

## Autostart
```powershell
powershell -ExecutionPolicy Bypass -File .\install-autostart.ps1 -WithTray
```

## Simulation
- `PC_AGENT_SIMULATE=1` (Standard): nichts wird ausgeführt
- `/mode?simulate=true|false` schaltet zur Laufzeit um
- App/Web-UI zeigen den Modus an