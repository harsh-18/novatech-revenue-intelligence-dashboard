# NovaTech Q Exploration Log

**Student Name:** Harsh Verma  
**Date:** September 18, 2026

## Scope and Dataset Distinction

The Q screenshots were captured from the original pre-indexed source datasets listed under **Others**. The dashboard was built from the modified `NovaTech Unified Revenue` dataset, which contains joined CRM, Marketing, and Support records plus additional fields.

Accordingly, this log distinguishes between:

- **Source-level Q result:** A result calculated from one original source file.
- **Dashboard comparison:** A result from the modified unified dataset.
- **Match:** Only used when the definitions and data grain are comparable.
- **Partial / Needs caution:** Used when a source result may differ from the joined dashboard because of one-to-many fan-out or different aggregation logic.

## Q Exploration Questions

| # | Question Asked | Q's Answer (source-level) | Dashboard Visual Used to Cross-Check | Dashboard Comparison | Match? | Notes |
|---|---|---|---|---|---|---|
| 1 | Which campaign channel has the highest conversion rate? | Direct Mail: 53.0%, with 79 responses out of 149 leads. | Marketing Funnel — Funnel Stage by Channel | The dashboard uses the same Marketing channel and funnel fields; compare using distinct `lead_id` and the same response definition. | Yes, subject to grain check | Q used the original Marketing source. The dashboard result is comparable only if joined rows are deduplicated. |
| 2 | What is the average deal size by company size? | Enterprise: $1,589.17; Small: $1,486.49; Medium: $1,353.30; Large: $1,257.47. | Sales Pipeline — Revenue by Company Size | The dashboard provides the corresponding company-size deal-value comparison. | Partial / Needs caution | Q used original CRM rows. The unified dataset can repeat CRM deals after joining Marketing and Support, so average deal value must be calculated at the opportunity grain. |
| 3 | What is the average resolution time for critical vs. low-priority tickets? | Critical: 1.79 days across 50 tickets; Low: 1.97 days across 1,500 tickets. | Customer Health — Resolution Time by Priority | The dashboard provides resolution time by priority. | Partial / Needs caution | Q used original Support rows. Use distinct `ticket_id` and exclude unresolved tickets from average resolution time. |
| 4 | What are the top 10 accounts by support ticket volume, and what is their total deal revenue? | Q returned an account table. The leading visible result was ACCT-041 with 334 tickets and $40,722 displayed total deal revenue. Other visible results included ACCT-035 with 180 tickets and $16,747, and ACCT-076 with 176 tickets and $16,275. | Customer Health — At-Risk Accounts table | The dashboard supports account-level support-volume and deal-value analysis. | Partial / Needs caution | This question combines sources. The displayed revenue may be affected by one-to-many fan-out; validate ticket counts and deal revenue separately by account. Only the visible rows are reported. |
| 5 | Are there any campaigns where we spent more than we earned back? | 2,207 of 2,240 source Marketing records, approximately 98.5%, had spending above attributed revenue. Only 33 records met or exceeded spend. Partner Referral and Paid Social were highlighted as major overspend contributors. | Marketing Funnel — Spend vs. Revenue combo chart | The dashboard shows spend-versus-attributed-revenue behavior. | Partial / Needs caution | Q used source-level record comparisons. Treat this as a diagnostic signal, not final campaign ROI, because joined records may duplicate spend and attributed revenue. |

## Source-Level Evidence

The same source-level Q session also confirmed:

- CRM: 499 rows; 315 Won and 184 Lost.
- Marketing: 2,240 rows; 609 responses; 27.19% overall response rate.
- Support: 3,000 tickets; 59 missing resolution timestamps.

These facts belong in the Data Verification Log. They are not automatically measures of the joined dashboard dataset.

## Reflection

### Where did Q agree with the dashboard?

Q agreed with the comparable dashboard views for channel performance, company-size deal values, priority-level resolution time, and spend-versus-attributed-revenue behavior. The source counts also matched the data dictionary and source-level QuickSight results.

### Where did Q disagree or struggle?

No direct numerical disagreement was visible in the supplied screenshots. The main issue is that Q and the dashboard did not necessarily use the same dataset grain. Q answered from original source files, while the dashboard uses a modified unified dataset with joined rows and added fields. Cross-source results therefore require distinct-identifier controls before being called an exact match.

### When would you use Q versus the dashboard?

Use the dashboard for recurring weekly metrics because its visuals, filters, and calculations are repeatable. Use Q for exploratory questions against a clearly identified source or Topic. For questions combining CRM, Marketing, and Support, validate the answer at the source grain or account grain before making revenue, budget, or retention decisions.

## Final Limitation Statement

The Q screenshots were intentionally retained rather than re-created against the modified unified dataset. Re-uploading was not required because the assignment’s source-verification questions concern the original source files. The report and logs identify the dataset distinction so that source-level Q answers are not misrepresented as raw counts from the unified joined dataset.
