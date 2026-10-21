# Hospital Operational Efficiency Across U.S. Regions: Population Aging and ED Performance

**Team 10 | Regional emergency department performance, hospital capacity, and population aging**

**Status:** Final project notebook with saved results; source CSVs are required to rerun the analysis.

How do emergency department (ED) visit times and hospital resources differ across U.S. regions, and how do those differences relate to an aging population?

This project integrates public hospital-performance, facility, population, Medicare-enrollment, and rural–urban classification data to explore regional disparities and identify priorities for further operational investigation.

> **Scope:** The ED measures cover patients of all ages, not exclusively adults aged 65+. Older-population and Medicare enrollment data are used in selected analyses to provide demographic context. With the available data, we cannot directly measure older patients’ ED experiences, but we identify regional patterns that may be relevant to planning care for an aging population. Our main measure, OP_18b, captures ED visit duratio, from arrival to departure for eligible discharged patients, rather than the waiting time to see a clinician.

## Contents

- [Project objective](#project-objective)
- [Key findings](#key-findings)
- [Data sources](#data-sources)
- [Methods and measures](#methods-and-measures)
- [Notebook guide](#notebook-guide)
- [How to run](#how-to-run)
- [Recommendations](#recommendations)
- [Limitations and next steps](#limitations-and-next-steps)
- [Team and acknowledgments](#team-and-acknowledgments)

## Project objective

We compare ED performance and hospital capacity across the Northeast, Midwest, South, and West, with additional comparisons by state, rural–urban setting, hospital size, and ownership.

The analysis is intended for hospital operations leaders, quality-improvement teams, and state health departments deciding **where to investigate delays and resource constraints first**. It is an exploratory benchmarking study, not a causal evaluation or a tested intervention.

Our main questions are:

1. Where are ED visits longest, and how have regional patterns changed over time?
2. How do rural and urban hospitals compare?
3. Do regional differences persist when hospitals are grouped by bed count?
4. How do hospital availability and bed supply compare with population size and aging?
5. Does a larger older-population share consistently correspond to longer ED visits?

## Key findings

- **The Northeast has the longest reported ED visit times.** Q6's heatmap uses eligible hospitals with reported `OP_18b` scores in the cleaned 2026 hospital release, without requiring a match to the provider file. In this sample, the median hospital score is approximately 195 minutes in the Northeast versus 133 minutes in the Midwest.
- **The Northeast remains slower within bed-size categories.** Q14 uses a different sample: hospitals with `OP_18b` scores matched to the Provider of Services file and assigned a bed-size category. Within the 300+ bed group, median ED visit-time scores are 219 minutes in the Northeast and 189 minutes in the Midwest. These within-size comparisons show that bed-size grouping alone does not eliminate regional differences; they are separate from Q6's overall regional medians and do not establish why the differences persist.
- **Urban hospitals report longer ED visits than rural hospitals** across the reporting years examined. Differences in staffing, patient volume, and case mix require additional data to investigate.
- **Hospital counts and bed supply tell different stories.** The Northeast has fewer but larger hospitals; the West has the lowest reported beds per resident.
- **Population aging does not show a clear state-level relationship with ED visit duration** in the exploratory scatterplots. This does not establish that age is unrelated to an individual patient's experience.

### Regional resource snapshot

| Region | Hospitals per 100,000 residents | Reported beds per 1,000 residents | Population aged 65+ |
| --- | ---: | ---: | ---: |
| Northeast | 0.93 | 3.36 | 20.11% |
| Midwest | 1.95 | 3.68 | 19.56% |
| South | 1.31 | 3.33 | 18.61% |
| West | 1.19 | 2.59 | 18.25% |

These calculations use 2025 population estimates and the hospital/facility snapshots selected in the notebook. Hospital counts and bed totals come from different source lists. Regional bed rates are calculated as total regional beds divided by total regional population; they differ from an unweighted average of state bed rates. For example, the Midwest's regional rate is **3.68 beds per 1,000 residents**, while Q12's unweighted average of state rates is **3.81**. Use the former when describing the regional population's overall bed supply.

## Data sources

| Source | Data used | Role in the analysis |
| --- | --- | --- |
| [CMS Hospital Compare via NBER](https://www.nber.org/research/data/centers-medicare-medicaid-services-cms-hospital-compare-data) | Timely and Effective Care by Hospital and by State; hospital release files from 2020–2026 and state files from 2020–2025 | ED performance, time trends, and geographic comparisons |
| [CMS Medicare Monthly Enrollment](https://data.cms.gov/summary-statistics-on-beneficiary-enrollment/medicare-and-medicaid-reports/medicare-monthly-enrollment/data) | June 2026 download; selected annual enrollment records | Medicare enrollment among adults aged 65+ |
| [U.S. Census Bureau](https://www.census.gov/data/tables/time-series/demo/popest/2020s-state-detail.html) | State civilian population estimates by age, 2020–2025 | Total population, older-population share, and per-capita denominators |
| [CMS Provider of Services](https://data.cms.gov/provider-characteristics/hospitals-and-other-facilities/provider-of-services-file-quality-improvement-and-evaluation-system) | Q2 2026 facility snapshot | Hospital beds, ownership, and facility identifiers |
| [USDA Rural–Urban Commuting Area codes](https://www.ers.usda.gov/data-products/rural-urban-commuting-area-codes) | 2020 ZIP-code classification | Rural–urban hospital comparisons |

**Release year is not the same as measurement year.** For example, the 2026 hospital release contains measures covering earlier periods. Trend analyses derive years from reporting-period end dates. Available periods differ by measure.

## Methods and measures

### Data preparation and integration

- Standardize state abbreviations, column names, dates, and numeric fields.
- Preserve and normalize ZIP codes and CMS facility identifiers for joining.
- Aggregate population aged 65+ and the corresponding Medicare enrollment age groups.
- Combine hospital releases and reconcile selected 2026 general-hospital and Rural Emergency Hospital records using address, ZIP code, state, and measure ID.
- Apply score and footnote exclusions to hospital-performance records; missing scores are not imputed.
- Join hospital records to RUCA classifications by ZIP code and to facility information by CMS Certification Number.
- Join population, enrollment, and state performance tables at the appropriate state or state–year level, using merge-cardinality validation.

### Main indicators

| Indicator | Interpretation |
| --- | --- |
| `OP_18b` | Median arrival-to-departure ED visit duration for the measure's eligible discharged-patient population; minutes, lower is better |
| `OP_22` | Percentage of ED patients who leave before being seen; lower is better |
| Hospitals per 100,000 residents | Hospital availability relative to population size |
| Beds per 1,000 residents | Reported hospital-bed supply relative to population size, not staffed ED capacity |
| Population aged 65+ (%) | Older residents divided by total residents |
| Medicare enrollment aged 65+ / population aged 65+ | Aggregate enrollment-to-population ratio, not an individual-level coverage measure |

The notebook also compares `OP_18a`, `OP_18c`, and `OP_23` where available. Consult its data dictionary and CMS measure documentation for measure-specific populations and units.

The analysis uses grouped totals, means, medians, per-capita rates, standardized scores, and visual comparisons. Charts include regional bars, time trends, scatterplots, a performance heatmap, and state choropleth maps. Hospital-size comparisons use three groups: **under 100 beds**, **100–299 beds**, and **300+ beds**. Hospital bed count is not a direct measure of ED patient volume.

## Notebook guide

The main analysis is in [A10-Hospital-Operational-Efficiency-for-Elderly-Across-US-Regions.ipynb](A10-Hospital-Operational-Efficiency-for-Elderly-Across-US-Regions.ipynb).

It contains the problem statement, executive summary, methods, data dictionary, Phase 1 summary, cleaning and merging workflow, 21 exploratory questions, conclusions, challenges, references, and individual generative-AI disclosures.

| Questions | Analysis theme |
| --- | --- |
| Q1–Q5 | Medicare enrollment, hospital availability, aging, and regional bed capacity |
| Q6–Q8 | Regional ED performance, time trends, and rural–urban comparisons |
| Q9–Q12 | State ED visit times and hospital-bed availability |
| Q13–Q14 | Hospital size and within-size regional comparisons |
| Q15–Q17 | Geographic distribution of beds, older residents, and Medicare enrollment |
| Q18–Q20 | Ownership, hospital size, and regional distribution |
| Q21 | Regional bed availability alongside population aging |

## How to run

### Recommended: Google Colab

The notebook is configured for Google Colab and mounts Google Drive. It is **not fully self-contained**: rerunning it requires access to the source CSV files.

1. Open the notebook in Google Colab.
2. Place the required CSV files in an accessible Google Drive folder.
3. Update **every path in the initial data-loading cell** to point to that folder. The current paths use the team's original `/content/drive/MyDrive/Data_team10/2nd Phase /Data set/` directory.
4. Run the import and Drive-mount cell, authorizing access to your own data folder.
5. Run all remaining cells from top to bottom. Later analyses depend on tables created earlier.

The imported analysis libraries are `pandas`, `matplotlib`, `seaborn`, and `plotly`. If needed, install them in a Colab code cell:

```python
%pip install pandas matplotlib seaborn plotly
```

Versions are not pinned in the supplied notebook. Its use of `format='mixed'` requires pandas 2.0 or later; `errorbar=None` in Seaborn requires a compatible modern release.

### Required input files

The data-loading cell references these 18 CSV files. Names are case-sensitive in some environments.

| Dataset | Expected filename(s) |
| --- | --- |
| Population | `sc-est2025-agesex-civ.csv` |
| RUCA | `RUCA-codes-2020-zipcode.csv` |
| Hospital performance | `2020timely_and_effective_care_hospital.csv` through `2026timely_and_effective_care_hospital.csv` (seven files) |
| Rural Emergency Hospitals | `reh2026_timely_and_effective_care_hospital.csv` |
| State performance, 2020 | `timely_and_effective_care_state-2020.csv` |
| State performance, 2021 | `Timely_and_Effective_Care-State_2021.csv` |
| State performance, 2022 | `timely_and_effective_care_state-2022.csv` |
| State performance, 2023–2025 | `timely_and_effective_care_state_2023.csv`, `timely_and_effective_care_state_2024.csv`, `timely_and_effective_care_state_2025.csv` |
| Medicare enrollment | `Medicare_Monthly_Enrollment_Jun_2026.csv` |
| Provider facilities | `Hospital_and_other_DATA_Q2_2026_RENAMED.csv` |

The filenames above are the team's local names. Newly downloaded files may need renaming or column harmonization, especially the renamed provider file. Use the notebook's loading, cleaning, and data-dictionary sections to check the expected schema. Newer source releases may produce different results.

### Local Jupyter alternative

Install the analysis libraries and Jupyter, remove or bypass the `google.colab` import and Drive-mount commands, and replace the Drive paths with local paths. The remaining workflow is organized as sequential Python notebook cells.

## Recommendations

1. **Investigate persistent Northeast delays.** Collect comparable staffing, ED arrival-volume, occupancy, and boarding data to test operational explanations rather than assuming hospital size explains the difference.
2. **Benchmark urban hospitals against relevant peers.** Track ED duration and patients leaving before being seen, then identify practices at better-performing comparable hospitals.
3. **Assess capacity using multiple indicators.** Review hospital counts, beds per hospital, and beds per resident together. Verify staffed beds, occupancy, and access before recommending expansion, particularly in the West.
4. **Bring age-specific outcomes into planning.** Use the observed growth in older populations and Medicare enrollment as context, while seeking age-stratified ED measures before drawing conclusions about older adults' care.

These are priorities for investigation and planning, not estimates of the effects of specific interventions.

## Limitations and next steps

- **Age mismatch:** All-age ED outcomes cannot establish how patients aged 65+ experience care. Obtain age-stratified or patient-level outcomes.
- **Descriptive design:** Regional and subgroup patterns do not establish causation. Add staffing, case mix, ED volume, and occupancy for more informative comparisons.
- **Different reporting periods:** Align measurement windows before comparing trends or linking performance with demographic changes.
- **Missing and suppressed scores:** Results represent reporting facilities and may be affected by nonrandom exclusions. Document coverage by measure, region, and year.
- **Different facility universes:** Hospital-count and bed analyses draw from different lists. Reconcile eligible facilities before treating them as a single capacity estimate.
- **Aggregation:** A median of hospital medians is not a patient-weighted regional median; an average of state rates is not a population-weighted regional rate.
- **Capacity proxies:** Total hospital beds do not measure staffed beds, ED treatment spaces, or immediately available capacity.
- **Reproducibility:** Original input CSVs and a pinned software environment are needed for a verified full rerun. The notebook's existing Google Drive paths must be updated for other users.

## Team and acknowledgments

**Team 10**

- Ateh Nji-Tanji
- Lujia Shao (Lydia)
- Sophia Lee
- Vicky Ye
- Muntira Samalapa (View)

**Sophia Lee's contribution:** Data cleaning and visualizations for the hospital- and state-level Timely and Effective Care datasets; Data Merging 2, 3, and 6; and contributions to Data Merging 1 and 4. 

We acknowledge CMS, NBER, the U.S. Census Bureau, and USDA for the public data used in this analysis. Source references and individual generative-AI disclosures are included at the end of the notebook.

This README summarizes the supplied notebook and its saved outputs. It does not indicate that the analysis has been independently rerun.
