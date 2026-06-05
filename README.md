# YOLO-Based Medical Ultrasound Detection

## Overview
Research portfolio on YOLO-based medical ultrasound object detection for heart valve regurgitation and Kawasaki disease recognition, including HBB/OBB detection, revised NMS, mutual exclusion post-processing, and mutual exclusion loss.

## Research Background
Heart valve regurgitation and Kawasaki disease recognition from ultrasound images.

## Research Questions
- Can YOLO-based detectors assist medical image recognition?
- How do different YOLO versions perform across HR and KD tasks?
- Can revised NMS and mutual exclusion mechanisms reduce conflicting predictions?

## Methods
- YOLOv5 / YOLOv7 / YOLOv8 / YOLOv9
- HBB and OBB detection
- Revised NMS
- Mutual Exclusion Post-processing
- Mutual Exclusion Loss

## Tasks
| Task | Disease | Detection type | Classes |
|---|---|---|---|
| Heart Regurgitation | AR, MR, TR, PR | HBB / OBB | 4 regurgitation types |
| Kawasaki Disease | Coronary artery lesions | OBB | LCA/RCA brightness and dilatation |

## Key Contributions
1. Compared multiple YOLO versions for medical ultrasound detection.
2. Designed revised NMS for disease-specific prediction conflicts.
3. Implemented mutual exclusion post-processing.
4. Integrated mutual exclusion loss into YOLO training.
5. Evaluated HBB and OBB suitability for different disease characteristics.

## Results Summary
Add result tables and selected charts.

## Publications / Presentations
- ASSE 2024 presentation: Detecting Heart Valve Regurgitation in Medical Images Using YOLOv7.
- Master's thesis: Utilizing Deep Learning to Analyze Medical Images for Heart Regurgitation and Kawasaki Disease Recognition.

## Limitations
No raw medical images or private clinical data are provided due to privacy restrictions.

## Repository Structure
...
