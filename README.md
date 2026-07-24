# IURIX Agent Skills

Offizielle Skills zur Integration von IURIX-Diensten in KI-Agenten. 

Jedes Skill ist eine reine, plattformunabhängige Markdown-Datei (`SKILL.md`), die von jedem fähigen Agenten direkt gelesen und ausgeführt werden kann.

## Verfügbare Skills

### 1. RICI — Österreichische & EU-Rechtsrecherche (`/rici`)
Durchsucht die angebundenen Rechtsdatenbanken über die RICI API: Bundesrecht, Judikatur, Findok (Steuerrecht), Landesrecht, Gesetzesmaterialien, Kollektivverträge (WKO), EU-Recht (EUR-Lex) sowie EuGH- (Curia) und EGMR-Judikatur (HUDOC).

**Installation:**
Füge die Datei `rici/SKILL.md` als Instruktion oder Context in den Workspace deines Agenten ein. Der Agent wird dich bei der ersten Nutzung selbstständig nach deinem IURIX API-Key fragen.

### 2. RITA — Österreichische Registerdaten (`/rita`)
Stellt österreichische Registerdaten als **MCP-Tools** bereit: Firmenbuch, ÖNACE-Branchen, Bilanzen, AG-Veröffentlichungen, Kollektivverträge, Vereinsregister und EU-MwSt. RITA ist ein gehosteter MCP-Server (`https://mcp.iurix.ai/mcp`) — du bindest ihn in deinen Agenten/Client ein und nutzt dein eigenes LLM.

**Installation:**
Siehe `rita/SKILL.md`. In Claude Code:
```bash
claude mcp add --transport http rita https://mcp.iurix.ai/mcp \
  --header "Authorization: Bearer <DEIN-KEY>"
```

*(FIRE — Deep Research befindet sich in früher Beta; ein Skill folgt, sobald der Drittanbieter-Zugang steht.)*

## API-Key

Jedes Skill benötigt einen **IURIX API-Key**, den du selbst zur Laufzeit angibst — dieses Repository enthält **keine** Credentials. Der Key gilt pro Kunde und ist bei einem Leak rotierbar.

## Mitwirken & Support

- **Fragen / Bugs:** Lege ein [Issue](https://github.com/IURIX-GmbH/iurix-skills/issues) an (Templates vorhanden). Dies ist ein **öffentliches** Repository — bitte **keine** vertraulichen oder personenbezogenen Daten in Issues.
- **Beiträge:** siehe [`CONTRIBUTING.md`](CONTRIBUTING.md).
- **Sicherheitslücken:** **nicht** öffentlich melden — siehe [`SECURITY.md`](SECURITY.md).

## Lizenz

Lizenziert unter der [Apache License 2.0](LICENSE). „IURIX“, „RICI“, „RITA“ und „FIRE“ sind Marken der IURIX GmbH (siehe [`NOTICE`](NOTICE)).