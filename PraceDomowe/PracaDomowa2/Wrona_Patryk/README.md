
# Patryk Wrona - Praca domowa 2

W tej pracy domowej przeanalizuję strukturę artykułu [Benchmarking deep learning models on large healthcare datasets](https://www.sciencedirect.com/science/article/pii/S1532046418300716). Przeanalizuję również najważniejsze tematy w nim zawarte oraz zwrócę uwagę na kod dołączony do artykułu.

## Struktura artykułu
Artykuł ma strukturę artykułu naukowego. Można wyróżnić w nim następujące po sobie części:

- Abstract
- Wstęp (Introduction)
- Powiązane prace (Related works)
- Opis zbioru danych MIMIC-III (MIMIC-III dataset)
- Eksperymenty Benchmarkowe (Benchmarking experiments)
- Streszczenie/wnioski końcowe (Summary)
- Podziękowania (Acknowledgments), Załączniki (Appendices), Referencje (References)

Wszystkie te części spełniają dobrze swoją rolę, typową dla artykułu naukowego.

## Konspekt artykułu

### Użyte dane. Kryteria wyboru obserwacji.

W swojej pracy, autorzy użyli następujących publicznie dostępnych zbiorów danych, pochodzących z Beth Israel Deaconess Medical Center:

- **MIMIC-III**
- **MIMIC-III CareVue** (podzbiór MIMIC-III, dane odnotowane używając wyłącznie systemu Philips CareVue)

Obserwacje dobierano używając selekcji Cohort, aby dotyczyły one tylko dorosłych pacjentów (tutaj wiek > 15). Ponadto, dla każdego pacjenta uwzględniano tylko pierwsze przyjęcie do szpitala (*first admission*), a resztę usuwano. 

### Definicje problemów: zmienne objaśniane - regresja/klasyfikacja

Używając modeli uczenia maszynowego, autorzy starali się rozwiązać poniższe zadania predykcyji:

- śmiertelność krótkoterminowa(po 2 i 3 dniach), długoterminowa, ogólna (po przyjęciu na oddział) - **klasyfikacja**
- ICD-9 Code Group( kody klasyfikujące rodzaj choroby) - **klasyfikacja** z wieloma etykietami (multi-label)
- długość pobytu w szpitalu - **regresja**

### Preprocessing

W celu uprzedniego przygotowania danych do uczenia, na zbiorze danych (oprócz selekcji Cohort) dokonano również poniższych metod preprocessingu:

- **ekstrakcja danych** - w relacyjnej bazie danych MIMIC-III jest 26 tabel. W tej pracy użyto ich 5 (*inputevents*, *outputevents*, *chartevents*, *labevents*, *prescriptions*). 
- **czyszczenie danych** - w danych obecne są braki danych, obserwacje odstające, duplikaty, błędne rekordy oraz niespójne jednostki. Autorzy część z tych problemów rozwiązali podczas selekcji cech oraz ich ekstrakcji(kolejny pkt), ale w ogólności przyjęli 3 schematy działania w celu poradzenia sobie z poniższymi problemami: 

1) *niespójne jednostki* - usuwanie małej części obserwacji o innych jednostkach, albo przekształcanie jednostek do jednej wynikającej z ogólnych zasad. Jeśli było to niemożliwe - następowało usuwanie zmiennej.

2) *wiele rekordów dla tego samego czasu* - średnia dla zmiennych ciągłych, pierwsze wystąpienie dla zmiennych kategorycznych

3) *zmienne jako zakresy* - mediana zmiennej ciągłej

- **Selekcja cech i ich ekstrakcja** - w celu wyczerpującego porównania benchmarków, na zbiorach MIMIC-III oraz MIMIC-III CareVue stworzono 3 zbiory cech:

1) *Feature Set A* - 17 cech, 'preprocessed'; np. usunięte obs. odstające

2) *Feature Set B* - 20 cech, 'raw clinical features'; obs. odstające zostają

3) *Feature Set C* - 136 cech, 'large number of raw clinical time series data'; dobrane bazując na względnie niskiej % zawartości braków danych

### Modele Uczenia Maszynowego & Metody Oceniania (ang. Scoring Methods)

Użyte i porównane zostały poniższe modele uczenia maszynowego:

