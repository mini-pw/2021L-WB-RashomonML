# Yevhenii Vinichenko - Praca domowa 2
Celem tej pracy domowej była analiza struktury artykułu [Predictive modeling in urgent care: a comparative study of machine learning approaches](https://academic.oup.com/jamiaopen/article/1/1/87/5032901). Poza tym należało również stworzyć krótki konspekt danego artykułu.
## Struktura artykułu
Jest to artykuł naukowy, którego podział na rozdziały wygląda następująco:
- Abstract
- Cel(Objective)
- Design
- Metody pomiaru wydajności modeli(Measurements)
- Wyniki i dyskusja(Results and discussion)

## Konspekt artykułu
### Wykorzystane dane. Kryteria wyboru obserwacji
Autorzy artykułu wykorzystali ogólniedostępną bazę danych **MIMIC-III**. Pod uwagę brano jedynie obserwacje dorosłych pacjentów(wiek >= 18). Pomijano też obserwacje, gdzie pobyt na OIT(ICU stay) wynosił ponad 24 godziny lub pacjenta przewożono do innego szpitalu.
### Definicje problemów
W artykule rozważano następujące problemy:
- Śmiertelność w szpitalach - **klasyfikacja** binarna
- Długość pobytu w szpitalu - **klasyfikacja** z wieloma klasami(multiclass classification)
- Diagnostyka różnicowa - **klasyfikacja** z wieloma etykietami(multilabel classification)
- Przewidywanie readmisji - **klasyfikacja** z wieloma klasami(multiclass classification)

### Preprocessing
W celu uproszczenia pracy z danymi, autorzy podjęli następujące kroki:
#### Szeregi czasowe
- Dla modeli klasycznych, dane, mające postać szeregów czasowych, uogólniano do średnich z 48 godzin
- Dla modeli typu collapse z danych, mających postać szeregów czasowych, tworzono 5 cech: min, max, średnia, odchylenie standardowe oraz liczba pomiarów podczas pobytu
- Standaryzacja

#### Tekst
- word2vec
- onehot

#### Binning
- Niektóre dane demograficzne(np. wiek) podzielono na grupy(bins)


### Modele uczenia maszynowego
Autorzy skupili się na dwóch typach modeli:
- Collapse model - wszystkie standardowe modele, które nie uwzględniają danych czasowych: SVM, las losowy, LR(Logistic Regression) oraz MLP(Feed-forward Multi-Layer Perceptron)
- Sequential model - były to dwa modele RNN(Recurrent Neural Network): Long Short-term Memory (LSTM) model i CNN(Convolutional Neural Network)

### Hyperparametry
Autorzy podają hyperparametry dla modeli RNN:
#### Klasyfikacja binarna i z wieloma etykietami
- funkcja aktywacji - `sigmoid`
- funkcja kosztu - binarna `cross-entropy`

#### Klasyfikacja z wieloma klasami
- funkcja aktywacji - `softmax`
- funkcja kosztu - kategoryczna `cross-entropy`
 
W jakości algorytmu uczącego w obu przypadkach użyty został Adam optimizer(zoptymalizowana wersja SGD(Stochastic Gradient Descent)) z wstępnym `learning rate` równym 0.005. Regularyzacja została zaimplementowana za pomocą technik `dropout` i `L2-regularization` w każdej z warstw LSTM.

### Miary oceny wydajności modeli. Kroswalidacja
Do oceny działania modeli wykorzystano następujące metryki:
- F1-score
- AUC(Area Under Curve)
- sensitivity score, specificity score

Autorzy użyli też kroswalidacji 5-warstwowej(5-fold).
### Kod
Autorzy podają w artykule link do ich repozytorium: https://github.com/illidanlab/urgent-care-comparative. Można tam znaleźć kod(`python`) zarówno do preprocessing'u jak i modeli. Do preprocessing'u autorzy tworzyli własne funkcje, zaś w implementacji modeli korzystali oni z wbudowanych(głównie `keras`). Kod jest w miarę czytelny i niewątpliwie nadaje się do reprodukowania. 

