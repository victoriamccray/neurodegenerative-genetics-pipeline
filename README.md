# Neurodegenerative Genetics Pipeline

A computational pipeline replicating and extending methods from [Shi et al. (2021)](https://www.frontiersin.org/articles/10.3389/fgene.2021.657636/full) for predicting neurodegenerative diseases from DNA methylation patterns.

![Python](https://img.shields.io/badge/python-3.8+-blue.svg)
![R](https://img.shields.io/badge/R-4.0+-276DC3.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)

## Overview

This project uses epigenetic biomarkers (DNA methylation) to classify neurodegenerative diseases including Alzheimer's Disease (AD), Parkinson's Disease (PD), Progressive Supranuclear Palsy (PSP), and Frontotemporal Dementia (FTD).

**Key Features:**
- Processes methylation data from NCBI GEO database (HumanMethylation27 platform)
- Maps CpG sites to gene annotations for biological interpretation  
- Implements Random Forest classifier for disease prediction
- Analyzes 1,137 samples across 6 conditions from brain tissue and peripheral blood

**Datasets:**
- **Platform:** GPL8490 (HumanMethylation27), GPL13534 (HumanMethylation450), GPL21145 (MethylationEPIC)
- **Conditions:** AD, PD, PSP, FTD, Normal aged, Healthy young
- **Tissues:** Multiple brain regions and peripheral blood
- **Source:** NCBI GEO accessions GSE15745, GSE53740, GSE57361, GSE66351, GSE138597

---

## Repository Structure

```
neurodegenerative-genetics-pipeline/
├── README.md                           # Project documentation
├── requirements.txt                    # Python dependencies
├── data/                               # Data files (see .gitignore)
│   ├── Text S1.txt                     # Sample metadata
│   └── Text S2.txt                 # CpG-to-gene mapping
├── notebooks/                          # Analysis notebooks
│   ├── 01_Load_Methylation_Data.Rmd    # R notebook for data loading
│   └── 02_Exploring_Data.ipynb         # Python exploratory analysis
├── src/                                # Source code
│   ├── gene_data_model.py              # Gene mapping utilities
│   └── modeling_ag_nd_predictors.py    # Classification model
└── outputs/                            # Generated results
    └── figures/                        # Visualizations
```

---

## Installation

### Prerequisites

**Python 3.8+**
```bash
pip install pandas scikit-learn numpy matplotlib seaborn
```

**R 4.0+**
```r
install.packages(c("GEOquery", "readr", "vctrs"))
```

### Clone Repository
```bash
git clone https://github.com/victoriamccray/neurodegenerative-genetics-pipeline.git
cd neurodegenerative-genetics-pipeline
```

---

## Pipeline Components

### 1. Gene Mapping (`gene_data_model.py`)

**Author:** Victoria McCray

Initializes CpG site-to-gene mapping using supplemental data from [Shi et al. (2021)](https://www.frontiersin.org/articles/10.3389/fgene.2021.657636/full).

**Functionality:**
- Reads Text S2.txt mapping file (CpG sites → gene symbols)
- Builds dictionary structure for efficient lookups
- Calculates gene set statistics (unique genes, coverage)

**Usage:**
```python
python gene_data_model.py
```

---

### 2. Data Loading (`01_Load_Methylation_Data.Rmd`)

**Author:** Joseph Maglio

R notebook providing two methods for accessing HumanMethylation27 platform data.

**Requirements:**
- R packages: `vctrs`, `GEOquery`, `readr`

**Method 1: Local File Loading**
- Loads from pre-downloaded file: `GPL8490_HumanMethylation27_270596_v.1.2.csv.gz`
- Faster for repeated analysis
- Requires prior data download

**Method 2: Direct GEO Access**
- Uses `GEOquery` to fetch data from NCBI GEO (accession: GSE15745)
- Always retrieves latest data
- Ensures reproducibility

**Usage:**
```r
# Open in RStudio and run chunks sequentially
# Or knit to HTML
rmarkdown::render("notebooks/01_Load_Methylation_Data.Rmd")
```

---

### 3. Disease Classification Model (`modeling_ag_nd_predictors.py`)

**Author:** Victoria McCray

Random Forest classifier for predicting neurodegenerative disease status from methylation data.

**Requirements:**
- Python 3.x
- `pandas`, `scikit-learn`

**Input:** 
- File: `Text S1.txt`
- Columns: `Organ`, `GPL`, `GSE`, `GSM`, `Age`, `Condition`, `Label`

**Data Processing:**
1. Drops non-predictive columns (`Organ`, `GPL`, `GSE`, `GSM`)
2. Uses `Age` as primary feature (will be extended to include CpG methylation values)
3. Splits data by pre-assigned `Train`/`Test` labels in `Condition` column

**Model Training:**
- Algorithm: Random Forest Classifier
- Training set: Samples labeled "Train"
- Testing set: Samples labeled "Test"

**Output:**
- Model accuracy score
- Classification report (precision, recall, F1-score per condition)

**Usage:**
```python
python modeling_ag_nd_predictors.py
```

**Current Limitations:**
- Uses only age as predictor (methylation data integration in progress)
- No feature selection implemented yet
- Class imbalance not addressed (PD: 25 samples, PSP: 30 samples vs Normal aged: 300 samples)

---

## Data Description

### Sample Metadata (Text S1.txt)

1,137 samples from multiple NCBI GEO studies:

| Condition | Training Samples | Testing Samples | Age Range | Description |
|-----------|-----------------|-----------------|-----------|-------------|
| Normal aged | 300 | 142 | 53-101 | Healthy older adults |
| Healthy young | 250 | 116 | 15-50 | Healthy younger adults |
| AD | 85 | 43 | 55-97 | Alzheimer's Disease |
| FTD | 85 | 38 | 51-93 | Frontotemporal Dementia |
| PSP | 30 | 12 | 56-85 | Progressive Supranuclear Palsy |
| PD | 25 | 11 | 51-80 | Parkinson's Disease |

### CpG-to-Gene Mapping (Text S2.txt)

- **Total CpG sites:** ~27,000
- **Mapped genes:** Multiple CpG sites per gene provide different regulatory information
- **Source:** Supplemental materials from Shi et al. (2021)

---
## Project Status

### Completed
- Data loading methods (R and Python)
- Gene mapping utilities
- Basic Random Forest classifier structure
- Exploratory data analysis

### Next Steps
- Integrate methylation values with classification model
- Feature selection and dimensionality reduction
- Model evaluation and visualization
- Address class imbalance issues
---

## Key Findings (To Be Updated)

**Exploratory Analysis Insights:**
- Class imbalance: PD (36 samples) and PSP (42 samples) are underrepresented
- Age overlap between conditions requires disease-specific methylation signatures
- 33% of samples from peripheral blood (clinically relevant for non-invasive testing)
- Multiple methylation platforms (may require platform-specific normalization)

**Model Performance:** *(Will be added after running full pipeline)*

---

## Technical Skills

| Category | Technologies |
|----------|-------------|
| **Programming** | Python, R |
| **Data Science** | pandas, scikit-learn, data wrangling |
| **Bioinformatics** | GEO data access, methylation analysis, gene annotation |
| **Machine Learning** | Random Forest, classification, train/test evaluation |
| **Statistics** | Feature engineering, model evaluation metrics |

---

## Citation

If you use this pipeline or find it helpful, please cite the original paper:

```bibtex
@article{shi2021comparative,
  title={Comparative Analysis of Multiple Neurodegenerative Diseases Based on Advanced Epigenetic Aging Brain},
  author={Shi, Lu and Zhao, Yulu and Fang, Jiali and Zhang, Jialin and Liu, Yang and Zhang, Yiming},
  journal={Frontiers in Genetics},
  volume={12},
  pages={657636},
  year={2021},
  publisher={Frontiers},
  doi={10.3389/fgene.2021.657636}
}
```

---

## Authors & Acknowledgments

**Contributors:**
- **Victoria McCray** - Pipeline development, Python implementation, exploratory analysis
- **Joseph Maglio** - R data loading methods

**Course:** BINF6310 - Northeastern University  
**Original Paper:** Shi et al. (2021) - Frontiers in Genetics

---


---

## Contact

**Victoria McCray**  
MSc Bioinformatics Candidate | Northeastern University  
[GitHub](https://github.com/victoriamccray) | [LinkedIn](https://linkedin.com/in/victoria-mccray-99399514a) | [Website](https://victoriamccray.github.io)

For questions or collaboration opportunities, feel free to reach out!

---

## References

1. Shi, L., et al. (2021). Comparative Analysis of Multiple Neurodegenerative Diseases Based on Advanced Epigenetic Aging Brain. *Frontiers in Genetics*, 12, 657636. https://doi.org/10.3389/fgene.2021.657636
2. NCBI Gene Expression Omnibus (GEO). https://www.ncbi.nlm.nih.gov/geo/
3. Illumina HumanMethylation27 BeadChip. Platform GPL8490.
