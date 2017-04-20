<properties
    pageTitle="Wybierz parametry do Zoptymalizuj algorytmy analityków | Microsoft Azure"
    description="Wyjaśnia, jak wybrać optymalną ustawionym dla algorytmu analityków."
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


# <a name="choose-parameters-to-optimize-your-algorithms-in-azure-machine-learning"></a>Wybierz parametry do Zoptymalizuj algorytmy analityków

W tym temacie opisano jak wybrać prawo hyperparameter dla algorytmu analityków. Większość algorytmów uczenia maszynowego ma parametry, aby ustawić. Gdy pociąg modelu, należy podać wartości dla tych parametrów. Skuteczność wyszkolonych modelu zależy od parametrów modelu, które można wybrać. Proces znajdowania optymalny zestaw parametrów jest znany jako *Wybór modelu*.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Istnieją różne sposoby model zaznaczenia. W uczenie maszynowe krzyżowe sprawdzanie poprawności jest jednym z najbardziej powszechnie stosowanych metod wybór modelu i jest domyślnym mechanizmem wybór modelu w uczenia. Ponieważ usługą sieci Web obsługuje zarówno R i Python, zawsze można zaimplementować własne mechanizmy wybór modelu przy użyciu R lub Python.

Istnieją cztery kroki w procesie znalezienie najlepszych zestaw parametrów:

1.  **Definiowanie przestrzeni parametr**: algorytm, najpierw zdecydować wartości parametrów dokładną, warto rozważyć.
2.  **Określ krzyżowo ustawienia**: zdecydować, jak wybrać Fałdy krzyżowe sprawdzanie poprawności dla zestawu danych.
3.  **Definiuj Metryka**: zdecydować, jakie Metryka używaną do określania najlepszych zestaw parametrów, takich jak dokładność, średni korzeń kwadrat błąd, precision, wycofywania lub f-score.
4.  **Kolejowego, oceny i porównania**: dla każdej kombinacji unikatowych wartości parametrów krzyżowe sprawdzanie poprawności jest przeprowadzane przez i oparty na metryki błąd definiowania. Po oceny i porównania można wybrać model najwydajniejsze.

Poniższy obraz przedstawia pokazuje, jak można to osiągnąć analityków.

![Znajdź najlepszy zestaw parametrów](./media/machine-learning-algorithm-parameters-optimize/fig1.png)

## <a name="define-the-parameter-space"></a>Zdefiniować parametr miejsca
Można zdefiniować parametr ustawiony na etapie inicjowania modelu. Okienko parametr wszystkie algorytmów uczenia maszynowego w dwóch trybach trainer: *Jeden parametr* oraz *Parametr zakres*. Wybierz tryb parametr zakres. W trybie zakres parametru można wprowadzić wiele wartości dla każdego parametru. W polu tekstowym można wprowadzić wartości rozdzielanych przecinkami.

![Drzewo decyzyjne promowanego dwie klasy, jeden parametr](./media/machine-learning-algorithm-parameters-optimize/fig2.png)

 Alternatywnie można zdefiniować maksymalne i minimalne punkty siatki i łączna liczba punktów, które mają zostać wygenerowane z **Konstruktora zakresu wykorzystania**. Domyślnie wartości parametrów są generowane na skali liniowej. Ale jeśli **Skali logarytmicznej** jest zaznaczone, wartości są generowane w skali logarytmicznej (czyli stosunek punkty przylegające jest stała zamiast ich różnica). Dla parametrów liczba całkowita można zdefiniować zakres za pomocą dywizów. Na przykład "1-10" oznacza, że wszystkie liczby całkowite z przedziału 1 i 10 (zarówno włącznie) tworzą zestaw parametrów. Tryb mieszany jest także obsługiwane. Na przykład, ustaw parametr "1-10, 20, 50" obejmuje liczby całkowite 1-10, 20 i 50.

![Drzewo decyzyjne promowanego dwie klasy, zakres parametru](./media/machine-learning-algorithm-parameters-optimize/fig3.png)

## <a name="define-cross-validation-folds"></a>Definiowanie Fałdy krzyżowe sprawdzanie poprawności
[Partycji i próbki] [ partition-and-sample] moduł może służyć do losowo przypisać Fałdy do danych. W następującej konfiguracji próbki dla modułu definiujemy pięciu Fałdy i losowo przypisania numeru ze zgięciem wystąpień próbki.

![Partycji i próbki](./media/machine-learning-algorithm-parameters-optimize/fig4.png)


## <a name="define-the-metric"></a>Definiowanie Metryka
[Dostrój Model Hyperparameters] [ tune-model-hyperparameters] moduł zapewnia obsługę empirycznie wybór najlepszych zestaw parametrów dla danego algorytmu i zestawu danych. Oprócz innych informacji dotyczących szkoleń w modelu, okienko **Właściwości** tego modułu zawiera metrycznych dla określenia najlepszych zestaw parametrów. Ma ona dwa pola listy rozwijanej różnych algorytmów klasyfikacji i Regresja, odpowiednio. Jeśli algorytm rozważany jest algorytm klasyfikacji, metryki regresji jest ignorowana i na odwrót. W tym konkretnym przykładzie metryka jest **Dokładność**.   

![Parametry wycierania](./media/machine-learning-algorithm-parameters-optimize/fig5.png)

## <a name="train-evaluate-and-compare"></a>Szkolenie, oceny i porównywania  
Sam [Dostroić Model Hyperparameters] [ tune-model-hyperparameters] moduł pociągów wszystkich modeli, które odpowiadają zestaw parametrów, ocenia różnych miar, a następnie tworzy najlepiej wyszkolonych model oparty na metrykę można wybrać. Niniejszy moduł ma dwa wejścia obowiązkowe:

* Uczeń niewykwalifikowany
* Zestaw danych

Moduł ma także opcjonalny zestaw danych wejściowych. Łączenie zestawu danych składanie informacji do wprowadzania obowiązkowego zestawu danych. Jeśli zestaw danych nie przypisano żadnych informacji ze zgięciem, 10-krotnie krzyżowo jest automatycznie wykonywany domyślnie. Jeśli sprawdzanie poprawności zestawu danych są udostępniane w porcie opcjonalny zestaw danych i nie odbywa się przydział ze zgięciem, wybrany tryb testowy pociągu i pierwszy zestaw danych jest używany do pociągu modelu dla każdej kombinacji parametrów.

![Klasyfikator drzewa decyzji promowane](./media/machine-learning-algorithm-parameters-optimize/fig6a.png)

Model jest następnie sprawdzane w zestawie sprawdzania poprawności danych. Port wyjściowy lewej modułu pokazuje różnych typów danych jako funkcje wartości parametrów. Port wyjściowy prawo daje wyszkolonych modelu, który odpowiada modelowi najwydajniejsze zgodnie z wybranym metrykę (**Dokładność** w tym przypadku).  

![Sprawdzanie poprawności zestawu danych](./media/machine-learning-algorithm-parameters-optimize/fig6b.png)

Możesz zobaczyć dokładnych parametrów wybranego przez port wyjściowy prawo wizualizacji. Ten model może służyć w punktacji zestaw testów lub usługi sieci web utworzysz po zapisaniu jako wyszkolonych modelu.

<!-- Module References -->
[partition-and-sample]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[tune-model-hyperparameters]: https://msdn.microsoft.com/library/azure/038d91b6-c2f2-42a1-9215-1f2c20ed1b40/
