# Data Dictionary — Stage 1A.1: Macro-Financial Outputs (`1A_1_Macro_Financial`)

**Release Gate:** `PUBLIC_GITHUB_READY` (Live on GitHub)  
**Scope:** `10` Certified CSVs | `192` Documented Columns | `FY 2009–10` to `FY 2025–26` (plus `FY 2026–27 BE` in SBE ledgers)  
**Audit Status:** Certified PASS (Nov 23, 2026 — `172` IEEE-754 float-noise cells scrubbed with zero metric drift)  
**Primary Sources:** Union Budget Demand for Grants & Statement of Budget Estimates (MoE School & Higher Education, MSDE, NITI AIM, DST — Net of `PSK`/`MUSK` Recoveries), RBI *State Finances: A Study of Budgets* (`31` States/UTs with Legislatures), and MoSPI NAS GDP / CPI Deflators.

---

## 1. Universal Fiscal Rules, Formulas & Structural Caveats

Read these six accounting rules before aggregating, joining, or plotting any table in `1A_1_Macro_Financial/`:

1. **`academic_year` Represents Indian Financial Year (April–March):** All years in this folder (`2009-10` to `2026-27`) are Indian Financial Years (`FY`). The column is named `academic_year` strictly to guarantee 1:1 join compatibility with UDISE+ and AISHE.
2. **Net Consolidation Formula (Zero Intergovernmental Double-Counting):**
   - `union_direct_net_cr = union_net_total_cr − union_grants_3601_3602_cr`
   - `consolidated_net_nominal_cr = union_direct_net_cr + rbi_31state_ed_total_cr`
   - `centre_direct_share_pct + state_execution_share_pct = 100%` (strictly additive execution split). By contrast, `centre_gross_budget_share_pct = union_net_total_cr / consolidated_net_nominal_cr × 100` includes `3601`/`3602` grants routed to States and must **not** be added to `state_execution_share_pct`.
3. **Reserve Fund Recovery Netting (`PSK` / `MUSK`):** `psk_musk_recoveries_cr` (`<= 0`) deducts Education Cess transfers routed through *Prarambhik Shiksha Kosh* (`PSK`) and *Madhyamik & Uchchtar Shiksha Kosh* (`MUSK`) from `gross_outlay_cr` to produce `net_dept_total_cr`. Gross distortion is defined as `gross_over_net_distortion_pct = (gross_outlay_cr − net_dept_total_cr) / net_dept_total_cr × 100`.
4. **Real Deflation & Best-Available Waterfall (`*_best_*`):**
   - All real series follow `real_cr = nominal_cr / (deflator / 100)` using spliced National GDP Deflators (`2011–12 = 100` or `2022–23 = 100`) or MoSPI CPI-Combined (`2011–12 = 100`).
   - In `1A1_digital_skilling_rd_2009_18_vs_2020_26_pivot.csv`, `*_best_nom_cr` selects **`Actual > RE > BE`** (`COALESCE(Actual, RE, BE)`)—whichever is the most final figure available for that FY.
5. **Ratios That Validly Exceed `100%` or Skew High (Do Not Flag as Errors):**
   - `utilization_pct_actual_vs_be` (`Actual / BE × 100`, up to `201.33%` in external ministries and `311.89%` in MoE schemes): Exceeds `100%` whenever mid-year supplementary grants are approved.
   - `slippage_pct_re_vs_be` (`(RE − BE) / BE × 100`): Negative = mid-year budget cut (`-100%` = zeroed out); positive = upward revision (extreme spikes reflect nominal token `BE` allocations funded at `RE`).
   - `loans_6202_to_cap_4202_pct` (`Loans_6202 / CapEx_4202 × 100`, up to `302.51%`): Unbounded ratio comparing student-loan disbursements (`6202`) to physical capital outlay (`4202`), not a bounded percentage share.
   - `reco_ratio_2202_over_4202` (`Revenue_2202 / Capital_4202`, up to `34,070.22`): Spikes steeply whenever a state records near-zero capital outlay (`4202`).
6. **Structural Breaks & Basis Hygiene:**
   - Always check `structural_break_flag` and `treasury_routing_note`: pre-`2014–15` SSA/MDM funds went directly to State Implementing Societies under Central Major Head `2202`, whereas post-`2014–15` Centrally Sponsored Schemes route via State Treasuries under `3601`. Pre-`2017` Plan/Non-Plan mapping (`total_capital_expenditure_cr` = Plan) cannot be compared directly to post-`2017` Capital Outlay (`4202`).
   - Never pool `Actual`, `RE` (Revised Estimate), `BE` (Budget Estimate), and `Actual/RE` silently; always filter on `data_basis` / `union_basis` / `rbi_basis`.
   - **Era Boundary Note (Stage 1B Cross-Lakehouse Alignment):** `1A_1` assigns `FY 2014–15` to Era 2 and `FY 2023–24` to Era 4, whereas `1A_2` (academic years) assigns `AY 2014–15` to Era 1 End and `AY 2023–24` to Era 3B (SDMS break).

---

## 2. File-by-File Column Dictionary (All 10 CSVs)

### `1A1_national_consolidated_net_series.csv`
- **Rows:** `10` | **Columns:** `29` | **Temporal Scope:** FY 2009-10 to 2025-26 (10 waves)
- **Purpose:** 10-wave consolidated National (Union MoE + 31 States/UTs with Legislatures) net public education expenditure, deflators, % of GDP, and Centre–State shares.

