# NovaTech Data Verification Log

**Student Name:** Harsh Verma  
**Date:** September 17, 2026

## Verification Scope

Q was queried against the original pre-indexed source datasets listed under **Others**:

- `novatech_crm_deals.csv`
- `novatech_marketing_campaigns.csv`
- `novatech_support_tickets.csv`

The dashboard was built from the modified `NovaTech Unified Revenue` dataset, which contains joined source records and additional fields. Therefore, the entries below verify source-level facts. They should not be interpreted as row counts from the joined dashboard dataset.

## Verification Log

| # | Knowledge Base | Question Asked | Expected Answer | Q's Actual Answer | Match? | Notes |
|---|---|---|---|---|---|---|
| 1 | NovaTech CRM Deals | How many rows are in CRM Deals? | 499 rows. | The `novatech_crm_deals.csv` dataset contains 499 rows. | Yes | Source-level count from the original CRM dataset. |
| 2 | NovaTech CRM Deals | How many CRM deals are Won and Lost? | 315 Won and 184 Lost. | The response shows 315 Won, 184 Lost, and 499 total deals. | Yes | Source-level outcome counts. |
| 3 | NovaTech Marketing Campaigns | How many rows are in Marketing Campaigns? | 2,240 rows. | The `novatech_marketing_campaigns.csv` dataset contains 2,240 rows. | Yes | Source-level count from the original Marketing dataset. |
| 4 | NovaTech Marketing Campaigns | What is the Marketing response rate? | 27.2%, calculated from 609 responses out of 2,240 records. | The overall response rate is 27.19%, or 609 responses out of 2,240 campaign records. | Yes | 27.19% is the more precise display of the rounded 27.2% value. |
| 5 | NovaTech Support Tickets | How many Support Tickets are there? | 3,000 tickets. | There are 3,000 support tickets in the dataset. | Yes | Source-level count from the original Support dataset. |
| 6 | NovaTech Support Tickets | How many Support tickets have a missing resolved timestamp? | 59 tickets. | There are 59 support tickets with a missing resolved timestamp; `ticket_resolved_date` is null. | Yes | Source-level missingness check. |
| 7 | NovaTech Reference Documents | Which campaign channel has the highest conversion rate? | Direct Mail, 79 responses out of 149 leads, or 53.0%. | Direct Mail has the highest conversion rate at 53.0%, with 79 responses out of 149 leads. | Yes | Source-level channel comparison. |

## Cross-Check

- **Fact verified:** CRM deal outcomes.
- **Chat said:** 315 Won, 184 Lost, and 499 total deals.
- **QuickSight shows:** The CRM outcome table displays Won = 315, Lost = 184, and Total = 499.
- **Consistent?:** Yes.
- **Scope note:** This cross-check is against the original CRM source, not a raw row count of the joined unified dataset.

## Interpretation Note

The Q evidence is valid because the questions were answered from the original source files. The modified unified dataset may contain additional columns and repeated rows created by one-to-many joins. For dashboard calculations, use distinct `opportunity_id`, `lead_id`, and `ticket_id` and avoid treating the unified row count as the original source row count.
