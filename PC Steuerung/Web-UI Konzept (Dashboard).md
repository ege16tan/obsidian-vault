---
tags:
  - projekt/pc-steuerung
  - thema/design
  - typ/plan
aliases:
  - Web-UI Konzept
  - Dashboard
  - Konzept HTML
  - Neues Web-UI
status: aktiv
stand: 2026-11-13
---

# Web-UI Konzept „Dashboard" (13.11.2026)

> Gelieferte Vorlage: `Konzept.html` (jetzt im Repo als `docs/webui-konzept.html`).
> Ausführlicher Abgleich: **`docs/webui-konzept.md`**. Status: **geprüft, nicht eingebaut**.

## Was die Vorlage zeigt

- **Sidebar:** Geräte-Liste (mehrere PCs) mit Online-Punkt, „＋ Gerät hinzufügen", Benutzer-Bereich
- **Header:** PC-Name, Online-Pill, klickbare **SIMULATION/ECHT**-Pill
- **Kacheln:** Status (groß, mit Herunterfahren/Ruhezustand bzw. Einschalten), CPU + Sparkline,
  RAM + Sparkline, Timer mit Fortschrittsring (1 h/2 h/Abbrechen), Energieplan als Segmente,
  Game Optimization, Ereignisse, „Benutzer & Rechte (später)"
- **Layout:** Handy zuerst, ab 700 px 4 Spalten, ab 900 px Sidebar; dunkel, Einblend-Animation
- Reines HTML/CSS/JS, **kein Build nötig** – passt zu `include_str!` im Agenten

## Was heute schon funktioniert (Endpunkte vorhanden)

`/status` · `/shutdown?delay=` · `/abort` · `/hibernate` · `/power-plan` · `/mode` ·
`/events` · `/plugins` · `/games` · `/game-config`

## Was fehlt oder nur halb passt

| Element | Problem |
|---|---|
| PC einschalten (WoL) | ✅ seit Phase 2: `/wake` ohne `mac` nutzt die MAC aus `device.json` |
| Timer-Ring | ✅ seit Phase 2: `/status` liefert `pending_seconds` (Gesamtdauer) |
| CPU-/RAM-Kacheln | ✅ seit Phase 2: `GET /metrics` (CPU-Stichproben, RAM, Laufzeit) |
| Geräte-Sidebar | ein Agent = ein PC; mehrere Geräte brauchen CORS/Auth oder einen Hub |
| Benutzer & Rollen | gibt es nicht (nur ein Basic-Auth-Paar) |
| Game-Kachel | keine Spielauswahl, kein Status, kein Stop – nur zwei Buttons |
| Ersteinrichtung | **fehlt komplett** (`/setup-status` + `/setup` + QR) – ohne sie kein Login |
| Verlauf/Logdatei | fehlt (heute zeigt die Web-UI `/log?lines=`) |
| Verbindungsfehler | kein 401-/Timeout-Zustand, nur „Offline"-Pill |
| Eigene Verzögerung | Konzept nur 60 s und 1 h/2 h, Sekundenfeld fehlt |
| CSRF | localhost-POSTs sind von fremden Seiten auslösbar (Kimi-Punkt 5) |
| Barrierefreiheit | 11-px-Labels, Status nur über Farbe, keine `aria`-Attribute |

## Entschieden (13.11.2026)

