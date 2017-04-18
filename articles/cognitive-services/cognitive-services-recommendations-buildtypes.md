<properties
    pageTitle="Tworzenie typów i jakość modelu: zalecenia interfejsu API | Microsoft Azure"
    description="Zalecenia Azure nauki komputera — Przewodnik Szybki start"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="luisca"/>

#  <a name="build-types-and-model-quality"></a>Tworzenie typów i jakość modelu #

<a name="TypeofBuilds"></a>
## <a name="supported-build-types"></a>Typy obsługiwanego kompilacji ##

Zalecenia dotyczące interfejsu API obsługuje obecnie dwa typy kompilacji: *zalecenie* i *zmianie wysokości progów*. Każdy jest wbudowany różne algorytmy i każda ma inny poziom. W tym dokumencie opisano każdej z tych wersjach i techniki umożliwiające porównanie jakości modeli wygenerowane.

Jeśli jeszcze nie tego już, zalecamy Kończenie [Przewodnik Szybki start](cognitive-services-recommendations-quick-start.md).

<a name="RecommendationBuild"></a>
### <a name="recommendation-build-type"></a>Typ kompilacji rekomendacji ###

Typ kompilacji rekomendacji używa factorization macierzy w celu włączenia zalecenia. Generuje wektorów [ukryty funkcji](https://en.wikipedia.org/wiki/Latent_variable) oparte na swoje transakcje w celu opisania poszczególnych elementów, a następnie za pomocą tych ukryty wektorów porównanie elementy, które są podobne.

Jeśli przeszkolenie model oparty na zakupy w sklepie electronics i podaj telefonu Lumia 650 jako dane wejściowe do modelu modelu zwróci zestaw elementów, które zwykle do zakupu przez osoby, które mogą zakupić telefonu Lumia 650. Elementy mogą nie być uzupełniające. W tym przykładzie jest możliwe, że modelu zwróci inne telefony, ponieważ osoby, które Lumia 650, takich jak może, takich jak inne telefony.

Tworzenie rekomendacji występują dwie funkcje ułatwiające atrakcyjnego:

* *Tworzenie rekomendacji obsługuje *zimnej element* położenie **

Elementy, które nie mają zastosowania istotne są nazywane zimnej elementów. Na przykład jeśli zostanie wyświetlony wysyłek telefonu, którego możesz nigdy nie zostały sprzedane przed, system nie można określić zalecenia dotyczące tego produktu transakcji tylko. Oznacza to, że system warto zapoznać z informacji na temat samego produktu.

Jeśli chcesz użyć położenie zimnej elementu, musisz funkcje informacje dotyczące poszczególnych elementów w wykazie. Oto, co kilka pierwszych linii z katalogu może wyglądać tak (Uwaga klucz = wartość format dla funkcji).

>Pro2 6CX 00001, powierzchniowy, powierzchnia, wpisz = sprzęt, miejsca do magazynowania = 128 GB pamięci = 4G, producenta = Microsoft

>00013 73H, wznawianie konsoli Xbox 360, gier,, typ = oprogramowanie, język angielski, ocena = = dojrzałych

>WAH 0F05, Minecraft konsoli Xbox 360, gier,, * typ = oprogramowanie, języka = hiszpański, ocena = młodzieżowej

Trzeba również ustawić następujące parametry kompilacja:

| Tworzenie parametru         | Notatki
|------------------     |-----------
|*useFeaturesInModel*     | Ustaw **wartość true**.  Wskazuje, jeśli można korzystać z funkcji ulepszanie modelu rekomendacji.
|*allowColdItemPlacement*   | Ustaw **wartość true**. Wskazuje, jeśli zalecenie należy również push zimnej elementów za pomocą funkcji podobieństwa.
| *modelingFeatureList*   | Rozdzielaną średnikami listę nazw funkcji może być używany w oknie Tworzenie rekomendacji ulepszanie zalecenia. Na przykład "języka, miejsca do magazynowania" dla poprzedniego przykładu.

**Tworzenie rekomendacji obsługuje zalecenia użytkownika**

Tworzenie rekomendacji obsługuje [zalecenia użytkownika](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3dd). Oznacza to, że oferuje spersonalizowanych zalecenia dla użytkowników według ich historie transakcji. Aby uzyskać zalecenia użytkownika może zawierać nazwy użytkownika i historii ostatnich transakcji dla tego użytkownika.

Jeden klasyczny przykład miejsce, w którym można zastosować zalecenia użytkownika znajduje się na logowania na stronie powitalnej. Można awansować zawartości, która ma zastosowanie do określonego użytkownika.

Można również zastosować typu konstruowania zaleceń, gdy użytkownik ma zapoznaj się z. W tym momencie użytkownik ma listę elementów, które użytkownik ma zakupu i umożliwiają rekomendacje oparte na bieżący koszyk rynku.

#### <a name="recommendations-build-parameters"></a>Zalecenia dotyczące tworzenia parametrów

| Nazwa  |   Opis |    Typ <br>  Prawidłowe wartości <br> (wartość domyślna)
|-------|-------------------|------------------
| *NumberOfModelIterations* |   Liczba iteracji, które wykonuje modelu jest uwzględniany przez całkowity czas obliczeń i dokładności modelu. Im większa liczba, tym dokładniejszy modelu, ale podczas uruchamiania trwa dłużej.  |   Liczba całkowita <br>  10-50 <br>(40)
| *NumberOfModelDimensions* |   Liczba wymiarów odnosi się do wiele funkcji, które będzie próbował znaleźć w danych modelu. Zwiększenie liczby wymiary, aby umożliwić lepiej dostosowywania wyników do mniejszych klastrów. Jednak za dużo wymiarów uniemożliwi modelu odnalezienie korelacji między elementami. |  Liczba całkowita <br> 10-40 <br>(20) |
| *ItemCutOffLowerBound* |  Określa minimalną liczbę punktów zastosowania elementu powinien być na dla niego mają być traktowane jako część modelu. |        Liczba całkowita <br> 2 lub więcej <br> (2) |
| *ItemCutOffUpperBound* |  Określa maksymalną liczbę punktów zastosowania, które elementy powinny być w dla niego mają być traktowane jako część modelu. |  Liczba całkowita <br>2 lub więcej<br> (2147483647) |
|*UserCutOffLowerBound* |   Określa minimalną liczbę transakcji, które mają być traktowane jako części modelu musiał wykonywać użytkownika. | Liczba całkowita <br> 2 lub więcej <br> (2)
| *UserCutOffUpperBound* |  Określa maksymalną liczbę transakcji, które mają być traktowane jako części modelu musiał wykonywać użytkownika. | Liczba całkowita <br>2 lub więcej <br> (2147483647)|
| *UseFeaturesInModel* |    Wskazuje, jeśli można korzystać z funkcji ulepszanie modelu rekomendacji. |     Wartość logiczna<br> Domyślne: PRAWDA
|*ModelingFeatureList* |    Rozdzielaną średnikami listę nazw funkcji może być używany w oknie Tworzenie rekomendacji ulepszanie zalecenia. To zależy od funkcji, które są ważne. |    Ciąg maksymalnie 512 znaków
| *AllowColdItemPlacement* |    Wskazuje, jeśli zalecenie należy również push zimnej elementów za pomocą funkcji podobieństwa. | Wartość logiczna <br> Domyślne: FAŁSZ
| *EnableFeatureCorrelation*    | Wskazuje, jeśli funkcje mogą być używane w uzasadnienie. | Wartość logiczna <br> Domyślne: FAŁSZ
| *ReasoningFeatureList* |  Rozdzielaną średnikami listę nazw funkcji ma być używany dla uzasadnienie zdań, takich jak objaśnienia rekomendacji. To zależy od funkcji, które są ważne dla klientów. | Ciąg maksymalnie 512 znaków
| *EnableU2I* | Włącz spersonalizowane rekomendacje, nazywany użytkownikowi element zalecenia (U2I). | Wartość logiczna <br>Domyślne: PRAWDA
|*EnableModelingInsights* | Określa, czy offline oceny należy przeprowadzić uzyskanie wniosków modelowania (oznacza to, że dokładność i różnorodnością metryki). Jeśli wartość true, podzestawu danych nie będą używane do szkoleń, ponieważ konieczne będzie można zarezerwować testowania modelu. Dowiedz się więcej o [ocen w trybie offline](#OfflineEvaluation). | Wartość logiczna <br> Domyślne: FAŁSZ
| *SplitterStrategy* | Włącz modelowania wniosków jest ustawiona na *wartość PRAWDA*, to jest jak dane powinny być podzielone na potrzeby obliczeń.  | Ciąg, *RandomSplitter* lub *LastEventSplitter* <br>Domyślne: RandomSplitter


<a name="FBTBuild"></a>
### <a name="fbt-build-type"></a>Typ kompilacji zmianie wysokości progów ###

Często zakupionych razem Konstruuj (zmianie wysokości progów) oznacza analizę, która oblicza liczbę powtórzeń dwóch lub trzech różnych produktów Współtworzenie występują razem. Następnie sortuje zestawy na podstawie funkcji podobieństwa (**współpracujących wystąpienia**, **Jaccard** **Podnieś**).

Traktować **Jaccard** i **Podnieś** jako sposoby znormalizować Współtworzenie wystąpień.  Oznacza to, że elementy zostaną zwrócone tylko wtedy, gdy ich w przypadku, gdy zakupiony razem z elementu nasion.

W naszym przykładzie telefonu Lumia 650 telefonu X zostaną zwrócone tylko wtedy, gdy telefon X został zakupiony w tej samej sesji jako telefon Lumia 650. Ponieważ może to być prawdopodobne, możemy oczekiwaniami elementów uzupełniające do 650 Lumia mają zostać zwrócone; na przykład ochrony ekranu lub karty power dla Lumia 650.

Obecnie dwa elementy są przyjmuje się, że można kupić w tej samej sesji, jeżeli występują w transakcja z tej samej nazwy użytkownika i sygnatura czasowa.

Kompilacjach zmianie wysokości progów nie obsługują zimnej elementów, ponieważ zgodnie z definicją spodziewają dwa elementy, które można kupić w tej samej transakcji. Gdy kompilacjach zmianie wysokości progów może zwracać zestawów elementów (triplets), nie obsługują spersonalizowane rekomendacje ponieważ akceptują elementu nasion pojedynczego jako danych wejściowych.


#### <a name="fbt-build-parameters"></a>Parametry kompilacji zmianie wysokości progów

| Nazwa  |   Opis |       Typ  <br> Prawidłowe wartości <br> (wartość domyślna)
|-------|---------------|-----------------------
| *FbtSupportThreshold* | Jak konserwatywnego model jest. Liczba wystąpień Współtworzenie elementów ma zostać uwzględniony w modelowania. |  Liczba całkowita <br> 3-50 <br> (6)
| *FbtMaxItemSetSize* | Bounds liczbę elementów w zestawie częste.| Liczba całkowita  <br> 2-3 <br> (2)
| *FbtMinimalScore* | Wynik minimalnego zestawu częstych powinien mieć mają zostać uwzględnione na liście zwróconych wyników. Wyższa lepiej. | Naciśnij dwukrotnie <br> 0 i powyżej <br> (0)
| *FbtSimilarityFunction* | Definiuje funkcję podobieństwa może być używany we kompilacji. **Podnieś** ma serendipity, **współpracujących wystąpienie** ma przewidywalności i **Jaccard** jest złamania między nimi. | Ciąg znaków,  <br>  <i>cooccurrence dźwigu, jaccard</i><br> Wartość domyślna: <i>jaccard</i>

<a name="SelectBuild"></a>
## <a name="build-evaluation-and-selection"></a>Tworzenie obliczeń i zaznaczenia ##

Te wskazówki mogą pomóc określić, czy należy używać kompilacji zalecenia lub tworzenie zmianie wysokości progów, ale nie zawiera ostateczną odpowiedź w przypadkach, gdzie można użyć jednej z nich. Ponadto nawet wtedy, gdy wiesz, że chcesz użyć typu kompilacji zmianie wysokości progów, może być nadal chcesz wybrać **Jaccard** lub **Podnieś** jako funkcja podobieństwa.

Najlepszym sposobem na Wybieranie między dwiema wersjami różnych jest je przetestować w świecie rzeczywistym (ocena online) i śledzenie stawki konwersji dla różnych wersjach. Kurs wymiany może być mierzone oparte na kliknięć rekomendacji, liczba rzeczywista zakupów z zalecenia wyświetlane lub nawet kwoty rzeczywiste zakupu, gdy były wyświetlane różne zalecenia. Możesz wybrać usługi metryki stopa konwersji według do celów biznesowych.

W niektórych przypadkach może być ma być obliczona modelu w trybie offline, zanim zostanie umieszczone w produkcji. Oceny w trybie offline nie jest zastąpienie oceny online, może służyć jako metryki.

<a name="OfflineEvaluation"></a>
## <a name="offline-evaluation"></a>Oceny w trybie offline  ##

Celem oceny offline jest przewidywanie dokładność (liczba użytkowników, którzy będą kupić zalecane elementy) i różnorodnością zaleceń (liczba elementów, które są zalecane).
Jako część obliczeń metryki dokładność i różnorodnością, system umożliwia znalezienie wyrazów próbki użytkowników i dzieli transakcje dla tych użytkowników w dwóch grupach: szkolenie zestawu danych i test dataset.

> [AZURE.NOTE]Aby użyć trybu offline metryki, musisz mieć sygnatury czasowe danych użycia.
> Dane dotyczące czasu jest wymagane do dzielenie zastosowania poprawnie między badania i szkolenia zestawy danych.

> Ponadto oceny w trybie offline nie może być rentowność wyniki zastosowania małych plików. Oceny być dokładne powinien mieć co najmniej 1000 punktów zastosowania w zestawie danych test.

<a name="Precision"></a>
### <a name="precision-at-k"></a>Dokładność w k ###
Poniższa tabela przedstawia wynik oceny offline dokładności u k.

| K | 1 | 2 | 3 |   4 |     5
|---|---|---|---|---|---|
|Wartość procentową |   13.75 | 18.04   | 21 |  24.31 | 26.61
|Użytkowników testowych |    10 000 |    10 000 |    10 000 |    10 000 |    10 000
|Użytkownicy traktowane jako | 10 000 |    10 000 |    10 000 |    10 000 |    10 000
|Użytkownicy nie są uwzględniane | 0 | 0 | 0 | 0 | 0

#### <a name="k"></a>K
W powyższej tabeli *k* reprezentuje liczbę zalecenia pokazano klienta. Tabela o następującej treści: "Jeśli podczas okresu próbnego tylko jedno zalecenie był widoczny odbiorcom, tylko 13.75 użytkowników, czy zostały kupione tego zalecenia." Zasad zachowania opiera się na założeniu, że model został szkolenia z danymi zakupu. Innym sposobem na to, że jest dokładność od 1 13.75.

Można zauważyć, że przedstawione więcej elementów do klienta prawdopodobieństwa klienta zakupu zalecane elementu rośnie. Do poprzedniego doświadczenia prawdopodobieństwo niemal zwiększa dwukrotnie procent 26.61 po zaleca się 5 elementów.

#### <a name="percentage"></a>Wartość procentową
Procent użytkowników, których interakcji z co najmniej jeden z zaleceniami *k* jest wyświetlana. Dzieląc liczbę użytkowników, których interakcji z co najmniej jedno zalecenie przez całkowitą liczbę użytkowników traktowane jako jest obliczana wartość procentowa. Zobacz użytkowników traktowane jako, aby uzyskać więcej informacji.

#### <a name="users-in-test"></a>Użytkowników testowych
Dane w tym wierszu reprezentuje całkowita liczba użytkowników w zestawie danych test.

#### <a name="users-considered"></a>Użytkownicy traktowane jako
Użytkownik jest traktowany jako tylko jeśli system zalecane co najmniej *k* elementy na podstawie modelu wygenerowanych przy użyciu zestawu danych szkoleniowe.

#### <a name="users-not-considered"></a>Użytkownicy nie są uwzględniane
Dane w tym wierszu reprezentuje wszyscy użytkownicy nie są uwzględniane. Użytkownicy, którzy nie dotarła co najmniej *k* polecane elementów.

Użytkownik nie są uwzględniane = użytkowników w przetestuj — traktowane jako użytkowników

<a name="Diversity"></a>
### <a name="diversity"></a>Różnorodnością ###
Metryki różnorodnością Zmierz zalecane elementy tego typu. Poniższa tabela przedstawia wynik oceny offline różnorodnością.

|Percentyl kolorem |    0-o 90 stopni.|  99 o 90 stopni.| 99-100
|------------------|--------|-------|---------
|Wartość procentową        | 34.258 | 55.127| 10.615


Suma elementów zalecane: 100 000

Unikatowych elementów zalecane: 954

#### <a name="percentile-buckets"></a>Pakiety percentyl
Każdy kolorem percentyl jest reprezentowany przez przedziale (wartości minimalne i maksymalne wartości z zakresu od 0 do 100). Elementy zbliżony 100 najpopularniejszych elementów i elementów zbliżony 0 jest najmniej popularne. Na przykład jeśli wartość procentową kolorem percentyl 99 100 jest 10.6, oznacza to, że wykonano 10.6 zaleceń zwrócone tylko górny jednego procenta najpopularniejsze elementy. Kolorem minimalną wartość ma charakter, a wartość maksymalna jest zastrzeżona, z wyjątkiem 100.
#### <a name="unique-items-recommended"></a>Unikatowych elementów zalecane
Unikatowych elementów polecane metryki pokazuje liczbę różnych elementów zwróconych oceny.
#### <a name="total-items-recommended"></a>Całkowita liczba elementów polecane
Całkowita liczba elementów polecane, że metryka pokazuje liczbę elementów polecane. Niektóre mogą być duplikaty.

<a name="ImplementingEvaluation"></a>
### <a name="offline-evaluation-metrics"></a>Metryki oceny w trybie offline ###
Dokładność i różnorodnością metryk offline może być przydatne po wybraniu za pomocą których tworzenie. W czasie kompilacji jako część odpowiednich zmianie wysokości progów lub zalecenie skonstruować parametrów:

-   Ustawianie parametrów kompilacji *enableModelingInsights* na **wartość true**.
-   Opcjonalnie wybierz *splitterStrategy* (albo *RandomSplitter* lub *LastEventSplitter*).
*RandomSplitter* dzieli dane użycia w pociągu i test zestawów oparte na procent test danej *randomSplitterParameters* i losowe zapełnić wartości.
*LastEventSplitter* dzieli dane użycia w pociągu i przetestować zestawy oparte na ostatniej transakcji dla każdego użytkownika.

Spowoduje to kompilacja korzysta tylko podzestawu danych szkoleń i używa pozostałe dane do obliczenia metryki oceny.  Po zakończeniu kompilacji można uzyskać wydruk oceny, należy zadzwonić [interfejsu API metryk kompilacji Get](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/577eaa75eda565095421666f), przekazując odpowiednich *modelId* i *buildId*.

 Oto wynik JSON oceny próbki.


    {
     "Result": {
     "precisionItemRecommend": null,
     "precisionUserRecommend": {
      "precisionMetrics": [
        {
          "k": 1,
          "percentage": 13.75,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 2,
          "percentage": 18.04,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 3,
          "percentage": 21.0,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 4,
          "percentage": 24.31,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 5,
          "percentage": 26.61,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        }
      ],
      "error": null
    },
    "diversityItemRecommend": null,
    "diversityUserRecommend": {
      "percentileBuckets": [
        {
          "min": 0,
          "max": 90,
          "percentage": 34.258
        },
        {
          "min": 90,
          "max": 99,
          "percentage": 55.127
        },
        {
          "min": 99,
          "max": 100,
          "percentage": 10.615
        }
      ],
      "totalItemsRecommended": 100000,
      "uniqueItemsRecommended": 954,
      "uniqueItemsInTrainSet": null,
      "error": null
      }
     },
    "Id": 1,
    "Exception": null,
    "Status": 5,
    "IsCanceled": false,
    "IsCompleted": true,
    "CreationOptions": 0,
    "AsyncState": null,
    "IsFaulted": false
    }
