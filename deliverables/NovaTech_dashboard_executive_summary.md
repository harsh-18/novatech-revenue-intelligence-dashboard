# NovaTech Revenue Intelligence Dashboard
## Executive Summary

**Prepared for:** Sarah Chen, VP of Revenue  
**Prepared by:** Harsh Verma  
**Date:** September 18, 2026

## Marketing Funnel

The Marketing Funnel provides a comprehensive overview of campaign performance, tracking lead progression, response rates, channel effectiveness, and revenue attribution across multiple marketing campaigns.

The overall Lead to Campaign Response Rate is 27.1%. The generated sheet summary identifies very low channel-level conversion in the displayed unified analysis; for example, Partner Referral is shown with 757 leads and only two campaign responses. Because the source-level Q verification separately reports Direct Mail at 53.0% using 79 responses out of 149 leads, these figures should not be treated as interchangeable. The difference likely reflects different filters, dataset grain, or metric definitions between the sheet summary and the source-level query.

The lead funnel shows strong early-stage activity, with Qualified Lead at 788 distinct leads, followed by Lead at 434 and Prospect at 384. Total campaign spend is shown as $353,510,938.80, compared with total attributed revenue of $25,050,491.47. This large gap signals poor apparent overall ROI in the displayed analysis; however, campaign spend and attributed revenue should be validated at the source grain before making budget decisions because the unified dataset contains joined records and may duplicate financial fields.

Digital Retarget is the top revenue contributor at $5,657,024.35, while NovaEdge Awareness contributes $209,812.52. These results suggest that the revenue team should investigate the campaign definitions, attribution logic, and source-level spend before reallocating budget. Campaigns should be compared using consistent filters and distinct lead identifiers.

## Sales Pipeline

The Sales Pipeline provides an overview of sales performance, tracking deal outcomes, win rates, and deal values across the opportunities represented in the dashboard.

The sheet summary reports 496 total opportunities, with 315 Won and 184 Lost, producing a displayed win rate of 63.5%. The source-level CRM verification reports 499 rows, with the same 315 Won and 184 Lost outcomes. Because these figures do not share the same total, the dashboard’s 496 opportunity total should be treated as the filtered or joined-analysis result, while 499 is the original CRM source total.

The average displayed deal value is approximately $1.59K, providing a baseline measure of typical deal size across the pipeline. The majority of the displayed opportunities are successfully closed, suggesting effective sales execution overall. For executive reporting, the team should confirm whether the average uses distinct `opportunity_id` and whether Lost deals with zero deal value are included.

The Sales Pipeline can support weekly review of outcomes by product, company size, sales region, manager, and loss reason. Further analysis should focus on the 184 Lost source deals and the primary reasons behind those losses.

## Customer Health

The Customer Health sheet provides an overview of support-ticket operations, tracking total volume, priority levels, resolution status, downtime impact, and monthly trends.

The sheet summary reports approximately 2.79K total support tickets, including 414 high-priority tickets. The source-level Support verification reports 3,000 tickets, including 59 tickets with a missing `ticket_resolved_date`. The difference indicates that the dashboard summary is based on a filtered, joined, or otherwise transformed view rather than the full source file. Both values should therefore be retained with their dataset context.

The summary reports only 58 unresolved tickets, suggesting a strong resolution rate across the displayed dashboard population. The source-level check identifies 59 missing resolution timestamps. This one-ticket difference should be documented and reconciled before using unresolved-ticket counts as an operational KPI.

Average downtime is 19.56 minutes, providing a baseline for measuring operational interruptions. Monthly ticket volume peaked at 159 in August 2023 and declined to 107 in February 2025. The reported three-month compounded growth rate of -5.8% signals a declining recent trend, although the revenue team should confirm the date filter and completeness of the most recent period.

Customer Health should be used to identify accounts combining high ticket volume, high-priority issues, negative sentiment, downtime, and high deal value. Those accounts require coordinated attention from Support, Customer Success, and Sales.

## Executive Interpretation

The dashboard provides a useful cross-functional view of marketing activity, sales outcomes, and customer-support health. It also exposes an important data-governance issue: the generated sheet summaries and the source-level Q answers do not always use the same population or grain. Examples include 496 versus 499 opportunities, approximately 2.79K versus 3,000 support tickets, and 58 versus 59 unresolved tickets.

For recurring weekly decisions, use the dashboard’s visible filters and definitions consistently. For financial or retention decisions, validate measures against the original source datasets and use distinct identifiers: `opportunity_id` for deals, `lead_id` for leads, and `ticket_id` for support tickets. Avoid blindly summing campaign spend, attributed revenue, deal value, or row counts after one-to-many joins.

The dashboard is therefore suitable as an operational starting point and decision-support tool. Before using it as the final financial reporting source, NovaTech should reconcile the source-to-dashboard populations and implement governed account-level and source-grain measures.
