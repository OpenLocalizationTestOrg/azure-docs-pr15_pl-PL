<properties
    pageTitle="Funkcja techniczny w procesie analizy Cortana | Microsoft Azure" 
    description="Omówiono na potrzeby funkcji techniczny i znajdują się przykłady jego rolę w procesie ulepszania danych maszynowego uczenia."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="zhangya;bradsev" />


# <a name="feature-engineering-in-the-cortana-analytics-process"></a>Funkcja techniczny w procesie analizy Cortana 

W tym temacie omówiono na potrzeby funkcji techniczny i znajdują się przykłady jego rolę w procesie ulepszania danych maszynowego uczenia. Przykłady użyta w celu przedstawienia tego procesu są pobierane z Azure maszynowego uczenia Studio. 

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

Ten **menu** łączy do tematów dotyczących sposobu tworzenia funkcje dla danych w różnych środowiskach. To zadanie jest krok w [Zespołu danych nauki proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

Funkcja konstrukcyjną próby Zwiększ przewidywanych możliwości nauki algorytmów, tworząc funkcje z nieprzetworzonych danych ułatwić procesu uczenia. Wybieranie funkcji i inżynierskie jest częścią TDSP przedstawionych w [Co to jest proces zespołu danych naukowych?](data-science-process-overview.md) Funkcja techniczny i wybierania są częściami w etapie **rozwoju funkcji** TDSP. 

* **Funkcja techniczny**: ten proces pozwala podjąć próbę, aby utworzyć dodatkowe funkcje odpowiednich z istniejących funkcji nieprzetworzonych danych, a aby zwiększyć przewidywanych możliwości Algorytm uczenia.

* **Wybieranie funkcji**: ten proces zaznacza klucza podzestaw oryginalny funkcji danych w celu zmniejszenia wymiarze problemu szkolenie.

Zwykle **Funkcja techniczny** najpierw stosowana jest wygenerować dodatkowe funkcje, a następnie kroku **Wybór funkcji** jest wykonywana w celu wyeliminowania nie ma znaczenia, zbędne lub wysoce skorelowany funkcje.

Szkolenie dane używane w maszynowego uczenia często może wzrosnąć wyodrębniania funkcji od nieprzetworzonych danych zebranych. Przykład funkcji odtwarzania w kontekście Dowiedz się, jak do klasyfikowania obrazy odręcznego znaków jest tworzenie planu nieco gęstości wykonane z bitowej nieprzetworzonych danych dystrybucji. To mapowanie może pomóc zwiększyć wydajność po prostu bezpośrednio przy użyciu rozkład nieprzetworzonych Znajdź krawędzie znaków.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="creating-features-from-your-data---feature-engineering"></a>Tworzenie funkcji z danych — funkcja inżynierskie

Dane szkolenia składa się z macierzą składający się z przykładami (rekordów lub obserwacji przechowywane w wierszach), z których każda ma zestaw funkcji (zmienne lub pól znajdujących się w kolumnach). Funkcje określone w projekcie doświadczenia mają charakteryzującą trendy w danych. Chociaż wiele pól nieprzetworzonych danych może być bezpośrednio uwzględnione w wybranej funkcji umożliwia przeszkolenie modelu, często jest wielkość liter, że dodatkowe funkcje (odtwarzania), muszą być skonstruowane z funkcji w nieprzetworzonych danych, aby wygenerować zestaw szkolenie rozszerzone.

Jakiego rodzaju funkcje czy tworzone ulepszanie zestawu danych podczas szkolenia modelu? Odtwarzania funkcji, które zwiększają szkolenia zawierają informacje, które lepiej odróżnia trendy w danych. Oczekujemy nowe funkcje, które zapewniają dodatkowe informacje, które nie są wyraźnie przechwycony lub łatwo widoczne w zestawie oryginalnego lub istniejących funkcji. Ale ten proces jest clipart. Decyzje dźwięku i wydajność często wymagają niektórych specjalizacji domeny.

Począwszy od Azure maszynowego uczenia, najłatwiej ujmij ten proces konkretnie przy użyciu próbek opisane w Studio. Dwa przykłady przedstawiono poniżej:

* Przykład regresji [przewidywania liczby roweru dzierżawy](http://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4) w eksperyment pod nadzorem, gdy wiadomo, że wartości docelowej
* Przykład klasyfikacji wyszukiwania tekstu przy użyciu [Funkcji mieszania](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/)

### <a name="example-1-adding-temporal-features-for-regression-model"></a>Przykład 1: Dodawanie funkcji czasowy modelu regresji ###

Można użyć doświadczenia "żądanie Prognozowanie z rowery" w Azure maszynowego uczenia Studio celu zademonstrowania sposobu mechanika funkcje dla zadania regresji. Celem tego doświadczenia jest oszacowanie zapotrzebowania na rowery, oznacza to, że liczba roweru dzierżawy w określonej miesiąc/dzień/godziny. Zestaw danych "dzierżawa roweru UCI zestawu danych" jest używany jako nieprzetworzonych danych wejściowych. Tego zestawu danych na podstawie rzeczywistych danych od firmy Bikeshare kapitału, która obsługuje sieci dzierżawa roweru w stanie Waszyngton kontrolera domeny w Stanach Zjednoczonych. Zestaw danych odpowiada liczbie roweru dzierżawy w określonej godziny dnia w latach 2011 i rok 2012 i zawiera 17379 wierszy i kolumn 17. Zestaw funkcji nieprzetworzonych zawiera warunkami atmosferycznymi (temperatura i wilgotność-wiatru) i typ dzień (dni wolnych/tygodnia). Pole przewidywanie jest "cnt", liczba przedstawiający dzierżawy roweru w określonej godziny, a które zakresy z zakresu od 1 do 977.

W celu tworzenia skutecznych funkcje dane szkolenia, cztery regresji, których modeli są utworzony przy użyciu tego samego algorytmu, ale cztery różne szkolenie zestawy danych. Cztery zestawy danych reprezentują tych samych nieprzetworzonych danych wejściowych, ale zwiększenie liczby funkcji ustawić. Te funkcje są podzielone na cztery kategorie:

1. A = pogody + świąt weekday + weekend funkcje przewidywane dnia
2. B = liczba rowery, które zostały wynajmowanych w każdej z poprzedniego 12 godzin
3. C = liczba rowery, które zostały wynajmowanych w każdej z poprzedniego 12 dni w tej samej godziny
4. D = liczba rowery, które zostały wynajmowanych w każdej z poprzedniego 12 tygodni w tej samej godziny i tego samego dnia

Oprócz A zestaw funkcji, które już istnieje w oryginalnym nieprzetworzonych danych, trzech zestawów funkcji są tworzone za pomocą funkcji inżynierskich proces. Funkcja Ustaw przechwycone B bardzo ostatnie żądanie rowery. Funkcja Ustaw przechwycone C zapotrzebowania na rowery na określoną godzinę. Funkcja Ustaw żądanie przechwycone D na rowery na określoną godzinę i określonego dnia tygodnia. Danych cztery szkolenie zawiera odpowiednio A zestaw funkcji, A + B, A + B + C i A + B + C + D.

W doświadczeniu Azure maszynowego uczenia te cztery zestawy danych szkolenia są utworzone za pomocą czterech gałęzi wstępnie przetworzone zestawu danych wejściowych. Z wyjątkiem po lewej stronie, znajdujących się w większości gałąź, każdy z tych oddziałów modułu [Wykonywanie skryptów R](https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/) , w którym zestaw uzyskana funkcje (funkcja ustawiono B, C i D) są odpowiednio wykonane i dołączone do zaimportowanego zestawu danych. Na rysunku poniżej pokazano skrypt R służy do tworzenia zestaw funkcji B w drugim gałąź po lewej stronie.

![Tworzenie funkcji](./media/machine-learning-data-science-create-features/addFeature-Rscripts.png)

Porównanie wyniki z czterech modeli przedstawiono w poniższej tabeli. Najlepsze wyniki są wyświetlane przez funkcje A + B + C. Uwaga częstotliwość błędów zmniejsza, kiedy zestaw funkcji dodatkowe znajdują się w danych szkoleniowych. Sprawdza naszych założenie, że zestaw funkcji, B, C zapewniają dodatkowe istotne informacje dotyczące zadania regresji. Ale dodanie funkcji D prawdopodobnie nie zapewnienie wszelkie dodatkowe obniżki stawki błędu.

![wynik porównania](./media/machine-learning-data-science-create-features/result1.png)

### <a name="example2"></a>Przykład 2: Tworzenie funkcje wyszukiwania tekstu  

Funkcja techniczny powszechnie jest stosowane w zadań związanych z wyszukiwania tekstu, takich jak analizy klasyfikacji i upodobania dokumentu. Na przykład gdy chcemy klasyfikowania dokumentów na kilka kategorii, typowe założeń jest możliwość word fraz zawarte w jednej kategorii dokumentu rzadziej ma być wykonywana w innej kategorii dokumentu. W innym słowa częstotliwość rozkład wyrazów i fraz będzie mógł określić kategorie innym dokumentem. W aplikacjach wyszukiwania tekstu ponieważ pojedynczych zawartość tekstowa zwykle służą jako danych wejściowych, funkcja rysunek techniczny proces jest potrzebne do utworzenia funkcji dotyczących częstotliwości wyraz i fraza.

Aby osiągnąć to zadanie, technika o nazwie **funkcji mieszania** jest stosowana do efektywne przekształcić funkcji dowolnego tekstu indeksy. Zamiast kojarzenie każdej funkcji tekst (wyrazów i fraz) do określonego indeksu, funkcje tej metody zastosowanie funkcji skrótu do funkcji i używając ich wartości mieszania jako indeksów bezpośrednio.

W Azure maszynowego uczenia, ma [Funkcji mieszania](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) modułu, który tworzy tych wyraz/fraza funkcje wygodny sposób. Poniższej ilustracji przedstawiono przykład za pomocą tego modułu. Zestaw wprowadzania danych zawiera dwie kolumny: ocena książki z zakresu od 1 do 5, a zawartością rzeczywisty przeglądu. Cel: ta [Funkcja mieszania](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) modułu jest pobrać wiele nowych funkcji Pokaż tej częstotliwości występowania odpowiednie wyrazy / Przejrzyj wyrażenia w wybranej książki. Aby użyć tego modułu, należy wykonać następujące czynności:

* Najpierw zaznacz kolumnę, która zawiera wprowadzania tekstu ("Kol2" w tym przykładzie).
* Drugi, ustaw "Hashing bitsize" na 8, co oznacza 2 ^ 8 = 256 funkcje zostaną utworzone. Program word i fazy w polu tekst będzie być skrócony do 256 indeksy. Parametr "Hashing bitsize" zakresu od 1 do 31. Wyraz/wyrazy / wyrażenia są rzadziej być skrócony w ten sam indeks, jeśli ustawienie, aby był większą liczbę.
* Trzecie Ustaw parametr "N-g" 2. Dzięki temu częstotliwość wystąpienie unigrams (funkcja dla każdej pojedynczego wyrazu) i bigrams (funkcja dla każdej pary słów sąsiadujących) z wprowadzania tekstu. Parametr "N-g" zakresu od 0 do 10, która wskazuje maksymalną liczbę kolejnych wyrazów, które mają zostać uwzględnione w funkcji.  

![Moduł "Funkcja Hashing"](./media/machine-learning-data-science-create-features/feature-Hashing1.png)

Na poniższej ilustracji pokazano, co to nowa funkcja wygląd takich jak.

![Przykład "funkcji Hashing"](./media/machine-learning-data-science-create-features/feature-Hashing2.png)


## <a name="conclusion"></a>Wnioski

Funkcje odtwarzania i zaznaczone zwiększenia efektywności procesu szkolenia, która ma zostać wyodrębnionych kluczowe informacje zawarte w danych. Są również zwiększyć możliwości tych modeli dokładnie klasyfikowania danych wejściowych i bardziej trwała przewidywanie wyniki zainteresowania. Aby wprowadzić więcej praktyce tractable nauka można także połączyć techniczny funkcji i wybierania. Robi to ulepszanie, a następnie zmniejszanie liczby funkcje skalibrować lub przeszkolenie modelu. Matematycznie mówiąc, funkcje zaznaczone, aby przeszkolenie modelu są minimalny zestaw zmiennych niezależnych, które wyjaśnić trendy w danych, a następnie pomyślnie przewidywanie wyników.

Należy zauważyć, że nie zawsze jest zawsze przeprowadzić wybór funkcji techniczny lub funkcji. Czy jest potrzebna, czy nie zależy od tego, możemy mieć lub zbieranie dane, algorytmu, który firma Microsoft wybierz i celem doświadczenia.
 
