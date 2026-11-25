# Hospital-Operations-Cost-Patient-Flow-Intelligence

Healthcare Encounter Analysis (Power BI)
An operational and financial analysis of a Synthea-generated healthcare dataset, delivered as a two-page Power BI dashboard.
1. Project Overview
This project examines how encounter volume, cost and payer coverage are distributed across encounter classes, encounter descriptions, payers and procedures, and where the financial burden is concentrated. The intended users are hospital operations and finance managers. The findings can support decisions on where to focus resources, which encounter types or procedures need a cost review, and which payers or patients need attention because of low or zero coverage.
2. Business Questions
• How have encounter volumes changed over time?
• Which encounter classes account for the largest proportion of encounters?
• What proportion of encounters lasted more than 24 hours, where valid timestamps are available?
• How do total claim costs vary across encounter classes?
• How much payer coverage is recorded, and how many encounters have zero recorded coverage?
• Which procedures occur most frequently, and which have the highest average base cost?
• How do average total claim costs vary by payer?
3. Dataset
The data is a Synthea-type synthetic healthcare dataset. Five tables were provided:
• Encounters: one row per patient encounter, with start and stop time, class, description, payer, base cost, total claim cost and payer coverage.
• Payers: insurance payer details.
• Patients: patient demographics.
• Organizations: hospital details.
• Procedures: procedures performed during encounters, with base cost.
Tables not provided: conditions, medications, observations, providers, allergies and payer transitions. As a result, the analysis is limited to operational and financial questions. It cannot cover clinical outcomes, diagnosis-level burden, doctor-level analysis or changes in a patient's coverage over time.
4. Tools
• Power BI Desktop for data cleaning (Power Query), modelling, DAX measures and dashboards.
5. Data Cleaning Steps
1.  Renamed the Id columns to match their tables, for example organization_id and payer_id, so keys are easy to tell apart in joins.
2.  Dropped columns that were not relevant to the analysis, such as Prefix, Suffix and Maiden.
3.  Converted the start and stop columns to a proper date-time type.
4.  Added DURATION (HOURS), which is the encounter end minus the encounter start, in hours. It is blank when either timestamp is missing or the end is earlier than the start.
5.  Added OVER 24HOURS, which is Yes if the duration is greater than 24 hours, No if it is 24 hours or less, and blank if the duration is blank.
6.  Kept blank payer coverage separate from zero coverage, and kept encounters with no payer as their own group.
6. Data Model
• Encounters is the central table.
• Encounters connect to Payers (by payer), Patients (by patient) and Organizations (by organization), each many-to-one.
• Procedures connect to Encounters (by encounter), many-to-one, so slicers on the Encounters side also filter the procedure visuals.
• A Calendar table connects to the encounter date to support trend analysis.
Cost and coverage measures are calculated from the Encounters table only. They are never summed through the Procedures table, because that would repeat each encounter once per procedure and inflate the totals.
7. KPI Definitions
• Total Encounters: count of rows in Encounters.
• Total Claim Cost: sum of Total_Claim_Cost.
• Cost per Encounter: Total Claim Cost divided by Total Encounters.
• Total Payer Coverage: sum of Payer_Coverage.
• Payer Coverage Rate: Total Payer Coverage divided by Total Claim Cost.
• Patient Out-of-Pocket: Total Claim Cost minus Total Payer Coverage.
• Zero Coverage Encounters: encounters where Payer_Coverage is not blank and equals 0.
• % Zero Coverage: Zero Coverage Encounters divided by Total Encounters.
• % Over 24 Hours: encounters where OVER 24HOURS is Yes, divided by encounters with a valid duration.
The full KPI definition sheet, with DAX for each measure, is provided in the project documentation.
8. Dashboard Structure
Page 1: Overview, trend and class comparison
• KPI cards for the headline measures.
• Encounter volume by year and month.
• Encounter class share and cost comparison.
• Encounters over 24 hours by class.
Page 2: Cost, payer coverage and procedures
• Payer coverage versus patient out-of-pocket.
• Zero-coverage encounters by class and payer.
• Average claim cost and coverage rate by payer.
• Most frequent procedures and procedures with the highest average base cost.
Interactivity: slicers for Year, Encounter Class and Payer, synced across both pages, with drill-down on the time axis and cross-filtering between visuals.
9. Key Findings
• The dataset holds 27,891 encounters and about $101M in total claim cost. 2014 and 2021 had significantly more encounters than other years (3,885 and 3,530).
• Ambulatory leads on encounters (12,537) and total claim cost ($36M), followed by Urgent Care ($23M) and Outpatient ($14M).
• Outpatient accounts for 23% of encounters but only 14% of total claim cost, while Urgent Care accounts for 13% of encounters but 23% of cost.
• Inpatient has the fewest encounters but the highest cost per encounter ($8k).
• Urgent Care Clinic produces the highest total claim cost ($23M) among descriptions, and Prenatal Initial Visit has the highest cost per encounter ($42k).
• Medicare leads on total claim cost ($25M), followed by Medicaid ($9M) and Humana ($4M).
• Coverage rates: Medicaid 94%, Dual Eligible 89%, Medicare 78% and Blue Cross Blue Shield 69%. All other payers are below 1%.
• Payers covered $31M of the cost, leaving $70M (about 69%) as patient out-of-pocket. 48.71% of encounters have zero recorded coverage.
10. Recommendations
1.  Investigate the payers with less than 1% coverage first, to find out whether their claims were unpaid, denied or never recorded.
2.  Strengthen eligibility checks at registration and follow up on denied and unpaid claims, with financial counselling for uncovered patients.
3.  Protect the Medicare and Medicaid relationships through clean, timely claims.
4.  Protect and review Urgent Care capacity, since it produces 23% of cost from 13% of encounters.
5.  Review Outpatient efficiency, since it takes 23% of encounters but only 14% of cost.
6.  Plan for inpatient, intensive care and maternity cost, which have the highest costs per encounter.
11. Validation
• Encounters by class add up to the total of 27,891.
• Total claim cost by class matches the sum of total payer coverage and patient out-of-pocket.
• Cost per encounter was recalculated by hand for a sample class and matched the dashboard.
• Zero-coverage and non-zero-coverage encounters add up to 100%.
• Only encounters with valid start and end times were used for the 24-hour measure, and the excluded count was recorded.
• Blank coverage was kept separate from zero coverage.
• Cost and coverage were calculated from the Encounters table only, to avoid double counting.
12. Limitations
• The data is synthetic (Synthea), so the findings show the method and not real hospital performance.
• Total claim cost is the amount billed, not the amount collected.
• Zero coverage can mean self-pay, an uncovered service or a recorded zero, and the data cannot tell these apart.
• Missing tables limit the analysis to operational and financial questions, and the recommendations are directional.
13. How to Use This Project
1.  Open the Power BI file (.pbix) in Power BI Desktop.
2.  If the data source paths have changed, update them under Transform data, then Data source settings.
3.  Refresh the data.
4.  Use the slicers on each page to filter by year, encounter class and payer.
14. Repository Structure
Adjust the names below to match your files.
• README.md: this file.
• healthcare_dashboard.pbix: the Power BI report.
• data/: the five source CSV files (encounters, payers, patients, organizations, procedures).
• docs/: data-quality assessment, KPI definition sheet and executive summary.
