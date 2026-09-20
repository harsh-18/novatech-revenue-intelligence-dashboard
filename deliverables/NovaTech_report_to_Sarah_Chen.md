# Revenue Intelligence Dashboard
## Report to Sarah Chen, VP of Revenue

**Prepared by:** Harsh Verma  
**Date:** September 20, 2026  
**Subject:** A unified weekly view of NovaTech’s customer journey

## Executive Summary

Sarah, the purpose of this dashboard is to replace the Monday-morning process of copying numbers from three disconnected tools into a slide deck. NovaTech already has useful CRM, marketing, and support data; the problem is that the data is siloed, so the revenue team cannot easily trace a campaign to a closed deal or identify whether a high-value account is also a high-support-risk account.

I built the NovaTech Revenue Intelligence Dashboard as a single interactive workspace with three views: Marketing Funnel, Sales Pipeline, and Customer Health. Together, these views connect the customer journey from marketing touchpoint to sales outcome to support experience. The dashboard is designed for weekly operating meetings and self-service investigation rather than predictive modeling or a pixel-perfect board report. This matches the brief’s priority: a practical tool the revenue team can begin using quickly.

The dashboard provides a foundation for faster decisions, but its measures must be interpreted at the correct level of detail. CRM contains 499 deals, Marketing contains 2,240 lead interactions, and Support contains 3,000 tickets. The three sources are connected through the account identifier, but one account can have multiple records in each source. Counts therefore use unique identifiers, and joined revenue totals require caution.

## What Was Built

### Marketing Funnel

This view addresses how marketing investment becomes pipeline. It supports campaign and channel analysis, funnel progression, response rates, and campaign-spend context. Filters are available for campaign name, channel, date range, and customer segment.

The Marketing source contains 2,240 leads and 609 responding leads, producing a 27.2% response rate. The dashboard can help the marketing team distinguish lead volume from engagement quality and compare campaigns before increasing spend. Campaign return on investment should be reviewed carefully because attributed revenue and campaign spend can be duplicated when data with one-to-many relationships is combined.

### Sales Pipeline

This view addresses deal flow, outcomes, win rates, products, regions, managers, loss reasons, average deal value, and time to close. CRM contains 499 closed opportunities: 315 Won and 184 Lost. Using unique opportunity identifiers, the overall win rate is approximately 63.2% across all 499 opportunities.

The Sales Pipeline should be the starting point for weekly conversion reviews. The team can compare loss reasons across regions, managers, product lines, and customer segments. Because NovaPulse is NovaTech’s core product line and the company sells across Enterprise, Large, Medium, and Small tiers, the dashboard can also help identify where the strongest conversion and deal-value patterns occur.

### Customer Health

This view addresses the risk that was previously difficult to see: whether important accounts are also experiencing significant support problems. It includes ticket volume, priority, resolution status, resolution time, product area, customer tier, region, and customer sentiment. Support contains 3,000 tickets, including 59 unresolved tickets—tickets that do not yet have a recorded resolution date.

The recommended risk lens is an account with high ticket volume, negative sentiment, and high deal value. For example, ACCT-010 has 67 support tickets alongside $1.76M in deal value, making it the highest-priority customer-health risk. This combination may threaten retention and expansion revenue. Sales and Support should review the account weekly, identify recurring ticket drivers, and create a recovery plan with the customer.

High-priority issues should be reviewed separately from routine ticket volume, especially where users affected, downtime, payment impact, security incidents, or data loss increase business exposure.

## Data Approach and Controls

The unified dataset uses CRM as the anchor source and connects Marketing and Support through the account identifier. This preserves CRM-account context while allowing cross-functional analysis. However, Marketing includes 150 orphan rows, or 6.7% of its source records, and Support includes 204 orphan rows, or 6.8%, for account identifiers that do not appear in CRM. Those records are not represented in a CRM-anchored output and should not be interpreted as zero activity.

The principal control is to use the appropriate unique identifier for each business question:

- Deal counts use unique opportunity identifiers.
- Lead counts use unique lead identifiers.
- Ticket counts use unique ticket identifiers.
- Response rate is the share of unique leads that responded.
- Unresolved tickets are tickets with no recorded resolution date.
- Days to close is the number of calendar days between when a sales opportunity is created and when it closes.
- Resolution time is the number of calendar days between when a support ticket is created and when it is resolved.

