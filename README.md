# Reduction or Relocation?

**Analyzing the Impact of NYC's Congestion Pricing Program on Yellow-Taxi Ridership in the Manhattan Central Business District**

WGU BSDA Capstone.

## Research question

Did the NYC Congestion Pricing CBD Tolling Program (launched **January 5, 2025**) cause a
statistically significant reduction in mean daily yellow-taxi trip counts inside the
Manhattan Central Business District (CBD)?

The study compares daily in-zone yellow-taxi pickup counts for **January–June 2024**
(pre-intervention) vs **January–June 2025** (post-intervention) using NYC TLC Yellow Taxi
Trip Records.

- **H0:** mean daily CBD count (2025) = mean daily CBD count (2024)
- **H1:** mean daily CBD count (2025) < mean daily CBD count (2024)  *(one-tailed)*
- **Test:** Welch's two-sample t-test (`equal_var=False`); effect size via Cohen's d; α = .05

## CBD zone definition (important — differs from the Task 2 proposal)

The MTA Congestion Relief Zone is **Manhattan at or below 60th Street** (excluding FDR Drive,
the West Side Highway, and the Hugh L. Carey Tunnel surface connections). The TLC Taxi Zone
Lookup CSV contains **no "below-60th-Street" flag**, so the CBD zones must be enumerated
explicitly.

This analysis uses the geographically-correct **38 taxi zones** below 60th Street. Manhattan
has only **69** taxi zones in total, so the **"63 zones" stated in the Task 2 proposal is not
supportable** under any below-60th-Street definition (the remaining 31 Manhattan zones lie
north of 60th Street or are harbor/river islands). The 38 `PULocationID`s used here are
hardcoded and justified in `notebooks/01_data_prep.ipynb`. **The Task 2 narrative's "63"
should be corrected or acknowledged** in the Task 3 writeup.

## Repository layout

```
data/
  raw/        # 12 monthly Yellow Taxi Parquet files (gitignored; download from TLC portal)
  lookup/     # taxi_zone_lookup.csv (PULocationID -> borough/zone)
  processed/  # generated: daily_cbd_counts.csv, cleaning_log.csv
notebooks/
  01_data_prep.ipynb      # load (DuckDB) -> filter CBD -> clean -> aggregate to daily counts
  02_data_analysis.ipynb  # Welch t-test, Cohen's d, 95% CI, visualizations
visuals/      # generated PNGs (time-series line chart, pre/post box plot)
requirements.txt
```

## How to run

```bash
pip install -r requirements.txt
jupyter notebook   # run 01_data_prep.ipynb, then 02_data_analysis.ipynb top-to-bottom
```

The raw Parquet files are excluded from version control. Download the Jan–Jun 2024 and
Jan–Jun 2025 Yellow Taxi Trip Records into `data/raw/` from the NYC TLC Trip Record Data
portal before running notebook 01.

## Data source & attribution

NYC Taxi & Limousine Commission (TLC) Trip Record Data — Yellow Taxi Trip Records and Taxi
Zone Lookup Table. <https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page>
Used under the NYC Open Data Terms of Use for non-commercial academic research.
