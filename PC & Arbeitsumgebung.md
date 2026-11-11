---
tags:
  - typ/doku
  - thema/pc
aliases:
  - PC und Arbeitsumgebung
  - Rechnerübersicht
status: aktiv
stand: 2026-11-09
---

# PC & Arbeitsumgebung

Diese Notiz sammelt den dokumentierten Hardwarekontext, lokale Arbeitsordner und die per Windows-Installationsinventar gefundenen KI-Apps. Pfade sind benutzerspezifisch und können sich ändern; vor Nutzung prüfen. Keine Passwörter, Tokens oder API-Schlüssel hier ablegen.

## PC-Hardware

Das in den Command-Center-Notizen verifizierte Testsystem:

| Komponente | Dokumentierter Stand |
|---|---|
| Mainboard | MSI H87-G43 (MS-7816) |
| CPU | Intel Core i7-4770 |
| GPU | NVIDIA GeForce GTX 1060, 6 GB |
| Betriebssystem | Windows 11 x64, HVCI aktiviert |
| Super-I/O | Nuvoton NCT6779D, 29 Sensoren |

Quelle: [[Command Center/Hardware-Verifikation H87-G43]] und [[Command Center/Hardware Control Center]]. RAM, Laufwerke, Netzwerkkarte und weitere PC-Daten sind dort nicht dokumentiert.

## Projekt- und Arbeitsordner

| Projekt / Ordner | Lokaler Pfad | Hinweis |
|---|---|---|
| Hardware Control Center | `D:\Command Center on Crack` | Konfigurierter GitHub-Checkout von `ege16tan/hardware-control-center` |
| Rust-Agent für PC Steuerung | `D:\Goodnight my pc\PC Agent Rust` | In der Rust-Migrationsnotiz als lokaler Entwicklungsordner festgehalten; genauer Repo-Status dort prüfen |
| PC Steuerung | nicht ermittelt | Kanonisches GitHub-Repo laut Projektnotizen: [ege16tan/pc-steuerung](https://github.com/ege16tan/pc-steuerung); lokaler Checkout nicht in den konfigurierten Repositories aufgeführt |
| ponytail | `C:\Users\egeta\.copilot\repos\ponytail` | Konfigurierter Checkout von `DietrichGebert/ponytail`; Projektbezug zum PC ist nicht dokumentiert |
| Cyber | `C:\Modding\Cyber` | Konfigurierter lokaler Ordner, kein GitHub-Repository in der App-Konfiguration |

## Gefundene KI-Entwicklungs-Apps

Schreibgeschützte Abfrage der Windows-Uninstall-Registry am 2026-11-09. Das ist keine vollständige Softwareinventur: portable Apps, manche Store-Installationen und nicht registrierte Anwendungen können fehlen.

| Anwendung | Registrierte Version | Zweck / Notiz |
|---|---:|---|
| Cursor (User) | 3.27.22 | KI-gestützter Code-Editor |
| GitHub Copilot | 1.1.35 | Copilot-Desktop-App laut Windows-Installationsinventar |
| OpenCode | 2.2.6 | KI-Coding-Agent/Entwicklungs-App |

## Weitere bekannte Arbeitsprogramme

Obsidian ist der Wissens-Vault selbst. Android Studio wurde in der laufenden Arbeitsumgebung als geöffnet erkannt. Die Angaben sind keine Aussage über eine vollständige Installation.

## Navigation

- [[Willkommen]] – Einführung in den Vault
- [[Projekte]] – Projektübersicht
- [[PC Steuerung/PC Steuerung|PC Steuerung]] – Android-App und PC-Agent
- [[Command Center/Command Center|Command Center]] – Hardware-Monitoring

## Aktualisierung

Hardware, App-Versionen und Repo-Pfade bei Änderungen nachprüfen. Bei der App-Liste unterscheiden zwischen „in der Registry gefunden“ und „tatsächlich installiert“; Einträge aus der Registry können veraltet sein.