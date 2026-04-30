# YOLO-RVT: End-to-End License Plate Recognition

## Mục đích / Purpose

Repository này chứa toàn bộ mã nguồn thực nghiệm (notebooks), trọng số mô hình (weights), và kết quả detection cho bài báo nghiên cứu **"End-to-End License Plate Recognition using YOLO-RVT"**. Cấu trúc được tổ chức để reviewer có thể dễ dàng tái thiết lập (reproduce) các thực nghiệm được báo cáo trong bài.

---

## Quy ước chung / Conventions

Mỗi thư mục thực nghiệm thường chứa:

| Thư mục | Nội dung |
|---|---|
| `Notebook/` | Jupyter Notebook (`.ipynb`) — mã huấn luyện & đánh giá mô hình |
| `Weight/` | File trọng số PyTorch (`.pth`) — checkpoint đã huấn luyện |
| `Detection/` | Notebook & kết quả cho bài toán phát hiện biển số (detection-only) |

- **Checkpoint 1 (chk1)**: kết quả huấn luyện giai đoạn 1 (ví dụ: 27 epoch)
- **Checkpoint 2 (chk2)**: kết quả huấn luyện giai đoạn 2 (ví dụ: 54 epoch)

---

## Cấu trúc thư mục / Directory Structure

```
Final/
├── MainResult/          # Kết quả chính — so sánh các biến thể YOLO-RVT trên VNLP1
├── Ablation/            # Thí nghiệm ablation study
├── AOLP/                # Thực nghiệm trên bộ dữ liệu AOLP (Đài Loan)
├── Rodosol/             # Thực nghiệm trên bộ dữ liệu RodoSol-ALPR (Brazil)
├── VNLP2/               # Thực nghiệm trên bộ dữ liệu VNLP2 (Việt Nam)
└── README.md            # File này
```

---

## 1. `MainResult/` — Kết quả chính

So sánh hiệu suất các biến thể backbone × recurrent head trên bộ dữ liệu **VNLP1**.

```
MainResult/
├── V8S_GRU/             # YOLOv8s + GRU head
│   ├── Notebook/        #   └── T_V8S_GRU_VNLP1_Checkpoint{1,2}.ipynb
│   └── Weight/          #   └── final_V8S_GRU_chk{1,2}.pth
├── V8S_LSTM/            # YOLOv8s + LSTM head
├── V10S_GRU/            # YOLOv10s + GRU head
├── V10S_LSTM/           # YOLOv10s + LSTM head
├── V11S_GRU/            # YOLOv11s + GRU head
├── V11S_LSTM/           # YOLOv11s + LSTM head
├── Detection/           # Baseline detection-only (không nhận dạng ký tự)
│   ├── V8s/             #   └── yolov8s detection notebook + results/
│   ├── V10s/            #   └── yolov10s detection notebook + results/
│   └── V11s/            #   └── yolov11s detection notebook + results/
└── 2Stage/              # Pipeline 2 giai đoạn (Detect → OCR riêng biệt)
    ├── Notebook/        #   └── 2stage_v11s_gru_checkpoint{1,2}.ipynb
    └── Weight/          #   └── final_2STAGE_V11S_GRU_chk{1,2}.pth
```

**Ý nghĩa:**
- `V{8,10,11}S_{GRU,LSTM}/`: Từng biến thể end-to-end YOLO-RVT, kết hợp backbone YOLOv8s/v10s/v11s với recurrent head GRU hoặc LSTM.
- `Detection/`: Chỉ chạy bài toán phát hiện biển số (không nhận dạng ký tự), dùng làm **baseline detection**.
- `2Stage/`: Pipeline hai giai đoạn truyền thống (phát hiện trước, nhận dạng sau) — dùng để so sánh với phương pháp end-to-end.

---

## 2. `Ablation/` — Ablation Study

Đánh giá ảnh hưởng của từng thành phần trong kiến trúc YOLO-RVT. Mỗi thư mục con tương ứng với **một thực nghiệm loại bỏ/thay đổi một thành phần**.

```
Ablation/
├── WithoutCrossAttention/   # Bỏ module Cross-Attention
│   ├── Notebook/
│   └── Weight/
├── WithoutRecurrentHead/    # Bỏ Recurrent Head (GRU/LSTM)
│   ├── Notebook/
│   └── Weight/
├── WithoutAugmention/       # Bỏ Data Augmentation
│   ├── Notebook/
│   └── Weight/
├── DecreaseViT/             # Giảm kích thước Vision Transformer
│   ├── Notebook/
│   └── Weight/
└── FreeZoneBackbone/        # Đóng băng (freeze) backbone, không fine-tune
    ├── Notebook/
    └── Weight/
```

**Ý nghĩa:**
- Mỗi thư mục loại bỏ hoặc thay đổi **đúng một yếu tố** so với mô hình đầy đủ, giúp đánh giá đóng góp riêng lẻ của từng thành phần.

---

## 3. `AOLP/` — Thực nghiệm trên AOLP Dataset

