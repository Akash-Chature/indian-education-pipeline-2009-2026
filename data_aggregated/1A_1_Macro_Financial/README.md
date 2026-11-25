# Stage 1A.1 — Macro-Financial Aggregated Outputs (FY 2009–10 to 2025–26)

**Audit Status:** Certified PASS (10 / 10 CSVs Verified — Nov 23, 2026)  
**Sources:** Union Budget Demand for Grants (MoE School & Higher Education, MSDE, NITI AIM, DST — Net of `PSK`/`MUSK` Recoveries), RBI *State Finances: A Study of Budgets* (31 States & UTs with Legislatures; Revenue `2202`, Capital `4202`, Education Loans `6202`), and MoSPI National Accounts / CPI Deflators (`2011–12` and `2022–23` spliced bases).

**Column Definitions & Formulas:** See [Data_Dictionary.md](Data_Dictionary.md) for all 192 columns, deflators, and accounting rules.
## File Manifest (10 Certified CSVs)

| # | Filename | Scope & Analytical Purpose |
| :---: | :--- | :--- |
| 1 | `1A1_national_consolidated_net_series.csv` | 10-wave consolidated National (Centre + 31 States/UTs minus `3601`/`3602` transfers) nominal & real (GDP/CPI deflated) series and `% of GDP` ratios. |
| 2 | `1A1_union_structural_decomposition.csv` | Union MoE School vs. Higher Education decomposition across Major Heads `2202`, `2203`, `3601`, `3602`, gross-vs-net `PSK`/`MUSK` recovery distortions, and structural break flags (`2014–15` routing shift & `2017` Plan/Non-Plan merger). |
| 3 | `1A1_state_31_revenue_capital_matrix.csv` | 31-state × 10-wave panel of nominal & real Revenue (`2202`) vs. Capital (`4202`) education outlays, `% of State NSDP`, capital share rankings, and era-specific real CAGRs. |
| 4 | `1A1_social_sector_crowding_out.csv` | Education's share of total State Social Sector expenditure across all 31 States/UTs (`2009–10` to `2025–26 BE`) with percentage-point crowding-out deltas. |
| 5 | `1A1_zone_and_demographic_rollups.csv` | Aggregated macro-fiscal rollups across 6 Administrative Zones and 4 Demographic-Fiscal Typologies (with explicit NSDP coverage flags). |
| 6 | `1A1_fiscal_anomalies_and_shocks.csv` | Flagged fiscal shocks: 7th Pay Commission revenue surges (`2014–15` → `2018–19`), COVID real revenue contractions (`2019–20` → `2021–22`), and `2024–25 RE` extreme spikes. |
| 7 | `1A1_rbi_loans_6202_covid_audit.csv` | State Government Education Loans & Advances (Major Head `6202`) vs. Capital Outlay (`4202`) across 31 States/UTs (`loans_6202_to_cap_4202_pct` validated up to `302.5%`). |
| 8 | `1A1_union_schemes_bodies_summary.csv` | Granular Union MoE scheme and autonomous-body ledger (BE / RE / Actual utilization, slippage %, and real actual outlays including UGC, AICTE, Central Universities, HEFA, Samagra Shiksha, RUSA/PM-USHA). |
| 9 | `1A1_external_msde_dst_niti_summary.csv` | External Union SBE ledger covering MSDE, Department of Science & Technology (DST), and NITI Aayog Atal Innovation Mission (AIM) across BE / RE / Actuals. |
| 10 | `1A1_digital_skilling_rd_2009_18_vs_2020_26_pivot.csv` | 4-Ministry (`MoE Digital/NEP + MSDE + NITI AIM + DST`) pre-NEP vs. post-NEP nominal and real (`2011–12` base) annual comparison through `2026–27 BE`. |

## Methodological Notes
* **Basis Discipline:** Never average or pool `Actual`, `RE` (Revised Estimates), and `BE` (Budget Estimates) without filtering on `data_basis` / `union_basis` / `rbi_basis`.
* **Geographic Symmetry:** RBI State Finances covers the **31 States & UTs with Legislatures** (`30` pre-Telangana in `2009–10` to `2012–13`; `31` from `2014–15` onward). The 5 non-legislature UTs (and Ladakh post-2019) do not maintain separate state budgets in RBI appendices.
* **Precision Audit:** All 10 files passed November 23, 2026 QA hardening (`172` IEEE-754 floating-point noise cells scrubbed with zero metric drift).

