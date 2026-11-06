# Persona Alpha — Research Terminal

**An interactive, explainable equity-research prototype powered by a five-model quantitative ensemble, a specialist ranker, and source-grounded AI investment research.**

**Status:** Historical, read-only beta  
**Portfolio as-of date:** June 30, 2023  
**Runs offline:** Yes

> **Research software, not investment advice.** This is a fixed historical demonstration—not a live stock screener, brokerage service, or recommendation to buy the displayed securities.

## What is Persona?

Persona Alpha is an experimental stock-selection and investment-research system. It combines quantitative ranking with transparent model explanations and a balanced, document-backed discussion of each company's business strengths and risks.

The current terminal presents two strategies built from the **same historical candidate universe**:

- **Core Alpha 20:** A diversified, 20-stock selection produced by the frozen five-model ensemble.
- **Convex Alpha 3:** A concentrated three-stock selection chosen by a separate LightGBM LambdaRank model *from within* Core Alpha's 20 candidates.

The interface lets users explore selections, inspect how their numerical scores were formed, and review contemporaneous evidence for and against each investment thesis.

## Try the offline terminal

1. Find the HTML dashboard file in this repository (`index.html` if renamed accordingly).
2. Download the **raw `.html` file**. On GitHub, open the file and select **Download raw file**.
3. Double-click the downloaded file to open it in a modern desktop browser.
4. Explore **Core Alpha 20** and **Convex Alpha 3**, search by ticker, and select holdings to read their explanations.

**No installation, internet connection, server, login, OpenAI account, or API key is required.**

The HTML contains a fixed snapshot of the research output; it does not retrain models or refresh market data.

### What you can explore

- Switch between the 20-stock Core portfolio and the three-stock Convex portfolio.
- Search, sort, and inspect individual holdings.
- View the scores and contributions from **all five Core models**.
- Examine **LambdaRank TreeSHAP** feature drivers for the concentrated Convex selections.
- Read up to three evidence-backed advantages and three material risks per company, with source details where validated.
- Export an individual holding's research record as JSON, or print its research view.

**Research completeness:** In the initial June 2023 snapshot, 19 of the 20 holdings have validated bull/bear research memos. One memo remains unavailable because its evidence quotations did not pass strict source validation. Missing analysis is displayed as missing rather than fabricated.

## How the system works

```text
Historical point-in-time market + fundamental features
                       |
                       v
          CORE ALPHA — 5-model ensemble
          |-- LightGBM             25.0%
          |-- Random Forest        25.0%
          |-- Logistic Regression  25.0%
          |-- MLP Neural Network   12.5%
          |-- Residual Network     12.5%
                       |
                       v
             Frozen Core Alpha Top 20
                       |
                       v
        CONVEX ALPHA — LightGBM LambdaRank
                       |
                       v
             Frozen Convex Alpha Top 3
                       |
          +------------+------------+
          |                         |
          v                         v
  Quantitative attribution   Luna research analyst
  - Five Core model votes    - Up to 3 strengths
  - LambdaRank TreeSHAP      - Up to 3 material risks
                             - Timestamped source evidence
          |                         |
          +------------+------------+
                       |
                       v
             Offline HTML Research Terminal
```

**Design principle:** The quantitative models select securities. Luna is a *post-selection research assistant*: it investigates potential advantages and risks using historical documents. **Its prose does not alter frozen portfolio membership or ranking.**

### Understanding the explanations

**Core Alpha:** The dashboard displays exact **model-family score contributions**, including MLP and ResNet. These are based on each model's weighted, centered cross-sectional score percentile—not five sets of feature-level SHAP values. They show which models supported or opposed a security's ensemble score; they do **not** imply a predicted percentage return.

**Convex Alpha:** The dashboard displays genuine **TreeSHAP feature attributions** for the fitted LambdaRank ranking score. These explain the ranker's score, not a calibrated probability of beating the benchmark or a causal effect on future returns.

**Luna:** Positive and negative research observations are tied to source material available before the historical selection cutoff. The system checks source identifiers and quotations; claims without sufficient validated evidence are omitted. An evidence-backed statement is not automatically a correct valuation or forecast.

