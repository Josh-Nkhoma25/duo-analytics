# The Political Economy of Public Debt in Malawi, 1994-2025

Empirical companion to my undergraduate dissertation, *"Tailspin into Debt? The Political Economy of Public Debt and Fiscal Governance in Malawi (1994-2025)"* (University of Malawi, Chancellor College, 2026). The dissertation has been accepted for presentation at the 4th Biennial Conference of the Political Science Association of Malawi (Salima, September 2026) and for publication in the *Malawi Journal of Politics and Public Affairs*.

## The question

Malawi's public debt has moved a great deal since 1994, and not always in step with the macroeconomic fundamentals that standard debt-sustainability frameworks emphasise. This project asks a narrower, more tractable version of that puzzle: does debt-to-GDP track political budget cycles, meaning elections and the year immediately before them, and does that relationship depend on the surrounding institutional environment? I use two V-Dem indices, clientelism and legislative oversight, as proxies for that environment, on the logic that a government facing weak oversight and operating in a more clientelistic system has both more room and more incentive to borrow around elections.

It would be premature to call any of this causal. Thirty-two annual observations from one country is enough to describe Malawi's own trajectory carefully, not enough to generalise about political budget cycles as a phenomenon. What follows should be read in that spirit.

## What the analysis finds

Running the numbers script produces the tables in `tables/`; the headline results are summarised here.

**Debt-to-GDP itself does not look stationary at levels** (augmented Dickey-Fuller p = 0.80 with a trend and constant, p = 0.62 with a constant only), while GDP growth, inflation, aid flows, and the clientelism index mostly do. That mixed order of integration is the standard justification for an error-correction approach rather than a simple levels regression, which is the framework used here.

Three nested long-run specifications were estimated. The baseline model (growth, inflation, election, pre-election) explains a moderate share of the variation in debt-to-GDP (R-squared 0.58); adding the two political-structure variables lifts this to 0.73. In that second specification, the clientelism index carries a negative and statistically significant coefficient. V-Dem codes *higher* values on this index as *less* clientelistic, so the sign actually runs the intuitive way: years and levels associated with more clientelistic governance are also associated with a higher debt-to-GDP ratio. Legislative oversight, by contrast, does not carry an independent effect once clientelism is in the model, which was not what I expected going in. The election and pre-election dummies are not significant in the baseline model but become significant, or close to it, once the political-structure controls are added, an interaction between specification and significance that is worth sitting with rather than explaining away.

The error-correction models are arguably the more informative piece. In both specifications the lagged error-correction term is negative and significant (Model 1: -0.51, p = 0.004; Model 2: -0.88, p < 0.001), which is the signature of genuine error-correction behaviour: when debt-to-GDP drifts away from the level implied by its long-run relationship with growth, inflation, and the political variables, it tends to move back. Short-run GDP growth is negative and significant in both ECMs, which is mechanical as much as political (faster growth mechanically shrinks the debt ratio's denominator).

Granger causality tests on the differenced series find no political variable Granger-causing debt-to-GDP at the 5 percent level; election timing comes closest (p = 0.073 at one lag) without clearing the conventional threshold. A Dynamic OLS re-estimation of the Model 2 specification, with Newey-West standard errors and one lead and lag of the differenced growth and inflation series, reproduces the clientelism and growth results and keeps pre-election significant, which is some reassurance that those two findings are not an artefact of the static specification.

## Honest scope notes

Two limitations are worth stating plainly rather than glossing over, since a big part of doing this kind of work well is being clear about what it does and does not show.

First, this module was originally built to cross-check a parallel specification developed in R (the source notebook this was cleaned up from repeatedly references matching R and `urca` output). That R script is not included here. If you are reading this because you have it, an `r/` folder alongside `scripts/` is where it belongs.

Second, `run_ardl_companion()` in the analysis script fits an ARDL model as a descriptive companion to the ECM results, but it does not implement a formal Pesaran-Shin-Smith bounds test for cointegration; `statsmodels` does not have a direct equivalent to R's `ARDL`/`urca` bounds-testing routines. The ECM specifications, which use the long-run OLS residuals directly as the error-correction term, carry the actual cointegration evidence in this project. The ARDL fit is included because it is a legitimate part of the toolkit and worth showing, not because it stands in for the bounds test.

## Data

`data/malawi_debt_dataset.csv` is a hand-assembled annual panel drawing on the Reserve Bank of Malawi, the Ministry of Finance and Economic Affairs debt bulletin, World Bank World Development Indicators, the African Development Bank's Malawi economic outlook, V-Dem v16, and CEIC exchange rate data. Full variable definitions, sources, and imputation notes are in `data/DATA_DICTIONARY.md`. Two 2024-25 series (aid flows and terms of trade) are carried forward or interpolated from the last observed value in the absence of a sourced figure; this is flagged per-variable in the dictionary rather than buried in code comments.

## Repository structure

```
malawi-debt-political-economy/
├── README.md
├── requirements.txt
├── data/
│   ├── malawi_debt_dataset.csv
│   └── DATA_DICTIONARY.md
├── scripts/
│   └── debt_political_economy_analysis.py
├── figures/
│   └── time_series_panel.png
├── tables/
│   ├── descriptive_statistics.csv
│   ├── unit_root_tests.csv
│   ├── long_run_models.txt
│   ├── error_correction_models.txt
│   ├── ardl_companion_model.txt
│   ├── granger_causality.txt
│   └── dols_robustness.txt
└── outputs/
    └── run_summary.txt
```

## Reproducing this

```bash
pip install -r requirements.txt
python scripts/debt_political_economy_analysis.py
```

Everything in `figures/`, `tables/`, and `outputs/` in this repository was generated by that one command against the data as checked in; running it again should reproduce it exactly, since there is no random seed involved anywhere in the pipeline.

## Tools

Python (pandas, statsmodels, scikit-learn, matplotlib, scipy). Originally developed and iterated on in Google Colab; restructured into a standalone script for this repository.
