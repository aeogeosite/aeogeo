---
name: aeogeo-research
description: Answer questions about current AEO/GEO research, changing AI-search recommendations, evidence conflicts, source provenance, or recent platform behavior by retrieving the living aeogeo MCP rulebook and insights.
compatibility: Needs the aeogeo MCP server at https://mcp.aeogeo.site (free without a key; Pro with a key or sign-in).
---

# aeogeo Research (`aeogeo-research`)

aeogeo is the up-to-date research source; your job is to reason about **what the evidence supports** for this user. Do not rely on static model priors for changing platform behaviors when current aeogeo material is relevant.

## Connect the aeogeo MCP server

The tools named below come from the aeogeo MCP server (Streamable HTTP). If `tools/list` shows no aeogeo tools, ask the user to add the server; do not guess what a tool would return.

- **Free:** `https://mcp.aeogeo.site` with no key. Free tools only.
- **Pro with a key:** the same address with the header `Authorization: Bearer <key>`. Keep the key in the `AEOGEO_API_KEY` environment variable; never paste it into a chat or commit it.
- **Pro with sign-in (OAuth):** `https://mcp.aeogeo.site/mcp`. The client opens a sign-in for the email used at checkout.

In Claude Code: `claude mcp add --transport http aeogeo https://mcp.aeogeo.site`. Other clients: <https://aeogeo.site/docs/mcp/>.

If a Pro tool answers `requires_pro`, say which part of the task needs Pro, finish what the free tools can do, and do not retry the call.

## Tier & tool availability

- **Keyed (Pro/Team):** `search_rules`, `fetch_rules`, `get_insights`, `get_changelog`, `list_categories`.
- **Keyless (Free fallback):** If `tools/list` shows only free tools, use `list_categories` and `fetch_sample_rules` for public rule statements (`confidence`, `level`, `why`, `anti_pattern`) and note that full source URLs, remediation, and `get_insights` require a Pro key.

## Runbook

1. Define topic, target AI platform/engine, relevant dates, content genre, and the decision the user is trying to make.
2. Use `search_rules`, `fetch_rules`, `get_insights`, `get_changelog`, and `list_categories` selectively to retrieve current claims, source dates, and disagreements.
3. Distinguish evidence classes (`evidence[].type`) and confidence rungs (`strong`, `moderate`, `limited`, `hypothesis`):
   - `first_party_docs` / `peer_reviewed` / `preprint` vs. `vendor_study` / `practitioner_article` / `transcript`.
   - `confidence: "hypothesis"` means published for testing, not yet confirmed by independent evidence — state this clearly to the user.
4. Seek counterevidence (`contested: true` on rules, disagreement and `recycled` / `stale` flags in `get_insights`). Distinguish the article publish date (`current_as_of`) from when an underlying statistic was measured.
5. Synthesize: **current best-supported guidance**, **what changed**, **what is disputed**, **which action to test**, and **which outcome remains unmeasured**.
6. Cite rule IDs (`aeo-…`), insight `claim_key`s, and source metadata returned by the MCP. Do not invent study names, authors, or uplift percentages.

## Boundaries

- **Do not run a website scanner (`scan_url`, `lint_*`) for a pure research question** unless the user also asks to audit a URL or draft.
- A claim mentioned in a webinar transcript is not inherently an independently replicated finding.
- "This improves machine readability" is not "this guarantees citations."
- Be explicit when a question is outside the fetched corpus or when `get_changelog` returns `complete: false` for versions older than `prev_version`.

## Efficiency

Start with 1–2 narrow `search_rules` / `get_insights` queries and expand only the most relevant categories. Do not page through the entire catalog. Note which fetched facts changed the answer; do not repeat tool output wholesale.
