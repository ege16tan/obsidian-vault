---
tags:
  - projekt/pc-steuerung
  - thema/api
  - typ/referenz
aliases:
  - API
  - Endpunkte
  - REST API
status: aktiv
stand: 2026-11-06
---

# API

Basis: `http://<pc-ip>:8000`

## Auth
- LAN: HTTP Basic Auth (Benutzername/Passwort)
- localhost: kein Login
- Fallback: `X-Token` (alter Token)

## Endpunkte
| Methode | Pfad | Zweck |
|---|---|---|
| GET | `/status` | online, simulate, power_plan, pending_shutdown, setup_required |
| POST | `/shutdown?delay=` | Herunterfahren (0–86400 s) |
| POST | `/abort` | Abbrechen |
| POST | `/hibernate` | Ruhezustand |
| POST | `/power-plan?name=high\|balanced\|saver` | Energieplan |
| POST | `/wake?mac=&port=` | Magic Packet |
| GET | `/ping` | Verbindungstest |
| GET | `/events` | letzte 50 Ereignisse |
| GET | `/log?lines=` | Logdatei (Klartext) |
| GET | `/setup-status` | Setup nötig? |
| POST | `/setup` | Zugangsdaten (nur localhost) |
| POST | `/mode?simulate=` | Simulation/Echtzeit |
| GET | `/plugins` | Plugins mit Status (`available`, `running`, `pid`, `uptime`, `tail`) |
| POST | `/plugins/{id}/start?game=&auto=` | Plugin starten (nur auf Anforderung; `auto=1` = laufendes Spiel erkennen) |
| POST | `/plugins/{id}/stop` | Plugin beenden (setzt den Energieplan zurück) |
| GET | `/plugins/{id}/output?lines=` | Ausgabe des Plugins (Klartext) |
| GET | `/games` | zuletzt gescannte Spiele, inkl. `running` |
| POST | `/games/scan` | Spiele suchen (Steam, Epic, GOG, eigene Ordner) |
| GET | `/game-config` | Kill-Liste, Suchordner, Automatik |
| POST | `/game-config` | Profil speichern (JSON) |


## /status Beispiel
```json
{"online":true,"host":"PC","simulate":true,"power_plan":"high","pending_shutdown":0,"setup_required":false}
```

## Fehler
- 401: Login fehlt/falsch
- 403: Zugriff verweigert
- 422: ungültige Parameter