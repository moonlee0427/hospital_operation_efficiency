# Hospital Operational Efficiency Across U.S. Regions: Population Aging and ED Performance

**Team 10 | Regional emergency department performance, hospital capacity, and population aging**

**Status:** Final project notebook with saved results; source CSVs are required to rerun the analysis.

## Contents

- [What is it?](#what-is-it)
- [Questions to answer](#questions-to-answer)
- [Tools used](#tools-used)
- [Methodology](#methodology)
- [What did we find?](#what-did-we-find)
- [So what](#so-what)
- [How to run](#how-to-run)
- [Limitations and next steps](#limitations-and-next-steps)
- [Team and acknowledgments](#team-and-acknowledgments)

---

## What is it?

This project tackles a central question in U.S. healthcare: **how do emergency department (ED) visit times and hospital resources differ across U.S. regions, and how do those differences relate to an aging population?**

We integrate public hospital-performance, facility, population, Medicare-enrollment, and rural–urban classification data to explore regional disparities and identify priorities for further operational investigation.

### Who is this for?

This project is a brief analysis of how hospital resources and ED performance are distributed across U.S. regions, and what that may mean for an aging population, a group with high healthcare needs. We aim to support:

- **Anyone seeking to understand and improve ED performance and hospital capacity**, by giving them a deeper look at what can be improved.
- **State health departments**, by helping them decide when and where to investigate first.

This is an exploratory benchmarking study, not a causal evaluation or a tested intervention.

> **Scope:** The ED measures cover patients of all ages, not exclusively adults aged 65+. Older-population and Medicare enrollment data provide demographic context only. We cannot directly measure older patients' ED experiences, but we identify regional patterns that may be relevant to planning care for an aging population. Our main measure, `OP_18b`, captures ED visit duration from arrival to departure for eligible discharged patients, not the wait to see a clinician.

---

## Questions to answer

**Objective:** compare ED performance and hospital capacity across the Northeast, Midwest, South, and West, with additional comparisons by state, rural–urban setting, hospital size, and ownership.

1. Where are ED visits longest, and how have regional patterns changed over time?
2. How do rural and urban hospitals compare?
3. Do regional differences persist when hospitals are grouped by bed count?
4. How do hospital availability and bed supply compare with population size and aging?
5. Does a larger older-population share consistently correspond to longer ED visits?

---

## Tools used

| Category | Tools |
| --- | --- |
| Environment | Google Colab, Google Drive, Jupyter Notebook |
| Language and data wrangling | Python, `pandas` |
| Visualization | `matplotlib`, `seaborn`, `plotly` (including state choropleth maps) |

### Data sources

| Source | Data used | Role in the analysis |
| --- | --- | --- |
| [CMS Hospital Compare via NBER](https://www.nber.org/research/data/centers-medicare-medicaid-services-cms-hospital-compare-data) | Timely and Effective Care by Hospital (2020–2026) and by State (2020–2025) | ED performance, time trends, geographic comparisons |
| [CMS Medicare Monthly Enrollment](https://data.cms.gov/summary-statistics-on-beneficiary-enrollment/medicare-and-medicaid-reports/medicare-monthly-enrollment/data) | June 2026 download | Medicare enrollment among adults aged 65+ |
| [U.S. Census Bureau](https://www.census.gov/data/tables/time-series/demo/popest/2020s-state-detail.html) | State population estimates by age, 2020–2025 | Total population, older-population share, per-capita denominators |
| [CMS Provider of Services](https://data.cms.gov/provider-characteristics/hospitals-and-other-facilities/provider-of-services-file-quality-improvement-and-evaluation-system) | Q2 2026 facility snapshot | Hospital beds, ownership, facility identifiers |
| [USDA RUCA codes](https://www.ers.usda.gov/data-products/rural-urban-commuting-area-codes) | 2020 ZIP-code classification | Rural–urban hospital comparisons |

**Release year is not the same as measurement year.** The 2026 hospital release contains measures covering earlier periods; trend analyses derive years from reporting-period end dates.

---

## Methodology

**Pipeline in four steps**

1. **Clean:** standardize states, columns, dates, and numbers; normalize ZIP codes and CMS facility IDs.
2. **Filter:** apply score and footnote exclusions (missing scores are *not* imputed).
3. **Merge:** join hospitals to RUCA (by ZIP) and facility data (by CMS ID); join population, Medicare, and state performance by state or state-year, with merge validation.
4. **Analyze:** totals, medians, per-capita rates, standardized scores, and visual comparisons.

**Key measures**

| Measure | Meaning | Better is |
| --- | --- | --- |
| `OP_18b` | Median ED visit duration (minutes) | Lower |
| `OP_22` | % of ED patients who leave before being seen | Lower |
| Hospitals per 100,000 residents | Hospital availability | — |
| Beds per 1,000 residents | Reported bed supply (not staffed ED capacity) | — |
| % population aged 65+ | Older residents ÷ total residents | — |

The notebook also compares `OP_18a`, `OP_18c`, and `OP_23` where available.

**Hospital size groups:** under 100 beds · 100–299 beds · 300+ beds. Bed count is not a direct measure of ED patient volume.

---

## What did we find?

### Visualization

The notebook ([A10-Hospital-Operational-Efficiency-for-Elderly-Across-US-Regions.ipynb](A10-Hospital-Operational-Efficiency-for-Elderly-Across-US-Regions.ipynb)) includes the following visuals.

| Visual | Shows | Notebook question |
| --- | --- | --- |
| Regional performance heatmap | ED measures by region | Q6 |
| Time-trend lines | Regional ED visit time over reporting years | Q7 |
| Rural vs. urban bars | ED visit time by RUCA setting | Q8 |
| State choropleth maps | ED visit time, beds, older residents, Medicare enrollment by state | Q9–Q17 |
| Within-size regional bars | ED visit time by region and bed-size group | Q13–Q14 |
| Scatterplots | Older-population share vs. ED visit time | Q10–Q11 |

![alt text](image.png)

### Key insights

- **Northeast ED visits are longest:** median `OP_18b` is about **195 min** in the Northeast vs. **133 min** in the Midwest (Q6).
- **The gap persists by hospital size:** among 300+ bed hospitals, **219 min** (Northeast) vs. **189 min** (Midwest) (Q14). Bed size alone does not explain the difference.
- **Urban hospitals report longer ED visits than rural hospitals** across the reporting years examined (Q8).
- **Hospital counts and bed supply tell different stories:** the Northeast has the fewest hospitals per 100,000 residents (**0.93**) but larger hospitals; the West has the lowest beds per 1,000 residents (**2.59**).
- **Aging shows no clear state-level link to ED duration** in the exploratory scatterplots. This does not establish that age is unrelated to an individual patient's experience.

### Regional resource snapshot

| Region | Hospitals per 100,000 residents | Reported beds per 1,000 residents | Population aged 65+ |
| --- | ---: | ---: | ---: |
| Northeast | 0.93 | 3.36 | 20.11% |
| Midwest | 1.95 | 3.68 | 19.56% |
| South | 1.31 | 3.33 | 18.61% |
| West | 1.19 | 2.59 | 18.25% |

Based on 2025 population estimates and the hospital/facility snapshots selected in the notebook. Regional bed rates are total beds divided by total population; they differ from an unweighted average of state rates (e.g., Midwest: **3.68** regional vs. **3.81** in Q12).

*Note on samples:* Q6 uses eligible hospitals with reported `OP_18b` scores in the cleaned 2026 release (no provider-file match required). Q14 uses hospitals matched to the Provider of Services file with a bed-size category. The two are separate samples.

---

## So what

| Finding | What it means for decision-makers |
| --- | --- |
| Persistent Northeast delays | Prioritize the Northeast for investigation. Collect staffing, ED arrival volume, occupancy, and boarding data to test operational explanations instead of assuming size is the cause. |
| Urban hospitals are slower | Benchmark urban hospitals against comparable peers on ED duration and `OP_22`, then identify practices at better-performing hospitals. |
| Beds and hospital counts diverge | Judge capacity with several indicators together (hospital counts, beds per hospital, beds per resident). Verify staffed beds, occupancy, and access before recommending expansion, especially in the West. |
| Aging is not a clear driver of ED duration at the state level | Use growing older populations and Medicare enrollment as planning context, and seek age-stratified ED measures before drawing conclusions about older adults' care. |

These are priorities for investigation and planning, not estimates of the effects of specific interventions.

---

## How to run

### Recommended: Google Colab

The notebook is configured for Google Colab with Google Drive. It is **not fully self-contained**: rerunning it requires the source CSV files.

1. Open the notebook in Google Colab.
2. Download and place the required CSV files in an accessible Google Drive folder.
3. Update **every path in the initial data-loading cell**.
4. Run the import and Drive-mount cell and authorize access.
5. Run all remaining cells top to bottom; later analyses depend on earlier tables.

### Required input files

The data-loading cell references 18 CSV files (names may be case-sensitive):

| Dataset | Expected filename(s) |
| --- | --- |
| Population | `sc-est2025-agesex-civ.csv` |
| RUCA | `RUCA-codes-2020-zipcode.csv` |
| Hospital performance | `2020timely_and_effective_care_hospital.csv` through `2026timely_and_effective_care_hospital.csv` (seven files) |
| Rural Emergency Hospitals | `reh2026_timely_and_effective_care_hospital.csv` |
| State performance, 2020 | `timely_and_effective_care_state-2020.csv` |
| State performance, 2021 | `Timely_and_Effective_Care-State_2021.csv` |
| State performance, 2022 | `timely_and_effective_care_state-2022.csv` |
| State performance, 2023–2025 | `timely_and_effective_care_state_2023.csv`, `..._2024.csv`, `..._2025.csv` |
| Medicare enrollment | `Medicare_Monthly_Enrollment_Jun_2026.csv` |
| Provider facilities | `Hospital_and_other_DATA_Q2_2026_RENAMED.csv` |

Newly downloaded files may need renaming or column harmonization, especially the renamed provider file. Newer source releases may produce different results.

### Local Jupyter alternative

Install the libraries and Jupyter, remove the `google.colab` import and Drive-mount commands, and replace the Drive paths with local paths.

### Notebook guide

The notebook contains the problem statement, executive summary, methods, data dictionary, cleaning and merging workflow, 21 exploratory questions, conclusions, challenges, references, and individual generative-AI disclosures.

| Questions | Analysis theme |
| --- | --- |
| Q1–Q5 | Medicare enrollment, hospital availability, aging, and regional bed capacity |
| Q6–Q8 | Regional ED performance, time trends, and rural–urban comparisons |
| Q9–Q12 | State ED visit times and hospital-bed availability |
| Q13–Q14 | Hospital size and within-size regional comparisons |
| Q15–Q17 | Geographic distribution of beds, older residents, and Medicare enrollment |
| Q18–Q20 | Ownership, hospital size, and regional distribution |
| Q21 | Regional bed availability alongside population aging |

---

## Limitations and next steps

- **Age mismatch:** All-age ED outcomes cannot show how patients aged 65+ experience care. Obtain age-stratified or patient-level outcomes.
- **Descriptive design:** Patterns do not establish causation. Add staffing, case mix, ED volume, and occupancy.
- **Different reporting periods:** Align measurement windows before linking performance with demographic change.
- **Missing and suppressed scores:** Results represent reporting facilities and may reflect nonrandom exclusions. Document coverage by measure, region, and year.
- **Different facility universes:** Hospital-count and bed analyses use different lists. Reconcile eligible facilities before treating them as one capacity estimate.
- **Aggregation:** A median of hospital medians is not a patient-weighted regional median; an average of state rates is not a population-weighted regional rate.
- **Capacity proxies:** Total beds do not measure staffed beds, ED treatment spaces, or available capacity.
- **Reproducibility:** Original CSVs and a pinned software environment are needed for a verified rerun.

---

## Team and acknowledgments

**Team 10**

- Ateh Nji-Tanji
- Lujia Shao (Lydia)
- Sophia Lee
- Vicky Ye
- Muntira Samalapa (View)

**Sophia Lee's contribution:** Data cleaning and visualizations for the hospital- and state-level Timely and Effective Care datasets; Data Merging 2, 3, and 6; and contributions to Data Merging 1 and 4.

We acknowledge CMS, NBER, the U.S. Census Bureau, and USDA for the public data used in this analysis. Source references and individual generative-AI disclosures are at the end of the notebook.

This README summarizes the supplied notebook and its saved outputs. It does not indicate that the analysis has been independently rerun.
