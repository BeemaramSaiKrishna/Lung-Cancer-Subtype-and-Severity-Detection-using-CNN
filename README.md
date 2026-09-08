#  Lung Cancer Subtype and Severity Detection Using Deep Learning

##  Overview

This project presents a **deep learning-based approach for lung cancer analysis using gene expression data**. The system is designed to classify lung cancer samples into two major histological subtypes:

* **LUAD – Lung Adenocarcinoma**
* **LUSC – Lung Squamous Cell Carcinoma**

In addition to identifying the cancer subtype, the project performs a second prediction task to classify the samples into:

* **Severe**
* **Non-Severe**

The proposed approach uses a **1D Convolutional Neural Network (CNN)** to learn patterns from high-dimensional genomic data. Since gene expression datasets contain tens of thousands of gene features, feature preprocessing and selection are performed before training the deep learning model.

The project uses gene expression datasets obtained from **The Cancer Genome Atlas (TCGA)** and implements a multi-output CNN capable of simultaneously predicting cancer subtype and severity.

---

##  Objectives

The main objectives of this project are:

1. **Lung Cancer Subtype Classification**

   * Classify gene expression samples into LUAD and LUSC.
   * Learn discriminative genomic patterns between the two cancer subtypes.

2. **Cancer Severity Prediction**

   * Categorize samples into Severe and Non-Severe groups.
   * Generate a severity score from the selected gene expression features.

3. **High-Dimensional Data Processing**

   * Process genomic data containing more than 60,000 gene-related features.
   * Remove features with zero variance.
   * Select the most relevant features for model training.

4. **Deep Learning-Based Prediction**

   * Develop a 1D CNN architecture for genomic feature learning.
   * Use a multi-output model to perform subtype and severity prediction simultaneously.

5. **Model Evaluation**

   * Evaluate the model using multiple classification metrics.
   * Analyze prediction performance using confusion matrices and ROC-AUC.

---

##  Dataset

The project uses gene expression datasets from **TCGA (The Cancer Genome Atlas)** for two lung cancer subtypes:

* `TCGA-LUAD.star_tpm.tsv`
* `TCGA-LUSC.star_tpm.tsv`

The datasets contain gene expression measurements for individual samples.

### Dataset Processing

The original datasets are transformed so that:

* Rows represent individual samples.
* Columns represent gene expression features.
* LUAD samples are assigned the label `0`.
* LUSC samples are assigned the label `1`.

The notebook contains:

* **589 LUAD samples**
* **552 LUSC samples**
* **1,141 total samples**
* Approximately **60,660 features** before feature selection.

After preprocessing and feature selection, the dataset is reduced to:

**1,141 samples × 1,000 selected features**

---

##  Methodology

The complete workflow consists of several stages.

### 1. Data Loading

The LUAD and LUSC gene expression datasets are loaded using Pandas.

The gene expression matrices are transposed so that the dataset follows the format:

```text
Samples × Genes
```

The two datasets are then combined into a single dataset.

---

### 2. Cancer Subtype Labeling

Binary labels are assigned to the two cancer subtypes:

```text
LUAD → 0
LUSC → 1
```

The combined dataset is shuffled before model development.

---

### 3. Data Standardization

Gene expression values are standardized using **StandardScaler** from Scikit-learn.

Standardization helps place gene expression features on a comparable scale and improves the learning process of the neural network.

---

### 4. Feature Selection

Gene expression datasets contain a very large number of features. Training a deep learning model directly on all available genes can increase computational requirements and may introduce irrelevant information.

Therefore, two feature-selection steps are applied.

#### Variance Threshold

Features with zero variance are removed using:

```text
VarianceThreshold(threshold=0)
```

This eliminates constant features that do not provide useful information for classification.

#### ANOVA F-Test

The **SelectKBest** method with the ANOVA F-test is then used to select the top **1,000 features**.

```text
SelectKBest(score_func=f_classif, k=1000)
```

This reduces the original high-dimensional genomic representation to a more manageable set of important features.

---

##  Severity Label Generation

A severity score is generated from the selected gene expression features.

For every sample, the mean expression across the selected 1,000 features is calculated.

The median of all severity scores is then used as the classification threshold.

```text
Score > Median → Severe
Score ≤ Median → Non-Severe
```

This produces a binary severity label for each sample.


---

##  Deep Learning Model

A **1D Convolutional Neural Network (CNN)** is used to learn patterns from the selected genomic features.

The input contains the 1,000 selected features, which are reshaped into a format suitable for a 1D convolutional network.

### Model Architecture

