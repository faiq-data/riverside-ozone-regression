# Predicting Ground-Level Ozone in Riverside

A statistical analysis in R of how temperature, wind speed and solar radiation relate to ground-level ozone, ending in a simple linear regression model and predictions for two temperature scenarios.

Individual coursework for a statistics module. The full write-up is in [`riverside_ozone_regression.pdf`](riverside_ozone_regression.pdf), and the code and commentary are in [`riverside_ozone_regression.Rmd`](riverside_ozone_regression.Rmd).

## The data

60 daily observations with five variables: `Day`, `Ozone`, `Temperature` (°F), `Wind_Speed` and `Solar_Radiation`. There are no missing values or duplicate rows.

The dataset (`riverside_air.csv`) was provided with the coursework and is not included in this repository.

## What the analysis covers

1. **Exploratory analysis:** summary statistics, IQR-based outlier detection, a histogram of ozone, a boxplot of temperature, a scatterplot matrix, and a linear vs LOESS comparison for solar radiation.
2. **Correlation analysis:** Pearson correlations, a hypothesis test for ozone and temperature, and the limits of Pearson's r when a relationship is not linear.
3. **Model selection:** three simple linear regressions compared using R², RSE and the F-test, with residual and Q-Q diagnostics.
4. **Final model:** coefficient interpretation and 95% prediction intervals at two temperatures.

## Key results

| Predictor | Correlation with ozone | R² | RSE | p-value |
| --- | --- | --- | --- | --- |
| Temperature | 0.346 | 0.120 | 10.73 | 0.0068 |
| Wind speed | -0.191 | 0.036 | 11.23 | 0.145 |
| Solar radiation | 0.205 | 0.042 | 11.20 | 0.117 |

- **Temperature is the only significant predictor** and was chosen as the final model: `Ozone = 28.73 + 0.480 × Temperature`. Each 1°F increase is associated with about 0.48 more units of ozone.
- **Outliers:** ozone on Day 11 (96.1) and temperature on Day 15 (48.8°F) fall outside the IQR bounds. They look like real measurements, so they were kept.
- **Predictions:** at 82°F the model predicts ozone of 68.1 (95% prediction interval 46.3 to 89.9). At 90°F it predicts 71.9 (49.6 to 94.2). Both temperatures are within the observed range (48.8 to 90.8°F), but very few days were hotter than 90°F, so the second prediction is less reliable.
- **Diagnostics:** the residuals show no clear pattern, and the Q-Q plot shows only small deviations in the tails.

## Limitations

- Temperature explains only about 12% of the variation in ozone, so predictions are imprecise, as the wide intervals show.
- It is a single-predictor model on 60 observations. Other factors, such as humidity or traffic, are not in the data.
- The relationship between solar radiation and ozone looks non-linear, so a straight-line model is a poor fit for that predictor.
- Next steps: fit a multiple regression with all three predictors, try a non-linear term for solar radiation, and check how much the two flagged outliers influence the fit.

## Run it yourself

1. Install R, then the packages: `install.packages(c("tidyverse", "GGally", "knitr", "rmarkdown"))`.
2. Put your copy of `riverside_air.csv` in the same folder as the Rmd file.
3. Render the report (a LaTeX installation such as TinyTeX is needed for the PDF):

```r
rmarkdown::render("riverside_ozone_regression.Rmd")
```

## Tools

R, tidyverse, ggplot2, GGally, knitr, R Markdown.
