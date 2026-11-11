---
tags:
  - projekt/pc-steuerung
  - typ/plan
  - thema/routinen
aliases:
  - Routinen
  - Routine-Feature
  - Automation
status: geplant
stand: 2026-11-11
---

# Routinen (geplant)

> **Der Zeitplan ist gestrichen.** Statt „täglich 23:00 herunterfahren" bauen wir
> **Routinen**: Auslöser → Aktionen, frei kombinierbar. Diese Notiz ist das Konzept,
> noch kein Code.

## Warum

Ein fester Zeitplan kann nur „um X Uhr aus". Gewünscht ist mehr: *„wenn ich diesen Knopf
drücke, geht der PC an und ist in diesem Modus"* oder *„um 23:00 aus, aber nur wenn
nichts läuft"*. Das ist eine Wenn-Dann-Liste, kein Kalender.

## Bausteine

**Auslöser (Trigger)**

| Auslöser | Beispiel | Läuft wo |
|---|---|---|
| Uhrzeit / Wochentag | täglich 23:00, Mo–Fr 07:30 | Agent (Windows-Aufgabenplanung) |
| App-Knopf / Shortcut | „Feierabend"-Button in der App oder als Android-Shortcut | App → Agent |
| Ereignis | PC startet, Agent startet, Countdown abgebrochen | Agent |
| Manuell | Web-UI-Button | Web-UI → Agent |

**Aktionen**

- `POST /shutdown?delay=` (Herunterfahren, mit Countdown und Abbrechen)
- `POST /wake?mac=` (Einschalten – nur wenn der Agent auf einem zweiten Gerät läuft;
  WoL aus dem ausgeschalteten PC ist nicht möglich)
- `POST /power-plan?name=` (Energieplan setzen)
- `POST /hibernate` (Ruhezustand)
- Kette: mehrere Aktionen mit Verzögerung (z. B. „Plan auf Höchstleistung, dann in 2 h aus")

## Datenmodell (Vorschlag)

```json
{
  "id": "feierabend",
  "name": "Feierabend",
  "enabled": true,
  "trigger": { "type": "manual", "label": "Feierabend" },
  "actions": [
    { "type": "power-plan", "name": "saver" },
    { "type": "shutdown", "delaySeconds": 120 }
  ]
}
```

Ablage im Datenordner (`routines.json`), API: `GET/POST/DELETE /routines`, Auslösen per
`POST /routines/{id}/run`. Die App zeigt Routinen als Karten mit „Ausführen"-Knopf.

## Der PC muss „offen" bleiben

Damit Routinen nach einem Neustart oder aus dem Ruhezustand laufen, muss der PC
**ohne Passwort-Eingabe** weiterarbeiten. Das ist eine **Windows-Einstellung**, keine
App-Funktion:

- *Einstellungen → Konten → Anmeldeoptionen* → „Kennwort nach dem Aufwachen verlangen: Nie"
- Optional: automatische Anmeldung (Autologon) – sicherheitsrelevant, nur bewusst
- Bildschirmsperre per Richtlinie/Energieplan steuern, nicht per App

> [!warning] Sicherheit
> Automatische Anmeldung und deaktivierte Sperre senken den Schutz physisch am Gerät.
> Das bleibt eine bewusste Entscheidung des Nutzers; die App darf so etwas nie still ändern.

## Offen

- Läuft die Routineplanung im Agenten (Aufgabenplanung) oder als Schleife im Agent-Prozess?
- Wie werden verpasste Auslöser nachgeholt (PC war aus)?
- Wie zeigt die App Konflikte an (z. B. Routine will herunterfahren, während ein Timer läuft)?

Siehe auch [[Offene Fragen]] (Punkt 3).
