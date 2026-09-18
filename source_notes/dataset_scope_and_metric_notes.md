# Dataset Scope and Metric Notes

## Purpose

This note documents the difference between NovaTech’s original source datasets and the modified unified dashboard dataset. It also defines the metric rules used for verification, dashboard analysis, and Topic Q testing.

## Original Source Datasets

| Dataset | Source file | Source rows | Primary identifier | Join key |
|---|---|---:|---|---|
| CRM Deals | `novatech_crm_deals.csv` | 499 | `opportunity_id` | `account_id` |
| Marketing Campaigns | `novatech_marketing_campaigns.csv` | 2,240 | `lead_id` | `account_id` |
| Support Tickets | `novatech_support_tickets.csv` | 3,000 | `ticket_id` | `account_id` |

All three datasets contain `account_id`, which connects CRM, Marketing, and Support records at the account level.

## Unified Dashboard Dataset

The dashboard uses `NovaTech Unified Revenue`, a transformed dataset created by joining the source areas through `account_id`.

CRM was used as the anchor table. Marketing and Support records were connected to CRM using the shared account identifier. The unified dataset contains additional fields and may contain repeated rows because one account can have multiple deals, leads, and support tickets.

The unified dataset should not be expected to have the same row count as any individual source file.

## Source-Level Q Verification

Baseline Q questions were asked against the original pre-indexed datasets listed under “Others.” These results verify the source files:

- CRM contains 499 rows.
- CRM contains 315 Won and 184 Lost deals.
- Marketing contains 2,240 rows.
- Marketing contains 609 responses.
- Marketing response rate is 27.19%, rounded to 27.2%.
- Support contains 3,000 tickets.
- Support contains 59 tickets with a missing `ticket_resolved_date`.
- Direct Mail has the highest source-level channel response rate at 53.0%, based on 79 responses from 149 leads.

These source-level results should not automatically be interpreted as row counts from the unified dashboard dataset.

## Metric Definitions

### CRM deals

Count deals using distinct `opportunity_id`.

```text
Deal count = distinct opportunity_id
```

The source CRM outcomes are:

- Won: 315.
- Lost: 184.
- Total: 499.

### Win rate

```text
Win rate = distinct Won opportunity_id / distinct opportunity_id
```

When using the complete source CRM population:

```text
Win rate = 315 / 499 = approximately 63.1%
```

If the dashboard displays a different denominator, report it as the dashboard population and identify the difference.

### Marketing leads

Count leads using distinct `lead_id`.

```text
Lead count = distinct lead_id
```

### Marketing response rate

A response is defined as `campaign_response = 1`.

```text
Response rate =
distinct responding lead_id / distinct lead_id
```

The original Marketing source contains 609 responses out of 2,240 records, producing:

```text
609 / 2,240 = 27.19%
```

### Support tickets

Count tickets using distinct `ticket_id`.

```text
Ticket count = distinct ticket_id
```

### Unresolved tickets

A ticket is unresolved when `ticket_resolved_date` is blank or null.

```text
Unresolved ticket =
ticket_resolved_date is blank or null
```

The original Support source contains 59 tickets with a missing resolution timestamp.

### Days to close

```text
Days to close =
deal_closed_date - deal_created_date
```

### Resolution time

```text
Resolution time =
ticket_resolved_date - ticket_created_date
```

Exclude unresolved tickets from average resolution-time calculations unless the metric explicitly treats unresolved tickets separately.

### Campaign ROI

The intended calculation is:

```text
Campaign ROI =
(revenue_attributed - campaign_spend) / campaign_spend
```

Campaign spend and attributed revenue must be handled carefully after joining. Summing these fields across repeated joined rows can produce inflated totals.

## One-to-Many Join Risk

A single account may have:

- Multiple CRM opportunities.
- Multiple Marketing interactions.
- Multiple Support tickets.

Joining these records can repeat a CRM opportunity, marketing lead, or support ticket across multiple unified rows. Simple row counts and direct sums may therefore be misleading.

Use the correct identifier for every measure:

| Business measure | Correct identifier |
|---|---|
| Deals | `opportunity_id` |
| Leads | `lead_id` |
| Tickets | `ticket_id` |
| Accounts | `account_id` |

Financial measures should be aggregated at their native grain before being combined at account level.

## Orphan Account Records

Marketing and Support include some account IDs that do not appear in CRM. These are orphan records in a CRM-anchored model.

Marketing contains 150 orphan rows, approximately 6.7% of the Marketing source.

Support contains 204 orphan rows, approximately 6.8% of the Support source.

Because CRM is the anchor, orphan Marketing and Support records may not appear in the CRM-anchored unified output. Their absence should not be interpreted as zero activity.

## Dashboard Summary Differences

The dashboard-generated summaries may use the transformed unified dataset or a filtered dashboard population. The following differences were observed:

- Sales Pipeline summary: 496 displayed opportunities versus 499 original CRM rows.
- Customer Health summary: approximately 2.79K displayed tickets versus 3,000 original Support tickets.
- Customer Health summary: 58 displayed unresolved tickets versus 59 source-level missing resolution timestamps.
- Marketing summary: 27.1% response rate versus 27.19% source-level response rate.

These differences are documented rather than silently corrected. Before using the dashboard for final financial reporting, reconcile the dashboard population with the relevant source population.

## Topic Notes

`NovaTech Revenue Intelligence Topic` was created from `NovaTech Unified Revenue`.

The Topic custom instructions define:

- `account_id` as the shared identifier.
- Distinct `opportunity_id` for deal counts.
- Distinct `lead_id` for lead counts.
- Distinct `ticket_id` for ticket counts.
- Won and Lost deal-stage meanings.
- Response-rate logic.
- Unresolved-ticket logic.
- One-to-many join limitations.
- Caution around campaign spend and attributed revenue.

The Topic could not be linked to the existing analysis because of managed workspace ownership and unavailable contributor permissions. Direct Topic Q testing may still be performed by opening the Topic in Q/Quick Chat and selecting it as the active data source.

## Reporting Rule

Every reported result should identify its scope:

- `Source-level`: calculated from one original source file.
- `Unified dashboard`: calculated from the transformed joined dataset.
- `Topic Q`: calculated from the Topic selected as the Q data source.
- `Dashboard summary`: generated from the dashboard’s displayed population and filters.

Do not compare numbers from different scopes without explaining the difference.
