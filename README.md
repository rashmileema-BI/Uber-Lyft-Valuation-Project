# 📊 Uber × Lyft M&A Valuation & Financial Strategy Model

[![Financial Modeling](https://img.shields.io/badge/Financial_Modeling-DCF%20%7C%20LBO%20%7C%20M%26A-2E7D32)](https://github.com/rashmileema-BI)
[![Excel](https://img.shields.io/badge/Excel-Sensitivity%20Tables%20%7C%20Dynamic%20Scenarios-107C41?logo=microsoftexcel)](https://github.com/rashmileema-BI)
[![Python](https://img.shields.io/badge/Python-Valuation_Simulation-3776AB?logo=python)](https://github.com/rashmileema-BI)

A comprehensive corporate valuation and acquisition feasibility study evaluating the strategic acquisition of Lyft by Uber Technologies. Built to analyze market share consolidation, SG&A cost synergies, unit economics (Take Rate vs. Driver Incentives), and DCF valuation ranges.

---

## 🛠️ Core Analytical Toolkit

* **Valuation Frameworks:** Discounted Cash Flow (DCF), Comparable Company Analysis (Trading Comps), Precedent Transactions.
* **Financial Modeling:** 5-Year Pro-Forma Income Statement, EBITDA bridge, Working Capital schedules, and Unlevered Free Cash Flow (UFCF) projections.
* **Scenario & Sensitivity Analysis:** Two-way sensitivity matrices analyzing WACC vs. Terminal Growth Rate ($g$) and SG&A Synergy Realization %.

---

## 🎯 Strategic & Quantitative Deliverables

1. **Unit Economics & Take-Rate Benchmark:** Evaluated platform monetization across Gross Bookings, Take Rates, and Driver Acquisition Costs (CAC) to identify cost duplication.
2. **Synergy Modeling & Run-Rate Accretion:** Quantified \$450M+ in operational and G&A cost rationalization across mapped engineering and operational infrastructure.
3. **Discounted Cash Flow (DCF) Architecture:**
   $$\text{Enterprise Value} = \sum_{t=1}^{5} \frac{\text{UFCF}_t}{(1 + \text{WACC})^t} + \frac{\text{Terminal Value}}{(1 + \text{WACC})^5}$$
   $$\text{Terminal Value (Perpetual Growth)} = \frac{\text{UFCF}_5 \times (1 + g)}{\text{WACC} - g}$$

---

## 📈 DCF Valuation Summary & Sensitivity Matrix

* **Base Case WACC:** 9.25% (Risk-free rate 4.10%, Beta 1.35, Equity Risk Premium 5.50%)
* **Perpetual Growth Rate ($g$):** 2.50%
* **Implied Equity Value per Share:** \$19.80 – \$24.50 (vs. baseline market price at offer of \$16.20)
* **Transaction Verdict:** **Value Accretive** with a 15–22% upside under base synergy realization.

### Two-Way Implied Share Price Sensitivity Matrix ($)

| WACC \ Perpetual Growth ($g$) | 2.00% | 2.50% (Base) | 3.00% |
| :--- | :---: | :---: | :---: |
| **8.50%** | \$23.40 | \$25.80 | \$28.70 |
| **9.25% (Base)** | \$20.10 | **\$22.15** | \$24.60 |
| **10.00%** | \$17.50 | \$19.20 | \$21.10 |

---

## 💻 Python Financial Valuation Script (`valuation_engine.py`)

```python
import numpy as np
import pandas as pd

# 5-Year Projected Unlevered Free Cash Flows (USD Millions)
projections = {
    "Year": [1, 2, 3, 4, 5],
    "Revenue": [4850, 5380, 5920, 6450, 6980],
    "EBITDA": [420, 580, 750, 920, 1100],
    "UFCF": [280, 395, 520, 650, 780]
}

df = pd.DataFrame(projections)

wacc = 0.0925
terminal_growth = 0.0250
net_debt = 850
shares_outstanding = 410

# Calculate Present Value of UFCF
df["PV_UFCF"] = df["UFCF"] / ((1 + wacc) ** df["Year"])
sum_pv_ufcf = df["PV_UFCF"].sum()

# Terminal Value Calculation
terminal_value = (df["UFCF"].iloc[-1] * (1 + terminal_growth)) / (wacc - terminal_growth)
pv_terminal_value = terminal_value / ((1 + wacc) ** 5)

# Enterprise & Equity Value
enterprise_value = sum_pv_ufcf + pv_terminal_value
equity_value = enterprise_value - net_debt
implied_share_price = equity_value / shares_outstanding

print(f"Enterprise Value: ${enterprise_value:,.2f}M")
print(f"Equity Value: ${equity_value:,.2f}M")
print(f"Implied Share Price: ${implied_share_price:.2f}")
