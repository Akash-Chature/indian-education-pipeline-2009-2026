# Stage 1A.5 — AISHE Higher Education Longitudinal Audit (AY 2011–12 to 2023–24)

**Audit Status:** Certified PASS (`12 / 12` CSVs Verified — `0` IEEE-754 float noise, `0` duplicate rows, `0` series-table double counting; Nov 27, 2026)  
**Temporal Design:** **7-Wave Longitudinal Panel** (`2011-12`, `2012-13`, `2014-15`, `2018-19`, `2019-20`, `2021-22`, `2023-24`; `279,599` rows across 8 canonical master tables; exact match to MoE/PIB `2023-24` benchmarks: `45,001,126` enrolment, `30.02` GER, `1.08` GPI, `1,732,294` faculty).  
**Column Definitions & Formulas:** See [`1A5_DATA_DICTIONARY.md`](1A5_DATA_DICTIONARY.md) (human-readable) or [`1A5_DATA_DICTIONARY.csv`](1A5_DATA_DICTIONARY.csv) (machine-readable).

## File Manifest (`12` Certified CSVs)

| # | Filename | Analytical Focus |
| :---: | :--- | :--- |
| 1 | `1A5_four_era_ger_gpi_and_macro_scorecard.csv` | 7-wave National, Zone, Typology, and State scorecard covering Universities, Colleges, Standalone institutions, Enrolment, GER/GPI (All/Female/SC/ST), Faculty, and Regular/Total PTR. |
| 2 | `1A5_four_era_macro_transitions.csv` | Era-to-era transition deltas and annualized CAGRs (`2011–15`, `2014–19`, `2018–22`, `2021–24`, `2011–24`) with `spans_pop_proj_break` flags. |
| 3 | `1A5_university_type_12tier_evolution_and_choice.csv` | 12-Tier University typology evolution showing State Private Universities (`464`) overtaking State Public (`462`), enrolment shares, caste/gender inclusion gaps, and implied university-dept PTR. |
| 4 | `1A5_premier_ini_dilution_and_torchbearer_audit.csv` | Premier Institutions of National Importance (`INI`: IIT/IIM/NIT/IIIT/IISER/AIIMS) audit tracking `2.69×` campus growth, `+10.52 pp` female jump, PTR loosening (`14.04 → 17.79`), `-9.28 pp` Full Professor drop, and `-7.00 pp` Ph.D. share decline. |
| 5 | `1A5_college_privatization_viability_and_rural_women_supply.csv` | Government vs. Private Aided vs. Private Unaided college expansion (`32,513` Private Unaided colleges; `78.46%` of new colleges in `2023-24`; `486` vs. `1,119` avg scale) and specialization mixes. |
| 6 | `1A5_odl_holding_pen_and_programme_levels.csv` | Regular vs. Distance (`40.56%` of university-level enrolment), Private External, and debut Online (`284.0k`) modes, plus state-level ODL GER inflation (`Delhi +31.36 pp`, `Kerala +8.20 pp`, `TN +5.60 pp`). |
| 7 | `1A5_discipline_and_programme_sorting_stem_vs_arts.csv` | Programme-level & discipline sorting exposing the "Women in STEM" split (`Medical 57.40%` & `Science 55.41%` vs. `Engg & Tech 31.26%`), `B.Tech.` stagnation, and `B.A.` massification (`13.30M`). |
| 8 | `1A5_social_equity_and_minority_pwd_inclusion.csv` | SC, ST, OBC, EWS, Muslim (`5.01%`), Other Minority, and PWD (`0.24% → 0.12%`) enrolment shares, GER gaps, and female parity across all 7 waves. |
| 9 | `1A5_outturn_efficiency_and_faculty_architecture.csv` | Graduate out-turn vs. enrolment ratios, faculty hierarchy (Professor / Associate / Assistant / Temp-Tutor `13.34%`), caste representation, and the frozen `-14.0 pp` Full-vs-Asst Professor female glass ceiling. |
| 10 | `1A5_foreign_students_and_hostel_infrastructure.csv` | International student inflows (`33.2k → 58.1k`, flat at `0.13%` of HE, top source countries) and boys'/girls' hostel intake capacity vs. occupancy rates (`89.24%` in INIs). |
| 11 | `1A5_state_36_and_district_adp_tertiary_diagnostics.csv` | 36-State `2023-24` composite tertiary stress scorecard (`composite_tertiary_stress_z`) + `2011-12` `T-40` 640-district Tertiary Desert ranking (`74 / 100` bottom districts outside 112 ADP). |
| 12 | `1A5_qa_benchmark_and_provenance_log.csv` | 8-check MoE/PIB parity, anti-double-counting (`T-6 = T-6a+b+c+d` zero residual), population-break, and transparency log. |

## Methodological Disclosures
* **Zero Double-Counting:** Every metric pulls strictly from one authoritative `(source_table, response_type)` per wave; multi-year historical series tables (`T-40..T-52`) are strictly excluded.
* **2021–22 Population Projection Break & Tamil Nadu Fix:** From `2021-22` onward, MoE adopted revised `2020` `18–23` population projections (`+10.12M` jump, flagged via `pop_projection_series_flag`); raw `'Tamilnadu'` spellings (`2019–24`) were normalized to `'Tamil Nadu'` (`0` drops).
* **Scope Transparency Flags:** AISHE does not disaggregate Foreign Students (`foreign_mode_disagg_available = 0`) or Out-turn (`outturn_mode_disagg_available = 0`) by mode of study, and omits tuition fees (linked to `1A_4` in Stage `1B`). District-level tables (`T-40`) exist strictly for `2011-12`.


