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
│   ├── Customer Health Dashboard.pdf
│   ├── Marketing Funnel Dashboard.pdf
│   ├── Sales Pipeline Dashboard.pdf
│   ├── NovaTech_dashboard_export.pdf
│   ├── NovaTech_dashboard_annotated.pdf
│   ├── NovaTech_dashboard_executive_summary.md
│   ├── NovaTech_report_to_Sarah_Chen.md
│   ├── NovaTech_data_verification_log_final.md
│   └── NovaTech_Q_exploration_log_final.md
├── screenshots/
│   ├── 01_datasets_spice/
│   ├── 02_data_preparation/
│   ├── 03_dashboard_sheets/
│   ├── 04_dashboard_interactivity/
│   ├── 05_dashboard_annotations/
│   ├── 06_before_topic_q/
│   ├── 07_topic_setup/
│   ├── 08_after_topic_q/
│   └── 09_submission_evidence/
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

## Evidence Directory Structure

All screenshot proofs are organized into numbered directories where **the filename prefix strictly matches the folder prefix**:

```text
screenshots/
├── 01_datasets_spice/
│   ├── 01_crm_deals_spice_import_499_rows.png
│   ├── 01_marketing_campaigns_spice_import_2240_rows.png
│   ├── 01_support_tickets_spice_import_3000_rows.png
│   ├── 01_support_tickets_spice_import_details.png
│   ├── 01_unified_revenue_dataset_summary.png
│   ├── 01_unified_revenue_spice_63420_rows_imported.png
│   ├── 01_unified_revenue_spice_dataset_summary.png
│   ├── 01_unified_revenue_spice_import_63420_rows.png
│   ├── 01_unified_revenue_spice_join_structure.png
│   └── 01_unified_revenue_spice_source_datasets.png
├── 02_data_preparation/
│   ├── 02_calculated_column_days_to_close_formula.png
│   ├── 02_calculated_columns_crm_editor.png
│   ├── 02_calculated_field_campaign_roi_formula.png
│   ├── 02_calculated_field_response_rate_formula.png
│   ├── 02_crm_data_prep_editor_view.png
│   ├── 02_crm_data_type_correction_preview.png
│   ├── 02_crm_data_type_correction_step.png
│   ├── 02_marketing_12_columns_datatype_changed.png
│   ├── 02_marketing_data_prep_editor_view.png
│   ├── 02_marketing_datatype_change_preview.png
│   ├── 02_support_9_columns_datatype_changed.png
│   ├── 02_support_data_prep_editor_view.png
│   ├── 02_support_datatype_transformation_steps.png
│   ├── 02_unified_join1_account_id_preview.png
│   ├── 02_unified_join1_crm_anchor_marketing_config.png
│   ├── 02_unified_join1_revenue_webvisits_preview.png
│   ├── 02_unified_join2_sentiment_downtime_preview.png
│   └── 02_unified_join2_support_tickets_config.png
├── 03_dashboard_sheets/
│   ├── 03_customer_health_monthly_ticket_trends.png
│   ├── 03_customer_health_sheet_kpis_top.png
│   ├── 03_marketing_funnel_revenue_by_channel.png
│   ├── 03_marketing_funnel_sheet_kpis_top.png
│   ├── 03_marketing_funnel_spend_vs_revenue.png
│   ├── 03_sales_pipeline_opportunity_count_stage.png
│   └── 03_sales_pipeline_sheet_kpis_top.png
├── 04_dashboard_interactivity/
│   ├── 04_customer_health_unified_visual.png
│   ├── 04_deal_stage_filter_action_before.png
│   ├── 04_deal_stage_filter_action_lost_selected.png
│   ├── 04_marketing_filter_all_27_1_percent.png
│   ├── 04_marketing_filter_direct_mail_50_7_percent.png
│   ├── 04_marketing_filter_email_9_4_percent.png
│   ├── 04_navigation_action_marketing_source.png
│   ├── 04_navigation_action_sales_pipeline_destination.png
│   ├── 04_sales_region_filter_all_496_opportunities.png
│   └── 04_sales_region_filter_east_128_opportunities.png
├── 05_dashboard_annotations/
│   ├── 05_customer_health_annotations.png
│   ├── 05_marketing_funnel_annotations.png
│   └── 05_sales_pipeline_annotations.png
├── 06_before_topic_q/
│   ├── 06_baseline_q1_crm_deals_won.png
│   ├── 06_baseline_q2_marketing_response_rate.png
│   └── 06_baseline_q3_unresolved_support_tickets.png
├── 07_topic_setup/
│   ├── 07_quicksight_home_before_topic.png
│   ├── 07_topic_active_published_version_2.png
│   ├── 07_topic_created_version_1.png
│   ├── 07_topic_dataset_unified_revenue.png
│   ├── 07_topic_overview_description.png
│   ├── 07_topic_owner_permissions_share.png
│   ├── 07_topic_semantic_layer_campaign_channel_description_synonyms.png
│   ├── 07_topic_semantic_layer_deal_value_description_synonyms.png
│   └── 07_topic_semantic_layer_ticket_id_description_synonyms.png
├── 08_after_topic_q/
│   ├── 08_post_topic_q1_crm_deals_won.png
│   ├── 08_post_topic_q2_marketing_response_rate.png
│   └── 08_post_topic_q3_unresolved_support_tickets.png
└── 09_submission_evidence/
    ├── 09_analysis_manage_qa_dialog_limitation.png
    ├── 09_analysis_manage_qa_settings.png
    ├── 09_analysis_topic_linkage_failed_ownership.png
    ├── 09_verification_q1_crm_deals_rows_499.png
    ├── 09_verification_q1_crm_deals_rows_499_check1.png
    ├── 09_verification_q2_crm_won_315_lost_184_table.png
    ├── 09_verification_q2_crm_won_315_lost_184_summary.png
    ├── 09_verification_q2_crm_won_315_lost_184_check1.png
    ├── 09_verification_q3_marketing_rows_2240.png
    ├── 09_verification_q3_marketing_rows_2240_check1.png
    ├── 09_verification_q4_marketing_response_rate_27_19pct.png
    ├── 09_verification_q5_support_tickets_3000.png
    ├── 09_verification_q5_support_tickets_3000_check1.png
    ├── 09_verification_q6_support_unresolved_59.png
    ├── 09_exploration_q1_campaign_channel_highest_conversion.png
    ├── 09_exploration_q1_campaign_channel_conversion_table.png
    ├── 09_exploration_q2_average_deal_size_by_company_size.png
    ├── 09_exploration_q2_deal_size_company_size_takeaways.png
    ├── 09_exploration_q3_average_resolution_time_priority.png
    ├── 09_exploration_q4_top_10_accounts_support_vs_revenue.png
    ├── 09_exploration_q4_top_10_accounts_takeaways.png
    ├── 09_exploration_q5_campaign_spend_exceeded_revenue.png
    ├── 09_exploration_q5_campaign_spend_worst_offenders.png
    ├── 09_exploration_q5_campaign_spend_key_takeaways.png
    └── 09_exploration_crm_deals_won_315_followup.png
```

