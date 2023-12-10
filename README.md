# Neurodegenerative Disorders and Genetics Pipeline
A replication of computational methods used in the paper "Comparative Analysis of Multiple Neurodegenerative Diseases Based on Advanced Epigenetic Aging Brain" by Shi and colleagues.

The `gene_data_model.py` file initializes the loading of supplemental data and materials available from the paper: https://www.frontiersin.org/articles/10.3389/fgene.2021.657636/full

The program reads a text file comprised of cpg sites and gene indexes into a dictionary, and provides simple calculations (such as unique gene set count).

# Classifier Overview
`modeling_ag_nd_predictors.py`

This Python script, modeling_ag_nd_predictors.py, demonstrates the use of a RandomForestClassifier to predict labels based on biological data. The data is read from a tab-delimited file (Text S1.txt) and preprocessed to train and evaluate the model.

# Requirements
Python 3.x
pandas
scikit-learn

Data Preparation
The script assumes that the input data file (Text S1.txt) contains columns: "Organ," "GPL," "GSE," "GSM," "Age," "Condition," and "Label."

Irrelevant columns ("Organ," "GPL," "GSE," "GSM") are dropped, and "Age" is used as a feature.

The data is split into training and testing sets based on the "Train" and "Test" labels in the "Condition" column.

Model Training and Evaluation
A RandomForestClassifier is initialized and trained on the training set.

Predictions are made on the test set, and model accuracy is calculated.

The script outputs the accuracy score and a classification report for further evaluation.
