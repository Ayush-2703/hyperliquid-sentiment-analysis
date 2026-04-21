# 📊 Hyperliquid × Fear/Greed — Sentiment-Driven Trader Analysis

> Analyzing how Bitcoin market sentiment shapes trader behavior and performance on Hyperliquid DEX.

---

## 🗂 Repository Structure

```
├── analysis.ipynb          # Full analysis notebook (Parts A–C + Bonus)
├── fear_greed_index.csv    # Bitcoin Fear/Greed Index (2018–2025)
├── historical_data.csv     # Hyperliquid trade history (~211K trades)
├── README.md               # This file
└── charts/
    ├── chart1_pnl_winrate.png
    ├── chart2_behavior.png
    ├── chart3_segments.png
    ├── chart4_cumulative.png
    ├── chart5_distribution.png
    ├── chart6_interaction.png
    └── chart7_importance.png
```

---

## ⚙️ Setup & How to Run

```bash
# 1. Clone / extract the repo
# 2. Install dependencies
pip install pandas numpy matplotlib seaborn scikit-learn pyarrow jupyter

# 3. Launch the notebook
jupyter notebook analysis.ipynb
```

All cells are self-contained and run top-to-bottom. Charts are auto-saved as PNGs.

---

## 📐 Methodology

### Data Preparation (Part A)
- **Fear/Greed Index**: 2,644 daily rows (Feb 2018 – May 2025), zero missing values. Classifications: Fear, Extreme Fear, Neutral, Greed, Extreme Greed. Simplified to 3 bins (Fear / Neutral / Greed) for statistical power.
- **Hyperliquid Trades**: 211,224 rows across **32 unique accounts**, parsed from `DD-MM-YYYY HH:MM` IST timestamps. Aligned to daily granularity for the merge (99.997% match rate).
- **Key metrics engineered**: daily PnL per account, win rate, trade frequency, average position size (USD), long/short ratio (using Open Long / Open Short directions), cumulative PnL.

### Analysis (Part B)
Three analysis dimensions:
1. **Performance** — median/mean daily PnL, win rate, drawdown % across sentiment regimes.
2. **Behavior** — trade frequency, position sizing, long/short bias across sentiment regimes.
3. **Segmentation** — traders split into 3 quantile tiers on (a) trade frequency, (b) win rate consistency, (c) avg position size.

---

## 💡 Key Insights

### Insight 1 — Greed Days Produce the Highest Median PnL
Median daily PnL: **Greed $909 > Fear $663 > Neutral $568**.  
While Fear days have the highest *mean* PnL ($7,161 vs $5,765), this is driven by outsized winning trades. On a typical day, Greed conditions are slightly more profitable.

### Insight 2 — Fear Triggers Larger, More Bullish Positioning (Contrarian Behavior)
During Fear days, traders increase average position size by **+70%** vs Greed days ($11,593 vs $6,822) and the long/short ratio surges to **8.9x** (vs 1.6x on Greed days). Hyperliquid traders systematically *buy the dip* — and the data shows it pays off.

### Insight 3 — High-Frequency Traders Underperform
Low-frequency traders (bottom tertile by trade count) earn a **3× higher median total PnL** than high-frequency traders. More trades ≠ more profit on Hyperliquid. Disciplined, selective trading wins.

### Insight 4 — Win Rates Are Robust Across All Sentiment (~84–86%)
Win rates barely move across sentiment (84.2% Fear → 85.6% Greed). The sentiment signal primarily affects *magnitude* of winning and losing trades, not the direction of outcomes. This suggests a skilled trader base.

### Insight 5 — Extreme Fear = Peak Activity & Largest Positions
Traders are most active (70 trades/day/account) and use the largest positions during Fear/Extreme Fear, then become more passive during Greed. This is classic market-cycle opportunism.

---

## 🎯 Strategy Recommendations (Part C)

### Strategy 1: Fear-Day Aggression for Consistent Winners
**Rule**: Accounts with 30-day rolling win rate > 85% should increase position size by **~25%** on Fear-classified days, with a long bias.  
**Rationale**: This cohort generates its highest mean PnL during Fear days ($7,161/day), driven by the validated buy-the-dip signal (L/S ratio 8.9x). The data confirms the instinct is profitable, not just frequent.

### Strategy 2: Frequency Throttle for Small Traders During Greed
**Rule**: Traders with avg position size < $2,500 should reduce daily trade count by **~30%** on Greed days.  
**Rationale**: Small-size traders show their worst relative performance on Greed days when they over-trade. Large traders outperform by being selective and sizing up. Throttling frequency during euphoric markets protects small accounts from FOMO-driven overtrading.

---

## 🤖 Bonus: Predictive Model

A **Gradient Boosting Classifier** was trained to predict next-day PnL bucket (Loss / Small Win / Big Win) using:
- Today's sentiment (encoded)
- Yesterday's PnL, trade count, win rate
- Average position size

**Results**: 5-fold CV accuracy **60.4% ± 1.6%** vs 57.4% majority-class baseline (+3.1pp lift).  
The most important feature is **previous win rate**, followed by **sentiment** and **average position size** — confirming that sentiment has real predictive value even controlling for individual trader quality.

---

## 📊 Charts Overview

| Chart | What it shows |
|-------|---------------|
| Chart 1 | Median daily PnL and win rate by sentiment |
| Chart 2 | Trade frequency, avg size, L/S ratio by sentiment |
| Chart 3 | Median total PnL by trader segment (freq / win rate / size) |
| Chart 4 | Cumulative PnL over time shaded by sentiment regime |
| Chart 5 | PnL distribution boxplots + drawdown % by full classification |
| Chart 6 | Sentiment × segment interaction (which segment benefits most from Fear/Greed) |
| Chart 7 | Feature importances from next-day PnL predictor |

---

*Dataset period: May 2023 – May 2025 | Trades: 211,224 | Accounts: 32 | Sentiment observations: 730 days matched*
