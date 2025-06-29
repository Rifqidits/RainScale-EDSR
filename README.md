# RainScale-EDSR
The demand for high-resolution precipitation forecasts is increasing, particularly to support climate and hydrological studies at the local scale. The proposed model adopts the Enhanced Deep Super Resolution (EDSR) architecture to perform spatial downscaling from a resolution of 0.25° to 0.1°.

---

## Objective
To develop a CNN-based super-resolution framework that improves the spatial resolution of precipitation datasets using:
- **Low-resolution input** from simulated or interpolated GCMs  
- **High-resolution ground-truth** from observational data (MSWEP)  
- **EDSR architecture** for reconstructing high-quality outputs

---

## Model Architecture
We adopt a modified version of the **EDSR** architecture containing:
- 19 Residual Blocks
- Conv2D layers (ReLU + Linear)
- Final reconstruction layer with upsampling
- Custom evaluation metrics: RMSE, R², PSNR, SSIM, KGE

---

## Dataset Sources
- **MSWEP V2 (Multi-Source Weighted-Ensemble Precipitation)**  
  Observational rainfall dataset at 0.1° resolution.
- **NEX-GDDP CMIP6: EC-Earth3**  
  Downscaled GCM outputs at 0.25°, covering Indonesia.
For now, the dataset used in this project is still under review and cannot be published until approved. While the dataset remains private, all code is owned by the author and is made publicly available for learning, experimentation, and non-commercial academic use. Attribution is appreciated.

---

This project aims to contribute toward more accurate and localized climate insights through deep learning. Your feedback, suggestions, or collaborations are warmly welcome.