| # | Column Name | Type | Nulls | Observed Range / Values | Definition, Formula & Caveats |
| :---: | :--- | :---: | :---: | :--- | :--- |
| 1 | `academic_year` | `string` | 0 | 10 waves: 2009-10 to 2025-26 | Indian Financial Year (April–March); named `academic_year` strictly for join compatibility with UDISE+ and AISHE. Primary key. |
| 2 | `era` | `category` | 0 | 4 Chronological Policy Eras | Chronological policy era classification (`RTE Era (2009-2014)`, `Pre-Covid Benchmark (2014-2019)`, `Post-Covid & Pre-NEP (2020-2023)`, `Post-NEP Era (2024-2026)`). FY boundary rule: FY 2014-15 = Era 2; FY 2023-24 = Era 4. |
| 3 | `union_basis` | `category` | 0 | RE, Actual | Accounting basis of Union MoE budget figures for the wave. Do not silently pool `Actual` with `RE`. |
| 4 | `rbi_basis` | `category` | 0 | Actual/RE, Actual, RE, BE | Accounting basis of RBI State Finances figures (`Actual/RE` in 2009-10 reflects RE for J&K and Jharkhand per RBI footnotes). Do not silently pool `Actual` with `RE`/`BE`. |
| 5 | `rbi_states_count` | `int` | 0 | 30 .. 31 | Number of States & UTs with Legislatures in the RBI wave (`30` pre-Telangana 2009-10 to 2012-13; `31` from 2014-15 onward). Excludes 5 non-legislature UTs (+ Ladakh post-2019). |
| 6 | `union_se_net_cr` | `float` | 0 | 25,338.00 .. 70,567.14 | Union Dept. of School Education & Literacy net expenditure (₹ Crores, nominal). Net of PSK/MUSK reserve-fund recoveries. |
| 7 | `union_he_net_cr` | `float` | 0 | 14,389.00 .. 55,392.68 | Union Dept. of Higher Education net expenditure (₹ Crores, nominal). Net of MUSK reserve-fund recoveries. |
| 8 | `union_net_total_cr` | `float` | 0 | 39,727.00 .. 123,364.96 | Combined Union Ministry of Education net expenditure (₹ Crores, nominal). `union_se_net_cr + union_he_net_cr`. |
| 9 | `rbi_31state_rev_2202_cr` | `float` | 0 | 155,773.08 .. 853,790.91 | Aggregated State Revenue Expenditure on Education, Sports, Art & Culture across 30/31 entities (₹ Crores, nominal). RBI Appendix II, Major Head `2202-2205`. |
| 10 | `rbi_31state_cap_4202_cr` | `float` | 0 | 4,586.19 .. 54,540.80 | Aggregated State Capital Outlay on Education across 30/31 entities (₹ Crores, nominal). RBI Appendix IV (`4APP_1`), Major Head `4202` (excludes `6202` loans). |
| 11 | `rbi_31state_ed_total_cr` | `float` | 0 | 160,359.27 .. 908,331.71 | Combined State Revenue + Capital education expenditure (₹ Crores, nominal). `rbi_31state_rev_2202_cr + rbi_31state_cap_4202_cr`. |
| 12 | `union_grants_3601_3602_cr` | `float` | 0 | 1,660.75 .. 48,871.95 | Central Grants-in-Aid to States (`3601`) and UTs with Legislature (`3602`) routed to State Treasuries (₹ Crores, nominal). Netted out to prevent Centre–State double counting. |
| 13 | `union_direct_net_cr` | `float` | 0 | 31,535.27 .. 81,438.07 | Direct Central education expenditure retained/executed outside State Treasuries (₹ Crores, nominal). `union_net_total_cr - union_grants_3601_3602_cr`. |
| 14 | `consolidated_net_nominal_cr` | `float` | 0 | 198,425.52 .. 983,532.13 | Consolidated National (Centre + 31 States/UTs) net education spend with zero intergovernmental double-counting (₹ Crores, nominal). `union_direct_net_cr + rbi_31state_ed_total_cr`. |
| 15 | `gdp_deflator_2011base` | `float` | 0 | 84.54 .. 178.31 | Spliced National GDP Deflator index (`FY 2011-12 = 100`). Spliced across `2004-05 -> 2011-12 -> 2022-23` NAS series. |
| 16 | `gdp_deflator_2022base` | `float` | 0 | 50.82 .. 107.19 | Spliced National GDP Deflator index rebased to `FY 2022-23 = 100`. Used for modern-price real series. |
| 17 | `cpi_combined_2011base` | `float` | 0 | 83.59 .. 212.21 | MoSPI All-India CPI-Combined index (`2011-12 = 100`). Household purchasing-power deflator. |
| 18 | `consolidated_real_gdp_2011base_cr` | `float` | 0 | 234,718.94 .. 551,575.00 | Consolidated net spend in constant `2011-12` GDP prices (₹ Crores). `consolidated_net_nominal_cr / (gdp_deflator_2011base / 100)`. |
| 19 | `consolidated_real_gdp_2022base_cr` | `float` | 0 | 390,456.92 .. 917,550.18 | Consolidated net spend in constant `2022-23` GDP prices (₹ Crores). `consolidated_net_nominal_cr / (gdp_deflator_2022base / 100)`. |
| 20 | `consolidated_real_cpi_2011base_cr` | `float` | 0 | 237,380.35 .. 463,481.63 | Consolidated net spend in constant `2011-12` CPI prices (₹ Crores). `consolidated_net_nominal_cr / (cpi_combined_2011base / 100)`. |
| 21 | `nominal_gdp_or_nsdp_cr` | `float` | 0 | 6,477,827.00 .. 34,635,638.00 | Official published India Nominal GDP at current prices (₹ Crores). Denominator for `consolidated_pct_of_published_gdp`. |
| 22 | `consolidated_pct_of_published_gdp` | `float` | 0 | 2.69 .. 3.23 | Consolidated net public education spend as `%` of published nominal GDP. `consolidated_net_nominal_cr / nominal_gdp_or_nsdp_cr * 100`. |
| 23 | `consolidated_pct_of_spliced_2022base_gdp` | `float` | 0 | 2.76 .. 3.32 | Consolidated net public education spend as `%` of back-spliced `2022-23` base GDP series. Robustness check for GDP base-year revisions. |
| 24 | `centre_direct_share_pct` | `float` | 0 | 6.86 .. 19.93 | Direct Central execution share of consolidated net education spend (`%`). `union_direct_net_cr / consolidated_net_nominal_cr * 100`; sums with `state_execution_share_pct` to `100%`. |
| 25 | `centre_gross_budget_share_pct` | `float` | 0 | 12.28 .. 21.75 | Total Union MoE budget share (including `3601/3602` grants routed to States) of consolidated net spend (`%`). `union_net_total_cr / consolidated_net_nominal_cr * 100`; overlaps with state execution. |
| 26 | `state_execution_share_pct` | `float` | 0 | 80.07 .. 93.14 | State government execution share of consolidated net education spend (`%`). `rbi_31state_ed_total_cr / consolidated_net_nominal_cr * 100`; `100 - centre_direct_share_pct`. |
| 27 | `era_real_gdp_cagr_pct` | `float` | 0 | -1.33 .. 9.06 | Annualized real compound growth rate (`%`, GDP-deflated `2011-12` base) across each policy era window. Era-level summary repeated on wave rows. |
| 28 | `actual_only_2018_19_to_2023_24_real_cagr_pct` | `float` | 0 | 3.74 .. 3.74 | Strict Actuals-to-Actuals annualized real GDP-deflated CAGR (`2018-19 Actual` to `2023-24 Actual`, `%`). Constant reference benchmark (`3.74%`) isolating audited actuals. |
| 29 | `treasury_routing_note` | `category` | 0 | 2 structural routing regimes | Documents the `2014-15` accounting shift from direct society routing (`2202`) to State Treasury routing (`3601/3602`). Critical caveat when comparing pre- vs. post-2014 Central shares. |

### `1A1_union_structural_decomposition.csv`
- **Rows:** `20` | **Columns:** `24` | **Temporal Scope:** FY 2009-10 to 2025-26 (10 waves × 2 Depts)
- **Purpose:** Union Ministry of Education decomposition by department (`School Education` vs. `Higher Education`), Major Heads (`2202/2203/3601/3602`), and `PSK/MUSK` reserve-fund recovery netting.

