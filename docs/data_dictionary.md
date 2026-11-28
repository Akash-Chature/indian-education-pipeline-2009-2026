# Canonical Source-Layer Schema Reference (Six Institutional Datasets)

**Scope:** Documents the harmonized source schemas, primary keys, accounting adjustments, and survey weighting formulas underlying the study's six analytical databases (`2009–2026`).  
**Published Output Tables:** For column-by-column definitions of the aggregated CSV files published in `data_aggregated/`, see [`output_dictionary_index.md`](output_dictionary_index.md) and each subfolder's `*_DATA_DICTIONARY.md`.  
**Privacy Notice:** Raw unit-level microdata files (ASER, NSSO, PLFS) and database binaries (`.duckdb`, `.parquet`) are excluded from this repository under data-governance and MoU rules (see [`../DATA_GOVERNANCE_AND_MOU.md`](../DATA_GOVERNANCE_AND_MOU.md)).

---

## 1. Macro-Financial Schema (Union Budget & RBI State Finances, `FY 2009–10` to `2025–26`)

* **Coverage:** 10 harmonized fiscal waves (`2009–10`, `2011–12`, `2012–13`, `2014–15`, `2018–19`, `2019–20`, `2021–22`, `2023–24`, `2024–25`, `2025–26`), plus four-ministry Statement of Budget Estimates (SBE) scheme ledgers through `2026–27` BE and State Education Loans (`6202`).
* **Core Relational Tables:**
  * **`union_budget_master` (`20` rows, `9` cols | PK: `academic_year, department`):** Tracks Net-of-Recoveries outlays for `School Education` and `Higher Education` across `general_ed_2202_cr`, `technical_ed_2203_cr`, `grants_in_aid_states_3601_cr`, `grants_in_aid_uts_3602_cr`, `total_revenue_expenditure_cr`, `total_capital_expenditure_cr`, and `data_basis` (`Actual` / `RE`).
  * **`rbi_state_finance_master` (`307` rows, `6` cols | PK: `academic_year, state_name`):** Covers 30 entities pre-Telangana (`2009–10` to `2012–13`) and 31 States/UTs with Legislatures (`2014–15` onward), tracking `state_revenue_ed_cr` (`2202–2205`, Appendix II), `state_capital_ed_cr` (`4202`, Appendix IV excluding `6202`), `total_social_sector_cr`, and `data_basis` (`Actual` / `RE` / `BE`).
* **Key Accounting Rules:**
  * **Reserve-Fund Recovery Netting:** Negative `Recoveries` entries for Prarambhik Shiksha Kosh (PSK) and Madhyamik & Uchhatar Shiksha Kosh (MUSK) are deducted from gross Union demands to prevent education-cess double-counting.
  * **Zero Centre–State Double-Counting:** Consolidated national spend nets out `3601 + 3602` Central Grants-in-Aid from the sum of Union and State spending.
  * **Structural Breaks:** Pre-`2014–15` society routing (`2202` → `3601`) and the `2017–18` Plan/Non-Plan to Revenue/Capital budget merger are explicitly flagged.

---

## 2. UDISE+ / DISE Schema (K–12 School Supply Census)

* **Coverage:** Longitudinal school-level administrative census (`2014–15` to `2025–26`, complete including `2023–24` Profile 2; `12.1M+` longitudinal school records; `1,466,682` schools in `2025–26`). Historical `2009–12` waves are reserved pending official NIEPA primary verification.
* **Key Harmonization Rules:**
  * **Dynamic Primary Key Deduplication:** School identifiers vary across eras (`udise_sch_code`, `pseudocode`, `schcd`); tables enforce strict `1:1` deduplication (`DISTINCT ON`) prior to profile, teacher, enrolment, and facility joins to prevent row explosion.
  * **Enrolment & Management Parity:** Reconciles to exact single-student Ministry of Education UDISE+ booklet totals across Government, Private Aided, and Private Unaided schools (`248.05M` in `2023–24`, `246.93M` in `2024–25`, `247.22M` in `2025–26`).
  * **Structural Break:** `2023–24` introduced Student Database Management System (SDMS) / Permanent Education Number (PEN) deduplication, separating pre-SDMS and post-SDMS enrolment trajectories.

