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
   - Removed sparse positions (`NT`, `LB`, `LS`) and a header row labeled `Pos`.
   - Boxplots (ordered by median) show clear weight differences by position.
   - Deep dive on **CB vs OT** using a QQ-plot suggests a **multiplicative + additive offset**:
     - Transform aligning CB to OT: `CB * 1.5 + 27`
   - Overlapping density plots confirm the offset models distributional shift well.

2. **Assumptions: Normality & Equal Variance**
   - **Pooled Residual QQ-Plot** (by position): residuals ~ normal.
   - **S–L Plot**: no monotonic spread trend → supports constant variance (by position).

3. **Defense Merge: Solo Tackles vs Broad Jump**
   - `defense_combine <- right_join(combine, defense, by = c("name" = "Player"))`
   - Raw scatter: left-skewed; many players with limited games → stats cluster near zero.
   - Applied **Box–Cox** (power = 2) to broad jump: improved linear fit but
     - S–L plot shows decreasing spread → assumptions not fully met.
   - **Conclusion**: proceed with caution; stopped here for this pairing.

4. **Games Played vs Receptions (Receiving)**
   - Positive trend: more games → more receptions, but spread increases with games.
   - Linear regression not ideal due to heteroscedasticity and logical constraints.

5. **Rushing Merge: Bench vs Avg Rushing Yards**
   - `rushing_combine <- right_join(combine, rushing, by = c("name" = "Player"))`
   - Raw relationship slightly negative with heteroscedasticity.
   - **Box–Cox** (λ = 1/2) on `Avg`:
     - Residual-dependence plot: no strong pattern (good).
     - S–L plot: no monotonic trend (good).
   - **Conclusion**: transformed model is a reasonable candidate for Avg Rushing ~ Bench.

---

## ✅ Key Findings

- Positions have **distinct weight profiles**; the largest gap is **CB vs OT**.
- Distributional alignment for CB→OT is well-captured by: `CB * 1.5 + 27`.
- Some **combine-performance** relationships require **re-expression** to approach linearity.
- Many performance stats are influenced by **games played**, driving skew and dispersion.
- **Bench vs Avg Rushing** benefits from √-transform on Avg, improving assumptions.

---

## 🏁 Conclusion

This analysis highlighted both the potential and the challenges of linking NFL combine metrics to player performance. Positional weight differences were clearly defined, while relationships like Bench vs Avg Rushing required careful transformation to satisfy regression assumptions. Other links (e.g., Solo Tackles vs Broad Jump) proved inconclusive due to unmet assumptions. Overall, the study shows that combine measures can provide useful context but should be interpreted cautiously alongside in-game statistics. Future work could extend this by applying multivariate models or considering interaction effects across multiple performance metrics.
