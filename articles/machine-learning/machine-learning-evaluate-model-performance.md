<properties 
    pageTitle="Ocenianie wydajności modelu w maszynowego uczenia | Microsoft Azure" 
    description="Wyjaśniono, jak ma być obliczona wydajność modelu w Azure maszynowego uczenia." 
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
    ms.date="08/19/2016" 
    ms.author="bradsev;garye" />


# <a name="how-to-evaluate-model-performance-in-azure-machine-learning"></a>Jak ma być obliczona wydajność modelu w nauki maszynowego Azure

W tym temacie dowiesz się, jak ma być obliczona wydajność modelu Azure maszynowego uczenia Studio, a także krótki opis metryki dostępnych dla tego zadania. Przedstawiono trzy typowe scenariusze pod nadzorem nauki: 

* regresji
* binarny klasyfikacji 
* multiclass Klasyfikacja

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


Ocena wydajności modelu jest jednego z etapów core w procesie nauki danych. Wskazuje, jak pomyślnego wyników (przewidywań) zestawu danych zostało przez model przeszkolony. 

Azure nauki komputer obsługuje oceny modelu za pomocą dwóch jego głównym maszynowego uczenia moduły: [Oceny modelu] [ evaluate-model] i [Sprawdź poprawność krzyżowe Model][cross-validate-model]. Te moduły umożliwiają Zobacz, jak modelu wykonuje pod względem liczbę miar, które są często używane w maszynowego uczenia się i statystyki.

##<a name="evaluation-vs-cross-validation"></a>Sprawdzanie poprawności oceny a krzyżowe##
Ocena i krzyżowego sprawdzania poprawności są standardowe metody do pomiaru wydajności modelu. Oba generują metryk oceny można sprawdzić lub porównania tych innych modeli.

[Ocena modelu] [ evaluate-model] oczekuje scored zestaw danych jako danych wejściowych (lub 2 w przypadku, gdy chcesz porównać wydajność 2 modeli). Oznacza to, że konieczne przeszkolenie modelu przy użyciu [Modelu przeszkolenie] [ train-model] modułu i uzupełnić przewidywań na niektórych zestawu danych przy użyciu [Modelu wynik] [ score-model] moduł, zanim możesz oszacować wyników. Oszacowanie jest na podstawie scored etykiet i prawdopodobieństwa wraz z PRAWDA etykiety, które są dane wyjściowe [Modelu wynik] [ score-model] modułu.

Możesz też umożliwia krzyżowego sprawdzania poprawności wykonywanie wielu czynności oceny wynik pociągu (zgięcia 10) automatycznie na różnych podzbiorów danych wejściowych. Dane wejściowe jest podzielony na 10 części, w których jedna jest zarezerwowana do testowania, i innych 9 szkoleń. Ten proces jest powtarzany 10 razy i zostaną obliczone metryki oceny. Dzięki temu podczas ustalania, jak model będzie generalize do nowych baz danych. [Sprawdź poprawność krzyżowe modelu] [ cross-validate-model] moduł pobiera model nieprzeszkolonych i niektórych oznaczona etykietą zestawu danych i wyświetla wyniki oceny każdego z 10 składając je, oprócz średnią wyników.

W poniższych sekcjach, firma Microsoft będzie tworzenia prostej regresji i klasyfikacji modele i oceny ich działania przy użyciu [Modelu oceny] [ evaluate-model] i [Krzyżowe sprawdzanie poprawności modelu] [ cross-validate-model] moduły.