| # | Column Name | Type | Nulls | Observed Range / Values | Definition, Formula & Caveats |
| :---: | :--- | :---: | :---: | :--- | :--- |
| 1 | `academic_year` | `string` | 0 | 10 waves: 2009-10 to 2025-26 | Indian Financial Year (April–March). Composite PK with `department`. |
| 2 | `department` | `category` | 0 | School Education, Higher Education | Union Ministry of Education department. Composite PK with `academic_year`. |
| 3 | `general_ed_2202_cr` | `float` | 0 | 5,707.61 .. 36,531.76 | Net Central Revenue outlay under Major Head `2202` (General Education: KVS, NVS, NCERT, UGC, CUs) in ₹ Crores. Pre-2014-15 includes society-routed SSA/MDM. |
| 4 | `technical_ed_2203_cr` | `float` | 0 | 0.00 .. 26,044.20 | Net Central Revenue outlay under Major Head `2203` (Technical Education: IITs, NITs, IIMs, IISERs, AICTE) in ₹ Crores. `0.00` for School Education. |
| 5 | `grants_in_aid_states_3601_cr` | `float` | 0 | 179.81 .. 47,023.73 | Net Central Grants-in-Aid to State Governments under Major Head `3601` (Samagra Shiksha, PM-POSHAN, RUSA) in ₹ Crores. Major structural jump in `2014-15` upon treasury routing shift. |
| 6 | `grants_in_aid_uts_3602_cr` | `float` | 0 | 0.00 .. 2,472.91 | Net Central Grants-in-Aid to UTs with Legislature under Major Head `3602` in ₹ Crores. Netted alongside `3601` in national consolidation. |
| 7 | `total_revenue_expenditure_cr` | `float` | 0 | 2,564.00 .. 70,566.34 | Total Net departmental revenue outlay (₹ Crores). Pre-2017 mapped from Net Non-Plan; Post-2017 = Net Revenue Section. See `structural_break_flag`. |
| 8 | `total_capital_expenditure_cr` | `float` | 0 | 0.00 .. 43,517.90 | Total Net departmental capital outlay (₹ Crores). Pre-2017 mapped from Net Plan (NOT comparable to post-2017 Capital `4202`). |
| 9 | `data_basis` | `category` | 0 | RE, Actual | Union budget accounting basis (`RE` for 2009-14 & 2025-26; `Actual` for 2018-25). Filter before computing actual-only trends. |
| 10 | `era` | `category` | 0 | 4 Policy Eras | Chronological policy era. FY era mapping. |
| 11 | `gdp_deflator_2011base` | `float` | 0 | 84.54 .. 178.31 | Spliced GDP Deflator (`2011-12 = 100`). Deflator for `net_dept_real_2011base_cr`. |
| 12 | `gdp_deflator_2022base` | `float` | 0 | 50.82 .. 107.19 | Spliced GDP Deflator (`2022-23 = 100`). Reference index. |
| 13 | `net_dept_total_cr` | `float` | 0 | 14,389.00 .. 70,567.14 | Net departmental total outlay (₹ Crores, nominal). `total_revenue_expenditure_cr + total_capital_expenditure_cr` = `gross_outlay_cr + psk_musk_recoveries_cr`. |
| 14 | `net_dept_real_2011base_cr` | `float` | 0 | 17,020.85 .. 42,194.15 | Net departmental outlay in constant `2011-12` GDP prices (₹ Crores). `net_dept_total_cr / (gdp_deflator_2011base / 100)`. |
| 15 | `dept_share_of_union_moe_pct` | `float` | 0 | 31.84 .. 68.16 | Department's share of total Union MoE net budget (`%`). `net_dept_total_cr / (SE + HE) * 100`. |
| 16 | `se_to_he_ratio` | `float` | 0 | 1.23 .. 2.14 | Ratio of School Education net outlay to Higher Education net outlay within the same FY. `union_se_net_cr / union_he_net_cr`. |
| 17 | `share_2202_pct` | `float` | 0 | 12.19 .. 85.06 | Major Head `2202` share of `net_dept_total_cr` (`%`). `general_ed_2202_cr / net_dept_total_cr * 100`. |
| 18 | `share_2203_pct` | `float` | 0 | 0.00 .. 49.35 | Major Head `2203` share of `net_dept_total_cr` (`%`). `technical_ed_2203_cr / net_dept_total_cr * 100`. |
| 19 | `share_3601_pct` | `float` | 0 | 0.32 .. 80.14 | Major Head `3601` share of `net_dept_total_cr` (`%`). `grants_in_aid_states_3601_cr / net_dept_total_cr * 100`. |
| 20 | `share_3602_pct` | `float` | 0 | 0.00 .. 3.50 | Major Head `3602` share of `net_dept_total_cr` (`%`). `grants_in_aid_uts_3602_cr / net_dept_total_cr * 100`. |
| 21 | `gross_outlay_cr` | `float` | 0 | 14,476.08 .. 129,541.14 | Gross Demand for Grants outlay prior to reserve-fund recovery netting (₹ Crores). Double-counts Education Cess transfers into and out of PSK/MUSK. |
| 22 | `psk_musk_recoveries_cr` | `float` | 0 | -58,974.00 .. 0.00 | Negative adjustment for Prarambhik Shiksha Kosh (PSK) & Madhyamik/Uchchtar Shiksha Kosh (MUSK) recoveries (₹ Crores). `<= 0`; `net_dept_total_cr = gross_outlay_cr + psk_musk_recoveries_cr`. |
| 23 | `gross_over_net_distortion_pct` | `float` | 0 | 0.00 .. 83.57 | Percentage inflation if gross Demand totals are cited instead of Net-of-Recoveries totals (`%`). `(gross_outlay_cr - net_dept_total_cr) / net_dept_total_cr * 100`. |
| 24 | `structural_break_flag` | `category` | 0 | 5 accounting break flags | Explicit warning tag flagging Pre-2017 Plan/Non-Plan mapping, `2014-15` `2202 -> 3601` routing shift, and Higher Ed `3601` floor. Must be respected in time-series charts. |

### `1A1_state_31_revenue_capital_matrix.csv`
- **Rows:** `307` | **Columns:** `32` | **Temporal Scope:** FY 2009-10 to 2025-26 (30/31 States × 10 waves)
- **Purpose:** Core 31-state fiscal panel tracking nominal & real Revenue (`2202`) vs. Capital (`4202`) education spend, `% of State NSDP`, capital share ranks, and multi-era CAGRs.

