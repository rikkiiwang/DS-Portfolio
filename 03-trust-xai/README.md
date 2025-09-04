# 🤝 Trust, Explainability & Telehealth Adoption  

This folder highlights two projects that connect advanced analytics with real-world human impact. Both demonstrate how data science and system design can directly shape the future of digital health, making healthcare more accessible, transparent, and trustworthy.

---

## 📌 Project 1: Telehealth Adoption Among the Aging Population  

### 🏥 Why It Matters 
Telehealth promises fewer hospital visits, lower costs, and better access — but older adults, who need it most, are often reluctant to adopt it. For companies in digital health, insurance, or healthcare delivery, the key question is: what really drives or blocks adoption among seniors?

Traditional industry reports often stop at describing “who adopts” (e.g., younger, urban, higher-income users). But that doesn’t explain why. Without those deeper drivers, companies risk designing solutions that fail to scale among older adults — the very group driving healthcare demand growth.

### 🔬 Methods 
- Dataset:
    - Conducted a secondary analysis using the **Health Information National Trends Survey (HINTS 6, 2022)**.  
    - The full dataset included 6,252 adults; filtered for respondents aged ≥65, resulting in 1,596 complete cases.  

- Analytical Approach:
    - Ordinal regression to test associations between telehealth adoption and predictors (wearable use, demographics, health).
    - Mediation analysis to reveal indirect effects of education, income, and health status on adoption pathways.
    - Conceptual model built to visualize direct and mediated effects.

### 📈 Results  
Table 1. Results from ordinal logistic regression that assesses the association between wearable device use, demographics, and telehealth adoption
among older US adults (N=1596)

| Factor | Odds Ratio (95% CI) | p-value | Insight |
|--------|----------------------|---------|---------|
| Wearable use | 1.32 (1.03–1.68) | .03 | Wearable users 32% more likely to adopt telehealth |
| Education | 1.37 (1.21–1.56) | <.001 | Higher education strongly increased adoption |
| Income | 1.15 (0.99–1.32) | .07 | Marginal positive effect |
| Rural location | 0.78 (0.66–0.92) | .003 | Rural residents adopted less (digital divide) |
| Health condition | 0.77 (0.68–0.86) | <.001 | Healthier seniors less likely to adopt |
| Gender | 1.04 (0.84–1.29) | .74 | No effect |
| Physical activity | 0.96 (0.92–1.01) | .11 | No effect |

Table 2. Results of the regression analysis that estimate the direct and indirect effects of the wearable device
| Regression Path | Estimate | SE | z-value | p-value | Interpretation |
|-----------------|----------|----|---------|---------|----------------|
| Telehealth adoption ~ Wearable use (b1)| 0.137 | 0.077 | 1.79 | .07 | Wearable use had a positive but marginal direct effect |
| Education ~ Wearable use (a1)| 0.339 | 0.054 | 6.29 | <.001 | Wearable users tend to have higher education |
| Telehealth adoption ~ Education (c1)| 0.162 | 0.034 | 4.77 | <.001 | Higher education strongly boosts adoption |
| Income ~ Wearable use (a2)| 0.347 | 0.047 | 7.38 | <.001 | Wearable users more likely to be higher-income |
| Telehealth adoption ~ Income (c2)| 0.114 | 0.039 | 2.89 | .004 | Higher income increases adoption |
| Health condition ~ Wearable use (a3)| 0.262 | 0.059 | 4.43 | <.001 | Wearable users more likely to report better health |
| Telehealth adoption ~ Health condition (c3)| –0.125 | 0.032 | –3.87 | <.001 | Healthier seniors adopt less (perceived lower need) |

<p align="center">
  <img src="figures/Conceptual model.png" alt="Conceptual model" width="450"/>
  <br>
  <em>Figure 1. Conceptual model illustrating the relationship between the use of wearable devices and telehealth adoption.</em>
</p>

Table 3. Total and indirect effects in the conceptual mediation model.
| Pathway | Indirect Effect | p-value | Interpretation |
|---------|-----------------|---------|----------------|
| Wearable → Education → Telehealth | +0.055 | <.001 | Wearables increase education-linked adoption |
| Wearable → Income → Telehealth | +0.040 | .007 | Wearables associated with higher-income adopters |
| Wearable → Health → Telehealth | –0.033 | .004 | Better health reduces need for telehealth |
| **Total Effect** | **+0.199** | .007 | Wearables increase adoption overall |


