# MedFairXNet

## Explainable, Uncertainty-Aware and Fairness-Oriented Deep Learning Framework for Melanoma Detection

---

## About the Project

MedFairXNet is a research project focused on computer-aided melanoma detection from dermoscopic images. The study develops and evaluates a proposed hybrid CNN–Transformer architecture, **MedFairXNet**, against five established deep-learning benchmark models across three dermoscopic datasets.

The project combines melanoma classification with lesion preprocessing and segmentation, explainability, uncertainty estimation, calibration, subgroup analysis, external evaluation, and architectural ablation.

The research does **not assume that the proposed model must outperform every benchmark**. Model performance is analysed using the experimental results obtained from the implemented research workflow.

---

## Research Objective

The primary objective of this research is to develop and evaluate a multi-dataset deep-learning framework for melanoma detection and to compare the proposed MedFairXNet architecture against established CNN and Transformer-based models under a common experimental framework.

The study does **not assume that the proposed model must outperform every benchmark**. Model performance is analysed using the experimental results obtained from the implemented research workflow.

---

## Datasets

Three dermoscopic datasets are used in the research.

### 1. HAM10000

HAM10000 is a large multi-source dermoscopic image dataset containing **10,015 images**.

**Dataset source:**

https://www.kaggle.com/datasets/kmader/skin-cancer-mnist-ham10000

The executed experiment uses HAM10000 for model development and held-out evaluation.

### Dataset Split

| Partition | Images |
|---|---:|
| Training | 7,000 |
| Validation | 1,505 |
| Held-out Test | 1,510 |
| **Total** | **10,015** |

This corresponds approximately to a **70% / 15% / 15%** split.

---

### 2. PH²

PH² is a dermoscopic image dataset developed for lesion segmentation and classification research.

**Dataset source:**

https://www.kaggle.com/datasets/spacesurfer/ph2-dataset

The evaluated dataset contains **200 images**, including melanoma and non-melanoma cases.

PH² is used as an independent dataset for external evaluation.

---

### 3. ISIC 2018

The ISIC 2018 dataset is used as an independent external evaluation dataset.

**Official source:**

https://challenge.isic-archive.com/data/

The ISIC 2018 Task 3 test set contains **1,512 images** used in the reported external evaluation.

---

## Experimental Design

The datasets are not all divided using the same train/validation/test procedure.

The experimental design is:

```text
                         HAM10000
                            |
             +--------------+--------------+
             |              |              |
             v              v              v
         Training       Validation       Test
          7,000           1,505          1,510
             |
             v
        Model Training
             |
             v
       Model Evaluation
             |
             +-----------------------------+
                                           |
                         +-----------------+-----------------+
                         |                                   |
                         v                                   v
                    ISIC 2018                              PH²
                  External Test                        External Test
```

HAM10000 is used for model development and held-out evaluation, while ISIC 2018 and PH² are used as independent evaluation datasets.

---

## Models

Six models are evaluated.

| Model | Architecture |
|---|---|
| DenseNet121 | Dense CNN |
| ResNet101 | Residual CNN |
| EfficientNetV2-S | Efficient CNN |
| ConvNeXt-Tiny | Modern CNN |
| Swin Transformer | Hierarchical Transformer |
| **MedFairXNet** | **Hybrid CNN–Transformer — Proposed** |

The benchmark models represent established CNN, efficient CNN, modern convolutional, and Transformer-based architectures.

---

## Proposed Model: MedFairXNet

MedFairXNet is the proposed hybrid CNN–Transformer architecture developed in this research.

The implemented architecture combines:

- EfficientNetV2-S
- Swin-T Transformer
- Feature projection
- Multi-head cross-attention
- Lesion attention
- Feature-pyramid processing
- Multi-scale Transformer processing
- Melanoma classification
- Uncertainty estimation
- Fairness-oriented representation components

The implemented model contains approximately **65.3 million parameters**.

