# YOLO-Based Medical Ultrasound Detection

Research portfolio on YOLO-based medical ultrasound object detection for **heart valve regurgitation (HR)** and **Kawasaki disease (KD)** recognition, including horizontal bounding box (HBB) detection, oriented bounding box (OBB) detection, revised non-maximum suppression (NMS), mutual exclusion post-processing, and mutual exclusion loss.

> **Note**  
> This repository is intended for academic and portfolio presentation. Raw medical images, patient-level annotations, and private clinical data are not included due to privacy and institutional restrictions.

---

## Overview

This repository summarizes a research track focused on applying deep learning and object detection to echocardiography-based medical image analysis. The main research goal is to evaluate whether YOLO-based object detectors can support computer-aided diagnosis (CAD) for cardiovascular-related ultrasound image interpretation.

The research covers two medical imaging tasks:

1. **Heart Valve Regurgitation Detection**  
   Detecting four major types of heart valve regurgitation from color Doppler echocardiography images.

2. **Kawasaki Disease Coronary Lesion Localization**  
   Localizing coronary artery abnormalities associated with Kawasaki disease, including brightness and dilatation patterns in left and right coronary arteries.

The work further investigates how different YOLO versions, bounding box formats, revised NMS strategies, and mutual exclusion mechanisms influence detection performance and clinical interpretability.

---

## Research Background

Cardiovascular diseases and heart valve diseases remain important clinical challenges. Echocardiography provides real-time, non-invasive imaging for examining cardiac structures, blood flow, valve function, and coronary artery abnormalities.

In this research track, YOLO object detection models are used to detect and localize disease-related visual patterns in echocardiography images. The work focuses on two disease contexts:

| Task | Clinical focus | Imaging target |
|---|---|---|
| Heart Regurgitation (HR) | Valvular regurgitation | Abnormal Doppler blood-flow patterns |
| Kawasaki Disease (KD) | Coronary artery lesion assessment | Brightness and dilatation around LCA/RCA |

The motivation is to develop automated detection workflows that may support clinicians by improving efficiency, reducing repetitive visual workload, and providing structured localization results.

---

## Research Questions

This repository is organized around the following research questions:

1. Can YOLO-based detectors assist medical image recognition in echocardiography?
2. How do different YOLO versions perform across HR and KD detection tasks?
3. How do HBB and OBB detection differ when applied to different disease characteristics?
4. Can revised NMS reduce redundant or conflicting predictions in medical object detection?
5. Can mutual exclusion mechanisms reduce clinically inconsistent multi-class predictions?
6. Which model-strategy combinations provide the best balance between accuracy, interpretability, and practical deployment?

---

## Tasks

| Task | Disease / condition | Detection type | Classes |
|---|---|---|---|
| Heart Regurgitation | Heart valve regurgitation | HBB / OBB depending on experiment | AR, MR, PR, TR |
| Kawasaki Disease | Coronary artery lesions | OBB | LCA-brightness, LCA-dilatation, RCA-brightness, RCA-dilatation |

### Heart Regurgitation Classes

| Class | Full name |
|---|---|
| AR | Aortic Regurgitation |
| MR | Mitral Regurgitation |
| PR | Pulmonary Regurgitation |
| TR | Tricuspid Regurgitation |

### Kawasaki Disease Classes

| Class | Description |
|---|---|
| LCA-brightness | Brightness pattern near the left coronary artery |
| LCA-dilatation | Dilatation pattern near the left coronary artery |
| RCA-brightness | Brightness pattern near the right coronary artery |
| RCA-dilatation | Dilatation pattern near the right coronary artery |

---

## Methods

The repository documents experiments and method designs involving:

- YOLOv5 / YOLOv7 / YOLOv8 / YOLOv9
- HBB and OBB detection
- Revised NMS
- Skip-aware class-wise NMS
- Mutual Exclusion post-processing
- Mutual Exclusion loss
- 5-fold cross-validation
- Model comparison under controlled experimental settings
- Statistical analysis using ANOVA and post-hoc comparisons

---

## Methodological Design

### 1. YOLO Model Comparison

Multiple YOLO versions were compared to evaluate architectural differences across detection tasks.

