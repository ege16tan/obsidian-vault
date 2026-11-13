---
tags:
  - typ/doku
  - thema/credits
aliases:
  - Credits
  - Danksagung
  - Fremdcode
status: aktiv
stand: 2026-11-11
---

# Credits

> [!info] Regel
> Wenn wir Code, eine Bibliothek, ein Tool **oder einen Agent-Skill** von jemand anderem
> nutzen, tragen wir es hier **und** in `CREDITS.md` im Repo ein (Quelle + Zweck).
> So können wir am Ende sauber Danke sagen.

## Agent-Skills (Entwicklungshilfe, nicht Teil der App)

Installiert global unter `~/.agents/skills` über die [Skills-CLI](https://skills.sh).
Pflege: `cline skill list -g`, Update mit `npx skills update -g`.

| Paket | Genutzte Skills | Wofür |
|---|---|---|
| [android/skills](https://github.com/android/skills) | `r8-analyzer`, `testing-setup`, `camerax`, `anr-audit-and-fix`, `edge-to-edge` | offizielle Android-Guidance |
| [chrisbanes/skills](https://github.com/chrisbanes/skills) | `compose-performance`, `compose-state-and-effects`, `compose-ui-testing-patterns`, `gradle-run` | Compose-Qualität, Gradle-Workflow |
| [obsidianmd/obsidian-skills](https://github.com/obsidianmd/obsidian-skills) | `obsidian-markdown`, `obsidian-bases`, `obsidian-cli` | genau dieser Vault |
| [apollographql/skills](https://github.com/apollographql/skills) | `rust-best-practices` | idiomatisches Rust |
| [wshobson/agents](https://github.com/wshobson/agents) | `rust-async-patterns` | Tokio/Axum |
| [trailofbits/skills](https://github.com/trailofbits/skills) | `rust-review` | Security-Review |
| [mattpocock/skills](https://github.com/mattpocock/skills) | `code-review`, `diagnosing-bugs` | Review/Debug |
| [vercel-labs/skills](https://github.com/vercel-labs/skills) | `find-skills` | Skills finden |

## Bibliotheken im Projekt

| Bereich | Genutzt |
|---|---|
| Agent (Rust) | tokio, axum, serde/serde_json, pbkdf2, sha2, subtle, base64, getrandom, chrono |
| Android | Kotlin, Jetpack Compose (BOM 2025.09.00), CameraX, ML Kit Barcode, Shizuku, R8/AGP |
| Windows | Inno Setup (Installer), PowerShell/WinForms (`NotifyIcon` fürs Tray) |

## Vorbilder (Idee, kein übernommener Code)

| Projekt | Lizenz | Was wir daraus gelernt haben |
|---|---|---|
| [Mem Reduct](https://github.com/henrypp/memreduct) von [Henry++](https://github.com/henrypp) | GPL-3.0 | Arbeitsspeicher unter Windows freigeben: Working Sets per `EmptyWorkingSet` trimmen, Standby-Liste per `NtSetSystemInformation(SystemMemoryListInformation, MemoryPurgeStandbyList)` leeren. |

> [!warning] Kein Code kopiert
> Mem Reduct steht unter **GPL-3.0**, unsere App unter MIT. Deshalb wurde **kein
> Quellcode übernommen** – das Plugin `game-optimizer` ruft dieselben Windows-APIs
> selbst auf. `memreduct.org` ist übrigens eine **Fan-Seite**, maßgeblich ist das
> GitHub-Repo von Henry++.


## Werkzeuge

- [Obsidian](https://obsidian.md) als Wissensbasis, Plugin *Local REST API with MCP* für Agent-Zugriff
- Android Studio (`D:\Android Studio`) für Build und Projekt-Index
- GitHub CLI (`gh`) für Releases

## Frühere Version

Der archivierte Python-Agent ([pc-steuerung-python](https://github.com/ege16tan/pc-steuerung-python))
war die Verhaltensreferenz für den Rust-Agenten – auch ihm gebührt Dank.
