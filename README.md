# Bank Marketing: Comparing Classifiers

## Overview
The classification goal is to predict whether a client subscribes to a term deposit.
The dataset comes from the UCI Machine Learning repository [link](https://archive.ics.uci.edu/ml/datasets/bank+marketing). The data is from a Portugese banking institution and is a collection of the results of multiple marketing campaigns.
It represents 17 marketing campaigns conducted by a Portuguese
bank between May 2008 and November 2010. The campaigns primarily used
telephone calls to promote long-term deposits.   

## Business objective
The business objective is to improve the effectiveness and efficiency
of the bank's telephone marketing campaigns by identifying clients
who are more likely to subscribe to a term deposit.
Predict subscription as a binary outcome: `yes = 1`, `no = 0`. 
- False positives represent predicted subscribers who do not subscribe.
- False negatives represent subscribers the model misses.

## Findings

Compared Logistic Regression, K-Nearest Neighbors (KNN), Decision Tree, and Support Vector Machine (SVM) models to predict whether a customer will subscribe to a term deposit.

| Model | Fit time (s) | Train accuracy | Test accuracy | Precision | Recall | F1 |
|---|---:|---:|---:|---:|---:|---:|
| Logistic Regression | 0.0746 | 88.73% | 88.74% | 0.00% | 0.00% | 0.0000 |
| KNN | 0.0491 | 89.17% | 88.06% | 35.42% | 7.33% | 0.1214 |
| Decision Tree | 0.1573 | 91.71% | 86.49% | 23.50% | 8.84% | 0.1284 |
| SVM | 31.1302 | 88.82% | 88.64% | 27.78% | 0.54% | 0.0106 |

Logistic Regression matches majority-baseline accuracy while finding no subscribers. Decision Tree has the highest default F1 but the largest train–test accuracy gap (5.22 percentage points), suggesting overfitting. KNN fits quickly, though its prediction cost is excluded. Default SVM is slowest to fit and detects very few subscribers.

### Cross-validation results

| Model | Best mean CV F1 | Total search time (s) |
|---|---:|---:|
| **SVM** | **0.262676** | 260.775082 |
| Logistic Regression | 0.255636 | 3.517837 |
| Decision Tree | 0.252478 | 2.933074 |
| KNN | 0.142829 | 3.042100 |

SVM is selected with `C=0.5`, `class_weight='balanced'`, using the RBF kernel. 
### Tuned model test results

| Model | Accuracy | Precision | Recall | F1 |
|---|---:|---:|---:|---:|
| Decision Tree | 65.02% | 17.02% | 54.31% | 0.259126 |
| **SVM (CV-selected)** | **60.46%** | **16.41%** | **61.31%** | **0.258931** |
| Logistic Regression | 58.44% | 15.81% | 62.18% | 0.252075 |
| KNN | 87.08% | 28.62% | 9.81% | 0.146067 |

Decision Tree and SVM have almost identical test F1, differing by only 0.000195. SVM remains the selected model because selection uses training cross-validation, not test rankings. Logistic Regression has the highest recall. KNN has the highest accuracy and precision but misses approximately 90% of subscribers.

### Selected SVM evaluation

| Metric | Value |
|---|---:|
| Training F1 | 0.2796 |
| Mean CV F1 | 0.2627 |
| Test F1 | 0.2589 |
| Test precision | 16.41% |
| Test recall | 61.31% |
| Test accuracy | 60.46% |
| Test balanced accuracy | 60.84% |

| Actual outcome | Predicted No | Predicted Yes |
|---|---:|---:|
| No subscription | 4,412 | 2,898 |
| Subscription | 359 | 569 |

The model identifies 569 of 928 subscribers, missing 359. Only 569 of its 3,467 positive predictions are correct. 

## Conclusions

- SVM achieved the highest mean cross-validation subscriber F1 (0.2627). Its test F1 was 0.2589, with 61.31% recall and 16.41% precision. Tuning improved subscriber detection, but many positive predictions remain incorrect.
- Logistic Regression offers nearly comparable performance at much lower search cost.
- Decision Tree and SVM have almost identical test F1, with different precision–recall tradeoffs.

## Future work

- **Business costs:** define the costs of false positives and false negatives before deciding whether the precision–recall tradeoff is acceptable.
- **Historical random split:** validate on later campaigns to assess future performance.
