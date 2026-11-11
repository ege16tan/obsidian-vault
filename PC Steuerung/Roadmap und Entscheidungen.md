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
stand: 2026-11-10
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

- **v0.8.0 (08.11.2026):** In-App-Updater über GitHub Releases; APK-Download mit Prüfsummenprüfung, Installation über Android-Systemdialog oder optional Shizuku. Release: https://github.com/ege16tan/pc-steuerung/releases/tag/v0.8.0

- **v0.9.0 (08.11.2026):** Python und Rust bleiben verfügbar; Rust ist Standard. Versionswechsel über Web-UI oder gemeinsames Tray. Agent-Manager, Start/Stopp, Port-Konfliktschutz und gemeinsame Autostart-Steuerung. Release: https://github.com/ege16tan/pc-steuerung/releases/tag/v0.9.0

- **v0.9.1 (08.11.2026):** Manager baut und tauscht bei vorhandenem Cargo die aktuelle Rust-EXE sicher aus; ein vorhandenes altes Binary blockiert die Web-UI nicht mehr. Release: https://github.com/ege16tan/pc-steuerung/releases/tag/v0.9.1

- **v0.9.2 (08.11.2026):** Android-Einstellungen als gruppierte Kartenübersicht mit Detailseiten neu gestaltet; System- und Shizuku-Installation bleiben verfügbar. Release: https://github.com/ege16tan/pc-steuerung/releases/tag/v0.9.2

- **Rust-Agent 0.1.0 (09.11.2026):** Release-Build kompiliert; wichtige Verhaltensunterschiede zur Python-Version angeglichen. Als Vorab-Release veröffentlicht: https://github.com/ege16tan/pc-steuerung/releases/tag/rust-v0.1.0. Noch kein Live-Gegencheck.

- **QR-Einrichtung (in Arbeit, uncommitted):** Schritt-Assistent in der Web-Oberfläche (PC-Name → WoL mit Adapter-Suche → Anmeldung → QR) und Android-Erststart-Assistent (QR-Scan oder manuell mit Verbindungstest). Neue Endpunkte `/pairing-info`, `/pairing-qr.svg`, `/network-adapters`, `/device-setup` in Rust und Python; Token-Anmeldung als Alternative.

- **Scanner-Fix v0.9.4 (09.11.2026):** zxing/Camera1 scheiterte auf Android 17 (Samsung SM-A556B); ersetzt durch CameraX + ML Kit. QR-Scan jetzt auch im Profil-Editor der Einstellungen.

- **Rust-only (10.11.2026):** Python-Agent ins Archiv-Repo https://github.com/ege16tan/pc-steuerung-python ausgelagert und archiviert; `PC App/` entfernt. Manager, Tray, Web-UI und Installer ohne Versionswechsel. Datenordner `%LOCALAPPDATA%\PC Steuerung`. Launcher `PC Steuerung.exe` und Installer 0.9.4 gebaut.
- **v0.9.4 WoL-Schritt (10.11.2026):** Android-Assistent fragt Wake-on-LAN optional ab (Standard aus), übernimmt die MAC aus dem QR-Code oder holt sie über `/pairing-info`. Release-APK gebaut, Live-Test auf dem Handy offen.

- **v0.9.5 (11.11.2026):** Widget und GUI-Launcher entfernt (Agent rein im Hintergrund, Tray automatisch), Debug-Konsole `start-agent-debug.bat`, Simulations-Banner statt Echt-Warnung (Echt ist Standard), Statuspunkt „PC aus" rot, Setup verlassbar, Markt+ mit Basis-Chips/Version-Badges, Energieplan-Animation, weicher Theme-Wechsel, UI-Texte in `strings.xml`, [[Credits]]. Release: https://github.com/ege16tan/pc-steuerung/releases/tag/v0.9.5

## Geplant
- **Live-Test auf dem Handy:** Scanner (CameraX/ML Kit) und WoL-Schritt real prüfen (0.9.5)
- **[[Routinen]]** statt Zeitplan: Auslöser + Aktionen, PC bleibt wach/entsperrt (siehe [[Offene Fragen]])
- **Markt+ ausbauen:** Detailseiten, Update-Hinweis pro Modul
- **Web-UI aus der App an/aus** (braucht Agent-Endpunkt)
- Repo später archivieren (vorher Katalog-Umzug)
- Finales Design polieren, Screenshots fürs README
- Rust-Agent: HTTP-Endpunkt-Tests + CI

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
- Android zunächst im kanonischen Repo zusammen mit Rust lassen: Updater und Modulkatalog zeigen auf `ege16tan/pc-steuerung`; ein eigenes Android-Repo würde beide URLs und den Release-Workflow erfordern.
- **Kein GUI-Launcher, kein Homescreen-Widget** (11.11.2026): Der Agent läuft ausschließlich im Hintergrund (Tray + Web-UI); das Widget war nie gewollt und wurde entfernt.
- **Tray ist Pflicht** (11.11.2026): Jeder Agent-Start startet das Tray automatisch (`Start-TrayIfNeeded`, Mutex), damit nie wieder „Agent läuft, aber kein Symbol" passiert.
- **Echt ist Standard, Simulation ist die Ausnahme** (11.11.2026): Die App warnt nur noch im Simulationsfall.
- **Zeitplan → Routinen** (11.11.2026): Statt fester Uhrzeit gibt es Wenn-Dann-Routinen ([[Routinen]]).
- **Credits sind Pflicht** (11.11.2026): Fremdcode, Bibliotheken und Agent-Skills werden in [[Credits]] und `CREDITS.md` geführt.
