# 1ClickReport MCP Server

> **Your AI marketing analyst, connected to live data.**
> 75 MCP tools across Google Ads, Meta Ads, GA4, Search Console, Shopify, Stripe and WordPress — read, analyze, manage, and monitor.

[![smithery badge](https://smithery.ai/badge/oneclickreport/marketing)](https://smithery.ai/servers/oneclickreport/marketing)
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

**75 tools. 7 platforms. Read, analyze, manage, monitor.**

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

- **Google Ads** — read + create and manage campaigns
- **Meta Ads** (Facebook + Instagram) — read + create and manage campaigns
- **Google Analytics 4** — traffic, conversions, funnels
- **Google Search Console** — organic search performance
- **Shopify** — orders, revenue, top products
- **Stripe** — revenue, MRR, churn
- **WordPress / GitHub** — SEO Autopilot: audits, drafts, redirects, pull requests

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

## Tools (75)

Full categorized list with descriptions: [docs/TOOLS.md](docs/TOOLS.md)

75 tools across 7 platforms — 42 read, 33 write.

**Solo ($25/mo)** — analytics across every connected platform (43 tools):
- Google Analytics 4: traffic, events, funnels, AI referral traffic (4)
- Google Ads: metrics, search terms, budgets, audits, recommendations, keyword research (read subset of 23)
- Meta Ads: ad metrics, Pages, creatives, audiences, messaging, targeting search (read subset of 18)
- Search Console: clicks, impressions, rankings, URL inspection, sitemaps, quick wins (4)
- Shopify: shop, orders, revenue, top products (4)
- Stripe: revenue, MRR, subscriptions (1)
- Monitoring: list rules, see activity (2)
- Discovery: list accounts / properties / sites (4)

**Team ($99/mo) and Agency ($249/mo)** — everything above plus 32 management tools:
- Google Ads management: create and update campaigns, ad groups, ads, keywords, conversions, audiences, targeting, assets
- Meta Ads management: create and update campaigns, ad sets, ads, custom audiences
- AI Monitoring Agents: create, update, delete rules
- SEO Autopilot (12 tools, own entitlement): WordPress audits, drafts, redirects, FAQ schema, authors, and GitHub pull requests

---

## Security

- **OAuth 2.0 with PKCE** — no passwords ever touch our system. Credentials are platform-owned (Google, Meta, Stripe).
- **Encrypted token storage** in Postgres. Tokens never logged, never returned in tool responses.
- **Tech Provider verified** by Meta, with Advanced Access approved for every permission the connector requests.
- **Hardcoded budget guardrails** — Meta write tools enforce $50/day max budget per ad set in code.
- **PAUSED-by-default writes** — every campaign / ad set / ad created via Claude lands in PAUSED state. Nothing goes live without explicit user activation.
- **One-click disconnect** — revoke any integration from your dashboard; tokens deleted immediately.
- **Plan-aware enforcement** — management tools gated behind an active Team or Agency plan; trial users get full access.
- **Data deletion** — privacy@1clickreport.com or via in-app disconnect flow. Public instructions at [1clickreport.com/data-deletion](https://www.1clickreport.com/data-deletion).

---

## Pricing

| Plan | Price | What's included |
|---|---|---|
| **Free trial** | 7 days, no credit card | Full Team access |
| **Solo** | $25/mo | All analytics tools across every connected platform, unlimited queries |
| **Team** | $99/mo | Solo + campaign creation and management + SEO Autopilot + 24/7 AI monitoring agents |
| **Agency** | $249/mo | Everything in Team, with more connected accounts and seats |

Trial users get full Team access. Legacy Pro and Premium plans are grandfathered: Pro keeps Solo-level access, Premium keeps Team-level access.

---

## Technical specs

| Field | Value |
|---|---|
| **Protocol** | MCP (Model Context Protocol) 1.27+ |
| **Transport** | Streamable HTTP (primary) + SSE (legacy fallback) |
| **Authentication** | OAuth 2.0 with PKCE |
| **Server URL** | `https://mcp.1clickreport.com/mcp` |
| **Tool count** | 75 |
| **Platforms supported** | Google Ads, Meta Ads, GA4, Search Console, Google Keyword Planner, Shopify, Stripe, WordPress, GitHub |
| **Verified credentials** | Meta Tech Provider, Google Ads Standard Access (writes), GA4 Data API, GSC API, Stripe API |

---

## Status

- ✅ Production live at `mcp.1clickreport.com/mcp` since Q4 2025
- ✅ Tech Provider verified by Meta
- ✅ Meta Advanced Access approved for every requested permission: `ads_read`, `ads_management`, `business_management`, `pages_show_list`, `pages_read_engagement`
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
