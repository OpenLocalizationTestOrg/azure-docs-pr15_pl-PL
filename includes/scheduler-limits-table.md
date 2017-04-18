W poniższej tabeli opisano wszystkich przydziałów głównych, limity, wartości domyślne i limity w harmonogramie Azure.

|Zasób|Opis limit|
|---|---|
|**Rozmiar zadania**|Maksymalny rozmiar zadania jest 16K. Jeśli położenie lub POPRAWKĘ wyników w zadaniu przekracza te limity, jest zwracana 400 Niewłaściwe żądanie kodu stanu.|
|**Żądanie rozmiar adresu URL**|Maksymalny rozmiar adresu URL żądania wynosi 2048 znaków.|
|**Rozmiar nagłówka agregacji**|Rozmiar maksymalna łączna nagłówka jest 4096 znaków.|
|**Liczba nagłówka**|Nagłówek maksymalna liczba jest 50 nagłówków.|
|**Rozmiar treści**|Treść maksymalny rozmiar wynosi 8192 znaki.|
|**Zakres cyklu**|Zakres cyklu maksymalna to 18 miesięcy.|
|**Czas na czas rozpoczęcia**|Maksymalna liczba "time na czas rozpoczęcia" to 18 miesięcy.|
|**Historię zatrudnienia**|Treść maksymalna liczba odpowiedzi przechowywane w historii zadań wynosi 2048 bajtów.|
|**Częstość**|Przydział domyślny maksymalny częstotliwość jest 1 godzina w zbiorze bezpłatne zadania i 1 minuty w zbiorze standardowe zadania. Maksymalna częstotliwość jest konfigurowany w zbiorze zadania, aby być mniejsza niż maksimum. Wszystkie zadania w zbiorze zadania są ograniczone wartość w zbiorze zadania. Przy próbie utworzenia zadania za pomocą wyższa częstotliwość niż częstotliwość maksymalna liczba w zbiorze zadanie żądania zakończy się niepowodzeniem z kodem stanu 409 konflikt.|
|**Zadania**|Przydział domyślny maksymalny zadania jest 5 zadań w zbiorze bezpłatne zadania i 50 zadań w zbiorze standardowe zadania. Maksymalna liczba zadań jest można konfigurować w zbiorze zadania. Wszystkie zadania w zbiorze zadania są ograniczone wartość w zbiorze zadania. Jeśli spróbujesz utworzyć więcej zadań niż przydziału zadania maksymalna żądania zakończy się niepowodzeniem z kodem stanu 409 konflikt.|
|**Przechowywanie historii zadań**|Historia zadania jest zachowywana dla maksymalnie 2 miesiące lub do wykonania ostatniej 1000.|
|**Zadania wykonane i błędowi przechowywania**|Zadania wykonane i błędowi są zachowywane w 60 dni.|
|**Limit czasu**|Istnieje statyczne przekroczenie limitu czasu żądania (nie można konfigurować) 60 sekund HTTP akcje. W przypadku operacji dłużej wykonaj protokoły asynchroniczne HTTP; na przykład zwrócić 202 od razu, ale kontynuować pracę w tle.|
