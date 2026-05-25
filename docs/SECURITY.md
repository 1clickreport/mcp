# Security & Data Handling

1ClickReport is a hosted MCP server that connects user-owned marketing platforms to Claude. Here's how we handle credentials, data, and access.

---

## Authentication & credentials

- **OAuth 2.0 with PKCE** for all platform connections (Google, Meta, Stripe)
- **No passwords ever** — users never give 1ClickReport their platform passwords
- **Per-user encrypted token storage** in PostgreSQL (Supabase, encrypted at rest via standard database encryption)
- **Tokens NEVER logged** to console, Sentry, or any external system
- **Tokens NEVER returned** in MCP tool responses

## Token redaction in error monitoring

When errors are captured for debugging (Sentry, internal Discord alerts), any field name matching the pattern `/token|secret|key|password|webhook/i` is replaced with `<REDACTED>` before being captured or sent anywhere. This includes nested objects.

## Plan enforcement & access control

- **Plan gate middleware** checks subscription status + `current_period_end` + 1-day grace window before allowing access to any tool
- **Write tools** (campaign creation, monitoring rule creation) gated behind active Premium subscription
- **Trial users** get full Premium access during their 7-day trial (no credit card required)
- **Expired / cancelled users** are blocked from all tools with a clear "renew at [URL]" message
- **Read-only escape hatch**: discovery tools (`list_*_accounts`) remain accessible during gate denial so users can see what they had

## Write safety guardrails

Every write tool includes hardcoded safety controls:

| Tool family | Guardrail |
|---|---|
| Meta Ads write | Hardcoded $50/day max budget per ad set, enforced in code at the tool boundary |
| All campaign creates | PAUSED-by-default — nothing goes live without explicit user activation in the platform UI |
| Monitoring rules | Two-gate flow — chat confirmation creates PENDING rule, user must activate from dashboard |
| Disambiguation | Tools throw explicit errors if user has multiple accounts and the call doesn't specify which — prevents silent wrong-account writes |

## Connection disconnect / token revocation

- **One-click disconnect** from the dashboard at `app.1clickreport.com/integrations`
- Disconnect immediately deletes all OAuth tokens for that platform from our database
- Disconnect also nulls the cached account/property/site list so a fresh reconnect starts clean

## Compliance posture

- **GDPR** — data subject access + deletion requests via support@1clickreport.com or in-app
- **Privacy policy** explicit about: what data is collected, why, retention periods, sub-processors
- **Sub-processors** listed in privacy policy: Railway (hosting), Supabase (database), Stripe (payments), Sentry (error monitoring)
- **Data deletion** instructions public at https://www.1clickreport.com/data-deletion

## Meta API compliance

- **Tech Provider verified** by Meta (Business Verification + Access Verification)
- **Standard Access approved** for `ads_read` + `business_management` (since Oct 2025)
- **Advanced Access in review** (submitted May 2026) for `ads_management`, `pages_show_list`, `pages_read_engagement`
- Per Meta's terms: we read data on behalf of the authenticated user only; we do not sell or share data with third parties

## Google API compliance

- **Google Ads Standard Developer Token** approved (read + write)
- **Google OAuth** with `email`, `profile`, and per-API scopes only
- **Restricted scopes** (Ads, GA4, GSC) used only for documented purposes per the Google API Services User Data Policy

## Stripe integration

- **Read-only Stripe API** scope on the Stripe Connect side
- We never store full card data or PII beyond what Stripe itself provides via their API

## Network security

- **TLS 1.2+ everywhere**, recognized CA, HTTPS-only endpoints
- **MCP server** at `mcp.1clickreport.com/mcp` — OAuth-gated, no anonymous access
- **Web dashboard** at `app.1clickreport.com` — session-based with secure cookies

## Architecture transparency

- Hosting: **Railway** (US region)
- Database: **Supabase Postgres** with row-level security on user-scoped tables
- Error monitoring: **Sentry** (with token redaction layer in front)
- Email delivery: **Resend** (transactional only) + Microsoft 365 (for `agents@` alerts)
- No AI provider stores user data: we use Claude via MCP, not by sending user data to a third-party AI

## Incident response

- **Sentry error monitoring** with user context (id only, not email) for actionable alerts
- **Discord webhooks** for real-time visibility on customer-affecting errors (separate channel from upgrade signals)
- **24-hour acknowledgment** target for any security report sent to security@1clickreport.com

---

## Reporting security issues

Please email **security@1clickreport.com** for responsible disclosure.

If you've found a vulnerability — we appreciate it and will respond within 24 hours. We don't have a formal bug bounty yet but happy to coordinate on coffee / merch / public acknowledgment.
