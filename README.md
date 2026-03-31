# Cavendish Tracking & Analysis

This repo contains a cleaned Python workflow for tracking a Cavendish-style torsion balance from video and reproducing the downstream analysis that was originally in C++.

## Contents
- `expCavendish_clean.ipynb`: video-tracking notebook. Loads `periodo.avi`, detects the moving marker inside a configurable ROI, writes an annotated video (`periodo_track.avi`), and exports pixel and calibrated coordinates (`distanze_prova_periodo*.txt`). Calibration uses four dots mapped to physical coordinates (example: 122 cm × 42 cm rectangle). Update color thresholds and ROIs if your markers change.
- `data_analysis.ipynb`: analysis notebook. Loads tracked points (default `points2.txt`), computes radius and angle, averages every 10 samples, fits a damped cosine with SciPy, and plots residuals. Saves fit parameters to `fit_parameters.json`.

## How to run
1. Open the notebooks in VS Code or Jupyter with a Python 3 environment that has `opencv-python`, `numpy`, `matplotlib`, and `scipy` installed.
2. In `expCavendish_clean.ipynb`, set paths/thresholds in the config cell, then run all cells to generate `periodo_track.avi` and coordinate text files.
3. In `data_analysis.ipynb`, set the input points file (e.g., `distanze_prova_periodo_calibrated.txt` or `points2.txt`) and rerun to recompute plots and fit results.

## Notes
- The tracking ROI and color thresholds are conservative; tweak them if frames are missed.
- The damped oscillation model is `p0 + p1 * exp(-p3 * t) * cos(p2 * t + p4)`; uncertainties come from the covariance matrix of the fit.
- If calibration detects fewer than four dots, adjust `roi_calibration` or the color ranges and rerun.
