# Two Decades of the Indian Education Pipeline (2009–2026)

**Structural Policy Shifts, Public Finance Realities, Foundational Outcomes, and Labor-Market Absorption**

**Author:** Akash Kumar Chature | **Publication:** *State & Signal* ([statesignal.substack.com](https://statesignal.substack.com))  
**Repository Scope:** Open-science research architecture, canonical multi-agency schemas, reproducible methodology documentation, and compliance-verified aggregated benchmarks.

---

## 1. Project Overview

Indian education and human-capital debates rarely suffer from a shortage of opinions; they suffer from fragmented data. Treasury budgets track rupees allocated, administrative school censuses record physical inputs and registers, household surveys capture private spending and school distance, and labour surveys summarize headline workforce participation—yet these datasets are almost never examined within a single analytical system.

This longitudinal study harmonizes **20M+ official records** across **six national datasets** (`2009–2026`) in a local DuckDB and Parquet analytical architecture, evaluated across **four chronological policy eras**:

1. **RTE Era (`2009–2014`):** Universal access expansion under the Right to Education Act and society-routed Centrally Sponsored Schemes.
2. **Pre-COVID Benchmark (`2014–2019`):** State-treasury grant routing (`3601`), Plan/Non-Plan budget merger, and private unaided expansion.
3. **Post-COVID & Pre-NEP (`2020–2023`):** Pandemic disruption, temporary public-school return, and student-registry deduplication.
4. **Post-NEP Era (`2024–2026`):** School consolidation, foundational learning recovery, tertiary massification, and current labour-market realities.

For the full conceptual blueprint, institutional blind-spot matrix, and cross-agency transmission chains, see [`docs/research_framework.md`](docs/research_framework.md).

---

## 2. Six-Dataset Architecture & Publication Status

| # | Dataset / Institutional Source | Coverage & Scope | Repository Status | Directory & Documentation |
| :---: | :--- | :--- | :--- | :--- |
| **1** | **Macro-Financial** (Union Budget 4-Ministry SBE + RBI State Finances) | FY `2009–10` to `2025–26` (10 waves; 31 States/UTs; 4-ministry R&D/skilling through `2026–27` BE) | **Available** (`10` CSVs + Dictionary) | [`data_aggregated/1A_1_Macro_Financial/`](data_aggregated/1A_1_Macro_Financial/) |
| **2** | **UDISE+ / DISE** (Ministry of Education School Supply Census) | AY `2014–15` to `2025–26` (12.1M+ school records; full student-count parity) | **Forthcoming** (awaiting official `2009–12` NIEPA historical verification) | Scheduled for upcoming public release |
| **3** | **ASER Rural Microdata** (ASER Centre Research MoU) | `2009–2024` (6 rural waves; 3.50M household/child/school audit records) | **Pending Institutional Review** | Held offline pending formal ASER Centre pre-publication clearance |
| **4** | **NSSO 71st Round** (MoSPI Schedule 25.2: Education) | Jan–Jun `2014` Cross-Section (`470,266` records: `65.9k` households, `93.5k` students, `639` districts) | **Available** (`12` CSVs + Dictionary) | [`data_aggregated/1A_4_NSSO71_Demand/`](data_aggregated/1A_4_NSSO71_Demand/) |
| **5** | **AISHE** (Ministry of Education Higher Education Census) | AY `2011–12` to `2023–24` (7 waves across 8 canonical tables; `279,599` records) | **Available** (`12` CSVs + Dictionary) | [`data_aggregated/1A_5_AISHE_HigherEd/`](data_aggregated/1A_5_AISHE_HigherEd/) |
| **6** | **PLFS & NSSO** (MoSPI 66th/68th Rounds + PLFS) | `2009–10` to `2025` (6 waves; `4,111,292` records: `778k` households, `3.33M` individuals) | **Available** (`12` CSVs + Dictionary) | [`data_aggregated/1A_6_PLFS_Labour/`](data_aggregated/1A_6_PLFS_Labour/) |

---

## 3. Published Aggregated Benchmarks (46 CSVs & 4 Data Dictionaries)

Universal column naming rules, unit conventions, and cross-dataset usage guidelines are documented in [`docs/output_dictionary_index.md`](docs/output_dictionary_index.md).

### A. Public Finance & Education Expenditure — [`data_aggregated/1A_1_Macro_Financial/`](data_aggregated/1A_1_Macro_Financial/)
*Manifest:* [`README.md`](data_aggregated/1A_1_Macro_Financial/README.md) | *Dictionary:* [`1A1_DATA_DICTIONARY.md`](data_aggregated/1A_1_Macro_Financial/1A1_DATA_DICTIONARY.md)
* **Stagnant GDP Effort vs. NEP 2020's 6% Target:** Consolidated net public education expenditure (Union Ministry of Education plus 31 States/UTs with Legislatures, strictly netting out Central Grants-in-Aid under Major Heads `3601 + 3602`) peaked at **3.23% of published GDP** in `FY 2011–12` and stood at **2.76%** in `FY 2023–24` Actual (`2.84%` in `FY 2025–26` RE/BE).
* **Nominal Expansion vs. Real Purchasing Power:** Nominal consolidated spending grew **4.02x** (`₹1,98,426 Cr` in `FY 2009–10` to `₹7,98,469 Cr` in `FY 2023–24` Actual), whereas constant-price (`2011–12` base) purchasing power expanded **1.98x** using the spliced GDP deflator and **1.70x** using CPI-Combined.
* **State Social-Sector Crowding Out:** Across all 31 States/UTs, education's share of State Social Sector budgets fell **-6.48 pp** in audited actuals (`46.08%` in `FY 2009–10` to `39.60%` in `FY 2023–24`) and **-10.19 pp** into `FY 2025–26` BE (`35.89%`).
* **Four-Ministry Innovation, Skilling & R&D Squeeze:** Across Ministry of Education Digital/NEP/R&D, MSDE, NITI Aayog AIM, and DST, combined real audited actual outlays declined **-4.34%** between `FY 2018–19` (`₹6,309.8 Cr`) and `FY 2024–25` (`₹6,036.0 Cr`, constant `2011–12` prices), masked by `₹46,940.1 Cr` (`FY 2026–27` BE) headline estimates amid steep mid-year Revised Estimate cuts.

### B. Household Education Demand & Costs (2014 Cross-Section) — [`data_aggregated/1A_4_NSSO71_Demand/`](data_aggregated/1A_4_NSSO71_Demand/)
*Manifest:* [`README.md`](data_aggregated/1A_4_NSSO71_Demand/README.md) | *Dictionary:* [`1A4_DATA_DICTIONARY.md`](data_aggregated/1A_4_NSSO71_Demand/1A4_DATA_DICTIONARY.md)
* **Household Wallet Burden & Private Cost Shock:** Across `49,170` student households (`wgt_combined`-weighted), families allocated **17.39%** of total annual household consumption to education (`13.35%` Rural, `22.94%` Urban). Moving from Government-only (`7.18%`, `₹5,544`) to Any Private Unaided (`25.51%`, `₹32,933`) multiplied wallet share **3.55x**; **99.87%** of government students reporting free education still incurred positive out-of-pocket costs (`₹1,807`/year mean).
* **Remedial Shadow Coaching & Hidden Markets:** **25.92%** of students took paid private coaching (**89.17%** citing *"Augmenting basic education"*, peaking at `56.22%` in East Zone); **11.33%** of General Private Unaided students (`30.47%` East) attended unrecognized schools; English-medium schooling carried a **6.30x** fee differential (`₹24,743` vs. `₹3,926` for home-language vernacular).
* **Distance Decay & Intra-Household Gender Allocation:** Secondary school distance (`< 1 km` to `>= 5 km`) lowered Rural Female (`14–17`) attendance by **-12.81 pp** (`76.95%` to `64.14%`). Within-household fixed effects across same-stage Classes 1–12 mixed-gender homes (`n = 13,275`) reveal **+14.79%** (`+₹819`/year) higher spending on sons (`Bihar +38.75%`, `Rajasthan +36.41%`).

### C. Higher Education Capacity & Equity (`AY 2011–12` to `2023–24`) — [`data_aggregated/1A_5_AISHE_HigherEd/`](data_aggregated/1A_5_AISHE_HigherEd/)
*Manifest:* [`README.md`](data_aggregated/1A_5_AISHE_HigherEd/README.md) | *Dictionary:* [`1A5_DATA_DICTIONARY.md`](data_aggregated/1A_5_AISHE_HigherEd/1A5_DATA_DICTIONARY.md)
* **Institutional Privatization Shift:** Total enrolment grew from **2.92 Cr** to **4.50 Cr** (`GER 20.80` to `30.02`, `GPI 0.88` to `1.08`), while State Private Universities (**464**) overtook State Public Universities (**462**) and Private Unaided Colleges reached **32,513** (**78.46%** of new colleges in `2023–24`) at less than half the average student scale of government colleges (`486` vs. `1,119`).
* **Distance Learning & Discipline Sorting:** Open & Distance Learning (`4.40M`) accounted for **40.56%** of university-level enrolment, inflating Delhi's headline GER by **+31.36 pp**. Broad STEM female representation (**46.13%**) reflects high shares in Medical (**57.40%**) and Pure Science (**55.41%**) alongside **31.26%** in Engineering & Technology, while `B.A.` enrolment expanded to **13.30M**.
* **Premier Institutions & Faculty Glass Ceiling:** Across Institutes of National Importance (`159` campuses), female student share rose **+10.52 pp** (`19.64%` to `30.16%`), while implied PTR widened (`14.04` to `17.79`), Full Professor share fell **-9.28 pp**, and national Ph.D. share fell to **13.39%**. Nationally, the female Professor-minus-Assistant Professor gap remained unchanged across 12 years (`-14.06 pp` to `-14.04 pp`).

### D. Labour-Market Absorption (`2009–10` to `2025`) — [`data_aggregated/1A_6_PLFS_Labour/`](data_aggregated/1A_6_PLFS_Labour/)
*Manifest:* [`README.md`](data_aggregated/1A_6_PLFS_Labour/README.md) | *Dictionary:* [`1A6_DATA_DICTIONARY.md`](data_aggregated/1A_6_PLFS_Labour/1A6_DATA_DICTIONARY.md)
* **Headline Expansion vs. Job Quality:** Between `2018–19` and `2025`, Age 15+ Usual Status (`PS+SS`) WPR climbed **+10.12 pp** (`47.31%` to `57.43%`) and UR declined to **3.11%**, yet Regular Salaried employment share remained flat (`23.81%` to `23.64%`). **79.39%** of the `+15.53 pp` female WPR surge derived from Own-Account and Unpaid Family work (`84.40%` in Rural India), whereas Diploma-holding women posted the highest salaried conversion (`69.1%` of post-2018 gains).
* **Graduate Over-Qualification & Vocational Payoff:** Unemployment peaks among Graduates (**11.18%**), and **30.52%** of employed Graduates (`+5.85 pp` since `2018–19`) work in low-skill occupations (`NCO 6–9`), led by Uttar Pradesh (`50.35%`) and Bihar (`43.10%`). Holding general education constant in `2025`, Formal Vocational Training lifts Secondary/Higher Secondary regular salaried share by **+17.99 pp** and reduces Diploma/Graduate+ unemployment by **-6.72 pp**.
* **Youth NEET, Working Students & Modern Services:** Youth (`15–29`) NEET stood at **25.85%** in `2025` (dominated by Domestic Duties for young women and Unemployed Searching for young men). Working youth students rose `2.54x` since `2018–19` to **4.16M** (`87.22%` unpaid family helpers in Rural `Q1` vs. `45.6%–45.7%` regular salaried in Urban `Q4–Q5`). Non-salaried gig-exposed proxy workers reached **21.24M** alongside **5.99M** in the Orange/Creative Economy.

---

## 4. Repository Structure

```text
indian-education-pipeline-2009-2026/
├── README.md                                  # Master project overview & published highlights
├── DATA_GOVERNANCE_AND_MOU.md                 # Licensing, privacy & MoU non-redistribution policy
├── LICENSE                                    # MIT License (code & original documentation)
├── .gitignore                                 # Blocks raw microdata, .duckdb, and .parquet files
├── docs/
│   ├── research_framework.md                  # Conceptual design, 4 eras, 5 chains & blind spots
│   ├── data_dictionary.md                     # Canonical 6-source analytical schemas & weighting
│   └── output_dictionary_index.md             # Naming conventions & guide to published CSVs
├── data_aggregated/
│   ├── 1A_1_Macro_Financial/                  # Union Budget & RBI State Finances (10 CSVs + dict)
│   ├── 1A_4_NSSO71_Demand/                    # NSSO 71st Round 2014 Cross-Section (12 CSVs + dict)
│   ├── 1A_5_AISHE_HigherEd/                   # AISHE 7-Wave Higher Education Panel (12 CSVs + dict)
│   └── 1A_6_PLFS_Labour/                      # NSSO/PLFS 6-Wave Labour Panel (12 CSVs + dict)
└── sql/                                       # Reproducible analytical SQL scripts (rolling release)
```

---

## 5. Data Governance, MoU Compliance & Methodological Disclosures

Full licensing, attribution, and privacy rules are documented in [`DATA_GOVERNANCE_AND_MOU.md`](DATA_GOVERNANCE_AND_MOU.md). **No unit-level microdata (ASER, NSSO, PLFS) or raw `.duckdb`/`.parquet` database files are stored or redistributed in this repository.**

* **Macro-Financial Accounting (`1A_1_Macro_Financial`):** Union totals are strictly Net-of-Recoveries (deducting PSK/MUSK reserve-fund double-counting), consolidated national series net out intergovernmental grants (`3601 + 3602`), and every table retains explicit `Actual` / `RE` / `BE` accounting-basis columns.
* **NSSO 71st Round Safeguards (`1A_4_NSSO71_Demand`):** Strictly a January–June `2014` cross-section. `hostel_exp` (`1–35`) records the Hostel Destination State/UT code (excluded from rupee wallet burden and used for inter-state migration); `nature_inst` was canvassed strictly for General Private Unaided students (dual denominators reported); `402` of `639` districts meet the `n >= 30` ranking gate.
* **AISHE Provenance & Population Break (`1A_5_AISHE_HigherEd`):** Each metric draws from a single authoritative `(source_table, response_type)` excluding multi-year retrospective series (`T-40..T-52`). From `2021–22` onward, GER denominators incorporate MoE's revised `2020` `18–23` population projection (`+10.12M` step, explicitly flagged); raw `Tamilnadu` spellings are normalized to `Tamil Nadu`; mode disaggregation for Foreign Students and Out-turn and tuition fees are not canvassed in AISHE.
* **PLFS / NSSO Frame & Activity Rules (`1A_6_PLFS_Labour`):** Uses First-Visit (`V1`) schedules weighted by `final_weight`. Calendar Year `2025` reflects MoSPI's expanded `270,472`-household / `764`-district frame (`revamped_2025_sample_flag = 1`). Occupation (`NCO`) and industry (`NIC`) shares anchor on Principal Status (`PS`) workers while five-bucket job quality covers `100%` of `PS+SS` workers. Gig-exposed and Orange Economy figures are reproducible `NIC-2008` (`2011–25`) structural proxies.
* **School Supply & Rural Ground-Truth Governance:** Longitudinal school supply tables (`UDISE+`, `2014–15` to `2025–26`) are staged for upcoming release alongside historical `2009–12` archive verification. Rural learning and school observation aggregates (`ASER`, `2009–2024`) are governed by a formal ASER Centre Research MoU and remain offline pending written pre-publication review.

---

*Contact: Akash Kumar Chature | [LinkedIn](https://www.linkedin.com/in/akash-chature-b77869168) | [State & Signal (Substack)](https://statesignal.substack.com)*
