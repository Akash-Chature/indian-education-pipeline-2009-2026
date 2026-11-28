# Household Education Demand, Costs & Attrition — NSSO 71st Round (2014)

**Directory:** `data_aggregated/1A_4_NSSO71_Demand/`  
**Temporal Design:** Cross-Sectional Benchmark — National Sample Survey Office (NSSO) 71st Round, Schedule 25.2 (*Social Consumption: Education*, January–June `2014`)  
**Underlying Sample:** `65,926` households (`36,479` Rural; `29,447` Urban), `310,827` individuals, and `93,513` currently attending students across 36 States/Union Territories and 639 districts.  
**Column Definitions & Formulas:** See [`1A4_DATA_DICTIONARY.md`](1A4_DATA_DICTIONARY.md) (human-readable) or [`1A4_DATA_DICTIONARY.csv`](1A4_DATA_DICTIONARY.csv) (machine-readable).

---

## File Manifest (12 Aggregated Datasets)

| # | Filename | Analytical Scope |
| :---: | :--- | :--- |
| 1 | `1A4_oop_expenditure_and_wallet_burden.csv` | Five-item annual Out-of-Pocket (OOP) private education expenditure (course fees, books and stationery, transport, private coaching, and other expenses), household education wallet burden as a share of annual consumption, and positive out-of-pocket incidence among students reporting free government education. |
| 2 | `1A4_mdm_scholarships_and_entitlement_targeting.csv` | Elementary Mid-Day Meal (PM-POSHAN) coverage, free textbook and stationery receipt, post-Class 8 government enrolment shifts among lower-consumption quintiles, scholarship incidence, and fee waivers. |
| 3 | `1A4_shadow_coaching_economics.csv` | Paid private coaching incidence across levels, zones, and states, conditional and unconditional annual coaching expenditure, coaching share of total education spending, and household-reported coaching purposes. |
| 4 | `1A4_govt_to_private_exodus_and_motives.csv` | Institution-switching matrix quantifying Government-to-Private versus Private-to-Government transitions alongside household reasons for choosing private institutions. |
| 5 | `1A4_block7_attrition_dropout_taxonomy_and_repeaters.csv` | Block 7 taxonomy of never-enrolled and ever-enrolled dropout reasons disaggregated by gender, sector, and grade band (including Grades 8–10 attrition), plus grade repetition rates. |
| 6 | `1A4_distance_decay_and_prejio_digital_divide.csv` | Adolescent (ages 14–17) attendance rates by physical distance to the nearest secondary school (`< 1 km` through `>= 5 km`) alongside baseline 2014 household computer ownership, internet access, and digital literacy. |
| 7 | `1A4_gar_nar_distance_and_vocational_gender_sorting.csv` | Gross Attendance Ratios (GAR), Net Attendance Ratios (NAR), full-time versus distance-learning enrolment shares, and gender and consumption-quintile sorting across Vocational/Industrial Training Institute (ITI), Engineering, Medicine, and Education (B.Ed.) tracks. |
| 8 | `1A4_within_household_brother_sister_rupee_bias.csv` | Within-household fixed-effects estimates comparing annual education expenditure, private unaided enrolment, coaching, and English-medium enrolment between sons and daughters inside the same mixed-gender households. |
| 9 | `1A4_hidden_variables_unrecognized_english_mobility.csv` | Incidence and pricing of unrecognized private schools, English-medium versus home-language vernacular fee differentials, student commute modes, and inter-state hostel migration. |
| 10 | `1A4_state_36_domain_diagnostic_scorecard.csv` | Composite demand-stress scorecard (`composite_demand_stress_z`) and primary failure-domain classification across 36 States/Union Territories, six Administrative Zones, and five State Typologies. |
| 11 | `1A4_nss_region_and_district_diagnostic_scorecard.csv` | Sub-state demand diagnostics across 89 National Sample Survey (NSS) Regions, 639 districts (`402` meeting the `n >= 30` ranking threshold), and the 2014 baseline profile of the 112 Aspirational Districts Programme (ADP) districts. |
| 12 | `1A4_qa_benchmark_and_suppression_log.csv` | Ten-point verification log documenting exact parity with published Ministry of Statistics & Programme Implementation (MoSPI) Report No. 575 benchmarks, schema notes, and thin-cell suppression counts. |

---

## Methodological & Usage Notes

* **Cross-Sectional Interpretation:** All tables represent the January–June `2014` cross-section (marking the early Right to Education Act benchmark) and should not be treated as a multi-year time series.
* **Survey Weighting & Quintiles:** Every mean, percentage, and ratio is computed strictly using official MoSPI household/person multipliers (`wgt_combined`). Usual Monthly Per Capita Expenditure (UMPCE) quintiles (`Q1` poorest to `Q5` richest) are constructed separately within Rural and Urban India so each quintile captures exactly `20.0%` of its sector population.
* **Raw-Data Field Safeguards:** (1) In Schedule 25.2 Block 6, `hostel_exp` stores a destination State/Union Territory code (`1–35`) rather than rupee expenditure; it is excluded from rupee wallet totals and used exclusively to measure inter-state student migration. (2) `nature_inst` (Recognized vs. Unrecognized) was canvassed strictly for General Private Unaided students; both all-student and General Private Unaided denominators are reported in `1A4_hidden_variables_unrecognized_english_mobility.csv`.
* **Thin-Cell Reliability (`low_n_flag`):** Cells with fewer than `30` unweighted observations are flagged (`low_n_flag = 1`). In district scorecards, domain metrics with `n < 30` are masked to blank (`NULL`) and excluded from composite stress rankings (`402` of `639` districts qualify).
* **Multi-Section Table Filtering:** Always filter multi-part tables on `section`, `slice_type`, or `geo_level` before summing or charting to prevent double-counting across stacked sub-tables.

---

## Data Verification

All 12 CSV files match published MoSPI NSS Report No. 575 headline totals (including General education mean OOP of `₹6,788`, Technical/Professional `₹62,841`, Vocational `₹27,676`, and Age 7+ literacy of `75.80%`), contain zero duplicate rows, and are fully documented in `1A4_DATA_DICTIONARY.md` and `1A4_DATA_DICTIONARY.csv`.