##<a name="evaluating-a-regression-model"></a>Ocena modelu regresji##
Załóżmy, że chcemy przewidywanie samochodu cena za pomocą niektórych funkcji, takich jak wymiary, Angielski koń parowy, specyfikacje aparat i tak dalej. Jest to problem typowe regresji, gdzie zmienną docelową (*Cena*) jest wartością liczbową ciągły. Firma Microsoft może składać modelu prosta regresji liniowej, który uwzględniając wartości funkcji samochodu można prognozować cena tego samochodu. Ten model regresji może służyć do tego samego zestawu danych, które możemy ćwiczenie wynik. Gdy mamy już prognozowane ceny dla wszystkich samochodów, możemy ocenić wydajności modelu sprawdzając ile przewidywań różni się od rzeczywistych cen średnio. Aby to zilustrować, firma Microsoft korzysta z *samochodów cena danych (Raw) w zestawie danych* dostępnych w sekcji **Zapisane zestawy danych** Azure maszynowego uczenia Studio.
 
###<a name="creating-the-experiment"></a>Tworzenie doświadczenia###
Dodaj następujące moduły do obszaru roboczego w Azure maszynowego uczenia Studio:

- Cena samochodów danych (Raw)
- [Regresji liniowej.][linear-regression]
- [Model pociągu][train-model]
- [Wynik modelu][score-model]
- [Ocena modelu][evaluate-model]


Łączenie portów, jak pokazano poniżej na rysunku 1 i w kolumnie Etykieta [Modelu pociągu] ustaw[ train-model] moduł *ceny*.
 
![Ocena modelu regresji](media/machine-learning-evaluate-model-performance/1.png)

Rysunek 1. Szacowanie modelu regresji.

###<a name="inspecting-the-evaluation-results"></a>Sprawdzanie wyników oceny###
Po uruchomieniu doświadczenia, możesz kliknąć port wyjściowy [Oceny modelu] [ evaluate-model] moduł i wybierz pozycję *Wizualizacja* , aby wyświetlić wyniki obliczeń. Są dostępne w przypadku modeli regresji metryki oceny: *Oznacza błędu bezwzględnego*, *Głównym oznacza błędu bezwzględnego* *Względne błędu bezwzględnego*, *Względny błąd kwadrat*i *Współczynnik oznaczanie*.

Termin "błąd" poniżej stanowi różnicę między prognozowanej wartości i wartość true. Wartość bezwzględna lub kwadrat ta różnica są zwykle obliczany do przechwytywania całkowita wielkość błędu we wszystkich wystąpieniach jako różnicy między wartością przewidywane i true może być ujemna w niektórych przypadkach. Wskaźniki błędów pomiaru wydajności przewidywanych modelu regresji pod względem odchylenie średniej jego przewidywań z wartości true. Niższe wartości błędu oznacza, że model jest dokładniejsze dokonując przewidywań. Ogólny błąd metrykę 0 oznacza, że modelu dopasowany danych.

Współczynnik wyznaczania, który jest nazywany także R kwadrat, jest również standardowy sposób pomiaru, jak dopasowanie do danych w modelu. Może być interpretowana jako część odmian wyjaśniono przez model. Wyższa udział w tym przypadku jest lepsza w przypadku 1 wskazuje dokładne dopasowanie.
 
![Metryki oceny regresji liniowej.](media/machine-learning-evaluate-model-performance/2.png)

Rysunek 2. Metryki oceny regresji liniowej.

###<a name="using-cross-validation"></a>Za pomocą krzyżowe sprawdzanie poprawności###
Jak wspomniano wcześniej, można wykonać powtarzających się szkolenia, wyników i ocen automatycznie przy użyciu [Krzyżowe sprawdzanie poprawności modelu] [ cross-validate-model] modułu. W tym przypadku wystarczy zestawu danych, model nieprzeszkolonych i [Krzyżowe sprawdzanie poprawności modelu] [ cross-validate-model] modułu (patrz rysunek poniżej). Uwaga należy ustawić kolumny etykietę *Cena* w [Krzyżowe sprawdzanie poprawności modelu] [ cross-validate-model] właściwości modułu.

![Krzyżowe — sprawdzanie poprawności modelu regresji](media/machine-learning-evaluate-model-performance/3.png)

Rysunek 3. Krzyżowe — sprawdzanie poprawności modelu regresji.

