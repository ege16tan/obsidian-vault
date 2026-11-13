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
status: offen
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
| PC einschalten (WoL) | `/wake` braucht `mac`, die MAC steckt in `device.json` (kommt über `/pairing-info`) |
| Timer-Ring | `/status` liefert nur die **Restzeit**, nicht die Gesamtdauer → Ring stimmt nicht |
| CPU-/RAM-Kacheln | Agent liefert **keine** Messwerte (im Konzept nur Attrappe) |
| Geräte-Sidebar | ein Agent = ein PC; mehrere Geräte brauchen CORS/Auth oder einen Hub |
| Benutzer & Rollen | gibt es nicht (nur ein Basic-Auth-Paar) |
| Game-Kachel | keine Spielauswahl, kein Status, kein Stop – nur zwei Buttons |
| Ersteinrichtung | **fehlt komplett** (`/setup-status` + `/setup` + QR) – ohne sie kein Login |
| Verlauf/Logdatei | fehlt (heute zeigt die Web-UI `/log?lines=`) |
| Verbindungsfehler | kein 401-/Timeout-Zustand, nur „Offline"-Pill |
| Eigene Verzögerung | Konzept nur 60 s und 1 h/2 h, Sekundenfeld fehlt |
| CSRF | localhost-POSTs sind von fremden Seiten auslösbar (Kimi-Punkt 5) |
| Barrierefreiheit | 11-px-Labels, Status nur über Farbe, keine `aria`-Attribute |

## Entscheidungen vor dem Einbau

- [ ] **Mehrere PCs?** (a) nur ein Gerät, (b) Liste im Browser + CORS, (c) Hub-Dienst
- [ ] **Benutzer/Rollen jetzt oder später?** (Empfehlung: später, nach Geräte-Token)
- [ ] **Polling 2 s behalten oder SSE?** (Empfehlung: Polling zuerst)
- [ ] **Farbschema:** bewusst dunkel-only oder hell/dunkel/System wie die App?
- [ ] **`page.html` splitten** (`ui.css` + `ui.js` + HTML) oder eine Datei lassen?
- [ ] Vorlage 1:1 übernehmen oder Kacheln anpassen (z. B. Verlauf statt „Benutzer bald")?

## Vorschlag Reihenfolge

1. **Phase 1 (kein Backend):** Layout übernehmen, echte Aufrufe, MAC aus `/pairing-info`,
   Setup + Verlauf behalten, Fehlerzustand, Game-Kachel mit Auswahl/Status/Stop
2. **Phase 2 (Backend klein):** `GET /metrics` (CPU/RAM), `pending_seconds` in `/status`,
   Wake-Fallback-MAC, `X-PC-Steuerung`-Header
3. **Phase 3 (optional):** SSE, mehrere PCs, Rollen

## Verwandt

- [[UI und Design]] – Regeln für UI-Aktionen
- [[Neue Ideen - gleiche App]] – Plugin/Game Optimization
- [[Kimi Review (2026-11-13)]] – CSRF, API/`/api/v1`, SSE
- [[API]] – Endpunkt-Referenz
