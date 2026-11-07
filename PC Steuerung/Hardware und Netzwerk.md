---
tags:
  - projekt/pc-steuerung
  - thema/hardware
  - typ/doku
aliases:
  - Hardware
  - Netzwerk
  - WoL-Setup
status: aktiv
stand: 2026-11-06
---

# Hardware und Netzwerk

## PC
- Windows 10/11
- Ethernet empfohlen (WoL)
- Feste IP per DHCP-Reservierung (z. B. 192.168.1.100)
- MAC-Adresse der Netzwerkkarte notieren

## Wake-on-LAN
- BIOS/UEFI: „Wake on LAN“ / „Power On by PCI-E“ aktivieren
- Netzwerkkarte: Energieverwaltung → „Gerät kann den Computer aus dem Ruhezustand aktivieren“
- Erweitert: „Wake on Magic Packet“ aktivieren
- Schnellstart deaktivieren (nicht `powercfg /h off`)
- Broadcast-Adresse (z. B. 192.168.1.255 oder 255.255.255.255)

## Firewall
- Port 8000 TCP nur LocalSubnet
- Kein Port-Forwarding, kein Internet
- Profil „Privat“ für das Heimnetz

## Agent
- Port 8000 TCP
- start.bat / Autostart
- Log: agent.log