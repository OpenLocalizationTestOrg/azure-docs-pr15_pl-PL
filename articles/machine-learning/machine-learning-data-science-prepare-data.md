<properties
    pageTitle="Zadania, aby przygotować dane dla rozszerzony komputera nauki | Microsoft Azure"
    description="Wstępne przetwarzanie i wyczyścić dane do przygotowania do nauki komputera."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016" 
    ms.author="bradsev" />


# <a name="tasks-to-prepare-data-for-enhanced-machine-learning"></a>Zadania, aby przygotować dane dla rozszerzony komputera szkoleniowe

Wstępne przetwarzanie i oczyszczania danych są ważne zadania, które zwykle należy przeprowadzić przed zestawu danych można skutecznie szkoleniowe na komputerze. Dane często jest naprawienie i mało wiarygodnych i może brakować wartości. Przy użyciu takich danych modelowania może spowodować błąd wyników. Te zadania należą do zespołu danych nauki proces (TDSP) i zazwyczaj postępuj zgodnie początkowej badań umożliwia odnajdowanie i Planowanie wstępne przetwarzanie wymagane zestawu danych. Aby uzyskać bardziej szczegółowe instrukcje dotyczące procesu TDSP Zobacz z instrukcjami podanymi w [Proces nauki danych zespołu](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

Wstępne przetwarzanie i usuwania zadań, takich jak zadania badań danych, można prowadzić w wielu różnych środowiskach, takich jak SQL lub gałęzi lub Azure maszynowego uczenia Studio i różne narzędzia i językach, takich jak R lub Python, zależności, miejsce, w którym dane są przechowywane i sposobu formatowania. Ponieważ TDSP jest iteracyjne charakter, te zadania może mieć miejsce na różne kroki procesu przepływu pracy.

W tym artykule wprowadza różne pojęcia przetwarzania danych i zadań, które można podjąć przed lub po ingesting danych do nauki maszynowego Azure.

Na przykład badań danych oraz wstępne przetwarzanie gotowe wewnątrz studio Azure maszynowego uczenia zobacz [wstępne przetwarzanie danych w Azure maszynowego uczenia Studio](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/) wideo.


## <a name="why-pre-process-and-clean-data"></a>Dlaczego wstępnie przetwarzania i Oczyszczanie danych?

Zebrane rzeczywistych danych pochodzących z różnych źródeł i procesów i mogą zawierać nieprawidłowości lub uszkodzone dane utraty jakości zestawu danych. Problemy z jakością typowych danych występujących są:

* **Nieukończone**: danych brakuje atrybuty lub zawierające brakujące wartości.
* **Noisy**: dane zawierają błędnych rekordów lub odstające.
* **Niespójne**: dane zawierają rekordy powodujące konflikt lub rozbieżności.

Danych dotyczących jakości jest wymagane w przypadku modeli przewidywanych jakości. Aby uniknąć "śmieci w śmieci limit" i poprawić jakość danych i w związku z tym modelu wydajności, konieczne jest przeprowadzenie ekran kondycji danych do wcześniej dodatkowych problemów z danymi ale okazało się na odpowiadające im przetwarzania danych i czyszczenia czynności.

## <a name="what-are-some-typical-data-health-screens-that-are-employed"></a>Jakie są niektóre ekrany kondycji typowych danych, które są wykorzystywane?

Firma Microsoft sprawdzić ogólnej jakości danych, zaznaczając pole wyboru:

* Liczba **rekordów**.
* Liczba **(lub **funkcji**)** .
* Atrybut **typy danych** (nominalna, liczba porządkowa lub ciągły).
* Liczba **brakujących wartości**.
* **Formedness źródło** danych.
    * Jeśli dane znajdują się w TSV lub CSV, sprawdź, czy separatory wiersza i kolumny separatory zawsze poprawnie oddzielić kolumn i wierszy.
    * Jeśli dane są w formacie HTML lub XML, sprawdź, czy dane również powstaje oparte na ich odpowiednich standardy.
    * Analizowanie również może być potrzebne do wyodrębniania półstrukturalnych lub niestrukturalne danych strukturalnych informacji.
* **Rekordy niespójne dane**. Zaznacz zakres wartości są dozwolone. np. Jeśli dane zawierają specjalizacji uczniów, sprawdzić, czy specjalizacji znajduje się w zakresie wyznaczonych, na przykład 0 ~ 4.

Gdy znajdziesz problemy z danymi, **przetwarzania czynności** są konieczne które często obejmuje czyszczenia brakujące wartości, normalizacji danych, discretization, tekst przetwarzania, aby usunąć i/lub zastąpić znaki osadzony, które mogą mieć wpływ na wyrównanie danych, mieszane dane wspólne typy pól i innych osób.

**Azure maszynowego uczenia używa poprawnego danych tabeli**.  Jeśli dane znajdują się już w formie tabelarycznej, wstępne przetwarzanie danych mogą być wykonywane bezpośrednio z Azure maszynowego nauki w Studio nauki komputera.  Jeśli dane nie znajdują się w formie tabelarycznej, powiedz, który jest w języku XML, może być konieczne, aby można było przekonwertować dane w formie tabelarycznej analizy.  

## <a name="what-are-some-of-the-major-tasks-in-data-pre-processing"></a>Jakie są niektóre najważniejsze zadania wstępne przetwarzanie danych?

* **Czyszczenie danych**: wypełnić lub brakujące wartości wykrywanie i usuwanie zakłócenia danych i wartości odstających.
* **Przekształcanie**: normalizowanie danych, aby zmniejszyć wymiary i hałasu.
* **Zmniejszenie danych**: przykładowe rekordy danych lub atrybuty łatwiejsze przetwarzania danych.
* **Discretization danych**: konwertowanie ciągły atrybuty atrybuty kategorii w celu ułatwienia pracy z niektórych metod nauki komputera.
* **Czyszczenie tekstu**: usuwanie osadzony znaki, które mogą spowodować niezgodność danych dla np karty osadzony w pliku danych tabulatorami osadzony nowych wierszy, które mogą spowodować przerwanie rekordów itp.

Poniższych sekcjach opisano niektóre z tych kroków przetwarzania danych.

## <a name="how-to-deal-with-missing-values"></a>Jak radzić sobie z brakujące wartości?

Przejrzane brakujące wartości, warto najpierw zidentyfikować powód brakujące wartości, aby lepiej uchwyt problem. Typowe metody brak obsługi wartość są:

* **Usuwanie**: usuwanie rekordów z brakujące wartości
* **Podstawianie manekina**: Zamień brakujące wartości z wartością fikcyjna: np., _Nieznany_ dla kategorii lub 0 dla wartości liczbowych.
* **Oznacza podstawiania**: Jeśli brakujących danych jest liczbowe, Zamień brakujące wartości średniej.
* **Podstawianie częste**: w przypadku kategorii brakujących danych Zamień brakujące wartości najczęściej elementu
* **Podstawianie regresji**: Zamień brakujące wartości uwzględniona wartości za pomocą metodą regresji.  

## <a name="how-to-normalize-data"></a>Jak normalizowanie danych?

Normalizacji danych skale ponownie wartości liczbowe do określonego zakresu. Metody normalizacji danych popularne obejmują:

* **Min maks normalizacji**: liniowo przekształcania danych do zakresu, na przykład od 0 do 1, gdzie wartość minimalna jest skalowany wartość maksymalna liczba od 0 do 1.
* **Wynik Z normalizacji**: przeskalowania danych na podstawie wartości średniej i odchylenia standardowego: Podziel różnicę między dane i średnie odchylenie standardowe.
* **Dziesiętna skalowania**: skalowanie danych przez przeniesienie punktu dziesiętnego wartość atrybutu.  

## <a name="how-to-discretize-data"></a>Jak discretize danych?

Dane można discretized konwertując ciągły wartości nominalnej atrybuty lub interwałów. Przedstawiono kilka sposobów w ten sposób:

* **Szerokość równości Binning**: dzielenie zakres wszystkich możliwych wartości atrybutu do grup N o tym samym rozmiarze, a także przydzielanie wartościami, które wykraczają się w Koszu z numerem Kosz.
* **Wysokość równości Binning**: dzielenie zakres wszystkich możliwych wartości atrybutu do grup N, każdy zawiera taką samą liczbę wystąpień, a następnie przypisz wartości, które wykraczają się w Koszu z numerem Kosz.  

## <a name="how-to-reduce-data"></a>Jak zmniejszyć danych?

Istnieją różne metody, aby zmniejszyć rozmiar danych dla danych łatwiejsze obsługi. W zależności od rozmiaru danych i domeny można zastosować następujące metody:

* **Rekord przy próbkowaniu**: przykładowe rekordy i wybrać tylko przedstawiciela podzestawu danych.
* **Atrybut próbek**: zaznacz tylko niektóre atrybuty najważniejszych danych.  
* **Agregacji**: dzielenie danych do grup i przechowywania numerów dla każdej grupy. Na przykład dzienny liczby zysk łańcucha restauracji w ciągu ostatnich 20 lat mogą zostać zsumowane z miesięczne przychody w celu zmniejszenia rozmiaru danych.  

## <a name="how-to-clean-text-data"></a>Jak wyczyścić dane tekstowe?

**Pola tekstowe w dane tabelaryczne** mogą zawierać znaki, które wpływają na granic kolumn wyrównanie i/lub rekordu. Np osadzony karty w niezgodność kolumny Przyczyna rozdzielanymi tabulatorami i osadzone znaki nowego wiersza podziału wierszy rekordu. Obsługa kodowania niewłaściwy tekst podczas pisania i odczytu tekstu prowadzi do utracie danych, przypadkowe wprowadzenie nieczytelny znaków, np wartości null i może również tekst wpływ analizy. Staranne analizowania i edytowania może być konieczne, aby oczyścić pól tekstowych do wyrównania pisane z wielkiej litery i/lub Wyodrębnij strukturę danych z danych tekstowych niestrukturalne lub półstrukturalnych.

**Poszukiwanie danych** udostępnia najwcześniejsze widok do danych. Wiele problemów z danymi może być niepokrytych w tym kroku i odpowiadające im metody mogą być stosowane do rozwiązania tych problemów.  Należy zadawać pytania, takie jak co to jest źródło problemu oraz jak ten problem może zostały wprowadzone. Pomaga to także zadecydować o czynności przetwarzania danych, które należy podjąć w celu ich rozwiązania. Rodzaj wniosków, jakie jedną zamierza dziedziczyć dane można także Wyznaczanie priorytetów nakładu przetwarzania danych.

## <a name="references"></a>Odwołania

>*Wyszukiwania danych: koncepcji i technik*, wydanie trzecie, Morgan Kaufmann 2011 Hanowi Jiawei, Micheline Kamber i Jian Pei