| YOLO version | Role in this research |
|---|---|
| YOLOv5 | Stable baseline and customizable framework |
| YOLOv7 | Conference study and baseline comparison for HR detection |
| YOLOv8 | Anchor-free framework and OBB support |
| YOLOv9 | Newer YOLO variant evaluated in HR experiments |

The purpose of comparing multiple YOLO versions is not only to find the highest-performing model, but also to understand whether customized strategies behave consistently across different YOLO generations.

### 2. HBB and OBB Detection

HBB detection is suitable for more regular rectangular localization tasks. OBB detection is especially relevant for KD because coronary artery patterns may appear rotated, elongated, or irregular in ultrasound images.

| Bounding box type | Use case | Reason |
|---|---|---|
| HBB | Heart regurgitation detection | Doppler flow regions can often be represented by standard rectangular boxes |
| OBB | Kawasaki disease lesion localization | Coronary artery abnormalities may appear tilted or irregular, making rotated boxes more appropriate |

### 3. Revised NMS

The revised NMS strategy modifies the standard post-processing stage to reduce redundant predictions and improve interpretability.

Key ideas:

- Apply class-wise selection rules.
- Keep only high-confidence predictions for specific classes.
- Use skip-aware logic for clinically frequent or structurally important classes.
- Control the maximum number of detections per image.

### 4. Mutual Exclusion Post-processing

Mutual Exclusion post-processing applies clinical consistency rules after NMS. If two overlapping predictions belong to mutually exclusive classes, the prediction with lower confidence is removed.

This method does not require retraining and can be used as a lightweight inference-stage correction.

### 5. Mutual Exclusion Loss

Mutual Exclusion loss introduces clinical conflict constraints during training. It is designed to discourage the model from learning inconsistent class combinations before inference.

The loss is integrated as an additional penalty term alongside the original YOLO loss components.

---

## Customized Strategy Groups

| Strategy | Description |
|---|---|
| Method 1-1 | Apply revised NMS only during post-processing |
| Method 1-2 | Apply Mutual Exclusion only during post-processing |
| Method 1-3 | Apply revised NMS and Mutual Exclusion during post-processing |
| Method 2-1 | Apply revised NMS during training-related workflow and post-processing |
| Method 2-2 | Apply Mutual Exclusion loss during training and Mutual Exclusion during post-processing |
| Method 2-3 | Apply revised NMS and Mutual Exclusion during both training-related workflow and post-processing |

---

## Key Findings

### Heart Regurgitation Detection

- YOLO-based models showed the ability to detect four major heart regurgitation types from echocardiography images.
- The YOLOv7 study found that `yolov7-e6e` achieved the best overall mAP50 among the evaluated YOLOv7 variants.
- Pulmonary regurgitation (PR) was generally more difficult to detect than AR, MR, and TR.
- Revised NMS and Mutual Exclusion strategies were more beneficial for HR than for KD.
- Method 1-3, which combines revised NMS and Mutual Exclusion post-processing, showed stable improvement in HR experiments.

### Kawasaki Disease Detection

- OBB detection was used for KD because coronary artery lesions may appear rotated or irregular.
- YOLOv8-OBB showed strong baseline performance for KD detection.
- Customized strategies produced more limited gains for KD than HR.
- Excessive constraints may suppress valid KD detections when the baseline model is already strong.

---

## Results Summary

### YOLOv7 Heart Regurgitation Study

| Model | Parameters | Input size | mAP50 | mAP50-95 |
|---|---:|---:|---:|---:|
| YOLOv7 | 37.6M | 448 | 0.798 | 0.367 |
| YOLOv7x | 71.3M | 448 | 0.736 | 0.292 |
| YOLOv7-w6 | 82.3M | 448 | 0.771 | 0.333 |
| YOLOv7-e6 | 111.9M | 448 | 0.788 | 0.360 |
| YOLOv7-d6 | 154.7M | 448 | 0.735 | 0.317 |
| YOLOv7-e6e | 166.4M | 448 | 0.808 | 0.361 |

### Class-wise YOLOv7 Performance

