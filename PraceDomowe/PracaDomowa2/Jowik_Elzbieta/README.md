---
title: "An outline of [*Predictive modeling in urgent care: a comparative study of machine learning approaches*](https://academic.oup.com/jamiaopen/article/1/1/87/5032901) research paper"
author: Elzbieta Jowik
date: "Created on Mar 04, 2021."
output: html_document
---

## 1. Statement of the problem 
A systematic comparative study of different ML algorithms for 4 predictive modeling problems in urgent care (eg mortality and prediction, differential diagnostics, and disease marker discovery).  

## 2. Dataset details & observations selection   
The study was conducted basing on medical histories, physiological time-series, and demographics data from the the Medical Information Mart for Intensive Care (MIMIC-III) database obtained from Physionet.org. MIMIC-III captures hospital admission data, laboratory measurements, procedure event recordings, pharmacologic prescriptions, transfer and discharge data, diagnostic data, and microbiological data from deidentified inpatient critical care unit from 2005 to 2011.  


## 3. Methodology  
### 3.1. Review of applied data pre-processing techniques 
#### 3.1.1. Criteria for the observations selection
Only nonpediatric patients (age 18) and admissions without multiple transfers or length of ICU stay <24 h participated in the study. Thus there were a total of 30 414 unique patients and 37 787 unique admissions left out of 46 520 unique patients and 58 976 unique admissions.

#### 3.1.2. Feature variables pre-processing
- Diagnosis codes are designed to capture the group-level disease specificity by only using the first 3 letters of their full length codes. In this way, the feature dimension got reduced to 942 group codes.  
- Since sampling frequency differed greatly per inpatient admission the reaserchers took the averages of time-series variables up to the first 48 hours of each admission across all prediction tasks. As a result it was possible to capture the temporal patterns of complex diseases hourly.  
- Each temporal variable was standardized by taking the difference with its mean and dividing by its standard deviation. 
- Missing data were imputed with the carry-forward strategy at each time-stamp.
- Age variable was quantized into 5 bins $(18, 25), (25, 45), (45, 65),
(65, 89), (89+, )$. 
- 4 types of feature representation strategies were compared across. All of them are described in detail within the article :
    - **Visit-level representation (physiologic features only)** - for collapse models [Support Vector Machines (SVM), Logistic Regression (LR), Ensemble Classifiers, and Feed-Forward MLPs), raw hourly averages for each time-series variable was converted into 5 summary features per variable: minimum value, maximum value,
mean, standard deviation, and number of observations for the duration of the admission. For sequential models, the researchers simply used the standardized hourly average data per admission to establish this baseline. 
    - **History-level representation (diagnostic history only)** - each admission was treated like a sentence, with medical events occurring as neighboring words. As an additional baseline, sum of one-hot vectors was also used to represent diagnostic
history for collapse models, denoted as onehot.
    - **Combined representation** -  for collapse models, Word2Vec embeddings of diagnostic history was concatenated with summary features from time-series data as features for prediction.  For sequential models, the researchers utilized 2 separate layers of input: one of them was fed into recurrent layer, and its output was merged with the other one.
    - **Embedded representation** - in this representation scheme, both diagnostic history and timeseries variables were treated as word vectors for representation. Time-series variables were discretized and included in the feature vector depending on whether or not the observed event was within 1 standard deviation of its mean value. In this setting, it was possible to map abnormal time-series values with frequently cooccurring diagnostic codes in the same word-vector space.


### 3.2. General study assumptions
- Four learning tasks adopted in the study were perceived as the benchmarks of ML algorithms.  
- For each given task, performance was estimated using standard measures including the area under the receiver operating characteristic (AUC) curve, F-1 score, sensitivity, and specificity. Microaveraged AUC was used for multiclass classification models.  
- Examined types of predictive models:    
    - Collapse models: SVM, Random Forest (RF), Gradient Boost Classifier (GBC), LR, and Feed-forward Multi-Layer Perceptron (MLP)
    - Sequential models: 2 RNN models ie the bidirectional Long Short-term Memory (LSTM) model and the Convolutional Neural Network w/ LSTM model (CNN-LSTM).


