# Iran War Risk and Global Financial Markets (2026)

## Summary

This report implements the identification-through-heteroskedasticity (ITH) framework of Rigobon (2003) and Rigobon and Sack (2003) using 2026 Iran-conflict coverage and daily market data. The GDELT Global Knowledge Graph (GKG) is the reproducible news source. GDELT DOC 2.0 headlines are retrieved only after GKG has selected the Table 1 dates; they label selected events but do not determine event dates or enter the estimators.

The saved notebook run selects 17 high-news trading dates and 17 matched lower-intensity trading dates. Median GKG intensity is 13.00 on high-news dates and 11.50 on matched controls. All selected dates have GKG coverage, and 10 of the 17 high-news dates have at least one cached DOC headline. Of the Table 2 results, only the high-yield bond ETF interval excludes zero at 95%; the remaining estimates are statistically imprecise in this short sample.

## Data and methodology

The GKG signal combines log-transformed counts of Iran-conflict documents, Iran documents, and negatively toned Iran-conflict documents. The high regime is the 17 trading dates with the greatest signal. For each high date, the notebook chooses a nearby, unused trading date whose GKG intensity is at or below the full-sample median as a low-news control.

Let daily market changes be \(\Delta x_t = Dz_t + e_t\), with Iran war risk as the first latent factor. ITH compares the high- and low-regime covariance matrices, \(\Delta\Sigma=\Sigma_H-\Sigma_L\). With the two-year Treasury-yield change as the normalized variable, two covariance-ratio estimators and a combined-instrument IV estimator recover each market variable's loading. Results are scaled to a latent shock that lowers the two-year yield by 25 bp. The notebook uses 5,000 bootstrap draws for 95% intervals.

This interpretation requires stable market loadings across regimes, a war-risk-specific covariance shift, and no systematic change in the variances of other shocks. GKG is a structured-news proxy for coverage intensity, not a direct measure of war probability, direction, or severity. The notebook also reports a ratio-gap diagnostic: large gaps between the two covariance-ratio estimators indicate sensitivity to the identifying restriction.

For Table 1 labels, the notebook queries GDELT DOC 2.0 only for the 17 already-selected dates, with a maximum of 250 articles per date. It caches title, URL, domain, and language; de-duplicates by URL (or title where URL is missing); and calculates an escalation score from headline words. These fields do not affect GKG counts, regime selection, or continuous estimators. A zero headline count means this bounded retrieval saved no headline, not that the day had no Iran-conflict coverage.

## Table 1. High-variance Iran-war-news dates

| No. | Date | GKG intensity | DOC headlines | Event label |
|---:|---|---:|---:|---|
| 1 | 2026-03-03 | 13.49 | 250 | Israel vs Iran: Middle East conflict may not stay regional — Security expert |
| 2 | 2026-03-04 | 13.34 | 0 | GKG-selected high-variance Iran-war-news date; validate using representative URL |
| 3 | 2026-03-05 | 13.20 | 250 | U.S. submarine torpedoes Iranian warship off Sri Lanka |
| 4 | 2026-03-06 | 13.04 | 249 | Chinese-language headline on ceasefire prospects and oil prices |
| 5 | 2026-03-10 | 13.05 | 0 | GKG-selected high-variance Iran-war-news date; validate using representative URL |
| 6 | 2026-03-11 | 12.92 | 250 | US denies escorting oil tanks through Hormuz Strait |
| 7 | 2026-03-12 | 13.09 | 0 | GKG-selected high-variance Iran-war-news date; validate using representative URL |
| 8 | 2026-03-13 | 13.06 | 250 | U.S. and Israel wanted quick win, but failed |
| 9 | 2026-03-17 | 12.86 | 250 | Finland president calls for India to broker an Iran ceasefire |
| 10 | 2026-03-18 | 13.00 | 250 | Iran hits Tel Aviv with cluster missiles after security-chief assassination |
| 11 | 2026-03-19 | 12.96 | 249 | Iran attacks energy sites, defying calls for restraint |
| 12 | 2026-03-26 | 12.79 | 250 | U.S. rejects reports of Iran talks in Pakistan |
| 13 | 2026-04-01 | 12.84 | 250 | Israel hits Iran with attacks and says it killed a Hezbollah commander |
| 14 | 2026-04-07 | 12.83 | 0 | GKG-selected high-variance Iran-war-news date; validate using representative URL |
| 15 | 2026-04-08 | 13.26 | 0 | GKG-selected high-variance Iran-war-news date; validate using representative URL |
| 16 | 2026-04-09 | 12.86 | 0 | GKG-selected high-variance Iran-war-news date; validate using representative URL |
| 17 | 2026-04-14 | 12.88 | 0 | GKG-selected high-variance Iran-war-news date; validate using representative URL |

