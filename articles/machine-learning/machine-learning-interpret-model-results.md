<properties
    pageTitle="Interpretowanie modelu powoduje maszynowego uczenia | Microsoft Azure"
    description="Jak wybrać optymalne zestaw algorytmu za pomocą i wizualizowanie wyników wyjść modelu parametrów."
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
    ms.author="bradsev" />


# <a name="interpret-model-results-in-azure-machine-learning"></a>Interpretowanie modelu powoduje nauki maszynowego Azure

W tym temacie wyjaśniono, jak wizualizacji i interpretowania wyników przewidywania w Azure maszynowego uczenia Studio. Po szkolenie modelu i gotowe przewidywań na bieżąco ją ("uzyskał modelu"), należy zrozumieć i interpretowania wyników przewidywania.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Istnieją cztery główne rodzaje maszynowego uczenia modeli można znaleźć w Azure komputera:

* Klasyfikacja
* Klaster
* Regresji
* Systemy recommender

Moduły używane do przewidywania u góry tych modeli są:

* [Wynik modelu] [ score-model] moduł dla klasyfikacji i regresji
* [Przypisz do klastrów] [ assign-to-clusters] moduł dla klastrów
* [Wynik Matchbox Recommender] [ score-matchbox-recommender] dla systemów rekomendacji

Ten dokument wyjaśniono, jak interpretować przewidywania wyników dla każdego z tych modułów. Aby uzyskać omówienie tych modułów Zobacz, [jak wybrać parametry Zoptymalizuj algorytmy w Azure maszynowego uczenia](machine-learning-algorithm-parameters-optimize.md).

Ten temat dotyczy interpretacji prognozowania, ale nie oceny modelu. Aby uzyskać więcej informacji na temat oceny modelu, Dowiedz się, [jak ma być obliczona wydajność modelu w Azure maszynowego uczenia](machine-learning-evaluate-model-performance.md).

Jeśli jesteś nowym użytkownikiem Azure maszynowego uczenia i uzyskać pomoc przy tworzeniu eksperyment proste, aby rozpocząć pracę, zobacz [Tworzenie prostego doświadczenia w Azure maszynowego uczenia Studio](machine-learning-create-experiment.md) w Azure maszynowego uczenia Studio.

## <a name="classification"></a>Klasyfikacja ##
Istnieją podkategorii klasyfikacji problemów:

* Problemy z tylko dwie klasy (Klasyfikacja klasy dwóch lub binarny)
* Problemy z więcej niż dwóch klas (Klasyfikacja wielu klasy)

Azure nauki komputera zawiera różnych modułów na zajęcie się każdego z tych typów klasyfikacji, ale metody do interpretacji ich wyników przewidywania są podobne.

### <a name="two-class-classification"></a>Dwie klasy klasyfikacji###
**Przykład doświadczenia**

