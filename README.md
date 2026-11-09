# Used Car Market Listings Analysis

**Tools:** Python, Pandas, Matplotlib, Seaborn, Jupyter Notebook  
**Dataset:** `car_sales_new.csv` — 9,576 rows × 10 columns  
**Project type:** Exploratory Data Analysis (EDA)

## Business Problem

The objective of this project is to understand the composition and listed-price patterns of a vehicle listings dataset. The analysis examines which makes and body styles appear most frequently, how listed prices vary with vehicle year and mileage, and how price distributions differ across engine types and drivetrains. It also identifies data-quality issues that could distort market comparisons.

**Important scope boundary:** the provided data contains vehicle-listing attributes and listed prices. It does not include a confirmed sale outcome, transaction price, listing date, or geographic location. The findings therefore describe this dataset's listings and must not be interpreted as completed sales, buyer demand, or a market-wide market-share estimate.

## About the Data

The supplied CSV contains **9,576 records and 10 columns**:

| Column | Description based on the supplied data |
|---|---|
| `car` | Vehicle make |
| `price` | Listed price (currency not specified) |
| `body` | Body-style category |
| `mileage` | Mileage value (unit not specified) |
| `engV` | Engine-volume value (unit not specified) |
| `engType` | Engine / fuel-type category |
| `registration` | Registration status (`yes` / `no`) |
| `year` | Vehicle model year |
| `model` | Vehicle model |
| `drive` | Drivetrain (`front`, `full`, `rear`, or missing) |

The dataset includes 87 distinct makes, 6 body-style categories, and 4 engine-type categories.

### Data quality assessment

| Check | Finding |
|---|---:|
| Missing `drive` | 511 (5.34%) |
| Missing `engV` | 434 (4.53%) |
| Missing values in other columns | 0 |
| Exact duplicate rows | 113 (1.18%) |
| Zero listed prices | 267 (2.79%) |
| Zero mileage values | 348 (3.63%) |
| `engV` values above 10 | 53 |
| `engV` values equal to 99.99 | 28 |
| Listed prices above 100,000 | 138 |
| Mileage values above 500 | 34 |

The original notebook fills missing `drive` with `UnSpecified`, imputes missing `engV` with its observed mean (approximately 2.646344), and replaces zero prices with the original price mean (15,633.317). After this price replacement, the mean is 16,069.21 and the median is 9,800. These imputed price values are not observed asking prices.

The original notebook does not remove the 113 exact duplicate rows. They should be investigated rather than automatically deleted because no unique listing identifier is supplied. The unit definitions for price, mileage, and engine volume are also absent and should be confirmed before operational use.

## Results of the Analysis

### 1. Listing composition by make

Volkswagen is the most frequently listed make (**936; 9.77%**), followed by Mercedes-Benz (**921; 9.62%**) and BMW (**694; 7.25%**). These are counts within the supplied dataset, not overall market shares.

![Top 10 car makes](assets/top_10_car_makes.png)

### 2. Listing composition by body style

Sedans form the largest category (**3,646; 38.07%**), followed by crossovers (**2,069; 21.61%**).

![Body style distribution](assets/body_style_distribution.png)

### 3. Price patterns by body style

After applying the original notebook's zero-price replacement, crossovers have the highest median listed price among body styles (**19,400**). Median is emphasized because the price distribution is right-skewed and contains high-price observations.

![Median price by body style](assets/median_price_by_body.png)

### 4. Price patterns by model year

The post-preprocessing Pearson correlation between model year and price is **+0.380**. For example, 2016 listings have a median price of **36,500** (459 listings), compared with **10,200** for 2008 listings (1,158 listings). This is a descriptive association and does not isolate the effect of vehicle age from make or model mix.

![Median price by model year](assets/median_price_by_year.png)

### 5. Engine-type distribution

Petrol accounts for **4,379 listings (45.73%)**, Diesel for **3,013 (31.46%)**, Gas for **1,722 (17.98%)**, and Other for **462 (4.82%)**. Petrol and Diesel together represent 77.19% of records.

![Engine type distribution](assets/engine_type_distribution.png)

### 6. Volkswagen model mix

Within the 936 Volkswagen records, the top models by listing count are Touareg (69), Passat B6 (66), Caddy груз. (63), T5 (Transporter) пасс. (61), and Polo (60). The top 10 models account for 577 records (61.65%) of the Volkswagen subset.

