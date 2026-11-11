# aeogeo plugin

[![Listed on mcpservers.org](https://mcpservers.org/badge.svg)](https://mcpservers.org/servers/aeogeosite/aeogeo)

Deterministic AEO/GEO checks and a source-cited rulebook for AI search, for the agent you already use. One MCP server plus three skills:

- **aeogeo-audit:** review a page or its HTML for AI-search readiness. The checks are deterministic; your agent judges meaning and context.
- **aeogeo-optimize:** make scoped, reviewable changes in a site or content repo, then re-check them.
- **aeogeo-research:** answer questions about current AEO/GEO guidance, with each rule's sources, confidence and what changed.

aeogeo doesn't query AI engines and doesn't promise rankings or citations. It checks pages and supplies maintained rules; your agent decides what applies.

Documentation: https://aeogeo.site/docs/mcp/

## Install

**Claude Code:** add this repository as a plugin marketplace, then install the `aeogeo` plugin:
```
/plugin marketplace add aeogeosite/aeogeo
/plugin install aeogeo@aeogeo
```

**Any MCP client:** add the server `https://mcp.aeogeo.site` (Streamable HTTP). Free tools need no key or sign-in. For Pro tools, send `Authorization: Bearer <your key>` to that address, or connect `https://mcp.aeogeo.site/mcp`, which signs you in with OAuth (Claude Desktop and other clients that add remote servers as connectors). Setup per client: https://aeogeo.site/docs/mcp/

**Cline:** see [llms-install.md](llms-install.md) (one settings entry; set `"type": "streamableHttp"`).

**Skills only** (clients that read `SKILL.md`, such as Claude Code, Cursor or Codex): copy the folders in `skills/` into the client's skills directory.

## Free and Pro

| | Free | Pro ($29/month or $290/year) |
|---|---|---|
| Tools | `plan_audit`, `scan_url`, `list_categories`, `fetch_sample_rules`, `get_hook_snippet`, `export_report` | adds `lint_html`, `lint_text`, `audit_schema`, `fetch_rules`, `search_rules`, `get_insights`, `get_changelog`, `scan_site` and more |
| Scan output | grades plus fixes for failed L0–L3 checks | every check, naming the rule behind each fix; rules come with their sources and confidence |

Plans: https://aeogeo.site/pricing/

## Data and security

No AI model runs on your content, and scanned pages aren't stored. Details: https://aeogeo.site/docs/security/ and https://aeogeo.site/privacy/