```text
Input Layer
     ↓
Conv1D – 64 filters
     ↓
MaxPooling1D
     ↓
Conv1D – 128 filters
     ↓
MaxPooling1D
     ↓
Flatten
     ↓
Dense – 128 neurons
     ↓
Dropout – 0.5
     ↓
 ┌───────────────┬────────────────┐
 ↓               ↓
Subtype Output   Severity Output
 ↓               ↓
Sigmoid          Sigmoid
```

The CNN contains two prediction branches:

### Subtype Output

Predicts:

```text
0 → LUAD
1 → LUSC
```

### Severity Output

Predicts:

```text
0 → Non-Severe
1 → Severe
```

Both outputs use **binary cross-entropy loss**.

The model is trained using the **Adam optimizer** for 30 epochs with a batch size of 32.

---

##  Technologies Used

### Programming Language

* Python

### Machine Learning / Deep Learning

* TensorFlow
* Keras
* Scikit-learn

### Data Processing

* Pandas
* NumPy

### Visualization

* Matplotlib

### Key Algorithms and Techniques

* StandardScaler
* Variance Threshold
* ANOVA F-test
* SelectKBest
* 1D Convolutional Neural Network
* Max Pooling
* Dropout
* Binary Classification
* Multi-output Deep Learning

---

##  Model Evaluation

The model is evaluated using several classification metrics:

* Accuracy
* Precision
* Recall
* F1 Score
* ROC-AUC
* Confusion Matrix
* ROC Curve

These metrics are calculated separately for:

1. Lung cancer subtype classification
2. Cancer severity detection

---

##  Results

The model achieved the following performance on the test dataset.

### Cancer Subtype Classification

| Metric    |      Score |
| --------- | ---------: |
| Accuracy  | **93.45%** |
| Precision | **93.52%** |
| Recall    | **92.66%** |
| F1 Score  | **93.09%** |
| ROC-AUC   | **93.41%** |

### Severity Detection

| Metric    |       Score |
| --------- | ----------: |
| Accuracy  |  **95.63%** |
| Precision | **100.00%** |
| Recall    |  **91.94%** |
| F1 Score  |  **95.80%** |
| ROC-AUC   |  **95.97%** |

The final test evaluation reports approximately **93.45% accuracy for subtype classification** and **95.63% accuracy for severity detection**.

---

##  Confusion Matrix

### Cancer Subtype Classification

```text
[[113   7]
 [  8 101]]
```

This indicates that the model correctly classified the majority of LUAD and LUSC samples while producing relatively few misclassifications.

### Severity Detection

```text
[[105   0]
 [ 10 114]]
```

The severity classifier correctly identified most Severe and Non-Severe samples in the test set.

---

##  Example Prediction

The trained model can take a test sample and simultaneously predict its cancer subtype and severity.

Example:

```text
Predicted Cancer Type: LUAD
Predicted Severity: Non-Severe
```

The notebook demonstrates this prediction using a sample from the test dataset.

---

##  Project Workflow

```text
TCGA Gene Expression Data
          ↓
   Load LUAD + LUSC
          ↓
   Transpose Dataset
          ↓
     Assign Labels
          ↓
    Combine Datasets
          ↓
    Standardization
          ↓
  Remove Constant Genes
          ↓
 ANOVA Feature Selection
          ↓
 Select Top 1,000 Features
          ↓
   Generate Severity Score
          ↓
 Create Severe / Non-Severe Labels
          ↓
    Train/Test Split
          ↓
       1D CNN
          ↓
 ┌────────┴─────────┐
 ↓                  ↓
Subtype          Severity
Prediction       Prediction
 ↓                  ↓
LUAD/LUSC       Severe/Non-Severe
```

---

##  How to Run the Project

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/Lung-Cancer-Subtype-Severity-Detection.git
```

### 2. Navigate to the Project Directory

```bash
cd Lung-Cancer-Subtype-Severity-Detection
```

### 3. Install Dependencies

```bash
pip install pandas numpy scikit-learn tensorflow matplotlib
```

### 4. Add the Dataset

Place the following files in the appropriate project directory:

```text
TCGA-LUAD.star_tpm.tsv
TCGA-LUSC.star_tpm.tsv
```

### 5. Run the Notebook

Open:

```text
CancerSeverityDetection.ipynb
```

The notebook can be executed using:

* Jupyter Notebook
* JupyterLab
* Google Colab

A GPU can be useful for faster deep learning model training.

---


## Key Highlights

*  Uses **TCGA gene expression data**
*  Classifies **LUAD vs LUSC**
*  Predicts **Severe vs Non-Severe**
*  Reduces genomic features to the **top 1,000 features**
*  Uses a **1D CNN**
*  Uses a **multi-output prediction architecture**
*  Achieves approximately **93.45% subtype accuracy**
*  Achieves approximately **95.63% severity accuracy**
*  Evaluates performance using Accuracy, Precision, Recall, F1, ROC-AUC and Confusion Matrix

---

