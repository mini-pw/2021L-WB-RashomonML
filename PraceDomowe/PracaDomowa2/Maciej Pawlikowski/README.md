# Praca domowa nr 1
## Artykuł nr 1: [Benchmarking deep learning models on large healthcare datasets](https://www.sciencedirect.com/science/article/pii/S1532046418300716)
### 1. Z jakich danych korzystano? Czy opisane zostały kryteria wyboru obserwacji? 

   MIMIC-III 

   MIMIC-III (CareVue)- podzbiór MIMIC-III z grubsza dopowiadający MIMIC-II 

   Użyto dwóch zestawów kryteriów inkluzji podczas selekcji cohort: wieku (brano pod uwagę tylko osoby mające więcej niż piętnaście lat) oraz uwzględniono tylko pierwsze przyjęcie danego pacjenta do szpitala. 

### 2. Definicje problemów: co było zmienną objaśnianą, czy są to problemy regresji czy klasyfikacji? 

   Klasyfikacja: 

   * Śmiertelność w szpitalach  

   * Krótko- terminowa śmiertelność (w czasie 2-3 dni od przyjęcia do szpitala) 

   * Długo-terminowa śmiertelność (30-365 dni od wypisania ze szpitala) 

   * ICD-9 code group- kody klasyfikujące grupy chorób  

   Regresja: 

   * Długość pobytu- długość czasu pomiędzy przyjęciem do szpitala, a wypisem 

 

### 3. Czy opisane zostały kroki preprocessingu? Czy w kodzie znaleźliście fragmenty odpowiedzialne za ten krok? 

   * Selekcja cohort wszystkich dorosłych (tu osoby powyżej 15 lat) oraz wzięcie pod uwagę tylko pierwszych przyjęć danych pacjentów do szpitala. 

   * Podczas ekstrakcji danych wybrano dane z tabel: inputevents, outputevents, chartevents, labevents i prescriptions 

   * Aby pozbyć się niespójnych jednostek obliczano procent występowania danej jednostki, jeśli wyniósł on mniej niż 90% konwertowano wszystkie dane do wybranej jednostki np. mg->gram, dose->ml/mg. 

   * Wiele rekordów w tym samym czasie zostało zastąpionych średnią, dla wartości numerycznych oraz wartością, która wystąpiła jako pierwsza dla kategorycznych. 

   * W przypadku styczności z wartościami przedziałowymi zastąpiono je średnią. 
   
   * W celu lepszego porównania benchmarków utworzono 3 zestawy cech: 

       * Zestaw A zawiera 17 cech preprocesowanych użytych do obliczania SAPS-II  

       * Zestaw B zawiera 20 cech- używany do obliczania SAPS-II 

       * Zestaw C zawiera 136 cech z pięciu tabel wybranych na podstawie ich niewielkich braków danych. 

   Preprocessing odbywa się z grubsza za pomocą funkcji stworzonych przez autorów np. processing_inputevents , processing_outputevents , processing_chartevents w pliku [8_processing.ipynb](https://github.com/USC-Melady/Benchmarking_DL_MIMICIII/blob/master/Codes/mimic3_mvcv/8_processing.ipynb).

 

### 4. Jakie modele ML zostały zastosowane? 

   Metody oceniania: 

   * SAPS-II- do mierzenia poważności choroby 

   * SOFA- opisuje dysfunkcje organów 

   * New SAPS-II- zmodyfikowana wersja SAPS-II 

   Modele ML: 

   * SuperLerner- nadzorowany algorytm służący znajdowaniu optymalnej kombinacji z zestawu algorytmów predykcyjnych 

   * FFN- Feedforward neural network- do modelowania danych nie związanych z czasem 

   * RNN- recurrent neural network- do modelowania sekwencji i serii czasowych 

### 5. Czy podane zostały hiperparametry modeli ML? 

   Użyto domyślnych hiper parametrów  dla wszystkich algorytmów SuperLerner, a dla modeli uczenia głębokiego użyto:  

   * Learning rate: 0.001 dla klasyfikacji/ 0.005 dla regresji 

   * Activation: ReLU 

   * Dropout rate: 0.1 

   * Batch size: 100 

   * Max epoch numer: 250 

 

 

### 6. Czy była stosowana kroswalidacja? Jakie miary oceny jakości modeli zostały zastosowane? 

   Dla wszystkich modeli predykcyjnych zastosowano 5- warstwową kroswalidację(3 warstwy na trenowanie, jedna na walidację oraz jedna na reportowanie wyników). 

   Miary jakości modeli: 

   * AUROC i AUPRC- ewaluacja modeli predykcyjnych dla klasyfikacji 

   * MSE- dla modeli regresji 