- **Super Learner Model** - asymptotycznie optymalny model/system, zbudowany na teorii kroswalidacji, wykorzystujący wiele różnych modeli uczenia maszynowego
- **Multimodal Deep Learning Model** - kombinacja FFN(Feed-Forward Network) i RNN (Recurrent Neural Network). FFN potrafi dokonać predykcji na danych bez znaku czasowego (*non-temporal features*), natomiast RNN wykorzystuje zmienne związane z czasem (*temporal features*).

Zestawiono je z używanymi w medycynie metodami oceny (ang. scoring methods):

- SAPS-II
- SOFA
- New SAPS-II (modyfikacja SAPS-II)

### Hiperparametry modeli ML

Dla poniższych modeli użyto następujące zestawy hiperparametrów:

- Super Learner Model - dla każdego bazowego algorytmu uczenia maszynowego zastosowano **domyślne hiperparametry**
- Multimodal Deep Learning Model - dla wszystkich modeli uczenia głębokiego: **learning rate** - 0.001 dla klasyfikacji oraz 0.005 dla regresji, **funkcja aktywacji** - ReLU, **dropout rate** - 0.1, **batch size** - 100, **max epoch number** - 250.

### Kroswalidacja. Miary oceny jakości modeli.

W algorytmie Super Learner wykorzystano kroswalidację 5-warstwową (*5-fold*) używając 3 warstw do treningu, 1 do walidacji a ostatniej do testowania w celu raportowania wyników.

Dla porównania modeli uczenia maszynowego i metod ocen, wykorzystano następujące metryki dla:

- klasyfikacji - **AUROC** oraz **AUPRC**
- regresji - **MSE**

### Wnioski i dodatkowe uwagi

W zdecydowanej większości przypadków lepszych predykcji dokonywał Multimodal Deep Learning Model - może poza nielicznymi przypadkami jak np. dane z ostatnich 24h, gdzie Super Learner był nieznacznie lepszy. Oba te modele okazywały się być lepsze od metod oceniania SAPS-II oraz SOFA.

Na koniec artykułu była również informacja o testach statystycznych, które wykazały, że Multimodal Deep Learning Model okazał się dostawać istotnie lepsze wyniki od pozostałych modeli.

## Dołączony kod oraz funkcje stworzone w celach preprocessingu

W większości kod jest pisany w języku Python, ale są również i skrypty w języku R.

Kod dołączony do artykułu znajduje się na [repozytorium github](https://github.com/USC-Melady/Benchmarking_DL_MIMICIII). Dobrze opisana jest głównie pierwsza strona repozytorium, aczkolwiek wydaje mi się, że brakuje trochę spójności między artykułem - nie ma żadnej informacji o Feature Setach A,B oraz C, a informacje który zbiór cech wynika z którego ze [skryptów preprocessingu](https://github.com/USC-Melady/Benchmarking_DL_MIMICIII/tree/master/Codes/mimic3_mvcv) trzeba wnioskować na podstawie artykułu. Na pewno jakby te skrypty do preprocessingu były lepiej podzielone np. w katalogi 'Feature Set A-C', wyglądałoby to lepiej. 

Czasami można też znaleźć dobre komentarze np. na temat użytej [struktury sieci neuronowej](https://github.com/USC-Melady/Benchmarking_DL_MIMICIII/blob/master/Codes/DeepLearningModels/python/tengwar/nnet/classifiers.py), ale są również i takie [skrypty](https://github.com/USC-Melady/Benchmarking_DL_MIMICIII/blob/master/Codes/mimic3_mvcv/r_process_mimic_all.r
), które wymagają głębszej analizy, w których brakuje jakichkolwiek komentarzy.

Co do samych funkcji wykorzystywanych do preprocessingu, autorzy w zdecydowanej większości tworzyli swoje własne funkcje np. [processing_inputevents](https://github.com/USC-Melady/Benchmarking_DL_MIMICIII/blob/master/Codes/mimic3_mvcv/8_processing.ipynb) albo [try_making_splits](https://github.com/USC-Melady/Benchmarking_DL_MIMICIII/blob/master/Codes/mimic3_mvcv/11_get_time_series_sample_17-features-processed_24hrs.ipynb).

Podsumowując, repozytorium polecam konfrontować z artykułem jako jego nieodłączną częścią oraz uważnie analizować zawarty w nim kod.