---

## 3. ASER Rural Microdata Schema (`2009–2024`, MoU-Governed)

* **Coverage:** Six rural waves (`2009`, `2012`, `2014`, `2018`, `2022`, `2024`) totaling `3,501,238` unit-level records across 32 rural States/UTs (accessed under a formal non-redistribution Research MoU with ASER Centre).
* **Core Relational Tables:**
  * **`aser_village_base` (`104,083` rows, `10` cols | PK: `village_primary_key`):** Village connectivity (`has_pucca_road`, `has_electricity_connection`) and primary/secondary school availability within the village.
  * **`aser_child_cognitive_base` (`3,299,037` rows, `18` cols | PK: `child_primary_key`):** Children aged `5–16` surveyed at home (`school_status`, `current_class`, `takes_private_tuition`, `reading_level` `0–4`, `math_level` `0–4`, `english_level` `0–4`, and `2018` `applied_arithmetic_bonus` `0–4`).
  * **`aser_school_observation_base` (`98,118` rows, `16` cols | PK: `school_primary_key`):** Unannounced rural government Primary and Upper Primary school audits (`total_enrolled`, `total_present_on_visit`, `total_teachers_appointed`, `teachers_present_on_visit`, `usable_drinking_water`, `usable_boys_toilet`, `usable_girls_toilet`, `mid_day_meal_served`).
* **Harmonization Rules:** Resolves the `2009` vs. `2012–24` `schtype_class` Primary/Upper Primary coding inversion, maps `2018–24` internal state/LGD codes to canonical `(state_name, district_name)`, and strictly isolates the `2018–22` village middle-school questionnaire shift.

---

## 4. NSSO 71st Round Schema (Schedule 25.2, Jan–Jun `2014`)

* **Coverage:** Nationally representative cross-section (`470,266` harmonized records across 36 States/UTs and 639 districts).
* **Core Relational Tables:**
  * **`nsso71_household_base` (`65,926` rows | PK: `HH_ID`):** Geography, sector (`Rural`/`Urban`), social group, religion, school distances (`dist_primary_school`, `dist_upper_primary_school`, `dist_secondary_school`), computer/internet access, household consumption (`hh_cons_exp`, `umpce`), and multipliers (`wgt_combined`).
  * **`nsso71_person_base` (`310,827` rows | PK: `HH_ID, psrl_no`):** Demographics, 15-category education (`gen_edu_code` mapped to 9-tier `canonical_edu_level`), literacy (`gen_edu_code >= 2`), IT skills (`14+`), attendance status (`5–29`), and Block 7 non-attendance/dropout history (`51,657` rows).
  * **`nsso71_student_course_exp` (`93,513` rows | PK: `HH_ID, psrl_no`):** Current course level, course category (`General`, `Technical/Professional`, `Vocational`), institution type and recognition (`nature_inst`), private-school motives (`reason_private`), switching (`changed_edu_inst`), entitlements (Mid-Day Meal, scholarships, textbooks, fee waivers), private coaching, and five-item annual expenditure (`course_fee`, `books`, `transport`, `priv_coaching`, `other_exp`, `tot_exp`).
* **Weighting & Safeguards:** Weighted strictly by `wgt_combined`. `hostel_exp` (`1–35`) encodes Hostel Destination State/UT (excluded from rupee totals); `nature_inst` applies strictly to General Private Unaided students.

---

## 5. AISHE Canonical 8-Table Schema (`AY 2011–12` to `2023–24`)

