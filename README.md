# REIT Cap Rate Spread Model
### Public real estate as an asset class — tracking the spread between REIT yields and the 10-Year Treasury

![Python](https://img.shields.io/badge/Python-3.10-blue) ![Status](https://img.shields.io/badge/Status-Complete-brightgreen) ![Tools](https://img.shields.io/badge/Built%20with-pandas%20%7C%20yfinance%20%7C%20Plotly-orange)

## Key Findings

- **Vornado (VNO) is the most extreme distress signal in the entire universe** — an 18% FFO yield against a 2% dividend yield implies an ~11% payout ratio, the lowest of any name analyzed. The market is pricing in significant cash-flow risk, not offering a clean bargain.
- **Healthcare's sector average (-8 bps) hides a sector that is actually bimodal**: Healthpeak (DOC) trades cheap (+259 bps, 86% payout) while Welltower (WELL) trades rich (-241 bps, 50% payout) — the two extremes of the entire 19-name dataset, both labeled "Healthcare."
- **Residential is the most internally consistent sector** — all four names (AVB, EQR, MAA, ESS) cluster within a 200 bps band and payout ratios of 50-67%, signaling market consensus on a stable risk profile.
- Sector-level averaging conceals more than it reveals for Office and Healthcare specifically — individual-ticker ranking is necessary to see the real story.

## What This Project Measures

| Metric | Formula | What it tells you |
|---|---|---|
| Dividend yield | Annual dividend / share price | Cash distributed to shareholders, as a % of price |
| FFO-yield proxy | (Net Income + D&A) / market cap | Closer estimate of true cash-generating yield — adds back the non-cash depreciation that distorts REIT earnings |
| Spread (bps) | REIT yield − 10Y Treasury yield | The risk premium real estate offers over the risk-free rate |
| Payout ratio | Dividend yield / FFO-yield | What share of generated cash flow is actually distributed — the sustainability check |

## Results

*Note: the notebook generates interactive Plotly charts when run live. The static images below are exported snapshots of those same charts, embedded here so the findings are visible directly on GitHub without needing to open or run the notebook.*

**All 19 REITs, ranked by FFO-yield spread vs. the 10-Year Treasury:**

   ![Ranked spread chart](01_ranked_spread.png)

**Sector averages — and why they can mislead** (note Healthcare's near-zero average, despite containing both the cheapest-looking and richest-looking names in the dataset):

![Sector spread chart](02_sector_spread.png)

**Dividend yield vs. FFO-yield proxy** — points far above the diagonal (VNO, KRC, BXP) retain much more cash than they distribute, consistent with conservative/defensive payout policy during a period of price pressure:

![Scatter chart](03_div_vs_ffo_scatter.png)

## Methodology

1. Pulled price, dividend yield, market cap, and income-statement data for 19 REITs across 6 property sectors via `yfinance`
2. Pulled the 10-Year Treasury yield from FRED's public CSV endpoint (no API key required)
3. Built an FFO-yield proxy (`Net Income + D&A`, divided by market cap) as a closer stand-in for true cap rates than dividend yield alone
4. Computed spread (REIT yield − Treasury yield) and payout ratio (dividend yield ÷ FFO yield) for every name
5. Ranked across the full universe (not just within sector) to surface names that sector averaging would hide

## Honest Limitations

- Dividend/FFO yield are **public market proxies**, not true property-level cap rates (NOI ÷ transaction price). Private market pricing from Green Street/CoStar/NCREIF can diverge meaningfully from public REIT pricing, especially at cycle turning points.
- The FFO-yield proxy is simplified (Net Income + D&A) and does not apply the full NAREIT FFO adjustment methodology (e.g., excluding gains on property sales).
- `yfinance` field availability and scaling (decimal vs. percent) varies by version — this project includes a normalization check to handle that inconsistency.

## Tools Used

Python · pandas · numpy · yfinance · FRED (public CSV endpoint) · Plotly · matplotlib · Google Colab

## How to Run

```bash
pip install -r requirements.txt
```
Then open `REIT_Cap_Rate_Spread_Model.ipynb` in Google Colab or Jupyter and run all cells.

## Next Steps

- Extend to a historical time series of the spread (quarterly snapshots over 5-10 years)
- Feed the current spread read into a property-level underwriting model's exit cap rate assumption
- Pull true FFO from company supplemental filings rather than approximating from GAAP net income

---
*This project is for educational and research purposes only and does not constitute investment advice.*
