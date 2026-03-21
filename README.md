Overview:-
This project presents a deep learning-based approach for detecting lung cancer subtypes (LUAD and LUSC) and predicting cancer severity using gene expression data.
A 1D Convolutional Neural Network (CNN) is used to learn patterns from high-dimensional genomic data and provide accurate classification results.

Objectives:-
Classify lung cancer into LUAD (Lung Adenocarcinoma) and LUSC (Lung Squamous Cell Carcinoma)
Predict severity levels (Severe / Non-Severe)
Handle high-dimensional gene expression data using feature selection techniques
Improve model performance using deep learning

Methodology:-
->Data Processing
Loaded gene expression datasets from TCGA
Transposed data (samples × genes format)
Applied Standardization (StandardScaler)
->Feature Selection
Removed constant genes using Variance Threshold
Selected top 1000 important genes using ANOVA F-test
->Severity Label Creation
Computed severity score using mean gene expression
Used median threshold to classify:
Above median → Severe
Below median → Non-Severe

Model Architecture:-
1D Convolutional Neural Network (CNN)
Layers used:
Conv1D
MaxPooling
Dense
Dropout
Multi-output model:
Subtype prediction
Severity prediction
Technologies Used
Python
TensorFlow / Keras
Scikit-learn
Pandas
NumPy
Matplotlib

Evaluation Metrics:-
Accuracy
Precision
Recall
F1 Score
ROC-AUC
Confusion Matrix

Results:-
Subtype Classification Accuracy: ~93–95%
Severity Detection Accuracy: ~90–96%
Model shows strong performance on high-dimensional genomic data
