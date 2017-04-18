<properties 
    pageTitle="Debugowanie modelu w Azure maszynowego uczenia | Microsoft Azure" 
    description="Wyjaśniono, jak sposób debugowania modelu w Azure maszynowego uczenia." 
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
    ms.topic="article" 
    ms.date="09/09/2016" 
    ms.author="bradsev;garye" />

# <a name="debug-your-model-in-azure-machine-learning"></a>Debugowanie modelu w nauki Azure komputera

W tym artykule wyjaśniono, jak debugowanie modelach w Microsoft Azure maszynowego uczenia. W szczególności obejmuje potencjalne powodów, dlaczego jedną z następujących dwóch scenariuszy błąd może wystąpić podczas uruchamiania modelu:

* [Model pociągu] [ train-model] modułu zgłasza błąd 
* [Wynik modelu] [ score-model] modułu tworzy niepoprawne wyniki 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="train-model-module-throws-an-error"></a>Moduł modelu pociągu zgłasza błąd

![obraz1](./media/machine-learning-debug-models/train_model-1.png)

[Model pociągu] [ train-model] moduł oczekuje następujące 2 danych wejściowych:

1. Typ Model klasyfikacji/regresji z kolekcji modeli dostarczony przez nauki maszynowego Azure
2. Szkolenie dotyczące dane z określonej kolumny etykiet. Kolumna etykiety Określa zmienną do prognozowania. Pozostałe w kolumnach są wyświetlane są uznawane funkcje.

Moduł ten zgłasza błąd w następujących przypadkach:

1. Kolumna etykieta określono niepoprawnie, ponieważ więcej niż jedna kolumna jest zaznaczona jako etykiety lub niepoprawne indeks jest zaznaczone. Na przykład stosuje się drugim przypadku, jeśli użyto indeksu kolumny 30 z wprowadzania zestawu danych, którego został tylko 25 kolumn.

2. Zestaw danych nie zawiera żadnych kolumn funkcji. Na przykład jeśli wprowadzania zestaw danych zawiera tylko 1 kolumny, która nie została oznaczona jako etykiety kolumn, może istnieć żadne funkcje umożliwiające tworzenie modelu. W tym przypadku [Modelu pociągu] [ train-model] modułu będzie sygnalizować błąd.

3. Wprowadzania zestawu danych (funkcje lub etykietę) zawiera nieskończoności jako wartość.


## <a name="score-model-module-does-not-produce-correct-results"></a>Moduł modelu wynik nie daje niepoprawne wyniki

![obraz2](./media/machine-learning-debug-models/train_test-2.png)

Typowe wykresie badania i szkolenia dla nadzorowany nauki [Podzielone dane] [ split] modułu oryginalny zestaw danych są podzielone na dwie części: element, który jest używany do przeszkolenie modelu i element, który jest ograniczone do wynik, jak przeszkolony modelu wykonuje na danych go nie szkolenie dotyczące. Model przeszkolony należy wynik testowymi danymi, po upływie którego wyniki są obliczane w celu określenia dokładności modelu.

[Wynik modelu] [ score-model] modułu wymaga dwóch danych wejściowych:

1. Model przeszkolony wynikiem [Modelu pociągu] [ train-model] modułu
2. Wyników zestawu danych nie, która nie została ćwiczenie modelu

Może się zdarzyć, że mimo że doświadczenia zakończyło się powodzeniem, [Wynik modelu] [ score-model] modułu tworzy niepoprawne wyniki. Kilka scenariuszy może powodować to niepożądane:

1. Jeśli etykiety jest kategorii i model regresji jest ćwiczenie dane, nieprawidłowych danych wyjściowych czy wytwarzane przez [Model wynik] [ score-model] modułu. Jest to, ponieważ regresji wymaga zmiennej ciągły odpowiedź. W tym przypadku należy bardziej odpowiednie używać modelu klasyfikacji. 
2. Analogicznie jeśli model klasyfikacji jest ćwiczenie zestaw danych o przestawny liczby w kolumnie Etykieta, może powodować niepożądane wyniki. Jest to, ponieważ klasyfikacji wymaga zmienna osobne odpowiedź, która tylko umożliwia używanie wartości z zakresu skończonej i zazwyczaj nieco małych zestawu klas.
3. Jeśli zestaw wyników danych nie zawiera wszystkich funkcji przeszkolenie modelem, [Wynik modelu] [ score-model] zwrócą błąd.
4. [Wynik modelu] [ score-model] nie chcesz wygenerować dowolne dane wyjściowe odpowiadającego wiersza w zestawie wyników danych zawierającą wartość Brak lub nieograniczony dla każdego z jego funkcji.
5. [Wynik modelu] [ score-model] mogą być identyczne wyjściowe dla wszystkich wierszy w zestawie wyników danych. Ten błąd może wystąpić, na przykład, w sekcji podczas próby klasyfikacji przy użyciu lasów decyzji, jeśli wybrano minimalną liczbę próbek na węzeł liścia być więcej niż liczba przykładów szkoleniowych dostępne.


<!-- Module References -->
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
 
