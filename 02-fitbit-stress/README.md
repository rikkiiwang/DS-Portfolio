## 📌 Project Overview  
This project investigates the relationship between **heart rate data collected via Fitbit Charge 3** and **self-reported stress levels** among intensive care unit (ICU) residents. The study aimed to assess whether wearable data could provide reliable insights into stress in high-pressure clinical environments.  

- **Population:** 57 ICU residents  
- **Duration:** 3 weeks  
- **Device:** Fitbit Charge 3  
- **Data collected:** Continuous HR, steps, sleep metrics + daily stress surveys (midday and end-of-day)  
- **Methods:** Spearman correlation, paired t-tests, point-biserial correlation, and mixed-effects models
  
---

## 🧠 Background & Motivation  
ICU residents face high levels of occupational stress and burnout, which negatively impacts well-being and patient care. While **wearables** have potential for continuous stress monitoring, their reliability in **real-world clinical environments** remains underexplored.  

This study contributes by analyzing long-term HR data in naturalistic ICU settings and aligning it with **self-reported stress levels and stressors**.

---

## 📊 Dataset & Preprocessing  
- **Source dataset:** [TILES-2019 dataset](https://doi.org/10.1038/s41597-022-01636-4)  
- **Preprocessing steps:**  
  - Removed empty files & duplicates  
  - Min-max normalization of HR  
  - Aligned HR with survey timestamps  
  - Extracted 4-hour HR segments prior to each survey (midday & evening)  

📈 Example visualization:  

<p align="center">
  <img src="Figures/raw_HR_example.png" alt="Raw heart rate data before surveys" width="600"/>
  <br>
  <em>Raw heart rate before midday (stress=3.0) and evening (stress=2.0) surveys.</em>
</p>

---

## 🔬 Analysis & Methods  

### Stage 1: Correlation Analysis  
- Explored relationships between HR metrics (mean, min, median, SD, percentile-based medians, peak HR) and self-reported stress.  
- Found **weak but significant correlations** for lower HR percentiles with stress.  

<p align="center">
  <img src="Figures/correlation_plot.png" alt="Correlation analysis" width="500"/>
</p>

---

### Stage 2: Stressor Analysis  
- Compared stress differences between midday and evening surveys using paired t-tests.  
- Identified **daily stressors** (self-care, partner conflict, household tasks, health issues).  
- "Other health" stressors (someone else’s health) showed strongest positive correlation with stress change.  

<p align="center">
  <img src="Figures/stressor_correlation.png" alt="Stressor correlations with stress change" width="500"/>
</p>

---

### Stage 3: Mixed-Effects Modeling  
- Accounted for repeated measures with random intercepts per participant.  
- Found **significant association** between self-reported stress and HR (p=0.03).  
- Highlighted **inter-individual variability** in baseline HR and stress responses.  

<p align="center">
  <img src="Figures/random_intercepts_hist.png" alt="Random intercepts distribution" width="500"/>
  <br>
  <em>Distribution of random intercepts showing variability in baseline HR.</em>
</p>

---

## ✅ Key Findings  
- HR alone is a **modest indicator of stress** in ICU environments.  
- Lower HR percentiles capture stress correlations better than upper percentiles.  
- Daily stressors, particularly health-related ones, influenced stress more than HR.  
- Mixed-effects modeling demonstrated **significant but individualized associations** between stress and HR.  

---

## 📌 Takeaways  
- **Wearables show promise** but should be combined with self-reports and other physiological markers (e.g., HRV, cortisol).  
- **Personalized modeling** is critical due to high inter-individual variability.  
- This study advances the case for **multimodal stress monitoring systems** in healthcare professionals.  

---
