---
title: "An outline of [*Benchmarking deep learning models on large healthcare datasets*](https://www.sciencedirect.com/science/article/pii/S1532046418300716)"
author: Ada Gąssowska
date: "10.03.2021"
output: html_document
---


## 1. Statement of the problem.

In this article the researchers present the benchmarking results for several clinical prediction tasks such as mortality prediction, length of stay prediction etc. using Deep Learning models next to ensemble of machine learning models

## 2.Analysis of used data and criteria for the selection of observations.

The researchers conducted their studies basing on MIMIC-III dataset, which contains data associated with 53,423 distinct hospital admissions for adult patients (aged 15 years or above) and 7870 neonates admitted to an ICU at the BIDMC. For each patient, only the first ICU admission was included. The dataset contains information about the patients such as age, gender, if any operations were performed, lab results, prescribed meds etc. For the benchmarking datasets, data was extracted from 5 of the 26 tables in this dataset (inputevemts, outputevents,chartevents,labevents,prescriptions), as these tables provide the most relevant clinical features for the tasks considered in the research.

## 3. Methodology

### 3.1. Data preprocessing 


The following steps of data preprocessing were mentioned in the article. The code fragments for this part of research could be easily found at https://github.com/USC-Melady/Benchmarking_DL_MIMICIII/tree/master/Codes/mimic3_mvcv.


#### 3.1.2 Observation selection

As mentioned before only patients whose age was >15years at the time of tha admission were considered. Also only their first admission was used in the benchmark dataset, to avoid any data leakage. 

#### 3.1.3 Data cleaning

As the data from MIMIC-III database has lots of erroneous entries (noise, missing values, outliers, duplicates, incorrect records, clerical mistakes), these issues had to be handled by the researchers. the article describes how they handled the problems of inconsistent units, multiple recordings at the same time and range of feature values. 

#### 3.1.4 Feature selection

Three sets of features were selected. Set A consists of 17 features used in the calcultion of the SAPS-II score. Set B consists of 20 features related to the features from the Set A (in this set all the raw values were considered). Set C consists of 136 raw features selected from the 5 tables mentioned (it includes features from the Set B). On these sets non-time series and time series features, that would be used in the experiments, were obtained. The article describes how they were handled. The features from the first 24 hours and first 48 hours after the admission were extracted. If there were multiple readings for particular time step, they were aggregated. Depending on the feature, either the average or the summation was taken into consideration. 


### 3.2 Benchmarking experiments.

#### 3.2.1 Prediction tasks.

The researchers described the predictions tasks which represent some of the important problems in critical care research. These are the tasks that are commonly used to benchmark machine learning algorithms. In the article they describe three different prediction tasks. Mortality prediction is formulated as binary classification task (the label indicates the death event for a patient). Couple of morality prediction benchmark tasks may be considered, such as In-hospital mortality prediction, shor-term mortality prediction and long-term mortality prediction. ICD-9 code group prediction is formulated as classification task to predict the diagnosis code group for each admission (nearly every health condition can be assigned a unique ICD-9 code group where each group usually include a set of similar diseases). The last consideret prediction problem is the Length of stay prediction. It is formulated as regression problem. In the article we can see the distribution of the length of stay for the patients. 


#### 3.2.2 Prediction algorithms. 

In this section they describe all the prediction alghoritms and the scoring systems that were used for benchmarking tasks. 


The *scoring methods* include SAPS-II, wchich is a scoring system designed to measure the severity of the disease for patients admitted to an ICU; SOFA, which stands for Sequential Organ Failure Assessment score, and New SAPS-II which is a modified version of SAPS-II which is obtained by fitting a logistic regression model using the same variables as those used in SAPS-II.

The researchers used *Super Learner models*. Super Learner is a learning algorithm which finds the optimal comination from a ser of prediction algorithms. It is built on theory of cross-validation. It requiers user-defined learning algorithms such as logistic regression, regression trees, random forest and shallow neural networks. In the article we may find specific description of how the Super Learner works. In the tests two variants of the Super Learner algorithm were used. The researchers provieded a table which describes algorithms used in Super Learner with corresponding R packages and Python libraries which can be very useful for reproduction. 

*Deep Learning Models* are described in other paragraph than other models, as the article focuses on their performance. Different neural networks are described, such as Feedforward Neural Networks, Recurrent Neural Networks and Multimodal Deep Learning model.

For all the prediction methods, 5-fold crossvalidation was conducted, and the mean and standard error of performance scores of all 5 testing folds. Area under the ROC curve and Area under Precission-Recall Curve were used to report the performance on the classifiaction tasks, and Mean Squared Error was uses to report results on the regression task. 

The researchers used the default parameters for each base algorithm in the Super Learner. 

As for the Deep Learning models, they were all trained with RMSProp  optimizer, with the learning rate of 0.001 on classifiaction tasks and 0.005 on regressions task. The ReLU activation was used for all the models. They also applied dropout with dropout rate set to 0.1, to avoid overfitting. The batch size was chosen to 100 ant the max epoch number was fixed at 250. Early stopping with best weight and batch normalization were applied. 

After this part the results were described, but since  it's not the task, I'm not going to describe them in this outline.

### 3.3 Methodology reproducibility

The code available at https://github.com/USC-Melady/Benchmarking_DL_MIMICIII/tree/master/Codes/mimic3_mvcv is quite well commented. The researchers wrote a lot of their own functions. The code seems to be clean and easy to read. The files are well named. 







