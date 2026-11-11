---
tags:
  - projekt/pc-steuerung
  - thema/android
  - thema/design
  - typ/plan
aliases:
  - UI und Design
  - Design
  - UI-Backlog
status: aktiv
stand: 2026-11-11
---

# UI und Design

> Was in der App bewusst so ist – und was aus dem Feedback vom 2026-11-11 noch offen ist.

## Entschieden (v0.9.5)

| Thema | Entscheidung | Warum |
|---|---|---|
| Modus-Anzeige | Banner zeigt **SIMULATION** (Ausnahme), Echt ist Standard | Simulation war früher der Sonderfall mit Warnton – jetzt umgekehrt |
| Statuspunkt | grün = online · gelb = Agent antwortet nicht · **rot = PC aus** · grau = unbekannt | „PC aus" fällt sofort auf, vorher war es unauffälliges Grau |
| Widget | **entfernt** | war nie gewollt, kostete Pflege und Testfläche |
| Einrichtung | „Abbrechen und zurück" (`OutlinedButton`), verwirft ein leeres neues Profil | vorher war man im Assistenten gefangen |
| Theme-Wechsel | kurzer Fade (Alpha 1 → 0,55 → 1) statt hartem Umschalten | „ploppt nicht mehr ins Gesicht", kein Zustandsverlust |
| Markt+ | Basis-Chips („Immer dabei") + Erweiterungen mit Versions-Badge, Refresh als Icon | war vorher nur eine Liste + ein Knopf |
| Energieplan | animierte Aktiv-Leiste oben, aktiver Plan skaliert leicht | Auswahl ist ohne Lesen erkennbar |
| Texte | zentrale `strings.xml` für Navigation, Status, Einrichtung, Einstellungen | Vorbereitung für Übersetzungen, saubere Diffs |

## Offen (Backlog)

- [ ] Restliche UI-Texte aus `MainActivity.kt` nach `strings.xml` (Timer-, Dialog- und Log-Texte)
- [ ] Markt+ tiefer ausbauen: Detailseite pro Modul, Update-Hinweis wenn Katalog-Version > installiert
- [ ] Web-UI in der App an/aus schalten (braucht Agent-Endpunkt, siehe [[Offene Fragen]])
- [ ] Feinere Animationen: Statuswechsel-Übergang, Snackbar-Position, Pull-to-Refresh-Feder
- [ ] Eigene App-Farben statt Dynamic Color (falls das Design „final" wird)
- [ ] Screenshots für README aus dem echten Gerät (Gerät: SM-A556B, `RZCX30F396P`)

## Regeln

- Deutsche Strings, Du-Form, keine Emojis in der App-UI (nur im README).
- Jede neue UI-Aktion braucht: Busy-Sperre, Snackbar-Feedback, Bestätigung bei destruktiven Aktionen.
- Bedienung muss im Simulationsmodus **identisch** aussehen, nur der Banner unterscheidet sich.