- To determine the optimal settings per model the researchers performed grid-search based on **cross-validation** performance for each learning task. The results of hyper-parameter tuning, procedures were not explicitly stated in the article. Instead, they were included in [Supplementary Material](https://academic.oup.com/jamiaopen/article/1/1/87/5032901#supplementary-data) and they turned out to be as follows:  
    - **Support Vector Machine** was trained with RBF kernel and penalty of 0.001 performed best. 
    - **Logistic regression** was trained on L2 penalty of 0.005 and 0.01 performed best on binary and multi-label tasks, respectively.
    - **Sparse logistic regression** (L1 penalty of 0.01) performed best on multi-class LOS prediction. 
    - The best performing feed-forward **MLP model** used two dense-layers with each layer containing 256 neurons. Regularization was achieved by adding a dropout of 0.5 at each hidden layer and a L2 regularizer of 1e-6. 
    - In case of sequential models for binary and multilabel classification tasks, sigmoid activation function was used at the fully connected output layer, and binary cross-entropy was used as loss function. For the multiclass case (eg LOS and readmission bins), softmax activation
was used at the output layer with categorical cross-entropy as loss function. Adam optimizer with initial learning rate of 0.005 was used in both cases

### 3.3. Subsequent problems types  

#### 3.3.1 In-hospital mortality  
In-hospital mortality task was modeled as a binary classification problem. 

**Measurements**  

The study evaluated performance of predictive models using AUC. Sensitivity, specificity, and f1-scores were included to aid the interpretation of AUC scores due to the presence of class-imbalance.  

#### 3.3.2. Length of stays  
Lenght of stays task was formulated as a multiclass classification problem using bins of lengths: $$(1, 2), (3, 5), (5, 8), (8, 14), (14, 21), (21, 30), (30+, )$$ to reflect the range of possible LOS values in terms of days. Such multi-class formulation of this problem mapped each data point to one label from a set of possible labels. In the case of LOS, each outcome fall into one of several mutually exclusive ranges.  

**Measurements**  

To evaluate the performance on LOS task, AUC, f1-score, sensitivity, and specificity were calculated for each bin, and a microaveraged AUC and f1 scores were calculated for the overall performance of the model across all bins. AUC and f1-scores were chosen to facilitate the
interpretation of LOS performance in comparison with other tasks.


#### 3.3.3. Differential diagnoses  
The task was to examine the top 25 most commonly appearing conditions (ICD-9 codes) in MIMC-III using a multilabel classification framework. In contrast to the multi-class model, the multi-label framework assumes that each patient can assume multiple labels. Diagnostic prediction model returns a 25-dimensional vector which gives a probability of whether or not the visit is *positive* for the corresponding ICD-9 code at the index of the output vector. This task is essentially treated as 25 mini-tasks for each visit.  

**Measurements**  

To evaluate the performance of predictions, AUC, f1-score, sensitivity, and specificity scores were calculated for each disease label, and a microaveraged AUROC and f1-score were calculated for each admission.

#### 3.3.4. Readmission prediction    
At first 2 types of readmissions were investigated. For the latter, in order to generate 6 classes  associated with each admission that corresponded to observed time-to-readmission: $$(1, 30), (30, 90), (90, 180), (180, 360), (360, 720), (720+, )$$ measured in days there were created 6 bins. In the end the prediction problem was formulated as a multiclass prediction problem. 

**Measurements**  

AUC, F1, sensitivity and specificity scores.

### 3.4. Methodology reproducibility  

The code for the features and models presented in the article is available at: <https://github.com/illidanlab/urgent-care-comparative>. The attached scripts themselves seem to have reusable potential - each individual processing step is separated in dedicated function and properly commented. The comments inside the code don't scrupulously go through all the nuances, but they do allow to efficiently find yourself inside the code and navigate between individual fragments. The functions dedicated to the conducted research are complex (it is vissible that they were created specifically for the study) and use many built-in Python functionalities. In general, the code is neat and understandable.









