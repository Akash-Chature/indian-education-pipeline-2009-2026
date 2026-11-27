# Stage 1A.6 — PLFS & NSSO Labour-Market Longitudinal Audit (2009–10 to 2025)

**Audit Status:** Certified PASS (`12 / 12` CSVs Verified — `0` IEEE-754 float noise, `0` exact duplicate rows, `100%` 6-wave MoSPI PS & PS+SS macro parity; Nov 27, 2026)  
**Temporal Design:** **6-Wave Longitudinal Panel** (`NSSO 66th 2009-10`, `NSSO 68th 2011-12`, `PLFS 2018-19`, `2021-22`, `2023-24`, and `PLFS 2025 Calendar Year`; `4,111,292` records: `778,434` households & `3,332,858` persons).  
**Column Definitions & Formulas:** See [`1A6_DATA_DICTIONARY.md`](1A6_DATA_DICTIONARY.md) (human-readable) or [`1A6_DATA_DICTIONARY.csv`](1A6_DATA_DICTIONARY.csv) (machine-readable).

## File Manifest (`12` Certified CSVs)

| # | Filename | Analytical Focus |
| :---: | :--- | :--- |
| 1 | `1A6_four_era_macro_ps_psss_and_job_quality.csv` | 6-wave National, Zone, Typology, and State LFPR / WPR / UR (`PS` & `PS+SS`), `SS-Only` share, and 5-bucket job quality (`Regular Salaried`, `Own-Account`, `Employer`, `Unpaid Family Helper`, `Casual Labour`). |
| 2 | `1A6_four_era_macro_transitions.csv` | Era-to-era percentage-point transition shifts (`2009–12`, `2011–18`, `2018–21`, `2021–24`, `2018–25`, `2009–25`) with `spans_2025_sample_revamp` flag. |
| 3 | `1A6_education_tier_social_and_umpce_labour_inversion.csv` | Labour-market outcomes across all 9 harmonized `canonical_edu_level` tiers (`Illiterate` to `Post-Graduate`), social groups, and `UMPCE` wealth quintiles (exposing `11.18%` Graduate UR and Urban `Q1–Q2` `15%` UR). |
| 4 | `1A6_graduate_technical_overqualification_and_mismatch.csv` | Vertical over-qualification (`30.52%` of employed Graduates & `62.14%` of HSec workers in low-skill `NCO 6–9` jobs; `UP 50.35%`) vs. formal Engineering (`79.65%` `NCO 1–3`) & Medicine (`86.07%`). |
| 5 | `1A6_women_lfpr_disguised_work_and_u_curve_audit.csv` | Decomposition of the `+15.53 pp` (`2018–25`) female WPR surge (`79.39%` Own-Account + Unpaid Family Labour; `12.69%` Salaried) and the female education U-curve (peaking in salaried quality among Diploma women). |
| 6 | `1A6_neet_youth_15_29_anatomy_and_drivers.csv` | Youth `15–29` NEET decomposition (`25.85%` in `2025`): Domestic Duties (`Code 92`), Subsistence Relief (`Code 93`: `10.36% → 3.74%`), and Male Unemployed Searching (`Code 81`). |
| 7 | `1A6_working_students_and_hh_income_bridge.csv` | Working students (`4.16M` youth `15–29` in `2025`, `2.54×` `2018–19`), contrasting Rural `Q1` Unpaid Farm Helpers (`87.22%`) against Urban `Q4–Q5` Regular Salaried college students (`45.6%–45.7%`). |
| 8 | `1A6_post_covid_reagrarianization_tracker.csv` | 6-sector `NIC` structural shares showing flat Manufacturing (`11.1% → 12.2%`), Female farm feminization (`53.17% → 57.23%`), and Educated Youth farm jump (`12.15% → 18.54%`, `67.42%` unpaid). |
| 9 | `1A6_self_employment_gig_and_orange_economy.csv` | Self-employment decomposition (`Own-Account` vs. `Employer` `3.57%`) plus `NIC-2008` (`2011–25`) structural proxies for Gig-Exposed work (`21.24M`) and the Orange / Creative Economy (`2.34×` to `5.99M`). |
| 10 | `1A6_vocational_training_conversion_roi.csv` | Controlled Formal vs. Informal vs. No Vocational Training ROI holding general education constant (`+17.99 pp` Sec/HSec salaried lift; `-6.72 pp` Dip/Grad+ UR drop; `1.45%` Central Zone reach). |
| 11 | `1A6_state_36_and_district_2025_adp_labour_diagnostics.csv` | `2025` 36-State & `742`-District composite labour stress ranking (`80 / 100` highest-stress districts outside 112 ADP) plus `2018–25` within-state ADP job-quality DiD (`+4.16 pp` graduate over-qualification). |
| 12 | `1A6_qa_benchmark_and_codebook_log.csv` | 8-check MoSPI 6-wave PS & PS+SS parity, working-student parity (`12,742` sample), `2025` frame flag, and hardening log. |

## Methodological Disclosures
* **Survey Weighting & Visit-1 Hygiene:** All rates, shares, quintiles, and population estimates use official MoSPI `final_weight` multipliers on Visit-1 schedules only (`0` revisit inflation).
* **Revamped 2025 Frame:** `2025` Calendar Year rows carry `revamped_2025_sample_flag = 1` (`270,472` HHs / `764` districts — `2.7×` larger than `2018–24` annual waves).
* **NCO/NIC & Proxy Scope:** Because `SS-Only` workers have `NULL` NCO/NIC in `2011–25`, all occupation/industry shares anchor strictly on `PS` workers across all 6 waves while 5-bucket activity quality covers `100%` of `PS+SS` workers. Gig & Orange economy figures are reproducible `NIC-2008` (`2011–25`) structural proxies pending MoSPI's Jan `2027` platform-worker module.

