## 📌 Project Overview - Wearable Data Integrity & Anomaly Detection
This project aims to enhance the reliability and trustworthiness of wearable health monitoring systems by identifying anomalies in physiological data, including heart rate and motion signals. I conducted two human-subject experiments simulating potential user scenarios and developed a benchmark framework for anomaly detection in wearable time-series data. Starting with KitNet as the baseline, we advanced to more sophisticated models, including an autoencoder with image-based representations (GAF + CNN), attention-enhanced hybrid networks, and Large Language Models (LLMs).

The project has resulted in three peer-reviewed publications. These findings highlight how anomaly detection can reduce false alerts, increase trust in health insights, and support the scalable use of wearables in telehealth, fitness, insurance, and safety-critical industries.


## 📖 Background & Research Motivation
Wearable devices, such as the Apple Watch and Fitbit, have become an integral part of daily life, enabling people to track their heart rate, sleep patterns, and activity levels. Yet, users often report false alerts (e.g., “abnormal heart rate” notifications while resting) or missing/incorrect data when the device is worn loosely, exposed to sweat/humidity, or suffers from poor connectivity. These errors undermine user trust and limit adoption in healthcare.

From the perspective of healthcare and telehealth, Doctors hesitate to rely on consumer wearables for remote monitoring due to concerns about uncertain data integrity. A single false alarm can lead to unnecessary clinic visits or overlooked true emergencies. Regarding the related Fitness & Wellness Apps, Users cancel subscriptions when the app’s recommendations (e.g., calorie burn or training intensity) don’t match their actual effort due to faulty data.

This project was motivated by such real-world challenges. Our goal was to improve the reliability of wearable health data by detecting anomalies caused by human behavior, environmental factors, or technical issues, ensuring more accurate and trustworthy monitoring.



## 🧪 Metrics & Experiment
Evaluation Metric: Reconstruction error (how far data deviates from expected normal patterns).
Baseline Model: KitNet, a lightweight neural-network ensemble originally developed for network intrusion detection, repurposed here for wearable data
. KitNet served as the baseline anomaly detector.
Experiment Setup: Heart rate and motion data were collected under baseline (normal wear) and test conditions (improper wearing, unstable network, high humidity). 
Key Result: KitNet detected anomalies but struggled with subtle changes and variability across users, exposing a performance gap that motivated advanced models.

## 📊 Data Processing & Feature Engineering
The raw dataset combined Apple Watch heart rate (7–8s intervals) and motion data (1s intervals). Steps included: 
- Synchronization of heart rate and motion timestamps.
- Interpolation to fill gaps in heart rate data.
- Feature Engineering: motion magnitude (via Euclidean norm), normalization (Min-Max scaling), and segmentation into time windows for training.
- Dimensionality Reduction: For complex inputs, Principal Component Analysis (PCA) was applied to balance motion and heart rate signals

## Model Development 
We tested and advanced multiple models:
| **Model**                                         | **Approach**                                                                | **Why Chosen**                                             | **Performance / Insights**                                                         |
| ------------------------------------------------- | --------------------------------------------------------------------------- | ---------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| **KitNet (Baseline)**                             | Ensemble autoencoders measuring reconstruction error                        | Lightweight, unsupervised, established baseline            | Detected anomalies but limited on subtle/contextual changes                        |
| **Autoencoder + GAF**                             | Converted time-series into images (Gramian Angular Field) + CNN autoencoder | To leverage image-recognition power of CNNs                | Achieved **88.9% accuracy** for improper wearing, **100%** for humidity            |
| **Autoencoder + Attention (Hybrid ConvAE-ALSTM)** | CNN for spatial patterns + LSTM for temporal trends + Attention for context | To capture both short- and long-term dependencies          | Outperformed KitNet, robust to noisy data                                          |
| **LLM (GPT-2 embeddings)**                        | Transformed PCA features into text-like sequences, embedded via GPT-2       | Tested frontier method: treating time series as “language” | Promising interpretability, but sensitive to noise; less accurate than autoencoder |

- **Model choice decisions:**  
  - Adopted GAF representation for Experiment 1 because literature showed its ability to transform 1D time series into 2D images that preserve temporal correlations, making anomalies more visible.  
  - Adopted Hybrid Autoencoder (CNN + LSTM + Attention) for Experiment 2 because prior anomaly detection work emphasized combining spatial + temporal features.  
  - Tested LLMs (GPT-2 embeddings) because emerging literature suggested LLMs can model sequential patterns when time series are serialized into tokens.
 
Data Transformation Using GAF
- We transformed heart rate and 3D acceleration (x, y, z) into **image representations** using the **Gramian Angular Field (GAF)** method.  
- GAF encodes time series into polar coordinates:  
  - Angular dimension = time  
  - Radial dimension = data values  
- This transformation preserves temporal correlations — critical for anomaly detection in physiological signals.  
- We used Gramian Angular Summation Field (GASF), computed from cosine/sine outer products of the polar coordinates.

<table>
<tr>
<td><img src="Figures/hr_normal.png" width="450"/><br><em>(a1) HR data, normal condition</em></td>
<td><img src="Figures/hr_gaf_normal.png" width="350"/><br><em>(a2) HR GAF image, normal condition</em></td>
</tr>
<tr>
<td><img src="Figures/hr_humidity.png" width="450"/><br><em>(b1) HR data, elevated humidity</em></td>
<td><img src="Figures/hr_gaf_humidity.png" width="350"/><br><em>(b2) HR GAF image, elevated humidity</em></td>
</tr>
</table>
<em>Figure 2: GAF Transformation of Heart Rate Data</em>

