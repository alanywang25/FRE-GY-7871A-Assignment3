# Iran War Risk and Global Financial Markets (2026)

## Summary

This report applies the heteroskedasticity-based war-risk framework of Rigobon (2003) and Rigobon and Sack (2003) to 2026 Iran-conflict coverage and daily market data. The GDELT Global Knowledge Graph (GKG) provides the primary, reproducible daily news measure. To make the high-news event calendar auditable, the notebook also calls GDELT's public DOC 2.0 article-list API for only the already-selected Table 1 dates and caches headline metadata locally. Those headlines label events; they do not determine event dates or enter the continuous estimators.

The current run selects 17 high-news trading dates and 17 matched low-news dates. Median GKG intensity is 11.35 on high-news dates and 9.85 on controls. All selected dates have GKG coverage. The bootstrap intervals in Table 2 all include zero, so the results do not establish a statistically precise or causal market effect of Iran war risk in this short sample.

## Data and methodology

The GKG signal is constructed from Iran-related conflict coverage and is used to find high-variance news regimes. For each high-news date, the notebook selects a nearby low-news trading date. The identification-through-heteroskedasticity (ITH) estimator compares the covariance matrices of the two regimes and normalizes the latent shock to a 25 bp decline in the two-year Treasury yield.

Market series are daily changes or log returns for the two-year Treasury yield, SPY, EFA, EEM, Brent, gold, the dollar, high-yield credit, Treasury duration, and VIX. The reported estimates depend on the crucial assumption that the high-minus-low covariance shift is primarily a war-risk variance shift and that market loadings remain stable.

After GKG selects the high-variance dates, the notebook calls GDELT's public DOC 2.0 API (`https://api.gdeltproject.org/api/v2/doc/doc`) separately for each selected date using `(Iran OR Iranian) AND (war OR strike OR attack OR missile OR Hormuz OR ceasefire OR nuclear)`. Each JSON article-list request is capped at 250 records and saves date, title, URL, domain, and language. Records are deduplicated by URL, or normalized title when a URL is unavailable. A transparent headline score counts escalation terms less one-half of de-escalation terms. These fields supply Table 1 labels and headline coverage counts only; they are not added to GKG document counts, used to select events, or treated as a continuous war-risk factor.

The API workflow is intentionally bounded and reproducible: the notebook requests only the 17 Table 1 dates, waits five seconds between calls, backs off after HTTP 429 responses, and caches results in `output/iran_war_risk_gdelt_doc_table1_headlines.csv`. DOC headline coverage is therefore a rate-limited sample rather than a complete universe of reporting. A missing cached headline means that no label was saved by this constrained retrieval, not that the date lacked Iran-conflict news.

## Table 1. High-variance Iran-war-news dates

GKG selects the dates below mechanically. DOC headline counts reflect a bounded article-list sample (up to 250 cached headlines per selected day), so a zero does not mean zero news coverage.

| No. | Date | GKG intensity | DOC headlines | Event label |
|---:|---|---:|---:|---|
| 1 | 2026-03-03 | 11.85 | 250 | Israel vs Iran: Middle East conflict may not stay regional — Security expert |
| 2 | 2026-03-04 | 11.74 | 0 | GKG-selected date; validate with saved GKG representative record |
| 3 | 2026-03-05 | 11.61 | 250 | U.S. submarine torpedoes Iranian warship off Sri Lanka |
| 4 | 2026-03-06 | 11.43 | 249 | Headline discusses ceasefire prospects and oil prices during the conflict |
| 5 | 2026-03-10 | 11.51 | 0 | GKG-selected date; validate with saved GKG representative record |
| 6 | 2026-03-11 | 11.35 | 250 | Updates on Hormuz shipping and Iranian vessels |
| 7 | 2026-03-12 | 11.35 | 0 | GKG-selected date; validate with saved GKG representative record |
| 8 | 2026-03-13 | 11.33 | 250 | U.S. and Israel sought a quick win, but failed |
| 9 | 2026-03-17 | 11.28 | 250 | Calls for an India-brokered Iran–U.S. ceasefire |
| 10 | 2026-03-18 | 11.37 | 250 | Iran hits Tel Aviv with cluster missiles after an assassination |
| 11 | 2026-03-19 | 11.30 | 249 | Iran attacks energy sites despite calls for restraint |
| 12 | 2026-03-26 | 11.15 | 250 | Reports on possible Iran talks and a prospective deal |
| 13 | 2026-04-01 | 11.14 | 250 | Israel launches attacks on Iran and reports a Hezbollah commander killed |
| 14 | 2026-04-02 | 11.09 | 0 | GKG-selected date; validate with saved GKG representative record |
| 15 | 2026-04-07 | 11.09 | 0 | GKG-selected date; validate with saved GKG representative record |
| 16 | 2026-04-08 | 11.80 | 0 | GKG-selected date; validate with saved GKG representative record |
| 17 | 2026-04-09 | 11.30 | 0 | GKG-selected date; validate with saved GKG representative record |

