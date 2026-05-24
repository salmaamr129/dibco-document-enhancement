# DIBCO Document Enhancement

Document image enhancement and preprocessing using the DIBCO dataset.

## Overview
This project focuses on document image processing using the DIBCO dataset.
The main objective is to improve the quality of degraded document images
through preprocessing and enhancement techniques to prepare them for
better readability and analysis.

## Dataset
The project uses the DIBCO dataset, which contains degraded handwritten
and printed document images.

- Original document images
- Ground truth (GT) images
- Noisy and degraded samples

## Pipeline
1. **Preprocessing** — Grayscale conversion, resize to 256×256, denoising, background normalization
2. **Filter Comparison** — Gaussian Blur, Median Filter, Sharpening
3. **Binarization** — Otsu, Adaptive Gaussian, Sauvola (best for documents)
4. **Morphological Refinement** — Opening and Closing to clean noise
5. **Evaluation** — Compare final result with Ground Truth

## Technologies
- Python, OpenCV, NumPy, Matplotlib
- Kaggle (runtime environment)
