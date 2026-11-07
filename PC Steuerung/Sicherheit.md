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
- Passwort nur als **PBKDF2-Hash** in `PC App/credentials.json` (gitignored)
- Alter `PC_AGENT_TOKEN`/`X-Token` bleibt als Fallback
- **Simulation** ist Standard; Echtmodus nur bewusst

## Vor Veröffentlichung
- [ ] Zugangsdaten eingerichtet
- [ ] Firewall auf LocalSubnet, kein Port-Forwarding
- [ ] Echtmodus nur bewusst
- [ ] Persönliche Daten entfernt (LAN-IPs, Hostnamen)
- [ ] Keine Secrets in der Git-Historie
- [ ] Pre-Push-Hook aktiv
- [ ] Checkliste in `SECURITY.md`

## Offene Punkte
- Passwort liegt in der Android-App im Klartext in SharedPreferences
- `usesCleartextTraffic` ist aktiv (HTTP im Heimnetz)
- Beides für lokale Debug-Nutzung okay, vor Veröffentlichung prüfen