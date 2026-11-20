# Oryx (Stock M8): Real-Time Retail Shelf Empty-Space Detection & Inventory Monitoring

Oryx (Stock M8) is an integrated computer vision and mechatronics system designed to automate inventory auditing in retail hypermarkets. By leveraging YOLOv10 and real-time camera telemetry, the system detects empty shelf space, tracks product availability, and optimizes replenishment schedules without requiring manual store audits.

---

## System Architecture

The project integrates three core subsystems:
1. **Physical Motion Controller (`Oryx_Motor_Control.ino`)**: Arduino firmware driving a motor rail platform to position camera sensors along shelf aisles.
2. **Dual-Model Detection Pipeline (`Oryx_model`)**:
   - `Oryx_Space_Final.pt`: Custom-trained YOLOv10 model for empty shelf space detection and bounding box localization.
   - `Oryx_Product_Final.pt`: Auxiliary YOLOv10 model for individual SKU and product package identification.
3. **Interactive Telemetry & Voice Assistant (`Oryx_eyse_chatbot`)**: An assistive tracking module utilizing dlib facial landmark tracking and speech interaction.

---

## Dataset & Annotation

- **Dataset**: [Oryx Shelf Dataset (Google Drive)](https://drive.google.com/file/d/1NmtMoH3OipUKnSgL8k9nrQPSteMmSXNG/view?usp=drive_link)
- **Annotation Tool**: Roboflow Annotate (Exported in YOLOv10 format).

![Data Annotation](https://github.com/user-attachments/assets/1557f9f8-6f17-48db-9ec0-eb5cfc240fa8)

### Preprocessing & Augmentation
- **Train/Val/Test Split**: 70% Training, 20% Validation, 10% Testing.
- **Resize**: Standardized for inference latency and training stability.
- **Auto-Orient**: Eliminates camera orientation bias.
- **Augmentation Pipeline**:
  - 90-degree rotations to accommodate varied camera mounting angles.
  - Exposure variations to build resilience against retail lighting conditions and reflections.

---

## Model Evaluation & Performance

### Confusion Matrix
![Confusion Matrix](https://github.com/user-attachments/assets/63d1f544-a170-4dcb-8e6f-817694531eae)

### Metrics: F1-Score, Precision, and Recall Curves
| Metric | Visualization |
| --- | --- |
| **F1-Confidence Curve** | ![F1 Score](https://github.com/user-attachments/assets/a1483877-4bd5-4ee8-9743-0e509d6162fb) |
| **Precision-Confidence Curve** | ![Precision](https://github.com/user-attachments/assets/03a04329-2e6c-4a9e-80b5-4e6ee35e9089) |
| **Recall-Confidence Curve** | ![Recall](https://github.com/user-attachments/assets/346791d8-8ca7-40a1-9a1e-6c32805a86d6) |

### Validation Results Summary
![Results](https://github.com/user-attachments/assets/8d6a061b-53d6-4cb4-9847-8639aa7e6fea)

---

## Inference & Real-Time Detection

The YOLOv10 model identifies vacant slots on shelving units and computes spatial coordinates for inventory alerts.

![Detection Sample 1](https://github.com/user-attachments/assets/e2111e6a-a725-4a33-beb4-35ea83ffab29)
![Detection Sample 2](https://github.com/user-attachments/assets/10a0dd03-ada4-496f-9a78-6f50d48aef3b)
![Detection Sample 3](https://github.com/user-attachments/assets/9500cd7a-ca9b-4194-a02d-21153e8ec537)
![Detection Sample 4](https://github.com/user-attachments/assets/03646dcc-61b3-4f82-81c9-f8cdfaf041b9)

---

## Quick Start

### 1. Environment Setup
```bash
git clone https://github.com/AmjedKhaled165/Oryx.git
cd Oryx
pip install -r requirements.txt
```

### 2. Optional: dlib Dependency for Voice & Face Module
If running the eye-tracking assistant subsystem (`Oryx_eyse_chatbot`):
```bash
pip install dlib-20.0.0-cp310-cp310-win_amd64.whl
```
*(Recommended environment: Python 3.10 64-bit on Windows)*

### 3. Running Detection Pipeline
Navigate to `Oryx_model` and execute:
```bash
cd Oryx_model
python models_run_code.py
```
*Note: Ensure weight paths (`Oryx_Space_Final.pt` / `Oryx_Product_Final.pt`) point to your local directory.*

---

## Tech Stack
- **Model**: Ultralytics YOLOv10
- **Computer Vision**: OpenCV, dlib
- **Embedded / Hardware**: Arduino C++ (`Oryx_Motor_Control.ino`)
- **Language**: Python 3.10
- **Data Engineering**: Roboflow