| # | Column Name | Type | Nulls | Observed Range / Values | Definition, Formula & Caveats |
| :---: | :--- | :---: | :---: | :--- | :--- |
| 1 | `academic_year` | `string` | 0 | 10 waves: 2009-10 to 2025-26 | Indian Financial Year (April–March). Composite PK with `state_name`. |
| 2 | `state_name` | `string` | 0 | 31 States/UTs with Legislature | Canonical State/UT name (`NCT of Delhi` normalized to `Delhi`; 30 pre-Telangana, 31 post-2014-15). 100% join-compatible across lakehouses. |
| 3 | `state_revenue_ed_cr` | `float` | 0 | 408.68 .. 105,473.63 | State Revenue Expenditure on Education, Sports, Art & Culture (₹ Crores, nominal). RBI Appendix II, Major Head `2202-2205`. |
| 4 | `state_capital_ed_cr` | `float` | 0 | 0.00 .. 8,735.50 | State Capital Outlay on Education (₹ Crores, nominal). RBI Appendix IV, Major Head `4202` (excludes `6202` loans). |
| 5 | `total_social_sector_cr` | `float` | 0 | 911.92 .. 273,560.48 | Total State Social Sector expenditure (Revenue + Capital Social Services, ₹ Crores, nominal). Denominator for social-sector share. |
| 6 | `data_basis` | `category` | 0 | Actual, RE, BE | RBI accounting basis (`Actual` 2009-24 except J&K/Jharkhand 2009-10 `RE`; `RE` 2024-25; `BE` 2025-26). Filter `data_basis = 'Actual'` for audited comparisons. |
| 7 | `era` | `category` | 0 | 4 Policy Eras | Chronological policy era. FY era mapping. |
| 8 | `zone` | `category` | 0 | 6 Administrative Zones | Administrative zone (`North`, `South`, `East`, `West`, `Central`, `North-East`). Unified taxonomy across all Stage 1A lakehouses. |
| 9 | `demographic_group` | `category` | 0 | 4 Demographic-Fiscal Typologies | Structural state typology (`Youthful / High-Fertility EAG`, `Ageing / Low-TFR South + Goa`, `High-GSDP Industrial / Metro & Other`, `North-East & Hill Special Category`). Unified taxonomy across Stage 1A. |
| 10 | `state_nsdp_current_cr` | `float` | 17 | 4,717.00 .. 4,484,537.00 | State Net State Domestic Product (NSDP) at current prices (₹ Crores). `17` NULLs where MoSPI/RBI has not yet published NSDP (e.g., `2025-26` or select NE/UT cells). |
| 11 | `state_nsdp_constant_cr` | `float` | 17 | 3,659.00 .. 2,505,899.00 | State NSDP at constant prices (₹ Crores). `17` NULLs matching `state_nsdp_current_cr`. |
| 12 | `gdp_deflator_2011base` | `float` | 0 | 84.54 .. 178.31 | Spliced National GDP Deflator (`2011-12 = 100`). Used to compute `real_*_2011base_cr`. |
| 13 | `gdp_deflator_2022base` | `float` | 0 | 50.82 .. 107.19 | Spliced National GDP Deflator (`2022-23 = 100`). Reference deflator. |
| 14 | `cpi_combined_2011base` | `float` | 0 | 83.59 .. 212.21 | MoSPI CPI-Combined index (`2011-12 = 100`). Reference deflator. |
| 15 | `nsdp_note` | `category` | 0 | 4 NSDP base/continuity notes | Documents NSDP base year (`2004-05` vs `2011-12`), Unified AP+TS pre-bifurcation handling, and `2025-26` NULL status. Transparency metadata. |
| 16 | `state_total_ed_cr` | `float` | 0 | 436.16 .. 106,338.49 | Combined State Revenue + Capital education spend (₹ Crores, nominal). `state_revenue_ed_cr + state_capital_ed_cr`. |
| 17 | `real_rev_ed_2011base_cr` | `float` | 0 | 462.98 .. 59,150.70 | State Revenue education spend in constant `2011-12` GDP prices (₹ Crores). `state_revenue_ed_cr / (gdp_deflator_2011base / 100)`. |
| 18 | `real_cap_ed_2011base_cr` | `float` | 0 | 0.00 .. 4,898.96 | State Capital education outlay in constant `2011-12` GDP prices (₹ Crores). `state_capital_ed_cr / (gdp_deflator_2011base / 100)`. |
| 19 | `real_total_ed_2011base_cr` | `float` | 0 | 477.41 .. 59,635.73 | Total State education spend in constant `2011-12` GDP prices (₹ Crores). `state_total_ed_cr / (gdp_deflator_2011base / 100)`. |
| 20 | `capital_share_pct` | `float` | 0 | 0.00 .. 25.21 | Capital outlay share of total state education spend (`%`). `state_capital_ed_cr / state_total_ed_cr * 100`. |
| 21 | `reco_ratio_2202_over_4202` | `float` | 1 | 2.97 .. 34,070.22 | Ratio of Revenue (`2202`) to Capital (`4202`) expenditure. `state_revenue_ed_cr / state_capital_ed_cr`; `1` NULL where CapEx = 0; spikes validly when CapEx is near zero. |
| 22 | `ed_spend_pct_of_nsdp` | `float` | 17 | 1.37 .. 11.39 | Total state education expenditure as `%` of current-price State NSDP. `state_total_ed_cr / state_nsdp_current_cr * 100`. |
| 23 | `rev_ed_pct_of_nsdp` | `float` | 17 | 1.32 .. 11.27 | State revenue education expenditure as `%` of current-price State NSDP. `state_revenue_ed_cr / state_nsdp_current_cr * 100`. |
| 24 | `cap_ed_pct_of_nsdp` | `float` | 17 | 0.00 .. 2.70 | State capital education outlay as `%` of current-price State NSDP. `state_capital_ed_cr / state_nsdp_current_cr * 100`. |
| 25 | `cap_share_rank_in_wave` | `float` | 0 | 1.00 .. 31.00 | Within-wave rank of `capital_share_pct` (`1` = highest capital share in that FY). Rank computed within each `academic_year`. |
| 26 | `cap_share_tier` | `category` | 0 | Top 5 / Mid-Tier / Bottom 5 | Capital-allocation tier within each wave (`Top 5 Capital Share`, `Mid-Tier`, `Bottom 5 Capital Share`). Derived from `cap_share_rank_in_wave`. |
| 27 | `cagr_real_actual_2009_10_to_2014_15_pct` | `float` | 7 | 0.01 .. 15.14 | State-level annualized real total spend CAGR (`FY 2009-10` to `2014-15`, `%`). `7` NULLs for Telangana (formed 2014). |
| 28 | `cagr_real_actual_2014_15_to_2019_20_pct` | `float` | 0 | 1.19 .. 12.78 | State-level annualized real total spend CAGR (`FY 2014-15` to `2019-20`, `%`). Pre-COVID 5-year benchmark. |
| 29 | `cagr_real_actual_2018_19_to_2023_24_pct` | `float` | 0 | -2.97 .. 7.10 | State-level annualized real total spend CAGR (`FY 2018-19` to `2023-24`, `%`). Strict Actuals-to-Actuals 5-year post-2018 trend. |
| 30 | `cagr_real_actual_2009_10_to_2023_24_pct` | `float` | 7 | 2.04 .. 8.45 | State-level 14-year real total spend CAGR (`FY 2009-10` to `2023-24 Actual`, `%`). `7` NULLs for Telangana. |
| 31 | `cagr_real_cap_actual_2014_15_to_2023_24_pct` | `float` | 10 | -11.67 .. 46.31 | State-level 9-year real Capital Outlay (`4202`) CAGR (`FY 2014-15` to `2023-24 Actual`, `%`). `10` NULLs where endpoint CapEx = 0. |
| 32 | `cagr_real_proj_2023_24_to_2025_26_BE_pct` | `float` | 0 | 2.31 .. 19.04 | Projected 2-year real total spend CAGR (`2023-24 Actual` to `2025-26 BE`, `%`). Budget-estimate projection; do not mix with audited CAGRs. |

