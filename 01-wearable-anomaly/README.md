## 📌 Project Overview
This project investigates how real-world factors affect the integrity of wearable health data and develops anomaly detection frameworks to identify data compromises.  
We designed two sequential experiments:  
1. **Experiment 1 (IDETC-CIE 2024):** Focused on human-related (improper wearing) and environmental-related (humidity) factors. Introduced a Gramian Angular Field (GAF)-based anomaly detection framework.  
2. **Experiment 2 (IEEE):** Extended to include a technical factor (unstable network), with more participants and more data collected. Compared a hybrid autoencoder model against an LLM-based GPT-2 embedding pipeline.  

Together, these studies highlight how **human-related, environmental-related, and technical contexts** can undermine wearable device data quality — and how anomaly detection models can safeguard health monitoring.


## 📖 Background & Research Motivation
- **Why integrity matters:** Remote health monitoring depends on reliable data. Inaccurate wearable data can cause misdiagnosis, undertreatment, or even patient harm.  
- **Gap in literature:** Prior research had explored improper wearing and environmental factors like humidity, but no study systematically integrated human-related, environmental-related, and technical factors in one framework.  
- **Model choice decisions:**  
  - Adopted GAF representation for Experiment 1 because literature showed its ability to transform 1D time series into 2D images that preserve temporal correlations, making anomalies more visible.  
  - Adopted Hybrid Autoencoder (CNN + LSTM + Attention) for Experiment 2 because prior anomaly detection work emphasized combining spatial + temporal features.  
  - Tested LLMs (GPT-2 embeddings) because emerging literature suggested LLMs can model sequential patterns when time series are serialized into tokens.  

---

## 🧪 Experiment 1: Human & Environmental Factors (ASME IDETC-CIE 2024)
- **Focus:** Improper Wearing (human-related) + Elevated Humidity (environmental-related).  
- **Experiment Design**
    The experiment was conducted in person in the laboratory with participants wearing the Apple Watch.  
    The entire session lasted 25 minutes and was divided into five 5-minute blocks (see Figure below).  
    1. **Normal Wearing (5 min):** Device worn correctly with guidance from the proctor. This served as the baseline condition.  
    2. **Resting (5 min):** Cool-down session to avoid carry-over effects.  
    3. **Test Condition 1 (5 min):** Either Improper Wearing (strap deliberately adjusted to be too loose/tight) or Elevated Humidity (room humidity increased to ~65%).  
    4. **Resting (5 min):** Another cool-down period.  
    5. **Test Condition 2 (5 min):** The second factor is not tested in Step 3 (randomized order).
  <p align="center">
  <img src="Figures/Flowchart.png" alt="Experiment 1 Procedure" style="width:50%;"/>
  <br>
  <em>Figure: Experiment 1 Procedure Flowchart</em>
  </p>

- **Data Processing**
  - Timestamp synchronization between HR and motion.  
  - Motion magnitude = Euclidean norm of 3D acceleration.  
  - Min-Max normalization → scale signals into [0,1].  
  - Sliding windows (30 steps) for segmentation.  
  - GAF transformation to create 2D images from time series.

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
