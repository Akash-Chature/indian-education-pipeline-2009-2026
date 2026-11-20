# Canonical Data Dictionary & Harmonization Standards

This document records the canonical relational schemas, primary keys, harmonization rules, survey weighting formulas, and methodological safeguards governing the six sealed DuckDB + ZSTD Parquet lakehouses used in *Two Decades of the Indian Education Pipeline (2009–2026)*.

> **Governance Notice:** Raw unit-level microdata files (ASER, NSSO, PLFS) and local `.duckdb`/`.parquet` lakehouses are strictly excluded from this public repository in compliance with the formal ASER Centre Research MoU and MoSPI NADA licensing terms. See [`DATA_GOVERNANCE_AND_MOU.md`](../DATA_GOVERNANCE_AND_MOU.md).

---

## 1. Cross-Lakehouse Pre-Flight Standards

All analytical outputs published in `data_aggregated/` enforce five universal rules:

1. **Geographic Symmetry:** Joins with RBI State Finances are restricted to the **31 States/UTs with Legislatures** (the 5 non-legislature UTs + Ladakh are explicitly isolated). All district-level joins are composite-keyed on `(canonical_state_name, canonical_district_name)` to prevent same-name collisions (e.g., *Bijapur* in Chhattisgarh vs. *Vijayapura* in Karnataka; *Bilaspur* in Chhattisgarh vs. Himachal Pradesh; *Raigarh* in Chhattisgarh vs. *Raigad* in Maharashtra).
2. **Zero Intergovernmental Double-Counting:** Consolidated National (Centre + State) public education expenditure nets out Central Grants-in-Aid (`3601` + `3602`) routed into State Treasuries (from `2014-15` onward) to prevent counting the same rupee twice.
3. **Explicit Fiscal Basis & Deflation:** Financial tables retain `data_basis` (`Actual`, `RE`, `BE`) without silent pooling, and pair nominal rupees (Rs. Crores) with real deflated series (MoSPI CPI/GDP deflator, `2011-12` base).
4. **Survey Weighting Integrity:** All NSSO and PLFS population estimates apply official MoSPI multipliers (`final_weight`, `wgt_combined`); unweighted sample counts are never reported as population shares.
5. **Dual Labor Status Reporting:** All PLFS/NSSO labor indicators report both Usual Principal Status (`PS`) and Usual Status (`PS+SS`).

---

## 2. Lakehouse 1: Macro-Financial (Union Budget & RBI State Finances)

* **Coverage:** 10 Waves (`2009-10`, `2011-12`, `2012-13`, `2014-15`, `2018-19`, `2019-20`, `2021-22`, `2023-24`, `2024-25`, `2025-26`) | **327 Certified Rows** (0 duplicate keys, 0 NULL primary keys).

### Table 1.1: `union_budget_master` (20 Rows, 9 Columns)
* **Primary Key:** `(academic_year, department)`
* **Key Columns:** `academic_year`, `department` (`'School Education'`, `'Higher Education'`), `general_ed_2202_cr`, `technical_ed_2203_cr`, `grants_in_aid_states_3601_cr`, `grants_in_aid_uts_3602_cr`, `total_revenue_expenditure_cr`, `total_capital_expenditure_cr`, `data_basis` (`'Actual'` or `'RE'`).
* **Accounting Rules:**
  * **PSK/MUSK Recovery Netting:** Deducts reserve-fund recoveries (*Prarambhik Shiksha Kosh* / *Madhyamik & Uchhatar Shiksha Kosh*) so totals match Net Developmental Head outlays.
  * **2014–15 Routing Break:** Pre-2014 SSA/MDM transfers went directly to societies under `2202`; from `2014-15` onward, Centrally Sponsored Schemes route via State Treasuries under `3601`.
  * **2017 Plan/Non-Plan Merger:** Pre-2017 Net Non-Plan/Plan columns harmonize cleanly with post-2017 Net Revenue/Capital sections.

### Table 1.2: `rbi_state_finance_master` (307 Rows, 6 Columns)
* **Primary Key:** `(academic_year, state_name)` — 30 entities pre-Telangana (`2009-10` to `2012-13`), 31 entities from `2014-15` to `2025-26` (`'NCT of Delhi'` normalized to `'Delhi'`).
* **Key Columns:** `academic_year`, `state_name`, `state_revenue_ed_cr` (Major Head `2202-2205`, Appendix II), `state_capital_ed_cr` (Major Head `4202`, Appendix IV; strictly excludes Loans & Advances `6202`), `total_social_sector_cr` (combined Revenue + Capital Social Services), `data_basis` (`'Actual'`, `'RE'`, `'BE'`).

