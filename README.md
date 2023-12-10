# Neurodegenerative Disorders and Genetics Pipeline
A replication of computational methods used in the paper "Comparative Analysis of Multiple Neurodegenerative Diseases Based on Advanced Epigenetic Aging Brain" by Shi and colleagues.

# Supplemental Data and Initialization
`gene_data_model.py` 
Author: Victoria McCray
The `gene_data_model.py` file initializes the loading of supplemental data and materials available from the paper: https://www.frontiersin.org/articles/10.3389/fgene.2021.657636/full

The program reads a text file comprised of cpg sites and gene indexes into a dictionary, and provides simple calculations (such as unique gene set count).


# Data Load Methods in R Notebook
`BINF6310 Final Project.Rmd`

Author: Joseph Maglio

## Overview
This R Notebook demonstrates two methods for loading methylation data using different approaches. The notebook is designed for loading data related to the HumanMethylation27 platform.

## Requirements
- R
- R packages: vctrs, GEOquery, readr

## Data Loading Methods
### Data Load Method 1
- Loads methylation data from a local file (GPL8490_HumanMethylation27_270596_v.1.2.csv.gz).

### Data Load Method 2
- Uses the GEOquery package to load data directly from NCBI GEO using a GEO accession number (GSE15745).


# Classifier Overview
`modeling_ag_nd_predictors.py`

Author: Victoria McCray

This Python script, modeling_ag_nd_predictors.py, demonstrates the use of a RandomForestClassifier to predict labels based on biological data. The data is read from a tab-delimited file (Text S1.txt) and preprocessed to train and evaluate the model.

## Requirements
- `Python 3.x`
- `pandas`
- `scikit-learn`

## Data Preparation
The script assumes that the input data file (Text S1.txt) contains columns: "Organ," "GPL," "GSE," "GSM," "Age," "Condition," and "Label."

Irrelevant columns ("Organ," "GPL," "GSE," "GSM") are dropped, and "Age" is used as a feature.

The data is split into training and testing sets based on the "Train" and "Test" labels in the "Condition" column.

## Model Training and Evaluation
A RandomForestClassifier is initialized and trained on the training set.

Predictions are made on the test set, and model accuracy is calculated.

The script outputs the accuracy score and a classification report for further evaluation.
