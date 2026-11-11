---
tags:
  - typ/plan
  - thema/entscheidung
aliases:
  - Offene Fragen
  - Entscheidungen offen
  - Fragen
status: offen
stand: 2026-11-11
---

# Offene Fragen

> [!tip] Wozu diese Seite?
> Alles, was im Gespräch offen bleibt, landet **hier** statt in einer Rückfrage.
> Jede Zeile: Frage → aktuelle Annahme (was der Code jetzt tut) → wie man sie kippt.

## 1. Echtmodus als Standard

**Frage:** Soll der Rust-Agent ohne `PC_AGENT_SIMULATE` wirklich echte Befehle ausführen?

- **Aktuell:** `agent-manager.ps1` setzt `PC_AGENT_SIMULATE=1`, wenn die Variable fehlt –
  der Agent startet also **simuliert**. Die App wertet „Simulation" jetzt als Ausnahme und
  zeigt das Banner nur noch im Simulationsfall.
- **Kippen:** in `agent-manager.ps1` die Zeile
  `if (-not $env:PC_AGENT_SIMULATE) { $env:PC_AGENT_SIMULATE = '1' }` auf `'0'` ändern –
  oder `PC_AGENT_SIMULATE=0` in der Umgebung setzen.
- **Risiko:** Herunterfahren/Ruhezustand wirken sofort. Nur mit Countdown testen.

## 2. Web-UI aus der App an/aus schalten

**Frage:** Soll die Android-App die PC-Weboberfläche deaktivieren können?

- **Aktuell:** nein. Es gibt nur den Link „PC-Weboberfläche öffnen" (`/`).
- **Nötig:** neuer Agent-Endpunkt (z. B. `POST /web-ui?enabled=false`), der die Route `/`
  abschaltet (oder 404 liefert), plus Schalter in den Einstellungen.
- **Achtung:** Wenn die Web-UI aus ist, kann man die Ersteinrichtung nicht mehr vom PC aus
  starten – Tray muss dann einen Weg anbieten.

## 3. Routinen – wie viel Automatik?

**Frage:** Reine Zeitpläne (23:00 aus) oder echte Wenn-Dann-Routinen?

- **Aktuell:** nur Konzept, siehe [[Routinen]].
- **Entscheidung nötig:** Auslöser (Uhrzeit / App-Knopf / Ereignis) und ob Routinen im
  Agenten (Windows-Aufgabenplanung) oder in der App laufen.
- **Nebenbedingung:** „PC offen halten ohne PIN" ist eine Windows-Einstellung
  (Anmeldeoptionen) – nicht etwas, das die App still umstellen sollte.

## 4. Markt+ wirklich als Markt

**Frage:** Reicht der Katalog mit Installieren/Deinstallieren, oder braucht es mehr
(Beschreibungen, Screenshots, Bewertungen, Versionierung)?

- **Aktuell:** Katalog aus `module-catalog/modules.json`, Karten mit Version-Badge,
  Basis-Funktionen als Chips. Kein Preis-, Bewertungs- oder Update-Fluss.

## 5. Vault-Inhalte für Command Center

**Frage:** Soll Command Center weiter nur dokumentiert bleiben oder wieder aktiv werden?

- **Aktuell:** doku-only, keine Schreibpfade (siehe [[Projekte]]).
