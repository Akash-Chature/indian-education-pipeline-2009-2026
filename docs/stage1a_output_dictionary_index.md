# Stage 1A Output CSV Naming Conventions, Formula Standards & 6-Folder Dictionary Index

**Repository:** `Akash-Chature/indian-education-pipeline-2009-2026`  
**Audit Certification:** PASS — `81 / 81` Stage 1A CSVs Verified across all 6 institutional lakehouses (`0` IEEE-754 float noise, `0` exact duplicate rows; Updated Nov 27, 2026)  
**Companion Document:** See [`docs/data_dictionary.md`](data_dictionary.md) for the underlying 6-lakehouse DuckDB/Parquet source schemas. This guide governs the **derived analytical output CSVs** published in `data_aggregated/`.

---

## 1. Six-Folder Release Gate & Dictionary Navigation Map

| Stage & Folder | Temporal Design | Certified CSVs | Release Gate & GitHub Status | Folder-Level Dictionary |
| :--- | :--- | :---: | :--- | :--- |
| **`1A_1_Macro_Financial`** | `FY 2009–10` to `2025–26` (Longitudinal) | `10` | **`LIVE NOW`** | [`data_aggregated/1A_1_Macro_Financial/DATA_DICTIONARY.md`](../data_aggregated/1A_1_Macro_Financial/DATA_DICTIONARY.md) |
| **`1A_2_UDISE_Supply`** | `AY 2014–15` to `2025–26` (Longitudinal) | `22` | **Staged (Dec 2 Cutoff)** *(awaiting official NIEPA `2009–12` DISE reply)* | Publishes with `1A_2` release |
| **`1A_3_ASER_Ground_Truth`** | `2009–2024`, 6 Rural Waves (Longitudinal) | `13` | **`PRIVATE HOLD (MoU)`** *(pending written ASER Centre pre-publication clearance)* | Held offline until ASER clearance |
| **`1A_4_NSSO71_Demand`** | Jan–Jun `2014` (**RTE-Endpoint Cross-Section**) | `12` | **`LIVE NOW`** | [`data_aggregated/1A_4_NSSO71_Demand/1A4_DATA_DICTIONARY.md`](../data_aggregated/1A_4_NSSO71_Demand/1A4_DATA_DICTIONARY.md) *(+ `.csv`)* |
| **`1A_5_AISHE_HigherEd`** | `AY 2011–12` to `2023–24` (Longitudinal) | `12` | **`LIVE NOW`** | [`data_aggregated/1A_5_AISHE_HigherEd/1A5_DATA_DICTIONARY.md`](../data_aggregated/1A_5_AISHE_HigherEd/1A5_DATA_DICTIONARY.md) *(+ `.csv`)* |
| **`1A_6_PLFS_Labour`** | `2009–10` to `2025`, 6 Waves (Longitudinal) | `12` | **`LIVE NOW`** | [`data_aggregated/1A_6_PLFS_Labour/1A6_DATA_DICTIONARY.md`](../data_aggregated/1A_6_PLFS_Labour/1A6_DATA_DICTIONARY.md) *(+ `.csv`)* |

---

## 2. Universal Column Naming Conventions (Across All 6 Stage 1A Folders)

Every output CSV adheres to a predictable `prefix_stem_suffix` grammar so external researchers can infer units, weighting, signs, and denominators at a glance:

### A. Unit & Metric Suffixes
| Suffix | Meaning & Unit | Example Columns |
| :--- | :--- | :--- |
| `_cr` | Indian Rupees in **Crores** (`₹ 1 Crore = ₹ 10 Million`) | `consolidated_net_nominal_cr`, `real_cap_ed_2011base_cr` |
| `_rs` | Indian Rupees in **exact ₹** (not Crores) | `mean_tot_exp_rs`, `govt_grants_receipt_rs`, `fe_gap_tot_exp_rs` |
| `_millions` | Weighted population / worker estimate in **Millions** (`SUM(weight) / 1e6`) | `wgt_pop_millions`, `wgt_workers_millions`, `wgt_youth_pop_millions` |
| `_pct` | Percentage (`%`, scale `0–100`; check folder caveats for ratios `>100%` or stacked delta rows) | `capital_share_pct`, `psss_wpr_pct`, `ger_all` |
| `_pp` | **Percentage-point difference** (`End % − Start %`, or `Group A % − Group B %`) | `delta_psss_wpr_pp`, `ger_odl_and_nonreg_inflation_pp`, `fe_gap_pvt_unaided_pp` |
| `_ratio` / `_multiplier` | Dimensionless ratio (Numerator ÷ Denominator) | `se_to_he_ratio`, `english_fee_multiplier`, `net_exodus_ratio_g2p_over_p2g` |
| `_cagr_pct` | Compound Annual Growth Rate (`((End / Start)^(1/n) − 1) × 100`, in `%`) | `era_real_gdp_cagr_pct`, `enr_total_cagr_pct` |
| `_z` | Standardized composite stress / deprivation z-score (mean `0`, SD `1`; higher = more severe) | `composite_demand_stress_z`, `composite_tertiary_stress_z`, `composite_labour_stress_z` |
| `n_*` / `_cnt` | Unweighted raw sample count or census institution count | `n_unweighted`, `n_hh`, `n_youth_15_29`, `universities_count` |
| `_flag` / `_note` | Explicit binary (`0`/`1`) or text audit disclosure tag | `low_n_flag`, `revamped_2025_sample_flag`, `pop_projection_series_flag` |