Đánh giá mô hình trên bộ dữ liệu **AOLP** (Application-Oriented License Plate) gồm 3 tập con:

```
AOLP/
├── AC/                  # Access Control (kiểm soát ra vào)
│   ├── Detection/       #   └── yolov11s_ac.ipynb + results/
│   ├── Notebook/        #   └── t-yolo-rvt-aolp-ac.ipynb
│   └── Weight/          #   └── final_V11S_GRU_AC.pth
├── LE/                  # Law Enforcement (giao thông)
│   ├── Detection/
│   ├── NoteBook/
│   └── Weight/
└── RP/                  # Road Patrol (tuần tra đường)
    ├── Detection/
    ├── NoteBook/
    └── Weight/
```

**Ý nghĩa:**
- Mỗi tập con (AC, LE, RP) đại diện cho một kịch bản ứng dụng khác nhau.
- `Detection/`: Kết quả detection-only trên từng tập con.
- `Notebook/`: Notebook huấn luyện & đánh giá YOLO-RVT end-to-end.
- `Weight/`: Trọng số tốt nhất trên từng tập con.

---

## 4. `Rodosol/` — Thực nghiệm trên RodoSol-ALPR Dataset

Đánh giá mô hình trên bộ dữ liệu **RodoSol-ALPR** (biển số Brazil) với hai chiến lược huấn luyện:

```
Rodosol/
├── Detection/                   # Baseline detection trên RodoSol
│   ├── yolo11s.ipynb
│   └── results/
├── Pretrained_108ep_VNLP/       # Transfer Learning từ VNLP pretrained weights
│   ├── Notebook/                #   └── rodosol-alpr-dataset_chkpoint{1,2,3}.ipynb
│   └── Weight/                  #   └── final_V11S_GRU_Rodosol_Pretrain_{100,200,300}epoch.pth
└── Scatch_Fusion_6+13/          # Training from Scratch với Fusion (6+13 classes)
    ├── Notebook/                #   └── V11S_GRU_Rodosol_Scratch_Fusion_chk{1,2,3}.ipynb
    └── Weight/                  #   └── final_V11S_GRU_Rodosol_Scratch_Fusion_{100,200,300}epoch.pth
```

**Ý nghĩa:**
- `Pretrained_108ep_VNLP/`: Sử dụng **Transfer Learning** — khởi tạo trọng số từ mô hình đã huấn luyện trên VNLP, sau đó fine-tune trên RodoSol.
- `Scatch_Fusion_6+13/`: Huấn luyện **từ đầu (from scratch)** với chiến lược fusion label (gộp 6 + 13 lớp ký tự).
- Checkpoint ở 100, 200, 300 epoch để theo dõi quá trình hội tụ.

---

## 5. `VNLP2/` — Thực nghiệm trên VNLP2 Dataset

Đánh giá mô hình trên bộ dữ liệu **VNLP2** (biển số Việt Nam, phiên bản 2):

```
VNLP2/
├── 2Stage/              # Pipeline 2 giai đoạn trên VNLP2
│   ├── Notebook/        #   └── t-yolo-rvt-datasetvn2.ipynb
│   └── Weight/          #   └── final_2Stage_V11S_GRU_VNLP2.pth
└── E2E/                 # End-to-End YOLO-RVT trên VNLP2
    ├── Notebook/        #   └── t-yolo-rvt-datasetvn2.ipynb
    └── Weight/          #   └── final_E2E_V11S_GRU_VNLP2.pth
```

**Ý nghĩa:**
- So sánh phương pháp **End-to-End** vs **2-Stage** trên cùng bộ dữ liệu VNLP2.

---

## Định dạng file / File Formats

| Đuôi file | Mô tả |
|---|---|
| `.ipynb` | Jupyter Notebook — chứa code huấn luyện, đánh giá, và kết quả thực nghiệm |
| `.pth` | PyTorch checkpoint — trọng số mô hình đã huấn luyện |
| `.pt` | YOLO pretrained weights (dùng cho baseline detection) |
| `data.yaml` | Cấu hình dataset cho YOLO detection (đường dẫn, số lớp, tên lớp) |
| `results/` | Thư mục chứa kết quả huấn luyện detection (metrics, plots) |

---

## Hướng dẫn tái thiết lập thực nghiệm / Reproducing Experiments

1. **Cài đặt môi trường:**
   - Python 3.8+
   - PyTorch với CUDA support
   - Ultralytics YOLO (`pip install ultralytics`)
   - Jupyter Notebook

2. **Chạy thực nghiệm:**
   - Mở file `.ipynb` tương ứng trong thư mục `Notebook/`.
   - Cập nhật đường dẫn dataset trong notebook nếu cần.
   - Chạy toàn bộ notebook để tái tạo kết quả.

3. **Sử dụng trọng số đã huấn luyện:**
   - Load file `.pth` từ thư mục `Weight/` để đánh giá mà không cần huấn luyện lại.

4. **Detection baseline:**
   - Mở notebook trong thư mục `Detection/` và chạy với pretrained YOLO weights (`.pt`).
