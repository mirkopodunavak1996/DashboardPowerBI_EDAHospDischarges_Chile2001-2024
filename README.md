# Hospital Discharges in Chile (2001-2024) Dashboard

An interactive Power BI dashboard built on 25.7 million hospital discharge records (2001–2024), designed for four different audiences: hospitals, pharmaceutical companies, government health authorities, and patient associations.

**[Open the live dashboard](https://app.powerbi.com/view?r=eyJrIjoiMTY3MzFmYjEtNTUwZS00ZTYxLTkyNmMtNTc5MDhiMzY3ODUzIiwidCI6IjI1YjAxNzk3LTg1NTAtNGFhNC05MTk0LTczMWNjY2Q0NDY0ZSJ9)** — opens directly in your browser, no account or installation needed. Opening it on a laptop gives a better view than on mobile phones.

---

## About the project

Each stakeholder interacts with hospital discharge data differently, so rather than building one generic report, this dashboard has four purpose-built views, all sharing the same underlying data model and a synced set of filters (diagnosis, by ICD-10 chapter/group/individual code, and year).

### Hospitals
For an individual facility reviewing its own activity. Covers length of stay (trend and national benchmark), diagnosis prevalence over time, mortality rate, and patient profile (age, sex, health insurance).

### Pharma
For a company evaluating a diagnosis of interest. Covers patient profile for the selected diagnosis, hospital distribution, and the public/private hospital market split over time.

### Government / Ministry of Health
A system-level view: disease burden over time (volume and relative share), length-of-stay differences between public and private hospitals, mortality by age group, and health insurance coverage.

### Patient Associations
An advocacy-oriented view: how many patients are affected and how that's changed over time, an interactive patient profile (age, sex/insurance, and count/mortality/LOS, all toggleable), which hospitals treat the condition most, and how the disease's total system burden (bed-days) ranks nationally against all other diagnoses.

---

## Data

- **Source:** Hospital discharge records, 2001–2024 (~25.7 million rows) – Ministerio de Salud, Chile ([https://deis.minsal.cl/](https://deis.minsal.cl/))
- **Fields:** sex, age group, health insurance, hospital name (available through 2020) and type (public/private), length of stay (raw and capped), discharge condition, and ICD-10 diagnosis codes at chapter, group, and individual level
- **Known data gaps**, called out directly on the relevant dashboard pages:
  - Hospital name is unavailable from 2021 onward
  - Hospital type is unavailable for 2023 specifically

## Technical notes

- **Star schema:** the original flat table was normalized into a fact table (`Fact_Discharges`) and two dimension tables (`Diagnosis`, `Hospital`) to keep the model performant at scale
- **Exact statistics at scale:** median and percentile (P25/P75) length-of-stay figures are computed exactly, using a frequency-based DAX technique, rather than approximated — direct `MEDIAN()`/`PERCENTILE` calls weren't feasible on the full 25M-row column
- **Dynamic diagnosis hierarchy:** a three-level slicer (Chapter → Group → Individual) lets users drill down or filter broadly, with selections at any level correctly rolling up through every measure
- **Field parameters:** used for interactive toggling between dimensions (e.g., sex vs. health insurance) and metrics (count vs. mortality rate vs. median LOS) without duplicating visuals
- **Disconnected tables + TREATAS:** used to build national ranking charts and grouped comparison visuals that stay stable regardless of which diagnosis a user has filtered to

## Tools

Power BI Desktop, DAX, Power Query (M), Power BI Service (Publish to Web)