# Education-to-Labour Market Outcomes — NSSO & PLFS (2009–10 to 2025)

**Directory:** `data_aggregated/1A_6_PLFS_Labour/`  
**Temporal Design:** Six-Wave Longitudinal Panel — National Sample Survey Office (NSSO) 66th Round (`2009–10`), NSSO 68th Round (`2011–12`), Periodic Labour Force Survey (PLFS) Annual Rounds (`2018–19`, `2021–22`, `2023–24`), and PLFS Calendar Year (`2025`)  
**Underlying Microdata:** `778,434` Visit-1 households and `3,332,858` individuals (`4,111,292` harmonized records across `612` to `764` decoded districts).  
**Column Definitions & Formulas:** See [`1A6_DATA_DICTIONARY.md`](1A6_DATA_DICTIONARY.md) (human-readable) or [`1A6_DATA_DICTIONARY.csv`](1A6_DATA_DICTIONARY.csv) (machine-readable).

---

## File Manifest (12 Aggregated Datasets)

| # | Filename | Analytical Scope |
| :---: | :--- | :--- |
| 1 | `1A6_four_era_macro_ps_psss_and_job_quality.csv` | Six-wave National, Zone, Typology, and State Labour Force Participation Rate (LFPR), Worker Population Ratio (WPR), and Unemployment Rate (UR) reported under both Usual Principal Status (PS) and Usual Principal + Subsidiary Status (PS+SS), Subsidiary-Only worker shares, and five-bucket employment quality. |
| 2 | `1A6_four_era_macro_transitions.csv` | Era-to-era percentage-point transition shifts (`2009–12`, `2011–18`, `2018–21`, `2021–24`, `2018–25`, and `2009–25`) across headline rates and job-quality categories, tagged with `spans_2025_sample_revamp`. |
| 3 | `1A6_education_tier_social_and_umpce_labour_inversion.csv` | Labour-market participation, unemployment, and employment quality across nine harmonized education tiers (`Illiterate` to `Post-Graduate`), social groups, and sector-specific Usual Monthly Per Capita Expenditure (UMPCE) consumption quintiles. |
| 4 | `1A6_graduate_technical_overqualification_and_mismatch.csv` | Education-to-occupation matching across National Classification of Occupations (NCO) skill tiers (High-Skill Divisions `1–3`, Mid-Skill `4–5`, Low-Skill/Manual `6–9`) and broad industries for general graduates, post-graduates, diploma holders, and technical degree cohorts. |
| 5 | `1A6_women_lfpr_disguised_work_and_u_curve_audit.csv` | Decomposition of post-`2018–19` female WPR changes across Regular Salaried, Own-Account, Employer, Unpaid Family Helper, Casual Labour, and Subsidiary-Only work, plus the female education–employment U-curve. |
| 6 | `1A6_neet_youth_15_29_anatomy_and_drivers.csv` | Youth (ages 15–29) Not in Employment, Education, or Training (NEET) decomposition across Unemployed Searching (`Code 81`), Domestic Duties Only (`Code 92`), Domestic Duties plus Free Collection (`Code 93`), and Disabled/Other (`Codes 94–97`). |
| 7 | `1A6_working_students_and_hh_income_bridge.csv` | Incidence and subsidiary-activity composition of individuals aged 15+ and youth aged 15–29 reporting educational attendance as their principal status while simultaneously working in subsidiary economic activity. |
| 8 | `1A6_post_covid_reagrarianization_tracker.csv` | Six-sector National Industrial Classification (NIC) structural employment shares tracking agriculture, manufacturing, construction, traditional services, modern services, and public/social services by sector, gender, and youth education tier. |
| 9 | `1A6_self_employment_gig_and_orange_economy.csv` | Disaggregation of self-employment (Own-Account workers vs. Employers vs. Unpaid Family Helpers) alongside reproducible `NIC-2008` (`2011–12` to `2025`) structural proxies for non-salaried platform/gig-exposed work and the Orange/Creative Economy. |
| 10 | `1A6_vocational_training_conversion_roi.csv` | Controlled comparison of labour-market outcomes across Formal Vocational, Informal/Non-Formal Vocational, and No Vocational Training groups holding general educational attainment constant. |
| 11 | `1A6_state_36_and_district_2025_adp_labour_diagnostics.csv` | `2025` state-level and 742-district composite labour-stress rankings (`composite_labour_stress_z`) alongside `2018–19` to `2025` within-state Difference-in-Differences (DiD) comparisons for the 112 Aspirational Districts Programme (ADP) districts. |
| 12 | `1A6_qa_benchmark_and_codebook_log.csv` | Eight-point verification log confirming exact six-wave Ministry of Statistics & Programme Implementation (MoSPI) PS and PS+SS macro parity, working-student counts, district thin-cell gating, and proxy definitions. |

---

## Methodological & Usage Notes

* **Survey Weighting & First-Visit Scope:** Every rate, share, quintile, and headcount estimate applies official MoSPI survey multipliers (`final_weight`) strictly to First-Visit (`V1`) schedules so urban panel revisits never inflate population totals.
* **Usual Principal Status (`PS`) vs. Usual Status (`PS+SS`):** Both `PS` and `PS+SS` metrics are reported side by side. Five-bucket employment quality covers `100%` of `PS+SS` workers. Because NSSO/PLFS `2011–25` leaves occupation (`NCO`) and industry (`NIC`) codes blank for Subsidiary-Only workers, all occupation skill-tier and industry shares are anchored strictly on `PS` workers across all six waves for longitudinal consistency.
* **Revamped `2025` Sampling Frame:** Calendar Year `2025` rows carry `revamped_2025_sample_flag = 1`, reflecting MoSPI's expanded monthly-continuous sample frame (`270,472` households / `764` districts — roughly `2.7x` larger than `2018–24` annual waves). Interpret `2023–24` to `2025` shifts alongside this frame expansion.
* **`2009–10` Exclusions & `NIC-2008` Proxies:** NSSO 66th Round (`2009–10`) does not include Block 9 monthly household consumption expenditure in the standard Schedule 10 archive (excluded from UMPCE quintiles) and uses `NIC-2004` (harmonized into six broad sectors, excluded from `NIC-2008` Gig-Exposed and Orange Economy proxies). Gig-Exposed estimates represent reproducible `NIC-2008` structural proxies pending MoSPI's dedicated platform-worker survey module.
* **Thin-Cell Reliability (`low_n_flag`):** Cells with fewer than `30` unweighted observations carry `low_n_flag = 1`. In `2025` district diagnostics, `742` of `764` districts meet the `n >= 30` qualification gate; the remaining `22` districts are masked to blank (`NULL`) and excluded from rankings.
* **Composite District Keys & Section Filtering:** Always pair `canonical_district_name` with `canonical_state_name` (to separate identically named districts such as Bijapur, Bilaspur, and Raigarh across states) and filter multi-part CSVs by `section`, `age_band`, and `slice_type` before summarizing.

---

## Data Verification

All 12 CSV files match official MoSPI NSSO and PLFS Annual Report Age 15+ PS and PS+SS benchmarks across all six waves (`2009–10` through `2025`), contain zero duplicate rows, and are fully documented in `1A6_DATA_DICTIONARY.md` and `1A6_DATA_DICTIONARY.csv`.
