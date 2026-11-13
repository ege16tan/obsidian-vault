---
tags:
  - projekt/pc-steuerung
  - thema/review
  - typ/doku
aliases:
  - Kimi
  - Kimi Ideen
  - Review
  - Web-UI Neubau
  - Code Review extern
status: offen
stand: 2026-11-13
---

# Kimi Review (2026-11-13)

> Externe Review von **Kimi** zum Rust-Agenten (gelesen: `Cargo.toml`, `main.rs`,
> `routes.rs`, `auth.rs`, `page.html`, `start-agent.bat`, `agent-manager.ps1`).
> Diese Notiz sammelt die Punkte, damit wir sie später wiederfinden – **nichts
> davon ist automatisch beschlossen**.

## Kurzfazit von Kimi

1. Start über Batch/PowerShell ist nicht optimal
2. Port 8000 ist ungünstig und konfliktanfällig
3. Login über normales HTTP ist im LAN unsicher
4. Die Web-Oberfläche ist ein großer, altmodischer HTML/JS-Block
5. Die Dokumentation sagt teilweise noch „FastAPI“, obwohl es Rust + Axum ist
6. Für unterwegs wäre Tailscale/WireGuard besser als ein offener Port

## Faktencheck: was stimmt, was nicht

| Kimi-Punkt | Stimmt? | Stand bei uns (geprüft 13.11.2026) |
|---|---|---|
| `start-agent.bat` ist nur ein Wrapper um PowerShell | **ja** | Datei ruft nur `agent-manager.ps1 -Action start` (hidden). Baut ggf. mit Cargo, startet Agent + Tray. |
| Port 8000 ist konfliktanfällig | **ja** | 8000 steht an ~10 Stellen fest: `main.rs`-Default, `agent-manager.ps1 -Port`, `open-setup.ps1`, `build-installer.ps1`-Kommentar, README, `docs/planung.md`, AGENTS.md. `PC_AGENT_PORT` existiert, wird aber nicht überall durchgereicht. |
| HTTP im LAN ist unsicher | **ja, aber bewusst** | Konzept ist „nur Heimnetz, kein Portforwarding, kein VPN“. Tailscale/WireGuard steht in [[Roadmap und Entscheidungen]] als **verschoben**. |
| Web-UI ist ein großer HTML/JS-Block | **ja** | `src/page.html` allein ~27 KB (CSS + HTML + JS + inzwischen Game-Optimization-Tab), per `include_str!` im Binary. |
| README sagt noch „FastAPI“ | **nein** | `Readme.md` nennt Rust. „FastAPI“ steht nur in **Code-Kommentaren** (`routes.rs`: „Validierung wie FastAPI“) als Paritäts-Hinweis zur alten Python-Version und in `CREDITS.md` (Archiv-Hinweis). Kein Doku-Fehler, nur Historie. |
| Simulation-Default vs. Manager | **ja, Feinschliff** | `main.rs`: ohne `PC_AGENT_SIMULATE` = Simulation. `agent-manager.ps1`: setzt für den normalen Start Echtbetrieb. Ist in README + AGENTS.md erwähnt, aber leicht zu übersehen. |
| Kein CSRF-Schutz auf localhost | **ja** | Localhost braucht keinen Login, `POST /shutdown`, `/hibernate`, `/mode` sind ohne Header erreichbar. |

## Kimis Punkte im Einzelnen

### 1. Start über Batch/PowerShell ersetzen
Vorschlag: `pc-agent.exe install | start | stop | status | uninstall` (bzw.
`--install-autostart` / `--remove-autostart`).
Gute Wege: **Windows-Dienst** für die API, **Aufgabenplanung** fürs Tray, oder beides.
Vorteile: kein verstecktes PowerShell-Fenster, stabiler Start nach Updates, Neustart bei Absturz.
**Kimi-Hinweis:** Als Dienst prüfen, ob `powercfg` im richtigen Benutzerkontext läuft
(Energiepläne sind benutzerabhängig); Herunterfahren geht als Dienst meist problemlos.

### 2. Port ändern
Vorschlag: Standard auf z. B. `47800`, besser noch:
- Port beim ersten Setup automatisch suchen, Konflikt erkennen, speichern
- Firewall-Regel passend anlegen
- QR-Code nutzt automatisch den echten Port
- optional **mDNS/Bonjour** (`pc-steuerung.local`), damit die App keine IP/Port mehr braucht

