# Installing the aeogeo MCP server

aeogeo is a hosted (remote) MCP server. There is nothing to clone, build or run locally: add one entry to the client's MCP settings and it connects over Streamable HTTP.

## Cline

Add this to `cline_mcp_settings.json` (Cline → MCP Servers → Configure → Configure MCP Servers):

```json
{
  "mcpServers": {
    "aeogeo": {
      "type": "streamableHttp",
      "url": "https://mcp.aeogeo.site",
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

Keep `"type": "streamableHttp"`: without it Cline falls back to the legacy SSE transport and the connection never completes.

This connects the free tools (`plan_audit`, `scan_url`, `list_categories`, `fetch_sample_rules`, `get_hook_snippet`, `export_report`). They need no key and no sign-in.

### Full access (optional)

Only if the user has an aeogeo key (`ag_live_…`, emailed once at checkout) or account. Pick one; never invent or guess a key.

- **Key:** keep the URL above and add a header. Ask the user to paste the key into the settings file themselves rather than into the chat.
  ```json
  "headers": { "Authorization": "Bearer ag_live_..." }
  ```
- **Sign-in:** set `"url": "https://mcp.aeogeo.site/mcp"` with no headers. Cline opens the aeogeo sign-in page in the browser (OAuth); the user enters their email and approves.

## Check it works

Call `list_categories` (no arguments) or `scan_url` with `{"url": "https://example.com"}`. A scan returns grades and fixes for failed checks. A grade or a pass is not a citation: aeogeo checks pages and supplies rules, it does not query AI engines.

## Other clients

The same server works in any MCP client that supports Streamable HTTP. Per-client setup: https://aeogeo.site/docs/mcp/

## Skills (optional)

`skills/` holds three `SKILL.md` folders (audit, optimize, research) that tell an agent how to use the tools. Copy them into the client's skills directory if it reads `SKILL.md` files; Cline works without them.