Ten of the 17 selected dates have a cached DOC headline. These labels make the event calendar more auditable, but they are not independently verified event chronologies and should not be read as proof that one headline fully characterizes a day.

## Table 2. ITH market effects

Each combined-IV estimate is the estimated response to a latent shock that lowers the two-year yield by 25 bp. The 95% bootstrap interval is the decision-relevant uncertainty measure.

| Market | Combined IV effect | 95% bootstrap interval | 95% precision? | Ratio gap |
|---|---:|---:|---|---:|
| Brent crude futures | -77.20 | [-190.67, 10.48] | No | 3.27 |
| Developed ex-US equities (EFA) | 14.71 | [-20.17, 40.48] | No | 1.13 |
| Emerging-market equities (EEM) | 7.67 | [-11.92, 30.68] | No | 0.03 |
| VIX | -8.15 | [-46.39, 34.55] | No | 2.81 |
| Gold futures | 21.64 | [-5.46, 48.93] | No | 1.32 |
| High-yield credit relative to Treasuries | 1.52 | [-2.90, 4.54] | No | 0.00 |
| U.S. dollar index | 3.13 | [-10.10, 13.69] | No | 2.05 |
| U.S. equities (SPY) | 6.08 | [-4.59, 14.89] | No | 0.00 |
| High-yield bond ETF | 4.06 | [-1.43, 9.53] | No | 0.09 |
| Treasury-duration ETF | 1.19 | [-0.34, 3.06] | No | 0.03 |

No interval excludes zero. The point estimates therefore should not be interpreted as evidence that war risk raised equities or lowered Brent/VIX; both conventional and nonconventional sign readings are statistically unsupported. Large covariance-ratio gaps for Brent, VIX, the dollar, gold, and EFA signal that the results are sensitive to the identifying covariance restriction.

There are two useful diagnostic patterns in the table. First, SPY, EEM, high-yield credit relative to Treasuries, and Treasury duration have very small ratio gaps, meaning the two covariance-ratio calculations are close in this sample. That agreement is a limited internal consistency check, not a significance test: their bootstrap intervals still include zero. Second, Brent has both the largest absolute response and the largest ratio gap. Its point estimate is therefore economically striking but methodologically fragile; it should be treated as the clearest example of why the combined-IV point estimate cannot stand alone.

The normalization also matters. Every entry is scaled to a hypothetical 25 bp fall in the two-year yield, not to a directly observed one-day headline shock. The large numerical effects for Brent, gold, and EFA partly reflect this scaling and the small-sample covariance estimate. Comparisons across columns are more informative than the absolute magnitudes: broad intervals and disagreement between the two single-instrument effects indicate that the sample does not tightly pin down the latent-factor loading.

## Table 3. Incremental variance accounting

| Market | Incremental variance share | Diagnostic |
|---|---:|---|
| Brent crude futures | 372.24% | Assumptions/normalization strained |
| Developed ex-US equities (EFA) | 163.32% | Assumptions/normalization strained |
| Emerging-market equities (EEM) | 16.79% | Conditional interpretation |
| VIX | 27.48% | Conditional interpretation |
| Gold futures | 170.19% | Assumptions/normalization strained |
| High-yield credit relative to Treasuries | 60.76% | Conditional interpretation |
| Two-year Treasury yield | 25.58% | Conditional interpretation |
| U.S. dollar index | 92.35% | Conditional interpretation |
| U.S. equities (SPY) | 50.96% | Conditional interpretation |
| High-yield bond ETF | 202.30% | Assumptions/normalization strained |
| Treasury-duration ETF | 13.83% | Conditional interpretation |

Shares above 100% are diagnostics, not economically meaningful variance shares. They indicate that the single-factor variance-shift interpretation is strained for those assets, consistent with the wide intervals and covariance-ratio disagreement in Table 2.