The research does not assume that MedFairXNet is superior to the benchmark models. Its performance is evaluated experimentally against all five benchmark architectures.

---

## Research Pipeline

```text
Dermoscopic Images
        |
        v
Image Preprocessing
        |
        v
Watershed Localization
        |
        v
U-Net Lesion Segmentation
        |
        v
Feature Representation
        |
   +----+----+
   |         |
   v         v
EfficientNetV2-S   Swin-T
   |         |
   +----+----+
        |
        v
Cross Attention
        |
        v
Lesion Attention
        |
        v
Feature Pyramid
        |
        v
Multi-scale Transformer
        |
        v
Melanoma Classification
        |
   +----+----+
   |         |
   v         v
Uncertainty  Explainability
        |
        v
Calibration & Subgroup Analysis
        |
        v
External Evaluation
```

The research pipeline integrates image preprocessing, lesion localization, segmentation, feature extraction, hybrid classification, explainability, uncertainty and external evaluation.

---

## Preprocessing and Segmentation

The implemented preprocessing workflow includes:

- Image resizing
- Hair removal
- 3×3 median filtering
- CLAHE
- Colour normalization
- Watershed-based lesion localization

A U-Net-based lesion segmentation component is used to identify the lesion region before classification.

The segmentation stage is evaluated using:

- Dice Score
- Intersection over Union (IoU)
- Boundary Accuracy
- Hausdorff Distance

---

## Evaluation Metrics

The six classification models are evaluated using multiple complementary metrics:

- Accuracy
- Precision
- Sensitivity
- Specificity
- F1-score
- ROC-AUC
- PR-AUC
- Brier Score
- Expected Calibration Error (ECE)
- Uncertainty analysis

Higher values are generally preferred for accuracy, precision, sensitivity, specificity, F1-score, ROC-AUC and PR-AUC.

Lower values indicate better performance for Brier Score and ECE.

### Metric Interpretation

| Metric | Meaning | Preferred Direction |
|---|---|---|
| Accuracy | Overall proportion of correct predictions | Higher |
| Precision | Reliability of positive predictions | Higher |
| Sensitivity | Ability to detect melanoma cases | Higher |
| Specificity | Ability to correctly identify non-melanoma cases | Higher |
| F1-score | Balance between precision and sensitivity | Higher |
| ROC-AUC | Overall discrimination ability | Higher |
| PR-AUC | Precision–recall performance | Higher |
| Brier Score | Probability prediction error | Lower |
| ECE | Calibration error | Lower |

---

## Main Experimental Results

The executed three-dataset ROC-AUC comparison produced the following results:

| Rank | Model | HAM10000 | ISIC 2018 | PH² | Mean AUC |
|---:|---|---:|---:|---:|---:|
| 1 | **Swin Transformer** | 0.770 | 0.872 | 0.900 | **0.847** |
| 2 | ConvNeXt-Tiny | 0.737 | 0.849 | 0.930 | **0.839** |
| 3 | EfficientNetV2-S | 0.741 | 0.761 | 0.809 | **0.771** |
| 4 | MedFairXNet | 0.687 | 0.740 | 0.844 | **0.757** |
| 5 | DenseNet121 | 0.758 | 0.758 | 0.734 | **0.750** |
| 6 | ResNet101 | 0.734 | 0.286 | 0.405 | **0.475** |

Based on mean ROC-AUC across the three evaluated datasets, **Swin Transformer demonstrated the strongest overall discrimination** in the executed experiment.

MedFairXNet is therefore not presented as the overall performance winner.

The results are reported according to the observed experimental outcomes.

---

## Dataset-Level Comparison

The results demonstrate that model performance varies across datasets.

### HAM10000

The evaluated models show different trade-offs between sensitivity, specificity, accuracy and ROC-AUC.

MedFairXNet demonstrates strong accuracy and specificity, while Swin Transformer demonstrates stronger sensitivity and ROC-AUC.

### ISIC 2018

Swin Transformer provides the strongest ROC-AUC among the evaluated models.