<table>
<tr>
<td><img src="Figures/motion_normal.png" width="450"/><br><em>(c1) Motion data, normal condition</em></td>
<td><img src="Figures/motion_gaf_normal.png" width="350"/><br><em>(c2) Motion GAF image, normal condition</em></td>
</tr>
<tr>
<td><img src="Figures/motion_humidity.png" width="450"/><br><em>(d1) Motion data, elevated humidity</em></td>
<td><img src="Figures/motion_gaf_humidity.png" width="350"/><br><em>(d2) Motion GAF image, elevated humidity</em></td>
</tr>
</table>
<em>Figure 3: GAF Transformation of Motion Data</em>

Intro of convolutional autoencoder
 
Challenge: Small, noisy datasets from real-world wearables made models prone to overfitting.
Solution: Hybrid autoencoder with attention improved generalization, while PCA ensured balanced feature inputs.

---


  









**Architecture highlights:**  in flowchart
- Input: (224 × 224 × 2) images (HR + motion)  
- Encoder: 3 convolutional layers (32 filters, 3×3 kernel, ReLU activation)  
- Downsampling: MaxPooling (2×2)  
- Bottleneck: latent representation (forces compact feature learning)  
- Decoder: Upsampling layers + final Conv layer with sigmoid activation  
- Output: reconstructed image  


#### 4. Threshold Calculation (L2 Norm + MAD)
- **Why L2 norm?** Compared to MSE/MAE, the Euclidean distance better captured anomaly magnitude in the feature space.  
- **Why MAD?** Median Absolute Deviation is robust to outliers, yielding **stable thresholds** for noisy wearable data.  
- Final decision rule:  
  - Compute reconstruction error using L2 norm.  
  - Flag anomaly if > (median + k × MAD).  

---

✅ **Key innovation:**  compare with other model or industry method, benchmark 
This framework combined **GAF (temporal-preserving representation)** + **PCA (dimensionality reduction)** + **Autoencoder (feature learning)** + **Robust thresholding (L2 + MAD)**.  
By training only on normal patterns, the model specialized in reconstructing baseline patterns, making deviation due to improper wearing or humidity easy to detect.
 
---


- **Baseline Condition (8 hours):**  
  Participants wore Apple Watches during daily activities (10 AM – 6 PM). This created a large, context-rich dataset of *normal* behavior.  
- **Controlled In-Lab Condition (25 minutes):**  
  Each participant completed five 5-minute sessions:  
  1. **Improper Wearing** (strap adjusted too loose/tight)  
  2. **Resting (control)**  
  3. **Unstable Network** (Wi-Fi interrupted for 2 minutes mid-session)  
  4. **Resting (control)**  
  5. **Elevated Humidity** (room humidity raised to ~65%)
  <p align="center">
  <img src="Figures/Experiment2_Flowchart.png" alt="Experiment 2 Procedure" style="width:40%;"/>
  <br>
  <em>Figure 4: Experiment 2 Procedure Flowchart</em>
  </p>

To enrich health-related signals and avoid overly static data, participants performed **guided low-intensity rehab exercises** (e.g., marching, side taps) during sessions. This setup aligned with clinical literature on rehabilitation exercises and ensured natural variability in heart rate and motion while keeping risk minimal.

#### 2. Data Processing
- **Synchronization:** Heart rate and motion data aligned on timestamps.  
- **Feature Engineering:** Motion magnitude = Euclidean norm of x, y, z acceleration.  
- **Normalization:** Min-Max scaling applied to [0,1].  
- **Segmentation:**  
  - Training set: baseline data segmented into a fixed-length window of 30 time steps.  
  - Testing set: each 5-min session adjusted to the same length (padding/truncation).
 
📊 Examples of normalized data segments:

<p align="center">
  <img src="Figures/exp2_training_segment.png" alt="Training Data Segment: Baseline HR and Motion" width="600"/>
  <br>
  <em>Figure 5. Example of Training Data Segment: Normalized heart rate and motion magnitude during baseline.</em>
</p>

<p align="center">
  <img src="Figures/exp2_testing_segment.png" alt="Testing Data Segment: Controlled Session HR and Motion" width="600"/>
  <br>
  <em>Figure 6. Example of Testing Data Segment: Normalized heart rate and motion magnitude during controlled conditions.</em>
</p>
 

**Contextual Factor Analysis:**  
- All three disruptions (human, environmental, technical) significantly increased reconstruction errors (ANOVA: F=26.14, p<0.00001).  
- Pairwise t-tests showed **very large effect sizes (Cohen’s d > 2.0)** for each factor compared to baseline.  
- Importantly, **improper wearing, unstable network, and humidity caused similar magnitudes of data degradation** — different origins, but equivalent impact on data integrity.  

#### 5. Key Takeaways
- **Human, environmental, and technical factors all independently compromise wearable data integrity.**  
- **Hybrid Autoencoder > LLM** for small, noisy, context-specific wearable datasets.  
- **LLMs still hold promise** for multimodal or text-rich contexts, but require better feature-to-text representation.  
- **Design implication:** anomaly detection in health wearables must handle *multiple disruption types simultaneously*, not just user errors.

## Publication
- Wang, R., & Liao, T. *Examining Social and Environmental Factors for Wearable Data Integrity: A Case Study of Health-CPS.* ASME IDETC-CIE 2024.
- Wang, R., & Liao, T. *Anomaly Detection in Multivariate Time Series Data of Wearable Devices: A Comparative Study of Autoencoders and Large Language Models.* IEEE Computational Intelligence Magazine, 2025. (Under Review)

