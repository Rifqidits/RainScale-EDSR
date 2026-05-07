# RainScale-EDSR

2.5× spatial super-resolution of daily rainfall over Java Island using an Enhanced Deep Super-Resolution (EDSR) CNN — bridging coarse GCM output to operational hydrology precision.

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=flat&logo=python&logoColor=white)
![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-FF6F00?style=flat&logo=tensorflow&logoColor=white)
![License](https://img.shields.io/badge/License-Custom-lightgrey?style=flat)

---

## Overview

Precipitation data from global climate models (GCMs) is typically available at coarse spatial resolutions (~0.25°), which is insufficient for local hydrological planning and flood risk assessment. RainScale-EDSR applies a super-resolution CNN to upscale daily rainfall grids by a factor of **2.5×** (0.25° → 0.1°) over Java Island.

The model is trained on **5,113 frames** from MSWEP V2 observational data and evaluated on **1,096 held-out test frames**, achieving state-of-the-art downscaling accuracy.

---

## Results

| Metric | EDSR (ours) | ResNet-SR (baseline) |
|--------|-------------|----------------------|
| RMSE (mm/day) | **1.71** | 2.14 |
| R² | **0.967** | 0.941 |
| KGE | **0.978** | 0.952 |
| PSNR (dB) | **32.4** | 29.8 |

---

## Model Architecture

EDSR with 8 residual blocks adapted for single-channel atmospheric data:

```
Input (0.25° grid, 1-channel)
  └── Conv2D(64, 3×3)
      └── × 8 Residual Blocks [Conv2D → ReLU → Conv2D → Add]
          └── Conv2D(64, 3×3)              # global residual add
              └── Conv2DTranspose(64, 3×3, strides=2)  # 2× upsampling
                  └── Conv2D(1, 3×3)       # output (0.1° grid)
```

A **ResNet-SR** baseline with identical block count was trained in parallel for comparison.

---

## Preprocessing Pipeline

Raw MSWEP V2 data undergoes a 5-stage pipeline before training:

1. Log-ε transform (handles zero-inflation in precipitation)
2. Stratified RobustScaling by rain-intensity tier
3. Outlier clipping (99.5th percentile)
4. Spatial feature extraction (Sobel edge + Gaussian blur channels)
5. MinMax normalization to [0, 1]

---

## Repository Structure

```
RainScale-EDSR/
├── myLibrary.py                                  # Custom metrics: RMSE, R², KGE, PSNR, SSIM
├── preprocessing.ipynb                           # Full 5-stage preprocessing pipeline
├── runEDSR_JI_N_I025-Conv2D-AddEval.ipynb       # Model training & evaluation
├── LICENSE
└── README.md
```

---

## Getting Started

### Prerequisites

- Python 3.10+
- TensorFlow 2.x
- Access to MSWEP V2 data (see Dataset note below)

### Installation

```bash
git clone https://github.com/Rifqidits/RainScale-EDSR.git
cd RainScale-EDSR
pip install -r requirements.txt
```

### Usage

1. Run `preprocessing.ipynb` to prepare the rainfall grids
2. Run `runEDSR_JI_N_I025-Conv2D-AddEval.ipynb` to train and evaluate the model
3. Custom metrics (RMSE, R², KGE, PSNR, SSIM) are available in `myLibrary.py`

---

## Dataset

| Source | Resolution | Coverage | Role |
|--------|-----------|----------|------|
| MSWEP V2 | 0.1° | Java Island | High-res ground truth |
| NEX-GDDP CMIP6 (EC-Earth3) | 0.25° | Indonesia | Low-res GCM input |

The dataset is **not included** in this repository due to legal and licensing restrictions. Please contact the author for academic access.

---

## License

Custom academic license — see [LICENSE](LICENSE) for details. Code is made available for educational and non-commercial academic use. Attribution is required.
