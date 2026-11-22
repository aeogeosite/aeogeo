---
name: aeogeo-optimize
description: Implement scoped AEO/GEO improvements in a website or content repository using aeogeo's current evidence, contextual agent judgment, deterministic checks, and existing project tests. Use when the user asks to change code.
compatibility: Needs the aeogeo MCP server at https://mcp.aeogeo.site (free without a key; Pro with a key or sign-in).
---

# aeogeo Optimize (`aeogeo-optimize`)

Change only what the user's request authorizes. Use aeogeo for **current research and repeatable checks**; use your host model to decide what actually makes sense for the content and repository.

## Connect the aeogeo MCP server

The tools named below come from the aeogeo MCP server (Streamable HTTP). If `tools/list` shows no aeogeo tools, ask the user to add the server; do not guess what a tool would return.

- **Free:** `https://mcp.aeogeo.site` with no key. Free tools only.
- **Pro with a key:** the same address with the header `Authorization: Bearer <key>`. Keep the key in the `AEOGEO_API_KEY` environment variable; never paste it into a chat or commit it.
- **Pro with sign-in (OAuth):** `https://mcp.aeogeo.site/mcp`. The client opens a sign-in for the email used at checkout.

In Claude Code: `claude mcp add --transport http aeogeo https://mcp.aeogeo.site`. Other clients: <https://aeogeo.site/docs/mcp/>.

If a Pro tool answers `requires_pro`, say which part of the task needs Pro, finish what the free tools can do, and do not retry the call.

## Tier & tool availability

- **Keyed (Pro/Team):** `lint_html`, `lint_text`, `audit_schema` for local pre-deploy verification; `search_rules`, `fetch_rules`, `get_insights` for current guidance; `scan_url` after deployment.
- **Keyless (Free fallback):** If only free tools are available in `tools/list`, use `fetch_sample_rules` for rule guidance and `scan_url` on deployed public URLs.

## Runbook

1. Inspect project conventions, target pages/templates, existing tests, and content meaning before editing.
2. Retrieve focused current aeogeo guidance (`search_rules` / `fetch_rules`) and run the applicable baseline lint (`lint_html` for HTML, `lint_text` for Markdown/plain text, `audit_schema` for JSON-LD, passing `content_type` when known).
3. Separate findings into:
   - **Hard technical blockers** (`judge: engine`, `severity: error` — e.g., page-level `noindex`, broken JSON-LD syntax, missing/conflicting canonical, unseen schema claims, blocked AI search crawlers).
   - **Strongly supported improvements** (`confidence: strong | moderate`).
   - **Optional / experimental hypotheses** (`confidence: limited | hypothesis`, `judge: agent`). Do not force optional FAQ schema, Article schema on non-articles, or artificial numeric leads onto prose where they hurt clarity.
4. Make minimal, reviewable edits that preserve factual accuracy. Never invent prices, ratings, review counts, author credentials, or citations to satisfy a check (`FR-TRUST-8`).
5. Re-run repository tests and the applicable aeogeo lint tool (`lint_html` / `lint_text` / `audit_schema`) to verify the fix locally. If the change is not deployed yet, do not claim `scan_url` verified production.
6. Summarize verified technical fixes, semantic edits, skipped/rejected heuristic nags, and any remaining items requiring human sign-off (`propose: true`).

## Verification boundary

A deterministic pass proves that syntax, crawler policy, or structural hygiene matches the rulebook for that input — not that an AI engine will cite the page. Keep `confidence: "hypothesis"` recommendations labeled as experiments after implementation.

## Safety

Do not invoke `export_report`, `set_retention_webhook`, or production deployment commands unless explicitly requested by the user.
