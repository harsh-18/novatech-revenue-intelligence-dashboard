# NovaTech Revenue Intelligence Dashboard

An Amazon QuickSight revenue-intelligence dashboard for NovaTech Solutions, integrating CRM deals, marketing campaigns, and support tickets into a unified view of the customer journey.

## Project Objective

NovaTech’s revenue team previously relied on separate weekly reports from CRM, Marketing, and Support systems. This project creates a single interactive dashboard covering:

- Marketing Funnel
- Sales Pipeline
- Customer Health
- Natural-language Q analysis

The dashboard was designed for Sarah Chen, VP of Revenue, to support weekly operating meetings and faster cross-functional decisions.

## Repository Contents

```text
novatech-revenue-intelligence-dashboard/
├── README.md
├── deliverables/
│   ├── NovaTech_dashboard_export.pdf
│   ├── NovaTech_dashboard_annotated.pdf
│   ├── NovaTech_dashboard_executive_summary.md
│   ├── NovaTech_report_to_Sarah_Chen.md
│   ├── NovaTech_data_verification_log_final.md
│   ├── NovaTech_Q_exploration_log_final.md
│   └── NovaTech_Q_exploration_log_topic_results.md
├── screenshots/
│   ├── dashboard/
│   ├── before_topic/
│   └── after_topic/
└── source_notes/
    └── dataset_scope_and_metric_notes.md
```

## Dashboard Views

### Marketing Funnel
Tracks campaign performance, lead progression, response rates, channel effectiveness, campaign spend, and attributed revenue.

### Sales Pipeline
Tracks deal outcomes, win rate, average deal value, product performance, sales regions, company size, and loss reasons.

### Customer Health
Tracks support volume, priority, resolution status, downtime, product areas, sentiment, and account-level risk indicators.

## Data Sources

The project uses three original source datasets:

- `novatech_crm_deals.csv` — 499 CRM deal records
- `novatech_marketing_campaigns.csv` — 2,240 marketing records
- `novatech_support_tickets.csv` — 3,000 support-ticket records

All three sources share `account_id`. The unified dashboard dataset uses CRM as the anchor and connects Marketing and Support through that field.

## Data Pipeline

The dashboard data pipeline follows a controlled, account-centric flow from source data into the QuickSight model and Q analysis:

1. Source extraction
   - Pull the three raw datasets from the source systems into the project workspace.
   - Preserve original record-level values for auditability and verification.

2. Data profiling and validation
   - Check record counts, missing values, date consistency, and key-field completeness.
   - Verify that `account_id` is the shared join key across CRM, marketing, and support.

3. Standardization and metric definition
   - Normalize dates, statuses, and categorical fields across the three source tables.
   - Align metric logic with business definitions such as distinct deal IDs, lead IDs, and ticket IDs.

4. Unified account-level modeling
   - Use CRM as the anchor table.
   - Join marketing and support records to the CRM account grain using `account_id`.
   - Apply careful aggregation rules to avoid inflated totals from one-to-many joins.

5. Dashboard preparation
   - Build the QuickSight dataset using the transformed, joined account-level structure.
   - Create the required KPI calculations for funnel, pipeline, and customer-health views.
   - Validate that operational metrics match documented business logic.

6. Q and Topic analysis
   - Run baseline Q questions against the original source datasets.
   - Create a Topic dataset and custom instructions for the unified revenue model.
   - Use Q for exploratory analysis, while keeping source-level verification separate from unified dashboard results.

7. Reporting and reconciliation
   - Compare dashboard outputs to source-level checks and document any differences.
   - Treat metric differences as expected reconciliation items before final financial or executive reporting.

## Metric Controls

The sources have different record grains. One account can have multiple deals, marketing interactions, and support tickets. One-to-many joins can therefore multiply rows. The dashboard and Topic use specific logic to keep metrics meaningful:

- Deal counts use distinct `opportunity_id`.
- Lead counts use distinct `lead_id`.
- Ticket counts use distinct `ticket_id`.
- Response rate is responding distinct leads divided by distinct leads.
- Unresolved tickets have a blank or null `ticket_resolved_date`.
- Days to close is calculated from deal creation and close dates.
- Resolution time is calculated from ticket creation and resolution timestamps.
- Campaign spend, attributed revenue, and joined deal value require caution because direct sums may be inflated after one-to-many joins.

## Verified Source Results

Baseline Q questions were asked against the original pre-indexed source datasets listed under Others. Verified results include:

- CRM: 499 rows; 315 Won and 184 Lost
- Marketing: 2,240 rows; 609 responses; 27.19% overall response rate
- Support: 3,000 tickets; 59 missing resolution timestamps
- Highest source-level campaign-channel response rate: Direct Mail at 53.0%, based on 79 responses from 149 leads

These are source-level verification results. They should not automatically be treated as raw row counts from the modified unified dashboard dataset.

## Dashboard Summary Context

The generated dashboard summaries may report a different population from the source-level Q results because the dashboard uses a transformed, joined dataset. For example:

- The Sales Pipeline summary reports 496 displayed opportunities, while the original CRM source contains 499 rows.
- The Customer Health summary reports approximately 2.79K displayed tickets, while the original Support source contains 3,000 tickets.
- The Customer Health summary reports 58 unresolved tickets, while the source-level check reports 59 missing resolution timestamps.
- The Marketing summary reports a 27.1% overall response rate, consistent with the more precise source-level value of 27.19% after rounding.

These differences are documented rather than silently overwritten. They should be reconciled before using the dashboard for final financial reporting.

## Topic and Q Analysis

A Topic named `NovaTech Revenue Intelligence Topic` was created, configured, and published using `NovaTech Unified Revenue`. Custom instructions define the shared account key, distinct identifiers, and metric logic for the unified data model.

The Topic could not be linked to the existing analysis because of managed workspace ownership and unavailable contributor permissions. However, the Topic was opened directly in Q/Quick Chat and continued to support exploratory analysis.

Baseline Q results and direct Topic Q results are documented separately. This distinction ensures that source-level answers are not mislabeled as unified-dataset results and that any before-and-after comparisons remain traceable.

## Evidence

The repository should include screenshots showing:

- the three dashboard sheets
- baseline Q responses from the original source datasets
- the Topic dataset selection
- Topic custom instructions
- the active published Topic version
- Topic ownership and share settings
- the failed analysis-to-Topic linking state
- direct post-Topic Q results, if available

## Recommendations

Use the dashboard for recurring weekly metrics and operational meetings. Use Q for exploratory questions, but validate important answers against the source definitions and dashboard measures before making final business decisions or communicating financial impacts.

## Security Note

Do not upload passwords, AWS access keys, private tokens, or other credentials to this repository.
