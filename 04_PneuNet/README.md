# 🤖 PneuNet: AI-Assisted COVID-19 Pneumonia Diagnosis
## 📌 Overview
This project developed PneuNet, a Vision Transformer–based deep learning model for AI-assisted pneumonia diagnosis from chest X-ray (CXR) images. The goal was to address the urgent healthcare need for accurate and rapid COVID-19 screening, where conventional diagnostic methods (RT-PCR, antibody tests) face challenges such as false negatives and delayed results. PneuNet demonstrates how AI diagnosis and intelligent medical systems can augment clinicians, enabling fast, scalable, and accurate radiographic screening.

## 🏥 Background
- Global health burden: Pneumonia remains one of the leading causes of death worldwide, accounting for 14% of deaths in children under 5. COVID-19 worsened this challenge, with over 6 million deaths globally.
- Diagnostic challenge: COVID-19 pneumonia often shows subtle ground-glass opacity textures in lung X-rays, which are hard to distinguish manually or with conventional CNN-based methods.
- Healthcare opportunity: CXR imaging is already widely available in clinics, making it an ideal low-cost, rapid screening tool if supported by intelligent AI diagnostic systems.

## ⚙️ Methods
### Model Architecture
- Backbone: Lightweight ResNet18 for spatial feature extraction.
- Transformer Encoder: Treats each channel (from ResNet) as a patch and applies multi-head attention to evaluate feature importance.
- MLP Classifier: Fully connected layers with dropout and ReLU for classification.
- Output Layer: Softmax for 3-category prediction (None Pneumonia, Normal Pneumonia, COVID-19).
<p align="center">
  <img width="520" height="500" alt="image" src="https://github.com/user-attachments/assets/74edc8b4-487a-48a5-8727-66538be56f24" />
  <br>
  <em>Figure 1. Architecture of PneuNet (a) and the details of Transformer Encoder (b)</em>
</p>

### Dataset
- Total images: 33,920 CXR images from seven public repositories.
- Splits: Training (64%), Validation (16%), Test (20%).
- Categories:
    - 3-class: None Pneumonia, Normal Pneumonia, COVID-19.
    - 4-class: Added Bacterial vs. Viral Pneumonia distinction.
 
## 📊 Results

Table1. PneuNet Classification Results

| Task / Class Setting       | Accuracy | Precision | Recall  | F1 Score |
|-----------------------------|----------|-----------|---------|----------|
| **Three-Category** (COVID-19 / None / Pneumonia) | 95.16%  | 97.11%   | 97.39%  | 97.26%  |
| – COVID-19                 | –        | 96.95%    | 98.45%  | 97.69%  |
| – None Pneumonia           | –        | 96.64%    | 97.35%  | 96.99%  |
| – Pneumonia                | –        | 97.74%    | 96.37%  | 97.10%  |
| **Binary** (COVID-19 vs None) | 99.32%  | 98.94%   | 98.84%  | 98.88%  |
| – COVID-19                 | –        | 98.87%    | 99.00%  | 98.93%  |
| – None Pneumonia           | –        | 99.00%    | 98.67%  | 98.83%  |
| **Four-Category** (COVID-19 / None / Bacterial / Viral) | 90.03%  | 89.58%   | 89.62%  | 89.59%  |
| – COVID-19                 | –        | 94.17%    | 95.76%  | 94.96%  |
| – None Pneumonia           | –        | 90.83%    | 89.34%  | 90.07%  |
| – Bacterial Pneumonia      | –        | 85.83%    | 88.03%  | 86.92%  |
| – Viral Pneumonia          | –        | 87.50%    | 85.37%  | 86.42%  |

Table2. Comparison of PneuNet vs other models
 
| Model                      | Precision  | Recall     | F1 Score   | Accuracy   |
| -------------------------- | ---------- | ---------- | ---------- | ---------- |
| COVID-Net (Wang et al.)    | 93.55%     | 93.33%     | 93.44%     | 93.33%     |
| CoroNet (Khan et al.)      | 91.85%     | 94.63%     | 93.22%     | 91.30%     |
| DarkCovidNet (Ozturk)      | 89.96%     | 85.35%     | 87.59%     | 87.02%     |
| Attention VGG-16 (Sitaula) | 85.61%     | 80.10%     | 82.77%     | 81.36%     |
| PneuNet (proposed)        | 97.11% | 97.39% | 97.26% | 95.16% |

## 💡 Key Insights
- PneuNet outperforms CNN-only and VGG-based transformer models in pneumonia classification.
- Channel-based attention improves feature recognition of subtle textures.
- Model generalizes across binary, three-class, and four-class settings, showing robust adaptability.
