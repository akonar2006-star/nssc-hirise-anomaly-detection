# Unsupervised Anomaly Detection in Mars HiRISE Orbital Imagery

**NSSC 2026 — Data Analytics Challenge | Team SAGE**

An unsupervised anomaly-detection project for Mars Reconnaissance Orbiter HiRISE image crops. The documented approach combines a convolutional autoencoder trained from scratch with Isolation Forest novelty scoring and reconstruction-error analysis.

> **Repository status:** This ZIP is assembled from the project report supplied with the submission. The report states that the full submission also contains an executed notebook and `MODEL_ITERATION_LOG.md`; those files were not included in the uploaded material, so they are not fabricated here.

## Team

- Priyasmita Chakraborty
- Arnabi Konar
- Niharika Jha

**Event:** NSSC 2026, Data Analytics Challenge  
**Organization:** Space Technology Students' Society, IIT Kharagpur

## Project highlights

| Metric | Result |
|---|---:|
| Image crops analysed | 10,422 |
| Source images | 172 |
| Final latent dimension | 256 |
| Trainable parameters | 7,830,689 |
| Final-epoch SSIM | 0.696 |
| MAD-rule threshold | 0.4556 |
| Crops flagged by MAD | 1,964 (18.8%) |
| Metadata join failures | 0 / 10,422 |

## Methodology

1. **Deep latent compression**  
   A symmetric convolutional autoencoder is trained from scratch. The loss is:
   `0.7 × MSE + 0.3 × (1 − SSIM)`.

2. **Novelty scoring**  
   A 300-tree Isolation Forest is fitted on the 256-dimensional latent vectors. The score sign is reversed so that a higher novelty score means a more anomalous sample.

3. **Data-driven thresholding**  
   Two thresholding methods are evaluated:
   - Median + `3 × 1.4826 × MAD`
   - Elbow detection on the sorted novelty-score curve

   The report adopts the MAD rule because it is more conservative.

4. **Interpretability**  
   Pixel-wise reconstruction-error heatmaps are inspected for the highest-novelty crops.

5. **Iteration / versioning**  
   The report documents three iterations covering reconstruction quality, output-size handling, and metadata fusion.

## Main finding

The highest-novelty crops are strongly affected by a black crop-boundary / splice artifact. Their reconstruction-error heatmaps show concentrated linear or triangular error along the crop edge. Therefore, the report recommends masking or excluding the boundary region before using the novelty ranking as a geological-anomaly shortlist.

## Repository structure

```text
nssc-hirise-anomaly-detection/
├── README.md
├── REPOSITORY_MANIFEST.md
├── requirements.txt
├── .gitignore
├── NSSC_Project_Report.pdf
├── docs/
│   └── NSSC_Project_Report.pdf
├── notebooks/
│   └── README.md
├── src/
│   └── README.md
└── results/
    └── README.md
```

The `notebooks/`, `src/`, and `results/` directories contain README placeholders only in this package because the corresponding executable notebook, source code, and result artifacts were not supplied.

## Recommended future work

- Mask the black fill / crop-boundary region before reconstruction-error scoring.
- Add a held-out validation split.
- Investigate source images with unusually high flag rates.
- Re-rank the anomaly candidates after boundary-artifact filtering.

## Reproducibility note

The report describes a run on the real 10,422-crop dataset. The dataset itself is not bundled in this repository ZIP.

## Reference repository style

This repository is organized in the same spirit as the provided example repository, with a clear README, manifest, dependency file, ignore rules, and separated project areas.

Example: https://github.com/LovelyDev-06/ttc_slm
