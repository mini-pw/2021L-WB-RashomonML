# Reproduction summary
In order to check whether results provided in [Predictive modeling in urgent care: a comparative study of machine learning approaches](https://academic.oup.com/jamiaopen/article/1/1/87/5032901) are true, we conducted reproduction of chosen models. Unfortunately, due to low disk capacities of our machines we had to skip preprocessing phase and our work was limited to retraining models on preprocessed data obtained from other group. 

We chose 3 models:
- Multi Layer Perceptron (MLP)
- Random forest
- Gradientboosting

and trained them for in-hospital mortality classification task on data based on summary features of time-series variables(denoted as X48 in the article). This phase could have been much easier if authors wrote more readable code and provided more comments in it.

We were only able to achieve comparable but lower results as those provided in the article. In our opinion this happened due to:
- authors trained random models without declaring the random seed
- not all exact hyperparameters provided
- updates of libraries used in code

In order to check if results from the article are achievable with any models we can make, we decided to move on with tuning and testing our own models.
