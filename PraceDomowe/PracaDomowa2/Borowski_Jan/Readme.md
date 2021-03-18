# Article: 'Benchmarking deep learning models on large healthcare datasets'

The research's main purpose was to perform benchmarks comparing deep learning models and state-of-the-art machine learning models on popular clinical data.


## Data 

Data used in the analyzed paper was extracted from MIMIC-III (Medical Information Mart for Intensive Care) database. This relational database contains 26 tables.

 - **Observations** Only records contains the first admission to the hospital of a person with an age higher than 15. 
 - **Featuers** features used for resarch was exctracted form tables: *inputevents*, *outputevents*, *chartevents*, *labevents*, *prescriptions*


#### Selected Features
For research purposes, scientists prepared 3 sets of features. 

 - **Set A** 17 preprocesd features,
 - **Set B** 20 raw fetrues used in set A,
 - **Set C** 136 raw varibles (contains set B).
 
List off all selected features is available in the article. 

## Tasks 

Three diffrent prediction task was tested: 

 - **length of stay** regression,
 - **ICD-9 code group** multilabel classification (20 classes),
 - **mortality** binary classification.


## Preprocessing 

#### Data cleaning 
In this research preprocessing was needed because real data were used. Data cleaning technics used in the article:
 
 - **inconsistent units** dropping observations or converting to a unified unit
 - **multiple recordings** average for numerical values and first value for categorical 
 - **range values** converting into median 
 
 
#### Time series 
Some of the features have a form of time series. Features were extracted 24h or 48h after admission to ICU. Time intervals for set A and B are 24h, for set C it is 1h. Values were aggregated with:   

- **summation** medications, fluids etc.
- **average**  other

If the model wouldn't handle time series, summary statistics were used

#### Missing values 

To fill-in, missing values, forward and backward imputation was performed. If some patients, certain features are completely missing.  In this case, mean imputation was performed.  

## Used ML algorithms 

Benchmark includes three types of prediction algorithms: 



 - **MMDL (Multimodal Deep Learning Model)** Feedworward NN and Recurrent NN (Gated Recurrent Unit GRU). This framework allows researchers to use both temporal and non-temporal variables at the same time.
 - **scoring methods** SAPS-II, SOFA, new SAPS-II with linear models 
 - **Super Learner models** Learning algorithm that is designed to find the optimal combination from a set of prediction algorithms. The algorithm estimates the risk associated to each algorithm in the provided collection using cross-validation. Super Learner builds an aggregate algorithm obtained as the optimal weighted combination of the candidate algorithms.


## Hyperparameters 

- **MMDL**:
  - optimizer: RMSProp
  - activation function: ReLu
  - dropout rate: 0.1
  - batch size: 100
  - learning rate: 0.001 (classification) / 0.005 (regression)
  - maximum number of epoches: 250

- **Super Learner**: default parameters for all base algorithms



## Methodology of performed Benchmark 

#### Training methodology 
 - data preprocesing 
 - prepearing time series for SuperLearner 
 - 5 folds corss-validation (3 train, 1 test, 1 validation)
 - standarization 
 
 
#### Used Metrics 

- classification: AUROC, AUPRC
- regression: MSE

