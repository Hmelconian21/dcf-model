# DCF Model

A two-stage discounted cash flow model that values any ticker from live yfinance data. It estimates its own WACC, tapers growth down to a terminal rate, and prints a sensitivity grid of fair values.

## Sample output

`python3 dcf.py MSFT --sensitivity`, run on 2026-10-07 (trimmed):

```
Using as base (3-yr average): $70,889,666,667
  FCF growth rate used:     4.0%  (historical CAGR: 4.0%)
  Analyst forward estimate: 31.7%  (cross-check only, not used automatically)  <- differs meaningfully from historical CAGR, worth digging into why
  Discount rate used:       10.0%  (estimated WACC)
  Cost of debt:             7.6%  (from actual interest expense / debt)
  Debt used (net debt/WACC): $40,294,000,000  (long-term + current debt from latest quarter's balance sheet (excludes lease liabilities))

  IMPLIED FAIR VALUE:     $140.87
  CURRENT PRICE:          $528.75
  IMPLIED DOWNSIDE:        -73.4%

SENSITIVITY: IMPLIED FAIR VALUE PER SHARE
  Growth \ Disc       8.0%       9.0%      10.0%      11.0%      12.0%
           0.0%    $174.32    $148.14    $128.99    $114.37    $102.84
           4.0%    $190.84    $161.98    $140.87    $124.76    $112.05
           8.0%    $208.46    $176.74    $153.53    $135.82    $121.86
```

## What it does

- **Base free cash flow:** operating cash flow minus capex, averaged over the last 3 fiscal years to smooth out one-off years. `--use-latest-fcf` uses the most recent year instead.
- **Tapered growth:** starts at the company's historical FCF growth rate (CAGR), or 8% when there isn't enough history, and steps down linearly to a 2.5% terminal rate over 5 years. Wall Street's forward estimate is printed as a cross-check and flagged when it differs by more than 10 points.
- **Estimated WACC:** cost of equity is a 4.5% risk-free rate plus beta × a 5% equity risk premium. Cost of debt is interest expense ÷ debt from the same fiscal year, with an assumed 21% tax rate. Debt excludes lease liabilities.
- **Sensitivity grid:** a 5×5 table of fair value per share, varying growth by ±2 and ±4 points and the discount rate by ±1 and ±2 points. The model also warns when base FCF is under 1.5% of market cap or negative, where DCF output is unreliable.

## How to run it

Requires Python 3.10+.

```bash
git clone https://github.com/Hmelconian21/dcf-model.git
cd dcf-model
pip install -r requirements.txt

python3 dcf.py MSFT                       # base case
python3 dcf.py MSFT --sensitivity         # add the growth × discount-rate grid
python3 dcf.py MSFT --growth 0.10 --discount-rate 0.09 --terminal-growth 0.03
python3 dcf.py --help                     # all options, incl. --years, --flat-growth
```

*Educational tool, not investment advice.*

Part of a set of Python finance tools — see [Stock Research System](https://github.com/Hmelconian21/Stock-Research-System) and the [live risk dashboard](https://henry-risk-dashboard.streamlit.app).