- [x] **Mehrere PCs?** → Phase 1 nur ein Gerät; die Sidebar wird zur Kopfzeile. Später: Geräteliste im Browser (localStorage) + CORS.
- [x] **Benutzer/Rollen?** → jetzt nein, Kachel raus. `credentials.json` bleibt erweiterbar.
- [x] **Polling oder SSE?** → Polling 2 s, pausiert bei verstecktem Tab (`visibilitychange`).
- [x] **Farbschema?** → Phase 1 nur dunkel, Farben als CSS-Variablen; Labels ≥ 12 px, Status mit Text **und** Symbol.
- [x] **`page.html` splitten?** → ja: `page.html` + `app.css` + `app.js`, je `include_str!`, eigene Routen ohne Login.
- [x] **Vorlage 1:1?** → Optik/Raster 1:1, Inhalte getauscht (CPU/RAM „–", Benutzer + Geräteliste raus, Setup/Verlauf/Fehler/Game-Auswahl rein).

## Vorschlag Reihenfolge

1. **Phase 1 (kein Backend):** Layout übernehmen, echte Aufrufe, MAC aus `/pairing-info`,
   Setup + Verlauf behalten, Fehlerzustand, Game-Kachel mit Auswahl/Status/Stop
2. **Phase 2 (Backend klein):** `GET /metrics` (CPU/RAM), `pending_seconds` in `/status`,
   Wake-Fallback-MAC, `X-PC-Steuerung`-Header
3. **Phase 3 (optional):** SSE, mehrere PCs, Rollen

## Umgesetzt (Phase 1, 13.11.2026)

- **Dateien:** `PC Agent Rust/src/page.html` (Gerüst), `src/app.css`, `src/app.js`
  → Routen `/`, `/app.css`, `/app.js` (ohne Login, `no-store`)
- **Kopfzeile** statt Sidebar: PC-Name, Status-Pill (Text + Symbol), Modus-Pill (klickbar)
- **Kacheln:** Status mit Herunterfahren/Ruhezustand/Abbrechen/Einschalten, CPU `–`,
  RAM `–`, Timer (1 h/2 h/eigene Sekunden, Ring nur mit gemerkter Dauer), Energieplan,
  Game Optimization (Auswahl, Automatisch, Stop, Scan, Plugin-Log), Ereignisse, Verlauf
- **Ersteinrichtung:** Name → optional Wake-on-LAN mit Adaptersuche → Zugangsdaten → QR
- **Fehlerzustand:** „Keine Verbindung" + Retry bei 401/Timeout
- **CSRF:** `X-PC-Steuerung: 1` bei allen POSTs; der Agent prüft es für localhost
  (Middleware `csrf_guard` → 403), `tray.ps1` sendet den Header ebenfalls
- **Polling:** alle 2 s, pausiert bei verstecktem Tab

**Verifiziert:** `cargo test` 36 grün; `/`, `/app.css` (text/css) und `/app.js`
(application/javascript) liefern 200; POST ohne Header 403, mit Header 200;
Setup-Ablauf (`/device-setup` → `/setup` → `setup_required: false`) durchgespielt;
`node --check` auf `app.js`; alle im JS benutzten Element-IDs existieren im HTML.
Ein Screenshot per Headless-Browser war in dieser Umgebung nicht möglich.

**Offen (Phase 2):** `/metrics` (CPU/RAM), `pending_seconds`, Wake-Fallback-MAC,
Hell/System-Theme, SSE, Geräteliste, Rollen.

## Phase 2 (13.11.2026)

- **`GET /metrics`** (`src/metrics.rs`): `cpu_percent`, `cpu_cores`, `ram_used_mb`,
  `ram_total_mb`, `ram_percent`, `uptime_s`. CPU aus zwei `GetSystemTimes`-Stichproben –
  die **erste** Antwort ist `null`, ab der zweiten (2 s später) steht ein Wert.
  RAM über `GlobalMemoryStatusEx`, Laufzeit über `GetTickCount64`. Keine neue Crate.
- **`/status` erweitert:** `pending_seconds` (Gesamtdauer), `device_name`, `wol_enabled`.
  Der Timer-Ring stimmt damit auch nach einem Neuladen.
- **`POST /wake` ohne `mac`:** nimmt die eingerichtete MAC aus `device.json`
  (422, wenn Wake-on-LAN aus ist). Das UI sendet keinen MAC-Parameter mehr.
- **Hell/System:** Palette als CSS-Variablen, Umschalter in der Kopfzeile
  (System → Hell → Dunkel, gespeichert in `localStorage.pcTheme`); „System" folgt
  `prefers-color-scheme` und reagiert live.
- **CPU-/RAM-Kacheln:** echte Werte plus Sparkline der letzten 30 Messungen.

**Verifiziert:** `cargo test` 41 grün; `/metrics` → erst `cpu_percent: null`, dann z. B.
`8.8` bei 8 Kernen, 31 % RAM; `/status` enthält `pending_seconds`; `/wake` ohne MAC →
422 ohne WoL, 200 mit MAC; `node --check` sauber; Agent auf Port 8000 neu gestartet.

**Offen (Phase 3):** SSE, Geräteliste über mehrere Agenten (CORS), Rollen/Rechte.

## Verwandt

- [[UI und Design]] – Regeln für UI-Aktionen
- [[Neue Ideen - gleiche App]] – Plugin/Game Optimization
- [[Kimi Review (2026-11-13)]] – CSRF, API/`/api/v1`, SSE
- [[API]] – Endpunkt-Referenz
