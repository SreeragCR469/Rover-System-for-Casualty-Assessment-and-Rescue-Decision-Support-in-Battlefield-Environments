# Results

This section presents the experimental evaluation and validation of the proposed rover system.

## 1. Machine Learning Model Evaluation

Six supervised machine learning classifiers were evaluated for fall detection using the extracted pose-based features.

| Model | Accuracy (%) | FALL Recall (%) | F1 Score | Inference (ms/frame) |
|---|---:|---:|---:|---:|
| Random Forest | 93.00 | 84.55 | 0.9151 | 0.0132 |
| Stacking Ensemble | 93.14 | 84.46 | 0.9171 | 0.0241 |
| Gradient Boosting | 92.04 | 82.73 | 0.9033 | 0.0018 |
| SVM (RBF) | 88.68 | 86.14 | 0.8692 | 0.0520 |
| MLP | 91.83 | 81.59 | 0.9003 | 0.0004 |
| Soft Voting | 92.04 | 82.50 | 0.9031 | 0.0655 |

Random Forest was selected for deployment because it provided a strong balance between classification performance, precision, and computational efficiency on the Raspberry Pi platform.

## 2. Fall Detection and Stability Analysis

A 15-frame confirmation mechanism was used to reduce transient false fall detections.

After a confirmed fall, the system monitored eight body landmarks for 60 seconds to determine whether the subject was stationary or moving.

The selected movement threshold was:

```text
σmax ≤ 0.015  →  STATIONARY
σmax > 0.015  →  MOVING# Results

This section presents the experimental evaluation and validation of the proposed rover system.

## 1. Machine Learning Model Evaluation

Six supervised machine learning classifiers were evaluated for fall detection using the extracted pose-based features.

| Model | Accuracy (%) | FALL Recall (%) | F1 Score | Inference (ms/frame) |
|---|---:|---:|---:|---:|
| Random Forest | 93.00 | 84.55 | 0.9151 | 0.0132 |
| Stacking Ensemble | 93.14 | 84.46 | 0.9171 | 0.0241 |
| Gradient Boosting | 92.04 | 82.73 | 0.9033 | 0.0018 |
| SVM (RBF) | 88.68 | 86.14 | 0.8692 | 0.0520 |
| MLP | 91.83 | 81.59 | 0.9003 | 0.0004 |
| Soft Voting | 92.04 | 82.50 | 0.9031 | 0.0655 |

Random Forest was selected for deployment because it provided a strong balance between classification performance, precision, and computational efficiency on the Raspberry Pi platform.

## 2. Fall Detection and Stability Analysis

A 15-frame confirmation mechanism was used to reduce transient false fall detections.

After a confirmed fall, the system monitored eight body landmarks for 60 seconds to determine whether the subject was stationary or moving.

The selected movement threshold was:

```text
σmax ≤ 0.015  →  STATIONARY
σmax > 0.015  →  MOVING
## 5. Demonstration Video

A demonstration of the developed rover system, including casualty assessment, environmental hazard monitoring, and dashboard output.

[▶ Watch the Project Demonstration](https://drive.google.com/drive/folders/1fagYlGnrdbUKubp6HfBWclfjgMLTXti3)
