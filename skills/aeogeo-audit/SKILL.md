---
name: aeogeo-audit
description: Audit pages and content for AEO/GEO using current aeogeo MCP guidance, mechanical checks, and independent agent semantic evaluation. Use for website AI-search readiness reviews, code/HTML content audits, and evidence-aware optimization recommendations.
compatibility: Needs the aeogeo MCP server at https://mcp.aeogeo.site (free without a key; Pro with a key or sign-in).
---

# aeogeo Audit (`aeogeo-audit`)

You are the **semantic evaluator**. aeogeo MCP provides current, curated guidance and deterministic verification. Do not outsource truth, originality, or contextual applicability to scanner grades.

## Connect the aeogeo MCP server

The tools named below come from the aeogeo MCP server (Streamable HTTP). If `tools/list` shows no aeogeo tools, ask the user to add the server; do not guess what a tool would return.

- **Free:** `https://mcp.aeogeo.site` with no key. Free tools only.
- **Pro with a key:** the same address with the header `Authorization: Bearer <key>`. Keep the key in the `AEOGEO_API_KEY` environment variable; never paste it into a chat or commit it.
- **Pro with sign-in (OAuth):** `https://mcp.aeogeo.site/mcp`. The client opens a sign-in for the email used at checkout.

In Claude Code: `claude mcp add --transport http aeogeo https://mcp.aeogeo.site`. Other clients: <https://aeogeo.site/docs/mcp/>.

If a Pro tool answers `requires_pro`, say which part of the task needs Pro, finish what the free tools can do, and do not retry the call.

## Tier & tool availability

- **Keyed (Pro/Team):** `scan_url` (detailed), `lint_html`, `lint_text`, `audit_schema`, `search_rules`, `fetch_rules`, `get_insights`, `get_changelog`, `list_categories`, `scan_site`, `get_site_scan`.
- **Keyless (Free fallback):** If `tools/list` shows only free tools (`plan_audit`, `scan_url`, `list_categories`, `fetch_sample_rules`, `get_hook_snippet`, `export_report`), use `scan_url` (which returns per-group grades + failed L0–L3 fundamentals) and `fetch_sample_rules`, and note that full rule evidence, `get_insights`, and local draft linting require a Pro key.

## The six levels

The rulebook sorts every rule into six readiness levels. An audit covers all six. Work through them in order: a fault at a lower level comes before anything above it, so a blocked crawler outranks any off-site work.

| Level | What it covers | Who judges it |
|---|---|---|
| L0 access | crawlers can reach the page | automated checks |
| L1 rendering | the content is in the HTML without JavaScript | automated checks |
| L2 structure | headings, structured data, page template | automated checks, plus you where a rule has no check |
| L3 answer shape | the page answers its question plainly and early | automated checks, plus you where a rule has no check |
| L4 authority | who stands behind the page, their expertise, entity facts that match everywhere, local details | you: `fetch_rules` with `author_expertise`, `authority_offsite`, and `local` for a local business |
| L5 off-site | third-party mentions, reviews, listings and press, and how the user measures results | you: `fetch_rules` with `authority_offsite` and `measurement`, plus your own web tools for the off-site facts |

A scan covers only the automated part of L0–L3. No automated check covers L4 or L5. If you stop at the scan, say that L4 and L5 were not judged. On the free tier, `fetch_sample_rules` gives public summaries of the same categories.

## Runbook

1. Identify the user's target, business purpose, content type (`blog`, `landing`, `product`, `faq`, `docs`, `general`), audience, and read-only vs. edit intent.
2. Discover **relevant current guidance** via `search_rules` / `fetch_rules` / `get_insights` (or `fetch_sample_rules` on the free tier), even if initial scanner findings are all green. Bound lookups to relevant categories; do not fetch the entire rulebook.
3. Run the applicable deterministic path: `scan_url` for a live public URL, `lint_html` for supplied HTML, `lint_text` for Markdown or plain text drafts, or `audit_schema` for standalone JSON-LD. Do not claim HTTP header or `/robots.txt` findings from plain text or isolated JSON-LD. For a whole site (Pro): `scan_site`, then `get_site_scan` with `view: "by_check"` once it is done, which lists each failing check once with its page count and `rule_id`; fetch those rules with `fetch_rules` and `ids`.
4. Respect check and rule ownership:
   - `judge: engine` / `evaluation_mode: deterministic` — trust the mechanical pass/fail (crawler policy, page-level `meta robots` / `X-Robots-Tag`, JSON-LD syntax, unseen schema claims, SSR word counts).
   - `judge: agent` / `evaluation_mode: hybrid | agent` — re-read the rule text and apply your own judgment. Rules with `checks: []` (`evaluation_mode: agent`) are intentional host-agent guidance (e.g., L4 authority, L5 offsite placement, factual substantiation), not missing checks.
5. Independently assess whether content actually answers its intended query, is accurate and useful, makes supported claims, cites real sources, and fits the page genre. Look for fabricated numbers, credentials, studies, and review counts.
6. Triage check results. Challenge false positives where the heuristic does not fit the genre (e.g., numeric BLUF demand on a valid non-numeric definitional lead, reference-doc terminology repetition, optional schema on a clean technical explainer). Explain skipped checks.
7. Judge L4 authority and L5 off-site against their rules (see the six levels above), checking off-site facts with your own web tools. Do this even when every check passes.
8. Report level by level from L0, and within that separately: **verified technical defects** (`judge: engine`), **agent semantic findings** (`judge: agent`, including all of L4 and L5), **evidence-based recommendations** (with rule `confidence`), and **uncertain experiments** (`confidence: hypothesis`). Include the rule IDs, confidence and ruleset version you relied on, and what to re-check. Name any level you did not judge.

## Stop rules

- An `A` grade is **not** evidence of high semantic quality or likely citations.
- A rule with `checks: []` may still be central to the audit.
- Correlation is not causation; do not invent expected citation uplift numbers.
- Do not call `export_report`, `set_retention_webhook`, or edit a repository unless requested.
- If the target cannot be accessed, state explicitly what was and was not evaluated.

## Cost control

For straightforward single-page audits, retrieve 1–2 focused rule searches/categories plus the single applicable lint/scan tool. Expand deeper into `get_insights` or `get_changelog` when a claim's confidence is disputed or the user asks for recent research changes. Do not repeat large lint outputs when a narrower call recovers the detail.
