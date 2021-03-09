# Introduction
### Article: 'Benchmarking deep learning models on large healthcare datasets'
The main purpose of the research described in the article is a comparison of performance between deep learning models and state-of-the-art machine learning models on a few popular clinical prediction tasks.
# 1. Data
The datasets prepeared for the experiment were build on data extracted from Medical Information Mart
for Intensive Care (MIMIC-III) database. MIMIC is a relational database which contains 26 tables. In sections 3.2.1-3.2.2 the authors describe selection of:

- **observations:** the records were reduced to these obtained from first hospital admission of adults (age>15)
- **tables:** features were taken from *inputevents*, *outputevents*, *chartevents*, *labevents*, *prescriptions* 

### Features selection
From the extracted data researchers constructed 3 datasets/feature sets (section 3.2.4):

- **set A:** 17 features; preprocessed
- **set B:** 20 features; raw variables which were used for obtaining set A
- **set C:** 136 features; raw variables (contains set B)

All selected features are listed in tables available in the article. 

# 2. Tasks definitions
In the research different prediction tasks were analyzed (section 4.1):

- **mortality** (*binary classification*): in-hospital, short-term, long-term
- **ICD-9 code group** (*multilabel classification*): ICD-9 codes grouped to 20 classes
- **length of stay** (*regression*)

# 3. Preprocessing
### Data cleaning
The MIMIC database is a source of raw data so there are many issues to solve before application of ML models will be possible. The authors mention their preprocessing steps in sections 3.2.3-3.2.4:

- **inconsistent units:** dropping observations or converting to unified unit (rules showed in appendix)
- **multiple recordings:** average (numerical variables) or first value (categorical variables)
- **range values:** median

### Time series features
Some of features are time series, depending on feature set further steps had to be taken. The time interval for sets A and B is 24h, for set C it is 1h. To obtain single value per time step values were aggregeted using:

- **summation:** usually fluids or medications
- **average:**  other

For models which do not handle time series variables, summary statistics of them where provided.

### Missing values
In case of missings forward and backward imputation was performed. If for certein patient feature was completely missing, it was imputed with mean.

# 4. ML algorithms

# 5. Hiperparamethers

# 6. Metrics & training methodology

# 7. Code review
