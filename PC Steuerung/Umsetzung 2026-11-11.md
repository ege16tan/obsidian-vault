---
tags:
  - projekt/pc-steuerung
  - typ/rueckblick
  - thema/release
aliases:
  - Umsetzung 2026-11-11
  - v0.9.5 Umsetzung
  - Änderungsprotokoll 11.11.
status: aktiv
stand: 2026-11-11
---

# Umsetzung 2026-11-11 (v0.9.5)

> Vollständiges Protokoll der Sitzung: was geändert wurde, womit es geprüft ist und was
> offen bleibt. Alles hier nachlesbar – auch was sonst als Rückfrage gestellt worden wäre.

## Release

- Tag: [`v0.9.5`](https://github.com/ege16tan/pc-steuerung/releases/tag/v0.9.5)
- Asset: `app-release.apk` (22,6 MB), signiert mit unverändertem Key
  (`CN=PC Steuerung, O=Personal, C=DE`, SHA-256 `5591d593…d01fd114a`)
- Commit: `e26c81b` (main), Vault: `7ec11df`
- App: `versionCode 16`, `versionName 0.9.5`

## Android-App

| Änderung | Datei/Stelle |
|---|---|
| Homescreen-Widget komplett entfernt | `WidgetProvider.kt`, `res/layout/widget.xml`, `res/xml/widget_info.xml`, 3 Drawables, Manifest-Receiver, Strings, Aufruf in `MainActivity` |
| Setup verlassbar („Abbrechen und zurück"), leeres Profil wird verworfen | `AppRoot.cancelSetup()`, `SetupScreen` |
| Statuspunkt „PC aus" rot statt grau | `statusColor()` |
| Banner zeigt SIMULATION statt ECHT, Schalter „Simulationsmodus" | Statuskarte + `SettingsOverview` |
| Markt+: „Immer dabei"-Chips, Versions-Badges, Refresh-Icon | `MarketplaceTab`, `ModuleCard` |
| Energieplan: animierte Aktiv-Leiste + Skalierung | `PlanTab` |
| Weicher Theme-Wechsel (Fade) | `MainActivity.onCreate` |
| UI-Texte nach `strings.xml` | Navigation, Status, Einrichtung, Einstellungen |

**Geprüft:** `compileDebugKotlin` ✅, `testDebugUnitTest` ✅, `assembleDebug` ✅
(1m30s), `assembleRelease` ✅ (3m35s, R8 + lintVital), Smoke-Test: App startet,
`MainActivity` resumed, kein Crash in Logcat (vor dem Umbau geprüft).

## Windows-Agent

| Änderung | Datei |
|---|---|
| GUI-Launcher entfernt (kein Fenster mehr) | `launcher/*` gelöscht, `PC Steuerung.exe` entfernt |
| Tray startet automatisch mit dem Agenten | `agent-manager.ps1` → `Start-TrayIfNeeded` (Mutex `Local\PCSteuerungTray`) |
| Debug-Konsole mit Live-Log | `start-agent-debug.bat`, `-ShowConsole` im Manager |
| Installer ohne Launcher-EXE, neue Startmenü-Einträge | `installer/pc-steuerung.iss`, `build-installer.ps1` |

**Geprüft:** PowerShell-Parser für `agent-manager.ps1` und `tray.ps1` → 0 Syntaxfehler.

## Dokumentation

- **Repo:** `CREDITS.md` (neu), `AGENTS.md`, `ROADMAP.md`, `Readme.md`, `docs/planung.md`
- **Vault:** [[Willkommen]] als Dashboard, [[Projekte]] als Projektliste, neue Notizen
  [[PC Agent (Rust)]], [[Routinen]], [[UI und Design]], [[Credits]], [[Offene Fragen]];
  [[PC Steuerung]], [[Android App]], [[Sicherheit]], [[Git und Workflow]],
  [[Roadmap und Entscheidungen]], [[Ideen und Backlog]] aktualisiert

## Offen / nicht erledigt

- **Live-Test auf dem Handy:** ADB verlor die Verbindung (`no devices`), APK 0.9.5 konnte
  nicht installiert werden. Zu prüfen: QR-Scanner (CameraX/ML Kit), WoL-Schritt, Theme-Wechsel.
- **Routinen** sind Konzept ([[Routinen]]), kein Code.
- **Web-UI an/aus aus der App** braucht einen Agent-Endpunkt ([[Offene Fragen]] Punkt 2).
- **Restliche UI-Texte** (Timer-, Dialog-, Log-Texte) stecken noch im Code.
- **Echtmodus-Default:** Der Manager setzt weiterhin `PC_AGENT_SIMULATE=1`, wenn die Variable
  fehlt. Umschalten siehe [[Offene Fragen]] Punkt 1.

## Notiz an mich

Der Pre-Push-Hook lief bei beiden Pushes sauber (`✅ Keine Probleme gefunden.`).
Der Ordner `bilder/` (Fremd-Screenshots) wurde bewusst **nicht** committet.
