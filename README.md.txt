# Power System Fault Detection using Machine Learning

## Overview

Fault detection in electrical power systems is critical for maintaining system stability and preventing equipment damage. Traditional protection methods rely on rule-based approaches such as overcurrent and differential relays. However, these methods may struggle with complex fault patterns and require manual tuning.

This project explores a data-driven approach using machine learning to automatically classify different types of power system faults based on electrical measurements.

The model is trained on simulated three-phase system data and is capable of identifying multiple fault conditions using current and voltage signals.

---

## Problem Statement

In a three-phase power system, faults such as line-to-ground (LG), line-to-line (LL), and three-phase faults (LLL) cause disturbances in current and voltage signals.

The goal of this project is to:

* Detect whether a fault has occurred
* Classify the type of fault based on electrical measurements
* Analyze how different features influence fault detection performance

---

## Dataset Description

The dataset consists of simulated electrical measurements from a three-phase power system.

### Input Features:

* Phase Currents: `Ia`, `Ib`, `Ic`
* Phase Voltages: `Va`, `Vb`, `Vc`

### Target Labels:

Fault conditions are encoded using four binary indicators:

* `[G, C, B, A]`

These are mapped to fault types:

* `No Fault`
* `LG` (Line-to-Ground)
* `LLG` (Double Line-to-Ground)
* `LLL` (Three-Phase Fault)
* `LLLG` (Three-Phase-to-Ground Fault)

---

## Methodology

### 1. Data Preprocessing

* Converted encoded fault labels into a single categorical variable
* Verified class distribution and dataset consistency
* Handled indexing and alignment issues for proper comparison

---

### 2. Exploratory Analysis

* Visualized current behavior under normal and fault conditions
* Observed significant differences in magnitude and stability
* Identified limitations of raw signal-based comparison

---

### 3. Feature Engineering

Initial models using raw electrical values showed limitations in distinguishing complex fault types.

To improve performance, the following features were introduced:

#### Current Magnitude

```
I_mag = √(Ia² + Ib² + Ic²)
```

#### Voltage Imbalance

```
Standard deviation of Va, Vb, Vc
```

#### Ratio-Based Features (Key Improvement)

```
Ia / (Ib + Ic)
Ib / (Ia + Ic)
Ic / (Ia + Ib)
```

These features capture **relative phase behavior**, which is critical for distinguishing between similar multi-phase faults.

---

### 4. Model Training

A Random Forest Classifier was used due to its ability to capture nonlinear relationships and handle complex feature interactions.

* Train-test split: 80/20
* Evaluation metrics:

  * Precision
  * Recall
  * F1-score
  * Accuracy

---

## Results

### Initial Model Performance

* High accuracy for simple faults (LG, LLG)
* Significant confusion between:

  * `LLL` and `LLLG`

### After Feature Engineering

* Introduction of ratio-based features significantly improved classification
* Near-perfect performance on test split

### Cross-Validation Results

```
Scores: [0.92, 1.00, 1.00, 1.00, 0.79]
Mean Accuracy: ~0.94
```

---

## Key Insights

* Fault detection is not solely dependent on magnitude; **relative phase relationships are crucial**
* Ratio-based features effectively capture phase imbalance and improve classification
* High test accuracy can be misleading; cross-validation reveals model stability
* Multi-phase faults exhibit similar patterns, making them harder to distinguish

---

## Model Limitations

* Dataset is simulated and may not reflect real-world noise and disturbances
* Performance varies across different data splits
* Some confusion remains between similar multi-phase faults
* Model may not generalize well to unseen real-world systems without retraining

---

## Project Structure

```
power_fault_detection_project
│
├── data
│   └── fault_dataset.xlsx
│
├── notebook
│   └── fault_detection.ipynb
│
├── model
│   └── fault_model.pkl
│
├── images
│   ├── magnitude_comparison.png
│   ├── imbalance_plot.png
│   └── confusion_matrix.png
│
└── README.md
```

---

## Technologies Used

* Python
* Pandas
* NumPy
* Matplotlib
* Seaborn
* Scikit-learn
* Joblib

---

## Future Work

* Apply the model to real-world power system datasets
* Incorporate time-series analysis for dynamic fault detection
* Use deep learning models for waveform-based classification
* Develop a real-time monitoring system for smart grids

---

## Conclusion

This project demonstrates how machine learning can be applied to power system fault detection by combining domain knowledge with data-driven techniques. The use of engineered features, particularly ratio-based features, significantly enhances the model’s ability to distinguish between complex fault types.

The work highlights the importance of not only building models but also understanding their limitations and behavior under varying conditions.

---

## Author

Sejal Patil
Electrical Engineering Student
Interested in Machine Learning, Power Systems, and Smart Grid Technologies
