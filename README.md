# Overview
Transformer Architecture from scratch in Python (using pytorch) and train it on a dataset.

<img width="298" height="420" alt="unnamed" src="https://github.com/user-attachments/assets/9a6e1097-7cc7-47b3-ad2a-00f88af7553f" />

---

## Model performance summary
![Model Snapshot](https://github.com/user-attachments/assets/f4e280af-5c4c-4a6e-a827-9d54026c8261)

| Metric                |             Value |
| --------------------- | ----------------: |
| Final validation loss |            0.0075 |
| Best validation loss  | 0.0003 (Epoch 13) |
| Final accuracy        |            99.75% |
| Peak accuracy         | 100.00% (Epoch 8) |
| Training duration     |         15 epochs |

---

## Detailed prediction analysis

| Sample | Input sequence | Expected output | Model prediction | Match | Token accuracy |
| -----: | -------------- | --------------- | ---------------- | :---: | -------------: |
|      1 | `8`            | `8`             | `8`              |   ✓   |         100.0% |
|      2 | `7 2`          | `7 2`           | `7 2`            |   ✓   |         100.0% |
|      3 | `9 3`          | `9 3`           | `9 3`            |   ✓   |         100.0% |
|      4 | `8`            | `8`             | `8`              |   ✓   |         100.0% |
|      5 | `5 9 7 3`      | `5 9 7 3`       | `5 9 7 3 3 3 3`  |   ✗   |         100.0% |

> Note: sample 5 shows a token-level mismatch where the model appended extra tokens after producing the expected sequence.

## Classification report
<img width="928" height="784" alt="Untitled" src="https://github.com/user-attachments/assets/f9abb097-59de-4558-8e72-1129f5c96779" />


```
              precision    recall  f1-score   support

           1       1.00      1.00      1.00       230
           2       1.00      1.00      1.00       261
           3       0.99      1.00      0.99       253
           4       1.00      0.99      1.00       247
           5       1.00      1.00      1.00       238
           6       0.99      1.00      1.00       257
           7       1.00      1.00      1.00       249
           8       1.00      1.00      1.00       237
           9       1.00      1.00      1.00       235
          10       1.00      0.99      1.00       500

    accuracy                           1.00      2707
   macro avg       1.00      1.00      1.00      2707
weighted avg       1.00      1.00      1.00      2707
```

**Notes**

* Total samples: 2707. Class supports are shown per class.
* Per-class metrics are excellent; small deviations occur for classes 3, 4, 6 and 10 (minor drops in precision or recall).

---

## Error analysis

**Common error patterns (from inspection of misclassified tokens):**

* Expected 10 but predicted 3 (multiple occurrences)
* Expected 7 but predicted 10
* Expected 4 but predicted 6

What this really means is: the model occasionally inserts or substitutes tokens that are numerically or positionally similar to the ground truth evidence of minor insertion/deletion behavior or token-confusion around classes 3 and 10.

**Actionable next steps**

1. Add per-class confusion matrix visualization to confirm whether errors are concentrated (e.g., 10↔3) or spread out.
2. Check tokenization: verify there are no token-boundary issues for multi-digit tokens like `10`.
3. Run a focused validation sample that logs full logits and top-5 predictions for misclassified examples to see whether errors are low-confidence swaps or high-confidence mistakes.

---

## Moderation notes

* Model successfully learned the sequence copying task.
* Final validation accuracy reported: 99.75% (consistent with earlier summary).
* Confusion matrix shows strong diagonal concentration, indicating near-perfect class separation.
* Error analysis indicates minor confusion between similar tokens especially `10` and `3` in the sample logs.
* Transformer architecture demonstrates effective learning of sequence relationships; the remaining errors are small and actionable.

---

*Last updated: August 13, 2025*
