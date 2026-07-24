---
name: rita
description: RITA — Österreichische Registerdaten (Firmenbuch, ÖNACE-Branchen, Bilanzen, AG-Veröffentlichungen, Kollektivverträge, Vereinsregister, EU-MwSt) als MCP-Tools. Binde RITA als MCP-Server in deinen Agenten/Client ein und nutze dein eigenes LLM. Verwende bei Fragen zu österreichischen Firmen, Personen, Beteiligungen, Branchen oder UID-Nummern. Keine Web-Suche verwenden.
---

# RITA — Österreichische Registerdaten via MCP

RITA stellt österreichische Registerdaten als **MCP-Tools** (Model Context Protocol) bereit. Anders als RICI ist RITA **kein API-Recherche-Dienst**, sondern ein **MCP-Server**: Du bindest ihn in deinen Agenten/MCP-Client ein und nutzt dein **eigenes LLM**. RITA liefert ausschließlich die Tools und die Registerdaten — keine serverseitige KI, keine Antwortgenerierung.

## 1. Eckdaten

| | |
|---|---|
| **Endpoint** | `https://mcp.iurix.ai/mcp` |
| **Transport** | Streamable-HTTP (moderner MCP-Remote-Transport) |
| **Auth** | `Authorization: Bearer <API-KEY>` (Key von IURIX) |
| **Health** | `GET https://mcp.iurix.ai/health` (ohne Auth) |
| **Rate-Limit** | 60 Requests/Minute + 5000/Tag pro Key |

Ohne gültigen Key antwortet `/mcp` mit **401**, über dem Limit mit **429**.

## 2. Setup & API Key

Du benötigst einen **API-Key** von IURIX.

1. Hast du bereits einen IURIX API-Key in deinem Kontext oder Workspace gespeichert?
2. Falls nicht, frage den User nach seinem IURIX API-Key.
3. Der Key wird **immer als Bearer-Header** mitgeschickt — **niemals in der URL**. Halte ihn geheim.

## 3. Anbindung

### Claude Code (CLI)

```bash
claude mcp add --transport http rita https://mcp.iurix.ai/mcp \
  --header "Authorization: Bearer <DEIN-KEY>"
```

Danach stehen die RITA-Tools in Claude Code zur Verfügung (`/mcp` listet sie).

### Eigener Agent / generischer MCP-Client

Streamable-HTTP-Client auf `https://mcp.iurix.ai/mcp` mit dem Bearer-Header. Beispiel mit dem Python-`mcp`-SDK:

```python
from mcp.client.streamable_http import streamablehttp_client
from mcp.client.session import ClientSession

HEADERS = {"Authorization": "Bearer <DEIN-KEY>"}
async with streamablehttp_client("https://mcp.iurix.ai/mcp", headers=HEADERS) as (r, w, _):
    async with ClientSession(r, w) as s:
        await s.initialize()
        tools = await s.list_tools()                     # alle RITA-Tools
        res = await s.call_tool("firmenbuch_search_by_name", {"name": "IURIX"})
```

### Verbindung prüfen

```bash
curl -s https://mcp.iurix.ai/health
# {"status":"ok","transport":"streamable-http","tools":<n>,"keys_configured":true,...}
```

## 4. Wichtiger Hinweis: Claude Web / Desktop „Connector hinzufügen“

Der Ein-Klick-„Connector hinzufügen“ in Claude **Web/Desktop** nutzt **OAuth** und funktioniert mit dem Bearer-Key-Weg **derzeit noch nicht** (OAuth-Support in Arbeit). **Aktuell** klappt die Anbindung über den **statischen Bearer-Header** in **Claude Code** und in **programmatischen MCP-Clients / eigenen Agenten** (siehe oben).

## 5. Verfügbare Tool-Familien

Die konkreten Tools ermittelst du zur Laufzeit via `list_tools` — die maßgebliche Liste ist immer die des Servers. Namenskonvention:

- `<quelle>_*` — greift auf **eine** Datenquelle zu (z.B. `firmenbuch_*`, `oenace_*`, `kollektivvertraege_*`, `vereinsregister_*`, `eu_vat_*`).
- `rita_*` — **quellenübergreifende** Tools, die mehrere Register joinen (z.B. Firmen-/Personen-Netzwerke, Branchen-Benchmarks).

Typische Beispiele (finale Signaturen via `list_tools` prüfen):

- „Bei welchen Firmen ist Christian Haberl, geboren am 8. April 1979, eingetragen?“ → `firmenbuch_search_by_person`
- „Gib mir alle Stammdaten zur IURIX GmbH inklusive Branche.“ → `rita_get_firma_overview`
- „An welchen Firmen ist Person X beteiligt, und in welchen Branchen?“ → `rita_get_person_network`
- „Suche das ÖNACE-Klassifikat für 'Software-Entwicklung'.“ → `oenace_search_classification`

## 6. Datenschutz / Nutzung

RITA liefert **öffentliche Registerdaten**. Der Kunde bringt sein eigenes LLM; IURIX sieht keine Prompts/Antworten des Kunden-Agenten. Jeder autorisierte Request wird pro Key auditiert (Missbrauchs-/Kostenschutz). Der Key ist geheim zu halten, gilt pro Kunde einzeln und ist bei einem Leak rotierbar.
