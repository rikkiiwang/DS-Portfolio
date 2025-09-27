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

Then the question "Would delaying the first progression gate from Level 30 to Level 40 improve early-game experience and retention without significantly hurting monetization?" comes. This A/B testing task was designed to validate whether shifting the gate later in the progression curve could improve user experience.
- Concern: Level 30 gate may be too early, frustrating players before they are invested.
- Hypothesis: Moving the gate to Level 40 would increase early engagement & retention, and in turn influence monetization.


### 1.2 About Cookie Cats:
<p align="center">
<img width="600" height="300" alt="image" src="https://github.com/user-attachments/assets/10de5afd-8023-4631-862b-cb31c64ec61e" />
</p>

- Genre: Casual match-3 puzzle.
- Core loop: Players clear levels by matching tiles.
- Monetization: Free-to-play (IAPs for boosters, skips, extra lives).
  

### 1.3 Why A/B Testing in Gaming?
In free-to-play mobile gaming, even small design changes can have a major impact on player behavior and business performance. A/B testing provides a reliable way to validate these changes with data rather than assumptions.
- Retention: whether players keep playing after install.
  For example, a casual puzzle studio noticed that Day-7 retention was only 13%, which is below the 15–20% industry benchmark. By running an A/B test that introduced a daily login reward at different progression points, the team measured whether players who received early incentives were more likely to return. The test confirmed that retention improved when rewards were granted within the first three sessions.
  
- Engagement: how many rounds they complete.
  For example, an RPG mobile game observed that average sessions per day dropped from 4 to 2.5 after players reached Level 10. Designers suspected that the difficulty curve was too steep. An A/B test compared the standard curve against a smoother progression curve. Results showed that the test group played ~30% more rounds per day, validating the design adjustment.
  
- Revenue: as retention drives lifetime value (LTV) and IAP conversion.
  In a match-3 game, most purchases occurred when players hit a paywall at Level 25, but only 2% of players converted while many quit entirely. To test whether shifting monetization later could improve long-term LTV, the studio ran an A/B test delaying the paywall to Level 35. While short-term revenue dipped slightly, the variant achieved a higher 30-day ARPU because more players stayed engaged long enough to spend later.

## 2. Challenges
Below we describe the key challenges faced in this project and how they were addressed.
### Challenge 1: Defining the Right Metrics
One of the first difficulties was aligning on which KPIs truly reflected business success. Different stakeholders had different priorities—marketing focused on user growth, product managers emphasized engagement, and finance was concerned with revenue. To create alignment, I collaborated across functions and mapped business goals to measurable metrics. I categorized the metrics into three aspects, such as user-related, engagement-related and monetization-related metrics
Table 1. Mapping Business Goals to Metrics
| **Business Goal**              | **Metric Translation**                  |
| ------------------------------ | --------------------------------------- |
| Improve player stickiness      | Retention rates (Day-1, Day-7)          |
| Boost engagement               | Number of rounds played                 |
| Protect monetization potential | Active retained players × average rounds played | 

### Challenge2: Accessing and Preparing Data
The dataset required for analysis was sourced from the data engineering (DE) team. It contained player-level records, with the following key fields:
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
| **Dimension**   | **Strata**                           |
| --------------- | ------------------------------------ |
| Age             | <18, 18–34, 35–50, >50               |
| Gender          | Male, Female                         |
| User Cohort     | New users (<7 days), Returning users |
| Device Activity | Active devices, Inactive devices     |
This ensured balanced distribution across A/B groups and reduced confounding effects.

However, even with stratification, we had to be cautious of Simpson’s Paradox—a phenomenon where trends observed in aggregated data disappear or even reverse when the data is broken down into subgroups. For example, if new players in Group B showed higher retention but older players in Group A were far more numerous, the aggregate results might incorrectly suggest that Group A performed better. To avoid this, we monitored both overall KPIs and subgroup KPIs, ensuring that improvements were consistent across segments.

