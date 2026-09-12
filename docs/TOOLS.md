# Tools Reference

Complete list of the 75 MCP tools exposed by the 1ClickReport server — 42 read, 33 write, across 7 platforms.

**Solo** users get every analytics tool (43 tools). **Team, Agency and trial** users also get the 32 management tools. The 13 SEO Autopilot tools sit behind their own entitlement.

---


## Google Ads (23 tools)

| Tool | Type | Plan | Description |
|---|---|---|---|
| `apply_google_recommendation` | Write | Team / Agency | Apply google recommendation |
| `audit_google_campaign` | Read | Solo + | Automated audit — google campaign |
| `create_google_ad` | Write | Team / Agency | Create google ad (created PAUSED where it can spend) |
| `create_google_ad_group` | Write | Team / Agency | Create google ad group (created PAUSED where it can spend) |
| `create_google_campaign` | Write | Team / Agency | Create google campaign (created PAUSED where it can spend) |
| `create_google_campaign_full` | Write | Team / Agency | Create google campaign full (created PAUSED where it can spend) |
| `get_campaign_negative_keywords` | Read | Solo + | Read campaign negative keywords |
| `get_google_ads_audiences` | Read | Solo + | Read google ads audiences |
| `get_google_ads_budgets` | Read | Solo + | Read google ads budgets |
| `get_google_ads_change_history` | Read | Solo + | Read google ads change history |
| `get_google_ads_metrics` | Read | Solo + | Read google ads metrics |
| `get_google_ads_search_terms` | Read | Solo + | Read google ads search terms |
| `get_google_keyword_ideas` | Read | Solo + | Read google keyword ideas |
| `get_google_recommendations` | Read | Solo + | Read google recommendations |
| `list_google_campaigns` | Read | Solo + | List google campaigns |
| `manage_google_conversions` | Write | Team / Agency | Add, change or remove google conversions |
| `manage_google_keywords` | Write | Team / Agency | Add, change or remove google keywords |
| `set_campaign_assets` | Write | Team / Agency | Set campaign assets |
| `set_campaign_targeting` | Write | Team / Agency | Set campaign targeting |
| `set_google_ads_audiences` | Write | Team / Agency | Set google ads audiences |
| `update_google_ad` | Write | Team / Agency | Update google ad |
| `update_google_ad_group` | Write | Team / Agency | Update google ad group |
| `update_google_campaign` | Write | Team / Agency | Update google campaign |


## Meta Ads (18 tools)

| Tool | Type | Plan | Description |
|---|---|---|---|
| `create_meta_ad` | Write | Team / Agency | Create meta ad (created PAUSED where it can spend) |
| `create_meta_adset` | Write | Team / Agency | Create meta adset (created PAUSED where it can spend) |
| `create_meta_campaign` | Write | Team / Agency | Create meta campaign (created PAUSED where it can spend) |
| `create_meta_campaign_full` | Write | Team / Agency | Create meta campaign full (created PAUSED where it can spend) |
| `create_meta_custom_audience` | Write | Team / Agency | Create meta custom audience (created PAUSED where it can spend) |
| `delete_meta_custom_audience` | Write | Team / Agency | Delete meta custom audience |
| `get_meta_ads_metrics` | Read | Solo + | Read meta ads metrics |
| `get_meta_build_assets` | Read | Solo + | Read meta build assets |
| `get_meta_messaging_breakdown` | Read | Solo + | Read meta messaging breakdown |
| `get_meta_page_engagement` | Read | Solo + | Read meta page engagement |
| `list_meta_audiences` | Read | Solo + | List meta audiences |
| `list_meta_campaigns` | Read | Solo + | List meta campaigns |
| `list_meta_creatives` | Read | Solo + | List meta creatives |
| `list_meta_pages` | Read | Solo + | List meta pages |
| `search_meta_targeting` | Write | Solo + | Search meta targeting |
| `update_meta_ad` | Write | Team / Agency | Update meta ad |
| `update_meta_adset` | Write | Team / Agency | Update meta adset |
| `update_meta_campaign` | Write | Team / Agency | Update meta campaign |


## WordPress / SEO Autopilot (12 tools)