| Class | YOLOv7 | YOLOv7x | YOLOv7-w6 | YOLOv7-e6 | YOLOv7-d6 | YOLOv7-e6e |
|---|---:|---:|---:|---:|---:|---:|
| AR | 0.7848 | 0.7376 | 0.8044 | 0.8034 | 0.7580 | 0.8148 |
| MR | 0.8442 | 0.7796 | 0.7714 | 0.8610 | 0.7648 | 0.8506 |
| PR | 0.7210 | 0.6140 | 0.7148 | 0.6636 | 0.6452 | 0.7158 |
| TR | 0.8428 | 0.7628 | 0.7948 | 0.8244 | 0.7724 | 0.8504 |

---

## Publications and Research Outputs

### Journal Articles

1. Chen, S. H., Weng, K. P., Hsieh, K. S., Chen, Y. H., Shih, J. H., Li, W. R., ... & Kao, T. Y. (2024). Optimizing object detection algorithms for congenital heart diseases in echocardiography: exploring bounding box sizes and data augmentation techniques. *Reviews in Cardiovascular Medicine, 25*(9), 335.

2. Chen, S. H., Kuo, H. C., Weng, K. P., Hsieh, K. S., Kao, T. Y., Chen, Y. H., ... & Liao, C. H. (2025). Revised NMS-driven pipeline for heart valve regurgitation and Kawasaki disease coronary aneurysm localization. *Computers in Biology and Medicine, 198*, 111125.

### Conference Paper / Presentation

3. Chen, S. H., Kao, T. Y., & Chen, Y. H. (2024, September). Detecting heart valve regurgitation in medical images – using YOLOv7. In *Proceedings of the 2024 5th Asia Service Sciences and Software Engineering Conference* (pp. 126–129).

### Master's Thesis

4. Kao, T. Y. Deep Learning for Heart Regurgitation and Kawasaki Disease Recognition in Medical Images. Master's thesis, Chang Gung University.

---

## Repository Structure

```text
YOLO-Medical-Ultrasound-Detection/
├── README.md
├── docs/
│   ├── thesis_summary.md
│   ├── methodology.md
│   ├── results_summary.md
│   └── publication.md
├── heart_regurgitation/
│   ├── README.md
│   ├── figures/
│   └── sample_outputs/
├── kawasaki_disease/
│   ├── README.md
│   ├── figures/
│   └── sample_outputs/
├── methods/
│   ├── revised_nms.md
│   ├── mutual_exclusion_postprocessing.md
│   └── mutual_exclusion_loss.md
├── experiments/
│   ├── model_comparison.md
│   ├── hbb_vs_obb.md
│   └── ablation_study.md
├── assets/
│   ├── architecture/
│   ├── result_charts/
│   └── qualitative_results/
└── LICENSE
```

---

## Privacy and Data Availability

The original medical image datasets are not publicly released in this repository because they contain clinical imaging data and may be subject to hospital, research ethics, or institutional restrictions.

This repository may include:

- Method descriptions
- Experimental summaries
- Publication references
- Architecture diagrams
- Result tables
- Non-sensitive visual examples, if permitted
- Pseudocode or cleaned implementation notes

This repository does not include:

- Raw medical images
- Patient identifiers
- Private annotations
- Hospital-specific file paths
- Full clinical datasets

---

## Limitations

- The research results depend on dataset quality, annotation consistency, and class balance.
- Medical ultrasound images are operator-dependent and may vary across hospitals, devices, and imaging views.
- Revised NMS and Mutual Exclusion strategies may improve one task while producing limited or negative effects in another task.
- KD detection is especially sensitive to small lesion boundaries, OBB annotation quality, and overlapping coronary features.
- Clinical deployment would require external validation, prospective testing, and clinician-in-the-loop evaluation.

---

## Future Work

- Validate the workflow on larger multi-center echocardiography datasets.
- Improve annotation consistency through expert review and inter-rater agreement analysis.
- Explore adaptive NMS and dynamic conflict-resolution strategies.
- Extend OBB-based detection to other ultrasound-based cardiovascular tasks.
- Build clinician-facing interfaces for real-time review and feedback.
- Evaluate model generalization across institutions and ultrasound devices.

---

## Suggested Citation

If referring to this repository or related work, please cite the corresponding publication or thesis listed in the **Publications and Research Outputs** section.
