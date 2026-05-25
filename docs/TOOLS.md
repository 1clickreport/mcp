# Tools Reference

Complete list of 40 MCP tools exposed by the 1ClickReport server.

**Pro plan** users get all read tools. **Premium plan + trial** users also get write tools.

---

## Google Analytics 4 (4 tools)

| Tool | Type | Description |
|---|---|---|
| `get_ga4_metrics` | Read | Website traffic, conversions, bounce rate, user behavior across any time range |
| `list_ga4_events` | Read | All tracked events with counts and parameters |
| `analyze_ga4_funnel` | Read | Multi-step conversion funnel analysis with drop-off rates per step |
| `list_ga4_properties` | Read | All GA4 properties the user has access to (discovery) |

## Google Ads (8 tools — read)

| Tool | Type | Description |
|---|---|---|
| `get_google_ads_metrics` | Read | Campaign, ad group, ad, and keyword-level performance metrics |
| `get_google_ads_search_terms` | Read | Actual search queries that triggered ads (with cost / conversion data) |
| `get_google_ads_budgets` | Read | Budget utilization and pacing across campaigns |
| `get_google_keyword_ideas` | Read | Keyword Planner: search volume, competition level, CPC estimates from seeds or URLs |
| `get_campaign_negative_keywords` | Read | Negative keyword lists per campaign |
| `get_google_ads_audiences` | Read | Audience targeting, demographics, and remarketing list performance |
| `audit_google_campaign` | Read | Automated 7-category audit (0-100 score) with critical/warning/suggestion recommendations |
| `list_google_ads_accounts` | Read | All accessible Google Ads accounts (MCC discovery, manager/leaf identification) |

## Meta Ads (5 tools — read)

| Tool | Type | Description |
|---|---|---|
| `get_meta_ads_metrics` | Read | Facebook & Instagram ad performance — 48 metrics, all hierarchy levels (account/campaign/ad set/ad) |
| `list_meta_ads_accounts` | Read | All accessible Meta ad accounts |
| `list_meta_pages` | Read | Facebook Pages the user manages |
| `list_meta_creatives` | Read | Existing Meta ad creatives (images, video, copy) |
| `get_meta_page_engagement` | Read | Post-level engagement metrics (reactions, comments, shares, impressions) |

## Google Search Console (2 tools)

| Tool | Type | Description |
|---|---|---|
| `get_gsc_metrics` | Read | Clicks, impressions, CTR, average position by query / page / country |
| `list_gsc_sites` | Read | All verified sites the user has access to |

## Stripe (1 tool)

| Tool | Type | Description |
|---|---|---|
| `get_stripe_metrics` | Read | Revenue, MRR, payments, subscriptions, customers, churn, refunds |

## AI Monitoring Agents — Read (2 tools)

| Tool | Type | Description |
|---|---|---|
| `list_monitoring_rules` | Read | All active and pending monitoring rules with rationale, severity, trigger counts |
| `get_rule_activity` | Read | Recent firings and status per rule (last 7-90 days) |

---

## Google Ads management (Premium, 9 tools)

| Tool | Type | Description |
|---|---|---|
| `create_google_campaign` | Write | Create campaigns (always PAUSED initially) |
| `create_google_ad_group` | Write | Create ad groups with keywords |
| `create_google_ad` | Write | Create responsive search ads (headlines + descriptions + final URL) |
| `update_google_campaign` | Write | Update budget, bidding strategy, status |
| `update_google_ad_group` | Write | Update ad group settings |
| `manage_google_keywords` | Write | Add, pause, remove keywords; change match types |
| `set_google_ads_audiences` | Write | Set audience targeting (demographics, remarketing lists) |
| `set_campaign_targeting` | Write | Location, language, schedule, device targeting |
| `set_campaign_assets` | Write | Callouts and structured snippets |

## Meta Ads management (Premium, 6 tools)

| Tool | Type | Description |
|---|---|---|
| `create_meta_campaign` | Write | Create campaigns with objective (PAUSED by default) |
| `create_meta_adset` | Write | Create ad sets with targeting, budget, schedule |
| `create_meta_ad` | Write | Create ads with creative (image, video, copy) |
| `update_meta_campaign` | Write | Update campaign budget, status, dates |
| `update_meta_adset` | Write | Update ad set targeting, budget, schedule |
| `update_meta_ad` | Write | Update ad creative, status |

## AI Monitoring Agents — Write (Premium, 3 tools)

| Tool | Type | Description |
|---|---|---|
| `create_monitoring_rule` | Write | Claude proposes rules grounded in your data, user confirms in chat, rule lands as PENDING in dashboard, user activates from dashboard before monitoring goes live (two-gate flow) |
| `update_monitoring_rule` | Write | Tune thresholds, severity, check frequency, or activate/pause |
| `delete_monitoring_rule` | Write | Remove a rule (activity history retained) |

---

## Safety guarantees on write tools

Every write tool in 1ClickReport includes hardcoded safety:

- **PAUSED-by-default**: campaigns, ad sets, and ads created via MCP are PAUSED until user activates them in the platform (Meta Ads Manager / Google Ads UI)
- **$50/day max budget cap** enforced in code on Meta write operations
- **Plan gate enforcement**: write tools blocked behind active Premium subscription (with grace window matching SaaS dashboard logic)
- **Two-gate activation for monitoring**: AI proposals → user chat-confirms → pending in dashboard → user activates → live

The intent is to let users move fast with Claude's help while preventing accidental large spends.

---

## Tool patterns for Claude

Claude is instructed to (via the MCP server's `instructions` block):

- **Always discover accounts first** — call `list_*` before fetching metrics for multi-account users
- **Confirm scope before queries** — date range, status filter, specific account
- **Suggest agents proactively** when analyzing ad accounts (Premium users)
- **Describe agents for Pro users** instead of trying to create them — the gate is plan-aware
- **Surface activation URLs** after writes so users know where to confirm/activate
- **Use real data baselines** when suggesting thresholds, not generic templates

See [docs/SETUP.md](SETUP.md) for first-conversation walkthroughs.