---

## 3. Lakehouse 2: UDISE+ & DISE School Supply Census

* **Coverage:** `2009-10` to `2025-26` (complete longitudinal pipeline including `2023-24` Profile 2) | **12.1M+ School Records** across waves (`1,471,473` schools in `2024-25`; `1,466,682` schools in `2025-26`).
* **Key Harmonization Rules:**
  * **Dynamic School Key Resolution:** Programmatically reconciles era-specific school keys (`udise_sch_code`, `psuedocode`, `schcd`) with strict `DISTINCT ON` 1:1 deduplication prior to multi-file profile joins to eliminate row explosion.
  * **Core Analytical Domains:** School management categories (Government, Private Unaided, Private Aided), longitudinal net closures/rationalizations, grade-wise enrolment registers, teacher counts & Pupil-Teacher Ratios (PTR), school grants, and physical/safety infrastructure indicators.

---

## 4. Lakehouse 3: ASER Rural Microdata (MoU-Governed)

* **Coverage:** 6 Waves (`2009`, `2012`, `2014`, `2018`, `2022`, `2024`) | **3,501,238 Certified Rows** across 32 rural States/UTs (0 duplicate keys, 0 orphan foreign keys).
* **Governance Boundary:** Raw unit-level keys (`village_primary_key`, `hh_primary_key`, `child_primary_key`, `school_primary_key`) are retained strictly offline; public outputs aggregate exclusively to State, District, or Era summaries.

| Table | Certified Rows | Cols | Grain & Core Variables |
| :--- | :---: | :---: | :--- |
| `aser_village_base` | `104,083` | 10 | Sampled rural villages: `survey_year`, `state_name`, `district_name`, pucca road, electrification, primary & secondary school proximity. |
| `aser_child_cognitive_base` | `3,299,037` | 18 | Children aged `5–16`: `age`, `gender`, `school_status` (`Govt`, `Private`, `Other`, `Not Enrolled`), `current_class`, `takes_private_tuition`, `reading_level` (`0–4`), `math_level` (`0–4`), `english_level` (`0–4`), `applied_arithmetic_bonus` (`2018`). |
| `aser_school_observation_base` | `98,118` | 16 | Unannounced visits to rural Govt schools: `school_type` (`Primary`, `Upper Primary` — `2009` code inversion resolved), `total_enrolled`, `total_present_on_visit`, teacher attendance, usable drinking water, usable boys'/girls' toilets, Mid-Day Meal served. |

---

## 5. Lakehouse 4: NSSO 71st Round (Schedule 25.2 — Education, 2014)

* **Coverage:** Jan–Jun 2014 Cross-Section across 36 States/UTs and 639 Census-2011 district codes | **470,266 Certified Rows** across 3 decoded tables (48 survey variables harmonized to human-readable labels).
* **Weighting Formula:** Weighted national/state indicators use `wgt_combined` (with exact integer `mlt` reconstruction applied to scientific-notation truncations).

| Table | Certified Rows | Primary Key | Core Analytical Variables |
| :--- | :---: | :--- | :--- |
| `nsso71_household_base` | `65,926` | `HH_ID` | `canonical_state_name`, `canonical_district_name`, `sector`, `hhsize`, `social_grp`, `religion`, `dist_primary_school`, `dist_upper_primary_school`, `dist_secondary_school`, `has_computer`, `has_internet_member_14plus`, `umpce`, `wgt_combined`. |
| `nsso71_person_base` | `310,827` | `(HH_ID, psrl_no)` | Demographics (`age`, `sex`), `gen_edu_code` (`1–16`), 9-tier `canonical_edu_level`, `is_literate`, IT skills, attendance/enrolment status, and Block 7 dropout/non-enrolment reasons (`b7_never_enrol_reason`, `51,657` rows). |
| `nsso71_student_course_exp` | `93,513` | `(HH_ID, psrl_no)` | `canonical_attend_level`, `course_category` (`General`, `Technical/Professional`, `Vocational`), `inst_type`, `reason_private`, `changed_edu_inst` (`Govt → Private`, etc.), `private_coaching`, itemized annual spend (`course_fee`, `books`, `transport`, `priv_coaching`, `other_exp`, `tot_exp`, `hostel_exp`). |

