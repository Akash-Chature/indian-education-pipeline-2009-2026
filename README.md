# Two Decades of the Indian Education Pipeline (2009–2026)

**Structural Policy Shifts, Public Finance Realities, Foundational Outcomes, and Labor-Market Absorption**

**Author:** Akash Kumar Chature  
**Publication:** *State & Signal* ([statesignal.substack.com](https://statesignal.substack.com))  
**Repository Scope:** Open-science analytical framework, canonical 6-lakehouse schemas, reproducible SQL documentation, and compliance-verified aggregated benchmarks.

---

## 1. Project Overview

Indian education and human-capital debates rarely suffer from a shortage of opinions; they suffer from fragmented data. Treasury documents track rupees allocated, administrative school censuses count physical buildings and enrolment registers, household surveys record private financial friction and foundational learning, and labor surveys track headline employment—yet these datasets are almost never reconciled in a single analytical system.

This longitudinal study harmonizes **20M+ records** across **six national datasets** (2009–2026) in a local **DuckDB + ZSTD Parquet** lakehouse architecture, evaluated across **four chronological policy eras**:
1. **Era 1 — RTE Era (`2009–2014`):** Right to Education Act rollout and society-routed Centrally Sponsored Schemes.
2. **Era 2 — Pre-COVID Benchmark (`2014–2019`):** Treasury-routed `3601` intergovernmental transfers and private unaided expansion.
3. **Era 3 — Post-COVID & Pre-NEP (`2020–2023`):** Pandemic shock, temporary public-school return, and SDMS/PEN deduplication.
4. **Era 4 — Post-NEP Era (`2024–2026`):** School consolidation, NIPUN Bharat, NCF 2023 rollout, and tertiary-to-labor realities.

---

## 2. Six-Lakehouse Architecture & Release Status (Updated Nov 23, 2026)

| # | Lakehouse / Institutional Source | Coverage & Certified Scale | Stage 1A Baseline Audit | Public GitHub Release Status |
| :---: | :--- | :--- | :---: | :--- |
| **1** | **Macro-Financial** (Union Budget 4-Ministry SBE + RBI State Finances) | `FY 2009–10` to `2025–26` (10 waves; 31 States/UTs; 327 core rows + SBE/6202 ledgers) | **PASS (10/10 CSVs)** | **LIVE NOW** (`data_aggregated/1A_1_Macro_Financial/`) |
| **2** | **UDISE+ / DISE** (MoE School Supply Census) | `AY 2014–15` to `2025–26` (12.1M+ school records; lossless EAV MoE parity) | **PASS (22/22 CSVs)** | **Held (Dec 2 Cutoff)** — awaiting official NIEPA `2009–12` DISE raw data |
| **3** | **ASER Rural Microdata** (Formal Research MoU) | 6 Waves (`2009–2024`; 3,501,238 rows across 32 rural States/UTs) | **PASS (13/13 CSVs)** | **Held Offline (MoU Gate)** — submitted for ASER Centre pre-publication review |
| **4** | **NSSO 71st Round** (Schedule 25.2: Education) | `2014` Cross-Section (470,266 weighted rows; 639 districts) | Scheduled (Week 2) | Upcoming |
| **5** | **AISHE** (MoE Higher Education Census) | `2011–12` to `2023–24` (8 canonical tables) | Scheduled (Week 2) | Upcoming |
| **6** | **PLFS & NSSO** (66th / 68th Rounds + PLFS) | 6 Waves (`2009–10` to `2025`; 4,111,292 weighted rows) | Scheduled (Week 2) | Upcoming |

---

## 3. Live Release 1: Stage 1A.1 Macro-Financial Highlights (`FY 2009–10` to `2025–26`)

All 10 compliance-verified CSV tables backing *State & Signal* Essay #1 (**"The Phantom NEP Budget"**) are now live in [`data_aggregated/1A_1_Macro_Financial/`](data_aggregated/1A_1_Macro_Financial/):

* **Stagnant GDP Effort vs. NEP 2020's 6% Target:** Consolidated net public education expenditure (Union MoE + 31 States/UTs with Legislatures, strictly netting out Central Grants-in-Aid under Major Heads `3601` + `3602` to eliminate Centre–State double counting) peaked at **3.23% of published GDP** in `FY 2011–12` and stood at **2.76%** in `FY 2023–24 (Actual)` (**2.84%** in `FY 2025–26 RE/BE`).
* **Nominal Illusion vs. Real Purchasing Power:** While nominal consolidated expenditure expanded **4.02×** (`₹1,98,426 Cr` in `2009–10` to `₹7,98,469 Cr` in `2023–24 Actual`), real purchasing power rose only **1.98×** (spliced GDP deflator, `2011–12` base) and **1.70×** (CPI-Combined deflated).
* **State Social-Sector Crowding Out:** Across all 31 States/UTs, education's share of State Social Sector budgets dropped **-6.48 pp** in actuals (`46.08%` in `2009–10` → `39.60%` in `2023–24 Actual`) and **-10.19 pp** into `2025–26 BE` (`35.89%`).
* **4-Ministry Innovation, Skilling & R&D Squeeze:** Across MoE Digital/NEP, MSDE, NITI AIM, and DST, combined real actual outlays fell **-4.34%** between `2018–19` (`₹6,309.8 Cr`) and `2024–25` (`₹6,036.0 Cr`, `2011–12` real prices), masked by `₹46,940.1 Cr` (`2026–27 BE`) paper headlines amid steep RE cuts and low utilization across flagship R&D/skilling schemes.

---

## 4. Repository Structure

```text
indian-education-pipeline-2009-2026/
├── README.md                          # Master project overview & release ledger
├── DATA_GOVERNANCE_AND_MOU.md         # Strict non-redistribution & MoU compliance firewall
├── LICENSE                            # MIT License (code & original documentation)
├── .gitignore                         # Blocks all raw microdata, .duckdb, and .parquet files
├── docs/
│   ├── research_framework.md          # 4 Eras, 3 Phases, 5 Chains of Reason & 6 Blind Spots
│   └── data_dictionary.md             # Canonical 6-lakehouse schemas & weighting formulas
├── data_aggregated/
│   └── 1A_1_Macro_Financial/          # LIVE: 10 certified Union & RBI fiscal CSVs (FY 2009-26)
└── sql/                               # Reproducible analytical queries (rolling upload)
```

---

## 5. Data Governance, MoU Compliance & Methodological Disclosures

Full licensing and privacy terms are documented in [`DATA_GOVERNANCE_AND_MOU.md`](DATA_GOVERNANCE_AND_MOU.md). No unit-level microdata (ASER, NSSO, PLFS) or local `.duckdb`/`.parquet` lakehouse files are stored or redistributed in this repository.

### Methodological & Audit Disclosures (Stage 1A Hardening — Nov 2026)
* **Fiscal Accounting Hygiene (`1A_1`):** All Union Budget totals are strictly Net-of-Recoveries (deducting `PSK`/`MUSK` reserve-fund transfers), consolidated national totals net out intergovernmental grants (`3601` + `3602`) to prevent Centre–State double counting, and every table retains explicit `Actual` / `RE` / `BE` basis flags without silent pooling.
* **Legacy DISE Quarantine (`1A_2` — Pending Release):** Corrupted third-party `2009–10` / `2011–12` Dropbox DISE archives (`250` rows) were permanently purged during forensic audit; UDISE+ longitudinal tables anchor strictly on `AY 2014–15` to `2025–26`, with explicit `Pending_NIEPA_2009_10` and `Pending_NIEPA_2011_12` placeholder slots reserved in `1A2_four_era_policy_scorecard.csv` pending official NIEPA access.
* **Preserved Raw Portal Anomalies (`1A_2` — Pending Release):** Four legacy `2014–15` Karnataka Private Aided/Unaided Pre-University College rows in `1A2_repeaters_rte_section16_audit.csv` report repeaters exceeding enrolment (`109.53%–263.64%`); all four are transparently retained and flagged (`raw_udise_rep_gt_enr_flag = 1`) rather than silently dropped.
* **Fiscal-Year vs. Academic-Year Era Alignment:** `1A_1_Macro_Financial` maps fiscal years (`FY 2014–15` = Era 2; `FY 2023–24` = Era 4), whereas `1A_2_UDISE_Supply` maps academic years (`AY 2014–15` = Era 1 End; `AY 2023–24` = Era 3B SDMS deduplication break); cross-institutional modules harmonize these boundaries explicitly.
* **ASER Centre Pre-Publication Firewall (`1A_3`):** All 13 Stage 1A.3 ASER aggregated CSVs (strictly `n < 30` suppressed) remain held offline pending formal pre-publication review and written clearance from ASER Centre (Pratham).

---

*Contact: Akash Kumar Chature | [LinkedIn](https://www.linkedin.com/in/akash-chature-b77869168) | [State & Signal (Substack)](https://statesignal.substack.com)*