## Rubric Traceability and Verification Summary

1. **Verify data quality and completeness across indexed knowledge bases (Passed)**
   - `deliverables/NovaTech_data_verification_log_final.md` records 7 checkable fact tests across CRM, Marketing, and Support.
   - `screenshots/01_datasets_spice/` demonstrates 100% SPICE import for all source datasets with matching row and column counts.
   - `screenshots/09_submission_evidence/` contains individual verification query screenshots.

2. **Transform and join datasets using no-code data preparation tools (Fully Met)**
   - **All four datasets in SPICE**: Proved by `01_crm_deals_spice_import_499_rows.png`, `01_marketing_campaigns_spice_import_2240_rows.png`, `01_support_tickets_spice_import_3000_rows.png`, and `01_unified_revenue_spice_import_63420_rows.png` (with detailed join structure and source dataset summaries).
   - **Data type corrections**: Proved by `02_crm_data_type_correction_step.png`, `02_marketing_12_columns_datatype_changed.png`, and `02_support_9_columns_datatype_changed.png`.
   - **Calculated fields**: Proved by `02_calculated_column_days_to_close_formula.png` (`dateDiff`), `02_calculated_field_campaign_roi_formula.png`, and `02_calculated_field_response_rate_formula.png`.
   - **Unified dataset joins**: Proved by `02_unified_join1_crm_anchor_marketing_config.png` and `02_unified_join2_support_tickets_config.png`, confirming CRM as the anchor table with left joins on `account_id`.

