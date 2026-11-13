# Soil Organic Carbon and Nutrient Status Analysis (Soil Health Card Data)

A Python project that cleans and analyses **real** village-level soil-test data from India's Soil Health Card (SHC) scheme, focusing on **soil organic carbon (SOC) status** and other nutrient ratings.

## Data
- **Source:** *Soil Nutrient Analysis*, Soil Health Card data, Ministry of Agriculture and Farmers Welfare, Government of India, published on the India Data Portal.
- **Dataset page:** https://ckandev.indiadataportal.com/dataset/soil-health-card
- **Original portal:** https://soilhealth.dac.gov.in/nutrient-dashboard
- **License:** Open Data Commons Attribution License
- **Size:** about 1.2 GB, so the raw file is **not** included here. Download it from the link above.
- **Structure:** long format, one row per village, year, nutrient and rating (Low / Medium / High, or classes such as Acidic / Neutral / Alkaline), with a `value` for that rating. The data contains **ratings, not measured values**, so the project analyses fertility *status* and does not compute carbon stock.

## What the notebook does
1. Reads the large file in chunks and keeps one state (set `STATE` in the settings cell).
2. Standardises place names, nutrient names and ratings.
3. Removes duplicate rows and reports conflicting duplicates.
4. Validates values (no negatives, no blanks) and detects whether `value` holds counts or percentages.
5. Checks coverage: villages with no organic carbon result are treated as gaps, not as Low.
6. Organic carbon status by district and by block, with a Nutrient Index Value.
7. Hotspot villages where at least half of results are rated Low organic carbon.
8. Links between Low organic carbon and other nutrient problems.
9. Soil pH and salinity classes, and change over time when several years are present.

## How to run
1. Download the CSV and save it in this folder as `soil_health_card.csv` (or change `FILE` in the notebook).
2. `pip install -r requirements.txt`
3. Open `Soil_Health_Card_Nutrient_Status_Analysis.ipynb` in Jupyter, set `STATE`, and choose **Run All**.

## Outputs
| File | Content |
|---|---|
| `oc_status_by_district.csv` | Share of Low / Medium / High organic carbon and index per district |
| `oc_status_by_block_worst_district.csv` | Same, by block, for the district with the highest Low share |
| `hotspot_villages_low_oc.csv` | Villages with at least 50% Low organic carbon results |
| `nutrient_status.csv` | Rating shares for every nutrient in the state |
| `oc_trend_by_year.csv` | Organic carbon ratings by year |
| `cleaning_log.csv` | Every cleaning step and how many rows it affected |
| `figures/` | Charts |

## Key findings
*(Run the notebook, then paste the "Key findings" printed in section 14 here.)*

## Limitations
- Ratings, not measurements: organic carbon percentages and carbon stock cannot be calculated.
- Ratings depend on the cut-offs used by the testing labs; this project does not recompute them.
- Results come from many labs; district differences can reflect testing practice as well as real soil differences.
- Untested villages are absent, not Low.
- If `value` is a percentage, district figures are simple averages across villages.
- Correlation does not mean cause.
