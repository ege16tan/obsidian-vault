---
tags:
  - projekt/pc-steuerung
  - typ/rueckblick
aliases:
  - Rückblick
  - Retrospektive
  - Projektverlauf
  - Historie
  - Tagebuch
status: aktiv
stand: 2026-11-06
---

# Rückblick – 2026-11-06

> Was an diesem Tag aus „PC Steuerung" geworden ist – ehrlich, inklusive Fehlern.

## Ausgangslage

Die Idee: Das Handy soll den Windows-PC im Heimnetz steuern – **einschalten, herunterfahren, Ruhezustand, Energieplan, Status**. Kein Cloud-Dienst, kein Port-Forwarding, alles lokal.

Zu Beginn war das ein Prototyp mit vielen offenen Enden. Am Ende des Tages war es ein benutzbares Projekt mit sauberem Repo und Dokumentation.

## Was entstanden ist

### Agent (PC-Seite)
- Wake-on-LAN-Endpunkt `/wake`
- **Basic Auth** mit Benutzername/Passwort statt rohem Token
- Ersteinrichtung über die Web-Oberfläche (`/setup`, nur localhost)
- Laufzeit-Umschalter Simulation ↔ Echtmodus (`/mode`)
- Persistentes Log `agent.log` + `/log`-Endpunkt
- Energieplan-Anzeige auch im Simulationsmodus
- Neu gestaltete Web-Oberfläche
- Tray mit Quick-Action-Popup (Start/Stop, Herunterfahren, Abbrechen, Web-UI)
- Autostart-Skripte

### Android-App
- Bottom-Navigation: Steuerung, Energie, Einstellungen
- Ein dynamischer Power-Button (online → Herunterfahren, offline → Einschalten)
- Herunterfahren-Dialog mit **konfigurierbarem Standard-Timer** (Sofort/10/30/60 s) plus Presets (1/2/4/8 h) und eigener Zeit
- Basic Auth in der App
- **WoL direkt vom Handy** (Magic Packet per `DatagramSocket`)
- Homescreen-Widget: Einschalten direkt, Herunterfahren nur mit Bestätigung
- Snackbar-Feedback, gesperrte Buttons während Aktionen
- Verbindungstest, MAC-/Port-Validierung
- Passwort-Sichtbarkeit und Wiederholung bei der Einrichtung
- ECHT-Modus-Warnung, Umlaute, robustes JSON-Parsing

### Sicherheit und Repo
- Privates GitHub-Repo bereinigt (Historie neu geschrieben)
- `SECURITY.md` mit Veröffentlichungs-Checkliste
- `ROADMAP.md`, MIT-`LICENSE`, `docs/planung.md`
- Pre-Push-Hook + GitHub Action (`scripts/check-sensitive.sh`)
- `credentials.json`, `agent.log`, Build-Artefakte und `opencode.json` bleiben draußen

### Dokumentation
- Obsidian: Projektordner mit Tags, Aliases und Index
- 11 Notizen für PC Steuerung, 7 für Command Center, 1 Projektübersicht
- Diese Rückblick-Notiz als Gedächtnis

## Der Vorfall (wichtig!)

Beim Testen wurde der **Echtmodus aktiviert** und versehentlich ein echter Shutdown ausgelöst. Der Countdown konnte rechtzeitig mit `/abort` gestoppt werden – der PC blieb an. Danach wurde der Agent **sofort wieder auf Simulation** gestellt.

**Lehre daraus:**
- Echtmodus nie „nur kurz" einschalten, ohne den Finger am Abbrechen zu haben.
- Vor jedem echten Test: Countdown lang genug wählen (mind. 60 s).
- Simulation bleibt der Standard – auch für Screenshots und Dokumentation.
- Der Abbrechen-Button muss immer erreichbar sein, nicht hinter einem Dialog versteckt.

## Entscheidungen, die geblieben sind

- **Nur Heimnetz** – kein Internet, kein VPN, kein Port-Forwarding
- **WoL direkt vom Handy** – funktioniert auch, wenn der Agent nicht läuft
- **Basic Auth** statt Token – Token bleibt nur Fallback
- **Simulation als Standard** – Echtmodus ist ein bewusster Debug-Schalter
- **Web-UI-Ersteinrichtung** – nur von localhost erreichbar
- **Eigener Rust-Branch** – Python bleibt Referenz, bis Rust nachweislich kompatibel ist

## Was gut lief

- Schrittweises Bauen mit sofortigem Testen (Build → Install → echte Requests)
- Sicherheitsentscheidungen früh statt nachträglich
- Ein Ort für Doku (Obsidian) statt verstreuter Notizen
- Klare Trennung: Agent = PC, App = Handy, WoL = direkt

## Was holprig lief

- Echtmodus-Test ohne ausreichende Absicherung (siehe Vorfall)
- Einige UI-Texte liegen noch im Code statt in `strings.xml`
- Widget-Status nur aktuell, solange die App läuft
- Passwort noch im Klartext in SharedPreferences (nur lokales Debug)
- Hardware-Tests (WoL aus dem Off, echter Shutdown, Ruhezustand, Firewall) stehen noch aus

## Offen für das nächste Mal

- [ ] App-Icon und finales Design
- [ ] Texte nach `strings.xml`
- [ ] Verschlüsselte Passwort-Speicherung
- [ ] Pull-to-refresh
- [ ] Gelber Zwischenstatus (Agent antwortet nicht)
- [ ] Widget-Status live über WorkManager
- [ ] Automatisierte Agent-Tests + CI
- [ ] Hardware-Tests am echten PC
- [ ] Rust-Migration (Branch `rust`)

## Wenn du in drei Monaten zurückkommst

1. [[PC Steuerung]] lesen – Überblick und Links.
2. [[Roadmap und Entscheidungen]] lesen – warum es so ist, wie es ist.
3. Diese Notiz lesen – was passiert ist, inklusive des Echtmodus-Vorfalls.
4. [[Test-Checkliste]] nehmen und den aktuellen Stand prüfen.
5. Erst dann ändern.

> **Merksatz:** Lesen → Verifizieren → Steuern. Das gilt für Lüfter im Command Center genauso wie fürs Herunterfahren hier.
