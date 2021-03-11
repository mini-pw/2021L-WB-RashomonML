# Homework 2 

Analysys of the article 'Benchmarking deep learning models on large healthcare datasets'. 

## Datasets

Reaserch described in the article uses MIMIC-II datasets of patients admitted to ICU at the Beth Israel Deaconess Medical Center in Boston, Massachusetts during 2001 to 2012. There were only used data of adults(aged 15-years or above) and only patient's first admission. 

## Defined problems

* Mortality prediction
* Forecasting length of stay 
* ICD-9 code group prediction

## Preprocessing

* Cohort selection - only patients that were >15 years old and only their first admission were taken into consideration. It was done to prevent information leakage and to ensure similar experimental settings compared to related works. 

* Data extraction - 5 out of 26 tables in the MIMIC-III were used in the reaserch. 

* Data cleaning - there were 3 issues conected with data cleaning: 

  - Inconsistency in the recording (units) of certain variables. If >=90% was in the same unit, then the rest was droped, else all values were coverted to the same unit. 
  
  - Some variables have multiple values recorded at the same time. For numerical recorings there was taken the averege of the recordings. For categorical features it was kept first occured value. 
  
  - Some variables was recorded as range rather than a single measurment. To hanlde it, it was taken the median of the range. 
  
* Feature selection and extraction - there were selected three sets of features to enable exhaustive benchmarking comparison study: 

  - Feature Set A - This feature set consists of the 17 features used in the calculation of the SAPS-II score.
 
  - Feature Set B - This feature set consists of the 20 features related to the 17 features used in SAPS-II score. 
 
  - Feature Set C - This feature set consists of 136 raw features selected from the 5 tables mentioned in Data extraction and includes the 20 features of Feature set B. 

## ML models

* Scoring methods: SAPS-II, SOFA, new SAPS-II
* Super Learner models - a learning algorithm that estimates the performers of ML learning algorithms using crossvalidation and then finds optimal combination. 
* Deep Learning Models - algorithm inpired by the structure of the human brain. It is used with great success in for example: speech recognition, computer vision or analyzing health-raleted data. 

## Hyper parameters

* Super Learner models - default hyper parameters. 
* Deep Learning Models: 
  - 0.001 on classification tasks, 
  - 0.005 on regression tasks, 
  - 0.1 dropout to all the deep learning models, 
  - 100 - the batch size, 
  - 250 - max epoch number. 
  
## Cross validation 

All the data was divided to 5 folds with stratified cross validation. 

To measure the model quality there were used AUROC and AUPRC. 
