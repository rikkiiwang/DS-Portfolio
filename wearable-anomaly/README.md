## 📌 Project Overview
This project investigates how real-world factors affect the integrity of wearable health data and develops anomaly detection frameworks to identify data compromises.  
We designed two sequential experiments:  
1. **Experiment 1 (IDETC-CIE 2024):** Focused on human-related (improper wearing) and environmental-related (humidity) factors. Introduced a Gramian Angular Field (GAF)-based anomaly detection framework.  
2. **Experiment 2 (IEEE):** Extended to include a technical factor** (unstable network), with more participants and more data collected. Compared a hybrid autoencoder model against an LLM-based GPT-2 embedding pipeline.  

Together, these studies highlight how **social, environmental, and technical contexts** can undermine wearable device data quality — and how anomaly detection models can safeguard health monitoring.


## 📖 Background
- **Why this matters:** Remote health monitoring depends on reliable data. Inaccurate signals → misdiagnosis, patient risk.  
- **Gap:** Few studies systematically test *human-related, environmental-related, and technical factors* on wearable data integrity.  
- **Contribution:** We designed a controlled experiment and developed models to detect anomalies in **multivariate time series (HR + motion)**.


## 🗂 Data Collection & Preprocessing
## 🧪 Experiment 1: Human & Environmental Factors
- **Focus:** Improper Wearing (social) + Elevated Humidity (environmental).  
- **Data Collection:**  
  - Apple Watch (HR + motion).  
  - Baseline: 8 hours daily activity.  
  - Controlled sessions: improper wearing vs humidity.  
- **Framework:**  
  - **Gramian Angular Field (GAF):** Converted time series to 2D images.  
  - Autoencoder trained on baseline → anomaly detection via reconstruction error.  


## 🧪 Experiment 2: Adding Technical Factors
- **Motivation:** The first experiment showed strong effects, but wearable devices also face **network-related technical issues**. We expanded the study with **more participants and more data points** to test this.  
- **Focus:** Improper Wearing (social) + Elevated Humidity (environmental) + **Unstable Network (technical)**.  
- **Data Collection:**  
  - Baseline: 8 hours.  
  - Controlled: 25 minutes with 5 segments (wearing, rest, unstable network, rest, humidity).  
- **Frameworks Compared:**  
  1. **Hybrid Autoencoder** (CNN + LSTM + Attention).  
  2. **LLM-based GPT-2 Pipeline**: PCA → tokenization → GPT-2 embeddings → anomaly scoring.
 
## 📊 Exploratory Data Analysis

