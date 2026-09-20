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

Amazon Q in QuickSight is the natural-language capability that allows revenue leaders to ask business questions about dashboard data. To evaluate its reliability, I tested three core business questions in a baseline Quick Chat session before configuring the Topic, and then re-tested them against the published Topic. The Topic configuration applied semantic metadata, field descriptions, business synonyms, and custom instructions to govern how Q interprets NovaTech's unified data.

Here is the direct comparison of the before-and-after evidence across the three questions:

### 1. Sales Deals Won
- **Question asked:** "How many CRM deals were won?"
- **Baseline Quick Chat answer:** Q returned 315 won CRM deals, noting that the count was based on distinct opportunities where the deal stage was marked as Won.
- **Topic-scoped answer:** Q confirmed 315 CRM deals won, consistently evaluating distinct opportunities in the Won stage.
- **How Topic configuration affected accuracy:** In a unified dataset where CRM deals are joined to multiple marketing touchpoints and support tickets, a naive query risks counting repeated joined rows rather than distinct deals. The Topic resolved this by defining synonyms (such as "deal," "opportunity," and "sales opportunity") mapped directly to unique opportunity identifiers, setting the default aggregation to distinct count, and establishing business rules for deal stages. This ensured Q always returns the accurate count of 315 distinct won opportunities instead of an inflated row count.

### 2. Marketing Campaign Response Rate
- **Question asked:** "What is the marketing campaign response rate?"
- **Baseline Quick Chat answer:** Q returned an overall marketing response rate of 27.08%, derived from 566 responding leads out of 2,090 unique leads in the unified dataset.
- **Topic-scoped answer:** Q confirmed 27.08% across the 2,090 unique leads within the unified CRM-anchored dataset.
- **How Topic configuration affected accuracy:** In the unjoined marketing source file, the response rate was 27.2% across 2,240 leads and 609 responses. In the CRM-anchored unified dataset, 150 orphan marketing leads were excluded because their accounts did not exist in CRM, leaving 2,090 unique leads. The Topic improved Q accuracy by providing custom instructions that defined the response rate calculation as unique responding leads divided by total unique leads, and mapping channel synonyms (such as "marketing channel" and "acquisition channel"). This prevented Q from erroneously summing non-distinct records, misinterpreting binary response indicators, or producing skewed percentages.

### 3. Unresolved Support Tickets
- **Question asked:** "How many unresolved support tickets are there?"
- **Baseline Quick Chat answer:** Q reported 58 unresolved support tickets, identifying tickets with no recorded resolution date.
- **Topic-scoped answer:** Q confirmed 58 unresolved support tickets based on unrecorded resolution dates.
- **How Topic configuration affected accuracy:** In the raw support dataset, 59 tickets lacked resolution timestamps out of 3,000 total tickets. Within the CRM-anchored unified dataset, one orphan unresolved ticket was excluded, leaving 58 unresolved tickets. Crucially, support ticket data does not contain a dedicated text status column labeled "Unresolved"—resolution is indicated solely by the presence or absence of a timestamp. The Topic's custom instructions established the explicit business rule that unresolved means a blank or unrecorded resolution date and instructed Q to count unique ticket identifiers. Without this semantic guidance, natural-language queries could fail to interpret what unresolved meant or produce inaccurate counts by counting repeated rows from joined tables.

## Where AI Analysis Agreed or Disagreed with the Dashboard

Comparing Amazon Q's natural-language answers to the interactive dashboard visuals shows both clear alignments and important distinctions:

### Where AI Analysis Agreed with the Dashboard
- **Channel Conversion:** Q identified Direct Mail as having the highest conversion rate at 53.0% (79 responses from 149 leads), which matched the Marketing Funnel channel performance ranking.
- **Deal Sizing by Company Tier:** Q confirmed that Enterprise accounts carried the highest average deal value at approximately $1,589, while Large accounts averaged approximately $1,257, matching the Sales Pipeline breakdown.
- **Support Ticket Resolution:** Q verified that Critical priority tickets had an average resolution time of approximately 1.79 days compared to 1.97 days for Low priority tickets, consistent with Customer Health visual trends.
- **Campaign Spend Inefficiencies:** Q flagged that Partner Referral and Paid Social represented significant overspend where campaign expenditure exceeded attributed revenue, reinforcing the spend-versus-revenue comparison on the Marketing Funnel sheet.

### Where AI Analysis Disagreed or Required Caution
- **Dataset Grain and Row Counts:** When queried against original source tables, Q returned raw source counts (499 CRM deals, 2,240 marketing leads, and 3,000 support tickets with 59 unresolved). In contrast, the dashboard uses the CRM-anchored unified dataset (496 unified opportunities, 2,090 leads, and 2,790 tickets with 58 unresolved). These differences represent reconciliation items caused by orphan account filtering rather than calculation errors.
- **Cross-Source Aggregations:** For questions combining metrics across sources—such as identifying top accounts by ticket volume alongside total deal revenue—Q displayed account tables. However, joined datasets can duplicate deal values across multiple support tickets if summed directly. While the visual dashboard applies distinct-count and governed aggregations, natural-language answers combining revenue and ticket counts must always be validated at the account grain before guiding financial commitments.

## Recommendations for the Revenue Team

1. Use the Sales Pipeline in the weekly revenue meeting to review the 315 Won versus 184 Lost outcomes, then segment loss reasons by region, manager, product, and customer tier.
2. Use the Marketing Funnel to compare response rate and downstream funnel progression, not lead volume alone. The 27.2% overall response rate is a baseline for campaign-level comparison.
3. Create a weekly Customer Health review for accounts combining high ticket volume, negative sentiment, high priority, and high deal value. Begin with ACCT-010, which has 67 tickets and $1.76M in deal value.
4. Investigate orphan Marketing and Support accounts separately so that activity outside CRM is not mistaken for missing demand or missing support.
5. Establish governed unique-count measures and account-level rollups before using joined revenue or return-on-investment figures for investment allocation.

## Natural-Language Analysis and Topic Governance

I created and published a Topic named NovaTech Revenue Intelligence Topic using the unified dataset. A Topic is a defined business vocabulary and set of instructions that helps Amazon Q interpret natural-language questions consistently. It includes custom instructions for the shared account identifier, unique-count rules, deal-stage meanings, response-rate logic, and unresolved-ticket logic. This creates a useful semantic layer for natural-language exploration.

The intended division of labor is straightforward: the dashboard answers recurring weekly questions with visible filters and fixed definitions, while natural-language querying supports ad-hoc questions such as which channel has the highest conversion rate, which product area has the longest resolution time, or which Enterprise accounts show negative sentiment.

The Topic reached an active published version. However, linking it to the existing analysis was not possible because it is owned by the managed lab account and contributor permissions were unavailable. This is a workspace-permission limitation, not a data-modeling decision. The Topic configuration and ownership state were documented, while the dashboard remains the validated source for the recurring views.

## Closing Recommendation

The dashboard should become NovaTech’s weekly operating view for revenue, marketing, and customer-success coordination. It addresses the original problem by putting campaign, pipeline, and support context in one place and reducing dependence on manual report pulls. The next technical improvement should be a governed semantic model with account-level and source-level measures, allowing the team to combine customer context without multiplying transactional financial values.

This first version is intentionally focused on descriptive reporting. It gives Sarah and the revenue team a dependable view of what happened and what requires attention now, while leaving predictive modeling and forecasting for a later phase after the metric definitions and account-level data model are governed.