The dates are selected solely by the GKG signal. Cached DOC headlines improve auditability but are not independently verified event chronologies; a linked story should be reviewed before treating a headline as a complete description of an event.

## Table 2. ITH market effects

Each effect is the combined-IV response to a latent shock that lowers the two-year yield by 25 bp. Effects are percent log returns or log changes except for the high-yield-minus-Treasury-duration relative return.

| Market | Combined IV effect | 95% bootstrap interval | 95% precision? | Ratio gap |
|---|---:|---:|---|---:|
| Brent crude futures | -70.03 | [-165.47, 5.94] | No | 4.21 |
| Developed ex-US equities (EFA) | 16.59 | [-13.79, 38.28] | No | 0.22 |
| Emerging-market equities (EEM) | 4.07 | [-23.15, 32.16] | No | 2.16 |
| VIX | -11.79 | [-47.68, 28.79] | No | 1.16 |
| Gold futures | 18.68 | [-1.38, 38.12] | No | 1.19 |
| High-yield credit relative to Treasuries | 1.50 | [-2.93, 3.89] | No | 0.18 |
| U.S. dollar index | 3.04 | [-9.58, 12.39] | No | 2.34 |
| U.S. equities (SPY) | 4.59 | [-5.79, 12.45] | No | 1.03 |
| High-yield bond ETF | 3.86 | [0.11, 8.66] | Yes | 0.14 |
| Treasury-duration ETF | 1.09 | [-2.18, 3.55] | No | 0.12 |

Only the high-yield bond ETF has a 95% bootstrap interval excluding zero. The other point estimates should not be treated as evidence of an effect or of a conventional risk-off pattern. Brent has the largest ratio gap and the largest absolute response, making its striking point estimate especially fragile. EEM and the dollar also have large ratio gaps; EFA, high-yield credit relative to Treasuries, high-yield bonds, and Treasury duration have closer covariance-ratio estimates, but estimator agreement is not a significance test.

## Table 3. Incremental variance accounting

| Market | Incremental variance share | Diagnostic |
|---|---:|---|
| Brent crude futures | 84.99% | Interpretable conditional on identification assumptions |
| Developed ex-US equities (EFA) | 57.65% | Interpretable conditional on identification assumptions |
| Emerging-market equities (EEM) | 1.32% | Interpretable conditional on identification assumptions |
| VIX | 15.95% | Interpretable conditional on identification assumptions |
| Gold futures | 35.18% | Interpretable conditional on identification assumptions |
| High-yield credit relative to Treasuries | 16.34% | Interpretable conditional on identification assumptions |
| Two-year Treasury yield | 7.10% | Interpretable conditional on identification assumptions |
| U.S. dollar index | 24.18% | Interpretable conditional on identification assumptions |
| U.S. equities (SPY) | 8.03% | Interpretable conditional on identification assumptions |
| High-yield bond ETF | 50.68% | Interpretable conditional on identification assumptions |
| Treasury-duration ETF | 3.18% | Interpretable conditional on identification assumptions |

The high-minus-low two-year-yield variance shift is positive, and no reported share exceeds 100% in this run. These are nevertheless conditional accounting quantities, not causal variance shares: the figures inherit the ITH assumptions and the estimated loadings. Larger shares indicate a larger estimated war-risk component of war-period volatility, not proof that war risk dominated the asset's variance.

