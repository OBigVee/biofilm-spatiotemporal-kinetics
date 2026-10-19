# Spatiotemporal Biofilm Kinetics & Camera Stabilization Pipeline

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![OpenCV](https://img.shields.io/badge/OpenCV-Computer%20Vision-green.svg)](https://opencv.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)](https://jupyter.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

An end-to-end computer vision and image processing pipeline engineered to quantify macro-scale microbial biofilm expansion, fluid diffusion, and dynamic hydration kinetics on historic stone substrates at the **Lincoln Memorial (Washington, DC)**.

This repository automates sub-pixel camera stabilization, replaces manual operator-dependent thresholding (Fiji/ImageJ) with standardized computer vision algorithms, and extracts continuous time-series kinetics from macro-video sequences.

---

## 🔬 Scientific Motivation

1. **Camera Shift & Distortion:** Handheld and tripod recordings during fluid droplet application suffer from continuous translation, jitter, and angular drift, preventing direct pixel-for-pixel comparison.
2. **Subjective Manual Thresholding:** Traditional biological image processing in Fiji/ImageJ relies on arbitrary manual cutoffs, introducing significant operator bias.
3. **Static Snapshots vs. Dynamic Kinetics:** Standard before-and-after comparisons miss the dynamic rate of absorption, wetting fronts, and steady-state saturation curves.
4. **Spatial Heterogeneity:** Biofilm growth on stone substrates is non-uniform, requiring validation between whole-image measurements and regional sub-division matrices.

---

## 🛠️ Key Pipeline Capabilities

### 1. Sub-Pixel Feature Registration & Stabilization
* **SIFT (Scale-Invariant Feature Transform)** keypoint extraction combined with **Lowe’s Ratio Test** for robust correspondence.
* **RANSAC Rigid Affine Estimation:** Resolves multi-axis translation ($\Delta x, \Delta y$), rotation ($\theta$), and scale ($s$) across a 1,853-frame video sequence, successfully compensating for $>200\text{ px}$ of camera drift back to a static spatial coordinate system ($t_0$).

### 2. Multi-Method Biomass Segmentation
* **Fiji/ImageJ Benchmark Reproduction:** Replicates reference manual thresholds for baseline validation.
* **Inverse Otsu Global Thresholding:** Minimizes intra-class intensity variance to automatically distinguish dark biomass from bare stone substrate without human bias.
* **Triangle Algorithm:** Peak-to-tail geometric thresholding optimized for asymmetric intensity distributions.

### 3. Continuous Video Time-Series Extraction
* Samples video footage at **2 frames per second (every $0.5\text{s}$)** across a 61-second recording ($124$ continuous measurement points).
* Computes real-time coverage percentage $C(t)$, mean optical density, and instantaneous expansion velocity $\frac{dC}{dt}$ (% per second).

### 4. 8-Tile Spatial Diffusion Matrix
* Partitions stone surfaces into an identical $4 \times 2$ grid (8 equal rectangular ROIs).
* Evaluates spatial diffusion from the impact epicenter outwards and demonstrates mathematical consistency between whole-image analysis and regional averages ($<0.02\%$ discrepancy).

---

## 📊 Summary Findings

### Static Image Pair (Sample 3)

| Method | Threshold Before | Threshold After | Coverage Before (%) | Coverage After (%) | Absolute Change ($\Delta\%$) | Relative Increase (%) |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Manual Fiji Reference** | $0 - 48$ | $0 - 81$ | **17.66%** | **36.14%** | **+18.48%** | **+104.6%** |
| **Automated Otsu** | $\le 118$ | $\le 121$ | **39.08%** | **48.91%** | **+9.83%** | **+25.2%** |
| **Triangle Algorithm** | $\le 162$ | $\le 166$ | **51.97%** | **60.49%** | **+8.52%** | **+16.4%** |

### Continuous Video Time-Series Kinetics (`VIDEO-2026-10-01-00-40-57.mp4`)

* **Pre-Drop Baseline ($t = 10\text{s} - 30\text{s}$):** Stable dry biofilm coverage averaging **$27.09\%$**.
* **Droplet Contact Event ($t \approx 32\text{s} - 35\text{s}$):** Rapid influx of liquid; optical density drops sharply from $173$ to $110$.
* **Post-Drop Plateau ($t = 48\text{s} - 61\text{s}$):** Saturated steady-state coverage averaging **$46.47\%$**.
* **Net Dynamic Growth:** $\mathbf{+19.38\%}$ absolute expansion, corroborating the static high-resolution measurements ($+18.6\%$).

---

## 📁 Repository Structure

```text
├── biofilm_quantification.ipynb        # Static 2-image registration & multi-method segmentation
├── video_biofilm_timeseries.ipynb      # 61-second video stabilization & continuous kinetics pipeline
├── biomass_methods_comparison_s3.csv   # Segmentation comparison metrics (Fiji, Otsu, Triangle)
├── biomass_roi_results_s3.csv          # 8-tile sub-region breakdown for static images
├── video_biofilm_timeseries.csv        # 124-point time-series dataset (timestamps, offsets, coverage)
├── S3 Before the dropimage.TIF         # Raw high-resolution pre-drop reference TIFF
├── after the drop.tiff                 # Raw high-resolution post-drop reference TIFF
├── README.md                           # Documentation & research overview
└── .gitignore                          # Excludes heavy venv and proprietary PDF/PPT presentations
```

---

## 🚀 Getting Started

### Prerequisites
* Python 3.10+
* Virtual environment (`venv`)

### Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/<your-username>/<repo-name>.git
   cd <repo-name>
   ```

2. **Set up virtual environment:**
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   ```

3. **Install dependencies:**
   ```bash
   pip install opencv-python-headless scikit-image scipy numpy matplotlib pandas ipykernel Pillow
   ```

4. **Launch Jupyter:**
   ```bash
   jupyter notebook
   ```
   Open `biofilm_quantification.ipynb` or `video_biofilm_timeseries.ipynb` and run all cells.

---

## 📜 Citation & Academic Collaboration

This computational computer vision workflow was developed to support graduate research in computational and applied mathematics investigating microbial dynamics on historic cultural heritage sites.

---

## 📄 License
This project is licensed under the [MIT License](LICENSE).
