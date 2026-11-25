# Stage 1A Output CSV Naming Conventions, Formula Standards & Dictionary Index

**Repository:** `Akash-Chature/indian-education-pipeline-2009-2026`  
**Audit Certification:** PASS — `45 / 45` Stage 1A CSVs Verified (`1,146` total columns across 3 institutional lakehouses; Nov 23, 2026)  
**Companion Document:** See [`docs/data_dictionary.md`](data_dictionary.md) for the underlying 6-lakehouse DuckDB/Parquet source schemas. This guide governs the **derived analytical output CSVs** published in `data_aggregated/`.

---

## 1. Release Gate & Navigation Map

| Folder | Certified CSVs | Documented Cols | Release Gate & GitHub Status | Folder-Level Dictionary |
| :--- | :---: | :---: | :--- | :--- |
| **`1A_1_Macro_Financial`** | `10` | `192` | **`PUBLIC_GITHUB_READY` — LIVE NOW** | [`data_aggregated/1A_1_Macro_Financial/DATA_DICTIONARY.md`](../data_aggregated/1A_1_Macro_Financial/DATA_DICTIONARY.md) |
| **`1A_2_UDISE_Supply`** | `22` | `732` | **`PUBLIC_GITHUB_READY` — Staged** *(held pending official NIEPA `2009–12` DISE reply; Dec 2 cutoff)* | `data_aggregated/1A_2_UDISE_Supply/DATA_DICTIONARY.md` *(publishes with `1A_2` release)* |
| **`1A_3_ASER_Ground_Truth`** | `13` | `222` | **`PRIVATE_ASER_REVIEW_ONLY_HOLD_GITHUB`** *(held offline pending written ASER Centre MoU clearance)* | Held offline until ASER Centre clearance |

---

## 2. Universal Column Naming Conventions (Across All Stage 1A CSVs)

Every output CSV adheres to a predictable `prefix_stem_suffix` grammar so external researchers can infer units, signs, and denominators at a glance:

### A. Unit & Metric Suffixes
| Suffix | Meaning & Unit | Example Columns |
| :--- | :--- | :--- |
| `_cr` | Indian Rupees in **Crores** (`₹ 1 Crore = ₹ 10 Million`) | `consolidated_net_nominal_cr`, `real_cap_ed_2011base_cr` |
| `_rs` | Indian Rupees in **exact ₹** (not Crores) | `govt_grants_receipt_rs`, `mean_grant_per_student_rs` |
| `_pct` | Percentage (`%`, scale `0–100`; see folder caveats for unbounded ratios `>100%` or stacked delta rows) | `capital_share_pct`, `rate_c9_10_pct`, `read_std2_pct` |
| `_pp` | **Percentage-point difference** (`End % − Start %`, or `Group A % − Group B %`) | `pro_male_private_gap_pp`, `fe_gap_pvt_enr_pp`, `DiD_govt_schools_pp` |
| `_ratio` | Dimensionless ratio (Numerator ÷ Denominator) | `se_to_he_ratio`, `primary_to_secondary_supply_ratio`, `reco_ratio_2202_over_4202` |
| `_cagr_pct` | Compound Annual Growth Rate (`((End / Start)^(1/n) − 1) × 100`, in `%`) | `era_real_gdp_cagr_pct`, `enr_c1_12_cagr_pct` |
| `_z` | Standardized composite z-score (mean `0`, SD `1`; higher = more severe deprivation) | `deprivation_index_z`, `rural_learning_deprivation_z` |
| `_cnt` / `n_*` | Integer or float entity count (schools, villages, households, tested children) | `gt_func_cnt`, `n_mixed_households`, `n_schools_visited` |
| `_flag` / `_note` | Explicit binary (`0`/`1`) or text audit disclosure tag | `raw_udise_rep_gt_enr_flag`, `suppressed_flag`, `structural_break_flag` |