### `1A1_social_sector_crowding_out.csv`
- **Rows:** `317` | **Columns:** `13` | **Temporal Scope:** FY 2009-10 to 2025-26 (All 31 + 31 States × 10 waves)
- **Purpose:** Education's share of State Social Sector budgets across all 31 States/UTs plus the `All 31 States & UTs` aggregate, with percentage-point crowding-out deltas.

| # | Column Name | Type | Nulls | Observed Range / Values | Definition, Formula & Caveats |
| :---: | :--- | :---: | :---: | :--- | :--- |
| 1 | `academic_year` | `string` | 0 | 10 waves: 2009-10 to 2025-26 | Indian Financial Year (April–March). Composite PK with `state_name`. |
| 2 | `era` | `category` | 0 | 4 Policy Eras | Chronological policy era. FY era mapping. |
| 3 | `state_name` | `string` | 0 | 32 unique (All 31 + 31 States) | State/UT name plus `All 31 States & UTs` national state-aggregate row. Filter `state_name != 'All 31 States & UTs'` before summing states. |
| 4 | `zone` | `category` | 0 | All India + 6 Zones | Administrative zone. Standard taxonomy. |
| 5 | `demographic_group` | `category` | 0 | All India + 4 Typologies | Demographic-fiscal typology. Standard taxonomy. |
| 6 | `data_basis` | `category` | 0 | Actual/RE, Actual, RE, BE | RBI accounting basis. Filter to `Actual` for audited trends. |
| 7 | `state_total_ed_cr` | `float` | 0 | 436.16 .. 908,331.71 | State total education spend (`2202 + 4202`, ₹ Crores, nominal). Numerator. |
| 8 | `total_social_sector_cr` | `float` | 0 | 911.92 .. 2,530,551.60 | Total State Social Sector expenditure (Revenue + Capital, ₹ Crores, nominal). Denominator. |
| 9 | `ed_share_of_social_sector_pct` | `float` | 0 | 19.53 .. 60.49 | Education's share of State Social Sector budget (`%`). `state_total_ed_cr / total_social_sector_cr * 100`. |
| 10 | `pp_chg_2009_10_to_2023_24_actual` | `float` | 7 | -16.00 .. 9.62 | Percentage-point shift in `ed_share_of_social_sector_pct` from `2009-10` to `2023-24 Actual` (`pp`). Negative = education crowded out by other social schemes; `7` NULLs for Telangana. |
| 11 | `pp_chg_2014_15_to_2023_24_actual` | `float` | 0 | -14.16 .. 4.01 | Percentage-point shift from `2014-15 Actual` to `2023-24 Actual` (`pp`). Post-Telangana 31-state complete span. |
| 12 | `pp_chg_2009_10_to_2025_26_be` | `float` | 7 | -21.96 .. 2.14 | Percentage-point shift from `2009-10` to `2025-26 BE` (`pp`). Includes budget estimates; `7` NULLs for Telangana. |
| 13 | `pp_chg_2014_15_to_2025_26_be` | `float` | 0 | -19.41 .. 0.91 | Percentage-point shift from `2014-15 Actual` to `2025-26 BE` (`pp`). Includes budget estimates. |

### `1A1_zone_and_demographic_rollups.csv`
- **Rows:** `100` | **Columns:** `22` | **Temporal Scope:** FY 2009-10 to 2025-26 (10 groups × 10 waves)
- **Purpose:** Aggregated RBI State Finances rollups across 6 Administrative Zones and 4 Demographic-Fiscal Typologies with explicit NSDP coverage guards.

| # | Column Name | Type | Nulls | Observed Range / Values | Definition, Formula & Caveats |
| :---: | :--- | :---: | :---: | :--- | :--- |
| 1 | `rollup_type` | `category` | 0 | Administrative Zone, Demographic-Fiscal Typology | Grouping axis (`6` Zones vs. `4` Typologies). Filter to ONE `rollup_type` before summing to avoid 2× double counting. |
| 2 | `academic_year` | `string` | 0 | 10 waves: 2009-10 to 2025-26 | Indian Financial Year (April–March). Composite PK with `rollup_type`, `group_name`. |
| 3 | `era` | `category` | 0 | 4 Policy Eras | Chronological policy era. FY era mapping. |
| 4 | `group_name` | `string` | 0 | 10 groups (6 Zones + 4 Typologies) | Zone or Demographic-Fiscal group name. Composite PK. |
| 5 | `states_in_group` | `int` | 0 | 3 .. 10 | Number of RBI-covered states/UTs inside the group in that wave. Varies by +1 in South after Telangana formation (2014-15). |
| 6 | `states_with_nsdp` | `int` | 0 | 1 .. 10 | Count of states in the group with non-null published NSDP in that wave. Matched denominator count for `ed_pct_of_group_nsdp`. |
| 7 | `data_basis` | `category` | 0 | Actual, RE, BE, Actual/RE | RBI accounting basis of the group rollup. `Actual/RE` appears in 2009-10 groups containing J&K or Jharkhand. |
| 8 | `nominal_rev_2202_cr` | `float` | 0 | 7,886.39 .. 351,705.06 | Group sum of nominal State Revenue education spend (`2202`, ₹ Crores). `SUM(state_revenue_ed_cr)`. |
| 9 | `nominal_cap_4202_cr` | `float` | 0 | 322.65 .. 29,641.71 | Group sum of nominal State Capital education outlay (`4202`, ₹ Crores). `SUM(state_capital_ed_cr)`. |
| 10 | `nominal_total_ed_cr` | `float` | 0 | 8,263.87 .. 381,346.77 | Group sum of nominal total education spend (`2202 + 4202`, ₹ Crores). `nominal_rev_2202_cr + nominal_cap_4202_cr`. |
| 11 | `real_rev_2202_2011base_cr` | `float` | 0 | 9,328.85 .. 197,239.84 | Group real Revenue education spend (`2011-12` GDP prices, ₹ Crores). `nominal_rev_2202_cr / (gdp_deflator_2011base / 100)`. |
| 12 | `real_cap_4202_2011base_cr` | `float` | 0 | 322.65 .. 16,623.38 | Group real Capital education outlay (`2011-12` GDP prices, ₹ Crores). `nominal_cap_4202_cr / (gdp_deflator_2011base / 100)`. |
| 13 | `real_total_ed_2011base_cr` | `float` | 0 | 9,775.40 .. 213,863.23 | Group real total education spend (`2011-12` GDP prices, ₹ Crores). `nominal_total_ed_cr / (gdp_deflator_2011base / 100)`. |
| 14 | `total_social_sector_cr` | `float` | 0 | 17,615.54 .. 992,917.42 | Group sum of nominal State Social Sector spend (₹ Crores). Denominator for `ed_share_of_social_sector_pct`. |
| 15 | `group_nsdp_current_cr` | `float` | 0 | 44,129.00 .. 10,975,740.00 | Sum of current-price NSDP across states with non-null NSDP in the group (₹ Crores). Check `nsdp_coverage_flag` before comparing across years. |
| 16 | `ed_spend_for_valid_nsdp_cr` | `float` | 0 | 3,465.50 .. 356,891.34 | Sum of `state_total_ed_cr` restricted strictly to states with non-null NSDP in that group-wave (₹ Crores). Ensures exact numerator–denominator parity in `ed_pct_of_group_nsdp`. |
| 17 | `capital_share_pct` | `float` | 0 | 0.87 .. 10.18 | Group Capital Outlay share of total education spend (`%`). `nominal_cap_4202_cr / nominal_total_ed_cr * 100`. |
| 18 | `reco_ratio_2202_over_4202` | `float` | 0 | 8.82 .. 114.43 | Group Revenue-to-Capital ratio (`2202 / 4202`). `nominal_rev_2202_cr / nominal_cap_4202_cr`. |
| 19 | `ed_share_of_social_sector_pct` | `float` | 0 | 30.15 .. 52.80 | Group education share of total State Social Sector spend (`%`). `nominal_total_ed_cr / total_social_sector_cr * 100`. |
| 20 | `ed_pct_of_group_nsdp` | `float` | 0 | 1.69 .. 7.85 | Group education spend as `%` of matched group NSDP. `ed_spend_for_valid_nsdp_cr / group_nsdp_current_cr * 100`. |
| 21 | `nsdp_coverage_flag` | `string` | 0 | Complete (100%) / Partial NSDP (x/y) | Explicit disclosure of NSDP completeness inside the group-wave. Highlights partial-NSDP waves transparently. |
| 22 | `share_of_national_state_ed_pct` | `float` | 0 | 5.06 .. 42.48 | Group's share of total All-31-States education expenditure (`%`). Sums to `100%` within each `(rollup_type, academic_year)`. |

