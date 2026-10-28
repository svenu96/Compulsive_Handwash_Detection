# Compulsive Handwash Detection

## Input features and prediction labels

The input is smartwatch motion data sampled at 50 Hz: accelerometer readings `acc x`, `acc y`, and `acc z` (acceleration on three axes), and gyroscope readings `gyro x`, `gyro y`, and `gyro z` (angular velocity on three axes). The target is the `relabeled` column:

| Label | Meaning |
| --- | --- |
| 0 | No handwash / other activity |
| 1 | Routine handwash |
| 2 | Compulsive handwash |

The final classification task used **only labels 1 and 2** to distinguish routine from compulsive handwashing; label 0 was excluded. For binary model training, labels 1 and 2 were recoded as 0 and 1, respectively.

## Participants and data counts

The dataset consists of data from **22 participants** over 28 days. Participant 30 was selected for the reported model experiments.

| Participant | Label 0 | Label 1 | Label 2 | Total reported data points |
| --- | ---: | ---: | ---: | ---: |
| 1 | 77,335,233 | 39,921 | 364,123 | 77,739,277 |
| 2 | 64,797,033 | 382,169 | 5,703 | 65,184,905 |
| 30 | Not reported | 94,855 | 219,903 | 314,758 for labels 1 and 2 |

## Class imbalance and balancing

- **DBSCAN:** In the three-label data, label 0 (no handwash) greatly outnumbers labels 1 and 2.
- **K-means:** After removing label 0, label 2 is more common than label 1 in the illustrated distribution. The participant 30 counts above likewise show more label 2 than label 1 before windowing. The Transformer section also reports **7,328 label-1 windows** and **3,163 label-2 windows** after sliding-window preparation.
- **Balancing methods:** Random undersampling and SMOTE oversampling of the training data were performed. Also calculated class weights and used a weighted loss so the less represented class has more influence during training.

## Model results

**Table 4 — multiclass results for participant 30** (reported as a comparison; the focused classification task above uses labels 1 and 2):

| Model | Accuracy | Mean F1-score |
| --- | ---: | ---: |
| Random Forest | 0.34 | 0.33 |
| Logistic Regression | 0.37 | 0.34 |
| Naive Bayes | 0.36 | 0.35 |
| XGBoost | 0.37 | 0.36 |

**Table 5 — binary results for participant 30:**

| Model | Accuracy | Mean F1-score |
| --- | ---: | ---: |
| Random Forest | 0.44 | 0.37 |
| Logistic Regression | 0.57 | 0.38 |
| Naive Bayes | 0.43 | 0.30 |
| XGBoost | 0.48 | 0.48 |

**Figure 43 — deep convolutional Transformer, binary routine-versus-compulsive classification:**

| Class | Precision | Recall | F1-score | Support |
| --- | ---: | ---: | ---: | ---: |
| 1 — routine handwash | 0.57 | 0.81 | 0.67 | 3,198 |
| 2 — compulsive handwash | 0.43 | 0.19 | 0.26 | 2,402 |
| Macro average | 0.50 | 0.50 | 0.47 | 5,600 |
| Weighted average | 0.51 | 0.55 | 0.50 | 5,600 |

Overall accuracy: **0.55** on **5,600** test windows. The confusion matrix `[[2602, 596], [1952, 450]]` (rows: true labels 1 and 2; columns: predicted labels 1 and 2).

## Result

As the handwash action, regardless of whether it is routine or compulsive, is the same, even a model as complex as a transformer was not able to distinguish the actions. 