MedFairXNet demonstrates relatively strong specificity and precision at the evaluated threshold, but lower sensitivity.

### PH²

ConvNeXt-Tiny achieves the highest reported ROC-AUC among the evaluated models.

Swin Transformer also performs strongly, while MedFairXNet shows strong specificity but limited sensitivity at the evaluated threshold.

The differences across datasets demonstrate why model quality should not be judged using a single dataset or a single metric.

---

## Segmentation Results

The executed lesion segmentation analysis reports the following results:

| Dataset | Dice Score | IoU | Boundary Accuracy | Hausdorff Distance |
|---|---:|---:|---:|---:|
| HAM10000 | 0.878 | 0.807 | 0.585 | 20.02 px |
| PH² | 0.830 | 0.741 | 0.283 | 30.15 px |

Dice Score and IoU measure the spatial overlap between the predicted lesion region and the reference lesion mask.

Boundary Accuracy evaluates agreement around lesion boundaries.

Hausdorff Distance measures the maximum boundary discrepancy, where lower values indicate smaller maximum boundary errors.

---

## Calibration Analysis

Calibration evaluates whether a model's predicted probabilities correspond appropriately to the observed outcomes.

Two calibration metrics used in the research are:

- Brier Score
- Expected Calibration Error (ECE)

Lower values indicate better calibration.

For HAM10000, MedFairXNet achieved:

| Metric | MedFairXNet |
|---|---:|
| ECE | 0.116 |
| Brier Score | 0.136 |

These results indicate favourable probability calibration for MedFairXNet on HAM10000, despite its lower overall ROC-AUC compared with the strongest benchmark models.

This demonstrates that discrimination and calibration represent different properties of a predictive model.

---

## Uncertainty Analysis

The research investigates whether prediction uncertainty is associated with classification errors.

The executed uncertainty analysis reports:

| Measure | Result |
|---|---:|
| Lowest uncertainty quartile error | 5.82% |
| Highest uncertainty quartile error | 39.68% |
| Chi-square statistic | 181.817763 |
| Degrees of freedom | 3 |
| p-value | 3.57 × 10⁻³⁹ |
| Ensemble probability SD | 0.054022 |
| Temperature-scaled entropy | 0.570029 |

The observed error rate increases substantially from the lowest to the highest uncertainty quartile.

The analysis therefore indicates a strong association between prediction uncertainty and classification error in the evaluated experiment.

---

## Explainable AI

The research incorporates several explainability techniques to investigate the visual regions contributing to model predictions.

The implemented approaches include:

- Grad-CAM
- LIME
- SHAP
- Attention visualization
- Occlusion analysis

These methods are used to examine model behaviour for different prediction cases, including correct predictions, incorrect predictions and uncertain predictions.

The explainability results are treated as interpretability evidence and are not presented as proof that the generated explanations are clinically faithful.

---

## Fairness-Related Analysis

The research includes fairness-oriented analysis as part of the proposed framework.

The original research objective includes evaluation across Fitzpatrick skin types I–VI.

However, validated Fitzpatrick skin-tone labels were not established in the executed metadata audit.

Therefore, the current experiment does not claim to have demonstrated fairness across Fitzpatrick skin types I–VI.

Available demographic and subgroup variables were instead used for exploratory subgroup analysis.

The absence of validated skin-tone annotations represents an important limitation of the current evaluation.

---

## Ablation Study

An ablation experiment was performed to investigate the contribution of major MedFairXNet components.

| Configuration | Validation AUC |
|---|---:|
| Full MedFairXNet | 0.681109 |
| Without Cross Attention | 0.716513 |
| Without Lesion Attention | 0.713698 |
| Without Feature Pyramid | 0.721588 |
| Without Multi-scale Transformer | 0.763603 |
| Without Uncertainty Head | 0.699659 |
| Without Fairness Regularization | 0.725741 |

