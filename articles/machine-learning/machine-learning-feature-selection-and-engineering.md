<properties
    pageTitle="Funkcja techniczny i zaznaczenia w Azure maszynowego uczenia | Microsoft Azure"
    description="Wyjaśniono celów wybór funkcji i funkcji inżynierskie oraz przykłady ich roli w procesie rozszerzenie danych nauki komputera."
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
    ms.date="09/12/2016"
    ms.author="zhangya;bradsev" />


# <a name="feature-engineering-and-selection-in-azure-machine-learning"></a>Funkcja techniczny i zaznaczenia w nauki maszynowego Azure

W tym temacie wyjaśniono celów techniczny funkcji i wybór funkcji w procesie rozszerzenie danych nauki komputera. Przedstawia go, te procesy wymagają za pomocą przykładów dostarczony przez Azure maszynowego uczenia Studio.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Szkolenie dane używane w maszynowego uczenia często można zwiększyć przez zaznaczenie lub wyodrębniania funkcji od nieprzetworzonych danych zebranych. Przykład funkcji odtwarzania w kontekście Dowiedz się, jak do klasyfikowania obrazy odręcznego znaków jest mapa gęstość bitowa wykonane z bitowej nieprzetworzonych danych dystrybucji. To mapowanie może pomóc zwiększyć wydajność rozkład nieprzetworzonych Znajdź krawędzie znaków.

Funkcje odtwarzania i zaznaczone zwiększenie wydajności podczas szkolenia, co pozwala podjąć próbę wyodrębnić kluczowe informacje zawarte w danych. Są również zwiększyć możliwości tych modeli dokładnie klasyfikowania danych wejściowych i bardziej trwała przewidywanie wyniki zainteresowania. Aby wprowadzić więcej praktyce tractable nauka można także połączyć techniczny funkcji i wybierania. Robi to ulepszanie, a następnie zmniejszanie liczby funkcje skalibrować lub przeszkolenie modelu. Matematycznie mówiąc, funkcje zaznaczone, aby przeszkolenie modelu są minimalny zestaw zmiennych niezależnych, które wyjaśnić trendy w danych, a następnie pomyślnie przewidywanie wyników.

Wybieranie funkcji i inżynierskie jest częścią większego procesu zwykle składa się z czterech kroków:

* Zbieranie danych
* Rozszerzanie danych
* Budowa modelu
* Przetwarzanie końcowe

Techniczny i wybierania składające się na kroku ulepszania danych nauki komputera. Z naszego punktu widzenia można wyróżnić trzy aspektów tego procesu:

* **Wstępne przetwarzanie danych**: ten proces próbuje upewnij się, czy zebranych danych jest czysty i spójne. Zawiera zadań takich jak integrowanie wiele zestawów danych, obsługa brakujących danych, obsługa niespójne dane i konwersji typów danych.
* **Funkcja techniczny**: ten proces próbuje utworzyć dodatkowe funkcje odpowiednich z istniejących funkcji nieprzetworzonych danych oraz zwiększenie przewidywanych możliwości Algorytm uczenia.
* **Wybieranie funkcji**: ten proces zaznacza klucza podzestaw oryginalny funkcji danych w celu zmniejszenia wymiarze problemu szkolenie.

W tym temacie opisano tylko techniczny funkcji i funkcji zaznaczania aspektów procesu ulepszania danych. Aby uzyskać więcej informacji w kroku przetwarzania wstępnego dane zobacz [wstępne przetwarzanie danych w Azure maszynowego uczenia Studio](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/).


## <a name="creating-features-from-your-data--feature-engineering"></a>Tworzenie funkcji z danych — funkcja inżynierskie

Dane szkolenia składa się z macierzą składający się z przykładami (rekordów lub obserwacji przechowywane w wierszach), z których każda ma zestaw funkcji (zmienne lub pól znajdujących się w kolumnach). Funkcje określone w projekcie doświadczenia mają charakteryzującą trendy w danych. Chociaż wiele pól nieprzetworzonych danych może być bezpośrednio uwzględnione w wybranej funkcji umożliwia przeszkolenie modelu, dodatkowe funkcje odtwarzania często muszą być skonstruowane z funkcji w nieprzetworzonych danych, aby wygenerować zestawu danych szkolenie rozszerzone.

Jakiego rodzaju funkcje czy tworzone ulepszanie zestawu danych podczas szkolenia modelu? Odtwarzania funkcji, które zwiększają szkolenia zawierają informacje, które lepiej odróżnia trendy w danych. Można się spodziewać nowe funkcje, które zapewniają dodatkowe informacje, które nie są wyraźnie przechwytywane lub łatwo widoczne w zestaw funkcji oryginalnego lub istniejących, ale ten proces jest clipart. Decyzje dźwięku i wydajność często wymagają niektórych specjalizacji domeny.

