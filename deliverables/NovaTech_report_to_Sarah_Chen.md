# Revenue Intelligence Dashboard
## Report to Sarah Chen, VP of Revenue

**Prepared by:** Harsh Verma 
**Date:** September 18, 2026  
**Subject:** A unified weekly view of NovaTech’s customer journey

## Executive Summary

Sarah, the purpose of this dashboard is to replace the Monday-morning process of copying numbers from three disconnected tools into a slide deck. NovaTech already has useful CRM, marketing, and support data; the problem is that the data is siloed, so the revenue team cannot easily trace a campaign to a closed deal or identify whether a high-value account is also a high-support-risk account.

I built the NovaTech Revenue Intelligence Dashboard as a single interactive workspace with three views: Marketing Funnel, Sales Pipeline, and Customer Health. Together, these views connect the customer journey from marketing touchpoint to sales outcome to support experience. The dashboard is designed for weekly operating meetings and self-service investigation rather than predictive modeling or a pixel-perfect board report. This matches the brief’s priority: a practical tool the revenue team can begin using quickly.

The dashboard provides a foundation for faster decisions, but its measures must be interpreted at the correct data grain. CRM contains 499 deals, Marketing contains 2,240 lead interactions, and Support contains 3,000 tickets. The three sources are connected with `account_id`, but one account can have multiple records in each source. Counts therefore use distinct identifiers and joined revenue totals require caution.

## What was built

### Marketing Funnel

This view addresses how marketing investment becomes pipeline. It supports campaign and channel analysis, funnel progression, response rates, and campaign-spend context. Filters are available for campaign name, channel, date range, and customer segment.

The Marketing source contains 2,240 leads and 609 responding leads, producing a 27.2% response rate. The dashboard can help the marketing team distinguish lead volume from engagement quality and compare campaigns before increasing spend. Campaign ROI should be reviewed carefully because attributed revenue and campaign spend can be duplicated if they are summed after a one-to-many join.

### Sales Pipeline

This view addresses deal flow, outcomes, win rates, products, regions, managers, loss reasons, average deal value, and time to close. CRM contains 499 closed opportunities: 315 Won and 184 Lost. Using distinct `opportunity_id`, the overall win rate is approximately 63.2% when calculated across all 499 opportunities.

The Sales Pipeline should be the starting point for weekly conversion reviews. The team can compare loss reasons across regions, managers, product lines, and customer segments. Because NovaPulse is NovaTech’s core product line and the company sells across Enterprise, Large, Medium, and Small tiers, the dashboard can also help identify where the strongest conversion and deal-value patterns occur.

### Customer Health

This view addresses the risk that was previously difficult to see: whether important accounts are also experiencing significant support problems. It includes ticket volume, priority, resolution status, resolution time, product area, customer tier, region, and customer sentiment. Support contains 3,000 tickets, including 59 unresolved records where `ticket_resolved_date` is blank.

The recommended risk lens is an account with high ticket volume, negative sentiment, and high deal value. Such an account deserves coordinated attention from Sales, Customer Success, and Support. High-priority issues should be reviewed separately from routine ticket volume, especially where users affected, downtime, payment impact, security incidents, or data loss increase business exposure.

## Data approach and controls

The unified dataset uses CRM as the anchor table and connects Marketing and Support through `account_id`. This preserves CRM-account context while allowing cross-functional analysis. However, Marketing includes 150 orphan rows, or 6.7% of its source records, and Support includes 204 orphan rows, or 6.8%, for account IDs that do not appear in CRM. Those records are not represented in a CRM-anchored output and should not be interpreted as zero activity.

The principal control is to use the correct identifier for each business question:

- Deal counts use distinct `opportunity_id`.
- Lead counts use distinct `lead_id`.
- Ticket counts use distinct `ticket_id`.
- Response rate is responding distinct leads divided by distinct leads.
- Unresolved tickets are tickets with a blank or null resolution timestamp.
- Days to close is `deal_closed_date − deal_created_date`.
- Resolution time is `ticket_resolved_date − ticket_created_date`.

These controls matter because a one-to-many join can multiply rows. A simple row count may overstate deals, leads, or tickets, and blindly summing `deal_value`, `campaign_spend`, or `revenue_attributed` may overstate financial performance. I recommend validating executive revenue totals against the original CRM source or a governed account-level model before using them for forecasting or budget decisions.

## Recommendations for the revenue team

1. Use the Sales Pipeline in the weekly revenue meeting to review the 315 Won versus 184 Lost outcomes, then segment loss reasons by region, manager, product, and customer tier.
2. Use the Marketing Funnel to compare response rate and downstream funnel progression, not lead volume alone. The 27.2% overall response rate is a baseline for campaign-level comparison.
3. Create a weekly Customer Health review for accounts combining high ticket volume, negative sentiment, high priority, and high deal value.
4. Investigate orphan Marketing and Support accounts separately so that activity outside CRM is not mistaken for missing demand or missing support.
5. Establish governed distinct-count measures and account-level rollups before using joined revenue or ROI figures for investment allocation.

## Natural-language analysis

I created and published a Topic named NovaTech Revenue Intelligence Topic using the unified dataset. The Topic includes custom instructions defining the shared `account_id`, distinct-count rules, deal-stage meanings, response-rate logic, and unresolved-ticket logic. This creates a useful semantic layer for natural-language exploration.

The intended division of labor is straightforward: the dashboard answers recurring weekly questions with visible filters and fixed definitions, while natural-language querying supports ad-hoc questions such as which channel has the highest conversion rate, which product area has the longest resolution time, or which Enterprise accounts show negative sentiment.

The Topic reached an active published version. However, linking it to the existing analysis was not possible because it is owned by the managed lab account, `AuthorPro_16829559@vocareum.com`, and contributor permissions were unavailable. This is a workspace permission limitation, not a data-modeling decision. The Topic configuration and ownership state were documented, while the dashboard remains the validated source for the recurring views.

## Closing recommendation

The dashboard should become NovaTech’s weekly operating view for revenue, marketing, and customer-success coordination. It addresses the original problem by putting campaign, pipeline, and support context in one place and reducing dependence on manual report pulls. The next technical improvement should be a governed semantic model with account-level and source-grain measures, allowing the team to combine customer context without multiplying transactional financial values.

This first version is intentionally focused on descriptive reporting. It gives Sarah and the revenue team a dependable view of what happened and what requires attention now, while leaving predictive modeling and forecasting for a later phase after the metric definitions and account-level data model are governed.
