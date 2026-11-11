---
tags:
  - typ/startseite
aliases:
  - Start hier
  - Einführung
  - Dashboard
  - Übersicht
status: aktiv
stand: 2026-11-11
---

# Willkommen im Vault

> [!info] In 10 Sekunden orientiert
> Zwei aktive Projekte: **PC Steuerung** (Android-App + Windows-Agent) und **Command Center**
> (Hardware-Monitoring). Diese Seite ist das Dashboard: Status, Einstiegspunkte, Konventionen.
> Für schnelle Sprünge: [[Projekte]] · [[Credits]] · [[Offene Fragen]].

## Auf einen Blick

| Projekt | Stand | Version | Nächstes |
|---|---|---|---|
| [[PC Steuerung/PC Steuerung\|PC Steuerung]] | aktiv, Release v0.9.5 gebaut und installiert | App `0.9.5` (versionCode 16), Agent `0.1.0` | Live-Test Scanner/WoL, Routinen, Design-Politur |
| [[Command Center/Command Center\|Command Center]] | pausiert, Lesen verifiziert | – | Lüfter-Schreibpfade nur nach Hardware-Verifikation |

## Einstieg nach Ziel

| Ich will … | Hier lang |
|---|---|
| verstehen, was die App kann | [[PC Steuerung/PC Steuerung\|Projektübersicht PC Steuerung]] |
| die App bauen/installieren | [[PC Steuerung/Android App\|Android-App (Build, ADB, Versionen)]] |
| den Agenten auf dem PC starten | [[PC Steuerung/PC Agent (Rust)\|PC-Agent (Rust)]] |
| die API aufrufen | [[PC Steuerung/API\|Endpunkte]] |
| wissen, was als Nächstes kommt | [[PC Steuerung/Roadmap und Entscheidungen\|Roadmap]] · [[PC Steuerung/Ideen und Backlog\|Backlog]] |
| Routinen planen (statt Zeitplan) | [[PC Steuerung/Routinen\|Routinen]] |
| Design-/UI-Themen nachlesen | [[PC Steuerung/UI und Design\|UI und Design]] |
| wissen, woher der Code/Ideengeber kommt | [[Credits]] |
| offene Entscheidungen sehen | [[Offene Fragen]] |
| Sicherheitsregeln prüfen | [[PC Steuerung/Sicherheit\|Sicherheit]] · [[PC Steuerung/Test-Checkliste\|Test-Checkliste]] |

## Vault-Aufbau

- **Ein Ordner pro Projekt** mit zentraler Übersichtsnotiz (`typ/index`), thematischen
  Unterlagen (`typ/doku`, `typ/referenz`) und Plan-/Aufgabenseiten (`typ/plan`, `typ/backlog`).
- **Frontmatter ist Pflicht:** `tags`, `aliases`, `status`, `stand` (Datum der letzten
  inhaltlichen Prüfung). `stand` ist das Vertrauenssignal – ältere Stände zuerst am Code prüfen.
- **Tags:** `projekt/pc-steuerung`, `projekt/command-center`, `thema/android`, `thema/rust`,
  `thema/api`, `thema/sicherheit`, `thema/hardware`, `thema/workflow`, `typ/*`.
- **Links:** Wikilinks `[[Notiz]]` innerhalb des Vaults, Markdown-Links nur nach außen.
  In Tabellen `[[Notiz\|Alias]]` verwenden.
- **Keine Geheimnisse:** keine Passwörter, Tokens, API-Keys oder privaten IPs in Notizen.

## Arbeitsregeln (gelten projektübergreifend)

- Änderungen im passenden Repository machen **und** die Doku hier mitziehen.
- PC Steuerung: Simulation ist der Ausnahme-Modus, echte Aktionen sind Standard – destruktive
  Aktionen (Herunterfahren) nur mit Countdown und erreichbarem Abbrechen.
- Command Center: keine Schreib-/Steuerpfade ohne Hardware-Verifikation und Aufsicht.
- Lesen → Verifizieren → Steuern.

## Letzte Änderungen

**2026-11-11 (v0.9.5)**

- Homescreen-Widget entfernt, GUI-Launcher entfernt (Agent läuft rein im Hintergrund)
- Tray startet automatisch, Debug-Konsole `start-agent-debug.bat`
- Simulationsmodus als Ausnahme (Banner), Echt ist Standard; Statuspunkt „PC aus" = rot
- Setup lässt sich verlassen, Markt+ aufgewertet, Energieplan-Animation, weicher Theme-Wechsel
- UI-Texte nach `strings.xml`, [[Credits]] angelegt

**2026-11-10** – Rust-only: Python-Agent archiviert, Datenordner `%LOCALAPPDATA%\PC Steuerung`.

**2026-11-06** – Grundaufbau, Sicherheitsmodell, [[PC Steuerung/Rückblick|Rückblick]] mit Echtmodus-Vorfall.

## Projekt-Repositories

- PC Steuerung: [ege16tan/pc-steuerung](https://github.com/ege16tan/pc-steuerung) (öffentlich, MIT)
- Command Center: [ege16tan/hardware-control-center](https://github.com/ege16tan/hardware-control-center)
- Python-Archiv: [ege16tan/pc-steuerung-python](https://github.com/ege16tan/pc-steuerung-python)