### `1A1_fiscal_anomalies_and_shocks.csv`
- **Rows:** `42` | **Columns:** `9` | **Temporal Scope:** FY 2009-10 to 2024-25 (42 flagged events)
- **Purpose:** Forensic audit log of major state fiscal anomalies, 7th Pay Commission surges, COVID real contractions, `2024-25 RE` spikes, zero-CapEx rows, and Telangana continuity checks.

| # | Column Name | Type | Nulls | Observed Range / Values | Definition, Formula & Caveats |
| :---: | :--- | :---: | :---: | :--- | :--- |
| 1 | `anomaly_category` | `category` | 0 | 7 anomaly classes | Forensic classification of the shock or accounting exception (e.g., `2024-25 RE Extreme Outlay Spike`, `7th Pay Commission Real Revenue Surge`, `COVID Real Revenue Contraction`, `Zero / Negative Capital Outlay`). Audit event taxonomy. |
| 2 | `academic_year` | `category` | 0 | 8 waves (2009-10 .. 2024-25) | Financial year in which the anomaly or transition endpoint occurs. FY format. |
| 3 | `state_name` | `string` | 0 | 27 unique states | State/UT exhibiting the flagged shock or exception. Canonical state name. |
| 4 | `zone` | `category` | 0 | 6 Administrative Zones | Administrative zone of the state. Standard taxonomy. |
| 5 | `demographic_group` | `category` | 0 | 4 Typologies | Demographic-fiscal typology of the state. Standard taxonomy. |
| 6 | `data_basis` | `category` | 0 | Actual, RE | Accounting basis of the flagged observation. Distinguishes audited shocks from provisional RE spikes. |
| 7 | `metric_value` | `float` | 0 | -24.28 .. 31,368.46 | Numeric magnitude of the flagged anomaly (interpret together with `metric_unit`). Mixed units across rows (`% Real Change` vs. `₹ Crore`). |
| 8 | `metric_unit` | `category` | 0 | % Real Change, ₹ Crore, ₹ Crore (Nominal) | Unit governing `metric_value`. Always group/filter by `metric_unit` before comparing. |
| 9 | `details` | `string` | 0 | 41 unique audit narratives | Plain-English forensic explanation citing exact before/after figures and verification notes. Human-readable audit trail. |

### `1A1_rbi_loans_6202_covid_audit.csv`
- **Rows:** `307` | **Columns:** `13` | **Temporal Scope:** FY 2009-10 to 2025-26 (30/31 States × 10 waves)
- **Purpose:** State-level audit of RBI Major Head `6202` (Loans & Advances for Education, Sports, Art & Culture) compared against Capital Outlay (`4202`) and Revenue (`2202`).

| # | Column Name | Type | Nulls | Observed Range / Values | Definition, Formula & Caveats |
| :---: | :--- | :---: | :---: | :--- | :--- |
| 1 | `academic_year` | `string` | 0 | 10 waves: 2009-10 to 2025-26 | Indian Financial Year (April–March). Composite PK with `state_name`. |
| 2 | `state_name` | `string` | 0 | 31 States/UTs | Canonical State/UT name. Composite PK. |
| 3 | `zone` | `category` | 0 | 6 Zones | Administrative zone. Standard taxonomy. |
| 4 | `demographic_group` | `category` | 0 | 4 Typologies | Demographic-fiscal typology. Standard taxonomy. |
| 5 | `state_loans_ed_6202_cr` | `float` | 0 | 0.00 .. 1,603.09 | State Loans & Advances for Education under RBI Major Head `6202` (Appendix IV `4APP_2`, ₹ Crores, nominal). Strictly excluded from `4202` Capital Outlay; heavily concentrated in Bihar + Haryana. |
| 6 | `data_basis` | `category` | 0 | Actual, RE, BE | RBI accounting basis. Filter by basis. |
| 7 | `era` | `category` | 0 | 4 Policy Eras | Chronological policy era. FY era mapping. |
| 8 | `state_revenue_ed_cr` | `float` | 0 | 408.68 .. 105,473.63 | State Revenue education spend (`2202`, ₹ Crores, nominal). Reference scale. |
| 9 | `state_capital_ed_cr` | `float` | 0 | 0.00 .. 8,735.50 | State Capital education outlay (`4202`, ₹ Crores, nominal). Denominator for `loans_6202_to_cap_4202_pct`. |
| 10 | `real_cap_ed_2011base_cr` | `float` | 0 | 0.00 .. 4,898.96 | Real State Capital Outlay (`4202`, `2011-12` GDP prices, ₹ Crores). `state_capital_ed_cr / (gdp_deflator_2011base / 100)`. |
| 11 | `gdp_deflator_2011base` | `float` | 0 | 84.54 .. 178.31 | Spliced National GDP Deflator (`2011-12 = 100`). Deflator. |
| 12 | `real_loans_6202_2011base_cr` | `float` | 0 | 0.00 .. 930.99 | Real State Education Loans (`6202`, `2011-12` GDP prices, ₹ Crores). `state_loans_ed_6202_cr / (gdp_deflator_2011base / 100)`. |
| 13 | `loans_6202_to_cap_4202_pct` | `float` | 1 | 0.00 .. 302.51 | Education Loans (`6202`) expressed as a ratio (`%`) of Capital Outlay (`4202`). `state_loans_ed_6202_cr / state_capital_ed_cr * 100`; validly exceeds `100%` (up to `302.51%`) where student-loan schemes exceed physical CapEx; `1` NULL where `4202 = 0`. |

