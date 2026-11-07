---
tags:
  - projekt/pc-steuerung
  - thema/test
  - typ/checkliste
aliases:
  - Test
  - Testplan
  - Checkliste
status: aktiv
stand: 2026-11-06
---

# Test-Checkliste

## Vorbereitung
- [ ] Agent läuft (Port 8000)
- [ ] Zugangsdaten eingerichtet
- [ ] Handy im selben WLAN
- [ ] PC per Ethernet

## Agent (curl)
- [ ] GET /status → 200
- [ ] POST /ping
- [ ] POST /shutdown?delay=60
- [ ] POST /abort
- [ ] POST /hibernate (nur Echtmodus)
- [ ] POST /power-plan?name=saver
- [ ] POST /wake?mac=...
- [ ] GET /events
- [ ] GET /log?lines=200
- [ ] POST /mode?simulate=true/false

## Auth
- [ ] LAN ohne Login → 401
- [ ] LAN mit Login → 200
- [ ] localhost ohne Login → 200
- [ ] POST /setup von LAN → 403

## App
- [ ] Status grün
- [ ] Herunterfahren-Dialog + Standard-Timer
- [ ] Timer (1/2/4/8 h + custom)
- [ ] Abbrechen bei laufendem Countdown
- [ ] Energieplan-Wechsel + aktive Markierung
- [ ] Widget: Einschalten direkt, Herunterfahren mit Bestätigung
- [ ] Verlauf + Aktualisieren
- [ ] Verbindung testen

## Hardware
- [ ] WoL aus dem ausgeschalteten Zustand
- [ ] Shutdown im Echtmodus (mit Aufsicht)
- [ ] Ruhezustand
- [ ] Firewall von außen blockt

## Sicherheit
- [ ] Keine Secrets im Repo
- [ ] Pre-Push-Hook aktiv
- [ ] Echtmodus nur bewusst