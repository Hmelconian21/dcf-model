# dcf-model

Two-stage discounted cash flow valuation for any ticker, with estimated WACC, tapered growth, and a sensitivity grid, using live data from yfinance.

## Run

Requires Python 3.10+.

```bash
pip install -r requirements.txt
python3 dcf.py NVDA
python3 dcf.py NVDA --sensitivity
python3 dcf.py --help
```

Educational tool, not investment advice.