The variance decomposition is most credible for the assets with shares below 100% and modest ratio gaps, such as EEM, Treasury duration, and the two-year yield. Even there, the figures are conditional accounting quantities rather than causal variance shares because other shocks may also change between the high- and low-news regimes. The 92% dollar share and 61% credit-relative share are not proof that war risk dominates those markets; they instead show how strongly the accounting result depends on the estimated loading and the selected regime contrast.

## Alternative-method comparison

The notebook reports three alternatives alongside ITH, all calibrated to the market response associated with a 25 bp decline in the two-year yield. The **high-minus-low event study** compares mean market changes on the selected high-news and matched low-news dates; it is transparent but can absorb unrelated same-day news. The **continuous GKG-intensity regression** uses the standardized daily GKG war-news measure as an observed proxy for war risk. The **GKG PCA factor** reduces standardized GKG coverage features to their first principal component, oriented so that greater conflict coverage is positive. Unlike ITH, both continuous approaches require the observed GKG measure to be a cardinal, linearly related proxy for the latent shock.

| Market | ITH | Event study | Continuous GKG | GKG PCA | All signs agree? |
|---|---:|---:|---:|---:|---|
| SPY | 6.08 | 7.55 | 1.12 | 1.16 | Yes |
| EFA | 14.71 | 51.98 | 7.82 | 7.50 | Yes |
| EEM | 7.67 | 3.02 | -1.23 | 0.38 | No |
| Brent | -77.20 | -221.70 | -26.29 | -20.56 | Yes |
| Gold | 21.64 | 136.21 | 19.79 | 17.44 | Yes |
| Dollar | 3.13 | 2.36 | -1.97 | -2.07 | No |
| High-yield ETF | 4.06 | 2.54 | 1.36 | 1.33 | Yes |
| Treasury-duration ETF | 1.19 | 7.03 | 2.12 | 2.09 | Yes |
| VIX | -8.15 | -36.92 | -8.25 | -7.57 | Yes |
| High-yield minus Treasury duration | 1.52 | -4.49 | -0.76 | -0.76 | No |

Several signs are consistent across methods, but sign agreement is not causal confirmation: the event study and continuous methods treat observed GKG intensity as a cardinal risk proxy and are exposed to coincident macroeconomic and geopolitical news. The ITH estimator is preferable only if the variance-shift assumption is credible; the diagnostics above show it is not uniformly persuasive in this sample.

The comparison separates three patterns. SPY, EFA, Brent, gold, high-yield ETFs, Treasury-duration ETFs, and VIX have the same sign in all four methods. This is directional robustness within the chosen data construction, but it is not statistical robustness because the ITH intervals remain wide. EEM, the dollar, and high-yield credit relative to Treasuries switch sign across methods, which makes their estimated direction especially specification-sensitive. The event-study magnitudes are often much larger than the continuous-GKG and PCA estimates—for example, EFA, Brent, and gold—consistent with event-day observations containing other news as well as the intended war-risk variation.

The continuous-GKG and PCA columns are closer to each other for most assets than either is to the event study. That pattern is expected because both exploit the daily GKG series, whereas the event study uses a sparse set of selected dates. It does not show that either continuous specification is correctly measured: both require GKG intensity to be a cardinal proxy for war risk, an assumption that the ITH approach is intended to relax. The appropriate conclusion is sensitivity analysis, not a choice of the largest or most intuitive coefficient.

## Conclusion and limitations

This replication produces an auditable 2026 Iran-war-news event calendar and a set of conditional market-response estimates. The short sample, daily closing prices, potentially overlapping global shocks, a mechanically constructed GKG signal, partial DOC headline coverage, and sensitivity of some covariance estimates all require cautious language.

## Reproducibility artifacts

The exact machine-readable tables are in `output/iran_war_risk_table_1_high_variance_dates.csv`, `output/iran_war_risk_table_2_financial_market_effects.csv`, `output/iran_war_risk_table_3_variance_decomposition.csv`, and `output/iran_war_risk_method_comparison.csv`.

## References

- Rigobon, R. (2003). *Identification Through Heteroskedasticity*. Review of Economics and Statistics.
- Rigobon, R., & Sack, B. (2003). *The Effects of War Risk on U.S. Financial Markets*. Journal of Banking & Finance.
- GDELT Project. Global Knowledge Graph and DOC 2.0 API.