### 💡 Key Insights 
- Wearable device use increased the likelihood of telehealth adoption (OR=1.32, *p*=.03).  
- Education and income were powerful mediators → higher education/income → more likely to adopt.
- Rural residents showed significantly lower adoption, reflecting digital divide challenges. (*p*=.003).  
- Healthier individuals were less likely to adopt → less perceived need for telehealth.

### 💼 Takeaway 
- Wearables are not just health gadgets; they are a bridge to telehealth adoption. However, whether seniors cross that bridge depends on education, income, and perceptions of health.  
- To expand telehealth in aging populations, companies must combine device promotion with targeted digital literacy and inclusion strategies.

---

## 📌 Project 2: Trust & Explainability in Wearable Health Monitoring  

### 🧑‍⚕️ Why It Matters  
AI-powered wearables (Apple Watch, Fitbit, Garmin) can provide health recommendations in real-time. But trust and usability determine whether people actually follow those recommendations. For the healthcare industry, trust isn’t a “nice-to-have” — it directly affects adoption, adherence, and market success. Companies often focus on improving algorithms, but without clear visualization and explainable AI (XAI), users may hesitate to act. This project bridges that gap by testing how usability, visualization design, contextual info, and AI transparency shape user trust.

### 🔬 Methods
- **Participants:** 25 Apple Watch users (ages 20–32), recruited from a prior Apple Watch anomaly detection study.  
- **Designed and conducted a Survey:** 26 questions (Qualtrics)  
  - **System Usability Scale (SUS):** Standard 10-item usability instrument (0–100 score).  
  - **Likert scales (1–5):** Ratings of trust, clarity, satisfaction.  
- **Scenarios Tested:**  
  1. **Usability:** General ease-of-use (SUS).  
  2. **Visualization:** Apple Watch interface vs. generated detailed summary.  
  3. **Contextual Information:** Raw heart rate vs. heart rate + environment information (humidity, activity).  
  4. **Explainable AI (XAI):** Recommendations with vs. without textual explanations.

### 📊 Results  
#### 1. Usability (SUS)  
- Average SUS = 78.4 (“Good” usability) 
- Range = 47.5–100 → most users rated highly, a few reported issues (battery, comfort, data tracking).
  <p align="center">
  <img src="figures/Distribution of SUS Scores.png" alt="Distribution of SUS Scores" width="450"/>
  <br>
  <em>Figure 2. Distribution of SUS Scores.</em>
</p>
  
### 2. Visualization Preferences  
- Generated visual summary rated higher than Apple Watch UI.  
  - Summary = 4.2/5 vs. Apple Watch = 3.8/5 (*t=2.30, p=0.029*).  
- Preference breakdown: 52% preferred summary, 28% preferred watch, 20% neutral.

Table 4. Group Preference
| Group | Understanding | Accuracy | Detail |
|-------|---------------|----------|--------|
| Apple Watch | 4.00 | 3.00 | 4.00 |
| Generated Summary | 4.57 | 2.71 | 4.29 |
| Neutral | 4.29 | 2.29 | 3.94 |  


#### 3. Contextual Information  
- Adding environmental data (humidity, activity) **did not significantly increase trust**.  
  - Paired t-test: t=0.0, p=1.0.  
  - Correlation: overall satisfaction correlated with contextualized trust (r=0.516).

Table 5. Correlation Analysis Result
| Variable                 | Baseline Score | Contextualized Score |
|---------------------------|----------------|-----------------------|
| overall_satisfaction      | 0.306          | 0.516                 |
| SUS_score                 | 0.304          | 0.383                 |
| understanding_confidence  | 0.285          | 0.446                 |
| accuracy_perception       | -0.303         | -0.167                |
| level_of_detail           | 0.076          | 0.470                 |


#### 4. Explainable AI (XAI)  
- Baseline trust = 1.97 (p=0.013) → users trusted AI even without explanations.
- AI explanations slightly improved trust (+0.14 units), but not statistically strong (*p=0.077*).  
- User satisfaction and usability (SUS score) showed positive but non-significant effects.

Table 6. Mixed Linear Model Regression Results
<p align="left">
<img width="600" height="350" alt="image" src="https://github.com/user-attachments/assets/80095968-e05f-4854-8a3b-0ad976b2f063" />
</p>

### 📈 Key Insights 
- Usability baseline: SUS score of 78.4 (Good usability), showing users found Apple Watch generally reliable.
- Visualization matters: Users preferred detailed visual summaries over small watch-screen displays — clarity builds trust.
- Contextual info ≠ game-changer: Adding factors like humidity did not significantly boost trust — users prioritize data clarity over complexity.
- Explainability helps (a little): AI recommendations with explanations slightly increased trust and adherence, though the effect wasn’t statistically strong.

---





