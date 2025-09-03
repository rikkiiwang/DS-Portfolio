## 📌 Project Overview  
This project applied Fitbit Charge 3 wearable data to explore the relationship between heart rate patterns and self-reported stress in ICU medical residents. The study serves as a practical demonstration of how consumer-grade wearables can be leveraged for workplace stress monitoring in high-pressure industries such as healthcare.

- **Population:** 57 ICU residents at Los Angeles County + USC Medical Center  
- **Duration:** 3 weeks  
- **Device:** Fitbit Charge 3, proximity sensors, smartphone app for surveys
- **Data collected:** Continuous HR, steps, sleep metrics + daily stress surveys (midday and end-of-day)  

By combining wearable signals with self-reports of stress, the project investigated whether physiological data can serve as a reliable indicator of occupational stress. Findings from this analysis provide insights for telehealth, wellness apps, and occupational health programs looking to integrate wearables for scalable, real-time stress monitoring.

---

## 🧠 Background & Motivation  
Workplace stress is a critical issue across industries. In the ICU, physicians operate in a high-stakes, unpredictable environment where burnout rates are significantly higher than in the general population. Similar stress challenges exist in other demanding sectors such as finance, aviation, customer support, and logistics, where poor stress management can reduce performance and safety.

Wearables like Fitbit offer continuous, unobtrusive tracking of physiological data (HR, sleep, activity). If combined with contextual information (self-reports, surveys, environmental data), they can enable early stress detection and personalized interventions. This project used the TILES-2019 dataset as a real-world testbed. By analyzing ICU residents’ heart rate and stress levels during their shifts to better understand:

- How heart rate metrics reflect stress in natural work settings.
- How daily stressors (e.g., health concerns, family issues) interact with physiology.
- Why personalized models are necessary due to individual variability.

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
  <img src="Figures/raw_HR_example.png" alt="Raw heart rate data before surveys" width="350"/>
  <br>
  <em>Figure 1. Raw heart rate before midday (stress=3.0) surveys.</em>
</p>
<p align="center">
  <img src="Figures/Comparison of heart rate patterns.png" alt="Heart rate pattern" width="450"/>
  <br>
  <em>Figure 2. Comparison of heart rate patterns between midday and end-of-day surveys for one participant.</em>
</p>

---

## 🔬 Analysis & Methods  

### Stage 1: Correlation Analysis  
- Explored relationships between HR metrics (mean, min, median, SD, percentile-based medians, peak HR) and self-reported stress.  
- Found **weak but significant correlations** for lower HR percentiles with stress.  

Table 1. Correlation coefficients between heart rate metrics and stress levels
| Heart rate metrics                  | Coefficient | P value |
| ----------------------------------- | ----------- | ------- |
| Mean heart rate                     | 0.1401      | <.001   |
| Minimum heart rate                  | 0.1068      | <.001   |
| Normalized mean heart rate          | 0.0609      | .02     |
| Normalized median heart rate        | 0.0639      | .11     |
| Ratio of heart rate above 100 (bpm) | 0.0289      | .28     |
| Maximum heart rate                  | 0.0237      | .37     |
| SD of heart rate                    | -0.1036     | <.001   |

Table 2. Correlation between upper percentiles of median heart rate and stress levels
| Heart rate percentiles    | Correlation coefficient | P value |
| ------------------------- | ----------------------- | ------- |
| Top 5% median heart rate  | 0.0227                  | .39     |
| Top 10% median heart rate | 0.0248                  | .35     |
| Top 25% median heart rate | 0.0239                  | .37     |
| Top 50% median heart rate | 0.0297                  | .26     |

Table 3. Correlation between lower percentiles of median heart rate and stress levels
| Heart rate percentiles       | Correlation coefficient | P value |
| ---------------------------- | ----------------------- | ------- |
| Bottom 5% median heart rate  | 0.1066                  | <.001   |
| Bottom 10% median heart rate | 0.1083                  | <.001   |
| Bottom 25% median heart rate | 0.1046                  | <.001   |
| Bottom 50% median heart rate | 0.0967                  | .002    |


---

### Stage 2: Stressor Analysis  
- Compared stress differences between midday and evening surveys using paired t-tests.  
- Identified **daily stressors** (self-care, partner conflict, household tasks, health issues).  
- "Other health" stressors (someone else’s health) showed strongest positive correlation with stress change.  

Table 4. Correlations between daily stressors and stress level changes
| Daily stressor | Coefficient | P value |
| -------------- | ----------- | ------- |
| Partner        | -0.0756     | .0504   |
| Family         | 0.0996      | .009    |
| Breakdown      | -0.0741     | .055    |
| Money          | 0.0062      | .87     |
| Self-care      | -0.0330     | .39     |
| Health         | 0.0098      | .80     |
| Other health   | 0.1694      | <.001   |
| Household      | -0.0386     | .32     |
| Child          | N/A         | N/A     |
| Discrimination | 0.0391      | .31     |
| None           | -0.1085     | .005    |

---

### Stage 3: Mixed-Effects Modeling  
- Accounted for repeated measures with random intercepts per participant.  
- Found **significant association** between self-reported stress and HR (p=0.03).  
- Highlighted **inter-individual variability** in baseline HR and stress responses.  

Table 5. Results of the linear mixed model on the influence of stress level and survey type on mean heart rate (part 1)
| Model                  | Dependent variable | Mean heart rate |
| ---------------------- | ------------------ | --------------- |
| Number of observations | Method             | REML            |
| Number of groups       | Scale              | 61.99           |
| Minimum group size     | Log-likelihood     | –5023.08        |
| Maximum group size     | Converged          | Yes             |
| Mean group size        | N/A                | —               |

Table 6. Results of the linear mixed model on the influence of stress level and survey type on mean heart rate (part 2)
| Variable                                   | Coefficient | SE   | z     | P value | \[0.025, 0.975] |
| ------------------------------------------ | ----------- | ---- | ----- | ------- | --------------- |
| Intercept                                  | 80.75       | 1.20 | 67.09 | <.001   | 78.41 – 83.13   |
| Survey\_Type \[end-of-day]                 | -0.64       | 0.47 | -1.36 | .17     | -1.57 – 0.28    |
| Stress\_Level                              | 0.33        | 0.15 | 1.28  | .03     | 0.03 – 0.63     |
| Group heart rate variance                  | 56.31       | 1.58 | —     | —       | —               |
| Group heart rate × Survey\_Type covariance | -0.35       | 0.47 | —     | —       | —               |
| Survey\_Type \[end-of-day] variance        | 1.67        | 0.26 | —     | —       | —               |

<p align="center">
  <img src="Figures/random_intercepts_hist.png" alt="Random intercepts distribution" width="400"/>
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
- For healthcare: Wearables can support stress monitoring in clinical staff, but should be combined with surveys or multimodal biomarkers (e.g., HRV, cortisol).
- For wellness apps: HR patterns can offer insight into stress but require personalization to avoid misleading results.
- For future research: Combining wearables with machine learning offers promise for scalable, real-world stress monitoring systems.


