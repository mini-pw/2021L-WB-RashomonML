### Reproduction stage summary

During preparing data for analysis of the article "Predictive modeling in urgent care: a comparative study of machine learning approaches" we encountered several problems with some of SQL files, for instance functions concerning time were written incorrectly. 

The main problem was that the script supposed to preprocess data did not work properly. To solve this problem we started debbuging the code and we found out that some lines around an invalid code were redundant and after fixing one of them the code started working.

The last problem that appeared during preparing data occurred in Main. The authors of the article did not refer to the package from which they used function, causing the whole script to fail. We repaired by adding prefix with library name to this function. 

Fortunately finally we managed to preprocess data, so that we were able to conduct Rashomon analysis.