### 3. Anmeldung verbessern
Variante A: Basic Auth behalten + nur privates WLAN, Firewall nur `localsubnet`,
kein Portforwarding, Passwort ≥ 12 Zeichen.
Variante B (Empfehlung): **Tailscale/WireGuard** – verschlüsselt, kein Portforwarding,
nur eigene Geräte.

### 4. Auth-Modell vereinfachen
Statt Basic + Token + Env-Token-Fallback nur noch zwei Wege:
- lokal am PC: keine Anmeldung (nur localhost)
- Handy: **Geräte-Token beim Pairing**, widerrufbar, Rechte je Gerät (z. B. nur Status/Shutdown)

### 5. CSRF bei der lokalen Web-Oberfläche
Da localhost ohne Login funktioniert, könnte ein fremdes Skript im Browser
`POST /shutdown` an `localhost:8000` schicken.
Vorschlag: CSRF-Token, zusätzlicher Zufalls-Header (nur die echte UI sendet ihn)
oder Session-Login auch auf localhost.

### 6. Web-Oberfläche neu konzipieren
Kritik: eine Datei, Alert/Confirm, Polling alle 2 s, Logik + Darstellung + API vermischt.
Optik-Vorschlag: großer PC-Status, sehr große „Herunterfahren“-Schaltfläche,
WoL als eigene Karte, Energieplan als Segment-Auswahl, Countdown prominent,
Ereignisse als Timeline, Einstellungen separat, **Handy-First**.
Technik: React/Vue/Svelte als statisches Frontend, aus Rust ausgeliefert
(`rust-embed` / `tower-http`), API unter `/api/v1`, Struktur
`src/{main.rs, routes/, services/, auth/, web/dist}`.

### 7. API auf `/api/v1` umbauen
Statt `POST /shutdown?delay=60` → `POST /api/v1/actions/shutdown {"delaySeconds":60}`,
`POST /api/v1/power-plan {"plan":"balanced"}`. Vorteile: einheitlich, dokumentierbar,
versionierbar, bessere Fehler.

### 8. Rust beibehalten
Klarstellung: aktives Projekt ist **Rust + Axum**, kein FastAPI mehr.
Kotlin-Alternative (Ktor + Compose Desktop) wurde verworfen: größere Laufzeit,
schwerere Installation, mehr Ressourcen. Empfehlung: Backend Rust lassen,
Web-UI trennen, Android über klare API anbinden.

### 9. Kommunikation Android ↔ PC
Option A: OpenAPI → Kotlin-Client generieren. Option B: Protobuf/gRPC (Overkill).
Option C: verbesserte REST-API (für dieses Projekt ausreichend).

### 10. Live-Updates statt Polling
SSE (`GET /api/v1/events/stream`) oder WebSocket für Status, Countdown, Ereignisse.

### 11. Dokumentation bereinigen
Siehe Faktencheck: README ist bereits Rust. Sinnvoll bleibt: Simulations-Default
(Entwicklung = Simulation, Installer/Manager = Echt) **deutlicher** trennen.

## Kimis Prioritätenliste

**P1 – schnell sinnvoll**
- [ ] Standardport weg von 8000 (z. B. 47800)
- [ ] Port aus **einer** zentralen Konfiguration lesen (nicht an 10 Stellen)
- [ ] QR-Setup nutzt immer den echten Port
- [ ] `start-agent.bat` schrittweise durch `pc-agent.exe`-Subkommandos ersetzen
- [ ] Installer/Autostart sauberer

**P2 – Sicherheit**
- [ ] Token-/Login-Konzept vereinfachen (Geräte-Token)
- [ ] CSRF-/Header-Schutz für die Web-UI
- [ ] Tailscale/WireGuard als empfohlenen Fernzugriff dokumentieren
- [ ] kein Portforwarding ins Internet (bleibt Regel)

**P3 – neue Web-Oberfläche**
- [ ] Frontend als eigenes Projekt
- [ ] API auf `/api/v1`
- [ ] SSE/WebSocket
- [ ] Design neu

**P4 – Ausbau**
- [ ] Agent als Dienst/Tray-Programm
- [ ] Konfigurationsdatei
- [ ] strukturierte Logs/Fehler

## Architektur-Empfehlung (übernommen)

```text
Android App
   │  REST /api/v1 (später Geräte-Token)
   ▼
Rust PC-Agent (Axum)
   ├── API
   ├── Wake-on-LAN
   ├── Shutdown/PowerPlan
   ├── lokale Web-UI
   ├── Tray
   └── Config/Logs
```

