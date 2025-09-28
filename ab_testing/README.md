# 🎮 A/B Testing in Mobile Gaming: Progression Gate Optimization 

<p align="center">
  <img width="664" height="302" alt="image" src="https://github.com/user-attachments/assets/79ddf2b9-e9e5-44b7-9f51-8a4d0fc27a0c" />
</p> 

## 1. Background  
### 1.1 Motivation
In the mobile gaming industry, progression pacing is one of the most critical design levers. If progression feels too restrictive, players churn before they form a habit; if it feels too loose, monetization opportunities diminish.

For Cookie Cats, the product team observed that:
1. Player churn spiked at Level 30, coinciding with the first progression gate.
2. Level completion rates and Day-7 retention were trending below benchmarks, particularly in North America and Europe.
3. While some users monetized at the gate through in-app purchases (IAPs), a larger portion simply abandoned the game, leading to limited lifetime value (LTV) growth.

Business Question:
Would delaying the first progression gate from Level 30 to Level 40 improve early-game retention and engagement without significantly hurting monetization?
- Concern: Level 30 gate may be too early, frustrating players before they are invested.
- Hypothesis: Moving the gate to Level 40 would increase early retention & engagement, and improve long-term monetization.

### 1.2 About Cookie Cats:
<p align="center">
<img width="600" height="300" alt="image" src="https://github.com/user-attachments/assets/10de5afd-8023-4631-862b-cb31c64ec61e" />
</p>

- Genre: Casual match-3 puzzle.
- Core loop: Players clear levels by matching tiles.
- Monetization: Free-to-play (IAPs for boosters, skips, extra lives).
  

### 1.3 Why A/B Testing in Gaming?
In free-to-play mobile gaming, even small design changes can have a major impact on player behavior and business performance. A/B testing provides a reliable way to validate these changes with data rather than assumptions.
- Retention: Determines whether players keep coming back.
  Example: A studio improved Day-7 retention from 13% → 16% by testing different timing for daily login rewards.
  
- Engagement: Captures how actively players interact with the game.
  Example: An RPG increased average sessions per day by 30% after testing a smoother difficulty curve.

- Revenue: Retention and engagement directly drive monetization.
  Example: A match-3 title delayed its first paywall by 10 levels; while short-term IAP dipped, 30-day ARPU increased because more players stayed long enough to spend.

---
## 2. Challenges
Below is the key challenges faced in this project and how they were addressed.
### Challenge 1: Defining the Right Metrics
One of the first difficulties was aligning on which KPIs truly reflected business success. Different stakeholders had different priorities—marketing focused on user growth, product managers emphasized engagement, and finance was concerned with revenue. To create alignment, I collaborated across functions and mapped business goals to measurable metrics. I categorized the metrics into three aspects, such as user-related, engagement-related and monetization-related metrics
Table 1. Mapping Business Goals to Metrics
| **Business Goal**              | **Metric Translation**                  |
| ------------------------------ | --------------------------------------- |
| Improve player stickiness      | Retention rates (Day-1, Day-7)          |
| Boost engagement               | Number of rounds played                 |
| Protect monetization potential | Active retained players × average rounds played | 

- **Engagement:** total game rounds played in the first week.  
- **Retention:**  
  - **Day-1 (D1)** – player returned the next day.  
  - **Day-7 (D7)** – player returned one week later.
  

### Challenge 2: Accessing and Preparing Data
The dataset required for analysis was sourced from the data engineering (DE) team. It contained player-level records, with the following key fields:

Table 1. Dataset Structure
| **Field**        | **Description**                                    |
| ---------------- | -------------------------------------------------- |
| `userid`         | Unique player ID                                   |
| `version`        | Experimental condition (`gate_30` or `gate_40`)    |
| `sum_gamerounds` | Number of rounds played in the first week          |
| `retention_1`    | Whether the player returned on Day-1 (binary flag) |
| `retention_7`    | Whether the player returned on Day-7 (binary flag) |



### Challenge 3: Sample Selection and Group Allocation
Another challenge was ensuring that test and control groups were balanced. Without proper randomization, biases (e.g., age, device type, or prior experience) could confound results.
To address this, we designed a stratified sampling approach (see Table 2), segmenting by demographics and device usage:

Table 2. Stratification Dimensions
| **Dimension**   | **Strata**                           |
| --------------- | ------------------------------------ |
| Age             | <18, 18–34, 35–50, >50               |
| Gender          | Male, Female                         |
| User Cohort     | New users (<7 days), Returning users |
| Device Activity | Active devices, Inactive devices     |

We also accounted for Simpson’s Paradox, where aggregate trends can reverse when analyzed by subgroups. For example, if new players retained better in Gate-40 but older players dominated Gate-30, aggregate data might falsely suggest Gate-30 was superior. To mitigate this, we monitored both overall and subgroup KPIs.

Table 3. Experimental Design
| **Group**   | **Gate Position** | **Players (N)** |  
|-------------|------------------|-----------------|  
| **Control** | Level 30         | 44,700          |  
| **Test**    | Level 40         | 45,489          |  


### Challenge 4: Statistical Test Selection
Different KPIs required different statistical methods:
- Engagement (continuous metric: game rounds):
  - Null hypothesis (H₀): There is no difference in mean game rounds between gate_30 and gate_40.
  - Alternative hypothesis (H₁): Mean game rounds differ between the two groups.
  - With a sufficiently large sample, we applied a z-test (sample variance approximates population variance).

