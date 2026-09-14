# Data Quality Profiling — Big Mac Index

## Objective

Audit and repair the data-preparation layer of a Big Mac Index purchasing-power-parity analysis, establishing that two upstream defects — an inverted PPP computation and an unbalanced-panel filter — materially distorted the resulting price comparisons.

## Methodology

- **PPP computation audit.** Traced the implied purchasing-power-parity calculation and identified a transposed numerator and denominator, which returned the reciprocal of the intended local-to-base price ratio. Corrected the expression and re-derived the series.
- **Sample-construction review.** Examined the filter that restricted the panel to countries observed in every period, and identified it as a source of survivorship bias: units exit the sample on the basis of data availability, which is not independent of the price levels being measured.
- **Bias quantification.** Constructed two parallel aggregates of the mean Big Mac price — one over the complete-panel subset, one over all available observations in each period — and compared them in levels, in percentage terms, and by the count of periods in which the complete-panel mean exceeds the all-available mean.
- **Reusable profiling utility.** Implemented `profile_dataframe()`, which reports the number of units, the number of periods, the panel structure, the count of complete units, whether the panel is balanced, and the share of missing values in each column.

## Key Findings

The PPP series was previously inverted; the corrected computation reverses the direction of every implied over- and undervaluation relative to the base currency.

Restricting the sample to complete panels is not a neutral cleaning step. The complete-panel average Big Mac price is **$X.XX** higher than the all-available average, a gap of **X.X%**, and the complete-panel average is the larger of the two in **XX of 45** periods. The surviving units are systematically higher-priced, so any conclusion drawn from the balanced subset overstates the global average price level.

The `profile_dataframe()` audit makes this visible before analysis begins, reporting completeness and balance alongside per-column missingness rather than leaving the filter's cost implicit.
