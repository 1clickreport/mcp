# Setup Walkthrough

Total time: ~2 minutes for the MCP connection + a few seconds per platform you connect.

---

## Step 1 — Add the connector in Claude.ai

1. Sign in at **https://claude.ai**
2. Click your profile icon → **Settings** → **Connectors**
3. Click **Add custom connector**
4. Fill in:
   - **Name**: `1ClickReport`
   - **URL**: `https://mcp.1clickreport.com/mcp`
5. Click **Add**

Claude will open the 1ClickReport OAuth flow in a new tab.

## Step 2 — Sign in with Google

We use Google OAuth for app authentication (not Facebook Login). One-click sign-in.

After authentication:
- New users → auto-registered with 7-day Premium free trial (no credit card)
- Existing users → resume your plan

You'll land on the 1ClickReport onboarding page at `app.1clickreport.com`.

## Step 3 — Connect at least one data source

From the onboarding page, click **Connect** on any of:

- **Google Ads** — read + write campaign data
- **Meta Ads** (Facebook & Instagram) — read + write campaigns
- **Google Analytics 4** — traffic, conversions, funnels
- **Google Search Console** — organic search performance
- **Stripe** — revenue, MRR, churn

Each opens an OAuth flow for that platform. Most take 30-60 seconds.

**You can connect as many or as few as you need.** Claude will only use the platforms you've connected.

## Step 4 — Ask Claude

Back in Claude.ai, start a new chat with the 1ClickReport connector enabled. Try:

### Read-only prompts (Pro plan)

```
"What Google Ads accounts do I have access to?"

"Audit my top-spending Google Ads campaign and tell me what to fix"

"Show me my Meta Ads ROAS by campaign for the last 30 days"

"What search queries are eating budget without converting?"

"Compare my Google Ads vs Meta Ads spend efficiency"

"Find keyword ideas for 'collagen drink' with US search volume and CPC"

"What are my top 10 search queries by clicks in Search Console?"

"Show me my Stripe MRR trend over the last 6 months"
```

### Write prompts (Premium plan + trial users)

```
"Create a Meta Ads campaign with a $20 daily budget targeting small business
 owners in the US, with conversions as the objective"

"Add these 5 negative keywords to my Brand Search campaign: [list]"

"Set up an alert if my CPA goes above $50 on any active campaign this week"

"Pause keyword 'cheap dentist' in my Dental Clinic campaign — it's wasting spend"
```

Claude will explain what it's about to do, ask for confirmation when appropriate, and execute. Writes always land in PAUSED state — nothing goes live until you activate them in the platform's native UI.

---

## How multi-account access works (for agencies)

If you have access to multiple Google Ads / Meta / GA4 accounts:

1. Claude will ask "Which account?" before pulling data — never assumes
2. Once you specify, the answer persists for the rest of the conversation
3. Mention by name ("analyze Tip Top Café") or by ID — both work
4. Each account's data is fetched on-demand; no cross-account leakage

## How AI monitoring agents work (Premium)

1. **Suggestion**: Claude analyzes your account data and proposes specific monitoring rules with rationale, severity, and thresholds grounded in YOUR baseline
2. **Confirmation**: You approve in chat with a simple "yes, set these up"
3. **Pending in dashboard**: Rule lands in `app.1clickreport.com/monitoring` as PENDING — you review the rationale, color-coded severity, and an Edit button
4. **Activation**: You click Activate (or batch-approve multiple). Rule goes live.
5. **Background evaluation**: SaaS evaluator runs every 6 hours, fetches the metric, compares to threshold
6. **Alert delivery**: If breached, you get an email with the spiked value, a 7-day sparkline, and the trend that triggered it

This is a **two-gate flow** — your chat-confirmation creates pending rules; dashboard-activation makes them live. Claude can never silently activate.

---

## Disconnect / data deletion

- **One-click disconnect**: dashboard → Integrations → Disconnect on any platform. OAuth tokens deleted immediately.
- **Account deletion**: Settings → Delete account, or email support@1clickreport.com
- **Full data deletion instructions**: https://www.1clickreport.com/data-deletion

---

## Troubleshooting

**OAuth popup blocked?** Allow pop-ups for `claude.ai` and `app.1clickreport.com`.

**"Account not found" error?** Your Claude session might have expired. Reconnect the connector in Claude Settings.

**Token expired?** Disconnect → reconnect the platform from the dashboard. Tokens are refreshed automatically on every call, but a manual reconnect resolves edge cases.

**Specific tool returning errors?** Most errors include actionable guidance (e.g., "Please ask the user which account to analyze"). Forward the exact error to support@1clickreport.com if it's unclear.