---

## 6. Lakehouse 5: AISHE Higher Education Census

* **Coverage:** `2011-12` to `2023-24` | **8 Canonical Master Tables** engineered for strict year-on-year `UNION ALL` compatibility.
* **Mandatory Double-Counting Safeguards:** Every table embeds `response_type` (`'Actual'`, `'Estimated'`, `'Actual+Estimated'`, `'Universe'`, `'Projected'`), `geo_level` (`'National'`, `'State'`, `'District'`), and `source_table` to isolate blended totals (`T-6`) from component tables and quarantine multi-year historical series tables (`T-40` to `T-52`).

| Canonical Table | Role & Core Dimensions / Metrics |
| :--- | :--- |
| `dim_institution` | University/College/Standalone counts by `management_type`, `specialisation`, rural flag, women-exclusive flag, enrolment size band. |
| `dim_population` | Projected `18–23` age-group population by `gender` and `social_category` (GER denominator). |
| `fact_enrolment` | Student enrolment by `inst_type`, `management_type`, `programme_level`, `programme_name`, `discipline`, `mode_of_study` (`Regular`, `Distance`), `gender`, `social_category`. |
| `fact_outturn` | Annual graduate out-turn across levels, disciplines, modes, and demographics. |
| `fact_staff` | Teaching and Non-Teaching faculty counts by `post_level`, `gender`, and `social_category`. |
| `fact_foreign_students` | International student inflows by `country_of_origin` and `programme_level`. |
| `fact_infrastructure` | Hostel intake capacity and residing counts by `facility_type` and `gender`. |
| `fact_macro_indicators` | Published `GER`, `GPI`, `PTR`, and `Colleges per Lakh` ratios by `mode_of_study` and demographic group. |

---

## 7. Lakehouse 6: PLFS & NSSO Employment–Unemployment Microdata

* **Coverage:** 6 Certified Waves — NSSO 66th (`2009-10`), NSSO 68th (`2011-12`), PLFS (`2018-19`, `2021-22`, `2023-24`, `2025` Calendar Year) | **4,111,292 Certified Rows** (`778,434` households + `3,332,858` persons; 0 duplicate keys, 0 orphan rows).
* **Visit-1 Quarantine:** PLFS waves ingest exclusively First Visit schedules (`HHV1`/`PERV1`) to prevent 4x urban panel revisit inflation.

### Table 6.1: `plfs_household_base` (778,434 Rows, 13 Columns)
* **Columns:** `survey_year`, `survey_type`, `hh_primary_key`, `state_code_raw`, `canonical_state_name`, `district_code_raw`, `canonical_district_name` (612 to 764 decoded districts across waves; 0 NULLs), `sector`, `social_group`, `religion`, `hh_size`, `usual_consumer_expenditure`, `final_weight`.

### Table 6.2: `plfs_person_base` (3,332,858 Rows, 18 Columns)
* **Columns:** `survey_year`, `survey_type`, `person_primary_key`, `hh_primary_key`, `age`, `gender`, `general_edu_level_raw`, `canonical_edu_level` (9-tier harmonized bridge to UDISE+/AISHE: `Illiterate`, `Below Primary`, `Primary`, `Upper Primary`, `Secondary`, `Higher Secondary`, `Diploma`, `Graduate`, `Post-Graduate`), `technical_edu_level`, `current_attendance_status`, `vocational_training_status`, `principal_activity_status`, `subsidiary_activity_status`, `nco_code_raw`, `nco_version` (`NCO-2004`/`NCO-2015`), `nic_code_raw`, `nic_version` (`NIC-2004`/`NIC-2008`), `final_weight`.
* **Multiplier Formulas (`final_weight`):**
  * **NSSO (`2009-10`, `2011-12`):** `MLT / 100.0` if `NSS == NSC`, else `MLT / 200.0`.
  * **PLFS Annual (`2018-19` to `2023-24`):** `MULT / (100.0 * No_Qtr)` if `NSS == NSC`, else `MULT / (200.0 * No_Qtr)`.
  * **PLFS Calendar Year (`2025`):** `MULT / 100.0` (annualized combined multiplier).
* **Status Definitions:** `PS` uses `principal_activity_status`; `PS+SS` classifies a respondent as employed if working in either `principal_activity_status` or `subsidiary_activity_status`, capturing unpaid family helpers and working students (`12,742` sample students aged 15+ reporting subsidiary work across the 6 waves).
