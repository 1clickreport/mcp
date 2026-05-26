# Reviewer Instructions — 1ClickReport MCP

> Step-by-step setup and verification flow for the Anthropic Claude Directory reviewer. Designed for someone unfamiliar with 1ClickReport. Estimated total time: 10-15 minutes.

---

## Quick facts

- **Connector name**: 1ClickReport
- **MCP Server URL**: `https://mcp.1clickreport.com/mcp`
- **Transport**: Streamable HTTP (primary) + SSE (legacy fallback)
- **Authentication**: OAuth 2.0 with PKCE + Dynamic Client Registration
- **Pre-configured demo account**: Sophia Martinez (Premium plan, 5 connectors pre-connected)
- **Reviewer access**: Long-lived signed token URL (provided separately in the submission form)
- **Operator**: AI Labs LLC (Sharjah Media City Free Zone, UAE License No. 2646422.01)

---

## 0. Important — read first

To save you the setup hassle, we've pre-provisioned a demo account ("Sophia Martinez") with Premium plan and 5 data connectors already authorized (Google Ads, GA4, Search Console, Stripe, Meta Ads). You access it via a **signed reviewer token URL** sent in the submission form.

You will NOT need to:
- Create your own account or sign in with Google
- Connect any data sources manually
- Use our credentials, Gmail, or other secrets

You WILL need to:
- Open the reviewer URL first to establish the session
- Then add the MCP connector in Claude.ai — the OAuth rides the existing session
- Run the test prompts in Claude

---

## 1. Open the reviewer token URL (~10 seconds)

