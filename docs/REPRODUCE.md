# How the figures were produced

Every number in this repository comes from three public sources. This page records the exact endpoints and filters used for the 24 November 2026 snapshot, so anyone can re-run the queries.

## 1. New York City claims

Dataset: Claims Report – Underlying Settlements and Claims Filed Data (256,923 rows, last updated 13 January 2025).

- API: `https://data.cityofnewyork.us/resource/ex6k-ym48.json`
- CSV: `https://data.cityofnewyork.us/api/views/ex6k-ym48/rows.csv?accessType=DOWNLOAD`
- Fields used: `fiscal_year_fy_`, `claim`, `borough`, `occurrence_date`, `disposition_date`, `claim_type`, `disposition_amount`, `claim_action`

Filters:

- Personal injury: `claim_action = 'SETTLED'`, characters 5–6 of `claim` equal `PI`, `disposition_amount > 0` → 48,429 rows
- Property damage: same, with `PD` → 15,044 rows, of which 12,170 are `Motor Vehicle` or `AUTOMOBILE ACCIDENT`

Claim-type labels merged:

| Category | Labels in the source |
|---|---|
| Motor vehicle | `Motor Vehicle`, `AUTOMOBILE ACCIDENT (PI)` |
| Sidewalk defect | `Sidewalk`, `DEFECTIVE SIDEWALK (PI)` |
| Roadway defect | `Roadway`, `DEFECTIVE ROADWAY (PI)` |
| Traffic control device | `Traffic Control Device`, `DEFECTIVE TRAFF/LIGHT/SIGN (PI)` |
| School | `School`, `SCHOOL (PI)` |
| Medical malpractice | `Medical Malpractice`, `MEDICAL MALPRACTICE (PI)` |
| Parks and recreation | `Parks & Recreation`, `RECREATION (PI)` |
| City property / building | `City Property`, `BUILDING AND PROPERTY (PI)` |
| Police action | `Police Action`, `PEACE OFFICER/POLICE ACTION (PI)` |
| Correction facility | `Correction Facility`, `CORRECTION FACILITY(PI)` |
| Civil rights | `Civil Rights`, `CIVIL RIGHTS CLAIMS` |
| Uniformed services employee | `Uniformed Services Employee`, `EMPLOYEE UNIFORMED SERVICE (PI)` |
| Other | everything else |

Years to resolution = `disposition_date` minus `occurrence_date`, in years.

## 2. US Treasury Judgment Fund

Dataset: Judgment Fund: Annual Report to Congress (122,172 rows, FY2009–FY2026, last updated 19 November 2026).

- API: `https://api.fiscaldata.treasury.gov/services/api/fiscal_service/v2/payments/jfics/jfics_congress_report`
- Fields used: `record_date`, `src_line_nbr`, `control_nbr`, `defendant_agency_nm`, `principal_amt`, `principal_citation_desc`

The claim type is the last part of `principal_citation_desc`, and the resolution type is the middle part:

`Federal Tort Claims Act/{Admin|Court|Settlement}/{Traffic|Med Mal|Prop-Maint|Aircrash|Fire-Flood|Miscellany}`

Steps:

1. Fetch one page per fiscal year with `filter=record_date:eq:YYYY-09-30` and `page[size]=10000`.
2. Keep rows whose description contains `Tort Claims`.
3. Add up `principal_amt` for rows sharing `control_nbr` and resolution type to get one amount per claim.
4. Drop claims with a total of $0 or less.

Two behaviours of this API to watch for:

- Always request `src_line_nbr` and `control_nbr`. If `fields` is set without a unique column, the API returns distinct rows and silently drops duplicates. In testing this lost about 2,300 of the 39,793 traffic rows.
- The `in` filter returned about 2,100 of the 39,793 traffic rows. Use `eq`.

## 3. City of Chicago

Source: Department of Law, Judgment/Verdict & Settlement Reports, one Excel file per year, 2012 to 31 August 2026 (15 files, 12,360 payment rows).

- Page: `https://www.chicago.gov/city/en/depts/dol/supp_info/judgment-and-settlement-payments-requests.html`
- Columns used: `CASE #`, `PAYMENT AMOUNT ($)`, `PRIMARY CAUSE`, `CITY DEPARTMENT`, `DISPOSTION`

Filters: payment above $0, and disposition containing `SETTLE`, `VERDICT` or `JUDG`.

The `PRIMARY CAUSE` column has 657 distinct labels. They were grouped by keyword, tested in this order:

| Category | Rule |
|---|---|
| (excluded) | contains `TOW` or `POUND` |
| Bicycle accident | contains `BICYC` or `BIKE` |
| Pedestrian struck by vehicle | contains `PEDESTRIAN` and one of `MVA`, `VEHICLE`, `STRUCK` |
| Police pursuit crash | contains `PURSUIT` |
| Vehicle crash, property damage only | `STRUCK WHILE PARKED`, or a vehicle label that also contains `PROPERTY DAMAGE`, `PDMVA`, `MVAPD` or `PD` |
| Vehicle crash, street condition or signal | a vehicle label containing `STREET COND`, `TRAFFIC LIGHT` or `SIGN` |
| Vehicle crash with city vehicle (injury or mixed) | any other label containing `MVA`, `VEHICLE COLLISION` or `FIRETR` |
| Fall (sidewalk, street or premises) | contains `FALL` |

The year is read from the report title inside each file. One file has no year in its title; its rows were assigned to 2014, the only year otherwise missing.

## What goes into the published base

The steps above produce 174 segments. Only these are published in `settlement-segments.csv`; the rest go to `excluded-segments.csv`.

| Source | Kept |
|---|---|
| New York City personal injury | Motor vehicle, sidewalk defect, roadway defect, traffic control device, school, parks and recreation, city property / building, medical malpractice |
| New York City property damage | Nothing |
| Federal traffic | Resolution type `Court` or `Settlement` only, plus the yearly rows for those two combined |
| Federal other | Medical malpractice (all resolution types); property maintenance and aircraft crash with resolution type `Court` or `Settlement` |
| Chicago | Bicycle accident, pedestrian struck by vehicle, police pursuit crash, fall |

## Percentiles

For a sorted list of `n` amounts, the percentile `p` is taken at position `(n − 1) × p`, with linear interpolation between the two neighbouring values, then rounded to the nearest dollar.

## Checks run on this snapshot

- New York City motor vehicle: the eight fiscal-year rows add up to the all-years row (4,623 claims, $991,982,776).
- Federal traffic: the three resolution types add up to the all-traffic row (38,914 claims), and so do the 18 fiscal-year rows.
- Dollar-band counts add up to the claim count for every segment in `amount-bands.csv`.
- New York City motor vehicle FY2023 total ($173,652,500) and mean ($308,441) match the Comptroller's annual claims report.