Począwszy od Azure maszynowego uczenia, najłatwiej ujmij ten proces konkretnie przy użyciu próbki opisane w Studio nauki komputera. Dwa przykłady przedstawiono poniżej:

* Przykład regresji ([przewidywania liczby dzierżawy roweru](http://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4)) w eksperyment pod nadzorem, gdy wiadomo, że wartości docelowej
* Przykład klasyfikacji wyszukiwania tekstu przy użyciu [Funkcji mieszania][feature-hashing]

### <a name="example-1-adding-temporal-features-for-a-regression-model"></a>Przykład 1: Dodawanie funkcji czasowy dla modelu regresji ###

Celu zademonstrowania sposobu mechanika funkcje dla zadania regresji, można użyć doświadczenia "żądanie Prognozowanie z rowery" w Azure maszynowego uczenia Studio. Celem tego doświadczenia jest oszacowanie zapotrzebowania na rowery, oznacza to, że liczba roweru dzierżawy w ramach określonego miesiąc, dzień lub godzinę. Zestaw danych **UCI dzierżawa roweru zestawu danych** jest używany jako nieprzetworzonych danych wejściowych.

Ten zestaw danych na podstawie rzeczywistych danych od firmy Bikeshare kapitału, która obsługuje sieci dzierżawa roweru w stanie Waszyngton kontrolera domeny w Stanach Zjednoczonych. Zestaw danych reprezentuje liczbę roweru dzierżawy w określonej godziny dnia, 2011 do daty 2012 i zawiera 17379 wierszy i kolumn 17. Zestaw funkcji nieprzetworzonych zawiera warunków pogody (temperatura, wilgotność, prędkość wiatru) i typ dzień (święta lub dnia tygodnia). Pole przewidywanie jest **cnt**, Zliczanie reprezentujący dzierżawy roweru w określonej godziny i które od 1 do 977.

Do tworzenia skutecznych funkcje w dane szkolenia, czterech modeli regresji utworzono przy użyciu tego samego algorytmu, ale z czterema zestawami danych różnych szkolenie. Cztery zestawy danych reprezentują tych samych nieprzetworzonych danych wejściowych, ale zwiększenie liczby funkcji ustawić. Te funkcje są podzielone na cztery kategorie:

1. A = pogody + świąt weekday + weekend funkcje przewidywane dnia
2. B = liczba rowery, które zostały wynajmowanych w każdej z poprzedniego 12 godzin
3. C = liczba rowery, które zostały wynajmowanych w każdej z poprzedniego 12 dni w tej samej godziny
4. D = liczba rowery, które zostały wynajmowanych w każdej z poprzedniego 12 tygodni w tej samej godziny i tego samego dnia

Oprócz A zestaw funkcji, które już istnieje w oryginalnym nieprzetworzonych danych, trzech zestawów funkcji są tworzone za pomocą funkcji inżynierskich proces. Funkcja Ustaw przechwycone B ostatnie zapotrzebowania na rowery. Funkcja Ustaw przechwytywanie C zapotrzebowania na rowery na określoną godzinę. Funkcja Ustaw żądanie przechwycone D na rowery na określoną godzinę i określonego dnia tygodnia. Każdy z czterech zestawów danych szkolenie zawiera zestawy funkcji A, A + B, A + B + C i A + B + C + D, odpowiednio.

W doświadczeniu Azure maszynowego uczenia te cztery zestawy danych szkolenia są utworzone za pomocą czterech gałęzi wstępnie przetworzone zestawu danych wejściowych. Z wyjątkiem oddziału po lewej stronie każdego z tych oddziałów zawiera [Wykonywanie skryptów R] [ execute-r-script] moduł, w którym zestaw uzyskana funkcje (funkcja ustawia B, C i D) jest odpowiednio wykonane i dołączone do zaimportowany zestaw danych. Na rysunku poniżej pokazano skrypt R służy do tworzenia zestaw funkcji B w drugim gałąź po lewej stronie.

![Tworzenie zestawu funkcji](./media/machine-learning-feature-selection-and-engineering/addFeature-Rscripts.png)

W poniższej tabeli przedstawiono porównanie wyniki z czterech modeli. Najlepsze wyniki są wyświetlane przez funkcje A + B + C. Zauważ, że częstotliwość błędów zmniejszenie po zestawy dodatkowych funkcji są zawarte w danych szkoleniowych. To sprawdza naszych założenie, że zestawy funkcji B i C zapewniają dodatkowe istotne informacje dotyczące zadania regresji. Dodawanie zestaw funkcji D nie wydaje się zapewnienie wszelkie dodatkowe obniżki stawki błędu.

![Porównaj wyniki](./media/machine-learning-feature-selection-and-engineering/result1.png)

### <a name="example2"></a>Przykład 2: Tworzenie funkcje w wyszukiwania tekstu  

Funkcja techniczny powszechnie jest stosowane w zadań związanych z wyszukiwania tekstu, takich jak analizy klasyfikacji i upodobania dokumentu. Na przykład do klasyfikowania dokumentów na kilka kategorii, należy typowe założeń jest możliwość wyrazem lub frazą, zawarte w jednej kategorii dokumentu rzadziej ma być wykonywana w innej kategorii dokumentu. Innymi słowy częstotliwość rozkład wyraz lub frazę będzie mógł określić kategorie innym dokumentem. W aplikacjach wyszukiwania tekstu funkcja rysunek techniczny proces jest potrzebne do utworzenia funkcji dotyczących częstotliwości wyraz lub frazę, ponieważ pojedynczych zawartość tekstowa zwykle służą jako danych wejściowych.

Aby osiągnąć to zadanie, technika o nazwie *funkcji mieszania* jest stosowana do efektywne przekształcić funkcji dowolnego tekstu indeksy. Zamiast kojarzenie każdej funkcji tekst (wyrazów lub fraz) do określonego indeksu, tej funkcji metody przez zastosowanie funkcji skrótu do funkcji i za pomocą ich wartości mieszania jako indeksów bezpośrednio.

W Azure maszynowego uczenia, ma [Funkcji mieszania] [ feature-hashing] modułu, który tworzy te funkcje wyraz lub frazę. Na poniższej ilustracji pokazano przykład użycia tego modułu. Zestaw danych wejściowych zawiera dwie kolumny: ocena książki z zakresu od 1 do 5 i rzeczywistych przeglądanie zawartości. Cel: ta [Funkcja mieszania] [ feature-hashing] modułu ma pobierać nowe funkcje, których są wyświetlane częstotliwości występowania odpowiednich wyrazów lub fraz w Recenzja wybranej książki. Aby użyć tego modułu, należy wykonać następujące czynności:

1. Zaznacz kolumnę, która zawiera wprowadzania tekstu (**Kol2** w tym przykładzie).
2. Ustawianie *Hashing bitsize* 8, co oznacza 2 ^ 8 = 256 funkcje są tworzone. Wyraz lub frazę w polu tekst jest następnie mieszany do 256 indeksy. Parametr *Hashing bitsize* zakresu od 1 do 31. Jeśli parametr jest ustawiony na większą liczbę, wyrazów lub fraz są rzadziej być skrócony w ten sam indeks.
3. Ustaw parametr *N g* 2. Częstotliwość wystąpienie unigrams (funkcja dla każdej pojedynczego wyrazu) i bigrams (funkcja dla każdej pary słów sąsiadujących) to pobiera z wprowadzania tekstu. Parametr *N g* zakresu od 0 do 10, która wskazuje maksymalną liczbę kolejnych wyrazów, które mają zostać uwzględnione w funkcji.  

![Funkcja mieszania modułu](./media/machine-learning-feature-selection-and-engineering/feature-Hashing1.png)

Na poniższej ilustracji pokazano wygląd tych nowych funkcjach.

![Przykład mieszania funkcji](./media/machine-learning-feature-selection-and-engineering/feature-Hashing2.png)

## <a name="filtering-features-from-your-data--feature-selection"></a>Funkcje z danych — wybór funkcji filtrowania  ##

*Wybieranie funkcji* jest procesem, który jest często stosowana do budowy zestawy danych szkolenia dla przewidywanych modelowania zadań, takich jak klasyfikacji lub regresji zadań. Celem jest wybrać podzbiór funkcji z oryginalnego zestawu danych, co pozwala ograniczyć wymiary przy użyciu minimalnego zestawu funkcji reprezentować maksymalnej ilości odchylenie w danych. Ten podzestaw funkcji zawiera tylko funkcje, które mają zostać uwzględnione na szkolenie modelu. Wybieranie funkcji pełni dwie funkcje:

* Wybieranie funkcji często zwiększa dokładność klasyfikacji o usuwaniu nie ma znaczenia, zbędne lub ściśle powiązane funkcje.
* Wybieranie funkcji zmniejszenie liczby funkcje, dzięki czemu bardziej efektywne szkolenie modelu. To jest szczególnie istotne w przypadku uczestników, które są drogich na szkolenie, takich jak maszyny wektorowa pomocy technicznej.

Mimo że wybór funkcji ma na celu zmniejszenia liczby funkcji w zestawie danych umożliwia przeszkolenie modelu, go nie zazwyczaj odwołuje się do terminów *redukcji wymiarze.* Funkcja metody wyodrębnić podzbiór funkcji oryginalne dane bez zmieniania ich.  Metod redukcji wymiarze zastosować odtwarzania funkcje, które mogą Przekształcanie funkcji oryginalnego, a więc je zmodyfikować. Przykłady metod redukcji wymiarze głównych części analizy, analiza korelacji kanonicznych i dekompozycji pojedynczą wartość.

Zaznaczenie funkcji filtr oparty jest jedną kategorię powszechnie stosowane metody funkcji w kontekście pod nadzorem. Tych metod obliczania korelacji między wszystkimi funkcjami i atrybut docelowy, zastosować środek statystyczne przypisuje wynik do poszczególnych funkcji. Funkcje są klasyfikowane następnie według wyników, których można używać do ustawiania próg zachowanie i usuwaniu określoną funkcję. Przykładami statystyczne miar używane w tych metodach korelacji liniowej Pearsona, wzajemnego informowania i test chi-kwadrat.

Azure maszynowego uczenia Studio przewiduje wybór funkcji modułów. Jak pokazano na poniższym rysunku, te moduły zawiera [filtr oparty wybór funkcji] [ filter-based-feature-selection] i [Fishera liniowej analizy Discriminant][fisher-linear-discriminant-analysis].

![Przykład zaznaczenia funkcji](./media/machine-learning-feature-selection-and-engineering/feature-Selection.png)


Na przykład za pomocą [filtr oparty wybór funkcji] [ filter-based-feature-selection] modułu opisane wcześniej przykładzie wyszukiwania tekstu. Załóżmy, że tworzenie modelu regresji po utworzeniu zestawu funkcji 256 za pośrednictwem [Funkcji mieszania] [ feature-hashing] modułu, a zmienna odpowiedzi jest **Kol1** reprezentuje książki Przejrzyj ocena z zakresu od 1 do 5. Ustaw **funkcji wyników metody** **Korelacji liniowej Pearsona**, **kolumnie docelowej** **Kol1**i **liczbę żądanych funkcji** **50**. Moduł [filtr oparty wybór funkcji] [ filter-based-feature-selection] następnie tworzy zestaw danych, który zawiera 50 funkcji razem z atrybutu docelowego **Kol1**. Na poniższej ilustracji pokazano przepływu tego doświadczenia i parametrów wejściowych.

![Przykład zaznaczenia funkcji](./media/machine-learning-feature-selection-and-engineering/feature-Selection1.png)

Na poniższej ilustracji pokazano wyniku zestawy danych. Każdej funkcji jest uzyskał oparte na korelacji liniowej Pearsona między sobą i atrybut docelowy **Kol1**. Funkcje z góry wyniki są zachowywane.

![Zestawy danych opartych na filtr funkcji zaznaczenia](./media/machine-learning-feature-selection-and-engineering/feature-Selection2.png)

Na poniższej ilustracji pokazano wyniki odpowiadające im wybranych funkcji.

![Wyniki wybranej funkcji](./media/machine-learning-feature-selection-and-engineering/feature-Selection3.png)

Stosując ten [filtr oparty wybór funkcji] [ filter-based-feature-selection] moduł, 50 z 256 funkcje są zaznaczone, ponieważ zawierają najbardziej funkcje powiązane z docelowej zmiennej **Kol1** na podstawie wyników metody **Korelacji liniowej Pearsona**.

## <a name="conclusion"></a>Wnioski
Funkcja techniczny i wybór funkcji są dwie czynności wykonywanych przygotowywanie dane szkolenia podczas tworzenia modelu maszynowego uczenia się. Zazwyczaj techniczny funkcji najpierw stosowana jest wygenerować dodatkowe funkcje, a następnie etapu wyboru funkcji jest wykonywana w celu wyeliminowania nie ma znaczenia, zbędne lub wysoce skorelowany funkcje.

Nie zawsze jest zawsze przeprowadzić wybór funkcji techniczny lub funkcji. Czy jest potrzebna zależy od tego, dane mają lub zbieranie, algorytmu, który możesz wybrać i celem doświadczenia.


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/
