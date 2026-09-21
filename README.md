# Iran War Risk and Global Financial Markets (2026)

This repository contains a replication-style analysis of Iran war risk and global financial variables. The main workflow is the GKG notebook:

- `Iran_War_Risk_2026_GKG.ipynb` — primary analysis, Tables 1–3, robustness methods, and written interpretations.
- `REPORT.md` — current written report based on the notebook's saved output tables.
- `AI_USE.md` — disclosure of AI-assisted work.

## Requirements

Use Python 3.10+ in a Jupyter environment with `numpy` and `pandas` installed. Internet access is required only when downloading missing GKG files, market data, or the optional GDELT DOC Table 1 headlines.

## Run the GKG notebook

1. Open `Iran_War_Risk_2026_GKG.ipynb` from this repository folder.
2. Restart the kernel and run cells in order.
3. Run the market-panel cell. It reads `output/iran_war_risk_estimation_panel.csv` and normalizes either supported column schema.
4. Run the GKG download/parser cell. It caches daily files under `gkg_daily/`; set `DOWNLOAD = False` after the needed files are available.
5. Run the Table 1 enrichment and selection cell. It requests DOC headlines only for the 17 GKG-selected event dates and caches them in `output/iran_war_risk_gdelt_doc_table1_headlines.csv`. Set `FETCH_GDELT_DOC_HEADLINES = False` to use an existing cache or to skip requests.
6. Run the remaining Table 1, ITH/Table 2, Table 3, alternative-method, and interpretation cells.

The analysis uses the 2026 sample dates specified in the setup cell. Re-run all downstream cells whenever you change the data, date range, or event-selection rule.

## Outputs

Generated GKG outputs are written to `output/`:

- `iran_war_risk_gkg_news_nlp.csv` — structured daily GKG measure.
- `iran_war_risk_gkg_doc_enriched_daily.csv` — GKG series enriched with Table 1 headline fields.
- `iran_war_risk_gdelt_doc_table1_headlines.csv` — cached DOC headline records for selected dates.
- `iran_war_risk_gkg_event_audit.csv` — high/low event-selection audit file.
- `iran_war_risk_table_1_high_variance_dates.csv` — Table 1.
- `iran_war_risk_gkg_results.csv` and `iran_war_risk_table_2_financial_market_effects.csv` — ITH estimates and Table 2.
- `iran_war_risk_table_3_variance_decomposition.csv` — Table 3.
- `iran_war_risk_method_comparison.csv` — alternative-method comparison.

The `output/*.csv` and `gkg_daily/` paths are ignored by Git because they are generated data and caches.
