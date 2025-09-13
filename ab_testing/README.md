# A/B Testing in Mobile Gaming: Progression Gate Optimization 

## Project Overview
This project evaluated the impact of shifting the **first progression gate** in a mobile puzzle game from **Level 30 → Level 40**.  
Using a randomized A/B test with 90,189 players, we measured engagement (rounds played) and retention (at Day 1 and Day 7).  

**Result:** Delaying the gate did **not improve engagement** and **reduced 7-day retention** (~5% relative decline).  
**Recommendation:** Keep the Level-30 gate.    

---

## 1. Background & Motivation  
Progression gates are common in free-to-play games. They:  
- Control the pace of gameplay,  
- Introduce friction that encourages in-app purchases,  
- Help sustain long-term player engagement.  

But if mistimed, they can backfire:  
- Too early: frustrates players before they’re invested.  
- Too late: weakens the sense of achievement and reduces habit-forming behavior.

The **business question** was simple:  
> *Would pushing the first gate later (to Level 40) keep players engaged longer, or would it hurt retention?*  

Testing ensured that the team didn’t rely solely on intuition to make a high-impact design change.  
 

---
## 2. Benchmark & Metrics  

### Evaluation Metrics  
- **Engagement:** total game rounds played in the first 14 days.  
- **Retention:**  
  - **Day-1 (D1)** – player returned the next day.  
  - **Day-7 (D7)** – player returned one week later.  

### Experiment Setup  
| **Group**   | **Gate Position** | **Players (N)** |  
|-------------|------------------|-----------------|  
| **Control** | Level 30         | 44,700          |  
| **Test**    | Level 40         | 45,489          |  

- **Design:** Randomized A/B split at install  
- **Sanity check:** Groups balanced at baseline (no significant differences)  
- **Decision rule:** Ship only if engagement increases and retention does not decrease.  

---

## 3. Randomization Check (AA Test)  

Before testing effects, simulate an AA-style test to validate the balance between groups.  

| **Metric**         | **Test**          | **p-value** | **Conclusion** |  
|---------------------|------------------|-------------|----------------|  
| Game Rounds         | Welch’s t-test   | 0.949       | ✅ Comparable |  
| Day-1 Retention     | Chi-square       | 0.075       | ✅ No diff |  
| Day-7 Retention     | Chi-square       | 0.064       | ✅ No diff |  

➡ Confirms groups are balanced → any difference is due to gate change.  

---
## 4. Results  

### Engagement (Rounds Played)  
| **Metric**       | **Gate 30** | **Gate 40** | **Difference** |  
|------------------|-------------|-------------|----------------|  
| Median Rounds    | ~17         | ~16         | Not significant |  

➡ No evidence that moving the gate later increased playtime.  

### Retention Rates  
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


### Insight  
While **Day-1 retention** remained nearly identical, **Day-7 retention dropped by 0.8 percentage points (~5% relative decline)** in the test group.  
This suggests that moving the first gate later (to Level 40) **weakened long-term player stickiness**, even though early engagement (rounds played) was unaffected.  

---
## 5. Insights & Recommendations  

### Key Findings  
- Delaying gate to Level 40 did ❌ not improve engagement.  
- Day-1 retention ~stable.  
- Day-7 retention ↓ 0.8pp (~5% relative).  
- 99.8% bootstrap certainty of worse retention.  

### Business Impact  
- Prevented a **harmful product change** before rollout.  
- Avoided revenue loss tied to **reduced long-term retention**.  
- Highlighted that **early friction supports habit formation**.  

### Final Recommendation  
🚫 Do not roll out Level-40 gate.  
✅ Keep Level-30 → better long-term stickiness and player value.  

---

## 📌 Key Takeaway  
This A/B test shows how **experimentation protects both players and business outcomes**.  
Instead of guessing, we measured:  
- Engagement → unchanged  
- Retention → harmed  
- Decision → **don’t launch**  

