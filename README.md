# Harrow GP Population Health Simulator

A browser-based synthetic population health simulator modelling a 10,000-patient GP list with demographic characteristics representative of **Harrow, North West London** — one of the most ethnically diverse boroughs in England.

🔗 **[Try it live →](https://davidlloyd73-cell.github.io/gp-population-simulator/)**

---

## What it does

The simulator runs a **monthly tick engine** across a synthetic patient population, modelling disease incidence, mortality, prescribing decisions, and the population-level impact of clinical interventions — all in real time, in a single browser tab, with no login, no installation, and no data leaving your machine.

It was built to answer a question every GP and PCN lead faces but rarely has tools to explore: *what actually happens to my population if we do this?*

---

## Interventions

Eight interventions can be activated in any combination:

| Intervention | Evidence basis | Effect modelled |
|---|---|---|
| 💊 Polypill | TIPS / PolyIran trials | −25% MI, −20% stroke for QRISK >10 |
| 🩺 Intensive DM | DAPA-HF, EMPEROR, UKPDS | SGLT2i / GLP-1; −22% DM mortality, −50% HF risk, −50% CKD progression |
| 🚭 Smoking cessation | ASH / NICE | 3%/month quit rate with programme support |
| 🎗️ Cancer elimination | Hypothetical | Removes existing cancer; suppresses new diagnoses |
| 🦠 COVID-19 wave | ONS COVID-19 mortality data | 3-month mortality spike up to 2.8× in elderly; Long COVID seeding |
| 📉 Deprivation shock | Marmot Review; PHE Place-based Health | +50% DM incidence, +60% depression, BMI creep, +12% mortality |
| ✅ Perfect NICE adherence | NG136 / NG28 / NG196 / NG106 / NG115 | Optimises all prescribing every simulation month |
| 🏥 Proactive Care / Frailty Caseload | PRISMA / GRACE / NHS England frailty guidance | Risk-stratified case management for moderate/severe frailty (eFI ≥9) |

### Uptake & adherence sliders

Each clinical intervention has two sliders: **Uptake %** (what proportion of eligible patients receive it) and **Adherence %** (what proportion maintain it). Every effect scales proportionally — dropping polypill uptake from 80% to 50% immediately shows the difference in the mortality curve and the cost panel. This replaces simple on/off toggles with something closer to real-world implementation.

---

## Frailty scoring & proactive caseload

The simulator calculates a simplified **Electronic Frailty Index (eFI)** for every patient, continuously updated each simulation month:

- **Age contribution** — +1 (65–74), +2 (75–84), +3 (85+)
- **High-weight conditions** — Heart Failure, Dementia, Stroke, COPD (+2 each)
- **Medium-weight conditions** — T2DM, CKD, AF, CAD, Cancer (+1 each)
- **Low-weight conditions** — HTN, Depression, Osteoporosis, Hypothyroidism, Asthma, Long COVID, Obesity, Smoking, High deprivation (+0.5 each)
- Maximum score capped at 15

| eFI Score | Band |
|---|---|
| 0–4 | Fit |
| 5–8 | Mild frailty |
| 9–12 | Moderate frailty |
| 13–15 | Severe frailty |

Patients scoring ≥9 (moderate/severe frailty) qualify for the **Proactive Care caseload**. With the intervention active, these patients receive:

- −20% annual mortality reduction (proactive management, medication review, falls prevention)
- −15% MI and stroke risk (cardiovascular risk optimisation)
- −15% new heart failure incidence (fluid management, ACEI/ARNi optimisation)

Evidence basis: PRISMA European trial, Nottingham NHS GRACE programme, NHS England Ageing Well / PCN frailty DES, Marmot Review social care modelling.

The **frailty_score**, **frailty_band**, and **on_proactive_caseload** columns are included in all CSV exports.

---

## Monte Carlo confidence funnel

Click **🎲 Monte Carlo (×20)** to run 20 full simulations with different random seeds. The mortality chart gains a **shaded green band** showing the 10th–90th percentile range of outcomes across all runs, with a dashed median line.

This teaches a critical lesson: population health outcomes are distributions, not single predictions. The width of the band shows how much of the result is driven by chance — and how much is genuinely attributable to the intervention.

Runs asynchronously with a progress bar. Takes roughly 30–60 seconds depending on your device.

---

## Cost & Value Analysis

When any clinical intervention is active, a **💰 Cost & Value Analysis** panel appears automatically. For each active intervention it calculates:

- **Patients treated** — eligible population × your uptake slider
- **Annual cost** — drug tariff / programme cost per patient
- **Admissions avoided per year** — trial effect sizes × uptake × adherence
- **Estimated savings** — avoided admissions × NHS reference costs
- **Net position** — green if cost-saving, red if net cost, with ROI %

### Reference costs used

| Item | Cost |
|---|---|
| Polypill | £165/pt/yr |
| SGLT2i (intensive DM) | £450/pt/yr |
| GLP-1 (intensive DM) | £1,200/pt/yr |
| Smoking cessation | £300/quitter |
| Proactive care programme | £500/pt/yr |
| MI admission | £5,500 |
| Stroke admission | £7,000 |
| Heart failure admission | £3,400 |
| DM complication | £2,800 |
| COPD admission | £3,200 |
| Blended frailty admission | £3,800 |

Six summary KPI tiles show total patients treated, total annual cost, total estimated savings, net position, admissions avoided per year, and cost per admission avoided.

All figures update in real time as you adjust uptake and adherence sliders.

> ⚠️ Indicative estimates only. NHS National Schedule of NHS Costs 2023/24 and BNF/Drug Tariff 2024. Not for commissioning decisions without validated health economic modelling.

---

## Scenario comparison

**📸 Save Mortality Curve** captures the current cumulative death trajectory at any point. Multiple saved scenarios overlay on the mortality chart as dashed coloured lines, allowing direct visual comparison — e.g. baseline vs polypill vs polypill + intensive DM vs proactive care.

---

## Demographics

Adjustable sliders allow the population to be reshaped before running:

- South Asian % — including Pakistani/Bangladeshi sub-fraction (3× T2DM risk)
- Black / Afro-Caribbean %
- White British % (auto-calculated)
- Other %
- Deprivation index (1–5)
- Baseline smoking prevalence
- Mean BMI offset

Changes take effect on **🔄 Rebuild Population**.

---

## CSV export

At any simulation timepoint, export the full dataset:

- **📥 Export All CSV** — all 10,000 patients with 39 variables: demographics, conditions, medications, vitals, QRISK, frailty score, frailty band, proactive caseload status, survival status, death month/year, active interventions
- **💀 Deaths Only** — filtered to deceased patients for mortality risk factor analysis

Filenames are automatically descriptive: `harrow-gp-sim_deaths_mo36_polypill-intensiveDM.csv`

---

## Charts

- **Population pyramid** — live age/sex distribution, updates each month
- **Cumulative mortality curve** — scenario overlay + Monte Carlo confidence band
- **Condition prevalence trends** — 15 conditions tracked over simulated time

---

## Clinical foundations

| Domain | Source |
|---|---|
| Mortality rates | ONS England & Wales life tables |
| Comorbidity multipliers | UKPDS, DAPA-HF, EMPEROR, EMPA-REG |
| Condition prevalence | QOF 2022/23 national averages |
| Ethnicity risk | Bhopal et al. (CVD); Bradford/Leicester cohort (T2DM); UK Biobank (HTN) |
| NICE guidelines | NG136 (HTN), NG28 (T2DM), NG196 (AF), NG106 (HF), NG115 (COPD) |
| Intervention effects | TIPS, PolyIran, DAPA-HF, EMPEROR-Reduced, EMPA-REG, Doll & Hill, REACT-2 |
| Frailty modelling | PRISMA trial; NHS England GRACE programme; Marmot Review |
| Frailty index | NHS Electronic Frailty Index (eFI) — simplified implementation |
| Admission costs | NHS National Schedule of NHS Costs 2023/24 |
| Drug costs | BNF / NHS Drug Tariff 2024 |

---

## Limitations

- Synthetic patients only — not derived from real individual records
- Simplified QRISK scoring (not the validated QRISK3 algorithm)
- Simplified eFI — not the validated 36-deficit Electronic Frailty Index
- No socioeconomic gradient modelled *within* ethnic groups
- Drug side-effects, discontinuation, and polypharmacy burden not modelled
- Multi-morbidity interaction effects are simplified
- Admission cost estimates are indicative; true NHS costs vary by complexity and setting
- Frailty intervention effects are modelled at population level — individual trajectories are not tracked
- **Not validated for clinical decision-making, service planning, or commissioning**

---

## Technical notes

- Single HTML file (~95 KB) — no server, no framework, no dependencies except Chart.js CDN
- Mulberry32 seeded pseudo-random number generator — any run is exactly reproducible
- Monte Carlo engine runs 20 × 60-month full simulations asynchronously in the browser
- Chart.js 4.4.1 — population pyramid, mortality curves with shaded confidence bands, prevalence trends
- eFI recalculated dynamically each tick from current patient state
- Runs entirely client-side; no patient data is transmitted anywhere

---

## Background

Built at a **Harrow, NW London GP practice** to support population health education, GP registrar training, and PCN scenario planning.

The practice has a long history of NHS innovation — including co-founding [Harmoni](https://en.wikipedia.org/wiki/Harmoni), the GP out-of-hours cooperative that grew to become the largest OOH provider in the UK, and running the Alexandra Avenue Walk-in Centre, at its peak the 36th busiest urgent care site in the country.

Developed with **[Claude AI](https://claude.ai)** (Anthropic). Built in a week. Proof that the barrier to population health modelling need not be a six-figure contract.

---

## Disclaimer

⚠️ **Educational and research use only.** This simulator uses synthetic data and approximated clinical parameters. It is not validated for clinical decision-making, service planning, or commissioning. Results should not be used to guide treatment of individual patients or to make resource allocation decisions without reference to primary evidence, validated models, and local epidemiological data.

---

## Licence

Open source — freely shareable and adaptable. If you build on this, please retain the methodology note and limitations section. Feedback and pull requests welcome.