### B. Analytical Prefixes & Time/Basis Tags
| Prefix / Token | Meaning | Where Used |
| :--- | :--- | :--- |
| `delta_*` / `net_*_chg_*` | Period-to-period absolute or percentage-point change (`End − Start`) | `1A2_four_era_policy_transitions.csv`, `1A2_national_state_consolidation_closures.csv` |
| `DiD_*` | Difference-in-Differences estimator: `(ADP_post − ADP_pre) − (NonADP_post − NonADP_pre)` | `1A2_aspirational_districts_112_did_audit.csv` |
| `fe_gap_*_pp` | Within-household Fixed-Effects gap (`Male − Female` in `pp`; positive = boys lead, negative = sisters lead) | `1A3_within_household_gender_fixed_effects.csv` *(post-clearance)* |
| `real_*_2011base_*` / `_2022base_*` | Inflation-adjusted real rupees (`nominal / (deflator / 100)`) | `1A_1_Macro_Financial` |
| `*_best_nom_cr` / `*_best_real_cr` | Best-available budget waterfall selecting **`Actual > RE > BE`** (`COALESCE`) | `1A1_digital_skilling_rd_2009_18_vs_2020_26_pivot.csv` |
| `act` / `re` / `be` | Union/RBI accounting basis: Audited `Actual`, `Revised Estimate`, `Budget Estimate` | `1A_1_Macro_Financial` |
| `c1`..`c12` / `cpp` / `_b` / `_g` | Formal Grades `1–12`, Pre-Primary (`PP`), Boys (`_b`), Girls (`_g`) | `1A_2_UDISE_Supply` |
| `pri` / `upr` / `sec` / `hsec` | Primary (`C1–5`), Upper Primary (`C6–8`), Secondary (`C9–10`), Higher Secondary (`C11–12`) | `1A_2_UDISE_Supply` |
| `le1` / `lt3` / `lt4` / `le30` | Threshold inequalities: `<= 1`, `< 3`, `< 4`, `<= 30` | `1A_2` (`killzone_le30`) & `1A_3` (`read_zero_floor_le1`, `cannot_divide_lt4`) |

---

## 3. Six Cross-Cutting Traps External Users Must Avoid

1. **Do Not Sum Across `rollup_type`, `section`, `slice_type`, or `All India` Rows:** Several rollups stack `National`, `Administrative Zone`, and `Demographic-Fiscal Typology` (or `All 31 States & UTs` + individual states) in the same CSV. Always filter to a single grouping level before taking sums.
2. **Do Not Mix `Actual`, `RE`, and `BE` Silently (`1A_1`):** Always filter on `data_basis` / `union_basis` / `rbi_basis` for audited comparisons.
3. **Respect `FY` vs. `AY` Era Boundaries (`1A_1` vs. `1A_2`):** `1A_1` maps fiscal years (`FY 2014–15` = Era 2; `FY 2023–24` = Era 4), while `1A_2` maps academic years (`AY 2014–15` = Era 1 End; `AY 2023–24` = Era 3B SDMS break).
4. **Do Not Sum Mutually Exclusive Caste Groups With Orthogonal Minorities (`1A_2`):** `General + SC + ST + OBC = 100%`; `Muslim` / `All Minorities` overlap with those categories.
5. **Filter Stacked Level-vs-Delta Rows Before Plotting (`1A_3`):** Four ASER tables store wave percentages (`0–100%`) and `pp` deltas/lifts (validly negative) in the same `*_pct` columns—always filter on `section`, `record_type`, or `mdm_status`.
6. **Check Portal Scope & Valid Out-of-Range Ratios Before Flagging Outliers:**
   - `1A_1`: `utilization_pct_actual_vs_be` (`>100%` via supplementary grants), `loans_6202_to_cap_4202_pct` (up to `302.51%`), `reco_ratio_2202_over_4202` (up to `34,070` when CapEx ≈ 0).
   - `1A_2`: `*_trans_*_pct` (apparent cohort ratios `>100%` in Urban/UT cells), `unspent_grant_pct` (negative when spending carry-over balances), `dw_nonfunc_gap_pp` (`-4.56 pp` raw portal quirk), `rate_c11_12_pct` (`4` flagged 2014–15 Karnataka PUC rows), and `cyber_safety_2025_pct` (Govt/Aided-only portal scope; coded `0` for all `136,945` Private Unaided schools).