![Top Volkswagen models](assets/volkswagen_top_models.png)

Diesel is also the most common engine type in the Volkswagen subset: 529 listings (56.52%), compared with 291 Petrol, 87 Gas, and 29 Other.

### 7. Price and mileage relationship

The post-preprocessing Pearson correlation between price and mileage is **-0.324**, indicating a moderate negative association. This pattern is not deterministic and is not adjusted for vehicle make, model, year, or body style.

![Price vs mileage](assets/price_vs_mileage_sample.png)

### 8. Price by engine type and drivetrain

The transformed-data average price is highest for `Other` engine type with `full` drivetrain (**40,618; n=64**), Diesel/full (**37,433; n=932**), and Petrol/full (**33,581; n=1,044**). Petrol/front averages **9,051** across 2,500 listings. Segment composition and sample size differ, so these are not controlled comparisons or evidence of a drivetrain premium.

![Mean price by engine and drivetrain](assets/mean_price_engine_drive_heatmap.png)

### 9. Correlation summary

| Variable pair | Pearson correlation after notebook preprocessing |
|---|---:|
| Price – year | +0.380 |
| Price – mileage | -0.324 |
| Mileage – year | -0.496 |
| Price – engine volume | +0.052 |

The simple linear association between engine volume and price is very weak in this dataset. These correlations do not control for vehicle make/model mix, body style, drivetrain, or outliers.

![Correlation heatmap](assets/correlation_heatmap.png)

### Additional price distribution view

![Price distribution](assets/price_distribution.png)

## Business Insights

1. **The listing mix is concentrated in a few makes and body styles.** Volkswagen and Mercedes-Benz are the two largest make categories, while sedans and crossovers dominate body-style listings.
2. **Listed prices are highly dispersed.** Before preprocessing, mean price is 15,633.32 versus a median of 9,200, with a maximum of 547,800. Use medians and percentiles alongside averages; do not present absolute price comparisons without confirming currency.
3. **Newer model years tend to have higher listed prices, while greater mileage tends to be associated with lower prices.** The corresponding correlations are +0.380 and -0.324 after notebook preprocessing. Neither relationship is causal.
4. **Crossovers have the highest body-style median price** in the transformed data (19,400), but a like-for-like analysis by make, model, and year is needed before making pricing recommendations.
5. **Full-drivetrain listings have higher average prices than front- or rear-drivetrain listings** in this sample. Different vehicle mixes and the presence of expensive models may explain this pattern; compare matched vehicle segments before treating it as a drivetrain effect.
6. **Data-quality issues should be addressed before operational pricing decisions.** Duplicate candidates, zero values, missing engine/drivetrain data, and extreme values affect interpretation.

## Next Steps

- Confirm the currency, mileage unit, engine-volume unit, and the meaning of zero price and zero mileage with the data owner.
- Investigate the 113 exact duplicate rows using listing identifiers or source metadata.
- Compare price results with zero-price records excluded and test robust alternatives to mean imputation for engine volume.
- Validate extreme observations such as price 547,800, mileage 999, and engine volume 99.99.
- Build like-for-like price comparisons by make, model, model year, body style, engine type, and drivetrain; report sample sizes, medians, and percentile ranges.
- Replace the random 100-row chart with full-dataset aggregation or set a fixed random seed for reproducibility.
- Add listing date, location, seller/listing identifiers, and confirmed sale outcome / transaction price if the business goal includes demand, conversion, or realized market value.

## Limitations

- No completed-sale status or transaction price is provided; the available price is a listing price.
- The currency and units for price, mileage, and engine volume are not specified in the supplied files.
- The dataset has no listing date, geography, or unique listing identifier.
- 113 exact duplicate rows exist, but their origin cannot be established from the available fields alone.
- Mean-imputing engine volume and replacing zero prices with the overall mean alter the resulting distributions and relationships.
- The original chart based on a random sample of 100 has no fixed seed and is not reproducible across runs.
- Group averages and correlations are unadjusted and may be affected by make/model composition and outliers.

## Project Files

- `Car_sales_analysis_with_business_insights.ipynb` — annotated notebook with the original code cells preserved.
- `car_sales_new.csv` — supplied dataset.
- `Car_sales_analysis_original.ipynb` — unchanged source notebook for reference.
- `assets/` — charts used in this README and selected original notebook chart exports.
- `CV_Title_and_Description.txt` — ATS-friendly project title and CV description.