3. **Dashboard Design & Interactivity (Fully Met)**
   - **Multi-page dashboard**: Proved by `03_marketing_funnel_sheet_kpis_top.png`, `03_sales_pipeline_sheet_kpis_top.png`, `03_customer_health_sheet_kpis_top.png`, and `NovaTech_dashboard_export.pdf`.
   - **Customer Health Unified Visual**: Proved by `04_customer_health_unified_visual.png`, demonstrating the "At-Risk Accounts — Support Volume and Deal Revenue" table built from the unified dataset combining CRM revenue with Support tickets.
   - **Interactive Filter Controls**: Proved by `04_marketing_filter_all_27_1_percent.png`, `04_marketing_filter_direct_mail_50_7_percent.png`, `04_marketing_filter_email_9_4_percent.png`, and `04_sales_region_filter_all_496_opportunities.png`, `04_sales_region_filter_east_128_opportunities.png`.
   - **One-Click Filtering Action**: Proved by `04_deal_stage_filter_action_before.png` and `04_deal_stage_filter_action_lost_selected.png`, demonstrating one-click filtering on the Opportunity Count by Deal Stage chart.
   - **Cross-Sheet Navigation**: Proved by `04_navigation_action_marketing_source.png` and `04_navigation_action_sales_pipeline_destination.png`, linking from Marketing Funnel to Sales Pipeline.

4. **AI-Powered Natural Language Querying (Fully Met)**
   - **Baseline Quick Chat**: Proved in `06_before_topic_q/`.
   - **Topic Setup & Semantic Layer**: Proved in `07_topic_setup/` with `07_topic_semantic_layer_campaign_channel_description_synonyms.png`, `07_topic_semantic_layer_deal_value_description_synonyms.png`, and `07_topic_semantic_layer_ticket_id_description_synonyms.png` showing field descriptions, semantic roles, and synonyms.
   - **Post-Topic Quick Chat**: Proved in `08_after_topic_q/`.
   - **Q Exploration Log**: Fully documented in `deliverables/NovaTech_Q_exploration_log_final.md` and `09_submission_evidence/`.

5. **Data-Driven Business Insights & Annotations (Fully Met)**
   - **Visible text annotations on sheets**: Proved by `05_customer_health_annotations.png`, `05_marketing_funnel_annotations.png`, and `05_sales_pipeline_annotations.png`, and exported in `deliverables/NovaTech_dashboard_annotated.pdf`.
   - **Three-part structure**: Every annotation adheres strictly to (1) Quantified Finding, (2) Business Implication, and (3) Recommended Action.
   - **Executive Report**: `deliverables/NovaTech_report_to_Sarah_Chen.md` and `deliverables/NovaTech_dashboard_executive_summary.md`.

## Recommendations

Use the dashboard for recurring weekly metrics and operational meetings. Use Q for exploratory questions, but validate important answers against the source definitions and dashboard measures before making final business decisions or communicating financial impacts.

## Security Note

Do not upload passwords, AWS access keys, private tokens, or other credentials to this repository.
