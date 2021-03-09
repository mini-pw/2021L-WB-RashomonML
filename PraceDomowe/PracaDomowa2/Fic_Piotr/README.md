# Introduction
## Article: 'Benchmarking deep learning models on large healthcare datasets'
The main purpose of the research described in the article is a comparison of performance between deep learning models and state-of-the-art machine learning models on a few popular clinical prediction tasks.
# 1. Data
The datasets prepeared for the experiment was build on data extracted from Medical Information Mart
for Intensive Care (MIMIC-III) database. MIMIC is a relational database which contains 26 tables. In sections 3.2.1-3.2.2 the authors describe selection of:

- **observations:** the records were reduced to these obtained from adults (age>15) and to only their first admission to the hospital ICU
- **tables:** the chosen tables are *inputevents*, *outputevents*, *chartevents*, *labevents*, *prescriptions* 

### Features selection
From the extracted data researchers constructed 3 datasets/feature sets (section 3.2.4):

- **set A:** 17 features; preprocessed
- **set B:** 20 features; raw variables which were used for set A
- **set C:** 136 features; raw variables 

All selected features are listed in tables available in the article. 

# 2. Tasks definitions
In the research different prediction tasks were analyzed (section 4.1):

- **mortality** (*binary classification*): in-hospital, short-term, long-term
- **ICD-9 code group** (*multilabel classification*): ICD-9 codes grouped to 20 classes
- **length of stay** (*regression*)
# 3. Preprocessing

# 4. ML algorithms

# 5. Hiperparamethers

# 6. Metrics & training methodology

# 7. Code review
