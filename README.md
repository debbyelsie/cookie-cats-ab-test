# cookie-cats-ab-test
A/B test analysis of a mobile game's level gate placement: moving the gate from level 30 to 40 reduced 7-day retention. Python, statistical testing, bootstrap.
Cookie Cats A/B Test: Where Should the First Gate Go?

Question: Does moving Cookie Cats' first gate from level 30 to level 40 change player retention?

Finding: Moving the gate to level 40 lowered 7-day retention from 19.0% to 18.2% (p ≈ 0.002), while day-1 retention showed no significant change.

Recommendation: Keep the gate at level 30, verify the experiment's assignment logic (a minor sample imbalance was flagged), and test dynamic gates for fast-progressing players next.

<img width="822" height="393" alt="image" src="https://github.com/user-attachments/assets/f33e0cef-820e-43d5-9cb6-aa00e93f6d1b" />


## Key Results
 
| Metric | gate_30 | gate_40 | Significant? |
|---|---|---|---|
| Day-1 retention | 44.8% | 44.2% | No |
| Day-7 retention | 19.0% | 18.2% | Yes (p ≈ 0.002) |
 
In the bootstrap analysis, gate_30 had higher 7-day retention in about 99% of resamples.
 
## Approach
1. **Sanity checks:** Confirmed no duplicates or missing values, and ran a Sample Ratio Mismatch (SRM) check on the group split.
2. **Data cleaning:** Removed an extreme outlier in game rounds played.
3. **Hypothesis testing:** Used two-proportion z-tests for day-1 and day-7 retention.
4. **Bootstrap analysis:** Resampled the data 1,000 times to measure confidence in the retention difference.
5. **Engagement check:** Compared game rounds between groups with a Mann-Whitney U test, since the data is heavily skewed.

   
## A Note on Data Quality
 
The SRM check flagged a small imbalance between groups (p = 0.0086). The imbalance is under 2%, and the retention gap is consistent across both the z-test and the bootstrap, so the direction of the result is likely reliable. In a real setting, I would verify the assignment logic with engineering before shipping a decision.


## Limitations
 
- **Short time window:** Behavior is only tracked up to day 7.
- **Diluted effect:** Only players who reach level 30 experience the change, but the median player plays about 16 rounds, so the true effect on affected players is likely larger.
- **No revenue data:** Players can pay to skip gates, so purchase behavior could change in ways this data can't show.
  
## What I'd Test Next
 
**Dynamic gates:** serve an earlier break only to fast-progressing players, who are most at risk of burnout, while giving slower players a longer runway. Primary metric: 7-day retention. Guardrail: in-app purchase rate.
 
The full write-up, including my hypothesis for why an earlier gate helps retention, is in the [notebook](cookie_cats_ab_test.ipynb).

## Tools
 
Python (pandas, NumPy, SciPy, statsmodels, Matplotlib) in Google Colab
 
## Data
 
[Mobile Games A/B Testing (Cookie Cats) on Kaggle](https://www.kaggle.com/yufengsui/mobile-games-ab-testing): 90,189 players randomly assigned to a gate at level 30 or level 40.
 
---
 
**Chidi** · Product Analyst · [LinkedIn](https://www.linkedin.com/in/chidiebere-mmuomaife)
