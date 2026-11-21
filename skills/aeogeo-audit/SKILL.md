---
name: aeogeo-audit
description: Audit pages and content for AEO/GEO using current aeogeo MCP guidance, mechanical checks, and independent agent semantic evaluation. Use for website AI-search readiness reviews, code/HTML content audits, and evidence-aware optimization recommendations.
---

# aeogeo Audit

You are the **semantic evaluator**. aeogeo MCP provides current, curated guidance and deterministic verification. Don't outsource truth, originality or contextual applicability to scanner grades.

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

1. Identify the user's target, business purpose, content type, audience and read-only/edit intent.
2. Discover **relevant current guidance** via `search_rules`/`fetch_rules`/`get_insights` (and `list_categories` when useful), even if initial scanner findings are all green. Bound lookups to relevant subjects; don't fetch the entire rulebook.
3. Run the applicable deterministic path: `scan_url` for a live page, `lint_html` for supplied HTML, `lint_text` only for text properties, or `audit_schema` for JSON-LD. Don't claim HTML headers/robots findings from plain text. For a whole site (Pro): `scan_site`, then `get_site_scan` with `view: "by_check"` once it is done, which lists each failing check once with its page count and `rule_id`; fetch those rules with `fetch_rules` and `ids`.
4. Independently assess whether content actually answers its intended query, is accurate and useful, makes supported claims, uses credible sources, and meets the user's context. Look for invented numbers, credentials, studies and review counts.
5. Triage check results. Challenge false positives (e.g., numeric BLUF demand on a correct nonnumeric answer, reference-doc repetition, optional schema treated as mandatory). Explain skipped/unknown checks.
6. Judge L4 authority and L5 off-site against their rules (see the six levels above), checking off-site facts with your own web tools. Do this even when every check passes.
7. Report level by level from L0, and within that separately: **verified technical defects**, **agent semantic findings** (all of L4 and L5), **evidence-based recommendations**, **uncertain experiments**. Include applicable rule IDs, confidence, version and re-check suggestions. Name any level you did not judge.

## Stop rules

- An A grade is **not** evidence of high semantic quality or likely citations.
- A rule with no automated check may still be central to the audit.
- Correlation is not causation; don't invent expected citation improvements.
- Do not publish reports, subscribe webhooks or edit a user's repository unless requested.
- If the target cannot be accessed, explicitly state what was and wasn't evaluated.

## Cost control

For straightforward tasks, use a small relevant rule set and just the needed scanner mode. Use deeper evidence retrieval when a claim's confidence is disputed or the user's question asks for up-to-date guidance. Avoid repeating large lint outputs when a narrower call can recover detail.

## Free and Pro tools

Free, no key or sign-in (`https://mcp.aeogeo.site`): `plan_audit`, `scan_url` (grades plus fixes for failed L0–L3 checks), `list_categories`, `fetch_sample_rules`, `get_hook_snippet`, `export_report`.
Pro (Bearer key, or OAuth at `https://mcp.aeogeo.site/mcp`): `lint_html`, `lint_text`, `audit_schema`, `fetch_rules`, `search_rules`, `get_insights`, `get_changelog`, `set_retention_webhook`, `scan_site`, `get_site_scan`.
If a Pro tool answers `requires_pro`, say which part of the task needs Pro, finish what the free tools can do, and don't retry the call.
