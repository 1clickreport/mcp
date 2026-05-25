# Architecture Overview

High-level view of how 1ClickReport's MCP server fits together. Implementation details intentionally light — this doc is for context, not reproduction.

---

## Components

```
┌─────────────────────────────────────────────────────────────┐
│  Claude (claude.ai, Desktop, Cursor, etc.)                  │
└─────────────────────────────────────────────────────────────┘
                          │ MCP (Streamable HTTP / SSE)
                          │ OAuth 2.0 + PKCE
                          ▼
┌─────────────────────────────────────────────────────────────┐
│  1ClickReport MCP Server   mcp.1clickreport.com/mcp         │
│  (Node.js, hosted on Railway)                               │
│                                                             │
│  • Tool dispatch (40 tools)                                 │
│  • Plan-gate middleware (subscription + grace window)       │
│  • Per-user OAuth token management                          │
│  • Cache-aware data fetch (self-healing background refresh) │
│  • Discord webhooks for error + upgrade-signal alerts       │
└─────────────────────────────────────────────────────────────┘
                          │
            ┌─────────────┴─────────────┐
            ▼                           ▼
┌──────────────────────┐    ┌──────────────────────────────┐
│  Postgres (Supabase) │    │  Platform APIs               │
│  • users + tokens    │    │  • Google Ads (gRPC)         │
│  • caches per-       │    │  • Meta Marketing (REST)     │
│    platform          │    │  • GA4 Analytics Data (REST) │
│  • monitoring rules  │    │  • Search Console (REST)     │
│  • activity log      │    │  • Stripe (REST)             │
└──────────────────────┘    └──────────────────────────────┘
        ▲
        │ shared DB
        ▼
┌─────────────────────────────────────────────────────────────┐
│  1ClickReport SaaS Dashboard  app.1clickreport.com          │
│  • OAuth callbacks (writes tokens + warms cache)            │
│  • Monitoring rule management (pending / active / paused)   │
│  • Evaluation cron (every 6h)                               │
│  • Email alert delivery                                     │
└─────────────────────────────────────────────────────────────┘
```

## Request flow — example tool call

User asks Claude: **"Show me my Google Ads spend last week"**

1. Claude calls MCP tool `get_google_ads_metrics` with start/end dates
2. MCP server receives the call via Streamable HTTP transport
3. Plan-gate middleware checks subscription status + grace window
4. `getUserContext()` loads user row from Postgres (encrypted tokens, plan info, cached account lists)
5. `addUserContext()` attaches `user_context.plan` + active monitoring agents summary to the response (fire-and-forget background refresh kicks off)
6. Tool handler:
   - Resolves account ID (cached or from request)
   - Refreshes Google Ads access token if needed (using stored refresh token)
   - Calls Google Ads API for the requested metrics
7. Result formatted + returned to Claude
8. Claude renders the answer in plain language to the user

## Cache layer

Each user row in Postgres has per-platform cache columns:
- `google_ads_accounts_cache` (jsonb) + `google_ads_cache_updated_at` (timestamp)
- Same pattern for `ga4_properties_cache`, `gsc_properties_cache`, `meta_accounts_cache`

**Cache policy**:
- **Populated on OAuth connect** by the SaaS dashboard (warm-on-connect)
- **Self-healing background refresh** kicks off on every tool call when cache is >6h old (non-blocking)
- **Hard-stale refresh** at >24h forces synchronous refetch with graceful degradation
- **Cache shape is shared** between SaaS and MCP — same Postgres table, same columns

This means a new user gets instant tool responses (cache populated by warm-on-connect), and the cache stays fresh in the background as they use Claude.

## Plan gate

Subscription enforcement happens in `src/middleware/plan-gate.js`:

- `checkActiveSubscription(toolName)` — gates ALL tools behind active subscription/trial
- `checkPremiumAccess(toolName)` — gates write tools behind Premium

Decision matrix:

| Plan | Status | Read tools | Write tools |
|---|---|---|---|
| Trial (within window) | trialing | ✅ Allow | ✅ Allow |
| Pro | active | ✅ Allow | ❌ PREMIUM_REQUIRED |
| Premium | active | ✅ Allow | ✅ Allow |
| Premium / Pro past period_end + grace | active (stale row) | ❌ Block | ❌ Block |
| Cancelled / expired | cancelled / expired_trial | ❌ Block (with upgrade URL) | ❌ Block |

Grace window (default 1 day) matches the SaaS dashboard logic so users see the same decision in both interfaces.

## Monitoring agents (Premium)

Two-gate approval flow:

1. **Gate 1 — chat confirmation**: User asks Claude to set up monitoring. Claude analyzes data, proposes rules with rationale + severity, asks for user OK in chat.
2. **MCP creates pending rule**: `create_monitoring_rule` writes to `monitoring_rules` table with `is_active=false`.
3. **Gate 2 — dashboard activation**: User visits `app.1clickreport.com/monitoring`, reviews rationale, color-coded severity, and clicks Activate.
4. **Background evaluation**: SaaS cron runs every 6h, evaluates active rules, writes to `rule_activity_log`, emails users on threshold breach.

Claude can never silently activate a rule. The dashboard activation is a hard requirement.

## Reliability + observability

- **Sentry** for error monitoring with user_id + tool name context (no PII)
- **Discord webhooks** for real-time alerts: errors → `#mcp-errors`, paywall hits → `#upgrade-signals`
- **Rate-limited per (user, tool, error_code)** to prevent alert storms
- **Token redaction** layer applied before any data leaves the server
- **Self-healing caches** mean transient platform API failures don't block user-facing responses
- **gRPC IPv4-only fix** at server boot to prevent Railway IPv6 hangs

## What's NOT in this repo

This is the public companion to the 1ClickReport MCP. The actual implementation (system prompts, plan-gate logic, monitoring evaluator, DB schema, OAuth flow internals, customer-specific code paths) is hosted SaaS source. If you're an MCP catalog or directory operator and need verification, email **suryansh@1clickreport.com**.
