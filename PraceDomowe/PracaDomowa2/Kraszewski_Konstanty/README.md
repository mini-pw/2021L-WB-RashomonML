# Homework number 2 - Konstanty Kraszewski

Analysis of an article [Predictive modeling in urgent care: a comparative study of machine learning approaches](https://academic.oup.com/jamiaopen/article/1/1/87/5032901).

## Goal
The goal of this study was to conduct a systematic comparative study of different ML algorithms for several predictive modeling problems in urgent care.

## Data
The study used data from the Medical Information Mart for Intensive Care (MIMIC-III) database obtained from Physionet.org which includes deidentified inpatient data from Beth Israel Hospital's critical care unit from 2005 to 2011. Only data of nonpediatric patients and admissions without multiple transfers or length of ICU stay < 24 h was considered. There were a total of 30 414 unique patients and 37 787 unique admissions.

## Tasks
The study adopted four learning tasks:
1) in-hospital mortality (binary classification), erformance evaluated using AUC, sensitivity, specificity and F1 scores;
2) length of ICU stays (multiclass classification using bins of different lengths), performance evaluated using AUC, sensitivity, specificity and F1 scores as well as microaveraged AUC and F1 scores;
3) differential diagnosis (multilabel classification for 25 most commonly appearing conditions), performance evaluated using AUC, sensitivity, specificity and F1 scores as well as microaveraged AUROC and F1 scores;
4) readmission prediction (multiclass classification using bins corresponding to time-to-readmission), erformance evaluated using AUC, sensitivity, specificity and F1 scores.

## Preprocessing
The article describes the preprocessing of patients' features, such as:
- reduction of the number of ICD-9 codes;
- taking hourly averages of time-series variables;
- imputing missing data with carry-forward strategy;
- standardization of each temporal variable by taking the difference with its mean and dividing by its standard deviation;
- quantization of age into five bins;
- further actions creating four feature representations.
Corresponding code fragments can be found in [preprocess.py](https://github.com/illidanlab/urgent-care-comparative/blob/master/preprocess.py) file.

## ML Models
The study examined two types of predictive models - collapse and sequential. There were five collapse models: SVM, Random Forest (RF), Gradient Boost Classifier (GBC), LR, Feed-forward Multi-Layer Perceptron (MLP); and two sequential models: the bidirectional Long Short-term Memory (LSTM) model and the Convolutional Neural Network with LSTM model (CNN-LSTM). The article itself does not contain all the information about the mechanics of these models, but more details and the hyperparameter tuning procedures can be found in the [Supplementary Material](https://academic.oup.com/jamiaopen/article/1/1/87/5032901#supplementary-data) provided with the article.
