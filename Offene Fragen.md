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

## 1. Echtmodus als Standard — **erledigt (12.11.2026)**

Der Manager setzt jetzt `PC_AGENT_SIMULATE=0`, wenn die Variable fehlt: **echte Aktionen
sind Standard**. Simulation nur, wenn `PC_AGENT_SIMULATE=1` ausdrücklich gesetzt ist oder
über `/mode?simulate=true` eingeschaltet wird (`http://127.0.0.1:8000/mode?simulate=true`).

> [!warning] Konsequenz
> Jeder Shutdown-Aufruf wirkt sofort. Immer mit Countdown (≥ 60 s) arbeiten, damit `/abort`
> erreichbar bleibt. Deshalb ist der Abbrechen-Knopf in der App nie hinter einem Dialog.

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

## 4. Markt+ – echter Download oder ehrliche Feature-Auswahl?

**Frage:** Soll der Marktplatz wirklich Module *herunterladen* (Platz sparen), oder reicht
eine ehrliche Aktivieren/Deaktivieren-Liste?

**Stand jetzt (v0.9.6):** Kein Download. Die Karten sagen offen, dass die Erweiterungen in
der App enthalten sind; der GitHub-Katalog steuert nur, welche Einträge erscheinen.

**Optionen, wenn echtes Nachladen gewünscht ist:**

| Option | Platzersparnis | Aufwand/Risiko |
|---|---|---|
| Play Feature Delivery (dynamische Feature-Module) | hoch | nur mit Play-Store-Verteilung; Sideload/Obtainium fällt weg |
| Zusätzliche APKs / Split-APKs | mittel | jede APK ist eine eigene App; geteilter Code nicht möglich |
| Module als Compose-„Screens aus JSON" (Remote-UI) | gering | kein fremder Code, aber nur einfache Oberflächen möglich |
| Dynamisches Code-Laden (`DexClassLoader`) | hoch | Sicherheitsrisiko, von Play verboten, Updates fragil |
| **Empfehlung:** Module bleiben drin, Markt+ = Feature-Schalter + Katalog | keine | robust, offlinefähig, kein Sicherheitsrisiko |

Die App ist mit R8 bereits auf ~22 MB geschrumpft; echte Größenprobleme gibt es derzeit nicht.

## 5. Vault-Inhalte für Command Center

**Frage:** Soll Command Center weiter nur dokumentiert bleiben oder wieder aktiv werden?

- **Aktuell:** doku-only, keine Schreibpfade (siehe [[Projekte]]).
