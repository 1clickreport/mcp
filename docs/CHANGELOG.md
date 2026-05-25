# Changelog

Recent notable updates to the 1ClickReport MCP Server.

---

## 2026-05-26

- **DNS / gRPC fix** — closed a latent issue where Railway's outbound IPv6 routing caused Google Ads `listAccessibleCustomers` to hang on cold-cache scenarios. Now uses IPv4-only DNS resolution via in-code monkeypatch. Reviewer first-call experience is now clean.

## 2026-05-25

- **Discord error + upgrade-signal alerts** — fire-and-forget alerts to two Discord channels: `#mcp-errors` for customer-affecting tool failures, `#upgrade-signals` for Pro users hitting Premium gates or trial-expired users. Rate-limited per (user, tool). Token redaction in payload.
- **Plan-gate hardening** — `checkActiveSubscription` and `checkPremiumAccess` now enforce `current_period_end` + 1-day grace window, matching SaaS dashboard logic. Closes a divergence where MCP allowed but web denied for "frozen active" rows.
- **User context enrichment** — Discord embeds and tool responses now include user email + derived plan (`premium | trial | pro | expired | unknown`) so alerts are actionable.

## 2026-05-23 — Monitoring Agents v1 launched

- **5 new MCP tools**: `create_monitoring_rule`, `update_monitoring_rule`, `delete_monitoring_rule`, `list_monitoring_rules`, `get_rule_activity`
- **Premium gate** — writes (create / update / delete) require Premium plan; reads available to any active plan
- **Plan-aware Claude behavior** — `user_context.plan` enum exposed; Pro users see specific agent suggestions framed as upgrade pitch instead of generic paywall
- **Two-gate flow** — Claude suggests, user confirms in chat, rule lands as PENDING in dashboard, user activates from dashboard
- **Granularity**: account, campaign, ad_group, keyword (Google Ads); account, campaign, ad_set (Meta); account only (GA4, GSC — sub-scopes in v1.5)
- **Active monitoring agents summary** auto-attached to every tool response (5-min cached, invalidated on mutate)

## 2026-05-19

- **Meta Advanced Access submitted** — added `pages_show_list`, `pages_read_engagement`, `ads_management` to the existing Standard Access scopes
- **`get_meta_page_engagement` tool** — fetches post-level engagement metrics (reactions, comments, shares) for Facebook Pages

## 2026-05-16

- **`list_meta_pages`** + **`list_meta_creatives`** tools added — discovery for the Meta page and creative inventory

## 2026-05-13

- **OAuth reconnect detection** — central error pattern matcher rewrites cryptic Google / Meta auth errors into actionable "reconnect this integration" messages with deep links
- **GAQL string escape** — fixed an injection-adjacent edge case where user-provided string values weren't escaped in Google Ads queries

## 2026-05-04 to 2026-05-17 — Monitoring Agents engineering window

- Iterative buildout of the agents system between MCP and SaaS sides
- Schema: `rationale`, `severity`, `entity_type`, `entity_id`, `extra_recipients` added to `monitoring_rules`
- Evaluator cron, per-rule timeout, retry-storm guard (`last_checked_at` before eval), per-platform error isolation
- Headline-first email template with 7-day sparkline
- Hard-stale (>24h) cache refresh + graceful degradation

## Q4 2025 — Production launch

- Initial MCP server with 25 tools across Google Ads, Meta Ads, GA4, Search Console, Stripe
- OAuth 2.0 with PKCE
- AI Dashboard Builder (separate SaaS feature)
- Meta Tech Provider verified status approved
- First paying customers across US, Europe, LATAM, and Asia

---

## Versioning

This documentation tracks notable feature additions and architectural changes. The underlying MCP server is continuously deployed — there's no traditional semver-versioned release. Use `mcp.1clickreport.com/mcp` for the current production endpoint at all times.