### `1A1_union_schemes_bodies_summary.csv`
- **Rows:** `483` | **Columns:** `12` | **Temporal Scope:** FY 2009-10 to 2026-27 (17 FYs)
- **Purpose:** Granular Union Ministry of Education Statement of Budget Estimates (SBE) ledger tracking BE, RE, Actuals, utilization %, slippage %, and real actuals across 37 canonical schemes and autonomous bodies.

| # | Column Name | Type | Nulls | Observed Range / Values | Definition, Formula & Caveats |
| :---: | :--- | :---: | :---: | :--- | :--- |
| 1 | `academic_year` | `string` | 0 | 17 FYs: 2009-10 to 2026-27 | Indian Financial Year (April–March). Includes `2026-27 BE` headlines. |
| 2 | `department` | `category` | 0 | School Education, Higher Education | Union MoE department. Filter to avoid mixing SE and HE totals. |
| 3 | `category_bucket` | `category` | 0 | 4 SBE buckets | Functional group (`Autonomous Bodies & Off-Budget Debt`, `Department Total (Net)`, `Digital e-Learning, NEP & Research`, `Flagship Schemes`). Never sum `Department Total (Net)` with component schemes. |
| 4 | `canonical_entity_name` | `string` | 0 | 37 canonical entities | Harmonized scheme or body name stitching renamed legacy schemes (e.g., `AICTE`, `Central Universities (CUs)`, `HEFA (Direct Equity Capital)`, `Samagra Shiksha`, `RUSA / PM-USHA`). Primary analytical entity key. |
| 5 | `raw_line_item` | `string` | 0 | 101 raw SBE line strings | Exact verbatim line-item label printed in the Union Budget SBE PDF for that FY. Audit provenance trail. |
| 6 | `budget_estimate_cr` | `float` | 62 | 0.00 .. 83,562.26 | Union Budget Estimate (`BE`) in ₹ Crores, nominal. `62` NULLs where scheme did not exist in BE of that FY. |
| 7 | `revised_estimate_cr` | `float` | 230 | 0.00 .. 70,567.14 | Union Revised Estimate (`RE`) in ₹ Crores, nominal. `230` NULLs for `2026-27` (BE-only) or non-sampled intermediate years. |
| 8 | `actual_expenditure_cr` | `float` | 230 | 0.00 .. 67,972.28 | Audited Actual expenditure in ₹ Crores, nominal. `230` NULLs for recent `RE`/`BE`-only years. |
| 9 | `utilization_pct_actual_vs_be` | `float` | 295 | 0.00 .. 311.89 | Actual expenditure as `%` of original Budget Estimate. `actual_expenditure_cr / budget_estimate_cr * 100`; validly exceeds `100%` (up to `311.89%`) when mid-year supplementary grants are approved; NULL when `BE = 0` or Actual unavailable. |
| 10 | `slippage_pct_re_vs_be` | `float` | 241 | -100.00 .. 74,900.00 | Mid-year revision (`RE` vs. `BE`) in `%`. `(revised_estimate_cr - budget_estimate_cr) / budget_estimate_cr * 100`; negative = mid-year cut (`-100%` = zeroed out); extreme positive values reflect nominal token `BE` lines (e.g., `₹0.01 Cr`) later funded at RE. |
| 11 | `real_actual_cr` | `float` | 230 | 0.00 .. 42,277.03 | Audited Actual expenditure in constant `2011-12` GDP prices (₹ Crores). `actual_expenditure_cr / (gdp_deflator_2011base / 100)`. |
| 12 | `era` | `category` | 0 | 4 Policy Eras | Chronological policy era. FY era mapping. |

### `1A1_external_msde_dst_niti_summary.csv`
- **Rows:** `167` | **Columns:** `12` | **Temporal Scope:** FY 2011-12 to 2026-27 (16 FYs)
- **Purpose:** External 3-Ministry SBE ledger covering MSDE (Skilling), DST (National R&D/ANRF/RDI), and NITI Aayog Atal Innovation Mission (AIM) across BE, RE, Actuals, utilization, and slippage.

| # | Column Name | Type | Nulls | Observed Range / Values | Definition, Formula & Caveats |
| :---: | :--- | :---: | :---: | :--- | :--- |
| 1 | `academic_year` | `string` | 0 | 16 FYs: 2011-12 to 2026-27 | Indian Financial Year (April–March). Includes `2026-27 BE`. |
| 2 | `department` | `category` | 0 | DST, MSDE, NITI Aayog + MoE (Stitched), Planning / NITI Aayog | External Union Ministry/Department (`NITI Aayog + MoE (Stitched)` tracks AIM continuity across institutional handoffs). Exclude `Department Total (Net)` before summing sub-schemes. |
| 3 | `category_bucket` | `category` | 0 | 5 functional buckets | Analytical grouping (`Department Total (Net)`, `National R&D & Deep-Tech (DST)`, `Skilling & Vocational (MSDE)`, `Innovation & NEP Continuity`, `Innovation & NEP (NITI Aayog)`). Separates totals from line schemes. |
| 4 | `canonical_entity_name` | `string` | 0 | 18 canonical entities | Harmonized scheme/department entity (e.g., `Total Net - DST`, `Autonomous Bodies (DST)`, `NM-ICPS`, `NRF / ANRF`, `PMKVY / PM SETU`). Primary entity key. |
| 5 | `raw_line_item` | `string` | 0 | 35 raw SBE line strings | Exact line-item text from official Union Budget SBE documents. Provenance audit column. |
| 6 | `budget_estimate_cr` | `float` | 27 | 0.00 .. 28,508.90 | Budget Estimate (`BE`) in ₹ Crores, nominal. NULL if entity did not exist in that FY's BE. |
| 7 | `revised_estimate_cr` | `float` | 82 | 0.00 .. 11,921.58 | Revised Estimate (`RE`) in ₹ Crores, nominal. NULL for BE-only years or uncollected waves. |
| 8 | `actual_expenditure_cr` | `float` | 82 | 0.00 .. 5,407.03 | Audited Actual expenditure in ₹ Crores, nominal. NULL for `2025-26 RE` and `2026-27 BE`. |
| 9 | `utilization_pct_actual_vs_be` | `float` | 112 | 0.00 .. 201.33 | Actual expenditure as `%` of `BE`. `actual_expenditure_cr / budget_estimate_cr * 100`; exceeds `100%` (up to `201.33%`) upon supplementary grants. |
| 10 | `slippage_pct_re_vs_be` | `float` | 90 | -100.00 .. 220.93 | Mid-year revision (`RE` vs. `BE`) in `%`. `(revised_estimate_cr - budget_estimate_cr) / budget_estimate_cr * 100`; negative = mid-year cut, positive = upward revision. |
| 11 | `real_actual_cr` | `float` | 82 | 0.00 .. 3,909.21 | Audited Actual expenditure in constant `2011-12` GDP prices (₹ Crores). `actual_expenditure_cr / (gdp_deflator_2011base / 100)`. |
| 12 | `era` | `category` | 0 | 4 Policy Eras | Chronological policy era. FY era mapping. |

