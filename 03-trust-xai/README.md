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


Table 2. Total and indirect effects in the conceptual mediation model.
| Pathway | Indirect Effect | p-value | Interpretation |
|---------|-----------------|---------|----------------|
| Wearable → Education → Telehealth | +0.055 | <.001 | Wearables increase education-linked adoption |
| Wearable → Income → Telehealth | +0.040 | .007 | Wearables associated with higher-income adopters |
| Wearable → Health → Telehealth | –0.033 | .004 | Better health reduces need for telehealth |
| **Total Effect** | **+0.199** | .007 | Wearables increase adoption overall |


<p align="center">
  <img src="Figures/Conceptual model.png" alt="Conceptual model" width="350"/>
  <br>
  <em>Figure 1. Conceptual model illustrating the relationship between the use of wearable devices and telehealth adoption.</em>
</p>

### 💡 Key Insights 
- Wearable device use increased the likelihood of telehealth adoption (OR=1.32, *p*=.03).  
- Education and income were powerful mediators → higher education/income → more likely to adopt.
- Rural residents showed significantly lower adoption, reflecting digital divide challenges. (*p*=.003).  
- Healthier individuals were less likely to adopt → less perceived need for telehealth.

### 💼 Takeaway 
Wearables are not just health gadgets; they are a bridge to telehealth adoption. However, whether seniors cross that bridge depends on education, income, and perceptions of health.  
To expand telehealth in aging populations, companies must combine device promotion with targeted digital literacy and inclusion strategies.

---

## 📌 Project 2: Trust & Explainability in Wearable Health Monitoring  

### 🧑‍⚕️ Why It Matters  
AI-powered wearables (e.g., Apple Watch, Fitbit) can provide health recommendations in real-time. But will people trust them enough to act? Trust and usability aren’t just “nice-to-haves” — they’re deal-breakers for adoption in healthcare.

### 🔬 What I Did
- Participants: 25 users (aged 20–32) recruited from a prior Apple Watch data integrity study.
- Survey Design: 26 questions, mixing System Usability Scale (SUS) with Likert ratings and controlled scenarios.
- Focus Areas Tested:
    - Usability (SUS scoring)
    - Data visualization (Apple Watch UI vs. detailed summary)
    - Contextual info (raw heart rate vs. heart rate + environment)
    - Explainable AI (XAI) (recommendations with vs. without explanation)

### 📈 Key Insights 
- Usability baseline: SUS score of 78.4 (Good usability), showing users found Apple Watch generally reliable.
- Visualization matters: Users preferred detailed visual summaries over small watch-screen displays — clarity builds trust.
- Contextual info ≠ game-changer: Adding factors like humidity did not significantly boost trust — users prioritize data clarity over complexity.
- Explainability helps (a little): AI recommendations with explanations slightly increased trust and adherence, though the effect wasn’t statistically strong.

---





