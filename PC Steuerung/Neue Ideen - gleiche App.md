---
tags:
  - projekt/pc-steuerung
  - thema/ideen
  - thema/roadmap
  - typ/plan
aliases:
  - New Ideas Same Old App
  - All-in-One
  - Neue Richtung
  - Hub
status: aktiv
stand: 2026-11-13
---

# Neue Ideen, gleiche App

> Entscheidung vom 2026-11-13: **„PC Steuerung" bleibt Name und Icon.** Der Kern bleibt
> unverändert – aber die App wächst zu einem **All-in-One-Werkzeug für meinen eigenen PC**:
> Steuerung + Spiel-Optimierung + Downloads + das, was später im Alltag fehlt.

## Kurzfassung

- **Name:** „PC Steuerung" – passt weiterhin, weil am Ende alles den PC steuert.
- **Icon:** bleibt vorerst. Neu bewerten erst, wenn das Design ohnehin angefasst wird.
- **Kern:** Ein/Aus, Status, Ruhezustand, Energieplan, Timer, Verlauf – bleibt wie es ist.
- **Neu:** Spiel-Optimierung und Downloads, danach nach Bedarf.
- **Rahmen:** rein privat. Keine Veröffentlichung, kein Play Store, keine Fremdnutzer.
- **Technik:** Zusatzfunktionen sind **Plugins**, die der Agent auf Knopfdruck startet und
  danach beendet. Kein 24/7-Dienst, keine Dauerüberwachung.

## Was bleibt (Kern, unverändert)

| Funktion | Weg | Status |
|---|---|---|
| Einschalten | WoL-Magic-Packet direkt vom Handy | bleibt |
| Status (grün/gelb/rot/grau) | TCP + `GET /status` | bleibt |
| Herunterfahren / Abbrechen | `POST /shutdown`, `POST /abort` | bleibt |
| Ruhezustand | `POST /hibernate` | bleibt |
| Energieplan | `POST /power-plan` | bleibt (wird vom Plugin mitbenutzt) |
| Timer, Verlauf | Module `timer`, `log` | bleiben |

## Feature 1: Game Optimization – als Plugin (umgesetzt 13.11.2026)

**Warum Plugin und nicht Button:** Die Optimierung soll **nicht** dauerhaft laufen. Die
Main-App gibt das Signal, das Plugin arbeitet, danach ist Ruhe. Kein Hintergrundwächter,
kein Dauerprozess.

**Ablauf:**

1. **Web-UI-Tab „Game Optimization"** – neue Leiste oben mit zwei Tabs
   (Steuerung · Game Optimization).
   - **Spiele scannen:** Steam (inkl. `libraryfolders.vdf`), Epic
     (`LauncherInstalled.dat`), GOG (Registry) und eigene Ordner.
   - **Profil:** Kill-Liste (Komma-getrennt), eigene Suchordner, Schalter
     „Automatisch das laufende Spiel optimieren".
   - **Aktionen:** pro Spiel „Optimieren", „Automatisch optimieren"
     (Agent erkennt das laufende Spiel), „Optimierung beenden".
2. **Plugin `game-optimizer.exe`** startet nur auf Anforderung:
   - Energieplan → Höchstleistung (vorheriger Plan wird gemerkt)
   - Kill-Liste per `taskkill /IM ... /F` schließen (das Spiel selbst nie)
   - Arbeitsspeicher bereinigen
   - Spiel-Priorität auf „hoch"
3. **Beenden:** `stop` auf stdin → Energieplan zurück. Beendete Apps werden bewusst
   **nicht** neu gestartet.

**Arbeitsspeicher – Vorbild Mem Reduct:** Working Sets per `EmptyWorkingSet` trimmen und
die Standby-Liste per
`NtSetSystemInformation(SystemMemoryListInformation, MemoryPurgeStandbyList)` leeren.
Aus Mem Reduct (Henry++, **GPL-3.0**) wurde **kein Code übernommen** – das passt nicht zu
unserer MIT-Lizenz. Nachgebaut mit eigenen Windows-API-Aufrufen, Credits in `CREDITS.md`
und [[Credits]]. Ohne Administratorrechte bleibt die Standby-Liste unberührt (nur Trimmen).

**Automatik, ehrlich definiert:** „Automatisch" heißt *Auswahl automatisch* (Agent nimmt
das laufende Spiel aus der Scan-Liste), **nicht** ein Dienst, der 24/7 Prozesse beobachtet.

**Neu im Agenten:**