## Alternative-method comparison

All methods are calibrated to the market response associated with a 25 bp decline in the two-year yield. The event study is the mean difference between selected high- and low-news dates. The continuous model regresses on standardized GKG intensity. The PCA model uses the first principal component of standardized log GKG counts for Iran documents, Iran-conflict documents, and negative-tone documents, oriented so more conflict coverage is positive; in the saved notebook run, PC1 explains 99.6% of standardized-feature variance. Unlike ITH, the two continuous methods require GKG coverage to be a cardinal, linearly related proxy for the latent factor.

| Market | ITH | Event study | Continuous GKG | GKG PCA | All signs agree? |
|---|---:|---:|---:|---:|---|
| SPY | 4.59 | -1.84 | 0.56 | 0.84 | No |
| EFA | 16.59 | 4.48 | 6.60 | 6.73 | Yes |
| EEM | 4.07 | -13.23 | 0.63 | 1.06 | No |
| Brent | -70.03 | -10.32 | -16.96 | -17.75 | Yes |
| Gold | 18.68 | 13.19 | 15.14 | 15.13 | Yes |
| Dollar | 3.04 | 0.01 | -2.05 | -1.96 | No |
| High-yield ETF | 3.86 | 0.42 | 1.06 | 1.07 | Yes |
| Treasury-duration ETF | 1.09 | 1.10 | 1.99 | 1.99 | Yes |
| VIX | -11.79 | 0.03 | -6.38 | -6.72 | No |
| High-yield minus Treasury duration | 1.50 | -0.68 | -0.93 | -0.91 | No |

EFA, Brent, gold, the high-yield ETF, and the Treasury-duration ETF have the same directional result in all four methods. This is directional robustness within this data construction, not causal confirmation; except for the high-yield ETF, the ITH intervals remain imprecise. SPY, EEM, the dollar, VIX, and high-yield credit relative to Treasuries change sign across methods and are especially specification-sensitive. The event-study two-year-yield mean difference is 1.29 bp, so calibration of that sparse comparison is potentially unstable.

ITH is preferable only when its regime-specific variance-shift assumption is credible, because it does not require a cardinal news-risk measure. The event study is transparent but can absorb coincident news. Continuous intensity and PCA use more daily information but are exposed to omitted variables and depend on GKG coverage features being valid quantitative proxies.

## Conclusion and limitations

The notebook produces an auditable Iran-war-news calendar and conditional same-day market sensitivities, not forecasts or investment recommendations. The only ITH response precise at the 95% level is the positive high-yield bond ETF response to the normalized 25 bp two-year-yield decline. Its sign also agrees across the event-study, continuous-GKG, and PCA comparisons, but this is descriptive support—not independent causal confirmation—because the methods share market data and related GKG inputs.

For the other assets, the results are best read as sensitivity analysis. Brent and gold have large ITH estimates and consistent signs across methods, but their intervals include zero; Brent also has the largest covariance-ratio gap. EEM, the dollar, VIX, SPY, and high-yield credit relative to Treasuries switch sign across specifications. A universal risk-off interpretation would therefore overstate what this sample supports.

A longer sample with intraday market data and independently coded, time-stamped events would better isolate announcement windows, test alternative matching rules, and assess covariance-shift stability. For now, the exercise provides a reproducible framework and a limited, asset-specific signal—not a definitive estimate of the financial-market effect of Iran war risk.

## Reproducibility artifacts

The machine-readable outputs are `output/iran_war_risk_table_1_high_variance_dates.csv`, `output/iran_war_risk_table_2_financial_market_effects.csv`, `output/iran_war_risk_table_3_variance_decomposition.csv`, and `output/iran_war_risk_method_comparison.csv`.

## References

- Rigobon, R. (2003). *Identification Through Heteroskedasticity*. Review of Economics and Statistics.
- Rigobon, R., & Sack, B. (2003). *The Effects of War Risk on U.S. Financial Markets*. Journal of Banking & Finance.
- GDELT Project. Global Knowledge Graph and DOC 2.0 API.
