---
tags:
  - typ/index
aliases:
  - Projekte
  - Projektübersicht
status: aktiv
stand: 2026-11-11
---

# Projekte

> Neue hier? [[Willkommen]] ist das Dashboard, diese Seite ist die reine Projektliste.

## Aktiv

### [[PC Steuerung/PC Steuerung|PC Steuerung]]

| Feld | Wert |
|---|---|
| Ziel | Windows-PC vom Android-Handy im Heimnetz steuern (An/Aus, Ruhezustand, Energieplan, Status) |
| Technik | Android: Kotlin/Compose · PC: Rust-Agent (einzelne EXE) · HTTP + Basic Auth |
| Version | App **0.9.5** (versionCode 16) · Agent **0.1.0** |
| Stand | Release gebaut, signiert und auf dem Testgerät installiert; Live-Test Scanner/WoL offen |
| Repo | [ege16tan/pc-steuerung](https://github.com/ege16tan/pc-steuerung) |
| Lokal | `D:\Goodnight my pc` |
| Nächstes | Routinen-Feature, Design-Politur, Rust-HTTP-Tests + CI |

### [[Command Center/Command Center|Command Center]]

| Feld | Wert |
|---|---|
| Ziel | Windows-Desktop-App (C#/.NET 10, WPF) für Hardware-Monitoring und Lüfterdiagnose |
| Stand | Lesen verifiziert; nur Fan #2 und #4 schreibverifiziert; aktive Lüftersteuerung **nicht** freigegeben |
| Repo | [ege16tan/hardware-control-center](https://github.com/ege16tan/hardware-control-center) |
| Lokal | `D:\Command Center on Crack` |
| Nächstes | Hardware-Verifikation weiterer Kanäle, sonst keine Schreibpfade |

> [!warning] Command Center bleibt vorerst Doku-only
> Keine Schreib- oder Steuerpfade ohne dokumentierte Hardware-Verifikation und Aufsicht.

## Querschnitt

- [[Credits]] – verwendete Bibliotheken, Tools und Agent-Skills
- [[Offene Fragen]] – Entscheidungen, die noch eine Antwort brauchen
- [[PC & Arbeitsumgebung]] – Hardware, lokale Ordner, installierte KI-Apps

## Schnellzugriff (Suchen)

- PC Steuerung: `tag:#projekt/pc-steuerung`
- Command Center: `tag:#projekt/command-center`
- Offene Arbeit: `tag:#typ/plan` · Ideen: `tag:#typ/backlog`
