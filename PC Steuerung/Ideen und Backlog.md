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

## Agent
- [x] Automatisierte Tests (cargo test, 20 grün)
- [ ] HTTP-Endpunkt-Tests für den Rust-Agenten
- [ ] CI (GitHub Actions)
- [ ] Routinen statt Zeitplan (siehe [[Routinen]])
- [ ] Home-Assistant-/Tasker-Integration
- [x] Mehrere PCs (Profile)
- [x] Rust-Migration ([[Rust-Migration]])

## Ideen
- [ ] Benachrichtigung bei Statuswechsel
- [ ] Sprachsteuerung
- [ ] Energieverbrauch-Anzeige
- [ ] Backup/Restore der Einstellungen