### Challenge 4: Statistical Test Selection
Different KPIs required different statistical methods:
- Engagement (continuous metric: game rounds):
  - Null hypothesis (H₀): There is no difference in mean game rounds between gate_30 and gate_40.
  - Alternative hypothesis (H₁): Mean game rounds differ between the two groups.
  - With a sufficiently large sample, we applied a z-test (sample variance approximates population variance).

- Retention (binary metric: return vs. not return):
  - Null hypothesis (H₀): Retention rate is the same across both groups.
  - Alternative hypothesis (H₁): Retention rate differs between groups.
  - Here too, a z-test for proportions was used given the large sample size.

- Why z-test instead of t-test?
-   For smaller samples, a t-test would be appropriate. But with >90k players, the Central Limit Theorem ensures normal approximation, allowing us to use z-tests to reduce computational complexity and cost.

### Challenge 5: Experiment Stability Verification
To ensure stability of results over time, we applied a bootstrap method. Bootstrapping allowed us to:
Re-sample player-level data thousands of times.
Estimate confidence intervals for retention and engagement metrics.
Verify that results were consistent and not overly dependent on a single random split.


         |

---
## 2. Metrics - >metric challenge

### Evaluation Metrics  
- **Engagement:** total game rounds played in the first week.  
- **Retention:**  
  - **Day-1 (D1)** – player returned the next day.  
  - **Day-7 (D7)** – player returned one week later.  


### Experiment Setup  
Table 2. Experimental Design
| **Group**   | **Gate Position** | **Players (N)** |  
|-------------|------------------|-----------------|  
| **Control** | Level 30         | 44,700          |  
| **Test**    | Level 40         | 45,489          |  

- **Design:** Randomized A/B split at install  
- **Sanity check:** Groups balanced at baseline (no significant differences)  
- **Decision rule:** Ship only if engagement increases and retention does not decrease.  

---
## 3. Conclusion
### 📌 Key Takeaway  
This case demonstrates how A/B testing translates abstract business goals (player stickiness, monetization) into measurable metrics. In mobile gaming, where design tweaks can make or break revenue, rigorous testing ensures evidence-based decision-making.

### Engagement (Rounds Played)  
Table 3. Engagement Metrics
| **Metric**       | **Gate 30** | **Gate 40** | **Difference** |  
|------------------|-------------|-------------|----------------|  
| Median Rounds    | ~17         | ~16         | Not significant |  

➡ No evidence that moving the gate later increased playtime.  

### Retention Rates  
Table 4. Retention Metrics
| **Version** | **Users Count** | **Day-1 Retention Rate** | **Day-7 Retention Rate** | **Total Game Rounds** |  
|-------------|-----------------|--------------------------|--------------------------|-----------------------|  
| **gate_30** (Control) | 44,700 | **0.4482** | **0.1902** | 2,344,795 |  
| **gate_40** (Test)    | 45,489 | 0.4423 | **0.1820** | 2,333,530 |  

- **Day-1 Retention:** ~44.8% for `gate_30` vs. 44.2% for `gate_40` → marginal difference.  
- **Day-7 Retention:** 19.0% for `gate_30` vs. 18.2% for `gate_40` → visible drop in the test group.  
- A combined metric (retained on both Day 1 & 7) also showed a **subtle decrease** in the Level-40 group.

  <img width="800" height="320" alt="image" src="https://github.com/user-attachments/assets/14f4189a-f9eb-45ee-985a-1b22d5a21c9c" />
  <br>  
  <em>Figure 1: Comparison of Day-1 and Day-7 retention between groups</em>  

While **Day-1 retention** remained nearly identical, **Day-7 retention dropped by 0.8 percentage points (~5% relative decline)** in the test group.  
This suggests that moving the first gate later (to Level 40) **weakened long-term player stickiness**, even though early engagement (rounds played) was unaffected.  


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

## Further Step



