# Stage 1A.4 — NSSO 71st Round Demand-Side Audit (Jan–Jun 2014 Cross-Section)

**Audit Status:** Certified PASS (`12 / 12` CSVs Verified — `0` IEEE-754 float noise, `0` exact duplicate rows; Nov 26, 2026)  
**Temporal Design:** **RTE-Era Endpoint Cross-Section** (`Schedule 25.2: Social Consumption — Education`, Jan–Jun `2014`; `470,266` records: `65,926` households, `310,827` persons, `93,513` students across `36` States/UTs & `639` districts).  
**Column Definitions & Formulas:** See [`1A4_DATA_DICTIONARY.md`](1A4_DATA_DICTIONARY.md) (human-readable) or [`1A4_DATA_DICTIONARY.csv`](1A4_DATA_DICTIONARY.csv) (machine-readable).

## File Manifest (`12` Certified CSVs)

| # | Filename | Analytical Focus |
| :---: | :--- | :--- |
| 1 | `1A4_oop_expenditure_and_wallet_burden.csv` | 5-item annual out-of-pocket expenditure (`course_fee`, `books`, `transport`, `priv_coaching`, `other_exp`), household wallet burden (`17.39%` overall; `3.55×` Private Unaided share jump), and `99.87%` positive OOP among "free" Govt students. |
| 2 | `1A4_mdm_scholarships_and_entitlement_targeting.csv` | Elementary PM-POSHAN (`MDM`) targeting (`85.09%`), free textbooks/stationery, Class 8 MDM cliff (`-19.80 pp` Rural `Q1–Q2` Govt share drop), scholarships (`20.14%`), and fee waivers. |
| 3 | `1A4_shadow_coaching_economics.csv` | Paid coaching incidence (`25.92%`, `56.22%` East Zone), conditional/unconditional spend, wallet share, and coaching purpose (`89.17%` *"Augmenting basic education"*). |
| 4 | `1A4_govt_to_private_exodus_and_motives.csv` | Institution-switching matrix (`7.51×` national `Govt→Pvt` net exodus; `17.52×` Central Zone) and household reasons for choosing private institutions. |
| 5 | `1A4_block7_attrition_dropout_taxonomy_and_repeaters.csv` | Block 7 never-enrolled & dropout reason taxonomy (gendered Grades `8–10` split: Economic/Financial for boys vs. Domestic/Marriage for girls) and repeater rates. |
| 6 | `1A4_distance_decay_and_prejio_digital_divide.csv` | Secondary school distance decay (`<1 km` to `≥5 km`: `-12.81 pp` Rural Female `14–17` attendance) and 2014 Pre-Jio household computer/internet access & IT skills. |
| 7 | `1A4_gar_nar_distance_and_vocational_gender_sorting.csv` | Gross & Net Attendance Ratios (`GAR`/`NAR`), full-time vs. distance modes, and gender/wealth sorting across Vocational/ITI (`82.80%` Male), Engineering (`74.87%` Male), Medicine (`64.22%` Female), and B.Ed. (`60.20%` Female). |
| 8 | `1A4_within_household_brother_sister_rupee_bias.csv` | Within-household (`HH_ID`) fixed-effects estimator comparing sons vs. daughters inside the exact same homes (`+14.79%` rupee bias in same-stage `C1–12` homes; `Bihar +38.75%`, `Rajasthan +36.41%`). |
| 9 | `1A4_hidden_variables_unrecognized_english_mobility.csv` | Unrecognized private schools (`11.33%` of Gen Pvt Unaided; `30.47%` East), the `6.30×` English-medium fee premium (`+₹20,817`), commute modes, and inter-state hostel student migration. |
| 10 | `1A4_state_36_domain_diagnostic_scorecard.csv` | 36-State, 6-Zone, and 5-Typology composite demand-stress scorecard (`composite_demand_stress_z`) and primary failure domain classification. |
| 11 | `1A4_nss_region_and_district_diagnostic_scorecard.csv` | 89-NSS-Region, 639-District (`402` qualifying at `n >= 30`), and 2014 Pre-ADP (`112` Aspirational Districts) baseline scorecard (`7/10` top-stress districts outside ADP). |
| 12 | `1A4_qa_benchmark_and_suppression_log.csv` | 10-check MoSPI parity (`General ₹6,788`, `Tech/Prof ₹62,841`, `Vocational ₹27,676`; `75.80%` Age 7+ literacy), schema audit, and `low_n_flag` verification log. |

## Methodological Disclosures
* **Survey Weighting:** Every mean, share, and sector-specific UMPCE quintile (`Q1`–`Q5`) is weighted strictly by `wgt_combined`.
* **Critical Raw-Data Safeguards:** (1) `hostel_exp` (`values 1–35`) is a **Hostel Destination State/UT code**, not rupees, and is excluded from rupee wallet totals (used strictly for Inter-State Migration); (2) `nature_inst` (`Recognized`/`Unrecognized`) was canvassed strictly for `General + Private Unaided` students (dual denominators reported).
* **Thin-Cell Gate:** `low_n_flag = 1` whenever `n_unweighted < 30`; `402 / 639` districts qualify for composite stress ranking (`237` thin districts masked per domain).