Po uruchomieniu doświadczenia, można sprawdzić wyniki obliczeń, klikając pozycję na port wyjściowy prawo [Krzyżowe sprawdzanie poprawności modelu] [ cross-validate-model] modułu. Szczegółowy widok metryki zapewnia dla każdej iteracji (składanie) i uśrednionych wyniki miar (rysunek 4).
 
![Wyniki sprawdzania poprawności krzyżowe modelu regresji](media/machine-learning-evaluate-model-performance/4.png)

Rysunek 4. Wyniki sprawdzania poprawności krzyżowe modelu regresji.

##<a name="evaluating-a-binary-classification-model"></a>Oceny Model klasyfikacji binarny##
W scenariuszu binarne klasyfikacji zmienną docelową obejmuje tylko dwa możliwych wyników, na przykład: {0, 1} lub {FAŁSZ, PRAWDA}, {dodatnie, ujemne}. Przyjęto założenie, zostanie wyświetlony zestaw danych dorosłego pracowników z niektórych demograficzne i zatrudnienia zmienne i że zostanie wyświetlony monit o przewidywanie poziomu dochodów, binarne zmiennej z wartościami {"< = 50K", "> 50K"}. Innymi słowy ujemne klasy reprezentuje pracowników, którzy mniejsze lub równe 50K na rok, a dodatnie zajęć — inni pracownicy. Tak jak w scenariuszu regresji możemy przeszkolenie modelu, wynik niektóre dane i oceniać wyniki. Główny różnica polega na tym wybór metryk, które oblicza Azure maszynowego uczenia i wyjściowe. Aby przedstawić scenariusz poziomu przewidywania dochód, użyjemy [dorosłego](http://archive.ics.uci.edu/ml/datasets/Adult) zestawu danych do tworzenia doświadczenia Azure maszynowego uczenia i oceniać wydajność modelu regresją klasy dwóch często używane klasyfikatora binarne.

###<a name="creating-the-experiment"></a>Tworzenie doświadczenia###
Dodaj następujące moduły do obszaru roboczego w Azure maszynowego uczenia Studio:

- Dorosłego klasyfikacji dwójkową dochód spisu zestawu danych
- [Regresją dwóch zajęć][two-class-logistic-regression]
- [Model pociągu][train-model]
- [Wynik modelu][score-model]
- [Ocena modelu][evaluate-model]

Łączenie portów, jak pokazano poniżej na rysunku 5 i ustaw kolumna etykieta [Modelu pociągu] [ train-model] moduł *dochód*.

![Oceny Model klasyfikacji binarny](media/machine-learning-evaluate-model-performance/5.png)

Rysunek 5. Szacowanie Model klasyfikacji binarne.

###<a name="inspecting-the-evaluation-results"></a>Sprawdzanie wyników oceny###
Po uruchomieniu doświadczenia, możesz kliknąć port wyjściowy [Oceny modelu] [ evaluate-model] moduł i wybierz pozycję *Wizualizacja* , aby wyświetlić wyniki obliczeń (rysunek 7). Są dostępne w przypadku modeli binarne klasyfikacji metryki oceny: *Dokładność*, *dokładności* *odwołać*, *Wynik F1*i *AUC*. Ponadto moduł Wyświetla macierzy zamieszania przedstawiające liczbę wyniki dodatnie PRAWDA, FAŁSZ negatywy, wyniki fałszywie dodatnie i negatywy PRAWDA, a także krzywych *ROC*, *Dokładność i odwoływanie*i *Podnieś* .

Dokładność jest po prostu część poprawnie niejawnych wystąpienia. Zazwyczaj jest pierwszy metryczne, których możesz przeglądać przy obliczaniu klasyfikatora. Jednak, gdy testowymi danymi jest niezrównoważone (miejsce, w którym większość wystąpień należą do jednej z grup) lub jeśli jest bardziej interesują w trakcie wykonywania jednego klasy z dokładność naprawdę nie Przechwytywanie skuteczność klasyfikatora. W scenariuszu poziomu klasyfikacji dochód założono, że testujesz niektóre dane, gdzie 99% wystąpień reprezentują osoby, które gromadzenie mniejsze lub równe 50 K na rok. Istnieje możliwość osiągnięcia 0.99 dokładność przez przewidywania klasy "< = 50K" dla wszystkich wystąpień. Klasyfikator pojawi się w tym przypadku do wykonania czynności nieźle ogólnego, ale w rzeczywistości nie jest on klasyfikować dowolną osób high-income (1%) poprawnie.

Z tego powodu warto obliczyć dodatkowe metryk Przechwytywanie ściśle określonych aspektów oszacowanie. Przed przejściem do szczegółów takie wskaźniki, jest zrozumieć macierzy zamieszania oceny klasyfikacji binarne. Etykiety klasy w zestawie szkolenie może potrwać na tylko 2 możliwe wartości, które zwykle określane jako dodatnie lub ujemne. Wystąpienia dodatnie i ujemne, które klasyfikatora przewiduje poprawnie są nazywane odpowiednio PRAWDA wyniki dodatnie (TP) i negatywy PRAWDA (TN). Podobnie niepoprawnie niejawnych wystąpień są nazywane wyniki fałszywie dodatnie (FP) i negatywy FAŁSZ (FN). Macierz zamieszania jest po prostu tabelę przedstawiającą liczbę wystąpień zaliczanych do każdej z tych kategorii 4. Azure nauki komputera automatycznie zdecyduje, który z dwóch klas w zestawie danych jest dodatnia zajęć. W przypadku wartość logiczna lub liczb całkowitych etykiety klasy wystąpienia oznaczona etykietą "prawda" lub "1" są przypisywane dodatnia zajęć. Jeśli etykiety są ciągami, tak jak w przypadku zestawu danych dochód etykiet są sortowane alfabetycznie i pierwszego poziomu została wybrana jest ujemna zajęć, podczas gdy drugiego poziomu jest dodatnia zajęć.

![Macierz zamieszania binarne klasyfikacji](media/machine-learning-evaluate-model-performance/6a.png)

Rysunek 6. Macierz zamieszania klasyfikacji binarne.

Rozważmy ponownie problemu klasyfikacji dochód, czy chcemy z pytaniem, czy szereg pytań oceny, które pomagają nam zrozumieć wydajności klasyfikatora używane. Jest bardzo naturalne pytanie: "poza osób, których modelu przewidziane na być zdobywanie > 50 K (TP + FP), ile były sklasyfikowane poprawnie (TP)?" To pytanie można odpowiedzieć sprawdzając **dokładności** modelu, który jest część wyniki dodatnie, które są sklasyfikowane poprawnie: TP/(TP+FP). Jest innego często zadawane pytania "poza wszystkich wysoki oprocentowaniu pracowników, którzy mają dochód > 50 k (TP + FN), ile klasyfikatora klasyfikowania poprawnie (TP)". Jest to faktycznie **odwołać**lub stopa dodatnią wartość PRAWDA: TP/(TP+FN) klasyfikatora. Może się okazać, że jest oczywiste dokonując dokładność i odwoływanie. Na przykład mieć stosunkowo strategii zestawu danych, Klasyfikator, który przewiduje głównie dodatnia wystąpienia woli wysoka odwołania, ale raczej niski dokładności dowolnej liczby ujemne wystąpienia może być nieprawidłowo klasyfikowana uzyskując dużej liczby wyniki fałszywie dodatnie. Aby wyświetlić wykres jak różnią się w tych dwóch miar, możesz kliknąć na krzywej "Dokładność i ODWOŁYWANIE" na stronie dane wyjściowe wynik obliczeń (u góry z lewej części rysunek 7).

![Wyniki obliczeń binarne klasyfikacji](media/machine-learning-evaluate-model-performance/7.png) rysunek 7. Wyniki obliczeń klasyfikacji binarne.

Inny metryki powiązanych często używany jest **Wynik F1**, który ma zarówno dokładności, jak i zastępująca pod uwagę. Średnia harmoniczna tych metryki 2, a jest obliczana jako takie: F1 = 2 (odwołanie dokładności x) / (dokładność + odwołanie). Wynik F1 jest dobrym sposobem podsumowywania oceny w wyniku jedną liczbę, ale zawsze jest dobrym rozwiązaniem jest przeglądać zarówno dokładności, jak i zastępująca ze sobą, aby lepiej zrozumieć, zachowanie klasyfikatora.

Ponadto jedną inspekcja PRAWDA dodatniej stawki, a FAŁSZ dodatniej stawki krzywej **Słuchawka operacyjnym cechy (ROC)** i odpowiadające im wartości **Obszar w obszarze krzywej (AUC)** . Dokładniejsze krzywej lewym górnym rogu, tym lepsze jest wydajności klasyfikatora (który jest maksymalizacja PRAWDA dodatniej stawki minimalizując stopa dodatnią wartość false). Krzywych zbliżony przekątnej wykres, w wyniku klasyfikatorami, które nadają się przewidywań znajdujące się blisko odgadnięcie losowe.

###<a name="using-cross-validation"></a>Za pomocą krzyżowe sprawdzanie poprawności###
Jak pokazano w przykładzie regresji oferujemy krzyżowego sprawdzania poprawności wielokrotnie przeszkolenie wynik i automatycznie oceny różnych podzbiorów danych. Podobnie, można użyć [Krzyżowe sprawdzanie poprawności modelu] [ cross-validate-model] modułu, model nieprzeszkolonych regresją i zestaw danych. Kolumna etykieta musi być ustawiona na *dochód* [Krzyżowe sprawdzanie poprawności modelu] [ cross-validate-model] właściwości modułu. Po uruchomiony doświadczenia i klikając port wyjściowy prawo [Krzyżowe sprawdzanie poprawności modelu] [ cross-validate-model] moduł, możemy widać wartości metryki binarne klasyfikacji dla każdego ze zgięciem u dodatkowo średniej i odchylenia standardowego każdego. 
 
![Krzyżowe — sprawdzanie poprawności modelu klasyfikacji binarny](media/machine-learning-evaluate-model-performance/8.png)

Rysunek 8. Krzyżowe — sprawdzanie poprawności modelu klasyfikacji binarne.

![Wyniki sprawdzania poprawności krzyżowe binarne klasyfikatora](media/machine-learning-evaluate-model-performance/9.png)

Rysunek 9. Wyniki sprawdzania poprawności między binarne klasyfikatora.

##<a name="evaluating-a-multiclass-classification-model"></a>Oceny Model klasyfikacji Multiclass##
W tym doświadczeniu użyjemy zawierający wystąpienia 3 różnych rodzajów (grupy) fabryki soczewki zestawu danych(http://archive.ics.uci.edu/ml/datasets/Iris "soczewki") [soczewki]popularne. Istnieje 4 wartości funkcji (sepal długości do szerokości i długości Motyw Płatek do szerokości) dla każdego wystąpienia. W poprzedniej doświadczeniach pracujemy szkolenia i testowanych modeli, korzystając z tym samym zestawów danych. W tym miejscu użyjemy [Podzielone dane] [ split] modułu do utworzenia 2 podzbiorów danych, szkolenie dotyczące pierwszego i wynik i oceny na sekundę. Zestaw danych soczewki jest publicznie [UCI maszynowego uczenia repozytorium](http://archive.ics.uci.edu/ml/index.html)i mogą być pobierane przy użyciu [Importowanie danych] [ import-data] modułu.

###<a name="creating-the-experiment"></a>Tworzenie doświadczenia###
Dodaj następujące moduły do obszaru roboczego w Azure maszynowego uczenia Studio:

- [Importowanie danych][import-data]
- [Multiclass decyzji las][multiclass-decision-forest]
- [Dzielenie danych][split]
- [Model pociągu][train-model]
- [Wynik modelu][score-model]
- [Ocena modelu][evaluate-model]

Połącz porty, jak pokazano poniżej na rysunku 10.

Ustawianie indeksu kolumny etykiety [Modelu pociągu] [ train-model] modułu do 5. Zestaw danych zawiera bez wiersza nagłówka, ale możemy wiedzieć, że etykiety klasy znajdują się w kolumnie piątej.

[Importowanie danych] wybierz polecenie[ import-data] moduł i ustawianie właściwości *źródła danych* , aby *Adres URL sieci Web za pośrednictwem protokołu HTTP*, a adres *URL* do http://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data.

Ustaw część wystąpienia ma być używany dla szkolenia [Podzielone dane] [ split] modułu (0,7 na przykład).
 
![Oceny Multiclass klasyfikatora](media/machine-learning-evaluate-model-performance/10.png)

Rysunek 10. Oceny Multiclass klasyfikatora

###<a name="inspecting-the-evaluation-results"></a>Sprawdzanie wyników oceny###
Uruchom doświadczenia i kliknij port wyjściowy [Oceny]modelu[evaluate-model]. Wyniki obliczeń są prezentowane w postaci macierz zamieszania, w tym przypadku. Macierz zawiera wartości rzeczywiste, a przewidywane wystąpienia dla wszystkich klas 3.
 
![Wyniki obliczeń multiclass klasyfikacji](media/machine-learning-evaluate-model-performance/11.png)

Rysunek 11. Wyniki obliczeń multiclass klasyfikacji.

###<a name="using-cross-validation"></a>Za pomocą krzyżowe sprawdzanie poprawności###
Jak wspomniano wcześniej, można wykonać powtarzających się szkolenia, wyników i ocen automatycznie przy użyciu [Krzyżowe sprawdzanie poprawności modelu] [ cross-validate-model] modułu. Czy potrzebujesz zestawu danych, model nieprzeszkolonych i [Krzyżowe sprawdzanie poprawności modelu] [ cross-validate-model] modułu (patrz rysunek poniżej). Ponownie należy ustawić kolumnę etykieta [Krzyżowe sprawdzanie poprawności modelu] [ cross-validate-model] modułu (indeks kolumny 5 w tym przypadku). Po z doświadczenia i klikając pozycję po prawej stronie wyjściowy port [Krzyżowe sprawdzanie poprawności modelu][cross-validate-model], można sprawdzić metryczne wartości dla każdego ze zgięciem, a także odchylenia standardowego i średniej. Metryki tutaj wyświetlane są podobne do nich omawiane w tym przypadku klasyfikacji binarne. Jednak należy pamiętać, że multiclass klasyfikacji, obliczeniowej wyniki dodatnie i negatywy PRAWDA i FAŁSZ wyniki dodatnie i negatywy następuje zliczania na podstawie na klasy istnieje żadna klasa ogólnej dodatnie lub ujemne. Na przykład podczas obliczania dokładności lub odwołania klasy "Soczewki setosa", przyjmowana jest wartość, to jest dodatnia zajęć i wszystkie pozostałe jako ujemne.
 
![Krzyżowe — sprawdzanie poprawności modelu Multiclass klasyfikacji](media/machine-learning-evaluate-model-performance/12.png)

Rysunek 12. Krzyżowe — sprawdzanie poprawności modelu klasyfikacji Multiclass.


![Wyniki sprawdzania poprawności krzyżowe Model klasyfikacji Multiclass](media/machine-learning-evaluate-model-performance/13.png)

Rysunek 13. Wyniki sprawdzania poprawności krzyżowe Model klasyfikacji Multiclass.


<!-- Module References -->
[cross-validate-model]: https://msdn.microsoft.com/library/azure/75fb875d-6b86-4d46-8bcc-74261ade5826/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[multiclass-decision-forest]: https://msdn.microsoft.com/library/azure/5e70108d-2e44-45d9-86e8-94f37c68fe86/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-logistic-regression]: https://msdn.microsoft.com/library/azure/b0fd7660-eeed-43c5-9487-20d9cc79ed5d/
 
