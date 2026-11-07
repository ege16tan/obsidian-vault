---
tags:
  - projekt/pc-steuerung
  - thema/roadmap
  - typ/plan
aliases:
  - Roadmap
  - Entscheidungen
  - Stand
status: aktiv
stand: 2026-11-07
---

# Roadmap und Entscheidungen

## Erledigt
- **v0.7.0 (07.11.2026): „Android besser"** released – Status-Polling nur im Vordergrund, verschlüsselte Passwörter (AndroidKeyStore AES/GCM mit Migration), R8-Minifikation, 32 Unit-Tests, 1,2-MB-APK
- **v0.6.0 (07.11.2026): Marktplatz „Markt+"** als 4. Tab released (Module Timer/Verlauf installierbar, Remote-Katalog aus GitHub mit lokalem Fallback) – APK auf GitHub Releases online
- Mehrere PCs (Profile)
- Android-App: Status, Shutdown, Abort, Ruhezustand, Energieplan, Timer
- WoL direkt vom Handy
- Homescreen-Widget
- Basic Auth statt Token
- Web-UI mit Ersteinrichtung
- Tray mit Quick-Action-Popup
- Autostart-Skripte
- Debug-Umschalter Simulation/Echtzeit
- UI-Verbesserungen (Bottom-Navigation, Snackbar, Validierung)

## Geplant
- **Rust-Agent live bringen**: SAC-Ausschaltung + Neustart, dann `cargo test`, Release-Build, Live-Test ([[Rust-Migration]])
- **Repo später archivieren**: davor Remote-Katalog außerhalb des Repos hosten (archiviertes Repo = Katalog friert ein; Umzug braucht ein App-Update mit neuer `CATALOG_URL`)
- App-Icon und finales Design
- Automatisierte Tests + CI
- Screenshots
- App-Shortcuts
- Zeitplan (täglich)
- Home-Assistant-/Tasker-Integration

## Entscheidungen
- **Nur Heimnetz**, kein Internet/VPN/Port-Forwarding
- **WoL direkt vom Handy** (nicht über den Agenten)
- **Basic Auth** statt Token (Token nur Fallback)
- **Simulation als Standard**, Echtmodus bewusst
- **Web-UI-Ersteinrichtung** (nur localhost)
- **PC-Agent in Rust, Android-App bleibt Kotlin** (kein iOS/KMP; ViewModel & Baseline Profiles abgelehnt)
- **Rust lokal in `PC Agent Rust/`** auf main (kein Branch nötig, Drop-in-Ziel)
- **Repo wird später archiviert** – vorher Katalog-Umzug planen
- **SAC wird dauerhaft ausgeschaltet** (Windows blockierte unsignierte Builds, OS-Fehler 4551)