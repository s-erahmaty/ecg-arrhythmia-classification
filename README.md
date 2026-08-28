# ECG Arrhythmia Classification

Binary classification of ECG heartbeats as **Normal** or **Abnormal** using machine learning models and features extracted from two-lead ECG signals.

## Dataset

The dataset is derived from the **MIT-BIH Arrhythmia Database (PhysioNet)** and contains features extracted from two ECG leads: **Lead II** and **Lead V5**.

- 175,729 heartbeats from 75 patients
- 34 ECG-derived features per heartbeat
- 17 features from Lead II and 17 features from Lead V5
- Original heartbeat classes (as documented by the dataset source):
  - N: Normal (`N`)
  - S: Supraventricular ectopic beat (`SVEB`)
  - V: Ventricular ectopic beat (`VEB`)
  - F: Fusion beat (`F`)
  - Q: Unknown beat (`Q`)
- For this project, the classes were grouped into:
  - **0 = Normal (N)**
  - **1 = Abnormal (S, V, F, Q)**
- Approximately 12.6% of the heartbeats are abnormal.

## Data Availability

The dataset used in this project was provided by the course instructor and is derived from the MIT-BIH Arrhythmia Database.

The processed dataset is not included in this repository because the exact distributed version was provided for coursework and is not publicly hosted by the author.

The notebook is provided with saved outputs so that the preprocessing steps, model development, evaluation, and results can be reviewed without access to the original dataset.

## Method

The main preprocessing and modeling steps were:

- Patient-level train/test split using `StratifiedGroupKFold`
- No patient appears in both training and test sets
- Stratification used to preserve the Normal/Abnormal class distribution as much as possible
- Highly correlated features (> 0.90) identified using the training data and removed
- Outliers analyzed using boxplots and the IQR method
- Outliers were retained because extreme values may represent meaningful heartbeat variation
- Feature scaling applied where required by the model
- Class imbalance handled using `RandomOverSampler` inside the training pipeline
- Patient-grouped cross-validation used during model tuning
- Hyperparameters tuned using `GridSearchCV`

Five machine learning models were compared:

- Decision Tree
- K-Nearest Neighbors (KNN)
- Logistic Regression
- Naive Bayes
- Random Forest

## Results

**Random Forest** was selected as the final model.

In the final 5-fold patient-level evaluation, feature selection was performed independently within each training fold to avoid data leakage.

| Metric | Score |
|---|---|
| Test F1 | 0.965 |
| Recall | 0.971 |
| ROC-AUC | 0.999 |
| 5-fold CV F1 | **0.950 ± 0.035** |

For comparison, KNN achieved a 5-fold CV F1-score of **0.930 ± 0.028**. Random Forest therefore showed better average performance across different patient groups.

## Limitations

- The model performs binary classification only and does not distinguish between different abnormal heartbeat types.
- The dataset contains only 75 patients, so model performance may vary when evaluated on new patient groups.

## How to Run

1. Clone or download this repository.

2. Install the required dependencies:

   `pip install -r requirements.txt`

3. Place the course-provided `ECGArrhythmia.csv` file in the project folder.

4. Open `ecg_arrhythmia.ipynb` in Jupyter Notebook or VS Code.

5. Run the notebook cells in order.

## Data Source

The dataset used in this project is derived from the **MIT-BIH Arrhythmia Database**, available through PhysioNet.

The provided dataset contains pre-extracted ECG features from **Lead II** and **Lead V5**.