* **Coverage:** Seven canonical waves (`2011–12`, `2012–13`, `2014–15`, `2018–19`, `2019–20`, `2021–22`, `2023–24`; `279,599` rows) across eight tables:
  1. `dim_institution` (Universities, Colleges, Standalone institutions by type/management/specialization)
  2. `dim_population` (Projected `18–23` population denominators by gender and SC/ST)
  3. `fact_enrolment` (Enrolment by institution type, management, programme level/name, discipline, mode of study, gender, and social category)
  4. `fact_outturn` (Graduate out-turn by level, programme, discipline, and gender)
  5. `fact_staff` (Teaching and non-teaching staff by post level, gender, and social category)
  6. `fact_foreign_students` (International students by country of origin, level, and gender)
  7. `fact_infrastructure` (Boys' and girls' hostel intake capacity and residing counts)
  8. `fact_macro_indicators` (Official GER, GPI, PTR, and Colleges per Lakh ratios)
* **Anti-Double-Counting Architecture:** Every table retains `source_table`, `geo_level` (`National`, `State`, `District`), and `response_type` (`Universe`, `Actual`, `Estimated`, `Actual+Estimated`, `Projected`). Multi-year retrospective series tables (`T-40..T-52`) are strictly isolated so blended totals (`T-6`) are never summed with component tables (`T-7/T-8/T-10`).

---

## 6. PLFS & NSSO Labour-Market Schema (`2009–10` to `2025`)

* **Coverage:** Six waves — NSSO 66th (`2009–10`), NSSO 68th (`2011–12`), PLFS (`2018–19`, `2021–22`, `2023–24`), and PLFS Calendar Year (`2025`) — totaling `4,111,292` First-Visit (`V1`) records (`778,434` households and `3,332,858` individuals across `612` to `764` decoded districts).
* **Core Relational Tables:**
  * **`plfs_household_base` (`13` cols | PK: `hh_primary_key`):** Wave, survey type, canonical state and district names (handling Telangana/Ladakh/UT restructurings), sector, social group, religion, household size, monthly usual consumer expenditure (`NULL` in `2009–10`), and `final_weight`.
  * **`plfs_person_base` (`18` cols | PK: `person_primary_key`):** Age, gender, raw and harmonized 9-tier `canonical_edu_level`, era-mapped `technical_edu_level`, `current_attendance_status`, `vocational_training_status`, `principal_activity_status` (`PS`), `subsidiary_activity_status` (`SS`), occupation (`nco_code_raw`, `NCO-2004`/`NCO-2015`), industry (`nic_code_raw`, `NIC-2004`/`NIC-2008`), and `final_weight`.
* **Official Multiplier Formulas (`final_weight`, Visit-1 Schedules Only):**
  * **NSSO (`2009–10`, `2011–12`):** `MLT / 100.0` if `NSS == NSC`, else `MLT / 200.0`.
  * **PLFS Annual (`2018–19` to `2023–24`):** `MULT / (100.0 * No_Qtr)` if `NSS == NSC`, else `MULT / (200.0 * No_Qtr)`.
  * **PLFS Calendar Year (`2025`):** `MULT / 100.0` (annualized combined multiplier published directly in `MULT`).

---

## 7. Canonical 9-Tier Education Crosswalk

Used to align education tiers seamlessly across household demand (`NSSO 71st`), tertiary supply (`AISHE`), and labour-market microdata (`NSSO/PLFS`):

| Tier | Canonical Label (`canonical_edu_level`) | School / Tertiary Equivalent |
| :---: | :--- | :--- |
| 1 | `Illiterate` | Not literate |
| 2 | `Below Primary` | Literate without formal schooling / below Class V |
| 3 | `Primary` | Classes I–V |
| 4 | `Upper Primary` | Classes VI–VIII (Middle) |
| 5 | `Secondary` | Classes IX–X |
| 6 | `Higher Secondary` | Classes XI–XII |
| 7 | `Diploma` | Diploma / Certificate (Technical & Non-Technical) |
| 8 | `Graduate` | Undergraduate degree (`B.A.`, `B.Sc.`, `B.Com.`, `B.Tech.`, etc.) |
| 9 | `Post-Graduate` | Post-Graduate degree, `M.Phil.`, and `Ph.D.` |
