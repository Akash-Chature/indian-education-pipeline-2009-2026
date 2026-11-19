# Two Decades of the Indian Education Pipeline (2009–2026)
### Structural Policy Shifts, Foundational Realities, and Labor Market Outcomes

**Author:** Akash Kumar Chature | **Status:** Active Empirical Research (Phase 1 — Quantitative Baselines)  
**Target Release:** Flagship Working Paper & Executive Dashboard Suite (January 2027)

---

## Executive Overview
Public debates on India’s education system frequently break down because institutional datasets operate in silos: fiscal reports stop at rupees disbursed, school censuses stop at paper enrollment, and labor surveys mask graduate underemployment behind headline participation ratios.

This open-science repository documents a harmonized, multi-agency empirical architecture uniting **20M+ administrative and unit-level survey records** across **six Indian public datasets** spanning four chronological policy eras:
1. **RTE Era (2009–2014):** Universal access expansion under the Right to Education Act.
2. **Pre-COVID Benchmark (2014–2019):** Pre-disruption institutional and learning baselines.
3. **Post-COVID & Pre-NEP (2020–2023):** Shock, private-to-public migration, and EdTech cycle.
4. **Post-NEP Era (2024–2026):** Consolidation, NCF 2023 transition, and current ground realities.

---

## Six-Lakehouse Data Architecture (Certified & Sealed)

| Lakehouse | Source Agency | Coverage & Scale | Analytical Mandate |
| :--- | :--- | :--- | :--- |
| **1. Macro-Financial** | Union Budget & RBI State Finances | 10 waves (2009–10 to 2025–26); 31 States/UTs | Net-of-recovery Central grants (`3601`/`3602`) & real deflated State Revenue (`2202`) vs. Capital (`4202`) |
| **2. UDISE+** | Ministry of Education (MoE) | 2009–10 to 2025–26 (12.1M+ school records; incl. 2023–24 P2) | School counts, 94,000+ govt closures vs. private unaided expansion, PTR, grants & compliance |
| **3. ASER Microdata** | ASER Centre *(Formal Research MoU)* | 6 waves (2009–2024); 3.50M child & school audit rows | Ground-truth rural reading/math testing & unannounced govt school usability/attendance |
| **4. NSSO 71st Round** | MoSPI (Schedule 25.2) | 2014 Cross-Section (470,266 weighted rows) | Out-of-pocket costs, shadow coaching tax, distance decay, Govt-to-Private switching & Grades 9–10 attrition |
| **5. AISHE** | Ministry of Education (MoE) | 2011–12 to 2023–24 (8 canonical tables) | Tertiary capacity, discipline GER/GPI, private affiliated vs. public split, Regular vs. Distance & PTR |
| **6. PLFS / NSSO** | MoSPI (66th, 68th & PLFS) | 6 waves (2009–2025); 4.11M weighted rows | Graduate unemployment, `PS` vs. `PS+SS` underemployment, `NCO`/`NIC` formal vs. gig tiers |

---

## Core Empirical Modules (Chains of Reason)
- **Module 1 (Fiscal-to-Usability):** Does real state Capital Outlay (`4202`) and Central transfer (`3601`) close the gap between UDISE+ paper facilities and ASER unannounced usability?
- **Module 2 (Learning Deficit & Household Exit):** Stress-testing NITI Aayog’s May 2026 school consolidation thesis against private expansion, ASER learning stagnation, and NSSO 71st shadow-coaching substitution.
- **Module 3 (Secondary Cliff & Tertiary Funnel):** Separating school distance shocks from economic opportunity costs at the Grades 9–10 leakage node, and mapping federal splits in AISHE capacity.
- **Module 4 (Credential-to-Absorption):** Testing whether tertiary out-turn converts into formal high-skill employment (`NCO`/`NIC`) or disguised graduate underemployment (`PS` vs. `PS+SS`).

---

## Data Governance & MoU Compliance
**No raw unit-level microdata is stored or redistributed in this repository.**  
ASER household and school microdata (2009–2024) are accessed under a formal non-redistribution Memorandum of Understanding (MoU) with ASER Centre (Pratham). MoSPI unit-level microdata (NSSO 71st, 66th, 68th, PLFS) are governed by NADA licensing terms. This repository publishes **only** canonical schema documentation, reproducible SQL/analytical logic, methodological notes, and aggregated state/district summary tables. See [`DATA_GOVERNANCE_AND_MOU.md`](./DATA_GOVERNANCE_AND_MOU.md) for full details.

---

## Roadmap & Upcoming Releases
- [x] **Infrastructure:** 6 DuckDB + ZSTD Parquet lakehouses built, audited & sealed.
- [ ] **Stage 1A (Late Nov – Early Dec 2026):** Single-institution baseline audits & aggregated CSV exports.
- [ ] **Wedge Brief (Mid-Dec 2026):** Empirical audit of NITI Aayog’s May 2026 school education report + Looker Studio Dashboard #1.
- [ ] **Flagship Working Paper (Jan 2027):** Complete monograph (LaTeX/SSRN) + Master Dashboard Suite.

*Contact: Akash Kumar Chature | [LinkedIn](https://www.linkedin.com/in/akash-chature-b77869168) | Substack: [Link]*
