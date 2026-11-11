---
tags:
  - projekt/pc-steuerung
  - thema/sicherheit
  - typ/doku
aliases:
  - Sicherheit
  - Security
  - Auth
status: aktiv
stand: 2026-11-06
---

# Sicherheit

## Modell
- **Basic Auth** für LAN-Zugriffe (Benutzername/Passwort)
- **localhost** (PC selbst) ohne Login → Tray + Web-UI
- Passwort nur als **PBKDF2-Hash** in `%LOCALAPPDATA%\PC Steuerung\credentials.json` (gitignored)
- Alter `PC_AGENT_TOKEN`/`X-Token` bleibt als Fallback
- **Simulation** ist bewusst die Ausnahme (Banner in der App); Echtbetrieb ist Standard

## Android-Seite
- Passwort liegt **verschlüsselt** in SharedPreferences (AndroidKeyStore, AES-256/GCM, `Crypto.kt`)
- `usesCleartextTraffic` ist aktiv, weil der Agent nur HTTP im Heimnetz spricht – nur im eigenen WLAN okay
- Keine Zugangsdaten im QR-Code (nur PC-Name, IP, Port, optional WoL-MAC)

## Vor Veröffentlichung
- [ ] Zugangsdaten eingerichtet
- [ ] Firewall auf LocalSubnet, kein Port-Forwarding
- [ ] Echtmodus nur bewusst
- [ ] Persönliche Daten entfernt (LAN-IPs, Hostnamen)
- [ ] Keine Secrets in der Git-Historie
- [ ] Pre-Push-Hook aktiv
- [ ] Checkliste in `SECURITY.md`

## Offene Punkte
- `usesCleartextTraffic` ist aktiv (HTTP im Heimnetz) – bewusst, aber vor Veröffentlichung prüfen
- Automatische Anmeldung / deaktivierte Sperre (für Routinen) bleibt eine bewusste Windows-Entscheidung, siehe [[Routinen]]