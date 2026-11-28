# Public Finance & Education Expenditure (FY 2009–10 to FY 2025–26)

**Directory:** `data_aggregated/1A_1_Macro_Financial/`  
**Temporal Scope:** Financial Years (April–March) `2009–10` to `2025–26` (10 harmonized waves; four-ministry innovation, skilling, and Research & Development (R&D) series extends to `2026–27` Budget Estimates)  
**Primary Sources:** Union Budget Expenditure Profile / Statement of Budget Estimates (Ministry of Education, Ministry of Skill Development & Entrepreneurship, Department of Science & Technology, and NITI Aayog Atal Innovation Mission), Reserve Bank of India (RBI) *State Finances: A Study of Budgets* (31 States and Union Territories with Legislatures), and Ministry of Statistics & Programme Implementation (MoSPI) National Accounts and Consumer Price Index (CPI) deflators.  
**Column Definitions & Formulas:** See [`1A1_DATA_DICTIONARY.md`](1A1_DATA_DICTIONARY.md) (human-readable) or [`1A1_DATA_DICTIONARY.csv`](1A1_DATA_DICTIONARY.csv) (machine-readable).

---

## File Manifest (10 Aggregated Datasets)

| # | Filename | Analytical Scope |
| :---: | :--- | :--- |
| 1 | `1A1_national_consolidated_net_series.csv` | Ten-wave consolidated national public education expenditure (Union Ministry of Education plus 31 States/Union Territories, netting out intergovernmental transfers under Major Heads `3601` and `3602`), nominal and inflation-adjusted real totals (Gross Domestic Product (GDP) and CPI-Combined deflators), and shares of GDP. |
| 2 | `1A1_union_structural_decomposition.csv` | Union Ministry of Education decomposition across School Education & Literacy and Higher Education by Major Heads (`2202`, `2203`, `3601`, `3602`), gross-versus-net reserve-fund recovery adjustments, and structural break indicators. |
| 3 | `1A1_state_31_revenue_capital_matrix.csv` | Panel of 31 States and Union Territories across 10 waves detailing nominal and real Revenue Expenditure (`2202`) and Capital Outlay (`4202`) on education, shares of Net State Domestic Product (NSDP), capital-share rankings, and era-specific real Compound Annual Growth Rates (CAGRs). |
| 4 | `1A1_social_sector_crowding_out.csv` | Education's share of total State Social Sector expenditure across all 31 States and Union Territories (`2009–10` to `2025–26` Budget Estimates) with percentage-point shifts over time. |
| 5 | `1A1_zone_and_demographic_rollups.csv` | Macro-fiscal aggregates grouped across six Administrative Zones and four Demographic-Fiscal State Typologies, including explicit NSDP coverage flags. |
| 6 | `1A1_fiscal_anomalies_and_shocks.csv` | Summary of major fiscal discontinuities, including 7th Central Pay Commission revenue surges (`2014–15` to `2018–19`), pandemic-period real revenue contractions (`2019–20` to `2021–22`), and sharp Revised Estimate spikes (`2024–25`). |
| 7 | `1A1_rbi_loans_6202_covid_audit.csv` | State government Education Loans & Advances (Major Head `6202`) benchmarked against Education Capital Outlay (`4202`) across 31 States and Union Territories. |
| 8 | `1A1_union_schemes_bodies_summary.csv` | Scheme- and institution-level Union Ministry of Education ledger tracking Budget Estimates (BE), Revised Estimates (RE), Audited Actuals, utilization rates, mid-year slippage, and real outlays across major bodies and programmes (including University Grants Commission, All India Council for Technical Education, Central Universities, Higher Education Financing Agency, Samagra Shiksha, and RUSA/PM-USHA). |
| 9 | `1A1_external_msde_dst_niti_summary.csv` | Union expenditure ledger covering the Ministry of Skill Development & Entrepreneurship (MSDE), Department of Science & Technology (DST), and NITI Aayog's Atal Innovation Mission (AIM) across BE, RE, and Audited Actuals. |
| 10 | `1A1_digital_skilling_rd_2009_18_vs_2020_26_pivot.csv` | Four-ministry comparison (Ministry of Education Digital/National Education Policy/R&D, MSDE, NITI Aayog AIM, and DST) contrasting pre-2020 and post-2020 nominal and constant-price (`2011–12` base) outlays through `2026–27` BE. |

---

## Methodological & Accounting Notes

* **Zero Centre–State Double-Counting:** Central Grants-in-Aid to States (`3601`) and Union Territories with Legislatures (`3602`) flow into State Treasuries and are recorded again as State Revenue Expenditure (`2202`). Consolidated national expenditure nets out `3601 + 3602` from combined Union and State totals so each rupee is counted exactly once.
* **Net-of-Recoveries Union Accounting:** Education cess transfers routed through Prarambhik Shiksha Kosh (PSK) and Madhyamik and Uchhatar Shiksha Kosh (MUSK) reserve funds are deducted from gross Union demands so all departmental totals reflect net developmental outlays.
* **Accounting Basis Discipline (`Actual` vs. `RE` vs. `BE`):** Every table preserves explicit basis columns (`data_basis`, `union_basis`, `rbi_basis`). Do not pool Budget Estimates or Revised Estimates silently with Audited Actuals when computing growth trends or regressions.
* **Structural Breaks:** (1) Prior to `2014–15`, Centrally Sponsored Schemes such as Sarva Shiksha Abhiyan and Mid-Day Meal were routed directly to State Implementing Societies under Central Major Head `2202` rather than through State Treasuries under `3601`. (2) Following the `2017–18` Union Budget abolition of the Plan/Non-Plan classification, pre-`2017` Plan/Non-Plan totals are not directly equivalent to post-`2017` Revenue/Capital accounts.
* **Geographic Scope & Column Naming:** RBI State Finances covers 30 entities pre-Telangana (`2009–10` to `2012–13`) and 31 States and Union Territories with Legislatures from `2014–15` onward (the five non-legislature Union Territories and post-2019 Ladakh are financed directly via the Union Budget). The fiscal-year column is named `academic_year` strictly to enable direct joins with school and higher-education datasets.
* **Unbounded Ratios:** Metrics such as `utilization_pct_actual_vs_be` (exceeding `100%` when supplementary grants are approved), `loans_6202_to_cap_4202_pct` (up to `302.5%`), and `reco_ratio_2202_over_4202` (Revenue divided by Capital) are ratios rather than bounded `0–100%` shares.

---

## Data Verification

All 10 CSV files are verified for exact reconciliation with official Union Budget net totals and RBI State Finance appendices, zero duplicate records, consistent rounding, and complete column-level documentation in `1A1_DATA_DICTIONARY.md` and `1A1_DATA_DICTIONARY.csv`.
