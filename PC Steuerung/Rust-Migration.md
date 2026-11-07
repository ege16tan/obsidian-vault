---
tags:
  - projekt/pc-steuerung
  - thema/rust
  - typ/plan
aliases:
  - Rust
  - Rust-Migration
  - Rust Agent
status: in Arbeit (Blocker: Windows SAC)
stand: 2026-11-07
---

# Rust-Migration

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
- **Code komplett** in `PC Agent Rust/` (noch nicht committet): 13 Endpunkte 1:1 vom
  Python-Agent, Web-UI eingebettet (`src/page.html`), Ereignis-Ringpuffer (50, neueste zuerst),
  Log-Rotation > 1 MB, `/setup` nur localhost, `PC_AGENT_SIMULATE=1` als Default.
- **PBKDF2-Parität als fester Testvektor**: salt `000102..0f`, Passwort `MeinTestpasswort1`,
  200 000 Iterationen SHA-256 ⇒ `38d15d24…7179` (liegt in `auth.rs`-Tests). Das bestehende
  `credentials.json` bleibt also gültig.
- **Patch-Crate `patches/windows-link/`** fertig: ersetzt `raw-dylib` durch klassische
  `#[link]`-Deklarationen – kein `dlltool.exe` nötig, weil die GNU-Toolchain alle
  Import-libs unter `lib/self-contained` mitbringt. Aktiv über `[patch.crates-io]` in `Cargo.toml`.
- **Toolchain**: Rust 1.99.1, `stable-x86_64-pc-windows-gnu` (self-contained MinGW, kein MSVC).
  Cargo immer mit VOLLPFAD: `C:\Users\egeta\.cargo\bin\cargo.exe`.

## Blocker: SAC (OS-Fehler 4551)
Windows „Smarte App-Steuerung" (`VerifiedAndReputablePolicyState = 1`) blockiert unsignierte,
frisch gebaute Programme (Build-Skripte und später auch die EXE). Nur der Nutzer kann das
beheben (Registrierung braucht Admin) – **Entscheidung: SAC dauerhaft aus**:

```powershell
reg add "HKLM\SYSTEM\CurrentControlSet\Control\CI\Policy" /v VerifiedAndReputablePolicyState /t REG_DWORD /d 0 /f
```

Danach **Neustart** (kein Ausschalten – der PC kommt normal wieder hoch).

## Nächste Schritte (nach dem Neustart)
1. `cargo test` (VOLLPFAD) – inkl. PBKDF2-Vehktor-Test
2. `cargo build --release` ⇒ `target\release\pc-agent.exe`
3. Live-Test im Simulationsmodus gegen die Android-App (curl + Handy)
4. Release-Paket (exe + README) erst nach erfolgreichem Live-Test
5. README/AGENTS ergänzen: SAC/dlltool-Situation, Repo-Archivierung + `CATALOG_URL`-Migration

## Referenz
- Python-Referenz: `PC App/main.py` (616 Zeilen)
- Endpunkt-Doku: [[API]]
