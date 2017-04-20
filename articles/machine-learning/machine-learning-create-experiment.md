<properties
    pageTitle="Proste doświadczenia w studiu uczenia maszynowego | Microsoft Azure"
    description="Ten samouczek uczenia się maszyny przeprowadzi eksperyment nauki łatwe danych. Będziesz możemy przewidzieć ceną samochodu przy użyciu algorytmu regresji."
    keywords="eksperyment regresji liniowej, algorytmów uczenia maszynowego, samouczek uczenia się maszyny, technik modelowania predykcyjnej, eksperyment nauki danych"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="07/14/2016"
    ms.author="garye"/>

# <a name="machine-learning-tutorial-create-your-first-data-science-experiment-in-azure-machine-learning-studio"></a>Maszyna learning samouczek: Tworzenie pierwszego eksperymentu nauki danych w studiu uczenia maszynowego Azure

Ten samouczek uczenia się maszyny przeprowadzi eksperyment nauki łatwe danych. Tworzymy modelu regresji liniowej, który przewiduje cen samochodów na podstawie różnych zmiennych takich jak zrobić i specyfikacji technicznych. Aby to zrobić, użyjemy omówienie do opracowania i iteracyjne na proste predykcyjnej, że eksperymentować analytics.

*Predykcyjną* jest rodzajem wydobywania danych, który używa bieżących danych do przewidywania przyszłych wyników. Na bardzo prosty przykład predykcyjną, obejrzyj nauki danych dla początkujących wideo 4: [przewidzieć odpowiedzi z prostego modelu](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) (runtime: 7:42).

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="how-does-machine-learning-studio-help"></a>Jak pomóc Studio uczenia maszynowego?

Studio uczenia maszynowego ułatwia skonfigurowania eksperymentu za pomocą przeciągania i upuszczania modułów zaprogramowany technik modelowania predykcyjnej. Aby uruchomić eksperymentu i przewidzieć odpowiedzi, użyjesz Studio uczenia maszynowego do *tworzenia modelu*, *pociąg modelu*i *wynik i badania modelu*.

