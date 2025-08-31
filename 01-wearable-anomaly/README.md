## 📌 Project Overview - Wearable Data Integrity & Anomaly Detection
This project aims to enhance the reliability and trustworthiness of wearable health monitoring systems by identifying anomalies in physiological data, including heart rate and motion signals. I conducted two human-subject experiments simulating potential user scenarios and developed a benchmark framework for anomaly detection in wearable time-series data. Starting with KitNet as the baseline, we advanced to more sophisticated models, including an autoencoder with image-based representations (GAF + CNN), attention-enhanced hybrid networks, and Large Language Models (LLMs).

The project has resulted in three peer-reviewed publications. These findings highlight how anomaly detection can reduce false alerts, increase trust in health insights, and support the scalable use of wearables in telehealth, fitness, insurance, and safety-critical industries.


## 📖 Background & Research Motivation
Wearable devices, such as the Apple Watch and Fitbit, have become an integral part of daily life, enabling people to track their heart rate, sleep patterns, and activity levels. Yet, users often report false alerts (e.g., “abnormal heart rate” notifications while resting) or missing/incorrect data when the device is worn loosely, exposed to sweat/humidity, or suffers from poor connectivity. These errors undermine user trust and limit adoption in healthcare.

From the perspective of healthcare and telehealth, Doctors hesitate to rely on consumer wearables for remote monitoring due to concerns about uncertain data integrity. A single false alarm can lead to unnecessary clinic visits or overlooked true emergencies. Regarding the related Fitness & Wellness Apps, Users cancel subscriptions when the app’s recommendations (e.g., calorie burn or training intensity) don’t match their actual effort due to faulty data.

This project was motivated by such real-world challenges. Our goal was to improve the reliability of wearable health data by detecting anomalies caused by human behavior, environmental factors, or technical issues, ensuring more accurate and trustworthy monitoring.



## 🧪 Benchmark, Metrics, and Experiment Setup
### Evaluation Metric  
- **Reconstruction Error:** Difference between input data and model reconstruction.  
- Larger errors = stronger deviation from expected "normal" patterns.  
- Used as the primary anomaly detection metric.

### Baseline Model  
- **KitNet**: Lightweight ensemble of autoencoders, originally for network intrusion detection.  
- Strengths: Fast, unsupervised, suitable for small devices.  
- Limitations: Detected obvious anomalies but struggled with subtle changes and user variability, exposing a performance gap.  

### Experiment Setup  
#### First Experiment (Pilot)  
- **Session Length:** 25 minutes  
- **Conditions:** Normal Wearing, Resting (control), Improper Wearing (loose strap)  
- **Limitation:** Small training dataset → led to expansion in the second experiment.  

#### Second Experiment (Expanded Study)  
- **Baseline Condition (8 hours):**  
  - Participants wore Apple Watches during daily activities (10 AM – 6 PM).  
  - Rich dataset of *normal* behavior, used for model training.  
- **Controlled In-Lab Condition (25 minutes):**  
  Each participant completed **five 5-minute sessions** including improper wearing, unstable network, elevated humidity adn two resting sessions (control).
   <p align="center">
   <img src="Figures/Experiment2_Flowchart.png" alt="Experiment 2 Procedure" style="width:25%;"/>
   <br>
   <em>Figure 4: Second Experiment Procedure Flowchart</em>
   </p>


## 📊 Data Processing & Feature Engineering
The raw dataset combined Apple Watch heart rate (7–8s intervals) and motion data (1s intervals). Steps included: 
- Synchronization of heart rate and motion timestamps.
- Interpolation to fill gaps in heart rate data.
- Feature Engineering: motion magnitude (via Euclidean norm), normalization (Min-Max scaling), and segmentation into time windows for training.
- Dimensionality Reduction
  - Challenge: Motion data had 3 axes (x, y, z) while heart rate had only 1 channel.  
    - Initial autoencoder tests showed poor results — the model overfit to motion data and ignored subtle heart rate variations.  
  - Diagnosis: The imbalance of **3 motion features vs. 1 heart rate feature** skewed the learning process.  
  - Solution: Apply **Principal Component Analysis (PCA)** on the motion data.  
    - Reduced the 3D motion signals into 1 principal component capturing the dominant variance.  
    - Balanced the input space into **two channels.  
  - Impact: 
    - Eliminated redundancy and noise from motion data.  
    - Ensured equal weighting of motion and heart rate inputs.  
    - Improved anomaly detection performance in the autoencoder and downstream LLM experiments.
   
Examples of normalized data segments:

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

Threshold Calculation (L2 Norm + MAD) of Autoencoder with GAF
- **Why L2 norm?** Compared to MSE/MAE, the Euclidean distance better captured anomaly magnitude in the feature space.  
- **Why MAD?** Median Absolute Deviation is robust to outliers, yielding **stable thresholds** for noisy wearable data.  
- Final decision rule:  
  - Compute reconstruction error using L2 norm.  
  - Flag anomaly if > (median + k × MAD).
This framework combined **GAF (temporal-preserving representation)** + **PCA (dimensionality reduction)** + **Autoencoder (feature learning)** + **Robust thresholding (L2 + MAD)**.  
By training only on normal patterns, the model specialized in reconstructing baseline patterns, making deviation due to improper wearing or humidity easy to detect.

Intro of convolutional autoencoder

Autoencoder with attention structure: (draw this in a flowchart)
**Architecture highlights:**  in flowchart
- Input: (224 × 224 × 2) images (HR + motion)  
- Encoder: 3 convolutional layers (32 filters, 3×3 kernel, ReLU activation)  
- Downsampling: MaxPooling (2×2)  
- Bottleneck: latent representation (forces compact feature learning)  
- Decoder: Upsampling layers + final Conv layer with sigmoid activation  
- Output: reconstructed image  
 
Challenge: Small, noisy datasets from real-world wearables made models prone to overfitting.
Solution: Hybrid autoencoder with attention improved generalization, while PCA ensured balanced feature inputs.

---

## Insights & Future Plans

This project highlights:
- User Trust: False alerts reduce confidence in wearables; anomaly detection can filter errors before they reach the user.
- Healthcare Applications: Reliable anomaly detection enables clinicians to trust wearable data for remote monitoring, potentially reducing hospital visits and improving early detection.
- Scalability: Lightweight methods like KitNet are ideal for devices with limited computation, while advanced autoencoders can run on the cloud for richer analytics.

Future Plans:
- Integrating explainable AI (XAI) to show why an anomaly was flagged (improving transparency).
- Expanding experiments to include multi-day, real-world data for greater robustness.
- Exploring LLM–Autoencoder hybrid models for interpretable, high-accuracy anomaly detection in wearable health systems.

  



















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

