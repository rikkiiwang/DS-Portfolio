# Ruijing's Data Science & Research Portfolio
This portfolio is a curated selection of my research projects and applied data science work, showcasing how I bridge machine learning, healthcare data, and user trust in AI-driven systems.

## 📌 Focus Areas
- ⌚ **Wearables & Health Data**: Physiological signal analysis, anomaly detection, telehealth adoption  
- 🤖 **Machine Learning**: Deep learning, LLMs, time-series modeling, recommender systems  
- 📊 **Applied Data Science**: A/B testing, RFM analysis, causal inference, business intelligence  
- 📈 **Communication & Impact**: Clear reporting, visualization, and decision-making support  

## 📬 Let’s Connect
If you're a recruiter, teammate, or fellow data enthusiast, feel free to explore these projects and reach out if you'd like to collaborate or chat about any of these ideas!  
I’m passionate about solving real-world problems at the intersection of **AI, healthcare data, and applied machine learning** through thoughtful analysis and scalable models.  
👉 [Connect with me on LinkedIn](https://www.linkedin.com/in/ruijingw/) · [Google Scholar](https://scholar.google.com/citations?user=26WzQFgAAAAJ&hl=en)

---

## 📂 Projects Overview

### 🔬 Research Projects 
These projects were conducted as part of my PhD research and collaborations. They resulted in peer-reviewed publications and focus on wearable health monitoring, anomaly detection, and AI explainability.
| Project | Topic | Skills & Tools | Data |
|---------|-------|----------------|--------------------|
| [Wearable Anomaly Detection](./01-wearable-anomaly/README.md) | Detect anomalies in Apple Watch HR + motion data under improper wearing, elevated humidity, unstable network conditions | PyTorch, Autoencoder, GPT-2, PCA, GAF | Private |
| [Fitbit Stress & Sleep Study](./02-fitbit-stress/README.md) | Modeling the link between stress, HR, and sleep | Python, Mixed-Effects, EBM, Scikit-learn | Public |
| [Trust & Telehealth Adoption](./03-trust-xai/README.md) | Trust in AI explanations + telehealth adoption analysis (HINTS dataset) | Ordinal logistic regression, Mediation analysis | Public |
| [PneuNet Diagnostic AI](./04-PneuNet/README.md) | Pneumonia detection from 30K+ X-rays using ViT + ResNet | PyTorch, Vision Transformer, ResNet | Public |

---

### 💻 Individual Projects 
These projects were done independently to explore new tools and techniques in machine learning, natural language processing, and data science. They demonstrate my hands-on skills and interest in continuous learning.
| Project | Topic | Skills & Tools | Data Link |
|---------|-------|----------------|-----------|
| [Multimodal Retrieval System](projects/multimodal-search/README.md) | Dual-tower CNN + Transformer for cross-modal search | FAISS, Triton, PyTorch | Synthetic demo |
| [RAG with LlamaIndex](projects/rag-pipeline/README.md) | RAG pipeline with Cohere re-rankers | LlamaIndex, Vector DB, Cohere API | Demo |
| [A/B Testing Framework](projects/ab-testing/README.md) | User engagement optimization with robust experiment design | Python, SQL, Causal inference | Simulated |
| [Movie Recommender Systems](projects/recommender-systems/README.md) | DeepFM & LightGCN for large-scale movie data | PyTorch, Spark, Pandas | Amazon dataset |

---

## 🛠 Tools & Techniques
`Python` · `PyTorch` · `TensorFlow` · `Scikit-Learn` · `SQL` · `R`  
`Mixed-Effects Models` · `Causal Inference` · `A/B Testing` · `Recommenders`  
`Time Series Analysis` · `LLMs` · `Transformers` · `Autoencoders`  
`Tableau` · `Power BI` · `Matplotlib` · `Seaborn`

---

## 🌟 Project Highlights

### ⌚ Wearable Data Integrity & Anomaly Detection
*Developed robust anomaly detection methods for wearable data streams, ensuring reliability in real-world health monitoring.*
- Collected and analyzed 200K+ Apple Watch datapoints under varied conditions (improper wearing, unstable networks, elevated humidity).
- Designed and evaluated autoencoder, CNN, and GPT-2 embedding pipelines for multivariate time series anomaly detection.
- Demonstrated how user-related, environmental and technical factors impact data integrity, providing insights critical for scaling remote health monitoring systems.
[Read More](./01-wearable-anomaly/README.md)
---

### 💤 Fitbit Stress & Sleep Signal Analysis
*Explored the relationship between stress, heart rate, and sleep in ICU residents.*  
- Mixed-effects modeling accounted for individual variability.  
- Built an Explainable Boosting Machine (EBM) to predict next-day stress. Achieved a 10% reduction in stress prediction error compared to baseline models.  
[Read More](./02-fitbit-stress/README.md)

---

### 🤝 Trust, Explainability & Telehealth
*Evaluated user trust in Apple Watch recommendations using survey data collected by me and explored the relationship between the use of wearables and telehealth adoption using the HINTS 6 dataset.*  
- Designed and conducted a survey to evaluate the usability, visualization, contextual information, and Explainable AI (XAI) in wearable health monitoring.  
- Conducted secondary analysis on HINTS 6 dataset (1,596 adults ≥65), showing wearable use is a significant predictor of telehealth adoption.
- Results provide business-ready insights for digital health companies on designing trustworthy AI interfaces and targeting adoption strategies for older populations.
[Read More](./03-trust-xai/README.md)

---
### 🫁 PneuNet: AI-Driven Pneumonia Detection  
*Built a deep learning pipeline to automate pneumonia detection from chest X-ray images, supporting faster and more reliable clinical screening.*  
- Processed a large-scale public chest X-ray dataset (normal vs. pneumonia cases).  
- Designed and trained convolutional neural networks (CNNs) for image classification.  
- Achieved strong performance in distinguishing pneumonia cases from healthy controls, demonstrating potential for real-world diagnostic support.  

[Read More](./04-PneuNet/README.md)  

