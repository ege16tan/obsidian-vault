---
tags:
  - projekt/pc-steuerung
  - thema/rust
  - typ/doku
aliases:
  - PC Agent (Rust)
  - Rust Agent
  - Agent starten
  - pc-agent
status: aktiv
stand: 2026-11-11
---

# PC Agent (Rust)

> Der Agent ist die **einzige** PC-Version. Er läuft **ohne Fenster** im Hintergrund;
> bedient wird er über das Tray-Symbol oder die Web-UI. Einen GUI-Launcher gibt es
> bewusst nicht mehr (v0.9.5).

## Starten

| Weg | Befehl | Ergebnis |
|---|---|---|
| Normal | `start-agent.bat` | Agent im Hintergrund **+ Tray-Symbol automatisch** |
| Nur Tray | `start-tray.bat` | Tray-Symbol (Mutex verhindert Duplikate) |
| Debug | `start-agent-debug.bat` | Agent in sichtbarer Konsole + `agent.log` live |
| Autostart | `PC Agent Rust\install-autostart.ps1 [-WithTray]` | Aufgabenplanung, „bei Anmeldung" |
| Stoppen | `powershell -File agent-manager.ps1 -Action stop` | Agent beendet, Port 8000 frei |

Der Manager (`agent-manager.ps1`) baut bei vorhandenem Cargo automatisch neu, tauscht die
EXE sicher aus, prüft Port 8000 und startet jetzt **immer** das Tray mit
(`Start-TrayIfNeeded`, Mutex `Local\PCSteuerungTray`).

## Daten und Konfiguration

| Sache | Ort / Variable |
|---|---|
| Datenordner | `%LOCALAPPDATA%\PC Steuerung` (per `PC_AGENT_DATA` überschreibbar) |
| Zugangsdaten | `credentials.json` (PBKDF2-Hash, gitignored) |
| Token (Fallback) | `token.json` |
| Geräte-Kopplung | `device.json` |
| Logs | `agent.log` (+ `agent.log.1`), Manager: `agent-manager.log` |
| Port | `PC_AGENT_PORT` (Standard 8000) |
| Simulation | `PC_AGENT_SIMULATE` (`0` = echt, jetzt Standard im Manager; `1` = nur loggen) |
| WoL-Broadcast | `PC_WOL_BROADCAST` (Standard `192.168.1.255`) |

> [!note] Simulationsschalter
> Der Manager setzt `PC_AGENT_SIMULATE=0`, wenn die Variable fehlt – **echte Aktionen sind
> Standard**. Simulation einschalten mit `PC_AGENT_SIMULATE=1` oder `POST /mode?simulate=true`.

## Agent läuft immer

Damit das Handy jederzeit verbinden kann, sind zwei Dinge eingerichtet (12.11.2026):

1. **Autostart:** `PC Agent Rust\install-autostart.ps1 -WithTray` registriert
   `PC Steuerung Agent` und `PC Steuerung Tray` in der Aufgabenplanung (bei Anmeldung).
2. **Tray-Watchdog:** `tray.ps1` prüft alle 2,5 s; ist der Agent gestoppt, startet es ihn
   automatisch (höchstens alle 20 s). Abschaltbar über das Tray-Menü
   „Agent automatisch starten".

Prüfen: `curl http://127.0.0.1:8000/status` → `{"online":true,...,"simulate":false}`.

## Endpunkte

Siehe [[API]]. Kurz: `/status`, `/shutdown?delay=`, `/abort`, `/hibernate`, `/power-plan`,
`/wake?mac=&port=`, `/events`, `/log?lines=`, `/ping`, `/setup-status`, `/pairing-info`,
`/pairing-qr.svg`, `/setup`, `/mode?simulate=`, `/` (Web-UI).

## Log lesen

```powershell
Get-Content "$env:LOCALAPPDATA\PC Steuerung\agent.log" -Tail 40 -Wait
```

Im Debug-Start passiert genau das automatisch nach dem Start.

## Verwandt

- [[PC Steuerung/PC Steuerung|Projektübersicht]] · [[PC Steuerung/API|API]] · [[PC Steuerung/Sicherheit|Sicherheit]]
- [[PC Steuerung/PC Agent (Python)|Python-Vorgänger (archiviert)]] · [[PC Steuerung/Rust-Migration|Rust-Migration]]