- Retention (binary metric: return vs. not return):
  - Null hypothesis (H₀): Retention rate is the same across both groups.
  - Alternative hypothesis (H₁): Retention rate differs between groups.
  - A z-test for proportions was used given the large sample size.

- Why z-test instead of t-test?
  - For smaller samples, a t-test would be appropriate. But with >90k players, the Central Limit Theorem ensures normal approximation, allowing us to use z-tests to reduce computational complexity and cost.

### Challenge 5: Experiment Stability Verification
To ensure stability of results over time, we applied a bootstrap method. Bootstrapping allowed us to:
- Re-sample player-level data thousands of times.
- Estimate confidence intervals for retention and engagement metrics.
- Verify that results were consistent and not overly dependent on a single random split.

---

## 3. Experiment Setup
Table 4. Experimental Design
| **Group**   | **Gate Position** | **Players (N)** |
| ----------- | ----------------- | --------------- |
| **Control** | Level 30          | 44,700          |
| **Test**    | Level 40          | 45,489          |

- Design: Randomized split at install.
- Sanity check: Groups balanced at baseline.
- Decision rule: Ship only if engagement increases and retention does not decrease.

---
## 4. Results & Conclusion 
This case demonstrates how A/B testing translates abstract business goals (player stickiness, monetization) into measurable metrics. In mobile gaming, where design tweaks can make or break revenue, rigorous testing ensures evidence-based decision-making.

### Engagement (Rounds Played)  
Table 4. Engagement Metrics
| **Metric**       | **Gate 30** | **Gate 40** | **Difference** |  
|------------------|-------------|-------------|----------------|  
| Median Rounds    | ~17         | ~16         | Not significant |  

➡ No evidence that moving the gate later increased playtime.  

### Retention Rates  
Table 5. Retention Metrics
| **Version** | **Users Count** | **Day-1 Retention Rate** | **Day-7 Retention Rate** | **Total Game Rounds** |  
|-------------|-----------------|--------------------------|--------------------------|-----------------------|  
| **gate_30** (Control) | 44,700 | **0.4482** | **0.1902** | 2,344,795 |  
| **gate_40** (Test)    | 45,489 | 0.4423 | **0.1820** | 2,333,530 |  

- **Day-1 Retention:** ~44.8% for `gate_30` vs. 44.2% for `gate_40` → marginal difference.  
- **Day-7 Retention:** 19.0% for `gate_30` vs. 18.2% for `gate_40` → visible drop in the test group.  
- A combined metric (retained on both Day 1 & 7) also showed a **subtle decrease** in the Level-40 group.

<p align="center">
<img width="800" height="320" alt="image" src="https://github.com/user-attachments/assets/14f4189a-f9eb-45ee-985a-1b22d5a21c9c" />
</p> <p align="center"><b>Figure 1.</b> Comparison of Day-1 and Day-7 retention between groups</p>
 

While **Day-1 retention** remained nearly identical, **Day-7 retention dropped by 0.8 percentage points (~5% relative decline)** in the test group. This suggests that moving the first gate later (to Level 40) **weakened long-term player stickiness**, even though early engagement (rounds played) was unaffected.  

### Bootstrap Verification
To validate the robustness of these findings, I performed a bootstrap analysis (500 resamples).
<p align="center">
<img width="592" height="276" alt="image" src="https://github.com/user-attachments/assets/7dcb5d4d-2df3-4dc4-8518-7cd24da304c7" />
</p> <p align="center"><b>Figure 2.</b> Bootstrap distributions for Day-1 (left) and Day-7 (right) retention across experimental groups.</p>

As shown above, the bootstrap distributions for Version A (gate_30) and Version B (gate_40) are clearly separated, confirming that differences in both Day-1 and Day-7 retention were statistically robust.

<p align="center">
<img width="1087" height="448" alt="image" src="https://github.com/user-attachments/assets/475e12f9-65cb-4c4a-b0c4-f8f878d7c755" />
</p> <p align="center"><b>Figure 3.</b> Bootstrapped differences in retention rates (Day-1 left, Day-7 right). Red dashed lines show 95% confidence intervals.</p>

Day-1 Retention: 95% CI = [-0.002, 0.012] → not significant (includes 0).
Day-7 Retention: 95% CI = [0.008, 0.022] → significant drop in Gate-40 group.

### Key Findings  
- Delaying gate to Level 40 ❌ did not improve engagement.  
- Day-1 retention ~stable.  
- Day-7 retention drop significantly.  
- 99.8% bootstrap certainty of worse retention.  

### Business Impact  
- Prevented a **harmful product change** before rollout.  
- Avoided revenue loss tied to **reduced long-term retention**.  
- Highlighted that **early friction supports habit formation**.  

### Final Recommendation  
🚫 Do not roll out Level-40 gate.  
✅ Keep Level-30 → better long-term stickiness and player value.  

## 5. Next Steps
- Subgroup deep dive: Explore retention by new vs. experienced users.
- Monetization analysis: Examine IAP behavior around Gate-30 to refine balancing.
- Future experiments: Test softer alternatives (e.g., reduced wait time, optional ads) to optimize both retention and revenue.