Przykład problemu klasyfikacji klasy dwóch jest klasyfikacji kwiaty soczewki. Zadanie jest do klasyfikowania soczewki kwiaty na podstawie ich funkcji. Zestaw danych soczewki udostępniana w Azure maszynowego uczenia jest podzbiorem popularne [soczewki zestawu danych](http://en.wikipedia.org/wiki/Iris_flower_data_set) zawierającego wystąpienia tylko dwa potokowy kwiat (klasy 0 i 1). Istnieją cztery funkcje dla każdego kwiatem (sepal długość, szerokość sepal, Motyw Płatek długość i szerokość Motyw Płatek).

![Zrzut ekranu przedstawiający doświadczenia soczewki](./media/machine-learning-interpret-model-results/1.png)

Rysunek 1. Soczewki klasyfikacji klasy dwóch problem doświadczenia

Aby rozwiązać ten problem, przeprowadzono doświadczenia jak pokazano na rysunku 1. Szkolenia i uzyskał modelu drzewa decyzji zwiększane dwóch zajęć. Teraz można wizualizowane wyniki przewidywania z [Modelu wynik] [ score-model] modułu, klikając pozycję port wyjściowy [Modelu wynik] [ score-model] modułu, a następnie klikając polecenie **Wizualizacja**.

![Wynik modelu modułu](./media/machine-learning-interpret-model-results/1_1.png)

Spowoduje to wyświetlenie wyników wyniki jak pokazano na rysunku 2.

![Wyniki doświadczenia klasyfikacji klasy dwóch soczewki](./media/machine-learning-interpret-model-results/2.png)

Rysunek 2. Wizualizowanie wyniku wynik model klasyfikacji dwóch zajęć

**Interpretacji wyników**

Istnieje sześć kolumn w tabeli wyników. Po lewej stronie czterech kolumn są cztery cechy. Prawo dwóch kolumnach, uzyskał etykiety i uzyskał prawdopodobieństwa są wynikami przewidywania. Kolumna uzyskał prawdopodobieństwa pokazuje prawdopodobieństwo kwiatu należący do klasy dodatnia (klasy 1). Na przykład numer pierwszej kolumny (0.028571), oznacza, że jest 0.028571 prawdopodobieństwo, że pierwszy kwiat należy do klasy 1. Kolumna etykiety uzyskał pokazuje przewidywane klasy dla każdego kwiat. To jest oparty na kolumnie uzyskał prawdopodobieństwa. Jeśli prawdopodobieństwo scored kwiatu jest większa niż 0,5, jest obliczana jako 1 zajęć. W przeciwnym razie jest obliczana jako 0.

**Publikowania usług sieci Web**

Po uwzględnione wyniki przewidywania i uznane za dźwięku doświadczenia można publikować jako usługi sieci web, dzięki czemu będzie wdrożyć go w różnych aplikacjach i nadaj jej uzyskanie przewidywań zajęć na nowy kwiatem soczewki. Aby dowiedzieć się, jak zmieniać eksperyment szkolenie do eksperyment wyników i opublikować go jako usługi sieci web, zawiera sekcja [Publikowanie Azure maszynowego uczenia usługi sieci web](machine-learning-walkthrough-5-publish-web-service.md). Ta procedura umożliwia eksperyment wyników jak pokazano na rysunku 3.

![Zrzut ekranu przedstawiający wyników doświadczenia](./media/machine-learning-interpret-model-results/3.png)

Rysunek 3. Wyników doświadczenia problem klasyfikacji klasy dwóch soczewki

Teraz należy ustawić dane wejściowe i wyjściowe usługi sieci web. Dane wejściowe są prawo port wejściowy modelu [Wynik][score-model], czyli kwiat soczewki funkcje danych wejściowych. Wybór danych wyjściowych zależy od tego, czy jesteś w klasie przewidywane (etykieta uzyskał) i scored prawdopodobieństwa. W tym przykładzie zakłada się, że interesuje Cię w obu. Aby zaznaczyć kolumny żądany, za pomocą [Wybieranie kolumn w zestawie danych] [ select-columns] modułu. Kliknij pozycję [Zaznacz kolumny w zestawie danych][select-columns], kliknij pozycję **Uruchom selektor kolumny**i wybierz **Uzyskał etykiet** i **Uzyskał prawdopodobieństwa**. Po ustawieniu port wyjściowy [Wybierz] kolumn w zestawie danych[ select-columns] i jego ponowne uruchomienie, należy gotowa do opublikowania wyników doświadczenia jako usługi sieci web, klikając pozycję **Publikowanie usługi sieci WEB**. Ostateczne doświadczenia wygląda rysunek 4.

![Doświadczenia klasyfikacji klasy dwóch soczewki](./media/machine-learning-interpret-model-results/4.png)

Rysunek 4. Ostateczne wyników doświadczenia soczewki klasy dwóch klasyfikacji problemu

Po uruchomieniu usługi sieci web i wprowadź wartości niektórych funkcji wystąpienia test, wynik zwraca dwóch liczb. Numer pierwszej etykiecie scored, a drugim scored prawdopodobieństwa. Ten kwiatem jest obliczana jako 1 zajęć przy prawdopodobieństwie 0.9655.

![Testowanie interpretowania modelu wynik](./media/machine-learning-interpret-model-results/4_1.png)

![Wyniki testu wyników](./media/machine-learning-interpret-model-results/5.png)

Rysunek 5. Wynik usługi sieci Web soczewki klasy dwóch klasyfikacji

### <a name="multi-class-classification"></a>Klasyfikacja wielu zajęć
**Przykład doświadczenia**

W tym doświadczenia w przypadku zadań rozpoznawania mowy litery jako przykład klasyfikacji multiclass. Pozwala podjąć próbę przewidywanie określonej litery (klasa) na podstawie niektórych wartości atrybutu odręcznego wyodrębnionych z obrazów odręcznego klasyfikatora.

![Przykład rozpoznawania listu](./media/machine-learning-interpret-model-results/5_1.png)

Dane szkolenia istnieje 16 funkcje wyodrębnionych z listu odręcznego obrazy. 26 liter formularza naszych 26 klasy. Rysunek 6 przedstawia doświadczenia przeszkolenie model klasyfikacji multiclass rozpoznawania litery i przewidywania na tę samą funkcję ustawianie zestawu danych test.

![Doświadczenia multiclass klasyfikacji rozpoznawania listu](./media/machine-learning-interpret-model-results/6.png)

Rysunek 6. Litera rozpoznawania multiclass klasyfikacji problemu doświadczenia

Wizualizowanie wyniki z [Modelu wynik] [ score-model] modułu, klikając pozycję port wyjściowy modelu [Wynik] [ score-model] modułu, a następnie klikając przycisk **Wizualizacja**, powinna być widoczna zawartość, jak pokazano na rysunku 7.

![Wynik modelu wyników](./media/machine-learning-interpret-model-results/7.png)

Rysunek 7. Wizualizowanie wynik wyniki model klasyfikacji wielu zajęć

**Interpretacji wyników**

Po lewej stronie 16 kolumn reprezentują wartości funkcji zestawu test. Kolumny o nazwach takich jak uzyskał prawdopodobieństwa dla klasy "XX" są tylko, takich jak kolumna uzyskał prawdopodobieństwa w przypadku dwóch zajęć. Są pokazywane prawdopodobieństwo należącego odpowiedniego wpisu do pewnej kategorii. Na przykład dla pierwszego wpisu istnieje 0.003571 prawdopodobieństwo, że jest "A," 0.000451 prawdopodobieństwo, że jest "B" itd. Ostatnia kolumna (uzyskał etykiety) jest taki sam, jako uzyskał etykiety w przypadku dwóch zajęć. Zaznacza klasy z największą prawdopodobieństwo scored jako przewidywane klasy odpowiedniego zapisu. Na przykład dla pierwszej pozycji scored etykiety jest "F" ponieważ ma największą prawdopodobieństwo "F" (0.916995).

**Publikowania usług sieci Web**

Możesz również wyświetlić scored etykietę dla każdej pozycji, a prawdopodobieństwo scored etykiety. Podstawowe logiczny jest znalezienie największa prawdopodobieństwo wśród wszystkich scored prawdopodobieństwa. W tym celu należy użyć [Wykonywanie skryptów R] [ execute-r-script] modułu. Kod R są wyświetlane w rysunek 8, a wynik doświadczenia jest wyświetlany w rysunek 9.

![Przykład kodu R](./media/machine-learning-interpret-model-results/8.png)

Rysunek 8. Kod R wyodrębnianie uzyskał etykiet i prawdopodobieństwa etykiet

![Wynik doświadczenia](./media/machine-learning-interpret-model-results/9.png)

Rysunek 9. Ostateczne wyników doświadczenia problemu multiclass klasyfikacji rozpoznawania mowy listu

Po publikowanie i uruchom usługę sieci web i wprowadź wartości niektórych funkcji wprowadzania, zwracany wynik wygląda rysunek 10. Niniejszy list odręcznego, z wyodrębnionej funkcje 16, jest przewidziane za "T" z prawdopodobieństwem 0.9715.

![Przetestuj interpretowania moduł wynik](./media/machine-learning-interpret-model-results/9_1.png)

![Wynik testu](./media/machine-learning-interpret-model-results/10.png)

Rysunek 10. Wynik usługi sieci Web multiclass klasyfikacji

## <a name="regression"></a>Regresji

Problemy z regresji różnią się od klasyfikacji problemów. Problem klasyfikacji próbujesz przewidywanie osobne klasy, takie jak klasy Kwiat soczewki należy. Ale jak widać w poniższym przykładzie problem regresji, próbujesz przewidywanie zmiennej ciągły, takich jak cena samochodu.

**Przykład doświadczenia**

Używanie przewidywania samochodów cena jako tym przykładzie dla regresji. Próbujesz przewidywanie cena samochodu oparte na jego funkcje, w tym nawiązywanie, typ paliwo typu części i kółko dysk. Doświadczenia jest wyświetlana w rysunek 11.

![Cena samochodów doświadczenia regresji](./media/machine-learning-interpret-model-results/11.png)

Rysunek 11. Cena samochodów regresji problem doświadczenia

Wizualizowanie [Modelu wynik] [ score-model] moduł, wynik wygląda rysunek 12.

![Wyników wyniki dla problemu przewidywania cena samochodów](./media/machine-learning-interpret-model-results/12.png)

Rysunek 12. Wynik wyników problemu przewidywania cena samochodów

**Interpretacji wyników**

Etykiety uzyskał jest kolumny wyników, w tym wyników wyniku. Liczby są przewidywane cena obligacji dla każdego samochodu.

**Publikowania usług sieci Web**

Możesz opublikować doświadczenia regresji do usługi sieci web i nadaj jej do przewidywania samochodów cena w taki sam sposób, jak w przypadku użycia dwóch klasy klasyfikacji.

![Wyników doświadczenia problemu regresji cena samochodów](./media/machine-learning-interpret-model-results/13.png)

Rysunek 13. Wyników doświadczenia problem regresji cena samochodów

Uruchamianie usługi sieci web, zwracany wynik wygląda tak: rysunek 14. Cena przewidywane ten samochód jest $15,085.52.

![Przetestuj interpretowania wyników moduł](./media/machine-learning-interpret-model-results/13_1.png)

![Wyniki modułu wyników](./media/machine-learning-interpret-model-results/14.png)

Rysunek 14. Wynikiem usługi sieci Web problemu regresji cena samochodów

## <a name="clustering"></a>Klaster

**Przykład doświadczenia**

Załóżmy zbioru danych i soczewki ponownie użyć do utworzenia eksperyment klastrów. W tym miejscu można odfiltrować etykiet klasy w zestawie danych tak, aby tylko zawiera funkcje mogą być używane w klastrze. W tym soczewki przypadek użycia, określ liczbę klastrów należy dwa podczas procesu szkolenia, co oznacza, że chcesz klaster kwiaty na dwie klasy. Doświadczenia jest wyświetlana w rysunek 15.

![Wypróbuj soczewki klastrów problem](./media/machine-learning-interpret-model-results/15.png)

Rysunek 15. Wypróbuj soczewki klastrów problem

Klaster różni się od klasyfikacji, w tym zbioru danych i szkolenia nie zawierają etykiet prawdy podłożem samodzielnie. Grupy klastrów szkolenie wystąpienia zestawu danych do klastrów różnią się. W trakcie procesu szkolenia modelu etykiety pozycje przez nauki różnice między ich funkcji. Po wykonaniu tej przeszkolony modelu może służyć do dalszej klasyfikacji przyszłych wpisów. Istnieją dwie części wyniku, które możemy Cię w obrębie klastrów problem. Pierwsza część jest etykiet zbioru danych i szkolenia, a druga jest klasyfikacji nowy zestaw danych z modelem przeszkolony.

Pierwsza część wynik można zwizualizować, klikając pozycję port wyjściowy po lewej stronie modelu [klaster pociągu] [ train-clustering-model] , a następnie klikając polecenie **Wizualizacja**. Wizualizacja jest wyświetlany na rysunku 16.

![Klaster wynik](./media/machine-learning-interpret-model-results/16.png)

Rysunek 16. Wizualizowanie klaster wynik dla zbioru danych i szkolenia

Wynik drugiej części klaster nowe wpisy z modelem klastrów przeszkolony, jest wyświetlany w rysunek 17.

![Wizualizowanie klaster wynik](./media/machine-learning-interpret-model-results/17.png)

Rysunek 17. Wizualizowanie klaster wyników na nowy zestaw danych

**Interpretacji wyników**

Mimo że wyniki z dwóch części wynikać z różnych doświadczenia etapy, wyglądu i są interpretowane w taki sam sposób. Pierwsze cztery kolumny są funkcje. Ostatnia kolumna przydziałów jest wynik prognozowania. Wpisy przypisane tę samą liczbę są przewidziane znajdować się w tym samym klastrze, oznacza to, że ich udostępnianie podobieństwa w określony sposób (odległość euklidesowa metrykę używa tego doświadczenia). Ponieważ określony liczbę klastrów 2, wpisy w przydziałów są oznaczone 0 lub 1.

**Publikowania usług sieci Web**

Możesz opublikować klastrów doświadczenia do usługi sieci web i nadaj jej dla klastrów przewidywań przypadek użycia tak samo jak klasyfikacji dwóch zajęć.

![Wyników doświadczenia soczewki klastrów problemu](./media/machine-learning-interpret-model-results/18.png)

Rysunek 18. Wyników doświadczenia problem klastrów soczewki

Po uruchomieniu usługi sieci web zwracany wynik wygląda rysunek 19. Ten kwiatów jest przewidziane w klastrze 0.

![Test interpretowania wyników modułu](./media/machine-learning-interpret-model-results/18_1.png)

![Wynik modułu wyników](./media/machine-learning-interpret-model-results/19.png)

Rysunek 19. Wynik usługi sieci Web soczewki klasy dwóch klasyfikacji

## <a name="recommender-system"></a>Recommender system
**Przykład doświadczenia**

Dla systemów recommender problem rekomendacji restauracji można użyć jako przykład: restauracje może polecić dla klientów w oparciu o historii ocena. Dane wejściowe składa się z trzech części:

* Oceny restauracji z klientami
* Dane funkcji klientów
* Danych funkcji restauracji

Istnieje kilka sposobów można zrobić dzięki [Pociągu Matchbox Recommender] [ train-matchbox-recommender] moduł w Azure maszynowego uczenia:

- Przewidywanie ocen dla danego użytkownika i pozycji
- Zalecamy elementy do danego użytkownika
- Znajdowanie użytkowników dotyczące danego użytkownika
- Znajdowanie elementów powiązanych z danym elementem

Możesz wybrać, co chcesz zrobić, wybierając cztery opcje w menu **Recommender przewidywania rodzaju** . W tym miejscu możesz szczegółową wszystkie cztery scenariusze.

![Matchbox recommender](./media/machine-learning-interpret-model-results/19_1.png)

Typowe eksperyment Azure maszynowego uczenia systemu recommender wygląda na rysunku 20. Aby dowiedzieć się, jak za pomocą tych modułów systemu recommender, zobacz [recommender matchbox pociągu] [ train-matchbox-recommender] i [wynik matchbox recommender][score-matchbox-recommender].

![Doświadczenia systemu recommender](./media/machine-learning-interpret-model-results/20.png)

Rysunek 20. Doświadczenia systemu recommender

**Interpretacji wyników**

**Przewidywanie ocen dla danego użytkownika i pozycji**

Po zaznaczeniu **Przewidywania ocena** w obszarze **Typ przewidywania Recommender**, zadajesz systemowi recommender przewidywanie ocen dla danego użytkownika i pozycji. Wizualizacja [Wynik Matchbox Recommender] [ score-matchbox-recommender] wynik wygląda rysunek 21.

![Wynik wynik system recommender — ocena przewidywania](./media/machine-learning-interpret-model-results/21.png)

Rysunek 21. Wizualizowanie wynik wynik systemu recommender — ocena przewidywania

Pierwszych dwóch kolumnach są pary element użytkownika dostarczony przez danych wejściowych. Trzecia kolumna zawiera wartości przewidywane użytkownika dla danego elementu. Na przykład w pierwszym wierszu klienta U1048 jest przewidziane do restauracji stawka 135026 jako 2.

**Zalecamy elementy do danego użytkownika**

Po zaznaczeniu **Rekomendacji elementu** w obszarze **Typ przewidywania Recommender**, prosisz systemu recommender zaleca elementów do danego użytkownika. Ostatni parametr wybierz w tym scenariuszu jest *zalecane zaznaczenia elementu*. Opcja **Z ocenianie elementów (oceny modelu)** jest przede wszystkim dla modelu obliczeń procesie szkolenie. Dla tego etapu przewidywania wybrany **Z wszystkich elementów**. Wizualizacja [Wynik Matchbox Recommender] [ score-matchbox-recommender] wynik wygląda rysunek 22.

![Wynik wynik systemu recommender — zalecenia elementu](./media/machine-learning-interpret-model-results/22.png)

Rysunek 22. Wizualizowanie wynik wynik systemu recommender — zalecenia elementu

Pierwszy sześć kolumn reprezentuje dany użytkownik identyfikatorów do zaleca elementów, zgodnie z danych wejściowych. Pięć kolumn reprezentują elementy zalecane użytkownikowi w dane w kolejności malejącej według istotności. Na przykład w pierwszym wierszu restauracji zalecany dla klienta U1048 jest 134986, a po nim 135018, 134975, 135021 i 132862.

**Znajdowanie użytkowników dotyczące danego użytkownika**

Po zaznaczeniu **Powiązanych użytkowników** w obszarze **Typ przewidywania Recommender**, prosisz system recommender znajdowanie powiązanych użytkowników do danego użytkownika. Powiązanych użytkowników są użytkownicy, którzy mają podobne preferencje. Ostatni parametr wybierz w tym scenariuszu jest *zaznaczenia użytkownikowi*. Opcja **Od użytkowników czy ocenianie elementów (oceny modelu)** jest przede wszystkim dla modelu obliczeń procesie szkolenie. Wybierz pozycję **Z wszystkich użytkowników** dla tego etapu przewidywania. Wizualizacja [Wynik Matchbox Recommender] [ score-matchbox-recommender] wynik wygląda rysunku 23.

![Wynik wynik systemu recommender — powiązanych użytkowników](./media/machine-learning-interpret-model-results/23.png)

Rysunek 23. Wizualizowanie wyniki wynik systemu recommender — powiązanych użytkowników

Pierwszy sześć kolumn pokazuje dany użytkownik identyfikatorów, aby znaleźć powiązanych użytkowników określonych danych wejściowych. Pięć kolumn zawierają przewidywane powiązanych użytkowników użytkownika w kolejności malejącej według istotności. Na przykład w pierwszym wierszu najtrafniejszych klienta dla klienta U1048 jest U1051, a po nim U1066, U1044, U1017 i U1072.

**Znajdowanie elementów powiązanych z danym elementem**

Po zaznaczeniu **Powiązanych elementów** w obszarze **Typ przewidywania Recommender**, zadajesz system recommender znajdowanie powiązanych elementów do danego elementu. Elementy pokrewne są najczęściej być łączone przez tego samego użytkownika elementów. Ostatni parametr wybierz w tym scenariuszu jest *zaznaczenia element pokrewny*. Opcja **Z ocenianie elementów (oceny modelu)** jest przede wszystkim dla modelu obliczeń procesie szkolenie. Dla tego etapu przewidywania wybrany **Z wszystkich elementów** . Wizualizacja [Wynik Matchbox Recommender] [ score-matchbox-recommender] wynik wygląda rysunek 24.

![Wynik wynik systemu recommender — powiązanych elementów](./media/machine-learning-interpret-model-results/24.png)

Rysunek 24. Wizualizowanie wyniki wynik systemu recommender — powiązanych elementów

Pierwszy sześć kolumn reprezentuje danego elementu identyfikatorów, aby znaleźć powiązanych elementów określonych danych wejściowych. Pięć kolumn zawierają przewidywane powiązanych elementów elementu w porządku malejącym pod względem istotności. Na przykład w pierwszym wierszu najtrafniejszych przedmiot dla 135026 jest 135074, a po nim 135035, 132875, 135055 i 134992.

**Publikowania usług sieci Web**

Proces publikowania tych doświadczeń jako usługi sieci web, aby uzyskać przewidywań przypomina dla każdego z czterech scenariuszy. W tym miejscu jako przykład przejść drugi scenariusz (zalecane elementy do danego użytkownika). Czy postępuj zgodnie z procedurą z pozostałe trzy.

Zapisywanie systemu przeszkolony recommender jako model przeszkolony i filtrowania danych wejściowych w kolumnie identyfikator jednego użytkownika, zgodnie z żądaniem, możesz nawiązać połączenie doświadczenia w 25 rysunek i opublikować go jako usługi sieci web.

![Wyników doświadczenia problemu rekomendacji restauracji](./media/machine-learning-interpret-model-results/25.png)

Rysunek 25. Wyników doświadczenia problemu rekomendacji restauracji

Uruchamianie usługi sieci web, zwracany wynik wygląda tak: 26 rysunek. Pięć restauracje zalecane dla użytkownika U1048 są 134986, 135018 134975, 135021 i 132862.

![Przykładowa usługa systemowa recommender](./media/machine-learning-interpret-model-results/25_1.png)

![Przykładowe wyniki doświadczenia](./media/machine-learning-interpret-model-results/26.png)

26 rysunek. Wynikiem usługi sieci Web restauracji rekomendacji problemu


<!-- Module References -->
[assign-to-clusters]: https://msdn.microsoft.com/library/azure/eed3ee76-e8aa-46e6-907c-9ca767f5c114/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-matchbox-recommender]: https://msdn.microsoft.com/library/azure/55544522-9a10-44bd-884f-9a91a9cec2cd/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-clustering-model]: https://msdn.microsoft.com/library/azure/bb43c744-f7fa-41d0-ae67-74ae75da3ffd/
[train-matchbox-recommender]: https://msdn.microsoft.com/library/azure/fa4aa69d-2f1c-4ba4-ad5f-90ea3a515b4c/
