# ProfitGuard Documentation

> **ProfitGuard** — Profit Leak Detection & Recovery Engine for Shopify merchants.

Last updated: 2026-03-23

---

## Table of Contents

1. [Getting Started](#getting-started)
2. [Dashboard](#dashboard)
3. [Metrics](#metrics)
4. [Product Profit](#product-profit)
5. [Action Center](#action-center)
6. [Cause Finder](#cause-finder)
7. [Dead Inventory](#dead-inventory)
8. [AI Weekly Summary](#ai-weekly-summary)
9. [Alert Thresholds](#alert-thresholds)
10. [Plans & Billing](#plans--billing)
11. [COGS Import](#cogs-import)
12. [FAQ](#faq)

---

## Getting Started

### Install the app
1. Install ProfitGuard from the Shopify App Store.
2. You will automatically start a **7-day free trial** with full access to all features.
3. No credit card required for the trial.

### First steps after install
- Visit the **Dashboard** to see your profit overview. Demo data is shown if you have no orders yet.
- Import your cost-of-goods (COGS) data via the **Product Profit** page for accurate margin calculations.
- Customize **Alert Thresholds** to match your store's normal trading patterns.

---

## Dashboard

The Dashboard is your profit command center. It shows:

| Section | Description |
|---|---|
| **Revenue (30d)** | Total revenue from the last 30 days, with % change vs previous 30 days |
| **Net Profit** | Revenue minus COGS, shipping, payment fees, refunds, and discounts |
| **Refund Rate** | Percentage of revenue refunded, with directional change |
| **Active Alerts** | Count of current warning-level profit alerts |
| **Revenue Trend** | 12-week sparkline showing weekly revenue trajectory |
| **Top Products** | Top 5 products by net profit, with mini bar chart |
| **Active Alerts list** | Snooze or resolve alerts directly from the dashboard |

### Profit Health badge
- ✅ **Healthy** — No alerts, refund rate ≤ 5%
- ⚠️ **Monitor** — 1–3 alerts or refund rate 5–10%
- 🔴 **Critical** — 4+ alerts or refund rate > 10%

### Order cap warning
If your plan has an order limit, a banner appears when the analysis was capped. Upgrade your plan to remove the cap.

---

## Metrics

The Metrics page provides a deeper analytical view:

- **Profit Health Score** — Composite score based on margin, refund rate, and trend
- **6 KPI Cards** — Revenue, Orders, COGS, Refunds, Discounts, Net Profit with period-over-period change
- **Where Your Money Goes** — Visual cost breakdown bar
- **Revenue & Refund Trend** — Dual-axis chart for the last 30 days
- **Period Comparison** — Side-by-side table comparing current vs previous period for 9 metrics
- **Profit Leaks** — Top cost drivers pulling your margin down
- **Recommendations** — Dynamic tips triggered by your actual data (not generic advice)

### Configurable payment fee rate
Set `PG_PAYMENT_FEE_RATE` (default `0.029`) and `PG_PAYMENT_FEE_FLAT` (default `0.30`) in your environment to match your Shopify payment gateway rate.

---

## Product Profit

Breaks down profit to the individual SKU level.

**How profit is calculated per product:**
```
Net Profit = Revenue − COGS − Shipping (proportional) − Payment Fees (by revenue share) − Refunds − Discounts
```

Revenue per line item uses `discountedTotalSet` from Shopify (actual price after discounts applied), not a quantity-based estimate.

### Key sections
- **Hero Banner** — Total net profit, margin, units sold, health badge
- **Top Profit Makers** — Best performers by net profit
- **Losing Money On** — Products with negative margin; shows break-even price and price increase needed
- **Best Sellers by Volume** — Top 5 by units with margin opportunity flags
- **Pricing Optimizer** — Break-even price targeting 20% margin, monthly recovery potential
- **Missing COGS Warning** — Lists products with no cost data and how to fix it
- **Full Product Table** — Sortable by Profit / Revenue / Margin / Units

### Status labels
| Label | Meaning |
|---|---|
| ⭐ Star | Margin ≥ 40% |
| ✅ OK | Margin 10–40% |
| ⚠️ Thin | Margin 0–10% |
| ❌ Losing | Negative margin |
| ❓ No COGS | Cost data missing |

---

## Action Center

Your central hub for all active profit leaks, ranked by financial impact.

- **Total profit at risk** — Sum of estimated losses across all active alerts
- **Top Money Leaks** — Ranked cards with estimated dollar impact, % change, and severity
- **Customer Risk Card** — Customers with high refund history and risk scores
- **Alert Cards** — Full detail for each alert with Snooze (7 days) and Resolve actions
- **Action Suggestions** — Prioritized steps with estimated recovery amounts

### Snooze vs Resolve
- **Snooze** — Hides the alert for 7 days. It reappears if the condition persists.
- **Resolve** — Permanently dismisses the alert. Use this when you've fixed the underlying issue.

---

## Cause Finder

Identifies which products are driving your refund costs and why.

- Analyses all refunded orders in the selected period (7 / 30 / 90 days)
- Extracts Shopify `refunds.note` as the reason per variant
- Allocates refund dollars proportionally by line item value
- Shows concentration warning when one SKU drives > 25% of total refunds

### Interpreting results
- **High concentration** → One product is the primary problem; investigate quality, descriptions, or sizing
- **Spread across many SKUs** → Systemic issue (e.g., shipping damage, misleading imagery)

---

## Dead Inventory

Finds products with zero sales in the last **60 days** that still have stock on hand.

**Why it matters:** Unsold stock ties up cash and incurs storage costs. Dead inventory = cash you can't invest in winning products.

- **Cash Tied Up** = `inventoryQuantity × unitCost` per variant
- **Worst Offenders** — Top 3 by cash tied up
- **Stale Days** — Days since last sale (real data shows 60+ days; demo shows varying days for illustration)

### Recommended actions shown in-app
1. Clearance sale (e.g., 30–50% off)
2. Bundle with bestsellers
3. Negotiate with suppliers to return or swap stock
4. Reduce reorder quantities for slow movers
5. Improve product listings (photos, descriptions, SEO)

---

## AI Weekly Summary

Generates a plain-English profit summary for your store using GPT-4o-mini.

**Requires:** `PROFITGUARD_AI_KEY` environment variable set to your OpenAI API key.

The summary covers:
- Revenue, orders, refunds, and net profit vs previous period
- Top 3 products by profit and any loss products
- Full cost breakdown (COGS, shipping, fees, refunds, discounts)
- Dead inventory alert with worst offenders
- 7 prioritized recommendations (🔴 critical / 🟡 attention / 🟢 positive)

If no OpenAI key is set, a **template-based summary** is generated automatically — no AI key needed for this fallback.

### Source badge
- 🤖 **AI-generated** — GPT-4o-mini powered narrative
- 📋 **Template-based** — Rule-based summary using your data

---

## Alert Thresholds

Customize when ProfitGuard raises alerts. Defaults work for most stores but you can tune them to reduce noise.

| Alert | Default threshold | What it means |
|---|---|---|
| Refund Rate Increase | +2 percentage points | Refund rate rose by 2 pts vs previous period |
| Revenue Drop | −10% | Revenue fell by 10% vs previous period |
| Discount Rate Increase | +2 percentage points | Discount usage rose by 2 pts |
| Order Drop | −20% | Order count fell by 20% |

### Preset configurations
- **Conservative** — Lower thresholds, more sensitive; good for stable, high-margin stores
- **Balanced** — Default settings; suitable for most stores
- **Strict** — Higher thresholds; good for stores with naturally volatile sales patterns

---

## Plans & Billing

| Plan | Price | Orders analysed/run | Key features |
|---|---|---|---|
| **Free** | $0 | 50 orders | Basic profit analytics, 1 alert/run |
| **Growth** | $19/mo | 500 orders | AI Weekly Summary, unlimited alerts, 90-day history |
| **Advanced** | $39/mo | 2,000 orders | Dead Inventory, Cause Finder, custom thresholds, 1-year history |
| **Premium** | $59/mo | Unlimited | All features, unlimited history, priority support |

### 7-day free trial
All new installs start with a 7-day free trial with full feature access. No credit card required.

### After trial expires
Your account reverts to the Free plan. Your data is preserved. Upgrade any time to restore full access.

### Cancellation
You can cancel at any time from the **Billing** page. Access continues until the end of your billing period.

---

## COGS Import

Accurate profit margins require cost-of-goods (COGS) data. There are two ways to add it:

### Option 1: CSV Import
1. Download the sample CSV from the **Product Profit** page
2. Fill in `variant_id` and `unit_cost` for each SKU
3. Upload via the Import COGS button

Sample CSV format:
```csv
variant_id,unit_cost,currency
gid://shopify/ProductVariant/123456789,12.50,USD
gid://shopify/ProductVariant/987654321,8.00,USD
```

### Option 2: Auto-enrichment (server-side)
Run the enrichment script to pull `inventoryItem.unitCost` from Shopify directly:
```bash
npm run enrich:cogs
```
Requires the shop's access token to be stored in the `Shop.accessToken` field.

### Option 3: Shopify variant cost sync
If you set costs directly in Shopify (Products → Variant → Cost per item), ProfitGuard reads these automatically via the Admin GraphQL API.

---

## FAQ

**How often is data refreshed?**  
Real-time on every page load (up to your plan's order limit). Weekly snapshots are saved automatically by the cron job every Monday.

**Does ProfitGuard work with multiple currencies?**  
Yes. Values are read in your shop's default currency (`shopMoney`). Multi-currency display conversion is planned for a future release.

**Why does my net profit look different from Shopify?**  
Shopify shows gross sales. ProfitGuard deducts COGS, estimated payment fees (2.9% + $0.30), shipping allocated per order, refunds, and discounts. Import COGS data for the most accurate result.

**What happens to my data if I uninstall?**  
Data is retained per our Privacy Policy for a defined period, then deleted. You can request immediate deletion by contacting support.

**Can I use ProfitGuard on multiple stores?**  
Yes. Each store installation is independent with its own data and billing.

**How do I disable demo/sample data?**  
Set `PG_DISABLE_DEMO=true` in your environment. All pages will show real empty states instead of demo data.

---

*For support, use the **Support** page inside the app.*  
*For privacy questions: support@zestorax.com*
