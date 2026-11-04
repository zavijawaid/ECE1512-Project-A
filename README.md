# ECE1512 Project A – Attention-Based Multiple Instance Learning (ABMIL)

This repository contains the implementation and evaluation of ABMIL for whole-slide image (WSI) classification. The project evaluates the baseline ABMIL architecture and investigates a research extension that incorporates critical-instance-guided attention, inspired by the Dual-Stream Multiple Instance Learning (DSMIL) framework.

The models are trained and evaluated on three histopathology datasets: **CAMELYON16, CAMELYON17, and BRACS**.

## 1. Project Overview

Whole-slide histopathology images are typically very large, making it computationally expensive to process them directly. MIL addresses this problem by representing each slide as a collection of smaller image patches.

The baseline ABMIL model uses an attention mechanism to assign importance weights to individual patches before combining their features to produce a slide-level prediction.

The proposed research extension improves this process by identifying important patches, referred to as critical instances, and using their similarities to other patches to guide attention.

## 2. Repository Structure

```text
ECE1512-Project-A/
│
├── config/
│   └── Configuration files
│
├── dataset_csv/
│   └── Dataset split and metadata CSV files
│
├── critical_instance_results/
│   └── Saved evaluation results in JSON format
│
├── ECE1512_ProjectA_Student.ipynb
│   └── Baseline ABMIL implementation
│
├── ECE1512_ProjectA_Research_Extension.ipynb
│   └── Critical-instance-guided ABMIL implementation
│
├── dataloader.py
│   └── Dataset loading and preprocessing
│
├── utils.py
│   └── Utility functions
│
├── wsi_visualization.py
│   └── Whole-slide image visualization utilities
│
├── requirements.txt
│   └── Python dependencies
│
└── README.md
```

## 3. Installation and Setup

Download the required .h5 feature files from the Zenodo website:

https://zenodo.org/records/23130543

Place the HDF5 file under the path expected by your YAML configuration in the `config/`, or edit data_dir in the YAML file. [3]

## 4. Running the Experiments

The experiments are implemented using Jupyter notebooks and can be executed locally or through Google Colab.

### 4.1 Baseline ABMIL

Open:

```text
ECE1512_ProjectA_Student.ipynb
```

This notebook implements the original ABMIL architecture and provides baseline results for comparison.

To run the baseline:

1. Install the required dependencies.
2. Configure the dataset and feature directory paths.
3. Select the dataset to evaluate.
4. Run the notebook cells in order.
5. Train the baseline ABMIL model.
6. Evaluate the selected checkpoint on the test set.

The baseline model consists of feature dimensionality reduction, gated attention-based aggregation, and a fully connected classifier.

### 4.2 Critical-Instance-Guided ABMIL Extension

Open:

```text
ECE1512_ProjectA_Research_Extension.ipynb
```

This notebook implements the proposed research extension.

To reproduce the experiments:

1. Configure the dataset and feature directory paths.
2. Select the dataset using the `DATASET` variable.
3. Load the corresponding training, validation, and test data.
4. Initialize the critical-instance-guided ABMIL model.
5. Train the model for 30 epochs.
6. Select the best checkpoint using validation AUROC and F1-score.
7. Evaluate the selected checkpoint on the held-out test set.
8. Save the evaluation results for comparison against the baseline.

The dataset selection variable can be set to:

```python
DATASET = 'C16'     # CAMELYON16
DATASET = 'C17'     # CAMELYON17
DATASET = 'BRACS'   # BRACS
```

Run the notebook separately for each dataset to reproduce the three experiments.

### Training Configuration

The following settings are used for the research extension:

| Parameter | Value |
|---|---|
| Training epochs | 30 |
| Auxiliary loss weight | 0.25 |
| Checkpoint selection | Validation AUROC + F1-score |
| Evaluation datasets | C16, C17, BRACS |
| Evaluation metrics | Accuracy, AUROC, F1-score, Cross-entropy loss |

The best-performing checkpoint is selected based on validation performance rather than simply using the model from the final training epoch.

## 5. Model Evaluation

Both the baseline and extended ABMIL models are evaluated using the same dataset splits and evaluation metrics.

The following metrics are reported:

- **Accuracy:** Percentage of correctly classified slides.
- **AUROC:** Measures how well the model separates different classes.
- **F1-score:** Balances precision and recall.
- **Cross-entropy loss:** Measures the difference between predicted probabilities and the actual labels.

The extension is compared against the baseline to determine whether critical-instance-guided attention improves slide-level classification.

## 6. Saved Results

Evaluation summaries for the research extension are stored in:

```text
critical_instance_results/
```

The results are saved as JSON files, with one file corresponding to each dataset.

```text
critical_instance_results_C16.json
critical_instance_results_C17.json
critical_instance_results_BRACS.json
```

Each JSON file contains:

- Dataset name
- Auxiliary loss weight
- Selected best epoch
- Baseline evaluation metrics
- Research extension evaluation metrics

These files allow the performance of the baseline and extended models to be compared without retraining.

## 7. Code Reproducibility Checklist

This repository and the project report are separate artifacts. The report includes its own reproducibility checklist in Appendix A. Following the Machine Learning Reproducibility Checklist guidelines:

- [x] **Training code:** The baseline ABMIL and critical-instance-guided ABMIL implementations are provided in their respective Jupyter notebooks.
- [x] **Evaluation code:** Both implementations include evaluation procedures for accuracy, AUROC, F1-score, and cross-entropy loss.
- [x] **Dependencies:** Required Python packages are listed in `requirements.txt`.
- [x] **Reproduction instructions:** The README provides instructions for downloading the datasets, configuring file paths, and running the experiments.
- [x] **Data:** Pre-extracted HDF5 feature files are available through Zenodo, with dataset configurations provided in `config/` and dataset split files in `dataset_csv/`.
- [x] **Model selection:** The best checkpoint is selected using the sum of validation AUROC and F1-score.
- [x] **Experimental results:** Baseline and extension results are saved in JSON files under `critical_instance_results/`.
- [x] **Training configuration:** Both models are trained for 30 epochs, with an auxiliary loss weight of 0.25 used for the proposed extension.

## 8. Notes

- The models use pre-extracted patch embeddings rather than processing raw whole-slide images during training.
- Original WSI files and extracted feature files must be provided separately.
- Dataset paths must be configured before running the notebooks.
- A GPU is recommended to accelerate model training.
- The baseline and research extension should be evaluated using the same dataset splits for a fair comparison.

## 9. References

**[1]** M. Ilse, J. Tomczak, and M. Welling, "Attention-based Deep Multiple Instance Learning," *Proceedings of the 35th International Conference on Machine Learning (ICML)*, vol. 80, pp. 2127–2136, 2018.

**[2]** B. Li, Y. Li, and K. W. Eliceiri, "Dual-stream Multiple Instance Learning Network for Whole Slide Image Classification With Self-supervised Contrastive Learning," *Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)*, pp. 14318–14328, 2021.

**[3]** University of Toronto, "ECE1512 Project A: Starter Code," ECE1512 course materials, University of Toronto, Toronto, ON, Canada, 2026. [Jupyter Notebook].
