# Aggregated Output CSV Naming Conventions & Dictionary Index

**Repository:** `Akash-Chature/indian-education-pipeline-2009-2026`  
**Scope:** Master guide to column naming grammar, unit conventions, and usage rules across all aggregated analytical CSVs published in `data_aggregated/`.  
**Source Schemas:** For the underlying six-dataset database architecture and survey weighting formulas, see [`data_dictionary.md`](data_dictionary.md).

---

## 1. Directory Navigation & Data Dictionaries

| Directory (`data_aggregated/`) | Temporal Design | Published CSVs | Folder Manifest | Column Data Dictionary |
| :--- | :--- | :---: | :--- | :--- |
| **`1A_1_Macro_Financial`** | FY `2009–10` to `2025–26` (Longitudinal; 4-ministry to `2026–27` BE) | `10` | [`README.md`](../data_aggregated/1A_1_Macro_Financial/README.md) | [`1A1_DATA_DICTIONARY.md`](../data_aggregated/1A_1_Macro_Financial/1A1_DATA_DICTIONARY.md) ([`.csv`](../data_aggregated/1A_1_Macro_Financial/1A1_DATA_DICTIONARY.csv)) |
| **`1A_4_NSSO71_Demand`** | Jan–Jun `2014` (Cross-Section) | `12` | [`README.md`](../data_aggregated/1A_4_NSSO71_Demand/README.md) | [`1A4_DATA_DICTIONARY.md`](../data_aggregated/1A_4_NSSO71_Demand/1A4_DATA_DICTIONARY.md) ([`.csv`](../data_aggregated/1A_4_NSSO71_Demand/1A4_DATA_DICTIONARY.csv)) |
| **`1A_5_AISHE_HigherEd`** | AY `2011–12` to `2023–24` (Longitudinal, 7 waves) | `12` | [`README.md`](../data_aggregated/1A_5_AISHE_HigherEd/README.md) | [`1A5_DATA_DICTIONARY.md`](../data_aggregated/1A_5_AISHE_HigherEd/1A5_DATA_DICTIONARY.md) ([`.csv`](../data_aggregated/1A_5_AISHE_HigherEd/1A5_DATA_DICTIONARY.csv)) |
| **`1A_6_PLFS_Labour`** | `2009–10` to `2025` (Longitudinal, 6 waves) | `12` | [`README.md`](../data_aggregated/1A_6_PLFS_Labour/README.md) | [`1A6_DATA_DICTIONARY.md`](../data_aggregated/1A_6_PLFS_Labour/1A6_DATA_DICTIONARY.md) ([`.csv`](../data_aggregated/1A_6_PLFS_Labour/1A6_DATA_DICTIONARY.csv)) |

*Additional Modules:* Longitudinal school supply tables (`UDISE+`, `2014–15` to `2025–26`) are forthcoming pending historical `2009–12` NIEPA archive verification. Rural cognitive and school-observation aggregates (`ASER`, `2009–2024`) are held offline pending formal pre-publication review under the ASER Centre Research MoU.

---

## 2. Universal Column Naming Conventions

Every analytical column follows a consistent `prefix_stem_suffix` convention so units, weighting, signs, and denominators are immediately clear.

### A. Unit & Metric Suffixes
| Suffix | Meaning & Unit | Example Columns |
| :--- | :--- | :--- |
| `_cr` | Indian Rupees in Crores (`₹1 Crore = ₹10 Million`) | `consolidated_net_nominal_cr`, `real_cap_ed_2011base_cr` |
| `_rs` | Indian Rupees in exact `₹` (not Crores) | `mean_tot_exp_rs`, `fe_gap_tot_exp_rs` |
| `_millions` | Survey-weighted headcount in Millions (`SUM(weight) / 1,000,000`) | `wgt_pop_millions`, `wgt_workers_millions` |
| `_pct` | Percentage (`%`, generally `0–100`; see ratio notes below) | `capital_share_pct`, `psss_wpr_pct`, `coaching_incidence_pct` |
| `_pp` | Percentage-point difference (`End % − Start %`, or `Group A % − Group B %`) | `delta_psss_wpr_pp`, `ger_odl_and_nonreg_inflation_pp`, `fe_gap_pvt_unaided_pp` |
| `_ratio` / `_multiplier` | Dimensionless ratio (`Numerator ÷ Denominator`) | `reco_ratio_2202_over_4202`, `english_fee_multiplier`, `net_exodus_ratio_g2p_over_p2g` |
| `_cagr_pct` | Compound Annual Growth Rate (`((End / Start)^(1/n) − 1) × 100`, in `%`) | `era_real_gdp_cagr_pct`, `enr_total_cagr_pct` |
| `_z` | Standardized composite stress / deprivation z-score (mean `0`, SD `1`; higher = more severe) | `composite_demand_stress_z`, `composite_tertiary_stress_z`, `composite_labour_stress_z` |
| `n_*` / `*_count` | Unweighted raw sample observation count or census institution count | `n_unweighted`, `n_hh`, `n_youth_15_29`, `universities_count` |
| `*_flag` / `*_note` | Explicit binary (`0`/`1`) or text methodological disclosure tag | `low_n_flag`, `revamped_2025_sample_flag`, `pop_projection_series_flag` |

