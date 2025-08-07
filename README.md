# SRCC Detection: Signet Ring Cell Detection with CenterNet & Knowledge Distillation

This repository contains the code and resources supporting the MSc dissertation:

**“Knowledge Distillation for Signet Ring Cell Detection in Histopathological Images Using CenterNet”**  
by Chatwipa Surapat, School of Computing, Newcastle University, 2025.

---

## Table of Contents
- [Overview](#overview)  
- [Repository Structure](#repository-structure)  
- [Setup Instructions](#setup-instructions)  
- [Usage Guide](#usage-guide)  
- [Evaluation Metrics](#evaluation-metrics)  
- [Results Summary](#results-summary)  

---

## Overview
This project demonstrates applying a teacher-student knowledge distillation framework combined with pseudo-labelling to compress a CenterNet-based detection model. The goal is to enable lightweight yet accurate detection of signet ring cells in digital pathology images.

---

## Repository Structure
```
SRCC_detection/
├── Baseline_student_model_training.ipynb
├── Data_Augmentation.ipynb
├── Data_Exploration.ipynb
├── Model_usage.ipynb
├── Student_model_training.ipynb
├── Student_model_training_noKD.ipynb
├── Teacher_model_training.ipynb
└── README.md
```

---

## Setup Instructions
We recommend using **Google Colab** to run the notebooks, especially if you do not have a GPU-enabled local environment. The notebooks are optimized for Colab and include setup instructions.

---

## Usage Guide

- **Data Exploration**:
  -  Examine the dataset and patching strategy:
  ```bash
  jupyter notebook Data_Exploration.ipynb
  ```
  -  Examine data augmentation strategy:
  ```bash
  jupyter notebook Data_Augmentation.ipynb
  ```

- **Train Teacher Model**:
  ```bash
  jupyter notebook Teacher_model_training.ipynb
  ```

- **Train Baseline & Student Models**:
  - Baseline:
    ```bash
    jupyter notebook Baseline_student_model_training.ipynb
    ```
  - Student + pseudo-labels :
    ```bash
    jupyter notebook Student_model_training_noKD.ipynb
    ```
  - Student + pseudo-labels + feature imitation:
    ```bash
    jupyter notebook Student_model_training.ipynb
    ```

- **Visualize Sample Predictions**:
  ```bash
  jupyter notebook Model_usage.ipynb
  ```
- **Additional Experiment**: explore stronger augmentation
  ```bash
  jupyter notebook [Strong_Augmented]_baseline_student_model_training.ipynb
  ```
---

## Evaluation Metrics
The following metrics are used in the analysis and reported in the dissertation:

- **Instance-level Recall**: This metric quantifies the proportion of actual signet ring cells that were correctly detected by the model in positive images, ranging from 0 to 1.
  
- **F1 Score**: In accordance with the rules of the grand challenge, only models achieving a precision greater than 0.2 were considered for final evaluation. Therefore, the F1-score was utilized alongside instance-level recall to ensure that high recall was not achieved at the expense of excessively low precision.
  
- **False Positive Score (FPs)**: This metric measures the average number of false positive detections per image within negative (non-SRCC) whole slide images.  A corresponding score (FPs), ranging from 0 to 100 which calculated as ```FPs = max(100 − FPNormal, 0)```. A score closer to 100 indicates a higher ability to avoid false detections in normal tissue, reflecting the model’s effectiveness at distinguishing signet ring cells from other tumour cells or background tissues.
  
- **FROC Score**: The FROC score was computed by averaging the instance-level recall values at predefined FPNormal levels of 1, 2, 4, 8, and 32, as specified by the grand challenge. In this study, the performance was evaluated across five confidence thresholds including 0.1, 0.3, 0.5, 0.7, and 0.9. If the detection output does not reach a specific predefined FPNormal level, the recall for that level is assigned the maximum recall value obtained at any preceding level.

---

## Results Summary
**Main Experiment**
| Model                                    | Recall (mean ± SD) | FPs (mean ± SD) | FROC Score (mean ± SD) | F1-Score (mean ± SD) |
| ---------------------------------------- | ------------------ | --------------- | ---------------------- | -------------------- |
| **Baseline (ResNet-18)**                 | 0.43 ± 0.07        | 99.95 ± 0.03    | 0.44 ± 0.08            | 0.44 ± 0.07          |
| **Teacher Model (ResNet-50)**            | 0.52 ± 0.06        | 99.95 ± 0.01    | **0.56 ± 0.05**           | 0.52 ± 0.05          |
| **Student Model (ResNet-18)**            |                    |                 |                        |                      |
| pseudo-labels                            | 0.47 ± 0.11        | 99.95 ± 0.01    | 0.47 ± 0.05            | 0.45 ± 0.03          |
| pseudo-labels and feature imitation      | **0.54 ± 0.06**        | **99.96 ± 0.01**    | **0.56 ± 0.05**            | **0.55 ± 0.03**          |

*Key findings:*
- The proposed student model reduces parameters by **68.8%** and memory footprint by **62.8%** compared to the teacher model, while maintaining performance.
- Pseudo-labels alone improved recall, but combining with feature imitation-based KD improved all metrics.
- CenterNet maintained competitive false-positive rates compared to RetinaNet-based methods.
  
**Supplementary Experiment (using CenterNet with ResNet-18 backbone)**
| Model                    | Recall (mean ± SD) | FPs (mean ± SD)  | FROC Score (mean ± SD) | F1-Score (mean ± SD) |
| ------------------------ | ------------------ | ---------------- | ---------------------- | -------------------- |
| Soft augmentation        | 0.43 ± 0.07        | 99.95 ± 0.03     | 0.44 ± 0.08            | 0.44 ± 0.07          |
| Strong augmentation      | **0.58 ± 0.05**    | **99.97 ± 0.02** | **0.64 ± 0.06**        | **0.56 ± 0.04**      |

*Key findings:*
- Stronger data augmentation significantly improved all key metrics compared to soft augmentation.
- This suggests potential for further performance gains through more advanced augmentation strategies in future work.