Fernzugriff:

```text
Android ── Tailscale/WireGuard ── Windows-PC
```

Nicht: `Internet ── offener Router-Port ── Port 8000`.
## Was bei uns schon anders entschieden ist (nicht überstimmen)

| Thema | Unsere Entscheidung | Warum das Kimis Punkt relativiert |
|---|---|---|
| Zusatzfunktionen | **Plugins** (`game-optimizer`), Start nur auf Knopfdruck | Kimi kennt den Plugin-Host noch nicht (Stand 13.11.2026) |
| Arbeitsspeicher | nach Mem-Reduct-Vorbild **neu gebaut** (kein GPL-Code) | Lizenz: unser Repo ist MIT |
| Name/Icon | „PC Steuerung“ bleibt, Icon bleibt vorerst | siehe [[Neue Ideen - gleiche App]] |
| Veröffentlichung | rein privat, kein Play Store | Marketing-/Kompatibilitätsaufwand entfällt |
| UI-Zusätze | kein GUI-Launcher, kein Widget; **Tray ist Pflicht** | Kimi fordert Dienst + Tray – Tray existiert schon |
| Wake-on-LAN | Magic Packet **direkt vom Handy** | Agent-Endpunkt `/wake` nur für Web-UI/Tests |
| Fernzugriff | verschoben (kein VPN nötig im Alltag) | Tailscale bleibt Option, keine Pflicht |

**Wichtig für den API-Umbau:** Die Android-App spricht **heute schon** die alten Pfade
(`/status`, `/shutdown?delay=`, `/power-plan`, `/plugins`, `/games`, `/game-config`).
Ein harter Wechsel auf `/api/v1` bricht die App → nur mit Alias-Routen oder
gemeinsamem App-Update.

## Offene Entscheidung: welche Web-Oberfläche?

Kimi schlägt drei Stufen vor:

- **A – Minimal-Panel:** Status, Herunterfahren, Ruhezustand, WoL
- **B – Kontrollzentrum:** + Energieplan, Timer, Logs, Einstellungen, mehrere PCs
- **C – modernes Dashboard:** große Live-Kacheln, dunkel, Animationen, Handy-First,
  später mehrere Geräte

Kimi vermutet **C**, „aber sauber und modern“.

**Meine Einordnung:**
- C ist attraktiv, kostet aber eine echte Build-Kette (Node/Vite → `web/dist` → `rust-embed`).
  Das ist ein eigener Arbeitsschritt, kein Nebenbei-Refactor.
- Zwischenschritt ohne Framework: `page.html` in Bausteine splitten (CSS/JS/Partials per
  `include_str!`) und CSS aufräumen – billig, sofort machbar.
- Erst **Design-Entscheidung** (Farben, Kacheln, Handy-First) treffen, dann Technik wählen.

## Meine Empfehlung als Reihenfolge

**Jetzt sinnvoll (klein, hoher Nutzen):**
1. Port zentral: eine Quelle (`PC_AGENT_PORT` + gespeicherte Wahl), alle Skripte
   (`agent-manager`, `open-setup`, Installer, README) ziehen daraus.
2. CSRF-/Header-Schutz für localhost (`X-PC-Steuerung: 1` von der UI, sonst 403 bei
   `POST` von localhost – oder echtes CSRF-Token).
3. Doku: Simulations-Default klar trennen (Entwicklung = Simulation, Manager/Installer = Echt).
4. `pc-agent.exe`-Subkommandos (`start|stop|status|install`), `start-agent.bat` bleibt
   als Kompatibilitäts-Wrapper.

**Später, mit eigenem Termin:**
5. Frontend-Neubau (Stufe C) inkl. `/api/v1` **mit** Alias-Routen für die App.
6. SSE statt Polling.
7. Geräte-Token statt Basic Auth (Pairing erweitern).

**Nicht nötig:** gRPC/Protobuf, Kotlin-Server, Dienste-Umbau um jeden Preis.

## Verwandt

- [[Neue Ideen - gleiche App]] – All-in-One-Richtung (Plugin-Stand)
- [[Roadmap und Entscheidungen]] – was beschlossen ist
- [[UI und Design]] – Regeln für UI-Aktionen
- [[Sicherheit]] – Auth, Simulation, Veröffentlichung
- [[API]] – aktuelle Endpunkte
- [[PC Agent (Rust)]] – Start, Datenordner, Tray
- [[Offene Fragen]] – u. a. Fernzugriff/Tailscale
