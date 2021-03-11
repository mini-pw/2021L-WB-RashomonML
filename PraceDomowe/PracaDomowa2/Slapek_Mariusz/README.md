## Praca domowa nr 2   

### *Predictive modeling in urgent care: a comparative study of machine learning approaches*

#### Z jakich danych korzystano? Czy opisane zostały kryteria wyboru obserwacji?  

The Medical Information Mart for Intensive Care (MIMIC-III) database was used in study (MIMIC-III captures hospital admission data, laboratory measurements, procedure event recordings, pharmacologic prescriptions, transfer and discharge data, diagnostic data, and microbiological data from 46 520 unique patients). In study was considered only nonpediatric patients (age 18) and discounting admissions without multiple transfers or length of ICU stay <24 h (30 414 unique patients and 37 787 unique admissions).


#### Definicje problemów: co było zmienną objaśnianą, czy są to problemy regresji czy klasyfikacji?

Problems:
- *In-hospital mortality* - binary classification problem  
- *Length of stays* - multiclass classification problem using bins of lengths (1, 2), (3, 5), (5, 8), (8, 14), (14, 21), (21, 30), (30+, )    
- *Differential diagnoses* - multilabel classification problem   
- *Readmission prediction* - multiclass prediciton problem   


#### Czy opisane zostały kroki preprocessingu? Czy w kodzie znaleźliście fragmenty odpowiedzialne za ten krok?

The steps of reprocessing were described:  
- diseases were grouped: using the first 3 letters of their full length codes (942 group coed)  
- used the averages of time-series variables (temporal variables)   
- temporal variables were standarized  
- carry-forward strategy for missing data  
- 5 bins - (18, 25), (25, 45), (45, 65), (65, 89), (89+, )  
- 4 types of feature representation:
  * visit-level representation    
  * history-level representation  
  * combined representation   
  * embedded representation   


#### Jakie modele ML zostały zastosowane?

Models used in research:
- collapse models:
  * SVM  
  * Random Forest   
  * Gradient Boost Classifier   
  * LR  
  * Feed-forward Multi-Layer Perceptron  

- sequential models:
  * Bidirectional Long Short-term Memory (LSTM) model  
  * Convolutional Neural Network w/ LSTM model (CNN-LSTM)

#### Czy podane zostały hiperparametry modeli ML?

In research was used cross-validation for each learning task. Hyperparameters tuning were not explicitly stated in the article. Instead of they were added in suplementary material (section 8.2).


#### Czy była stosowana kroswalidacja? Jakie miary oceny jakości modeli zostały zastosowane?

Cross-validation was used (5-fold). Measures of model quality:
- AUC
- f1-score
- precision
- recall
- sensitivity and specificity scores
- microaveraged AUC (multiclass classification problem)

#### Uwagi dotyczące kodu

Code: https://github.com/illidanlab/urgent-care-comparative.


Authors used built-in Python functionalities, but unfortunetely they did not keep the code clean (there are few comments, *typing* is not used, variables names are not intuitve).
