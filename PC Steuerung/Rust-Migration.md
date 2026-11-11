---
tags:
  - projekt/pc-steuerung
  - thema/rust
  - typ/plan
aliases:
  - Rust
  - Rust-Migration
  - Rust Agent
status: abgeschlossen
stand: 2026-11-10
---

# Rust-Migration

> **Abgeschlossen am 10.11.2026.** Der Rust-Agent ist die einzige PC-Version. Die Python-Version wurde ins Archiv-Repo ausgelagert und aus dem Haupt-Repo entfernt.

> Lokal in `D:\Goodnight my pc\PC Agent Rust\` (Nachfolger vom Python-Agent `PC App/`).
> Python bleibt bis zum Live-Test die Referenz.

## Ziel
Rust-Agent als **Drop-in-Ersatz** für den Python-Agenten – die Android-APK läuft unverändert weiter.
Ergebnis: eine einzelne `pc-agent.exe`, keine Python-Installation am PC.

## Stack
- Tokio + Axum, Serde/serde_json
- PBKDF2 + SHA-2 (Crates `pbkdf2`/`sha2`), base64, subtle, getrandom, chrono
- Windows: `std::process::Command` + `CREATE_NO_WINDOW`
- WoL: `std::net::UdpSocket`

## Stand vom 07.11.2026
- **Code-Stand:** Rust-Agent mit denselben 13 HTTP-Routen und eingebetteter Web-UI wie Python; Änderungen für nahe Funktionsparität sind committed und auf `main` gepusht (6f22ee3).
- **PBKDF2-Parität als fester Testvektor**: salt `000102..0f`, Passwort `MeinTestpasswort1`,
  200 000 Iterationen SHA-256 ⇒ `38d15d24…7179` (liegt in `auth.rs`-Tests). Das bestehende
  `credentials.json` bleibt also gültig.
- **Patch-Crate `patches/windows-link/`** fertig: ersetzt `raw-dylib` durch klassische
  `#[link]`-Deklarationen – kein `dlltool.exe` nötig, weil die GNU-Toolchain alle
  Import-libs unter `lib/self-contained` mitbringt. Aktiv über `[patch.crates-io]` in `Cargo.toml`.
- **Toolchain**: Rust 1.99.1, `stable-x86_64-pc-windows-gnu` (self-contained MinGW, kein MSVC).
  Cargo immer mit VOLLPFAD: `C:\Users\egeta\.cargo\bin\cargo.exe`.

## Build-Status
Der frühere SAC-Build-Blocker verhindert den aktuellen Release-Build nicht mehr: `cargo build --release` war am 09.11.2026 erfolgreich.
## Nächste Schritte
1. Live-Gegenprobe im Simulationsmodus gegen Python und die Android-App.
2. `cargo test` ausführen, wenn die Verifikation ansteht.
3. Vor dem Archivieren des Python-Repos den Remote-Modulkatalog aus dem Repo lösen und danach die Android-URLs koordinieren.

## Stand vom 09.11.2026
- `cargo build --release` ist erfolgreich durchgelaufen.
- Nahe Funktionsparität: gleiche Routen, Erfolgsantworten, Web-UI, Aktionen, Authentifizierung und Simulationsstandard. Angeglichen wurden HTTP-Status beim Versionswechsel, wiederholte Query-Parameter, Python-kompatible Countdown-Rundung und Log-Zeilenenden.
- Im gemeinsamen Repository verwenden beide Agenten `PC App/credentials.json` und `PC App/agent.log`; alte Rust-Dateien neben der EXE werden beim Start übernommen. Ohne `PC App/`-Ordner bleibt Rust eigenständig und nutzt den EXE-Ordner.
- Vorab-Release: https://github.com/ege16tan/pc-steuerung/releases/tag/rust-v0.1.0
- In diesem Arbeitsschritt wurden keine Live-Gegenprobe und keine `cargo test`-Ausführung gemacht. Das genaue 422-Fehlerformat für ungültige Eingaben unterscheidet sich noch zwischen FastAPI/Pydantic und Axum.
- Simulation bleibt Standard (`PC_AGENT_SIMULATE=1`); der Echtmodus wurde nicht aktiviert.
## Referenz
- Python-Referenz: `PC App/main.py` (616 Zeilen)
- Endpunkt-Doku: [[API]]

## Stand vom 10.11.2026
- Python-Agent nach https://github.com/ege16tan/pc-steuerung-python ausgelagert und dort archiviert; `PC App/` aus dem Haupt-Repo entfernt.
- Web-UI, Tray, Manager und Installer sind Rust-only; der Versionswechsel ist entfernt.
- Datenordner ist jetzt `%LOCALAPPDATA%\PC Steuerung` (`credentials.json`, `token.json`, `device.json`, `agent.log`). Alte `PC App/`-Daten werden beim Start übernommen.
- `cargo test`: 20 Tests grün. `cargo build --release` und Installer-Build (`PC-Steuerung-Setup-0.9.4.exe`) erfolgreich.
- Launcher `PC Steuerung.exe` startet Agent, Tray und Web-UI.
- Offen: Release-APK 0.9.4 auf dem Handy installieren und Scanner/WoL live testen.