## Historical snapshot and evaluation

- **Portfolio selection date:** June 30, 2023.
- **Forecasting objective:** Explore securities that might outperform **VOO** over the **following 12 months** (roughly June 2023 to June 2024), measured in percentage-point excess return.
- **Historical training discipline:** Models were fitted on earlier chronological training examples with forward-return label-maturity purging. For the January–June 2023 validation fold, the latest training labels had matured by late December 2022.
- **Research evaluation:** The development notebooks examine 12 monthly selection cohorts spanning July 2022 through June 2023.

### Retrospective diagnostics — not live performance

| Research strategy | Mean 12-month forward excess versus VOO across 12 historical monthly cohorts |
|---|---:|
| Core Alpha 20 | **+22.25 percentage points** |
| Convex Alpha 3 | **+63.00 percentage points** |

**Interpret these numbers carefully.**

These are research-stage averages of **overlapping 12-month forward-label cohorts**, not an investable equity curve, a compounded portfolio CAGR, or a forecast for today's market.

These historical folds were consulted repeatedly during model development, so they are **not an untouched final holdout**. Results can be optimistic and are not evidence that comparable future returns will occur.

Portfolio implementation, survivorship and point-in-time constituent eligibility, delistings, dividends, trading costs, taxes, and execution assumptions require further independent audit.

**The percentages above describe historical realized-label diagnostics. The terminal does not yet output a calibrated, live 12-month excess-return prediction per stock.**

## Data and research provenance

The research pipeline draws on historical quantitative data and primary financial documents, including SEC EDGAR filings, supplemented by dated financial-news excerpts.

The offline export includes selected research material and citations, but it is **not** a complete public reproduction of the original data pipeline.

Historical research is limited to information judged available at the selection date. Source availability is tracked, and the system abstains when evidence cannot be verified.

Source-grounding mitigates hallucinations; it does not make automated analysis infallible.

**Licensing note:** Financial-data and news-content providers may restrict redistribution. Review those rights before publishing additional datasets, long excerpts, or model outputs derived from licensed content. Publicly posted HTML can be downloaded and inspected by anyone.

## What's included here?

This repository is currently intended to demonstrate the **exported offline terminal**.

The HTML is a static, self-contained artifact. Reproducing the original training process requires the separate research notebooks, datasets, and model artifacts, which may not be included in this public-facing repository.

For project maintainers, the broader workflow is:

1. Frozen quantitative research and historical selections — Notebook 04.
2. Explainability artifact export with parity and SHAP-additivity checks.
3. Evidence retrieval, post-selection Luna bull/bear memos, and application-data assembly — Notebook 05.
4. Self-contained HTML export for offline viewing.

## Roadmap

### Near term — Research beta

- [x] Five-family frozen Core Alpha ranking
- [x] Concentrated Convex Alpha LambdaRank selection
- [x] Historical model-parity and LambdaRank SHAP-additivity audits
- [x] Offline interactive portfolio and company-research dashboard
- [x] Five model-family contributions visible in Core Alpha
- [x] Source-validated bull/bear memos for 19/20 displayed holdings
- [ ] Recover the remaining failed research memo without weakening citation checks
- [ ] Recover fitted Core model artifacts for genuine *feature-level* Core attributions
- [ ] Complete an untouched chronological holdout and executable portfolio accounting

### Future iterations

- Calibrated, prospective 12-month excess-return forecasts with uncertainty estimates
- Point-in-time refreshed portfolios and rigorous delisting/dividend/cost treatment
- Company valuation dashboards with auditable DCF calculations and sensitivity analysis
- Private authenticated deployment, scheduled updates, and appropriate legal/data-licensing review

## Disclaimer

**For educational and research purposes only.**

Nothing in this repository constitutes individualized investment advice, a solicitation to transact, a promise of performance, or a substitute for professional due diligence.

Historical backtests and automated document summaries are subject to data errors, methodological limitations, and uncertainty. Investment decisions carry risk, including loss of principal.

---

*Persona Alpha — Quantitative discovery. Explainable decisions. Evidence on both sides.*

