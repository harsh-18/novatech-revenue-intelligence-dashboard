NovaTech Revenue Intelligence Dashboard
An Amazon QuickSight revenue-intelligence dashboard for NovaTech Solutions, integrating CRM Deals, Marketing Campaigns, and Support Tickets into a unified view of the customer journey.

Project Objective
NovaTech’s revenue team previously relied on separate weekly reports from CRM, Marketing, and Support systems. This project creates a single interactive dashboard covering:

Marketing Funnel.

Sales Pipeline.

Customer Health.

Natural-language Q analysis.

The dashboard was designed for Sarah Chen, VP of Revenue, to support weekly operating meetings and faster cross-functional decisions.

Repository Contents
text
novatech-revenue-intelligence-dashboard/
├── README.md
├── deliverables/
│   ├── NovaTech_dashboard_export.pdf
│   ├── NovaTech_dashboard_annotated.pdf
│   ├── NovaTech_dashboard_executive_summary.md
│   ├── NovaTech_report_to_Sarah_Chen_reframed.md
│   ├── NovaTech_data_verification_log_final.md
│   ├── NovaTech_Q_exploration_log_final.md
│   └── NovaTech_Q_exploration_log_topic_results.md
├── screenshots/
│   ├── dashboard/
│   ├── before_topic/
│   └── after_topic/
└── source_notes/
    └── dataset_scope_and_metric_notes.md
Dashboard Views
Marketing Funnel
Tracks campaign performance, lead progression, response rates, channel effectiveness, campaign spend, and attributed revenue.

Sales Pipeline
Tracks deal outcomes, win rate, average deal value, product performance, sales regions, company size, and loss reasons.

Customer Health
Tracks support volume, priority, resolution status, downtime, product areas, sentiment, and account-level risk indicators.

Data Sources
The project uses three original source datasets:

novatech_crm_deals.csv — 499 CRM deal records.

novatech_marketing_campaigns.csv — 2,240 Marketing records.

novatech_support_tickets.csv — 3,000 Support-ticket records.

All three sources share account_id. The unified dashboard dataset uses CRM as the anchor and connects Marketing and Support through that field.

Metric Controls
The sources have different record grains. One account can have multiple deals, marketing interactions, and support tickets. One-to-many joins can therefore multiply rows. The dashboard and Topic use these controls:

Deal counts use distinct opportunity_id.

Lead counts use distinct lead_id.

Ticket counts use distinct ticket_id.

Response rate is responding distinct leads divided by distinct leads.

Unresolved tickets have a blank or null ticket_resolved_date.

Days to close is calculated from deal creation and close dates.

Resolution time is calculated from ticket creation and resolution timestamps.

Campaign spend, attributed revenue, and joined deal value require caution because direct sums may be inflated after one-to-many joins.

Verified Source Results
Baseline Q questions were asked against the original pre-indexed source datasets listed under Others. Verified results include:

CRM: 499 rows; 315 Won and 184 Lost.

Marketing: 2,240 rows; 609 responses; 27.19% overall response rate.

Support: 3,000 tickets; 59 missing resolution timestamps.

Highest source-level campaign-channel response rate: Direct Mail at 53.0%, based on 79 responses from 149 leads.

These are source-level verification results. They should not automatically be treated as raw row counts from the modified unified dashboard dataset.

Dashboard Summary Context
The generated dashboard summaries may report a different population from the source-level Q results because the dashboard uses a transformed, joined dataset. For example:

The Sales Pipeline summary reports 496 displayed opportunities, while the original CRM source contains 499 rows.

The Customer Health summary reports approximately 2.79K displayed tickets, while the original Support source contains 3,000 tickets.

The Customer Health summary reports 58 unresolved tickets, while the source-level check reports 59 missing resolution timestamps.

The Marketing summary reports a 27.1% overall response rate, consistent with the more precise source-level value of 27.19% after rounding.

These differences are documented rather than silently overwritten. They should be reconciled before using the dashboard for final financial reporting.

Topic and Q Analysis
A Topic named NovaTech Revenue Intelligence Topic was created, configured, and published using NovaTech Unified Revenue. Custom instructions define the shared account key, distinct identifiers, deal outcomes, response-rate logic, unresolved-ticket logic, one-to-many join limitations, and campaign ROI caution.

The Topic could not be linked to the existing analysis because of managed workspace ownership and unavailable contributor permissions. However, the Topic was opened directly in Q/Quick Chat and could be selected as a data source for independent testing.

Baseline Q results and direct Topic Q results are documented separately. This distinction ensures that source-level answers are not mislabeled as unified-dataset results and that any before-and-after comparison uses the correct dataset context.

Evidence
The repository should include screenshots showing:

The three dashboard sheets.

Baseline Q responses from the original source datasets.

The Topic dataset selection.

Topic custom instructions.

The active published Topic version.

Topic ownership and share settings.

The failed analysis-to-Topic linking state.

Direct post-Topic Q results, if available.

Recommendations
Use the dashboard for recurring weekly metrics and operational meetings. Use Q for exploratory questions, but validate important answers against the source definitions and dashboard measures. Before making budget, revenue, or retention decisions, reconcile source-to-dashboard populations and implement governed account-level and source-grain measures.

Security Note
Do not upload passwords, AWS access keys, private tokens, or other credentials to this repository.