Wprowadź Studio uczenia maszynowego: [https://studio.azureml.net](https://studio.azureml.net). Po zalogowaniu się do komputera Learning Studio przed, kliknij przycisk **Zaloguj się tutaj**. W przeciwnym wypadku kliknij przycisk **Zarejestruj się** i wybrać jedną z opcji bezpłatnych i płatnych.

Aby uzyskać więcej ogólnych informacji na temat Studio uczenia maszynowego, zobacz [Co to jest Studio uczenia maszynowego?](machine-learning-what-is-ml-studio.md)

## <a name="five-steps-to-create-an-experiment"></a>Pięć kroków, aby utworzyć eksperyment

W tym komputerze nauki samouczek będzie wykonaj pięć podstawowe kroki tworzenia eksperymentu w studiu uczenia maszynowego w celu tworzenia, szkolenie i ocena modelu:

- Utwórz model
    - [Krok 1: Pobieranie danych]
    - [Krok 2: Przetwarzanie wstępne danych]
    - [Krok 3: Definiowanie funkcji]
- Pociągu modelu
    - [Krok 4: Wybrać i zastosować Algorytm uczenia się]
- Ocena i testowanie modelu
    - [Krok 5: Przewidzieć nowych cen samochodów]

[Krok 1: Pobieranie danych]: #step-1-get-data
[Krok 2: Przetwarzanie wstępne danych]: #step-2-preprocess-data
[Krok 3: Definiowanie funkcji]: #step-3-define-features
[Krok 4: Wybrać i zastosować Algorytm uczenia się]: #step-4-choose-and-apply-a-learning-algorithm
[Krok 5: Przewidzieć nowych cen samochodów]: #step-5-predict-new-automobile-prices


## <a name="step-1-get-data"></a>Krok 1: Pobieranie danych

Istnieje wiele przykładowych zestawów danych dołączone Studio uczenia maszynowego, które można wybierać i można importować dane z wielu źródeł. W tym przykładzie użyjemy dołączona przykładowa zestawu danych, **danych dotyczących cen samochodów (Raw)**.
Ten zestaw danych zawiera wpisy dla liczby pojedynczych samochodów, w tym informacje takie jak marka, model, specyfikacje techniczne i ceny.

1. Rozpocznij nowy eksperyment klikając **+ Nowy** u dołu okna Studio uczenia maszynowego, wybierz **EKSPERYMENTOWAĆ**, a następnie wybierz **Pusty eksperymentować**. Wybierz nazwę domyślną doświadczenia w górnej części obszaru roboczego i zmień jego nazwę na inną opisową, na przykład, **przewidywania cen samochodów**.

2. Z lewej strony eksperymentu kanwy jest paleta zestawów danych i modułów. Typ **samochodu** w polu wyszukiwania u góry tej palety, aby znaleźć zestaw danych etykietę **danych dotyczących cen samochodów (Raw)**.

    ![Szukaj palety][screen1a]

3. Przeciągnij zestawu danych na obszar roboczy eksperymentu.

    ![Zestaw danych][screen1]

Aby zobaczyć, jak wygląda te dane, kliknij port wyjściowy w dolnej części samochodowych zestawu danych, a następnie wybierz **Wizualizacja**.

![Port wyjściowy modułu][screen1c]

Zmienne w zestawie danych są wyświetlane jako kolumny, a każde wystąpienie samochód jest wyświetlany jako wiersz. Prawej kolumnie (kolumna 26 i zatytułowany "cena") jest zmienną docelową, który zamierzamy spróbować przewidzieć.

![Wizualizacja zestawu danych][screen1b]

Zamknij okno wizualizacji, klikając przycisk "**x**" w prawym górnym rogu.

## <a name="step-2-preprocess-data"></a>Krok 2: Przetwarzanie wstępne danych

Zestawu danych wymaga zwykle niektóre wstępne przetwarzanie przed mogą być analizowane. Być może zauważono brakujące wartości w kolumnach różnych wierszy. Te brakujące wartości muszą być czyszczone, więc model można prawidłowo analizować dane. W naszym przypadku usuniemy wszystkie wiersze, które nie mają wartości. Ponadto kolumna **znormalizowane straty** ma znaczną część brakujących wartości, więc możemy całkowicie wykluczenie tej kolumny z modelu.

> [AZURE.TIP] Czyszczenie brakujących wartości z danych wejściowych jest warunkiem wstępnym przy użyciu większości modułów.

Najpierw usuniemy kolumnę **znormalizowane straty** , a następnie usuniemy każdy wiersz, który ma brakujące dane.

1. W polu wyszukiwania u góry palety moduł [Wybierz kolumny w zestawie danych] wpisz **Wybierz kolumny** [ select-columns] moduł, a następnie przeciągnij go do doświadczenia kanwy i podłącz je do portu wyjściowego zestawu danych, **danych dotyczących cen samochodów (Raw)** . Moduł ten pozwala nam wybrać kolumny danych, które chcemy, aby dołączyć lub wykluczyć w modelu.

2. Wybierz [Wybierz kolumny w zestawie danych] [ select-columns] moduł i kliknij przycisk **Uruchom selektor kolumny** w okienku **Właściwości** .

    - Po lewej stronie kliknij przycisk **z regułami**
    - W obszarze **Rozpocząć z**kliknij **wszystkie kolumny**. To instruuje [Wybierz kolumny w zestawie danych] [ select-columns] przejść przez wszystkie kolumny (z wyjątkiem tych mamy zamiar wykluczyć).
    - Z listy rozwijane Wybierz **Wyklucz** i **nazwy kolumn**, a następnie kliknij wewnątrz pola tekstowego. Zostanie wyświetlona lista kolumn. Wybierz **znormalizowane straty**, a zostaną dodane do pola tekstowego.
    - Kliknij przycisk znacznik wyboru (OK), aby zamknąć Selektor kolumny.

    ![Wybierz kolumny][screen3]

    W okienku właściwości dla **Wybierz kolumny w zestawie danych** wskazuje, że będzie przechodzić przez wszystkie kolumny z zestawu danych z wyjątkiem **znormalizowanych straty**.

    ![Wybierz kolumny w właściwości zestawu danych][screen4]

    > [AZURE.TIP] Dwukrotne kliknięcie modułu i wprowadzając tekst, można dodać komentarz do modułu. Może to pomóc w Zobacz na pierwszy rzut oka, co robi moduł w eksperymencie. W takim przypadku kliknij dwukrotnie [Wybierz kolumny w zestawie danych] [ select-columns] moduł i wpisz komentarz "Wyklucz znormalizowane straty."

3. Przeciągnij [Czystego brakujące dane] [ clean-missing-data] przez moduł eksperymentu kanwy i połączyć go z [Wybierz kolumny w zestawie danych] [ select-columns] modułu. W okienku **Właściwości** wybierz **Usuń cały wiersz** w polu **Tryb czyszczenia** do czyszczenia danych przez usunięcie wierszy, które nie mają wartości. Kliknij dwukrotnie moduł i wpisz komentarz "Usuń brakujących wierszy wartość".

    ![Czyste właściwości brakujących danych][screen4a]

4. Uruchomienie eksperymentu, klikając przycisk **Uruchom** w obszarze roboczym doświadczenia.

Po zakończeniu eksperymentu, wszystkie moduły mają zielony znacznik wyboru, aby wskazać, że one zakończone pomyślnie. Ponadto stan **zakończone uruchomiona** w prawym górnym rogu.

![Doświadczenia pierwszego uruchomienia][screen5]

To wszystko, co mamy zrobić w doświadczeniu z tym punktem, czyszczenia danych. Jeśli chcesz wyświetlić oczyszczone zestaw danych, kliknij port wyjściowy lewej [Czystego brakujące dane] [ clean-missing-data] moduł ("oczyszczone w zestawie danych") i wybierz opcję **Wizualizacja**. Należy zauważyć, że kolumna **znormalizowane strat** nie jest już dostarczany i nie ma żadnych brakujących wartości.

Teraz, że dane są czyste, możemy przystąpić do określić jakie funkcje zamierzamy w przewidywanych modelu.

## <a name="step-3-define-features"></a>Krok 3: Definiowanie funkcji

W maszynie nauki *Funkcje* są wymierne właściwości poszczególnych coś, który Cię interesuje. W naszym dataset każdy wiersz reprezentuje jeden samochód, a każda kolumna jest cechą tego samochodu.

Znalezienie dobrego zestawem funkcji umożliwiających tworzenie modelu predykcyjnej wymaga doświadczenia i wiedzy o problemie, który chcesz rozwiązać. Niektóre funkcje są lepsze do przewidywaniu miejsce docelowe niż inne. Ponadto, niektóre funkcje mają silne powiązania z innymi funkcjami (na przykład miasto mpg kontra autostrada mpg), więc nie będzie dodać wiele nowych informacji do modelu i mogą być usunięte.

Niech utworzyć model, który używa tylko podzbiór funkcji w naszym zestawu danych. Możesz wrócić i wybrać różne funkcje, ponownie uruchom doświadczenia i sprawdzić, czy możesz uzyskać lepsze wyniki. Aby rozpocząć, spróbujmy jednak następujące funkcje:

    make, body-style, wheel-base, engine-size, horsepower, peak-rpm, highway-mpg, price


1. Przeciągnij inny [Wybierz kolumny w zestawie danych] [ select-columns] moduł do doświadczenia kanwy i podłącz je do portu wyjściowego lewej [Czystego brakujące dane] [ clean-missing-data] modułu. Kliknij dwukrotnie moduł i wpisz "Wybierz funkcje do przewidywania."

2. Kliknij przycisk **Uruchom selektor kolumny** w okienku **Właściwości** .

3. Kliknij przycisk **z regułami**.

4. W obszarze **Rozpocząć z** kliknij **żadnych kolumn**, a następnie wybierz **Include** i **nazwy kolumn** w wierszu filtrów. Wprowadź listę nazw kolumn. Poleca moduł przejść tylko kolumny, które jest określona.

    > [AZURE.TIP] Uruchamiając eksperymentu, zapewniamy, że definicje kolumn dla naszych danych przez [Czyste brakujące dane] przechodzą z zestawu danych[ clean-missing-data] modułu. Oznacza to, że inne moduły, gdy połączysz będą również informacje z zestawu danych.

5. Kliknij przycisk znacznik wyboru (OK).

![Wybierz kolumny][screen6]

Produkuje zestawu danych, który będzie używany w Algorytm uczenia się w kolejnych krokach. Później wrócić i spróbować ponownie z innego wyboru funkcji.

## <a name="step-4-choose-and-apply-a-learning-algorithm"></a>Krok 4: Wybrać i zastosować Algorytm uczenia się

Teraz, że dane są gotowe, konstruowanie predykcyjnej modelu składa się z kształcenia i testowania. Pociągu modelu, a następnie przetestować model, aby zobaczyć, jak daleko jest w stanie przewidzieć ceny będziemy używać naszych danych. Na razie nie przejmuj się Dlaczego musimy pociągu i przetestowanie modelu.

*Klasyfikacja* i *regresji* są dwa typy nadzorowane maszyny technik nauczania. Klasyfikacja przewiduje odpowiedzi ze zdefiniowanym zestawem kategorii, takich jak kolorów (czerwony, niebieski lub zielony). Regresja jest używany do przewidywania numeru.

Ponieważ chcemy, aby przewidzieć, cena, która jest liczbą, użyjemy modelu regresji. W tym przykładzie będziemy szkolić model prostej *regresji liniowej* , a w następnym kroku, możemy go przetestować.

1. Możemy korzystać z danych na szkolenia i badań przeprowadzanych przez dzielenie go na oddzielne szkolenia i testowania zestawów. Wybierz i przeciągnij [Podziału danych] [ split] przez moduł eksperymentu kanwy i podłącz go do wyjścia [Wybierz kolumny w zestawie danych] [ select-columns] modułu. Ustaw **część wierszy w pierwszym zestaw danych wyjściowych** do 0,75. Dzięki temu będziemy używać 75 procent danych do pociągu modelu i powstrzymywać 25 procent do testowania.

    > [AZURE.TIP] Zmieniając wartość **Losowa materiału siewnego** służy do tworzenia różnych próbek losowych szkolenia i testowania. Ten parametr kontroluje posiew generator liczb pseudolosowych.

2. Uruchomienie eksperymentu. Umożliwia [Wybieranie kolumn w zestawie danych] [ select-columns] i [Podziału danych] [ split] moduły, aby przekazać definicje kolumn do modułów będziemy dodawać następny.  

3. Aby wybrać algorytm uczenia, rozwiń kategorię **Szkoleń maszyna** w palecie moduł z lewej strony obszaru roboczego, a następnie rozwiń **Zainicjować modelu**. Wyświetla kilka kategorii moduły, które mogą być używane do inicjowania algorytmów uczenia maszynowego.

    Do tego eksperymentu, wybierz [Regresji liniowej] [ linear-regression] modułu w ramach kategorii **regresji** (moduł można znaleźć również przez wpisanie w polu wyszukiwania paleta "regresji liniowej") i przeciągnij go do obszaru roboczego eksperymentu.

4. Znajdź i przeciągnij [Model pociągu] [ train-model] modułu do obszaru roboczego eksperymentu. Połącz port wejściowy lewego w wyniku [Regresji liniowej] [ linear-regression] modułu. Połączenia prawym portu wejściowego szkolenia dane wyjściowe (port lewej) [Podziału danych] [ split] modułu.

5. Wybierz [Model pociągu] [ train-model] moduł, kliknij polecenie **Uruchom selektor kolumny** w okienku **Właściwości** , a następnie zaznacz kolumnę **Cena** . Jest to wartość, która ma zamiar przewidzieć naszego modelu.

    ![Zaznacz kolumnę "cena"][screen7]

6. Uruchomienie eksperymentu.

Wynik jest używany do nowe próbki do przewidywania wyniku modelu regresyjnego przeszkolony.

![Stosowanie Algorytm uczenia maszynowego][screen8]

## <a name="step-5-predict-new-automobile-prices"></a>Krok 5: Przewidzieć nowych cen samochodów

Teraz, że mamy już przeszkoleni modelu przy użyciu 75 procent naszych danych, stosujemy je zdobyć pozostałe 25 procent danych, aby zobaczyć, jak dobrze funkcje naszego modelu.

1. Znajdź i przeciągnij [Modelu wynik] [ score-model] moduł do eksperyment kanwy i podłączyć lewym port wejściowy do wyjścia [Model pociągu] [ train-model] modułu. Połączyć prawo port wejściowy do testu dane wyjściowe (prawo port) [Podziału danych] [ split] modułu.  

    ![Ocena na modelu modułu][screen8a]

2. Aby uruchomić doświadczenia i wyświetlania danych wyjściowych z [Modelu wynik] [ score-model] moduł, kliknij port wyjściowy, a następnie wybierz **Wizualizacja**. Dane wyjściowe pokazuje przewidywane wartości dla ceny i znanych wartości z danych testowych.  

3. Wreszcie, aby przetestować jakość wyników, zaznacz i przeciągnij [Oceny modelu] [ evaluate-model] moduł do eksperyment kanwy i podłączyć lewym port wejściowy do wyjścia [Modelu wynik] [ score-model] modułu. (Istnieją dwa porty wejściowe, ponieważ [Model oceny] [ evaluate-model] moduł może służyć do porównywania dwóch modeli.)

4. Uruchomienie eksperymentu.

Aby wyświetlić dane wyjściowe z [Oceny modelu] [ evaluate-model] moduł, kliknij port wyjściowy, a następnie wybierz **Wizualizacja**. Dla naszego modelu wyświetlane są następujące dane statystyczne:

- **Oznacza błąd bezwzględne** (MAE): średnia błędy bezwzględne ( *Błąd* jest różnica między wartością rzeczywistą a prognozowanej wartości).
- **Średni błąd kwadrat głównego** (RMSE): pierwiastek kwadratowy z średnią kwadratów błędów prognoz na test dataset.
- **Względne odchylenie bezwzględne**: średnia błędy bezwzględne względem różnica bezwzględna między wartościami rzeczywistymi a średnią wszystkich wartości rzeczywistych.
- **Względny błąd kwadratowy**: Średnia kwadratów błędów względem kwadrat różnicy między wartościami rzeczywistymi a średnią wszystkich wartości rzeczywistych.
- **Współczynnik oznaczanie**: znany również jako **wartość R-kwadrat**, to metryka statystyczne wskazujące, jak również modelu dopasowanie do danych.

Dla każdego błędu statystyki, mniejsze jest lepszy. Mniejszą wartość wskazuje, że prognozy dopasować wartości rzeczywiste. **Współczynnik oznaczanie**, im bliżej jej wartość jest jeden (1.0), tym lepiej prognoz.

![Wyniki oceny][screen9]

Końcowe eksperyment powinien wyglądać następująco:

![Samouczek uczenia się maszyny: zakończyć eksperyment regresji liniowej, który używa techniki modelowania predykcyjnej.][screen10]

## <a name="next-steps"></a>Następne kroki

Teraz, zakończyła się pierwszy poradnik uczenia się maszyny i mieć eksperymentu, ustawianie, użytkownik może przejść spróbować zwiększyć modelu. Na przykład można zmienić używanych w swoim przewidywania funkcji. Lub zmodyfikować właściwości [Regresji liniowej] [ linear-regression] algorytm lub wypróbuj inny algorytm całkowicie. Można również dodać wiele maszyny, w tym samym czasie nauki algorytmów do eksperymentu i porównywania dwóch przy użyciu [Modelu oceny] [ evaluate-model] modułu.

> [AZURE.TIP] Przycisk **ZAPISZ jako** w obszarze roboczym doświadczenia umożliwia kopiowanie każdej iteracji eksperymentu. Wszystkich iteracji eksperymentu można wyświetlić, klikając **Zobacz uruchomić HISTORIĘ** w obszarze roboczym. Zobacz [Manage eksperymentować iteracji w omówienie] [ runhistory] więcej informacji.

[runhistory]: machine-learning-manage-experiment-iterations.md

Kiedy jesteś zadowolony z danym modelem, można wdrożyć go jako usługi sieci web ma być używany do przewidywania cen samochodów przy użyciu nowych danych. Zobacz [Wdrażanie usługi sieci web z usługą sieci Web] [ publish] więcej informacji.

[publish]: machine-learning-publish-a-machine-learning-web-service.md

Dla bardziej rozległe i szczegółowe wskazówki modelowanie predykcyjne techniki tworzenia, szkolenia, punktacji i wdrażania modelu, zobacz [opracowanie rozwiązania do analizy predykcyjnej korzystając z usługą sieci Web][walkthrough].

[walkthrough]: machine-learning-walkthrough-develop-predictive-solution.md

<!-- Images -->
[screen1]:./media/machine-learning-create-experiment/screen1.png
[screen1a]:./media/machine-learning-create-experiment/screen1a.png
[screen1b]:./media/machine-learning-create-experiment/screen1b.png
[screen1c]: ./media/machine-learning-create-experiment/screen1c.png
[screen2]:./media/machine-learning-create-experiment/screen2.png
[screen3]:./media/machine-learning-create-experiment/screen3.png
[screen4]:./media/machine-learning-create-experiment/screen4.png
[screen4a]:./media/machine-learning-create-experiment/screen4a.png
[screen5]:./media/machine-learning-create-experiment/screen5.png
[screen6]:./media/machine-learning-create-experiment/screen6.png
[screen7]:./media/machine-learning-create-experiment/screen7.png
[screen8]:./media/machine-learning-create-experiment/screen8.png
[screen8a]:./media/machine-learning-create-experiment/screen8a.png
[screen9]:./media/machine-learning-create-experiment/screen9.png
[screen10]:./media/machine-learning-create-experiment/complete-linear-regression-experiment.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
