# Praca domowa nr 2

### Artykuł nr 2: Predictive modeling in urgent care: a comparative study of machine learning approaches 

https://academic.oup.com/jamiaopen/article/1/1/87/5032901 

## 1. Z jakich danych korzystano? Czy opisane zostały kryteria wyboru obserwacji?

W przygotowaniu modeli opisanych w artykule korzystano z bazy danych MIMIC-III (Medical Information Mart for Intensive Care), do której otrzymano dostęp poprzez stronę www.physionet.org. Baza danych zawiera dane pacjentów z odziału intensywnej terapii w latach 2005-2011, które obejmują: dane dotyczące przyjęcia i wypisania ze szpitala, pomiary laboratoryjne, recepty, dane diagnostyczne i mikrobiologiczne.

Do badań wybrano rekordy dotyczące osób dorosłych (powyżej 18. roku życia), pomijając przypadki pobytu poniżej 24 godzin oraz wielokrotnych transferów.

## 2. Definicje problemów: co było zmienną objaśnianą, czy są to problemy regresji czy klasyfikacji?

Artykuł dotyczy tworzenia modeli dla czterech różnych zadań (zmiennych objaśnianych):

* przewidywanie, czy dany pacjent umrze w szpitalu - problem klasyfikacji binarnej
* przewidywanie długości pobytu - problem klasyfikacji wieloklasowej: długość pobytu została podzielona na przedziały - kubełki
* odróżnienie określonych chorób, które powodują podobne objawy - problem klasyfikacji wieloklasowej
* przewidywanie, czy dany pacjent powróci do szpitala - problem klasyfikacji wieloklasowej: liczb dni, po których pacjent zostanie przyjęty ponownie została podzielona na przedziały - kubełki


## 3. Czy opisane zostały kroki preprocessingu? Czy w kodzie znaleźliście fragmenty odpowiedzialne za ten krok?

Tak, są opisane kroki preprocessingu.

1. Pogrupowano choroby poprzez używanie tylko pierwszych trzech liter (z ponad 14 tysięcy możliwości wyodrębiono około 950).
2. Korzystano ze średniej wartości tzw. temporal variables. Zostały one również ustandaryzowane - wzięto różnice między średnią a wartością i podzielono przez odchylenie standardowe.
3. Wiek rozdzielono do kubełków oznaczających pewien przedział wiekowy.
4. Badania były wykonane na czterech różnych reprezentacjach opisanych danych: 
    1. reprezntacja wizyt: tylko cechy fizjologiczne
    2. reprezentacja hitorii: tylko diagnozy historyczne
    3. połączone dane dt. wizyt i informacje demograficzne
    4. historia diagnozy oraz cechy fizjologiczne

## 4. Jakie modele ML zostały zastosowane?

W badaniach zostały użyte następujące modele ML:

* Collapse models: SVM, Random Forest, Gradient Boost Classifier, LR oraz Feed-forward Multi-Layer Perceptron.

* Sequential models: (deep learning) Bidirectional Long Short-term Memory (LSTM) model oraz Convolutional Neural Network w/ LSTM model (CNN-LSTM)

## 5. Czy podane zostały hiperparametry modeli ML?

W celu znalezienia najbardziej optymalnych hiperparametrów przeprowadzono tzw. grid-reserach opierający się na **kroswalidacji**. W artykule nie podano dokładnych wartości tych parametrów, jednak pojawiły się one w *Supplementary Materials* - dodatkowym dokumencie dołączonym do artykułu.


## 6. Czy była stosowana kroswalidacja? Jakie miary oceny jakości modeli zostały zastosowane?

Tak, kroswalidacja była stosowana przy szukaniu hiperparametrów, tak jak opisałem w punkcie 5.

Dla każdego z modeli jego jakość została opisana standardowymi miarami, takimi jak AUC (pole pod wykresem krzywej ROC), wskaźnik F1, miara precision, miara recall oraz Microaveraged AUC dla modeli klasyfikacji wieloklasowej.

## 7. Uwagi dotyczące kodu

Kod użyty do badań został umieszczony na https://github.com/illidanlab/urgent-care-comparative. Na etapie preprocessingu autorzy stworzyli własne funkcje. Zawierają one ogólne komentarze, pozwalające zorientować się, co teraz dany kod ma robić. Wstawiony kod jest w postaci skryptów napisanych w języku Python. Brakuje mi jednak dokładniejszych opisów, które mogłyby pojawić się we wstawkach markdownowych w notebooku. Jednakże, kod został udostępniony w postaci skryptów i nie możemy oczekiwać dłuższych opisów.






