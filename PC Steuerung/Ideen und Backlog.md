---
tags:
  - projekt/pc-steuerung
  - thema/ideen
  - typ/backlog
aliases:
  - Ideen
  - Backlog
status: offen
stand: 2026-11-06
---

# Ideen und Backlog

## App
- [x] Eigenes App-Icon + finales Design
- [ ] Texte nach strings.xml (Navigation/Status/Einstellungen erledigt, Timer-/Dialog-Texte offen)
- [x] Pull-to-refresh
- [x] App-Shortcuts
- [ ] Widget-Status live (WorkManager) – **entfällt**, Widget wurde v0.9.5 entfernt
- [x] Drei-Zustands-Status (gelb = Agent antwortet nicht) · „PC aus" jetzt rot
- [x] Passwort verschlüsselt (AndroidKeystore AES/GCM)
- [ ] Screenshots fürs README (Gerät: SM-A556B)
- [ ] Markt+ ausbauen (Detailseiten, Update-Hinweis pro Modul)
- [ ] Web-UI aus der App an/aus schalten (Agent-Endpunkt nötig)
- [x] **Game Optimization als Plugin** (13.11.2026): Plugin-Host, Spiele-Scan, Web-UI-Tab, RAM-Bereinigung nach Mem-Reduct-Vorbild ([[Neue Ideen - gleiche App]])
- [ ] **Android-App anbinden**: Plugin aus der App starten/beenden
- [ ] **Downloads im Blick**: read-only, laufend + zuletzt fertig
- [ ] Kill-Liste je Spiel statt global
- [ ] Entscheiden: eigener Tab „Werkzeuge" oder Module weiter unter „Steuerung"
- [ ] **Downloads im Blick** als Modul (`downloads`): read-only, laufend + zuletzt fertig
- [ ] Kill-Liste in den Einstellungen pflegen (Allowlist bekannter Prozessnamen)
- [ ] Entscheiden: eigener Tab „Werkzeuge“ oder Module weiter unter „Steuerung“

## Agent
- [x] Automatisierte Tests (cargo test, 20 grün)
- [ ] HTTP-Endpunkt-Tests für den Rust-Agenten
- [ ] CI (GitHub Actions)
- [ ] Routinen statt Zeitplan (siehe [[Routinen]])
- [ ] Home-Assistant-/Tasker-Integration
- [x] Mehrere PCs (Profile)
- [x] Rust-Migration ([[Rust-Migration]])
- [ ] Kimi-Review abarbeiten: Port zentral, CSRF für localhost, `pc-agent.exe`-Subkommandos ([[Kimi Review (2026-11-13)]])

## Ideen
- [ ] Benachrichtigung bei Statuswechsel
- [ ] Sprachsteuerung
- [ ] Energieverbrauch-Anzeige
- [ ] Backup/Restore der Einstellungen