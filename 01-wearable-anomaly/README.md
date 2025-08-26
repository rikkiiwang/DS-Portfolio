## 📌 Project Overview - Wearable Data Integrity & Anomaly Detection
This project investigates how real-world factors affect the integrity of wearable health data and develops anomaly detection frameworks to identify data compromises.  
We designed two sequential experiments:  
1. **Experiment 1 (IDETC-CIE 2024):** Focused on human-related (improper wearing) and environmental-related (humidity) factors. Introduced a Gramian Angular Field (GAF)-based anomaly detection framework.  
2. **Experiment 2 (IEEE Computational Intelligence Magazine):** Extended to include a technical factor (unstable network), with more participants and more data collected. Compared a hybrid autoencoder model against an LLM-based GPT-2 embedding pipeline.  

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
- **Experiment Design:**
  The experiment was conducted in person in the laboratory with participants wearing the Apple Watch. The entire session lasted 25 minutes and was divided into five 5-minute blocks (see Figure below).  
    1. **Normal Wearing (5 min):** Device worn correctly with guidance from the proctor. This served as the baseline condition.  
    2. **Resting (5 min):** Cool-down session to avoid carry-over effects.  
    3. **Test Condition 1 (5 min):** Either Improper Wearing (strap deliberately adjusted to be too loose/tight) or Elevated Humidity (room humidity increased to ~65%).  
    4. **Resting (5 min):** Another cool-down period.  
    5. **Test Condition 2 (5 min):** The second factor is not tested in Step 3 (randomized order).
  <p align="center">
  <img src="Figures/Flowchart.png" alt="Experiment 1 Procedure" style="width:40%;"/>
  <br>
  <em>Figure 1: Experiment 1 Procedure Flowchart</em>
  </p>
  
### ⚙️ Proposed Multi-channel Framework (Experiment 1)
#### 1. Data Transformation Using GAF
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



#### 2. Dimensionality Reduction with PCA
- Because motion data had **three channels** and could dominate the HR signal, we applied **Principal Component Analysis (PCA)** to the GAF motion images.  
- PCA distilled high-dimensional motion into its **principal components**, ensuring:  
  - More efficient computation  
  - Balanced representation between motion and heart rate  
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


#### 3. Anomaly Identification Using Autoencoder
- We trained a convolutional autoencoder on baseline (normal) data.  
- The autoencoder reconstructs GAF inputs → reconstruction error measures how well the test data fits the normal pattern.  
- High reconstruction error = anomaly.  

**Architecture highlights:**  
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

✅ **Key innovation:**  
This framework combined **GAF (temporal-preserving representation)** + **PCA (dimensionality reduction)** + **Autoencoder (feature learning)** + **Robust thresholding (L2 + MAD)**.  
By training only on normal patterns, the model specialized in reconstructing baseline patterns, making deviation due to improper wearing or humidity easy to detect.
 
---

## 🧪 Experiment 2: Human, Environmental, and Technical Factors (IEEE Submission)
- **Motivation:**  Building on Experiment 1, where we studied only human- and environmental-related factors (improper wearing, elevated humidity), this follow-up experiment systematically introduced a **third dimension — technical factors (unstable network)**.  
- This expansion addressed a key research gap identified in the literature: although improper device use and environmental challenges were studied, **network instability was rarely quantified as a source of data integrity loss in wearable systems**.

#### 1. Experiment Design
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
 
#### 3. Models

**Hybrid Autoencoder (Proposed)**  
- **Encoder:** CNN layers (local feature extraction) → Dense → LSTM (temporal dependencies) → Multi-head Self-Attention (contextual importance).  
- **Bottleneck:** Dense layer for latent compact representation.  
- **Decoder:** LSTM + Deconv layers reconstruct sequences.  
- **Training:** Only on *baseline* data; anomaly threshold set at 95th percentile reconstruction error.  

**LLM-based Approach (Comparative)**  
- PCA → Tokenization → Serialize features into numerical strings → GPT-2 embeddings.  
- Session anomaly score = Euclidean norm of embedding.  
- Motivation: inspired by recent work on treating time series as text for LLM-based anomaly detection:contentReference[oaicite:2]{index=2}.  

#### 4. Results
**Model Comparison:**  
- Autoencoder achieved **85.3% overall accuracy**, outperforming the LLM model (**68%**):contentReference[oaicite:3]{index=3}.  
- Autoencoder reconstruction errors spiked during improper wearing, unstable network, and elevated humidity, while resting stayed close to baseline.  
- LLM struggled to preserve temporal structure after numerical-to-text transformation, reducing anomaly detection precision.  

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

