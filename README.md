# Harrow GP Population Health Simulator

A browser-based synthetic population health simulator modelling a 10,000-patient GP list with demographic characteristics representative of **Harrow, North West London** — one of the most ethnically diverse boroughs in England.

🔗 **[Try it live →](https://davidlloyd73-cell.github.io/gp-population-simulator/)**

---

## What it does

The simulator runs a **monthly tick engine** across a synthetic patient population, modelling:

- **Disease incidence and progression** — hypertension, type 2 diabetes, atrial fibrillation, heart failure, CKD, COPD, asthma, depression, cancer, dementia, stroke, and more
- **Mortality** — age- and sex-adjusted ONS life table rates with comorbidity multipliers drawn from published trial data
- **QRISK3-based cardiovascular risk** — simplified scoring used to drive prescribing decisions
- **CHA₂DS₂-VASc scoring** for AF anticoagulation
- **Ethnicity-adjusted prevalence** — South Asian CVD uplift, Pakistani/Bangladeshi 3× T2DM risk, Black hypertension elevation, all calibrated against QOF 2022/23 national data

---

## Interventions (combinable)

| Intervention | Evidence basis | Effect modelled |
|---|---|---|
| 💊 Polypill | TIPS / PolyIran trials | −25% MI, −20% stroke for QRISK >10 |
| 🩺 Intensive DM | DAPA-HF, EMPEROR, UKPDS | SGLT2i / GLP-1; −22% DM mortality, −50% HF risk |
| 🚭 Smoking cessation | ASH / NICE | 3%/month quit rate with programme support |
| 🎗️ Cancer elimination | Hypothetical | Removes existing cancer; suppresses new diagnoses |
| 🦠 COVID wave | ONS COVID-19 mortality data | 3-month mortality spike; Long COVID seeding |
| 📉 Deprivation shock | Public health modelling | +50% DM incidence, +60% depression, BMI creep, +12% mortality |
| ✅ Perfect NICE | NICE NG136 / NG28 / NG196 / NG106 | Optimises all prescribing every tick |

Any combination of interventions can be active simultaneously, and **scenario capture** lets you overlay mortality curves for direct comparison.

---

## Demographics

Sliders allow adjustment of:
- South Asian %, including Pakistani/Bangladeshi sub-fraction
- Black / Afro-Caribbean %
- White British % (auto-calculated)
- Other %
- Deprivation index (1–5)
- Baseline smoking prevalence
- Mean BMI offset

Changes take effect on population rebuild.

---

## Charts

- **Population pyramid** — live age/sex distribution
- **Cumulative mortality curve** — with scenario overlay for intervention comparison
- **Condition prevalence trends** — tracked over simulated time

---

## CSV export

At any point during a simulation run, you can export:

- **All 10,000 patients** — full dataset including conditions, medications, vitals, QRISK, and death data
- **Deaths only** — filtered to deceased patients with month and year of death, for analysis of contributory factors

Exported files are automatically named to include the simulation month and active interventions — e.g. `harrow-gp-sim_deaths_mo36_polypill-intensiveDM.csv`

---

## Clinical foundations

| Domain | Source |
|---|---|
| Mortality rates | ONS England & Wales life tables |
| Comorbidity multipliers | UKPDS, DAPA-HF, EMPEROR, EMPA-REG |
| Condition prevalence | QOF 2022/23 national averages |
| Ethnicity risk | Bhopal et al. (CVD); Bradford/Leicester cohort (T2DM); UK Biobank (HTN) |
| NICE guidelines | NG136 (HTN), NG28 (T2DM), NG196 (AF), NG106 (HF), NG115 (COPD) |

---

## Limitations

- Synthetic patients only — not derived from real individual records
- Simplified QRISK scoring (not the validated QRISK3 algorithm)
- No socioeconomic gradient modelled within ethnic groups
- Drug side-effects and discontinuation not modelled
- Multi-morbidity interaction effects are simplified
- **Not validated for clinical decision-making or service commissioning**

---

## Technical notes

- Single HTML file — no server, no dependencies, no installation
- Seeded Mulberry32 RNG for reproducible results
- Chart.js 4.4.1 for visualisation
- Runs entirely in the browser; no data leaves your machine

---

## Background

Built at a **Harrow, NW London GP practice** to support population health education, public health training, and scenario planning discussions among clinical colleagues.

The practice has a long history of NHS innovation — including co-founding [Harmoni](https://en.wikipedia.org/wiki/Harmoni), the GP out-of-hours cooperative that became the largest OOH provider in the UK, and running the Alexandra Avenue Walk-in Centre, at one point the 36th busiest urgent care site in the country.

Developed with **[Claude AI](https://claude.ai)** (Anthropic).

---

## Disclaimer

⚠️ **Educational and research use only.** This simulator uses synthetic data and approximated clinical parameters. It is not validated for clinical decision-making, service planning, or commissioning. Results should not be used to guide treatment of individual patients or to make resource allocation decisions without reference to primary evidence and local epidemiological data.

---

## Licence

Open source — freely shareable and adaptable. If you build on this, please retain the methodology note and limitations section. Feedback and pull requests welcome.