These controls matter because a one-to-many relationship can multiply rows when sources are combined. A simple row count may overstate deals, leads, or tickets, and blindly summing deal value, campaign spend, or attributed revenue may overstate financial performance. I recommend validating executive revenue totals against the original CRM source or a governed account-level model before using them for forecasting or budget decisions.

## How Topic Configuration Improved Q Accuracy

Amazon Q in QuickSight is the natural-language capability that allows users to ask business questions about dashboard data. Before the Topic was configured, the baseline Quick Chat tests for deals won, marketing response rate, and unresolved tickets relied mainly on broad matching of question wording to available data fields. That approach is potentially ambiguous in a unified dataset because one sales opportunity can be associated with multiple marketing records or support tickets.

The Topic improved Q accuracy by adding business descriptions, synonyms, and custom instructions that define NovaTech’s key measures. For example, it identifies a ticket identifier as a unique support-ticket record and instructs Q to count unique tickets when calculating ticket volume. It also defines an unresolved ticket as a ticket without a recorded resolution date. These instructions make the answer to an unresolved-ticket question follow the agreed support definition rather than a generic count of repeated joined rows.

The same configuration improved sales and marketing questions. Synonyms such as “deal,” “opportunity,” and “sales opportunity” help Q interpret a question about deals won as a count of unique sales opportunities. Business guidance for campaign response rate tells Q to interpret the measure as the share of campaign records that received a response. In addition, synonyms such as “channel,” “marketing channel,” and “acquisition channel” help Q connect common executive language to the correct campaign dimension.

The Topic-scoped answers therefore aligned more closely with the dashboard’s business definitions than the baseline Quick Chat answers. The configuration does not remove the need to validate results: the unified dataset contains one-to-many relationships, so revenue and campaign-spend totals still require the safeguards defined in the Topic. However, the before-and-after tests show that descriptions, synonyms, and counting instructions reduce ambiguity and make common executive questions more consistent with NovaTech’s defined dashboard metrics.

## Recommendations for the Revenue Team

1. Use the Sales Pipeline in the weekly revenue meeting to review the 315 Won versus 184 Lost outcomes, then segment loss reasons by region, manager, product, and customer tier.
2. Use the Marketing Funnel to compare response rate and downstream funnel progression, not lead volume alone. The 27.2% overall response rate is a baseline for campaign-level comparison.
3. Create a weekly Customer Health review for accounts combining high ticket volume, negative sentiment, high priority, and high deal value. Begin with ACCT-010, which has 67 tickets and $1.76M in deal value.
4. Investigate orphan Marketing and Support accounts separately so that activity outside CRM is not mistaken for missing demand or missing support.
5. Establish governed unique-count measures and account-level rollups before using joined revenue or return-on-investment figures for investment allocation.

## Natural-Language Analysis

I created and published a Topic named NovaTech Revenue Intelligence Topic using the unified dataset. A Topic is a defined business vocabulary and set of instructions that helps Amazon Q interpret natural-language questions consistently. It includes custom instructions for the shared account identifier, unique-count rules, deal-stage meanings, response-rate logic, and unresolved-ticket logic. This creates a useful semantic layer for natural-language exploration.

The intended division of labor is straightforward: the dashboard answers recurring weekly questions with visible filters and fixed definitions, while natural-language querying supports ad-hoc questions such as which channel has the highest conversion rate, which product area has the longest resolution time, or which Enterprise accounts show negative sentiment.

The Topic reached an active published version. However, linking it to the existing analysis was not possible because it is owned by the managed lab account and contributor permissions were unavailable. This is a workspace-permission limitation, not a data-modeling decision. The Topic configuration and ownership state were documented, while the dashboard remains the validated source for the recurring views.

## Closing Recommendation

The dashboard should become NovaTech’s weekly operating view for revenue, marketing, and customer-success coordination. It addresses the original problem by putting campaign, pipeline, and support context in one place and reducing dependence on manual report pulls. The next technical improvement should be a governed semantic model with account-level and source-level measures, allowing the team to combine customer context without multiplying transactional financial values.

This first version is intentionally focused on descriptive reporting. It gives Sarah and the revenue team a dependable view of what happened and what requires attention now, while leaving predictive modeling and forecasting for a later phase after the metric definitions and account-level data model are governed.