# Injury Settlement Value Base

Summary statistics for **30,827 paid personal injury claims** against US government bodies, built from public payment records. Published by [Settlement Value Calculator](https://settlementvaluecalculator.com/).

Each row is a segment (for example "New York City, motor vehicle injury, FY2023") with the number of paid claims, the median, the 10th to 99th percentiles, the mean, and the share of large payouts. There are 87 segments.

Only accident-type personal injury claims are included. Property damage claims, and claim types where injury and property damage cannot be told apart, have been removed. See [What is excluded](#what-is-excluded).

> This is a statistical base, not a case list. It does not contain individual claims, names, or case descriptions. Links to the full public records are in [Sources](#sources).

## What the data shows

- **Averages mislead.** New York City motor vehicle injury claims average $214,576, but the median is $40,000. About 6% of claims were paid $1 million or more.
- **Federal and city crash claims land in the same range.** The median federal traffic claim settled during a lawsuit was $42,500, close to New York City's $40,000.
- **Injury claims take years.** The median New York City motor vehicle injury claim was resolved 3.0 years after the incident.
- **Falls pay less than crashes at the top end.** The 90th percentile for a New York City sidewalk defect claim was $212,500, against $645,000 for a motor vehicle claim.
- **The payer matters.** The median sidewalk defect injury claim in New York City was $35,000; the median fall claim in Chicago (sidewalk, street or premises) was $15,000.

## Key facts

| | |
|---|---|
| Paid personal injury claims summarised | 30,827 |
| Segments | 87 |
| Vehicle-related claims | 8,961 |
| Years covered | 2009 to 2026 (varies by source) |
| Payers | City of New York, City of Chicago, US federal agencies |
| Result types | Settlements, court judgments, administrative awards (medical malpractice only) |
| Formats | CSV, JSON, XLSX |
| Snapshot | 24 November 2026 |
| Licence | Data: CC BY 4.0 |

## Headline figures

| Segment | Paid claims | 25th pct | Median | 75th pct | 90th pct |
|---|---:|---:|---:|---:|---:|
| New York City, motor vehicle (FY2016–2023) | 4,623 | $15,000 | $40,000 | $125,000 | $645,000 |
| New York City, sidewalk defect | 4,412 | $10,000 | $35,000 | $95,000 | $212,500 |
| New York City, roadway defect | 2,154 | $2,500 | $22,500 | $75,000 | $225,000 |
| New York City, school | 2,082 | $12,500 | $30,000 | $98,750 | $325,000 |
| New York City, parks and recreation | 706 | $15,000 | $45,000 | $125,000 | $250,000 |
| New York City, city property / building | 423 | $10,000 | $45,000 | $190,000 | $650,000 |
| New York City, medical malpractice | 1,059 | $80,000 | $225,000 | $509,384 | $1,300,000 |
| Federal traffic claim, lawsuit settlement (FY2009–2026) | 2,496 | $13,000 | $42,500 | $130,000 | $376,250 |
| Federal traffic claim, court judgment | 1,566 | $9,925 | $32,000 | $125,000 | $413,025 |
| Federal premises claim, lawsuit settlement | 385 | $17,500 | $50,000 | $145,000 | $340,000 |
| Federal medical malpractice, all resolutions | 9,444 | $45,000 | $125,000 | $300,000 | $650,000 |
| Chicago, pedestrian struck by vehicle (2012–2026) | 92 | $4,000 | $36,491 | $112,886 | $498,976 |
| Chicago, bicycle accident | 93 | $5,000 | $15,000 | $65,000 | $121,315 |
| Chicago, fall on sidewalk, street or premises | 663 | $5,000 | $15,000 | $46,508 | $94,919 |

Amounts are nominal US dollars.

## Files

| File | Contents |
|---|---|
| `data/settlement-segments.csv` | 87 personal injury segments with counts, percentiles, mean, min, max |
| `data/settlement-segments.json` | The same 87 segments as JSON |
| `data/vehicle-claims-segments.csv` | The 37 vehicle-related segments only (crashes, pedestrian, bicycle, pursuit) |
| `data/amount-bands.csv` | 15 segments with the number of claims in each of nine dollar bands |
| `data/excluded-segments.csv` | 87 segments removed from the base (property damage, mixed and non-accident claims), for reference |
| `data/summary-stats.json` | Snapshot date, totals by source, headline segments |
| `data/settlement-value-base.xlsx` | The same data as an Excel workbook, with notes |
| `docs/REPRODUCE.md` | Exact endpoints, filters and category rules used, so the figures can be re-run |
| `datapackage.json` | Machine-readable schema (Frictionless Data Package) |
| `CITATION.cff` | Citation metadata for GitHub's "Cite this repository" button |
| `CHANGELOG.md` | Release history |
| `LICENSE` | Licence terms for the data |

## Data dictionary

`settlement-segments.csv`

| Field | Meaning |
|---|---|
| `dataset` | Source and period |
| `breakdown` | How the source was split (claim category, fiscal year, borough, resolution type) |
| `segment` | Claim type or cause |
| `sub_segment` | Year, borough or resolution type within the segment |
| `cases` | Number of paid claims with an amount above $0 |
| `total_paid_usd` | Sum of amounts paid |
| `mean_usd` | Average amount paid |
| `min_usd`, `max_usd` | Smallest and largest amount paid |
| `p10_usd` … `p99_usd` | 10th, 25th, 75th, 90th, 95th and 99th percentiles |
| `median_usd` | 50th percentile |
| `cases_100k_plus`, `cases_1m_plus` | Paid claims of at least $100,000 and at least $1,000,000 |
| `median_years_to_resolution` | Median years from incident to resolution (New York City only) |

`amount-bands.csv` gives claim counts in these bands: under $5k, $5k–$10k, $10k–$25k, $25k–$50k, $50k–$100k, $100k–$250k, $250k–$500k, $500k–$1M, $1M and over.

## Coverage by source

| Source | Paid claims | Period | What is included |
|---|---:|---|---|
| New York City, personal injury claims | 15,556 | FY2016–FY2023 | Motor vehicle, sidewalk defect, roadway defect, traffic control device, school, parks and recreation, city property, medical malpractice. Year and borough splits for the four largest. |
| US Treasury Judgment Fund, traffic | 4,062 | FY2009–FY2026 | Federal traffic claims resolved in a lawsuit, by settlement or court judgment, with yearly figures |
| US Treasury Judgment Fund, other | 10,270 | FY2009–FY2026 | Medical malpractice (9,370); property maintenance (691) and aircraft crash (209), lawsuit resolutions only |
| City of Chicago | 939 | 2012–Aug 2026 | Falls, bicycle accidents, pedestrians struck by vehicles, police pursuit crashes |

## What is excluded

The source records hold about 126,000 paid claims. Roughly 95,000 were removed so that the base reflects personal injury values only. The removed segments are in `data/excluded-segments.csv`.

| Removed | Why |
|---|---|
| New York City vehicle property damage (12,170 claims) | Property damage only |
| Chicago "property damage only" crashes (1,682) | Property damage only |
| Federal administrative awards for traffic, property maintenance and aircraft claims (about 36,000) | Injury and property-damage-only claims are not distinguished |
| Chicago "crash with city vehicle" and "street condition" categories (2,151) | Injury and property damage are mixed under one label |
| Federal fire and flood claims (230) | Mostly property loss |
| New York City police action, correction facility, civil rights, uniformed services employee and "other" (32,873) | Not accident injury claims |
| Federal miscellaneous tort (7,077) and Camp Lejeune (3,090) | Claim type unknown; Camp Lejeune pays from a fixed government grid |

## Sources

All figures are computed from records published by the paying government body.

- **New York City Comptroller**, Claims Report – Underlying Settlements and Claims Filed Data: https://data.cityofnewyork.us/d/ex6k-ym48
- **US Treasury, Bureau of the Fiscal Service**, Judgment Fund: Annual Report to Congress: https://fiscaldata.treasury.gov/datasets/judgment-fund-report-to-congress/
- **City of Chicago Department of Law**, Judgment and Settlement Payment Requests: https://www.chicago.gov/city/en/depts/dol/supp_info/judgment-and-settlement-payments-requests.html

## Methodology

- **Inclusion.** Only claims with a payment above $0. New York City rows are claims marked settled and coded by the City as personal injury. Chicago rows are payments marked settlement, verdict or judgment. Federal traffic, property maintenance and aircraft rows are limited to claims resolved in a lawsuit.
- **Claim-level amounts.** Federal payment lines sharing a control number and resolution type were added together to give one amount per claim.
- **Category mapping.** New York City's older and newer claim-type labels were merged (for example "Motor Vehicle" and "AUTOMOBILE ACCIDENT (PI)"). Chicago's 657 cause labels were grouped by keyword.
- **Percentiles** use linear interpolation between ranked values.
- **Reproducible.** The endpoints, filters and keyword rules are written out in [`docs/REPRODUCE.md`](docs/REPRODUCE.md).
- **Check against a published figure.** For New York City motor vehicle claims in FY2023 this base gives $173,652,500 in total and a mean of $308,441, matching the Comptroller's annual claims report. The report's median is $65,000; this base computes $60,000.

## Limitations

- **Government defendants only.** Every claim is against a city or a federal agency. Settlements paid by private insurers may differ.
- **No injury detail.** None of the sources records injury type or severity.
- **Limited sub-types.** Only Chicago separates bicycle and pedestrian cases, and those samples are under 100 claims each. No source identifies motorcycle, truck or wrongful death claims.
- **Federal lawsuit resolutions are not labelled injury or property.** Restricting to lawsuits removes most property-only claims, but a small share may remain. "Property maintenance" is the Treasury's own label, read here as premises claims.
- **Federal medical malpractice.** The "all resolutions" row (9,444) includes 74 claims with no resolution type, so it is slightly larger than the three resolution rows combined.
- **Instalments.** Large Chicago payments made in instalments appear as separate rows.
- **Nominal dollars.** No inflation adjustment.

## Using the data

```python
import pandas as pd

df = pd.read_csv("data/settlement-segments.csv")
mv = df[(df.segment == "Motor vehicle") & (df.sub_segment == "All years")]
print(mv[["cases", "p25_usd", "median_usd", "p75_usd"]])
```

## How to cite

Settlement Value Calculator, *Injury Settlement Value Base*, 24 November 2026 snapshot. https://settlementvaluecalculator.com/

```bibtex
@misc{svc2026settlementbase,
  author = {{Settlement Value Calculator}},
  title  = {Injury Settlement Value Base},
  year   = {2026},
  note   = {24 November 2026 snapshot},
  url    = {https://settlementvaluecalculator.com/}
}
```

## Licence

The data in this repository is released under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). You may use and adapt it, including commercially, if you credit Settlement Value Calculator and link to https://settlementvaluecalculator.com/. The underlying records are published by the government bodies listed in Sources.

## Disclaimer

This data is general information, not legal advice. Past payments do not predict the value of any individual claim. Talk to a licensed attorney about your own case.

## About

[Settlement Value Calculator](https://settlementvaluecalculator.com/) is a free accident settlement estimator covering all 50 states and DC.
