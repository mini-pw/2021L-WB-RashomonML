# Praca domowa 2
# Karol Degórski

W niniejszej pracy domowej przeanalizuję artykuł naukowy pt.: "Modelowanie predykcyjne w intensywnej opiece lekarskiej: badanie porównawcze metod uczenia maszynowego" dostępny na stronie https://academic.oup.com/jamiaopen/article/1/1/87/5032901.
## Cel badania
Celem przeprowadzonego badania naukowego było porównanie kilku algorytmów uczenia maszynowego dla wybranych problemów związanych z modelowaniem predykcyjnym w intensywnej opiece lekarskiej. Zdecydowano się na przeprowadzenie tego badania z powodu rosnącej liczby danych klinicznych zapisywanych na elektronicznych kartach zdrowia pacjentów, które wykazują doskonałe przesłanki do wyciągania z tych danych wniosków i rozwiązań różnych problemów klinicznych przy użyciu uczenia maszynowego. Zajęto się szczególnym scenariuszem opieki zdrowotnej nad pacjętami w przypadku ich wizyt na SOR, specyficznym miejscu dla lekarza, który musi podejmować szybkie decyzje i dysponować odpowiednim i optymalnym zasobem do realizacji założonych działań. Poznanie tych czynników ma ogromne znaczenie w aspekcie poprawy jakości obsługi pacjentów, a także eliminacji błędów medycznych oraz podejmowania decyzji, które pozwolą wypracować lepsze scenariusze czy plany w tego typu opiece. 

## Zbiór danych
Skorzystano z bazy danych MIMIC-III pochodzącej ze strony Physionet.org. Baza ta została udostępniona przez szpital Beth Israel i zawiera pozbawione elementów identyfikacyjnych dane pacjentów z odziałów intensywnej opieki z lat 2005 - 2011. Zawiera one dane medyczne o ponad 46 tysiącach różnych pacjentów. 

## Kryteria wyboru obserwacji
W badaniu wykorzystano dane tylko o pacjentach niepediatrycznych (wiek powyżej 18 lat), którzy nie byli wielokrotnie transferowani i przebywali na oddziałach intensywnej terapii powyżej 24 godzin. Po zastosowaniu tych kryteriów liczba unikalnych pacjentów wynosiła 30 414, a uniklanych przyjęć 37 787.


## Definicje problemów
* Śmiertelność wewnątrzszpitalna - modelowano za pomocą klasyfikacji binarnej
* Długość pobytu - modelowano za pomocą klasyfikacji wieloklasowej, dzieląc długość pobytu w szpitalu na 7 przedziałów
* Diagnostyka różnicowa - modelowano za pomocą klasyfikacji wieloetykietowej 
* Przewidywanie pacjenta do ponownego przyjęcia - modelowano za pomocą klasyfikacji wieloklasowej, w podziale na 6 klas, w zależności od czasu, który upłynął do powrotu pacjenta, do szpitala

## Kroki preprocessingu
* Zgrupowano różne kody diagnostyczne ICD-9, których liczba w bazie jest 14 567, w grupy kodów biorąc pod uwagę tylko 3 pierwsze litery, tak aby badać specyfikę chorób na poziomie grup. Dzięki temu zmniejszono liczbę różnych kodów do 942
* Każda zmienna czasowa została znormalizowana biorąc różnicę jej i średniej, a następnie dzieląc to przez odchylenie standardowe
* Brakujące dane zostały uzupełnione za pomocą strategii przeniesienia na każdym znaczniku czasu
* Wiek pacjentów podzielono na 5 przedziałów
* Badania laboratoryjne, które są wykonywane standardowo dla pacjentów chorujących na daną chorobę, różnią się częstotliwością ich wykonywania dla każdego pacjenta. Dlatego też użyto średnich godzinowych badań wykonywanych przez pierwsze 48 godzin. Dane te zostały również poddane procesowi normalizacji, a brakujące dane zostały imputowane
* Dla niektórych modeli (SVM, LR, MLP) zmienne dotyczące wizyt zareprezentowano jako: minimum, maksimum, średnia, odchylenie standardowe, liczba obserwacji
* Autorzy porównali 4 strategie reprezentacji cech (wszystkie zostały szczegółowo opisane w artykule)
  * Reprezentacja na poziomie wizyty (tylko cechy fizjologiczne)
  * Reprezentacja na poziomie historii (tylko historia diagnostyczna)
  * Połączona reprezentacja
  * Reprezentacja osadzona

## Zastosowane modele uczenia maszynowego
* Maszyna wektorów nośnych (SVM)
* Las losowy (RF)
* Klasyfikacja z użyciem wzmocnienia gradientowego (GBC)
* Regresja logistyczna (LR)
* Perceptron wielowarstwowy (MLP)
* Długotrwała pamięć krótkotrwała (LSTM)
* Konwolucyjne sieci neuronowe (CNN-LSTM)

## Kroswalidacja i hiperparametry
W badaniu zastosowano 5-krotną kroswalidację krzyżową dla każdego zadania uczenia maszynowego.
Hiperparametry do powyższych metod zostały podane przez autorów w materiałych dodatkowych sekcja 8.2:
* SVM - użyto jądra RBF i kary (penalty) 0,001
* Regresja logistyczna - trenowano na karze wynoszącej 0,005 dla zadań binarnych i 0,01 dla wieloklasowej
* Perceptron wielowarstwowy - użyto dwóch warstw, na każdej 256 neuronów, 0,5 dropout i L2 wynoszące 1e-6
* Modele sekwencyjne - użyto optymalizacji Adam na poziomie 0,005, jako funkcji aktywacji użyto sigmoid dla kalsyfikacji binarnej i softmax dla wieloklasowej


## Miary oceny jakości modeli

Zastosowano następujące miary oceny jakości modeli:
* obszar pod krzywą (AUC) - prawdopodobieństwo, że klasyfikator przydzieli wyższą rangę dla losowego przypadku pozytywnego niż negatywnego 
* miara f1 - średnia harmoniczna z precyzji i czułości
* czułość (sensitivity) - prawdpopodobieństwo, że klasyfikacja jest poprawna jeśli przypadek jest pozytywny
* specyficzność (specificity) - prawdpopodobieństwo, że klasyfikacja jest poprawna jeśli przypadek jest negatywny

## Kod
Dołączony do artykułu naukowego kod w Pythonie jest przejrzyście napisany. Zawiera niezbędne komentarze, które pozwalają w relatywnie łatwy sposób odnaleźć np. dany element preprocessingu. Autorzy pisali własne funkcje z wykorzystaniem wbudowanych funkcji np. MinMaxScaler(), LabelEncoder(), dzięki czemu kod jest spójny z artykułem. Wykorzystano również ogólodostępne pakiety np. pakiet keras do głębokich modeli uczenia maszynowego. 
