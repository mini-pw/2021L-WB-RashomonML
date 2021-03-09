# Introduction
### Article: 'Benchmarking deep learning models on large healthcare datasets'
The main purpose of the research described in the article is a comparison of performance between deep learning models and state-of-the-art machine learning models on a few popular clinical prediction tasks.
# 1. Data
The datasets prepared for the experiment were built on data extracted from the Medical Information Mart
for Intensive Care (MIMIC-III) database. MIMIC is a relational database that contains 26 tables. In sections 3.2.1-3.2.2 the authors describe the selection of:

- **observations:** the records were reduced to these obtained from first hospital admission of adults (age>15)
- **tables:** features were taken from *inputevents*, *outputevents*, *chartevents*, *labevents*, *prescriptions* 

### Features selection
From the extracted data researchers constructed 3 datasets/feature sets (section 3.2.4):

- **set A:** 17 features; preprocessed
- **set B:** 20 features; raw variables which were used for obtaining set A
- **set C:** 136 features; raw variables (contains set B)

All selected features are listed in the tables available in the article. 

# 2. Tasks definitions
In the research different prediction tasks were analyzed (section 4.1):

- **mortality** (*binary classification*): in-hospital, short-term, long-term
- **ICD-9 code group** (*multilabel classification*): ICD-9 codes grouped to 20 classes
- **length of stay** (*regression*)

# 3. Preprocessing
### Data cleaning
The MIMIC database is a source of raw data so there are many issues to solve before the application of ML models will be possible. The authors mention their preprocessing steps in sections 3.2.3-3.2.4:

- **inconsistent units:** dropping observations or converting to a unified unit (rules showed in appendix)
- **multiple recordings:** average (numerical variables) or first value (categorical variables)
- **range values:** median

### Time series features
Some of the features are time series, depending on the feature set further steps had to be taken. The time interval for sets A and B is 24h, for set C it is 1h. To obtain a single value per time step values were aggregated using:

- **summation:** usually fluids or medications
- **average:**  other

For models which do not handle time series variables, summary statistics of them were provided.

### Missing values
In the case of missings forward and backward imputation was performed. If for certain patient feature was completely missing, it was imputed with mean.

# 4. ML algorithms
Benchmark compares in general three types of prediction algorithms (section 4.2):

- **scoring methods:** SAPS-II, SOFA and new SAPS-II scores combined with simple logistic regression can be used for mortality prediction
- **Super Learner models** is an advanced method of building models ensemble. An optimally weighted combination of basic ML models is achieved by performing cross-validation. Available algorithms cover all of the most popular from simple regression to shallow neural networks.
- **Multimodal Deep Learning Model (MMDL)** is a framework that allowed the authors to create an ensemble of both Feedworward NN and Recurrent NN (more precisely Gated Recurrent Unit GRU). This solution can work with both temporal and non-temporal variables at the same time.
- additionally to results of these ensembles, where it is possible full results are presented (single ML models and NN) 

# 5. Hyperparameters

- **Super Learner**: default parameters for all base algorithms
- **MMDL**:
  - optimizer: RMSProp
  - activation function: ReLu
  - dropout rate: 0.1
  - batch size: 100
  - learning rate: 0.001 (classification) / 0.005 (regression)
  - maximum number of epoches: 250

# 6.1 Training methodology
Benchmarking schema is described in section 4.3:

- preprocessing
- preparing time series variables for SuperLearner (replaced with basic statistics)
- stratified 5-folds (3 training, 1 validation, 1 testing) cross-validation
- standardization (with parameters from the training set)

# 6.2 Metrics
Algorithms were evaluated with the following metrics:

- classification: AUROC, AUPRC
- regression: MSE

# 7. Code review
All hope in README file which precisely describes the following steps to reproduce results. After a brief exploration, all steps described in the article should have their corresponding python scripts.
