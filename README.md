# Two Decades of the Indian Education Pipeline (2009–2026)

**Structural Policy Shifts, Public Finance Realities, Foundational Outcomes, and Labor-Market Absorption**

**Author:** Akash Kumar Chature  
**Publication:** *State & Signal* ([statesignal.substack.com](https://statesignal.substack.com))  
**Repository Scope:** Open-science analytical framework, canonical 6-lakehouse schemas, reproducible SQL documentation, and compliance-verified aggregated benchmarks.

---

## 1. Project Overview

Indian education and human-capital debates rarely suffer from a shortage of opinions; they suffer from fragmented data. Treasury documents track rupees allocated, administrative censuses count physical buildings and enrolment registers, household surveys capture private financial friction and foundational learning, and labor surveys track headline employment—yet these datasets are almost never reconciled in a single analytical system.

This longitudinal study harmonizes **20M+ records** across **six national datasets** (`2009–2026`) in a local **DuckDB + ZSTD Parquet** lakehouse architecture, evaluated across **four chronological policy eras**:
1. **Era 1 — RTE Era (`2009–2014`):** Right to Education Act rollout and society-routed Centrally Sponsored Schemes.
2. **Era 2 — Pre-COVID Benchmark (`2014–2019`):** Treasury-routed `3601` intergovernmental transfers and private unaided expansion.
3. **Era 3 — Post-COVID & Pre-NEP (`2020–2023`):** Pandemic shock, temporary public-school return, and SDMS/PEN deduplication.
4. **Era 4 — Post-NEP Era (`2024–2026`):** School consolidation, NIPUN Bharat, NCF 2023 rollout, and tertiary-to-labor realities.

---

## 2. Six-Lakehouse Architecture & Stage 1A Release Ledger (Updated Nov 27, 2026)

All six standalone Stage 1A baseline audits (`81` hardened CSVs + `6` paired Data Dictionaries; `0` IEEE-754 float noise, `0` duplicates) are **100% QA-passed**. Currently, **`46` public CSVs + `4` Data Dictionaries** are live on GitHub, with `1A_2` staged for our Dec 2 NIEPA cutoff and `1A_3` held offline under our ASER Centre MoU:

| # | Lakehouse / Institutional Source | Coverage, Design & Certified Scale | Stage 1A Audit | Public GitHub Status |
| :---: | :--- | :--- | :---: | :--- |
| **1** | **Macro-Financial** (Union 4-Ministry SBE + RBI State Finances) | `FY 2009–10` to `2025–26` (`10` waves; `31` States/UTs — **Longitudinal**) | **PASS (`10` CSVs)** | **LIVE NOW** ([`1A_1_Macro_Financial/`](data_aggregates/1A_1_Macro_Financial/)) |
| **2** | **UDISE+ / DISE** (MoE School Supply Census) | `AY 2014–15` to `2025–26` (`12.1M+` school records; lossless EAV MoE parity — **Longitudinal**) | **PASS (`22` CSVs)** | **Held (Dec 2 Cutoff)** — awaiting official NIEPA `2009–12` DISE raw data |
| **3** | **ASER Rural Microdata** (Formal Research MoU) | `2009–2024` (`6` waves; `3,501,238` rows across `32` rural States/UTs — **Longitudinal**) | **PASS (`13` CSVs)** | **PRIVATE HOLD (MoU)** — submitted for ASER Centre pre-publication review |
| **4** | **NSSO 71st Round** (`Schedule 25.2`: Education) | Jan–Jun `2014` (`470,266` records: `65.9k` HHs, `93.5k` students — **RTE-Endpoint Cross-Section**) | **PASS (`12` CSVs)** | **LIVE NOW** ([`1A_4_NSSO71_Demand/`](data_aggregates/1A_4_NSSO71_Demand/)) |
| **5** | **AISHE** (MoE Higher Education Census) | `AY 2011–12` to `2023–24` (`7` canonical waves; `279,599` rows — **Longitudinal**) | **PASS (`12` CSVs)** | **LIVE NOW** ([`1A_5_AISHE_HigherEd/`](data_aggregates/1A_5_AISHE_HigherEd/)) |
| **6** | **PLFS & NSSO** (`66th` / `68th` Rounds + PLFS) | `2009–10` to `2025` (`6` waves; `4,111,292` records: `778k` HHs, `3.33M` persons — **Longitudinal**) | **PASS (`12` CSVs)** | **LIVE NOW** ([`1A_6_PLFS_Labour/`](data_aggregates/1A_6_PLFS_Labour/)) |

---

## 3. Highlights From Live Stage 1A Public Releases (`46` CSVs Across 4 Institutions)

* **Universal Naming Guide & Formula Index:** [`docs/stage1a_output_dictionary_index.md`](docs/stage1a_output_dictionary_index.md)

### A. Stage 1A.1 — Macro-Financial (`FY 2009–10` to `2025–26`)
* **Folder & Dictionary:** [`data_aggregates/1A_1_Macro_Financial/`](data_aggregates/1A_1_Macro_Financial/) | [`DATA_DICTIONARY.md`](data_aggregates/1A_1_Macro_Financial/DATA_DICTIONARY.md)
* **Stagnant GDP Effort vs. NEP 2020's 6% Target:** Consolidated net public education expenditure (Union MoE + 31 States/UTs, netting out `3601`/`3602` grants) peaked at **3.23% of published GDP** (`FY 2011–12`) and stood at **2.76%** (`FY 2023–24 Actual`; **2.84%** `FY 2025–26 RE/BE`). While nominal spend expanded **4.02×** (`2009–24`), real purchasing power rose **1.98×** (GDP-deflated) / **1.70×** (CPI-deflated).
* **State Crowding Out & 4-Ministry R&D Squeeze:** Education's share of State Social Sector budgets dropped **-6.48 pp** in actuals (`46.08% → 39.60%`, `2009–24`). Across 4 ministries (`MoE Digital/NEP + MSDE + NITI AIM + DST`), combined real actual outlays fell **-4.34%** (`2018–19 → 2024–25`), masked by `₹46,940.1 Cr` (`2026–27 BE`) paper headlines.

### B. Stage 1A.4 — NSSO 71st Round Household Demand (`Jan–Jun 2014` RTE-Endpoint Cross-Section)
* **Folder & Dictionary:** [`data_aggregates/1A_4_NSSO71_Demand/`](data_aggregates/1A_4_NSSO71_Demand/) | [`1A4_DATA_DICTIONARY.md`](data_aggregates/1A_4_NSSO71_Demand/1A4_DATA_DICTIONARY.md)
* **The "Free Education" Mirage & Private Wallet Shock:** Across `49,170` student households, families spent **17.39%** of total annual consumption on education (`7.18%` Govt-Only vs. `25.51%` Any Private Unaided — a **3.55×** wallet-share jump and **5.94×** rupee spike). Among `34,129` Govt students reporting `edu_free = 'Yes'`, **99.87%** still paid out-of-pocket (`₹1,807/yr` mean).
* **Remedial Coaching, Hidden Markets & Sibling Rupee Bias:** **25.92%** took paid coaching (**89.17%** citing *"Augmenting basic education"*, peaking at **56.22%** in East Zone); **11.33%** of General Private Unaided students attended unrecognized schools (`24.10%` in Rural `Q1`); English medium carried a **6.30×** fee premium (`₹24,743` vs. `₹3,926`); and inside same-stage `C1–12` mixed-gender homes (`n = 13,275` HHs), families spent **+14.79%** more on sons (`Bihar +38.75%`, `Rajasthan +36.41%`).

### C. Stage 1A.5 — AISHE Higher Education Longitudinal Audit (`AY 2011–12` to `2023–24`)
* **Folder & Dictionary:** [`data_aggregates/1A_5_AISHE_HigherEd/`](data_aggregates/1A_5_AISHE_HigherEd/) | [`1A5_DATA_DICTIONARY.md`](data_aggregates/1A_5_AISHE_HigherEd/1A5_DATA_DICTIONARY.md)
* **Privatization Tipping Point:** Total enrolment climbed `2.92 Cr → 4.50 Cr` (`GER 20.80 → 30.02`, `GPI 0.88 → 1.08`), but **State Private Universities (`464`) overtook State Public Universities (`462`)**, while Government Universities + Colleges lost **-10.62 pp** of enrolment share (`49.41% → 38.79%`) as `32,513` Private Unaided Affiliated Colleges scaled at `2.30×` smaller average campus size (`486` vs. `1,119`).
* **INI Dilution, ODL GER Distortion & `B.A.` Massification:** Inside premier INIs (`159`), implied PTR loosened (`14.04 → 17.79`), Full Professor share fell **-9.28 pp**, and INI share of Indian Ph.D.s dropped **-7.00 pp** (`20.39% → 13.39%`). ODL accounts for **40.56%** of university-level enrolment, inflating Delhi's GER by **+31.36 pp** (`21.57% → 52.93%`). Meanwhile, `B.A.` massified to `13.30M` while Engineering & Technology remained `31.26%` Female and the Full-vs-Asst Professor female gap stayed frozen at **-14.0 pp** for 12 years.

### D. Stage 1A.6 — PLFS & NSSO Labour-Market Longitudinal Audit (`2009–10` to `2025`)
* **Folder & Dictionary:** [`data_aggregates/1A_6_PLFS_Labour/`](data_aggregates/1A_6_PLFS_Labour/) | [`1A6_DATA_DICTIONARY.md`](data_aggregates/1A_6_PLFS_Labour/1A6_DATA_DICTIONARY.md)
* **Jobless-Quality Paradox & Graduate Credential Crash (`2018–19 → 2025`):** Headline `PS+SS` WPR jumped **+10.12 pp** (`47.31% → 57.43%`) while Regular Salaried share stayed flat (**23.81% → 23.64%**). Graduate unemployment sits at **11.18%** (`41.4×` Illiterates), Graduate salaried share fell **-5.98 pp** (`57.80% → 51.82%`), and **30.52%** of employed Graduates (`50.35%` in UP) now work vertically over-qualified in low-skill `NCO 6–9` jobs.
* **Female LFPR Decomposition, Vocational ROI & ADP Blind Spot:** **79.39%** of the `+15.53 pp` female WPR surge (`2018–25`) came from Own-Account + Unpaid Family Labour (`84.40%` Rural), while Regular Salaried contributed **12.69%** (except **Diploma women**, where `69.1%` of gains were salaried). Controlled Formal Vocational Training lifts Secondary/HSec salaried share by **+17.99 pp** and cuts Diploma/Grad+ UR by **-6.72 pp**. Across `742` qualifying districts in `2025`, **80 / 100** highest-stress labour districts sit outside NITI Aayog's 112 ADP list (replicated independently in AISHE `74/100` and NSSO `7/10`), and ADP districts diverged by **+4.16 pp** in graduate over-qualification (`2018–25`).

---

## 4. Repository Structure

```text
indian-education-pipeline-2009-2026/
├── README.md                                  # Master project overview & Stage 1A release ledger
├── DATA_GOVERNANCE_AND_MOU.md                 # Strict non-redistribution & MoU compliance firewall
├── LICENSE                                    # MIT License (code & original documentation)
├── .gitignore                                 # Blocks all raw microdata, .duckdb, and .parquet files
├── docs/
│   ├── research_framework.md                  # 4 Eras, 3 Phases, 5 Chains of Reason & 6 Blind Spots
│   ├── data_dictionary.md                     # Canonical 6-lakehouse source schemas & weighting rules
│   └── stage1a_output_dictionary_index.md     # Output CSV naming conventions, formulas & 6-folder index
├── data_aggregates/
│   ├── 1A_1_Macro_Financial/                  # LIVE: 10 Union & RBI fiscal CSVs + README + Dictionary
│   ├── 1A_4_NSSO71_Demand/                    # LIVE: 12 NSSO 71st (2014) CSVs + README + Dictionary
│   ├── 1A_5_AISHE_HigherEd/                   # LIVE: 12 AISHE (2011–24) CSVs + README + Dictionary
│   └── 1A_6_PLFS_Labour/                      # LIVE: 12 PLFS/NSSO (2009–25) CSVs + README + Dictionary
└── sql/                                       # Reproducible analytical queries (rolling upload)
```

---

## 5. Data Governance, MoU Compliance & Methodological Disclosures

Full licensing and privacy terms are documented in [`DATA_GOVERNANCE_AND_MOU.md`](DATA_GOVERNANCE_AND_MOU.md). No unit-level microdata (ASER, NSSO, PLFS) or local `.duckdb`/`.parquet` lakehouse files are stored or redistributed in this repository.

### Stage 1A Audit & Methodological Disclosures (All 6 Lakehouses)
* **Macro-Financial (`1A_1` — Live):** Union Budget totals are strictly Net-of-Recoveries (`PSK`/`MUSK` deducted); consolidated national totals net out intergovernmental grants (`3601` + `3602`); all tables retain explicit `Actual` / `RE` / `BE` basis flags without silent pooling.
* **UDISE+ Supply (`1A_2` — Staged, Dec 2 Cutoff):** Corrupted third-party `2009–10` / `2011–12` Dropbox DISE archives (`250` rows) were permanently purged during forensic audit; UDISE+ longitudinal tables anchor strictly on `AY 2014–15` to `2025–26` with explicit `Pending_NIEPA_2009_10` / `2011_12` placeholder slots pending official NIEPA access, and `4` legacy `2014–15` Karnataka PUC rows are retained and flagged (`raw_udise_rep_gt_enr_flag = 1`).
* **ASER Ground Truth (`1A_3` — Private MoU Hold):** All `13` Stage 1A.3 ASER aggregated CSVs (strictly `n < 30` suppressed) remain held offline pending formal pre-publication review and written clearance from ASER Centre (Pratham).
* **NSSO 71st Demand (`1A_4` — Live, 2014 Cross-Section Only):** `hostel_exp` (`values 1–35`) is a Hostel Destination State/UT code (excluded from rupee Wallet Burden; used strictly for Inter-State Student Migration); `nature_inst` (`Recognized` vs. `Unrecognized`) was canvassed strictly for `General + Private Unaided` students (dual denominators reported); `402 / 639` districts qualify at `n >= 30`.
* **AISHE Higher Education (`1A_5` — Live):** `2021–22` onward incorporates MoE's revised `2020` `18–23` population projection (`+10.12M` jump); raw `'Tamilnadu'` population spellings (`2019–24`) were normalized (`0` drops); AISHE omits mode disaggregation for Foreign Students (`foreign_mode_disagg_available = 0`) and Out-turn (`outturn_mode_disagg_available = 0`) as well as tuition fees.
* **PLFS / NSSO Labour (`1A_6` — Live):** `2025` Calendar Year reflects MoSPI's revamped `270,472`-HH / `764`-district sample frame (`revamped_2025_sample_flag = 1`); `NCO`/`NIC` splits anchor strictly on `PS` workers (since `SS-Only` omits NCO/NIC in `2011–25`) while 5-bucket job quality covers `100%` of `PS+SS` workers; Gig & Orange economy figures are reproducible `NIC-2008` (`2011–25`) structural proxies pending MoSPI's dedicated Jan `2027` platform-worker module.
* **Cross-Lakehouse Era Alignment (`Stage 1B`):** `1A_1` maps fiscal years (`FY 2014–15` = Era 2; `FY 2023–24` = Era 4), `1A_2` maps academic years (`AY 2014–15` = Era 1 End; `AY 2023–24` = Era 3B SDMS break), and `1A_4` is a `2014` RTE-endpoint cross-section; Stage 1B cross-institutional modules harmonize these boundaries explicitly.

---

*Contact: Akash Kumar Chature | [LinkedIn](https://www.linkedin.com/in/akash-chature-b77869168) | [State & Signal (Substack)](https://statesignal.substack.com)*