### B. Analytical Prefixes & Domain Tokens
| Prefix / Token | Meaning | Primary Directory |
| :--- | :--- | :--- |
| `ps_*` vs. `psss_*` | Usual Principal Status (`PS`) vs. Usual Principal + Subsidiary Status (`PS+SS`) | `1A_6_PLFS_Labour` |
| `wrk_*` / `ss_*` | Five-bucket employment quality shares (`regular_salaried`, `own_account`, `employer`, `unpaid_helper`, `casual_labour`) | `1A_6_PLFS_Labour` |
| `nco_*` / `nic_*` | Occupation skill tiers (`NCO 1–3` High, `4–5` Mid, `6–9` Low) and industry sectors (`NIC-2008`), anchored on `PS` workers | `1A_6_PLFS_Labour` |
| `wgt_*` | Survey-multiplier weighted population estimates (`wgt_combined` in NSSO 71st; `final_weight` in NSSO/PLFS) | `1A_4_NSSO71_Demand`, `1A_6_PLFS_Labour` |
| `ger_*` / `gpi_*` / `ptr_*` | Gross Enrolment Ratio, Gender Parity Index, Pupil-Teacher Ratio | `1A_5_AISHE_HigherEd` |
| `gar_*` / `nar_*` | Gross Attendance Ratio, Net Attendance Ratio (household demand-side) | `1A_4_NSSO71_Demand` |
| `fe_gap_*` | Within-household Fixed-Effects gap (`Male − Female`; positive = higher value for sons) | `1A_4_NSSO71_Demand` |
| `delta_*` / `DiD_*` | Era-to-era transition change or Difference-in-Differences (`ΔADP − ΔNonADP`) | `1A_5_AISHE_HigherEd`, `1A_6_PLFS_Labour` |

---

## 3. Essential Rules for External Users

1. **Always Filter Multi-Section & Multi-Level Tables:** Never sum blindly across `section`, `geo_level`, `slice_type`, or `All India` rows within a single CSV. Filter to one `section` and one `geo_level` / `slice_type` before aggregating or plotting.
2. **Never Pool `Actual`, `RE`, and `BE` Silently (`1A_1_Macro_Financial`):** Always inspect and filter on `data_basis`, `union_basis`, or `rbi_basis` before computing growth rates or regressions.
3. **Respect Cross-Section vs. Longitudinal Designs:** `1A_4_NSSO71_Demand` is strictly a January–June `2014` cross-section; `1A_1_Macro_Financial`, `1A_5_AISHE_HigherEd`, and `1A_6_PLFS_Labour` are multi-era longitudinal panels.
4. **Apply Thin-Cell Gating (`low_n_flag = 1`):** In household and labour tables (`1A_4` and `1A_6`), `low_n_flag = 1` marks unweighted sample cells with `n < 30`. District composite rankings automatically mask and exclude `n < 30` domains (`402/639` districts qualify in `1A_4`; `742/764` qualify in `1A_6`).
5. **Respect Blank (`NULL`) Semantics:** Blank cells indicate non-applicable sub-sections, schemes not yet active in a given year, unreported denominators, or thin-cell suppression—never treat blank cells as zero (`0`). Exact blank counts and reasons are documented in every folder's `*_DATA_DICTIONARY.md`.
6. **Check Domain-Specific Structural Disclosures:**
   * `1A_1_Macro_Financial`: `academic_year` stores Indian Financial Years (April–March); ratios such as `utilization_pct_actual_vs_be`, `loans_6202_to_cap_4202_pct`, and `reco_ratio_2202_over_4202` validly exceed `100%`.
   * `1A_4_NSSO71_Demand`: `hostel_exp` (`1–35`) is a destination State/UT code (not rupees); `nature_inst` covers strictly General Private Unaided students.
   * `1A_5_AISHE_HigherEd`: `2021–22` onward reflects MoE's revised `2020` `18–23` population projection (`pop_projection_series_flag`); Foreign Student and Out-turn tables omit mode disaggregation.
   * `1A_6_PLFS_Labour`: `2025` carries `revamped_2025_sample_flag = 1`; `NCO`/`NIC` shares anchor strictly on `PS` workers; Gig-Exposed and Orange Economy columns are reproducible `NIC-2008` (`2011–25`) structural proxies; always pair `canonical_district_name` with `canonical_state_name`.