### B. Analytical Prefixes & Domain Tokens
| Prefix / Token | Meaning | Where Used |
| :--- | :--- | :--- |
| `ps_*` vs. `psss_*` | Usual Principal Status (`PS`) vs. Usual Status (`PS+SS`, adding subsidiary-only workers) | `1A_6_PLFS_Labour` |
| `wrk_*` / `ss_*` | 5-bucket worker quality shares (`regular_salaried`, `own_account`, `employer`, `unpaid_helper`, `casual_labour`) | `1A_6_PLFS_Labour` |
| `nco_*` / `nic_*` | Occupation skill tiers (`NCO 1–3` High, `4–5` Mid, `6–9` Low) and industry sectors (`NIC-2008`) — anchored strictly on `PS` workers | `1A_6_PLFS_Labour` |
| `wgt_*` | Survey-multiplier weighted totals (`wgt_combined` in NSSO 71st; `final_weight` in PLFS/NSSO) | `1A_4_NSSO71_Demand`, `1A_6_PLFS_Labour` |
| `ger_*` / `gpi_*` / `ptr_*` | Gross Enrolment Ratio, Gender Parity Index, Pupil-Teacher Ratio | `1A_5_AISHE_HigherEd` |
| `gar_*` / `nar_*` | Gross Attendance Ratio, Net Attendance Ratio (household demand-side) | `1A_4_NSSO71_Demand` |
| `fe_gap_*` | Within-household Fixed-Effects difference (`Male − Female`; positive = boys receive more) | `1A_4_NSSO71_Demand`, `1A_3_ASER_Ground_Truth` *(post-clearance)* |
| `delta_*` / `DiD_*` | Era-to-era transition change or Difference-in-Differences (`ΔADP − ΔNonADP`) | `1A_2`, `1A_3`, `1A_5`, `1A_6` |

---

## 3. Eight Cross-Cutting Rules External Users Must Follow

1. **Never Sum Across `section`, `geo_level`, `rollup_type`, `slice_type`, or `All India` Rows:** Multi-part CSVs stack national benchmarks, zone/typology rollups, and state/district slices in one file. Always filter to a single `section` and `geo_level` / `slice_type` before aggregating.
2. **Never Pool `Actual`, `RE`, and `BE` Silently (`1A_1`):** Always filter on `data_basis` / `union_basis` / `rbi_basis`.
3. **Respect Cross-Section vs. Longitudinal Designs:** `1A_4` (NSSO 71st) is strictly a **2014 RTE-Endpoint Cross-Section**; `1A_1`, `1A_2`, `1A_3`, `1A_5`, and `1A_6` are multi-era longitudinal panels.
4. **Apply Thin-Cell Gating (`low_n_flag = 1`):** In `1A_4` and `1A_6`, `low_n_flag = 1` marks unweighted cells with `n < 30`. District stress rankings already mask and exclude `n < 30` domains (`402/639` districts qualify in `1A_4`; `742/764` qualify in `1A_6`).
5. **Respect `1A_4` Schema Safeguards:** `hostel_exp` (`1–35`) is a destination State/UT code (not rupees; excluded from wallet burden), and `nature_inst` covers strictly `General + Private Unaided`.
6. **Respect `1A_5` Population Break & Provenance Locks:** `2021–22+` GER uses MoE's revised `2020` `18–23` population projection (`pop_projection_series_flag = 'Revised_2020_Projection_Break'`). Every AISHE metric pulls from a single authoritative `(source_table, response_type)` with zero multi-year series (`T-40..T-52`) double counting.
7. **Respect `1A_6` `PS` vs. `PS+SS` & `2025` Frame Rules:** `2025` carries `revamped_2025_sample_flag = 1` (`270,472` HHs); `NCO`/`NIC` shares anchor strictly on `PS` workers while 5-bucket job quality covers `100%` of `PS+SS` workers; Gig and Orange economy rows are `NIC-2008` (`2011–25`) structural proxies.
8. **Respect `1A_1` / `1A_2` Valid Out-of-Range Ratios & Portal Scope Caveats:** Check folder dictionaries before flagging `>100%` utilization/loan/apparent-transition ratios, negative carry-over unspent grants, `4` tagged 2014–15 Karnataka PUC repeater rows, or `cyber_safety_2025_pct` (Govt/Aided-only portal scope).

