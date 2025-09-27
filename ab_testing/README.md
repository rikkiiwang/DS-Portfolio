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

### 1.2 About Cookie Cats:
- Genre: Casual match-3 puzzle.
- Core loop: Players clear levels by matching tiles.
- Monetization: Free-to-play (IAPs for boosters, skips, extra lives).
  

### 1.3 Why A/B Testing in Gaming?
In the mobile gaming industry, player experience and monetization are tightly coupled. Even small design changes (like gate placement) can shift:
- Retention – whether players keep playing after install.
- Engagement – how many rounds they complete.
- Revenue – as retention drives lifetime value (LTV) and IAP conversion.

Relying on intuition is risky. A/B testing provides a scientific way to validate changes before global rollout.

## 2. Challenges
### Challenge 1: hard to define metrics
collaborate with marketing.... we define the metrics from three aspects, such as user-related, monetization-related metrics
Table 1. Mapping Business Goals to Metrics
| **Business Goal**              | **Metric Translation**                  |
| ------------------------------ | --------------------------------------- |
| Improve player stickiness      | Retention rates (Day-1, Day-7)          |
| Boost engagement               | Number of rounds played                 |
| Protect monetization potential | Active retained players × average rounds played | 

challenge: get data from DE
The dataset contains player-level records including:
- userid → unique player ID
- version → experimental condition (gate_30 or gate_40)
- sum_gamerounds → number of rounds played in the first week
- retention_1 → whether the player returned on Day-1
- retention_7 → whether the player returned on Day-7
<img width="519" height="198" alt="image" src="https://github.com/user-attachments/assets/676cd7a5-3538-41ab-af31-597893bb319a" />

Figure 1: Example of the dataset structure

### Challenge 2: sample圈选和分流方案设计
simpson:
table( age gender old/new users(< 7 days) active/inactive devices) stratefied sampling
### Challenge 3: 不同类型指标的检验方案选取
game rounds vs. retention 
H0
H1
Z-test vs. t-test Why?
在样本量足够的情况下，样本方差替代总体方差，为了减小实验难度和成本，用了z-test
z-test在两种指标上的区别
### challenge4: 实验稳定性验证--bootstrap
plot

-》background
- Concern: Level 30 gate may be too early, frustrating players before they are invested.
- Hypothesis: Moving the gate to Level 40 would increase early engagement & retention, and in turn influence monetization.

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



