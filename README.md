📊 Hyperliquid × Fear/Greed Sentiment Analysis
How market emotion shapes trader behavior and performance on Hyperliquid

🚀 Setup & How to Run
Requirements
bash
pip install pandas numpy matplotlib seaborn scikit-learn pyarrow
Run the Analysis
bash
# Place both datasets in the same directory:
#   historical_data.csv
#   fear_greed_index.csv

jupyter notebook analysis_notebook.ipynb
# OR
python3 analysis_script.py
📁 Repository Structure
├── analysis_notebook.ipynb   # Full analysis notebook (recommended)
├── README.md                 # This file
├── historical_data.csv       # Input: Hyperliquid trade data
├── fear_greed_index.csv      # Input: Bitcoin Fear/Greed index
└── charts/
    ├── chart1_performance.png     # PnL & Win Rate by sentiment
    ├── chart2_behavior.png        # Behavior shifts by sentiment
    ├── chart3_segments.png        # Trader segmentation
    ├── chart4_timeseries.png      # Drawdown & risk
    ├── chart5_heatmap.png         # Feature importance (model)
    ├── chart6_model_clusters.png  # Predictive model + clusters
    └── chart7_dashboard.png       # Summary dashboard
📋 Methodology
Data Sources
Dataset	Rows	Date Range	Key Fields
Fear/Greed Index	2,644	2018–2025	value (0–100), classification
Hyperliquid Trades	211,224	2023–2025	Account, PnL, Size, Direction
Part A — Data Preparation
Loaded & audited both datasets: 0 missing values, 0 duplicates in either
Parsed timestamps: Timestamp IST format dd-mm-yyyy HH:MM → date string, aligned with FG date keys
Merged on date: 211,218 matched rows across 479 trading days
Derived metrics:
daily_pnl: sum of Closed PnL per account-day (closing trades only)
win_rate: fraction of trades with PnL > 0
trade_count: closing trade volume per account-day
avg_size_usd: average position size
ls_ratio: long/(long+short) ratio from open trades
max_loss: worst single trade (drawdown proxy)
4 trader segments via K-Means clustering
🔍 Key Insights
Insight 1 — Win Rate Peaks at Sentiment Extremes (Not Greed)
Extreme Greed wins: 89.1% win rate, $130 mean PnL/trade
Fear beats Greed on win rate: 87.1% vs 76.3%
Greed and Extreme Fear are similar at ~76%
⚠️ Contrarian observation: "greedy" days are NOT the most profitable for these traders
Insight 2 — Traders Go Long During Fear (Buy the Dip)
Fear days: 72.3% of new positions are LONG
Greed days: only 44.4% long (more short-selling / balanced)
Neutral: 61.2% long
This "dip-buying" behavior during fear is profitable — Fear days show 87.1% win rates
Insight 3 — Smaller Position Sizes on High-Sentiment Days
Median position size drops from $835 (Fear) → $555 (Greed) → $436 (Extreme Greed)
Despite smaller sizes, Extreme Greed shows highest mean PnL — fewer but more precise trades
Larger positions during fear = higher absolute exposure but still high win rates
Insight 4 — Drawdown is Worst on Fear Days
Avg worst single-trade loss: Fear = -$387, Greed = -$375, Neutral = -$311
Fear increases volatility in both directions — bigger wins AND bigger losses
Insight 5 — Four Distinct Trader Archetypes Exist
Archetype	Active Days	Win Rate	Avg Size	Total PnL
🐋 Mega Whale	~19	77%	$61,348	$1.6M
🎯 Precision Snipers	~22	92%	$14,753	$242K
📈 Steady Grinders	~154	86%	$5,217	$563K
⚡ Mid-Freq Alphas	~31	69%	$6,658	$114K
💡 Strategy Recommendations
Strategy 1 — The Fear-Dip Long Protocol
When to use: Fear/Extreme Fear days (index < 30)

Rules:

✅ Increase long bias — win rate is 87% on Fear days for longs
✅ Keep position sizes smaller than your usual — risk of large individual losses is elevated
✅ Best for Precision Snipers & Steady Grinders archetypes
❌ Avoid high-leverage directional bets — drawdown is worst during Fear (-$387 avg)
Why it works: Traders who buy dips during fear and close into relief rallies achieve the highest win rates (87.1%) in the dataset.

Strategy 2 — The Greed Rebalance Rule
When to use: Greed/Extreme Greed days (index > 60)

Rules:

✅ Reduce long exposure — shift to 50/50 or even slightly short-biased
✅ Trim position sizes — Extreme Greed sees smaller sizes yet HIGHEST mean PnL ($130/trade)
✅ Take profits aggressively — Extreme Greed is the sentiment that precedes reversals
❌ Do NOT chase momentum with oversized longs — this is where Mid-Freq Alphas underperform
Why it works: Extreme Greed generates the best mean PnL/trade ($130) but with the smallest median sizes — suggesting the winning move is selective, high-confidence trades, not broad long exposure.

Bonus Rule — Match Your Archetype to Your Strategy
Steady Grinders: Trade daily with small sizes; consistent 86% WR validates high-frequency discipline
Precision Snipers: Trade only on clear setups; 92% win rate shows selectivity > frequency
Mid-Freq Alphas: Tighten up on Greed days — 69% WR suggests they give back profits during bullish sentiment
Mega Whales: Sentiment-agnostic; size is their edge, not timing
🤖 Predictive Model
A Random Forest classifier was trained to predict whether a given account-day would be profitable:

Accuracy: 94.3% (5-fold CV: 93.7% ± 1.1%)
Top features (by importance):
win_rate (0.68) — day-level trade accuracy dominates
fear_greed_value (0.069) — sentiment index is 2nd most important
trade_count (0.063)
avg_open_size (0.062)
Interpretation: The Fear/Greed index is a statistically significant predictor of day profitability, ranking 2nd out of 7 features.

Analysis period: May 2023 – May 2025 | 32 unique traders | 479 trading days | 211,218 trades



