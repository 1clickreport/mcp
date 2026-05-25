# 1ClickReport MCP Server

> **Your AI marketing analyst, connected to live data.**
> 40 MCP tools across Google Ads, Meta Ads, GA4, Search Console, and Stripe — read, analyze, manage, and monitor.

[![MCP](https://img.shields.io/badge/MCP-1.27%2B-blue)](https://modelcontextprotocol.io)
[![OAuth](https://img.shields.io/badge/Auth-OAuth%202.0%20%2B%20PKCE-green)](#security)
[![Status](https://img.shields.io/badge/status-production-brightgreen)](https://mcp.1clickreport.com/mcp)
[![Meta](https://img.shields.io/badge/Meta-Tech%20Provider%20Verified-blue)](https://developers.facebook.com)

**Connect to Claude in 2 minutes.** Add `https://mcp.1clickreport.com/mcp` as a custom connector → sign in with Google → ask Claude *"Where am I wasting ad spend?"*

[Get started](#quick-start) · [Tools](docs/TOOLS.md) · [Security](#security) · [Pricing](#pricing) · [1clickreport.com](https://www.1clickreport.com)

---

## What it does

1ClickReport turns Claude into a full-stack marketing analyst with live access to your ad data. Ask anything in plain English:

- **"Where am I wasting ad spend this month?"** → Claude pulls 30 days of search terms, keyword performance, and conversions. Returns ranked waste sources with fix recommendations.
- **"Audit my top-spending Google Ads campaign"** → 7-category scoring system (structure, ad quality, keywords, budget, performance, negatives, audiences). Returns score 0-100 + specific issues categorized as critical/warning/suggestion.
- **"Set up monitoring agents that alert me if CPA spikes"** → Claude analyzes your baseline, proposes data-grounded thresholds, you confirm in chat. SaaS-side evaluator runs every 6h and emails you when breached.
- **"Create a Meta Ads campaign targeting small business owners with a $20 daily budget"** → Claude creates campaign + ad set + ads in PAUSED state. You review in Meta Ads Manager before activating.
- **"Compare my Google Ads vs Meta Ads ROAS last quarter"** → Cross-platform analysis with budget reallocation recommendations.

**40 tools. 6 platforms. Read, analyze, manage, monitor.**

---

## Who it's for

- **Marketing agencies** managing 10-100+ client accounts who want to audit, monitor, and report at scale without switching dashboards
- **In-house marketing managers** at e-commerce / SaaS / services companies who want an always-on AI analyst
- **DTC founders** who run their own ads and want to find waste fast
- **Marketing consultants** delivering campaign audits and ongoing optimization to clients

---

## Quick start

### 1. Add the connector to Claude

```
1. Go to https://claude.ai/settings/connectors
2. Click "Add custom connector"
3. Name: 1ClickReport
   URL:  https://mcp.1clickreport.com/mcp
4. Click Add → Connect
5. Sign in with Google (one-click OAuth, no Facebook login)
```

### 2. Connect your data sources

After OAuth, you'll land on the onboarding page. Connect one or more:

- **Google Ads** — read + create campaigns
- **Meta Ads** (Facebook + Instagram) — read + create campaigns
- **Google Analytics 4** — traffic, conversions, funnels
- **Google Search Console** — organic search performance
- **Stripe** — revenue, MRR, churn

### 3. Ask Claude

```
"What Google Ads accounts do I have?"
"Audit my Brand Search campaign"
"Set up an alert if my CPA goes above $50 on any campaign"
"Show me my Meta Ads vs Google Ads performance this month"
"Where am I wasting spend?"
```

That's it. No CSVs, no dashboards, no waiting for an analyst.

---

## Tools (40)

Full categorized list with descriptions: [docs/TOOLS.md](docs/TOOLS.md)

**Pro plan ($25/mo)** — Read & analyze (29 tools):
- Google Analytics 4: traffic, events, funnels (4)
- Google Ads: metrics, search terms, budgets, audits, keyword research (8)
- Meta Ads: ad metrics, Pages, page engagement, creatives (5)
- Search Console: clicks, impressions, rankings (2)
- Stripe: revenue, MRR, subscriptions (1)
- Monitoring: list rules, see activity (2)
- Discovery: list accounts/properties/sites (4)

**Premium plan ($99/mo)** — Everything above PLUS write (11 more):
- Google Ads management: create + update campaigns, ad groups, ads, keywords, audiences, targeting (8)
- Meta Ads management: create + update campaigns, ad sets, ads (6)
- AI Monitoring Agents: create + update + delete rules (3)

---

## Security

- **OAuth 2.0 with PKCE** — no passwords ever touch our system. Credentials are platform-owned (Google, Meta, Stripe).
- **Encrypted token storage** in Postgres. Tokens never logged, never returned in tool responses.
- **Tech Provider verified** by Meta. Tier upgrade in progress (May 2026).
- **Hardcoded budget guardrails** — Meta write tools enforce $50/day max budget per ad set in code.
- **PAUSED-by-default writes** — every campaign / ad set / ad created via Claude lands in PAUSED state. Nothing goes live without explicit user activation.
- **One-click disconnect** — revoke any integration from your dashboard; tokens deleted immediately.
- **Plan-aware enforcement** — write tools gated behind active Premium subscription; trial users get full access.
- **Data deletion** — privacy@1clickreport.com or via in-app disconnect flow. Public instructions at [1clickreport.com/data-deletion](https://www.1clickreport.com/data-deletion).

---

## Pricing

| Plan | Price | What's included |
|---|---|---|
| **Free trial** | 7 days, no credit card | Full Premium access |
| **Pro** | $25/mo | All read & analyze tools, AI Dashboard Builder, unlimited queries |
| **Premium** | $99/mo | Pro + campaign creation/management + 24/7 AI monitoring agents |

Trial users get full Premium access. After trial, pick a plan or downgrade to read-only.

---

## Technical specs

| Field | Value |
|---|---|
| **Protocol** | MCP (Model Context Protocol) 1.27+ |
| **Transport** | Streamable HTTP (primary) + SSE (legacy fallback) |
| **Authentication** | OAuth 2.0 with PKCE |
| **Server URL** | `https://mcp.1clickreport.com/mcp` |
| **Tool count** | 40 |
| **Platforms supported** | Google Ads, Meta Ads, GA4, Search Console, Google Keyword Planner, Stripe |
| **Verified credentials** | Meta Tech Provider, Google Ads Standard Access (writes), GA4 Data API, GSC API, Stripe API |

---

## Status

- ✅ Production live at `mcp.1clickreport.com/mcp` since Q4 2025
- ✅ Tech Provider verified by Meta
- ✅ Approved Meta scopes: `business_management`, `ads_read`
- 🔄 Meta Advanced Access in review (May 2026) — adding `ads_management`, `pages_show_list`, `pages_read_engagement`
- ✅ Paying customers across US, Europe, LATAM, and Asia

---

## Documentation

- [Tools list (full)](docs/TOOLS.md)
- [Setup walkthrough](docs/SETUP.md)
- [Security & data handling](docs/SECURITY.md)
- [Architecture overview](docs/ARCHITECTURE.md)
- [Changelog](docs/CHANGELOG.md)

---

## Links

- **Website**: https://www.1clickreport.com
- **Dashboard**: https://app.1clickreport.com
- **Documentation**: https://www.1clickreport.com/docs/
- **Privacy Policy**: https://www.1clickreport.com/privacy-policy
- **Terms of Service**: https://www.1clickreport.com/terms
- **Refund Policy**: https://www.1clickreport.com/refund-policy

---

## Founder

Built by [Suryansh Jaiswal](https://www.linkedin.com/in/suryanshjaiswal/) — solo founder, Dubai, UAE. Bootstrapped, profitable.

Reach me at **suryansh@1clickreport.com**.

---

## License

MIT — see [LICENSE](LICENSE).

This repo contains documentation, manifest, and setup guides for the 1ClickReport MCP Server. The server itself is a hosted SaaS at `mcp.1clickreport.com/mcp` — source code is proprietary and not published here.
