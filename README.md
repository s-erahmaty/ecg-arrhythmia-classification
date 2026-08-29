# ECG Arrhythmia Classification ❤️

Machine learning classification of ECG heartbeats as **Normal** or **Abnormal** using features extracted from two-lead ECG signals.

This project focuses on developing a **patient-aware machine learning pipeline**, with particular attention to data leakage, class imbalance, and generalization to unseen patients.

---

## 📌 Project Overview

The goal of this project is to classify individual ECG heartbeats into two categories:

- **0 — Normal**
- **1 — Abnormal**

The abnormal class combines supraventricular, ventricular, fusion, and unknown heartbeat types.

Five machine learning models were explored during model development:

- Decision Tree
- K-Nearest Neighbors (KNN)
- Logistic Regression
- Naive Bayes
- Random Forest

Following the initial model comparison, **KNN and Random Forest were carried forward to the final 5-fold patient-level evaluation**.

**Random Forest achieved the strongest final evaluation performance and was selected as the final model.**

---

## 📊 Dataset

The dataset was provided by the course instructor and is derived from the **MIT-BIH Arrhythmia Database (PhysioNet)**.

It contains pre-extracted features from two ECG leads:

- **Lead II**
- **Lead V5**

### Dataset Characteristics

| Property | Value |
|---|---:|
| Heartbeats | 175,729 |
| Patients | 75 |
| Total features | 34 |
| Lead II features | 17 |
| Lead V5 features | 17 |
| Abnormal heartbeats | ~12.6% |

The original heartbeat categories were:

| Class | Description |
|---|---|
| N | Normal beat |
| S | Supraventricular ectopic beat |
| V | Ventricular ectopic beat |
| F | Fusion beat |
| Q | Unknown beat |

For binary classification, the classes were grouped as:

- **Normal:** N
- **Abnormal:** S, V, F, Q

---

## 🧠 Machine Learning Pipeline

### 1. Patient-Level Data Splitting

`StratifiedGroupKFold` was used to ensure that heartbeats from the same patient remain in the same group.

This means that:

- No patient appears in both the training and test sets
- Patient identity is respected during evaluation
- The Normal/Abnormal class distribution is preserved as much as possible

This approach provides a more realistic estimate of how the model may perform on **unseen patients**.

---

### 2. Feature Analysis

Highly correlated features with correlation greater than **0.90** were identified using the training data and removed.

Outliers were analyzed using:

- Boxplots
- Interquartile Range (IQR)

Outliers were retained because extreme ECG feature values may represent meaningful physiological or abnormal heartbeat variation.

---

### 3. Feature Scaling

Feature scaling was applied where required by the machine learning algorithm.

---

### 4. Class Imbalance

Approximately **12.6%** of the heartbeats belong to the abnormal class.

To address this imbalance, `RandomOverSampler` was applied **inside the training pipeline**.

Applying oversampling only to the training data prevents information from the validation or test sets from entering the oversampling process and reduces the risk of data leakage.

---

### 5. Model Training and Hyperparameter Tuning

Patient-grouped cross-validation was used during **model tuning and final evaluation**.

Hyperparameters were optimized using `GridSearchCV`.

The following models were explored during model development:

- Decision Tree
- K-Nearest Neighbors
- Logistic Regression
- Naive Bayes
- Random Forest

KNN and Random Forest were subsequently evaluated using the final 5-fold patient-level procedure.

---

## 🏆 Results

**Random Forest achieved the strongest final evaluation performance and was selected as the final model.**

### Final Test Performance

| Metric | Score |
|---|---:|
| F1-score | **0.965** |
| Recall | **0.971** |
| ROC-AUC | **0.999** |

### Final 5-Fold Patient-Level Evaluation

| Model | Mean F1-score |
|---|---:|
| **Random Forest** | **0.950 ± 0.035** |
| KNN | 0.930 ± 0.028 |

Random Forest achieved a higher average F1-score across different patient groups in the final evaluation.

During this final 5-fold evaluation, **feature selection was performed independently within each training fold** to reduce the risk of data leakage.

---

## 📋 Confusion Matrix

The confusion matrix summarizes the predictions of the final Random Forest model on the held-out test set.

![Random Forest Confusion Matrix](confusion_matrix.png)

The final model correctly classified **33,301 Normal heartbeats** and **3,767 Abnormal heartbeats**.

Only **160 Normal heartbeats were incorrectly classified as Abnormal**, while **110 Abnormal heartbeats were incorrectly classified as Normal**.

This result shows that the Random Forest model was able to distinguish Normal and Abnormal heartbeats with few classification errors on the held-out test set.

---

## 🔬 Key Takeaways

This project highlights several important considerations when applying machine learning to biomedical data:

- Patient-level train/test separation
- Prevention of data leakage
- Class imbalance handling
- Correlated feature removal
- Outlier analysis
- Hyperparameter optimization
- Patient-grouped cross-validation
- Evaluation of generalization to unseen patients

The project also demonstrates why randomly splitting individual heartbeats can produce overly optimistic results when multiple samples belong to the same patient.

---

## ⚠️ Limitations

- The model performs **binary classification** and does not distinguish between individual abnormal heartbeat types.
- The dataset contains only **75 patients**, so performance may vary on larger or more diverse patient populations.
- The model uses **pre-extracted ECG features** rather than raw ECG waveforms.
- External validation on an independent ECG dataset was not performed.

---

## 🛠️ Technologies

- Python
- Pandas
- NumPy
- scikit-learn
- imbalanced-learn
- Matplotlib
- Jupyter Notebook

---

## 📁 Repository Structure

```text
ecg-arrhythmia-classification/
│
├── ecg_arrhythmia.ipynb
├── confusion_matrix.png
├── requirements.txt
├── README.md
└── .gitignore
```

The dataset is intentionally not included in this repository.

---

## ▶️ How to Run

### 1. Clone the repository

```bash
git clone https://github.com/s-erahmaty/ecg-arrhythmia-classification.git
```

### 2. Enter the project directory

```bash
cd ecg-arrhythmia-classification
```

### 3. Install the required dependencies

```bash
pip install -r requirements.txt
```

### 4. Add the dataset

Place the course-provided `ECGArrhythmia.csv` file inside the project directory.

After adding the dataset locally, the folder should look like this:

```text
ecg-arrhythmia-classification/
│
├── ECGArrhythmia.csv
├── ecg_arrhythmia.ipynb
├── confusion_matrix.png
├── requirements.txt
├── README.md
└── .gitignore
```

> **Note:** `ECGArrhythmia.csv` is not included in this GitHub repository.

### 5. Run the notebook

Open `ecg_arrhythmia.ipynb` using Jupyter Notebook or VS Code and run the cells in order.

---

## 📂 Data Availability

The exact processed dataset used in this project was distributed by the course instructor and is therefore not included in this repository.

The notebook contains saved outputs so that the preprocessing workflow, model development, evaluation, and results can still be reviewed without access to the original dataset.

The underlying data is derived from the **MIT-BIH Arrhythmia Database**, available through PhysioNet.

---

## 🚀 Future Improvements

Possible extensions of this project include:

- Multi-class classification of individual arrhythmia types
- Training models directly on raw ECG waveforms
- Deep learning using PyTorch
- CNN-based ECG classification
- External validation on an independent ECG dataset
- Evaluation using a larger and more diverse patient population
