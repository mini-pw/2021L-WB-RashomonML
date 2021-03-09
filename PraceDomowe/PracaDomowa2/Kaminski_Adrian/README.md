<h1 style="text-align:center;">Adrian Kamiński</h1>

<h2 style="text-align:center;">Homework 2</h2>

## Article: [Benchmarking deep learning models on large healthcare datasets](https://www.sciencedirect.com/science/article/pii/S1532046418300716)

<details>
<summary><strong><em>Given task</em></strong></summary>
    
1. Z jakich danych korzystano? Czy opisane zostały kryteria wyboru obserwacji?
2. Definicje problemów: co było zmienną objaśnianą, czy są to problemy regresji czy klasyfikacji?
3. Czy opisane zostały kroki preprocessingu? Czy w kodzie znaleźliście fragmenty odpowiedzialne za ten krok?
4. Jakie modele ML zostały zastosowane?
5. Czy podane zostały hiperparametry modeli ML?
6. Czy była stosowana kroswalidacja? Jakie miary oceny jakości modeli zostały zastosowane?

</details>



### 1 Data used and observations included


Medical Information Mart for Intensive Care III (MIMIC-III) - this publicly available dataset was used in this study.

This database contain informations about patients admitted to an Intesive Care Unit (ICU) at Beth Isreal Deaconess Medical Center in Boston, Massachusetts during 2001 to 2012.

<details>
<summary><strong><em>Specific tables that were used</em></strong></summary>

- inputevents
- outputevents
- chartevents
- labevents
- prescriptions
    
</details>
Authors have chosen only adult patients (>15years at the time of ICU admission) and then, for each patient, they only used their first admission (to prevent possible information leakage in the analysis).

### 2 Prediction tasks

- **mortalilty risk** - a binary <u>classification</u> task 
    - *In-hospital mortality*
    - *Short-term mortality* (2 and 3 days after admission to ICU)
    - *Long-term mortality* (30 days and 1 year after being discharged from the hospital)
- **ICD-9 code group** - a <u>classification</u> task (multi-task prediction) (ICD codes are used to classify diseases this codes were grouped into 20 diagnosis groups)
- **Length of stay** - <u>regression</u> problem

### 3 Preprocessing/Codes

In article these steps of preprocessing were described:
- *Cohort selection*
    - only adult patients >15 years
    - only first admission
- *Data extraction* 
    - tables listed in **1 Data used and observations included**
- *Data cleaning*
    - different units and data types ('dose'/'mg' and some variables were recorded in both numeric and string data type)
    - some variables have multiple values recorded at the same time
    - for some variables the observation was recorded as a range rather than a single measurement
- *Feature selection and extraction*
    - **Feature A** - consists of the 17 features used in the calculation of the SAPS-II score, outliers dropped and relevant features merged
    - **Feature B** - consists of the 20 features related to the 17 features used in SAPS-II score, raw values
    - **Feature C** - consists of 136 raw features selected from the 5 tables mentioned in **1**

On [GitHub](https://github.com/USC-Melady/Benchmarking_DL_MIMICIII) we can find codes that can help with the reproduction of this study. 

Codes are commented, so this should be helpful if we wish to recreate this work/part of it. There are a lot of files but README.md on the first page is quite informative and it definitaly helps to figure out what each file is responsible for.

Most function used are written by authors so it can take a while to figure out how this whole thing work together.

### 4 Prediction algorithms

Scoring methods:
- SAPS-II -  Simplified Acute Physiology Score
- New SAPS-II - a modified version of SAPS-II
- SOFA - Sepsis-related Organ Failure Assessment score

Machine Learning models:
- Super Learner algorithm 
    - Super Learner I: Super Learner with categorized variables
    - Super Learner II: Super Learner with non-transformed variables
    
<details>
<summary><strong><em>All algorithms used in Super Learner algorithm</em></strong></summary>

    - Standard logistic regression
    - Logistic regression based on the AIC
    - Generalized additive model
    - Generalized linear model with penalized maximum likelihood
    - Multivariate adaptive polynomial spline regression
    - Bayesian generalized linear model
    - Generalized boosted regression model
    - Neural network
    - Bagging classification trees
    - Pruned recursive partitioning and Regression Trees
    - Random forest
    - Bayesian additive regression trees

</details>

- Deep Learning Models
    - FFN - Feedforward neural networks
    - RNN - Recurrent Neural Networks
    - MMDL - Multimodal Deep Learning Model

### 5 Hyper parameters

For each algorithm in Super Learner default hyper parameters were used. 

Deep learning models parameters were given click below to find theirs exact value.

<details>
<summary><strong><em>Deep learning model parameters</em></strong></summary>

- All models were trained with RMSProp optimizer method with learning rate of 0.001 on classification tasks and 0.005 on regression tasks
- ReLU activation
- dropout rate = 0.1
- batch size = 100
- max epoch number = 250
    
</details>

### 6 Cross-validation/Evaluation metrics

*Cross-validation* was used. To be specific, a 5-fold cross validation was conducted and the mean and standard error of performance scores of all 5 testing folds was reported. 

Authors used the following evaluation metrics:
- Area under the ROC curve (AUROC) and Area under Precision-Recall Curve (AUPRC) - on classification tasks
- Mean Squared Error (MSE) - on regression task

### Results

To sum up, in most cases MMDL model was the most accurate.

And in every prediction task, the results of MMDL model trained on **Feature C** (136 raw feature) were almost always better then results of any other prediction algorithm (used in this article).