### `1A1_digital_skilling_rd_2009_18_vs_2020_26_pivot.csv`
- **Rows:** `18` | **Columns:** `26` | **Temporal Scope:** FY 2009-10 to 2026-27 (18 FYs)
- **Purpose:** 4-Ministry (`MoE Digital/NEP + MSDE + NITI AIM + DST`) annual time-series pivot comparing Pre-NEP (`2009–2019`) vs. Post-COVID/NEP (`2019–2026`) nominal and real innovation/skilling outlays.

| # | Column Name | Type | Nulls | Observed Range / Values | Definition, Formula & Caveats |
| :---: | :--- | :---: | :---: | :--- | :--- |
| 1 | `academic_year` | `string` | 0 | 18 FYs: 2009-10 to 2026-27 | Indian Financial Year (April–March). Primary key. |
| 2 | `moe_digital_nep_rd_act_cr` | `float` | 9 | 115.51 .. 2,826.60 | MoE Digital, e-Learning, NEP & Research schemes — Audited Actuals (₹ Crores, nominal). NULL where Actuals not yet audited/sampled. |
| 3 | `moe_digital_nep_rd_re_cr` | `float` | 9 | 0.00 .. 4,500.25 | MoE Digital/NEP/R&D — Revised Estimate (`RE`, ₹ Crores, nominal). NULL where RE unavailable. |
| 4 | `moe_digital_nep_rd_be_cr` | `float` | 3 | 202.34 .. 8,600.00 | MoE Digital/NEP/R&D — Budget Estimate (`BE`, ₹ Crores, nominal). Includes `2026-27 BE`. |
| 5 | `msde_net_act_cr` | `float` | 12 | 1,007.47 .. 2,982.17 | Ministry of Skill Development & Entrepreneurship (MSDE) Net Total — Audited Actuals (₹ Crores, nominal). MSDE formed Nov 2014; NULL in earlier years & unaudited FYs. |
| 6 | `msde_net_re_cr` | `float` | 12 | 1,901.71 .. 3,300.83 | MSDE Net Total — Revised Estimate (`RE`, ₹ Crores, nominal). NULL pre-formation or BE-only FY. |
| 7 | `msde_net_be_cr` | `float` | 8 | 1,804.28 .. 9,885.80 | MSDE Net Total — Budget Estimate (`BE`, ₹ Crores, nominal). NULL pre-formation. |
| 8 | `niti_aim_act_cr` | `float` | 13 | 27.08 .. 341.96 | NITI Aayog Atal Innovation Mission (AIM) — Audited Actuals (₹ Crores, nominal). AIM launched 2016; NULL pre-launch & unaudited FYs. |
| 9 | `niti_aim_re_cr` | `float` | 13 | 109.35 .. 343.21 | NITI AIM — Revised Estimate (`RE`, ₹ Crores, nominal). NULL pre-launch or BE-only FY. |
| 10 | `niti_aim_be_cr` | `float` | 10 | 155.00 .. 405.00 | NITI AIM — Budget Estimate (`BE`, ₹ Crores, nominal). NULL pre-launch. |
| 11 | `dst_net_act_cr` | `float` | 11 | 2,511.66 .. 5,407.03 | Dept. of Science & Technology (DST) Net Total — Audited Actuals (₹ Crores, nominal). NULL in unaudited/unsampled FYs. |
| 12 | `dst_net_re_cr` | `float` | 11 | 2,550.19 .. 11,921.58 | DST Net Total — Revised Estimate (`RE`, ₹ Crores, nominal). Reflects mid-year RDI/ANRF cuts. |
| 13 | `dst_net_be_cr` | `float` | 6 | 2,873.22 .. 28,508.90 | DST Net Total — Budget Estimate (`BE`, ₹ Crores, nominal). Includes `2026-27 BE` paper headline. |
| 14 | `regime_period` | `category` | 0 | Pre-NEP (2009-2019), NEP / Post-COVID (2019-2026) | Binary policy-regime grouping for pre- vs. post-NEP comparisons. Analytical slice. |
| 15 | `era` | `category` | 0 | 4 Policy Eras | Chronological policy era. FY era mapping. |
| 16 | `gdp_deflator_2011base` | `float` | 1 | 84.54 .. 178.31 | Spliced GDP Deflator (`2011-12 = 100`). `1` NULL for `2026-27 BE` (future FY deflator not yet realized). |
| 17 | `moe_digital_nep_rd_best_nom_cr` | `float` | 1 | 0.00 .. 8,600.00 | MoE Digital/NEP/R&D best-available nominal outlay (₹ Crores). Waterfall priority: `COALESCE(Actual, RE, BE)` (`Actual > RE > BE`). |
| 18 | `moe_digital_nep_rd_best_real_cr` | `float` | 2 | 0.00 .. 2,523.79 | MoE Digital/NEP/R&D best-available real outlay (`2011-12` GDP prices, ₹ Crores). `moe_digital_nep_rd_best_nom_cr / (gdp_deflator_2011base / 100)`. |
| 19 | `msde_net_best_nom_cr` | `float` | 6 | 1,007.47 .. 9,885.80 | MSDE best-available nominal outlay (₹ Crores). `COALESCE(Actual, RE, BE)`. |
| 20 | `msde_net_best_real_cr` | `float` | 7 | 833.42 .. 2,339.60 | MSDE best-available real outlay (`2011-12` GDP prices, ₹ Crores). `msde_net_best_nom_cr / (gdp_deflator_2011base / 100)`. |
| 21 | `niti_aim_best_nom_cr` | `float` | 9 | 27.08 .. 405.00 | NITI AIM best-available nominal outlay (₹ Crores). `COALESCE(Actual, RE, BE)`. |
| 22 | `niti_aim_best_real_cr` | `float` | 10 | 15.35 .. 232.39 | NITI AIM best-available real outlay (`2011-12` GDP prices, ₹ Crores). `niti_aim_best_nom_cr / (gdp_deflator_2011base / 100)`. |
| 23 | `dst_net_best_nom_cr` | `float` | 3 | 2,511.66 .. 28,049.32 | DST best-available nominal outlay (₹ Crores). `COALESCE(Actual, RE, BE)`. |
| 24 | `dst_net_best_real_cr` | `float` | 4 | 2,324.54 .. 6,685.75 | DST best-available real outlay (`2011-12` GDP prices, ₹ Crores). `dst_net_best_nom_cr / (gdp_deflator_2011base / 100)`. |
| 25 | `combined_4ministry_best_nom_cr` | `float` | 0 | 477.98 .. 46,940.12 | Combined 4-Ministry best-available nominal outlay (₹ Crores). Row sum of the 4 `*_best_nom_cr` series (treating pre-formation NULLs as 0). Peaks at `₹46,940.12 Cr` in `2026-27 BE`. |
| 26 | `combined_4ministry_best_real_cr` | `float` | 1 | 565.41 .. 10,787.40 | Combined 4-Ministry best-available real outlay (`2011-12` GDP prices, ₹ Crores). `combined_4ministry_best_nom_cr / (gdp_deflator_2011base / 100)`; `1` NULL for `2026-27 BE`. |

