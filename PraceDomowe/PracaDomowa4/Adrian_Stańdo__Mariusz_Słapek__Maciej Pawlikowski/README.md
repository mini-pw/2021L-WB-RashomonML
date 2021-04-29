Fakt, że nie udało nam się odwzorować dokładnch rezultatów wynika z kilku problemów:
- Hiperparametry podawane w raporcie nie zgadzały się z tymi w kodzie. Do części modeli podane były zupełnie różne wartości, a do reszty w raporcie nie było ich w ogóle. 
- W kodzie nigdzie nie ustawiono random_state, co mogło powodować, że modele uczyły się na różnych zbiorach. 
- Napotkaliśmy kilka problemów przy preprocesingu, który wymagał pewnych modyfikacji w kodzie przed uruchomieniem oraz problemy z wymaganiami 100 GB wolnej pamięci na komputerze wymaganej do zbudowania bazy danych.
