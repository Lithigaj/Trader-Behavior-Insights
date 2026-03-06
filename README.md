# 📊 Trader Performance vs Market Sentiment

> Analyzing how Bitcoin Fear/Greed sentiment relates to trader behavior and performance on Hyperliquid DEX

---

## 📖 Table of Contents
- [Overview](#overview)
- [Datasets](#datasets)
- [Project Structure](#project-structure)
- [Installation](#installation)
- [How to Run](#how-to-run)
- [Methodology](#methodology)
- [Key Insights](#key-insights)
- [Strategy Recommendations](#strategy-recommendations)
- [Charts](#charts)
- [Tech Stack](#tech-stack)

---

## Overview

This project investigates whether Bitcoin market sentiment (Fear vs Greed) influences trader decision-making and profitability on the Hyperliquid decentralized exchange. The analysis covers:

- ✅ Performance differences (PnL, win rate, drawdown) across sentiment regimes
- ✅ Behavioral shifts (trade frequency, position size, long/short bias)
- ✅ Trader segmentation into 3 behavioral groups
- ✅ Actionable strategy rules derived from findings
- ✅ **Bonus:** Random Forest profitability classifier + K-Means behavioral clustering

---

## Datasets

| Dataset | File | Rows | Columns | Date Range |
|---------|------|------|---------|------------|
| Bitcoin Fear/Greed Index | `fear_greed_index.csv` | 2,644 | 4 | Feb 2018 – May 2025 |
| Hyperliquid Trader Data | `historical_data.csv` | — | 16 | — |

### Columns

**`fear_greed_index.csv`**
```
timestamp | value | classification | date
```

**`historical_data.csv`**
```
Account | Coin | Execution Price | Size Tokens | Size USD | Side |
Timestamp IST | Start Position | Direction | Closed PnL | Transaction Hash |
Order ID | Crossed | Fee | Trade ID | Timestamp
```

---

## Project Structure

```
├── 📓 trader_sentiment_analysis.ipynb   # Main analysis notebook
├── 📄 README.md                         # This file
├── 📂 data/
│   ├── fear_greed_index.csv             # Bitcoin Fear/Greed Index
│   └── historical_data.csv             # Hyperliquid trade history
└── 📂 charts/
    ├── chart1_performance.png           # PnL, win rate, drawdown by sentiment
    ├── chart2_behavior.png              # Trade frequency, size, long ratio
    ├── chart3_segments.png              # Trader segment comparison
    ├── chart4_heatmap.png               # Sentiment × segment heatmaps
    ├── chart5_timeline.png              # PnL over time colored by sentiment
    ├── chart6_coins.png                 # Top coins by sentiment
    ├── chart7_model.png                 # Random Forest feature importance
    └── chart8_clustering.png           # K-Means behavioral archetypes
```

> **Note:** For simplicity, you can place all files (notebook + CSVs) in the same folder instead of using subfolders.

---

## Installation

**Prerequisites:** Python 3.8+

Install all required libraries with:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
```

---

## How to Run

**Step 1** — Clone the repository
```bash
git clone https://github.com/Lithigaj/Trader-Behavior-Insights
cd Trader-Behavior-Insights
```

**Step 2** — Place the data files in the same folder as the notebook
```
fear_greed_index.csv
historical_data.csv
trader_sentiment_analysis.ipynb
```

**Step 3** — Launch Jupyter and run all cells
```bash
jupyter notebook trader_sentiment_analysis.ipynb
```
Then go to: **Cell → Run All**

---

## Methodology

1. **Data Cleaning**
   - Removed duplicates from both datasets
   - Filled missing `Closed PnL` and `Size USD` values with `0` (representing open/unclosed trades)

2. **Date Alignment**
   - Parsed `Timestamp IST` column (format: `DD-MM-YYYY HH:MM`) from trades data
   - Merged with sentiment index on a daily date key

3. **Feature Engineering**
   - Computed daily metrics per trader: total PnL, win rate, trade count, average position size, long/short ratio
   - Added drawdown proxy: cumulative PnL minus its running peak per trader

4. **Sentiment Mapping**
   - Collapsed 5 original labels → 3 broad classes for cleaner analysis:
     - `Extreme Fear` + `Fear` → **Fear**
     - `Neutral` → **Neutral**
     - `Greed` + `Extreme Greed` → **Greed**

5. **Trader Segmentation** (3 schemes)
   - Frequent vs Infrequent (split at median trade count)
   - Large vs Small position size (split at median size USD)
   - Consistent Winner / Consistent Loser / Mixed (based on total PnL + win rate threshold)

6. **Bonus**
   - Random Forest classifier to predict profitable trading days
   - K-Means clustering (k=3) to identify behavioral archetypes

---

## Key Insights

| # | Insight |
|---|---------|
| 1 | 📉 **Fear days = lower PnL and win rates** — drawdowns are deepest across all trader segments |
| 2 | 📈 **Greed days = more trades, bigger sizes, stronger long bias** — momentum-chasing behaviour dominates |
| 3 | ⚡ **Frequent traders outperform on Greed days** but both segments suffer equally on Fear days |
| 4 | 🏆 **Consistent Winners are sentiment-resilient** — stable win rates across all market regimes |
| 5 | 🪙 **Coin selection matters** — some assets remain profitable regardless of sentiment regime |

---

## Strategy Recommendations

| Rule | Condition | Action |
|------|-----------|--------|
| **Rule 1** | Fear/Greed score < 40 *(Fear zone)* | Reduce position size by **30–40%** across all trader segments |
| **Rule 2** | Fear/Greed score > 60 *(Greed zone)* | Frequent traders: scale up + lean long. Infrequent traders: max **1–2 high-conviction setups** |
| **Rule 3** | Always | Benchmark sizing and trade selectivity against **Consistent Winner** archetype behavior |

---

## Charts

| Chart | Description |
|-------|-------------|
| `chart1_performance.png` | Avg PnL, Win Rate, and Drawdown across Fear / Neutral / Greed |
| `chart2_behavior.png` | Trade frequency, position size, and long ratio by sentiment |
| `chart3_segments.png` | PnL and win rate comparison across 3 trader segments |
| `chart4_heatmap.png` | Heatmap: sentiment × frequency segment and sentiment × performance segment |
| `chart5_timeline.png` | Market-wide daily PnL over time, colored by sentiment regime |
| `chart6_coins.png` | Avg PnL per trade for top 8 coins, split by sentiment |
| `chart7_model.png` | Random Forest feature importance + confusion matrix |
| `chart8_clustering.png` | K-Means trader archetypes scatter plot + elbow curve |

---

## Tech Stack

![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=flat&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=flat&logo=numpy&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557C?style=flat)
![Seaborn](https://img.shields.io/badge/Seaborn-4C9BE8?style=flat)
![Scikit--learn](https://img.shields.io/badge/Scikit--learn-F7931E?style=flat&logo=scikit-learn&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=flat&logo=jupyter&logoColor=white)

---
