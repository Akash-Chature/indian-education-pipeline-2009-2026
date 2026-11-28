# Higher Education Capacity, Privatization & Equity — AISHE (AY 2011–12 to 2023–24)

**Directory:** `data_aggregated/1A_5_AISHE_HigherEd/`  
**Temporal Design:** Seven-Wave Longitudinal Panel (`2011–12`, `2012–13`, `2014–15`, `2018–19`, `2019–20`, `2021–22`, and `2023–24`)  
**Primary Source:** All India Survey on Higher Education (AISHE), Ministry of Education, Government of India (harmonized across eight canonical institution, population, enrolment, out-turn, faculty, international student, hostel infrastructure, and macro-indicator tables).  
**Column Definitions & Formulas:** See [`1A5_DATA_DICTIONARY.md`](1A5_DATA_DICTIONARY.md) (human-readable) or [`1A5_DATA_DICTIONARY.csv`](1A5_DATA_DICTIONARY.csv) (machine-readable).

---

## File Manifest (12 Aggregated Datasets)

| # | Filename | Analytical Scope |
| :---: | :--- | :--- |
| 1 | `1A5_four_era_ger_gpi_and_macro_scorecard.csv` | Seven-wave National, Zone, Typology, and State scorecard tracking Universities, Colleges, Standalone Institutions, total and female enrolment, Gross Enrolment Ratio (GER), Gender Parity Index (GPI), teaching faculty, and Pupil-Teacher Ratio (PTR). |
| 2 | `1A5_four_era_macro_transitions.csv` | Era-to-era transition deltas and annualized Compound Annual Growth Rates (CAGRs) across `2011–15`, `2014–19`, `2018–22`, `2021–24`, and `2011–24`, carrying explicit population-projection break flags. |
| 3 | `1A5_university_type_12tier_evolution_and_choice.csv` | Twelve-tier University institutional evolution tracking count and enrolment shares across Central, State Public, State Private, Deemed, Open, and specialized universities, along with social inclusion and implied university-department PTR. |
| 4 | `1A5_premier_ini_dilution_and_torchbearer_audit.csv` | Longitudinal audit of Institutes of National Importance (INIs — IITs, IIMs, NITs, IIITs, IISERs, AIIMS) tracking campus expansion, female representation, implied PTR, senior-professor faculty depth, doctoral (Ph.D.) share, and hostel occupancy. |
| 5 | `1A5_college_privatization_viability_and_rural_women_supply.csv` | College expansion and average enrolment scale across Government, Private Aided, and Private Unaided affiliated colleges, annual new-college composition, and institutional specialization mixes. |
| 6 | `1A5_odl_holding_pen_and_programme_levels.csv` | Mode-of-study decomposition across Regular, Open & Distance Learning (ODL), Private External, and Online modes (`2023–24`), plus state-level ODL inflation of headline GER. |
| 7 | `1A5_discipline_and_programme_sorting_stem_vs_arts.csv` | Enrolment trends and gender composition across major disciplines and flagship degree programmes (comparing Broad STEM, Engineering & Technology, Medical Science, Pure Science, Commerce, and Arts/Humanities). |
| 8 | `1A5_social_equity_and_minority_pwd_inclusion.csv` | Enrolment shares, GER gaps, and female parity across Scheduled Caste (SC), Scheduled Tribe (ST), Other Backward Classes (OBC), Economically Weaker Section (EWS), Muslim, Other Minority, and Persons with Disabilities (PwD) cohorts. |
| 9 | `1A5_outturn_efficiency_and_faculty_architecture.csv` | Graduate out-turn-to-enrolment ratios across degree levels alongside teaching faculty hierarchy (Professor, Associate Professor, Assistant Professor, and Temporary/Tutor/Visiting staff), social representation, and the senior-faculty gender gap. |
| 10 | `1A5_foreign_students_and_hostel_infrastructure.csv` | International student enrolment by level and top source countries alongside boys' and girls' hostel intake capacity and occupancy rates. |
| 11 | `1A5_state_36_and_district_adp_tertiary_diagnostics.csv` | Composite `2023–24` state-level tertiary stress scorecard (`composite_tertiary_stress_z`) paired with the `2011–12` 640-district college-density distribution and 112 Aspirational Districts Programme (ADP) overlap analysis. |
| 12 | `1A5_qa_benchmark_and_provenance_log.csv` | Eight-point verification log confirming exact parity with official Ministry of Education / Press Information Bureau `2023–24` benchmarks, mode-reconciliation residuals (`0`), and source-table provenance. |

---

## Methodological & Usage Notes

* **Zero Double-Counting Across Response Scopes:** Official AISHE reports publish distinct Universe, Actual, Estimated, and Actual+Estimated tables alongside multi-year retrospective series (`T-40` to `T-52`). Every table here draws from a single authoritative `(source_table, response_type)` per wave and excludes multi-year series tables so component modes reconcile to the exact student (`45,001,126` total enrolment in `2023–24`).
* **Population Denominator Break (`2021–22` Onward):** Starting in `2021–22`, the Ministry of Education shifted the `18–23` projected population denominator from Census 2011 projections (`142.33M` in `2019–20`) to revised 2020 projections (`152.44M` in `2021–22`, `149.88M` in `2023–24`). Rows affected by this break are flagged via `pop_projection_series_flag` and `spans_pop_proj_break`.
* **Geographic Normalization:** Inconsistent raw state spellings in population tables (`Tamilnadu` without a space in `2019–20` to `2023–24`) are standardized to `Tamil Nadu` so regional rollups carry zero missing state populations.
* **Source Availability Disclosures:** (1) AISHE does not disaggregate international students (`foreign_mode_disagg_available = 0`) or graduate out-turn (`outturn_mode_disagg_available = 0`) by mode of study; (2) AISHE does not collect student tuition fees (for household tertiary cost benchmarks, see [`../1A_4_NSSO71_Demand/`](../1A_4_NSSO71_Demand/)); (3) district-level college tables (`T-40`) are published only in the `2011–12` wave.
* **Multi-Level Filtering:** Never sum across `geo_level` (`National`, `Zone`, `Typology`, `State`, `District`) or `section` values within the same CSV.

---

## Data Verification

All 12 CSV files match official Ministry of Education `2023–24` headline figures (`45,001,126` enrolment, `30.02` GER, `1.08` GPI, `1,732,294` faculty, `1,289` universities, and `48,246` colleges), contain zero duplicate rows, and are fully documented in `1A5_DATA_DICTIONARY.md` and `1A5_DATA_DICTIONARY.csv`.
