---
name: aeogeo-optimize
description: Implement scoped AEO/GEO improvements in a website or content repository using aeogeo's current evidence, contextual agent judgment, deterministic checks, and existing project tests. Use when the user asks to change code.
---

# aeogeo Optimize

Change only what the user's request authorizes. Use aeogeo for **current research and repeatable checks**; use your host model to decide what actually makes sense for the content and repository.

## Runbook

1. Inspect project conventions, target pages, relevant tests and existing content meaning.
2. Retrieve focused current aeogeo guidance, including relevant uncertainty and dissent. Run applicable baseline lint/scans.
3. Separate **hard blockers**, **strongly supported improvements**, and **experiments**. Don't let a weak rule override the site's facts or user intent.
4. Make minimal, reviewable changes. Preserve factual meaning and accessibility. Avoid FAQ/schema/keyword cargo cults.
5. Run repository checks and aeogeo lints that actually cover changed surfaces. Where site is not deployed, don't claim a live scan verified production.
6. Summarize changes, validation, unresolved agent-judgment items, and any changed research assumptions.

## Verification boundary

A deterministic test can prove that a syntax/accessibility/crawler rule passes for a specific input, not that a website will appear in an AI answer. Any recommendation supported only by a hypothesis remains labeled as such after implementation.

## Safety

No publication, report export, deployment or external webhook changes unless requested. Respect the user's repository permissions and existing CI gates.

## Free and Pro tools

Free, no key or sign-in (`https://mcp.aeogeo.site`): `plan_audit`, `scan_url` (grades plus fixes for failed L0–L3 checks), `list_categories`, `fetch_sample_rules`, `get_hook_snippet`, `export_report`.
Pro (Bearer key, or OAuth at `https://mcp.aeogeo.site/mcp`): `lint_html`, `lint_text`, `audit_schema`, `fetch_rules`, `search_rules`, `get_insights`, `get_changelog`, `set_retention_webhook`, `scan_site`, `get_site_scan`.
If a Pro tool answers `requires_pro`, say which part of the task needs Pro, finish what the free tools can do, and don't retry the call.