The executed ablation experiment shows that each tested component-removal configuration achieved a higher validation AUC than the full MedFairXNet configuration.

The largest improvement was observed when the Multi-scale Transformer component was removed.

This finding suggests that the current architecture may contain components whose additional complexity does not translate into improved validation performance under the evaluated configuration.

---

## Computational Environment

The research was executed using a Kaggle computational environment.

| Component | Configuration |
|---|---|
| Platform | Kaggle |
| Python | 3.12.13 |
| PyTorch | 2.10.0+cu128 |
| CUDA | 12.8 |
| GPU | NVIDIA Tesla T4 |
| Input Resolution | 224 × 224 |
| Random Seed | 42 |

The notebook contains the implemented experimental workflow and the corresponding outputs.

---

## Running the Project

### Clone the Repository

```bash
git clone https://github.com/Subhash-R11/MedFairXNet.git
```

### Enter the Repository

```bash
cd MedFairXNet
```

### Open the Notebook

Open:

```text
Research_Project.ipynb
```

using one of the following:

- Jupyter Notebook
- JupyterLab
- VS Code
- Kaggle

The datasets should be downloaded separately from their respective authorized sources.

---

## Dataset Sources

### HAM10000

https://www.kaggle.com/datasets/kmader/skin-cancer-mnist-ham10000

### PH²

https://www.kaggle.com/datasets/spacesurfer/ph2-dataset

### ISIC 2018

https://challenge.isic-archive.com/data/

The datasets are not redistributed in this repository.

Users are responsible for following the licensing conditions and terms of use associated with each dataset.

---

## Reproducibility

The research notebook records the major experimental settings and procedures used in the study, including:

- Dataset preparation
- Dataset partitioning
- Image preprocessing
- Lesion localization
- U-Net segmentation
- Benchmark model evaluation
- MedFairXNet evaluation
- Classification metrics
- Calibration analysis
- Uncertainty analysis
- Explainability analysis
- Subgroup analysis
- External evaluation
- Ablation experiments

The reported results correspond to the executed experimental workflow.

---

## Limitations

The current study has several limitations:

1. The research uses retrospective public datasets rather than prospectively collected clinical data.
2. PH² is considerably smaller than HAM10000 and ISIC 2018, which may affect the stability of its performance estimates.
3. Prospective clinical validation has not been performed.
4. A dermatologist reader study has not been performed.
5. Validated Fitzpatrick I–VI skin-tone labels were not established in the executed metadata audit.
6. Complete clinician-validated evaluation of XAI faithfulness has not been performed.
7. The evaluated MC-dropout diagnostic produced zero probability variation for the tested checkpoint.
8. Multi-seed replication of all ablation conditions remains future work.

These limitations should be considered when interpreting the reported experimental results.

---

## Future Work

Future research can investigate:

- Multi-seed experimental evaluation
- Architectural simplification
- Larger and more diverse dermoscopic datasets
- Validated skin-tone annotations
- More comprehensive fairness evaluation
- Improved uncertainty estimation
- XAI faithfulness evaluation
- Calibration under distribution shift
- Prospective dermatologist evaluation
- Human–AI comparison
- Clinical validation

A particularly important direction is further investigation of the MedFairXNet architecture because the executed ablation experiment indicates that removing individual components can improve validation AUC.

---

## Scientific Reporting

The research follows an evidence-based reporting approach.

MedFairXNet is not automatically considered the best model simply because it is the proposed architecture.

The experimental results show that different architectures have different strengths across datasets and evaluation metrics.

In the reported three-dataset ROC-AUC comparison, **Swin Transformer achieves the highest mean ROC-AUC**.

MedFairXNet demonstrates favourable calibration on HAM10000, while the ablation results indicate that several architectural components require further investigation.

---

## Disclaimer

This repository is intended for research and educational purposes.

The models and results presented here are not intended to replace professional medical diagnosis or clinical judgement.

Performance on public datasets does not establish clinical effectiveness, clinical deployment readiness, or regulatory approval.