In your reviewer browser, paste the **reviewer token URL** (provided in the Anthropic submission form's test credentials field).

```
https://app.1clickreport.com/auth/reviewer?token=<TOKEN>
```

This signs you in as the demo account (`sophia.martinez2025@gmail.com`, Premium plan) and lands you on the 1clickreport dashboard. Keep this tab open — your session must remain active in this browser for the next steps.

Properties of this URL:
- 120-day expiry, multi-use, single-account allowlist, server-side revocable
- Does not reveal any account credentials
- Cannot be used to access any account other than the demo (`isReviewer: true` session flag, blocks billing changes and irreversible operations)

---

## 2. Add the connector in Claude.ai (~30 seconds)

In the **same browser** (so the Sophia session is available):

1. Open https://claude.ai
2. Settings → **Connectors** → **Add custom connector**
3. Enter:
   - **Name**: `1ClickReport`
   - **URL**: `https://mcp.1clickreport.com/mcp`
4. Click **Add** → **Connect**

A new tab opens for our OAuth approval. Because your Sophia session is already established (from step 1), the OAuth completes without asking for a new login. The tab closes and the Claude.ai connector shows **Connected ✅**.

---

## 3. Account-purpose mapping (read before testing tools)

Once connected, Claude can see multiple accounts in Sophia's session. **Use the right account for the right tool**:

### Google Ads

| Tool category | Use this account | Why |
|---|---|---|
| **Read** (metrics, search terms, audits, budgets, audiences) | `myPediaclinic - New` (`1761841500`, AED) | Live client account with active campaigns and rich historical data |
| **Write** (create/update campaigns, ad groups, ads, keywords) | `1clickreport` (`2214480292`, USD) | Verification-paused account with 17 historical campaigns. Safe sandbox: all creates land PAUSED, account-level pause prevents any ad serving even if a campaign were enabled. |
| Other visible accounts (Solomon SPS, Zyler AI MCCs, Claude Review MCC) | — | Ignore; organizational, not part of test surface |

### Meta Ads

| Tool category | Use this account | Why |
|---|---|---|
| **Read** (metrics, creatives, pages, engagement) | `Mypedia Clinic` Meta account | Live data, real metrics |
| **Write** (create/update campaigns, ad sets, ads) | `Dubai Wow Tours` | Own ad account under AI Labs LLC's Business Manager. Creates land PAUSED with hardcoded $50/day max budget cap at the tool boundary. |

### Other connectors (single source, auto-targeted)

| Connector | Account |
|---|---|
| **GA4** | `1clickreport` property |
| **Search Console** | `1clickreport.com` site |
| **Stripe** | connected Stripe account (read-only metrics) |

---

## 4. Verify each tool category (~5-10 minutes)

After Sophia's connector is active in Claude.ai, run the prompts below.

### Discovery (always works)

```
"What MCP tools does 1ClickReport expose?"
```
Claude lists the available tools. Verify the 40 tool names are visible (29 read-and-write + 11 Meta).

```
"What Google Ads accounts do I have access to?"
```
Calls `list_google_ads_accounts`. Returns 8 accounts including Mypedia Clinic and 1clickreport.

### Read tools — point at the right account per the mapping above

**Google Ads (use Mypedia Clinic `1761841500`):**
```
"Get my Google Ads metrics for the Mypedia Clinic account for the last 90 days"
"Show me search terms for the Mypedia Clinic account last month"
"Audit a campaign on the Mypedia Clinic account — pick one with the most spend"
"What's the budget utilization on Mypedia Clinic's campaigns?"
"Find keyword ideas for 'pediatric clinic Dubai' in the UAE"
```

**Meta Ads (use Mypedia Clinic):**
```
"Show me Meta Ads metrics for the Mypedia Clinic account last 30 days"
"List the creatives in Mypedia Clinic's Meta account"
```

**GA4 (1clickreport property):**
```
"How much traffic did the 1clickreport GA4 property get last week?"
"What events are tracked in the 1clickreport GA4 property?"
"Analyze the conversion funnel: landing-page-view → button-click → signup"
```

**Search Console (1clickreport.com):**
```
"What are my top 10 search queries by clicks last week for 1clickreport.com?"
"How is my CTR trending over the last 30 days?"
```

**Stripe:**
```
"Show me Stripe MRR for the last 6 months"
"How many active subscriptions does the connected Stripe account have?"
```

**Monitoring agents:**
```
"What monitoring rules are active on my account?"
"Show me recent monitoring activity"
```

### Write tools — point at the right account per the mapping above

**IMPORTANT SAFETY NOTES — these tools are DESIGNED to be safe:**

1. **All campaign creates land in PAUSED state** in the underlying ad platform. Nothing goes live without explicit user activation in the platform UI.
2. **The Google Ads "1clickreport" account is verification-paused at the account level** — no ad serving possible even if a campaign were enabled.
3. **Meta write tools enforce a hardcoded $50/day max daily budget** at the tool boundary regardless of what's requested.
4. **Sophia is on Premium + `isReviewer: true` session flag** — irreversible operations and billing changes are blocked by an audit layer.
5. **Monitoring rules use a two-gate flow**: chat-confirmation creates a PENDING rule; the user must explicitly activate in the SaaS dashboard. Claude can never silently activate.

**Google Ads write (use `1clickreport` account `2214480292`):**
```
"Create a Google Ads search campaign in the 1clickreport account called 'Reviewer Test Search' with $10 daily budget, MAXIMIZE_CLICKS bidding"
```
Returns campaignId, status: PAUSED. Verify in https://ads.google.com (account ID 221-448-0292) that the campaign exists in PAUSED state.

```
"Now create an ad group in that campaign with keywords 'project management software', 'team collaboration tool'"
"Write a responsive search ad for that ad group — headlines about project management for small teams"
"Update that campaign's daily budget to $20"
```

**Meta Ads write (use `Dubai Wow Tours`):**
```
"Create a Meta Ads campaign in the Dubai Wow Tours account with $20 daily budget targeting tourists interested in UAE travel"
"Create an ad set with targeting countries 'United Arab Emirates', age 25-55, $15 daily budget"
"Create an ad with headline 'Discover Dubai's hidden gems'"
```
Verify in Meta Ads Manager (under AI Labs LLC's Business Manager → Dubai Wow Tours ad account) that creations land PAUSED.

**Monitoring rule:**
```
"Set up an alert on the Mypedia Clinic Google Ads account — alert me if CPA exceeds 200 AED in the last 7 days"
```
Claude proposes the rule with rationale, severity, and threshold. After your confirmation, it calls `create_monitoring_rule` → rule lands PENDING in the SaaS dashboard. View at `app.1clickreport.com/monitoring` (Sophia's session) to see the PENDING rule and an Activate button. Confirm Claude never auto-activates.

---

## 5. Verify the gate logic (subscription handling)

Sophia is provisioned as **Premium**, so all write tools work. To verify plan-gating behavior, email `suryansh@1clickreport.com` and we'll switch Sophia's plan to Pro temporarily. Re-run a write prompt — tool returns a clear `PREMIUM_REQUIRED` error with upgrade pitch. Email us to switch back to Premium when done.

---

## 6. Verify disconnect and session cleanup

1. In Claude.ai → Settings → Connectors → 1ClickReport → **Disconnect**
2. In the Sophia browser tab → close it, or sign out of `app.1clickreport.com`
3. Reviewer token URL stays valid for re-entry until you confirm review complete and we revoke it server-side
4. To request immediate revocation: email `suryansh@1clickreport.com`

Data deletion for real (non-reviewer) users: https://www.1clickreport.com/data-deletion

---

## Anthropic review criteria checklist

| Criterion | Compliance |
|---|---|
| Tools have `title` + `readOnlyHint`/`destructiveHint` annotations | ✅ All 40 tools annotated |
| Read and write tools are separated | ✅ Verified |
| Tool descriptions name the target API explicitly | ✅ Each description ends with "Queries the X API (docs URL)" |
| Tool descriptions match actual behavior | ✅ Reviewer can verify against the test prompts above |
| OAuth 2.0 used for authenticated services | ✅ With PKCE + DCR; no API key flows |
| Privacy policy URL is HTTPS and live | ✅ https://www.1clickreport.com/privacy-policy |
| Terms of Service is HTTPS and live | ✅ https://www.1clickreport.com/terms |
| Public documentation by submission date | ✅ https://github.com/1clickreport/mcp |
| Software doesn't transfer money/crypto | ✅ Read-only Stripe access |
| Software doesn't generate AI images/audio/video | ✅ N/A |
| Working examples of prompts | ✅ Section 4 above + public README |
| Tools handle errors gracefully | ✅ Actionable text on permission errors, missing data, etc. |
| Tools are frugal with tokens | ✅ Responses scoped to user's query |
| Test access provided | ✅ Reviewer token URL (this document + submission form) |

---

## What's worth flagging in your review notes

**Pre-connected by design**: All 5 data sources (Google Ads, GA4, Search Console, Stripe, Meta Ads) are already authorized in Sophia's session. This is the demo account pattern — we did the OAuth dance ahead of time so the reviewer doesn't need our credentials or any third-party logins.

**Multi-account scenarios per platform**: Google Ads and Meta Ads each show multiple accounts in `list_*_accounts` output. Use the account-purpose mapping in Section 3 to point reads at live-data accounts and writes at safe sandbox accounts.

**Reviewer token isolation**: The `/auth/reviewer?token=...` endpoint bypasses normal signup, sets an `isReviewer: true` session flag (excludes from MRR, audit log, blocks billing changes and irreversible operations), is rate-limited per IP, and is server-side revocable.

**Operator transparency**: 1ClickReport is operated by AI Labs LLC (Sharjah Media City Free Zone, UAE, License No. 2646422.01). "1ClickReport" is the trade name; AI Labs LLC is the legal entity.

---

## Direct contact for review

If anything is unclear, blocks the review, or needs more sample data:

- **Email**: suryansh@1clickreport.com (founder, responds within hours)
- **Support**: support@1clickreport.com
- **Public docs**: https://github.com/1clickreport/mcp
- **Privacy policy**: https://www.1clickreport.com/privacy-policy
- **Terms**: https://www.1clickreport.com/terms

Thank you for the review.
