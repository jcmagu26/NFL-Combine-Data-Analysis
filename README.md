# 🏈 NFL Combine Data Analysis

Author: Jane Maguire  
Date: 2024-04-18

This project explores relationships between **NFL Scouting Combine** measurements and **2018 on-field performance** (rushing, passing, receiving, and defensive stats). Using exploratory data analysis (EDA), data transformations, and regression diagnostics, the analysis investigates which combine attributes may relate to player performance and where linear modeling assumptions hold (or fail).

---

## 📦 Datasets

- `NFLcombine.csv` — Player combine measurements (e.g., weight, bench, broad jump, position).
- `defense_2018.csv` — Defensive stats (e.g., Solo tackles).
- `receiving_2018.csv` — Receiving stats (e.g., receptions, games played).
- `rushing_2018.csv` — Rushing stats (e.g., average rushing yards).

**Sources**
- Combine results: https://nflcombineresults.com/
- 2018 game stats: https://www.footballdb.com/
- Combine test descriptions: https://en.wikipedia.org/wiki/NFL_Scouting_Combine
- Course notes/templates: https://mgimond.github.io/ES218/

---

## 🛠️ Tech Stack

- **Language**: R (RMarkdown)
- **Packages**: `dplyr`, `ggplot2`, `tidyr`, `tukeyedar`, `knitr`
- **Rendering**: HTML with table of contents and code folding

---

## 🔍 Analysis Overview

1. **Position vs Weight (Combine)**
   - Boxplots show strong positional differences in weight, especially **CB vs OT**.
   - QQ-plot & density plots suggest offset transformation: `CB * 1.5 + 27`.

2. **Assumptions: Normality & Equal Variance**
   - Pooled QQ-plot: approximately normal residuals by position.
   - S–L plot: variance independent of medians → constant spread.

3. **Defense Merge: Solo Tackles vs Broad Jump**
   - Raw data heavily skewed; applied Box–Cox (λ = 2).
   - Assumptions not fully satisfied → inconclusive model.

4. **Games Played vs Receptions**
   - Positive trend: more games → more receptions, but wide spread.
   - Simple regression not appropriate due to heteroscedasticity.

5. **Rushing Merge: Bench vs Avg Rushing Yards**
   - Raw scatter: weak negative trend with spread issues.
   - Box–Cox (λ = 0.5) improved assumptions.
   - Final model reasonable: √Avg Rushing ~ Bench.

---

## ✅ Key Findings

- **Player position strongly determines combine weight.**
- **CB vs OT** weight distributions differ by ~multiplicative 1.5 + additive 27 offset.
- **Transformations (Box–Cox)** were crucial for meeting regression assumptions.
- **Games played drives performance stats**, making raw metrics misleading.
- Some combine–performance links (e.g., Bench vs Avg Rush) can be modeled, others remain inconclusive.

---

## 🏁 Conclusion

This analysis highlighted both the potential and the challenges of linking NFL combine metrics to player performance. Positional weight differences were clearly defined, while relationships like Bench vs Avg Rushing required careful transformation to satisfy regression assumptions. Other links (e.g., Solo Tackles vs Broad Jump) proved inconclusive due to unmet assumptions. Overall, the study shows that combine measures can provide useful context but should be interpreted cautiously alongside in-game statistics. Future work could extend this by applying multivariate models or considering interaction effects across multiple performance metrics.
