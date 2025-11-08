# Clustering Guided Residual Neural Networks for Multi-Tx Localization in Molecular Communications

> This repository contains the implementation code for multi-transmitter localization in molecular communication systems using clustering-guided residual neural networks.

## Table of Contents

- [Overview](#overview)
- [Paper Information](#paper-information)
- [Project Structure](#project-structure)
- [Dataset](#dataset)

---

## Letter Information

**Title**: Clustering Guided Residual Neural Networks for Multi-Tx Localization in Molecular Communications

**Focus Areas**:
- Multi-transmitter localization in diffusion-based molecular communication
- Integration of clustering algorithms with deep learning
- Evaluation across K=2, 3, and 4 transmitter configurations

**Key Contribution**: The work demonstrates that residual neural networks can effectively correct imperfect clustering estimates by learning correction patterns from limited simulation data, leading to significant improvements in localization accuracy.

---

## Project Structure

```
letter-repo/
├── README.md                          # This file
├── model_2tx.ipynb                    # K=2 transmitters pipeline
├── model_3tx.ipynb                    # K=3 transmitters pipeline
├── model_4tx.ipynb                    # K=4 transmitters pipeline
├── dataset/
│   ├── config.csv                     # Ground-truth TX configurations
│   ├── 0.csv                          # Particle trajectory data (scenario 0)
│   ├── 1.csv                          # Particle trajectory data (scenario 1)
│   ├── ...
│   ├── 9.csv                          # Particle trajectory data (scenario 9)
│   └── angle_results.zip              # Compressed angle estimation results
└── uniform_test/
    └── angle_results/
        └── all_results_enhanced.csv   # Wide format clustering results (input to convert_wide_to_long)
```

---

## Dataset

The dataset is located in the `dataset/` directory and contains:

### config.csv

Ground-truth configuration for all 10 simulation scenarios:

| Column | Description | Values |
|--------|-------------|--------|
| `step_time` | Simulation timestep | 1e-06 s |
| `radius` | Spherical receiver radius | 5.0 μm |
| `diffusion_coef` | Diffusion coefficient | 79.4 μm²/s |
| `tx_count` | Number of transmitters | K ∈ {2, 3, 4} |
| `tx_centers` | Ground-truth TX positions (3D) | List of K [x, y, z] coordinates in μm |
| `molecule_counts` | Particles emitted per TX | [10000, 10000, ...] (K values) |
| `rx_center` | Receiver position | [0, 0, 0] |
| `from` | Actual captured particle counts | Per-TX capture counts |

### Data CSVs (0.csv through 9.csv)

Individual particle trajectory data for each scenario:

| Column | Description |
|--------|-------------|
| `x, y, z` | 3D particle position in μm |
| `time` | Simulation time in seconds |
| `tx_index` | Source transmitter index (0 to K-1) |

**Note**: This dataset is a subset of the full dataset used in the paper. It provides representative scenarios for demonstrating the method. Dataset for 3 and 4 TXs are in the same format.

### uniform_test/angle_results/ (Clustering Results)

Directory containing clustering and angle estimation results processed by `convert_wide_to_long()`:

#### all_results_enhanced.csv (Input - Wide Format)

Raw clustering results in wide format. Each row represents a scenario with clustering method outputs (e.g., RAW, KMeans centers, ANGLE-ONLY, SIZE-ONLY, ANGLE+SIZE).

When processed by `convert_wide_to_long()`, this is converted to long format with the following columns:

| Column | Description |
|--------|-------------|
| `file_index` | Scenario index (0-9) |
| `tx_index` | Transmitter index (0 to K-1) |
| `method` | Clustering/correction method (e.g., "RAW", "KMeans centers", "ANGLE-ONLY", "SIZE-ONLY", "ANGLE+SIZE") |
| `est_size` | Estimated particle count for this TX |
| `est_center` | Estimated 3D center position [x, y, z] for this TX |
| `true_size` | Ground-truth particle count for this TX (filled from config.csv) |

The long format output is saved as `size_estimation_*Tx.csv` for further model training and evaluation.