# YOLO-RVT: End-to-End License Plate Recognition

---

## General Conventions

Each experiment directory typically contains:

| Directory   | Description                                                                   |
|-------------|-------------------------------------------------------------------------------|
| `Notebook/` | Jupyter Notebooks (`.ipynb`) containing training and evaluation code          |
| `Weight/`   | PyTorch weight files (`.pth`) containing trained checkpoints                  |
| `Detection/`| Notebooks and results for the license plate detection-only task               |

- **Checkpoint 1 (chk1)**: results from the first training stage (e.g., 27 epochs)
- **Checkpoint 2 (chk2)**: results from the second training stage (e.g., 54 epochs)

---

# Repository Structure

```text
├── Ablation/                         # Ablation study experiments — evaluating the contribution of each component
│   ├── WithoutCrossAttention/        # Without Cross-Attention module
│   ├── WithoutRecurrentHead/         # Without Recurrent Head (GRU/LSTM)
│   ├── WithoutAugmention/            # Without Data Augmentation
│   ├── DecreaseViT/                  # Reduced Vision Transformer size
│   └── FreeZoneBackbone/             # Frozen backbone without fine-tuning
│
├── MainResult/                       # Main results — comparison of YOLO-RVT variants on VNLP1
│   ├── V8S_GRU/                      # YOLOv8s + GRU head
│   ├── V8S_LSTM/                     # YOLOv8s + LSTM head
│   ├── V10S_GRU/                     # YOLOv10s + GRU head
│   ├── V10S_LSTM/                    # YOLOv10s + LSTM head
│   ├── V11S_GRU/                     # YOLOv11s + GRU head
│   ├── V11S_LSTM/                    # YOLOv11s + LSTM head
│   ├── 2Stage/                       # Two-stage pipeline (Detect → OCR separately)
│   └── Detection/                    # Training YOLO
│
├── AOLP/                             # Experiments on the AOLP dataset (Taiwan)
│   ├── AC/                           # Evaluation on the AC Subset of the AOLP Dataset
│   ├── LE/                           # Evaluation on the LE Subset of the AOLP Dataset
│   └── RP/                           # Evaluation on the RP Subset of the AOLP Dataset
│
├── Rodosol/                          # Experiments on the RodoSol-ALPR dataset (Brazil)
│   ├── Pretrained_108ep_VNLP/        # Transfer learning using VNLP pretrained weights
│   └── Scatch_Fusion_6+13/           # Training from scratch with Fusion (6+13 classes)
│
├── VNLP2/                            # Experiments on the VNLP2 dataset (Vietnam)
│   ├── 2Stage/                       # Two-stage pipeline on VNLP2
│   └── E2E/                          # End-to-End YOLO-RVT on VNLP2
```

---

# Dataset Download

Due to storage limitations, all datasets used in our experiments are hosted via a single Google Drive link. The only exception is the AOLP dataset and Rodosol-alpr dataset, which must be downloaded from its official repository.

* All other datasets: https://drive.google.com/drive/folders/1G_hezDG_PC3aLRjFZMM2LFmwEAAtEoq-?usp=sharing

* AOLP Dataset: https://github.com/AvLab-CV/AOLP

* Rodosol-alpr Dataset: https://github.com/raysonlaroca/rodosol-alpr-dataset

This repository uses the following datasets:
| Dataset        | Description                                |
|----------------|--------------------------------------------|
| VNLP           | Vietnamese License Plate Dataset           |
| VNLP2          | Vietnamese License Plate Dataset 2         |
| AOLP           | Taiwan license plate dataset (AC, LE, RP)  | 
| Rodosol-alpr   | Brazilian License Plate Dataset            |