| Baustein | Datei | Aufgabe |
|---|---|---|
| Plugin-Host | `src/plugins.rs` | Manifeste (`plugins\*.json`), starten/stoppen, Ausgabe-Ringpuffer, Journal |
| Spiele-Scan | `src/games.rs` | Steam/Epic/GOG/eigene Ordner, Cache `games.json`, Config `game-config.json` |
| Plugin | `src/bin/game-optimizer.rs` | die eigentliche Optimierung (eigenes EXE, keine neue Crate) |
| Web-UI | `src/page.html` | Tab „Game Optimization" |

**Endpunkte:** `GET /plugins`, `POST /plugins/{id}/start?game=&auto=`,
`POST /plugins/{id}/stop`, `GET /plugins/{id}/output?lines=`, `GET /games`,
`POST /games/scan`, `GET /game-config`, `POST /game-config`.

**Verifiziert (13.11.2026):** `cargo test` 35 grün, Release-Build von `pc-agent.exe` und
`game-optimizer.exe`, Live-Test im Simulationsmodus: Scan findet 29 Spiele (Steam,
D:/E:-Bibliotheken, Duplikate und Redist-Dateien aussortiert), Start → Läuft mit PID →
Ausgabe korrekt → `stop` beendet mit Code 0 und schreibt `game-optimizer.status.json`.

## Feature 2: Downloads im Blick (geplant)

**Ziel:** sehen, was der PC gerade lädt, ohne zum PC zu laufen. Erste Version **nur lesen**.

**MVP:**
- `GET /downloads` liest den Download-Ordner (`%USERPROFILE%\Downloads`, überschreibbar per
  `PC_AGENT_DOWNLOADS`).
- **Laufend** erkannt an: `.crdownload` (Chrome/Edge), `.part` (Firefox), `.download`, `.!ut`
  sowie Dateien, die in den letzten 2 Minuten gewachsen sind.
- Anzeige: Name, Größe, Zuwachs in MB/s (aus zwei Messungen). Restzeit nur, wenn die
  Gesamtgröße bekannt ist – sonst ehrlich „läuft" statt Fake-Prozent.
- **Fertig**: zuletzt abgeschlossene Dateien der letzten 24 Stunden.

Später: Abbrechen/Löschen, Öffnen/Umbenennen, echter Downloader (aria2/qBittorrent-API).

## Wie es weitergeht

- [x] Plugin-Host im Agenten (starten/stoppen nur auf Anforderung)
- [x] Spiele-Scan (Steam, Epic, GOG, eigene Ordner)
- [x] Web-UI-Tab „Game Optimization" mit Profil und Start/Stop
- [x] Plugin mit Energieplan, Kill-Liste, RAM-Bereinigung, Spiel-Priorität
- [ ] **Android-App anbinden** (gleiche Endpunkte, Start über die App)
- [ ] Downloads-MVP (read-only)
- [ ] Feinschliff/Design – **erst danach** Icon und Theme neu überdenken

## Offene Fragen

- [ ] Kill-Liste **je Spiel** statt global?
- [ ] Spiel-Erkennung über den Prozesspfad (statt nur Dateiname) – genauer bei
      „UnityPlayer.exe" o. ä.
- [ ] Auto-Rücklauf nach X Minuten (Timer-Logik existiert bereits)?
- [ ] Soll der Agent im Alltag **ohne** Admin laufen (Standby-Liste bleibt dann zu)?
- [ ] Eigener Tab „Werkzeuge", wenn Downloads dazukommen?
- [ ] „PC neu starten" als eigene Aktion? (fehlt bisher)

## Nicht-Ziele

- Kein Overclocking, RGB, Registry-Tuning oder „PC-Booster"-Versprechen.
- Kein vollwertiger Download-Manager mit Warteschlange und Scheduler.
- Keine Cloud, kein Zugriff von unterwegs, keine Veröffentlichung.
- Keine Änderung an Auth-, Token- oder Simulationsmodell.
- Kein Code aus GPL-Projekten (auch nicht „nur ein bisschen").

## Verwandt

- [[Ideen und Backlog]] – laufende Liste
- [[Roadmap und Entscheidungen]] – Stand und Begründungen
- [[UI und Design]] – Regeln für neue UI-Aktionen
- [[PC Agent (Rust)]] – Agent, Endpunkte, Datenordner
- [[Android App]] – Aufbau, Module, Build
- [[API]] – Endpunkt-Referenz (wird hier erweitert)
- [[Credits]] – Mem Reduct als Vorbild
