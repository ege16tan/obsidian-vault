---
tags:
  - typ/doku
  - thema/tools
  - thema/mcp
aliases:
  - Tools und MCP
  - MCP-Server
  - Tavily
  - Lovable
status: aktiv
stand: 2026-11-12
---

# Tools und MCP

> Welche externen Werkzeuge mein Agent hier benutzen kann, wie sie eingerichtet sind und
> wo Zugangsdaten liegen. **Keine Tokens oder Keys in dieser Notiz.**

## MCP-Server (Cline)

Konfiguration: `C:\Users\egeta\.cline\data\settings\cline_mcp_settings.json`

| Name | Transport | Ziel | Anmeldung |
|---|---|---|---|
| `android-studio` | streamable HTTP | `http://127.0.0.1:64342/stream` | keine (localhost, JetBrains-Plugin `com.intellij.mcpServer`) |
| `obsidian` | streamable HTTP | `http://127.0.0.1:27123/mcp/` | Bearer-API-Key des Plugins *Local REST API with MCP* |
| `lovable` | stdio-Brücke (`mcp-remote`) | `https://mcp.lovable.dev` | OAuth (Browser), Token in `~/.mcp-auth/` |

- **Android Studio:** 25 Tools (build_project, analyze_calls, apply_patch, search_*, …).
  Tools brauchen meist `projectPath`; offenes Projekt: `D:/Goodnight my pc/Android App`.
- **Obsidian:** 20 Tools (vault_read, vault_write, search_query, tag_list, …). HTTPS-Port
  27124 scheitert an Node-TLS (self-signed), deshalb der Insecure-Port 27123 (nur localhost).
- **Lovable:** 42 Tools (get_me, list_projects, create_project, send_message, get_diff,
  read_file, deploy_project, query_database, Analytics …). Server-Version geprüft: `Lovable 1.23.6`.
  Achtung: `create_project`/`send_message` verbrauchen Lovable-Credits.

> [!note] Neue MCP-Server
> `cline mcp add <name> <url> --transport streamable-http --yes` für Server ohne OAuth;
> bei OAuth-Servern die `mcp-remote`-Brücke verwenden:
> `cline mcp add <name> -- node <npm>\node_modules\mcp-remote\dist\proxy.js <url>`.
> Einmal im Terminal starten, im Browser autorisieren, danach liegen die Tokens in `~/.mcp-auth`.

## Tavily (Web-Suche, Extract, Research)

Eingerichtet am 2026-11-12 nach `https://tavily.com/agent-setup/SKILL.md`:

| Ebene | Zustand |
|---|---|
| CLI | `tvly` 0.2.1 (`tavily-cli` via pip, `…\Roaming\Python\Python315\Scripts\tvly.exe`), Ordner ist in der User-`PATH` |
| Auth | OAuth, `tvly auth --json` → `{"authenticated": true, "method": "oauth"}`; Konfig in `~/.tavily/config.json` |
| Agent-Skills | global installiert: `tavily-search`, `tavily-sdk`, `tavily-entity`, `tavily-list`, `tavily-monitor`, `tavily-research` (`tavily-ai/skills`) |
| Test | `tvly search "Tavily Search API" --json` → echte Treffer (≈16 KB JSON) |

Nützliche Befehle:

```powershell
tvly search "suchbegriff" --json
tvly extract "https://example.com" --json
tvly research "frage" --json
tvly auth --json
```

> [!info] Hinweis
> Die SKILL.md nennt zusätzlich `tavily-cli`, `tavily-extract`, `tavily-map`, `tavily-crawl`;
> das Repo `tavily-ai/skills` liefert aktuell die sechs oben installierten Skills.
> Use-Case-Skills (Academic / People / Product-Competitor) lassen sich bei Bedarf nachziehen.

## Verwandt

- [[Credits]] · [[PC & Arbeitsumgebung]] · [[Projekte]]