| Tool | Type | Plan | Description |
|---|---|---|---|
| `audit_keyword_cannibalization` | Read | Solo + | Automated audit — keyword cannibalization |
| `audit_wordpress_seo` | Read | Solo + | Automated audit — wordpress seo |
| `create_wordpress_author` | Write | Team / Agency | Create wordpress author (created PAUSED where it can spend) |
| `get_wordpress_post` | Read | Solo + | Read wordpress post |
| `list_wordpress_authors` | Read | Solo + | List wordpress authors |
| `list_wordpress_posts` | Read | Solo + | List wordpress posts |
| `manage_wordpress_redirects` | Write | Team / Agency | Add, change or remove wordpress redirects |
| `open_seo_pr` | Write | Team / Agency | Open seo pr |
| `publish_blog_post` | Write | Team / Agency | Publish blog post |
| `set_post_faq_schema` | Write | Team / Agency | Set post faq schema |
| `set_wordpress_featured_image` | Write | Team / Agency | Set wordpress featured image |
| `update_wordpress_post` | Write | Team / Agency | Update wordpress post |


## Google Analytics 4 (4 tools)

| Tool | Type | Plan | Description |
|---|---|---|---|
| `analyze_ga4_funnel` | Read | Solo + | Multi-step funnel analysis with per-step drop-off |
| `get_ai_referral_traffic` | Read | Solo + | Sessions arriving from ChatGPT, Claude, Perplexity, Gemini and other AI engines |
| `get_ga4_metrics` | Read | Solo + | Traffic, conversions, engagement and revenue by any dimension and date range |
| `list_ga4_events` | Read | Solo + | Tracked events with counts and parameters |


## Search Console (4 tools)

| Tool | Type | Plan | Description |
|---|---|---|---|
| `find_gsc_quick_wins` | Read | Solo + | Pages ranking just off page one, ranked by opportunity |
| `get_gsc_metrics` | Read | Solo + | Clicks, impressions, CTR and average position by query, page, country or device |
| `inspect_url` | Read | Solo + | Google's URL Inspection verdict for a single page (indexed, excluded, why) |
| `list_sitemaps` | Read | Solo + | Submitted sitemaps with last read and error counts |


## Shopify (4 tools)

| Tool | Type | Plan | Description |
|---|---|---|---|
| `get_shopify_metrics` | Read | Solo + | Store revenue, order volume and average order value over time |
| `get_shopify_orders` | Read | Solo + | Orders with totals, status and customer detail |
| `get_shopify_shop` | Read | Solo + | Store details, currency, plan and domains |
| `get_shopify_top_products` | Read | Solo + | Best-selling products by revenue or units |


## Stripe (1 tools)

| Tool | Type | Plan | Description |
|---|---|---|---|
| `get_stripe_metrics` | Read | Solo + | Revenue, MRR, payments, subscriptions, customers, churn and refunds |


## AI Monitoring Agents (5 tools)

| Tool | Type | Plan | Description |
|---|---|---|---|
| `create_monitoring_rule` | Write | Team / Agency | Propose a rule grounded in real account data; lands PENDING for activation |
| `delete_monitoring_rule` | Write | Team / Agency | Remove a rule; activity history is retained |
| `get_rule_activity` | Read | Solo + | Recent firings and status per rule |
| `list_monitoring_rules` | Read | Solo + | Active and pending monitoring rules with rationale and trigger counts |
| `update_monitoring_rule` | Write | Team / Agency | Tune thresholds, severity, frequency, or activate and pause |


## Account discovery (4 tools)

| Tool | Type | Plan | Description |
|---|---|---|---|
| `list_ga4_properties` | Read | Solo + | GA4 properties the user can access |
| `list_google_ads_accounts` | Read | Solo + | Google Ads accounts, including manager/leaf structure |
| `list_gsc_sites` | Read | Solo + | Verified Search Console properties |
| `list_meta_ads_accounts` | Read | Solo + | Meta ad accounts the user can access |

---

## Safety guarantees on write tools

Every write tool in 1ClickReport includes hardcoded safety:

- **PAUSED-by-default**: campaigns, ad sets, and ads created via MCP are PAUSED until user activates them in the platform (Meta Ads Manager / Google Ads UI)
- **$50/day max budget cap** enforced in code on Meta write operations
- **Plan gate enforcement**: management tools blocked behind an active Team or Agency plan (with grace window matching SaaS dashboard logic)
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
