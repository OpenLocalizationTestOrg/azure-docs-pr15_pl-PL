<properties 
    pageTitle="Korzystanie z linią regresji liniowej w maszynowego uczenia | Microsoft Azure" 
    description="Porównanie modeli regresji liniowej w programach Excel i Azure maszynowego uczenia Studio" 
    metaKeywords="" 
    services="machine-learning" 
    documentationCenter="" 
    authors="garyericson" 
    manager="jhubbard" 
    editor="cgronlun"  />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/09/2016" 
    ms.author="kbaroni;garye" />

# <a name="using-linear-regression-in-azure-machine-learning"></a>W Azure maszynowego uczenia za pomocą regresji liniowej.

> *Kate Baroni* i *Jan Boatman* są przedsiębiorstwa architekci rozwiązania w firmy Microsoft danych wniosków Centrum najlepszych. W tym artykule opisują ich obsługi migracji już istniejącego pakietu analizy regresji na rozwiązanie w chmurze za pomocą Azure maszynowego uczenia.  
 
&nbsp; 
  
[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  
 
## <a name="goal"></a>Cel:

Nasze projektu pracę z dwóch cele pamiętać:  

1. Aby zwiększyć dokładność naszej organizacji miesięczny zysk prognozy za pomocą analizy przewidywanych  
2. Potwierdź, zoptymalizować, zwiększyć prędkość, za pomocą Azure ML i skalę naszych wyników.  

Przykład wielu firmach organizacji przez moduł miesięcznych dochodów procesu prognozowania. Zespół małych analitycy biznesowi została zasięgu zadaniem za pomocą komputera nauki obsługuje ten proces i zwiększyć dokładność prognozy.  Zespół wydatków kilka miesięcy zbieranie danych z wielu źródeł i przecinającymi identyfikowanie atrybutów klucza dotyczącym sprzedaży prognozowania usług analiz statystycznych atrybuty danych.  Następne kroki było rozpocząć tworzenie prototypów statystyczne regresji modele danych w programie Excel.  W ciągu kilku tygodni było modelu regresji programu Excel, który został outperforming bieżącego pola i Finanse procesy prognozowania. To stała się wynik prognozowania według planu bazowego.  


Firma Microsoft następnie następnym krokiem umożliwiły na początku przesunięciu naszych przewidywanych analizy na ml Azure, aby dowiedzieć się, jak można poprawić Azure ML na wydajność przewidywanych.


## <a name="achieving-predictive-performance-parity"></a>Osiągnięcia spójności przewidywanych wydajności

Nasze priorytet było osiągnięcia spójności między modelami regresji Azure ML i Excel.  Podane dokładnie te same dane i ten sam podział dla szkoleń i testowania danych trzeba osiągnięcia spójności przewidywanych wydajności między programem Excel a Azure ML.   Początkowo firma Microsoft nie powiodło się. Model programu Excel outperformed modelu Azure ML.   Błąd został z powodu braku opis ustawienia narzędzie podstawowej w Azure ML. Po synchronizacji zespołowi produktu Azure ML firma Microsoft zdobyte lepszego zrozumienia podstawy ustawienie wymagane dla naszych zestawów danych, a osiągnięcia spójności między dwoma modelami.  

### <a name="create-regression-model-in-excel"></a>Tworzenie modelu regresji w programie Excel
Nasze regresji programu Excel używany model standardowej regresji liniowej znaleziony w dodatku Analysis ToolPak programu Excel. 

Pracujemy obliczana *Średnia bezwzględne % błędu* i używać go jako miary wydajności dla modelu.  Zajęło 3 miesiące w modelu pracy za pomocą programu Excel.  Firma Microsoft przełączyć duża część nauki do doświadczenia Azure ML, która ostatecznie znajdowała się przydatne podczas opis wymagań.

### <a name="create-comparable-experiment-in-azure-machine-learning"></a>Tworzenie porównywalna doświadczenia w nauki komputera Azure  
Firma Microsoft obserwowane poniższe czynności, aby utworzyć naszych doświadczenia w Azure ML:  

1.  Przekazane zestawu danych w pliku csv do Azure ML (plik bardzo mały)
2.  Utworzony eksperyment nowy i [Wybierz kolumny w zestawie danych] [ select-columns] moduł wybrać te same funkcje danych używane w programie Excel   
3.  Używane [Podzielone dane] [ split] moduł ( *Względne wyrażenia* w trybie) do dzielenia danych na dokładnie samej zestawy pociągu, podobnie jak w programie Excel  
4.  Badawcze, mające z [Linią regresji liniowej] [ linear-regression] moduł (tylko opcji domyślnych,) opisane i porównaniu wyników, aby nasze modelu regresji programu Excel

### <a name="review-initial-results"></a>Przeglądanie wyników początkowej
Na początku modelu programu Excel wyraźnie outperformed modelu Azure ML:  

|   |Program Excel|Azure ML|
|---|:---:|:---:|
|Wydajność|   |  |
|<ul style="list-style-type: none;"><li>Dostosowane R-kwadrat</li></ul>| 0.96 |N/D!|
|<ul style="list-style-type: none;"><li>Współczynnik <br />Oznaczanie</li></ul>|N/D!|   0.78<br />(dokładność niski)|
|Oznacza błędu bezwzględnego |  $9. 5M|  $ 19.4 M|
|Oznacza błędu bezwzględnego (%)|   6.03%|  12.2%

Gdy zostało naszych proces i wyniki przez programistów i naukowców danych w zespole Azure ML, szybko udostępniana porady przydatne.  

* Kiedy używać [Regresji liniowej] [ linear-regression] modułu w Azure ML, znajdują się dwie metody:
    *  Online zejścia gradientowego: Mogą być bardziej odpowiednie dla dużą skalę problemów
    *  Zwykłe najmniejszych kwadratów: To metodę, która większość osób traktować po ich usłyszeć regresji liniowej. W małych zestawów danych zwykłej najmniejszych kwadratów może być bardziej odpowiednim wyborem.
*  Należy rozważyć, czy ta parametr grubość uregulowania L2 w celu zwiększenia wydajności. Domyślnie i naszych małego zestawu danych jest ustawiany 0,001, możemy Ustaw 0,005 w celu zwiększenia wydajności.    

### <a name="mystery-solved"></a>Zagadki rozwiązanie!
W przypadku stosowania możemy zalecenia, możemy osiągnięte samej wydajności według planu bazowego w ML Azure jako w programie Excel:   

|| Program Excel|Azure ML (imienia)|Azure ML z najmniejszych kwadratów|
|---|:---:|:---:|:---:|
|Wartość oznaczona etykietą  |Wartości rzeczywiste (liczbowe)|tym samym|tym samym|
|Uczeń  |Excel -> danych analizy -> regresji|Regresji liniowej.|Regresji liniowej.|
|Opcje uczeń|N/D!|Ustawienia domyślne|zwykłe najmniejszych kwadratów<br />L2 = 0,005|
|Zestaw danych|26 wierszy, funkcje 3, 1 etykieta.   Wszystkie liczbowych.|tym samym|tym samym|
|Podziel: pociągu|Program Excel ćwiczenie najpierw 18 wierszy, badany na ostatniej 8 wierszy.|tym samym|tym samym|
|Podziel: Test|Formuła regresji stosowany do ostatniej 8 wierszy w programie Excel|tym samym|tym samym|
|**Wydajność**||||
|Dostosowane R-kwadrat|0.96|N/D!||
|Współczynnik wyznaczania|N/D!|0.78|0.952049|
|Oznacza błędu bezwzględnego |$9. 5M|$ 19.4 M|$9. 5M|
|Oznacza błędu bezwzględnego (%)|<span style="background-color: 00FF00;">6.03%</span>|12.2%|<span style="background-color: 00FF00;">6.03%</span>|

Ponadto współczynników programu Excel w porównaniu do grubości funkcji Azure modelu przeszkolony:

||Współczynniki programu Excel|Funkcja Azure grubości|
|---|:---:|:---:|
|Punkt przecięcia różnica|19470209.88|19328500|
|Funkcja odpowiedzi|0.832653063|0.834156|
|Funkcja B|11071967.08|11007300|
|Funkcja C|25383318.09|25140800|

## <a name="next-steps"></a>Następne kroki

Trzeba używać Azure ML usługi sieci web w programie Excel.  Nasze analitycy biznesowi korzystania z programu Excel i firma Microsoft wymagane sposób wywoływania Azure ML usługi sieci web przy użyciu danych programu Excel w wierszu i zwraca wartość przewidywane do programu Excel.   

Również trzeba zoptymalizować naszego modelu przy użyciu opcji i algorytmy dostępne w Azure ML.

### <a name="integration-with-excel"></a>Integracja z programem Excel
Nasze rozwiązanie było operationalize naszego modelu regresji Azure ML, tworząc usługi sieci web z przeszkolony modelu.  W ciągu kilku minut Usługa sieci web została utworzona i firma Microsoft może wywołać bezpośrednio z programu Excel w celu zwrócenia wartości przewidywanego przychodu.    

Sekcja *Pulpitu nawigacyjnego usług sieci Web* zawiera skoroszytu programu Excel do pobrania.  Skoroszyt zawiera wstępnie sformatowane informacji schematu i interfejsu API usługi sieci web osadzony.   Po kliknięciu *Pobierz skoroszyt programu Excel*zostanie otwarty i zapisaniu go na komputerze lokalnym.    

![][1]
 
Otwórz skoroszyt skopiuj parametry wstępnie zdefiniowanych do sekcji parametr niebieski, tak jak pokazano poniżej.  Parametry są wprowadzone, program Excel połączenia z usługą sieci web AzureML i przewidywane uzyskał etykiety będzie wyświetlany w sekcji zielony przewidywane wartości.  Skoroszyt będzie utworzyć przewidywań parametrów według przeszkolony modelu dla wszystkich elementów wiersza wprowadzone w obszarze Parametry.   Aby uzyskać więcej informacji na temat korzystania z tej funkcji zobacz [przez inne usługi Azure maszynowego uczenia sieci Web z programu Excel](machine-learning-consuming-from-excel.md). 

![][2]
 
### <a name="optimization-and-further-experiments"></a>Optymalizacja i dalsze doświadczenia
Teraz, gdy było planu bazowego z naszego modelu programu Excel, możemy przenoszone wcześniejsze zoptymalizować nasze Azure ML regresji Model liniowy.  Użyliśmy moduł [filtr oparty wybór funkcji] [ filter-based-feature-selection] aby poprawić w naszym zaznaczone dane początkowe elementy i pomógł nam osiągnąć lepszą wydajność 4.6% oznacza błędu bezwzględnego.   W przyszłych projektach użyjemy tej funkcji, które można zapisać nam tygodni w iteracji atrybutów danych, aby znaleźć odpowiedniego zestawu funkcji służących do modelowania.  

Następny planujemy dołączanie dodatkowych algorytmów, takich jak [Bayesian] [ bayesian-linear-regression] lub [Zwiększane algorytmów] [ boosted-decision-tree-regression] w naszym doświadczeniu do porównywania wydajności.    

Jeśli chcesz poeksperymentować z regresji, warto zestawu danych do wypróbowania jest dataset przykładowe regresji efektywności energetycznej zawiera wiele atrybutów liczbowych. Zestaw danych znajduje się w ramach zestawy danych przykładowych w ML Studio.  Za pomocą różnych modułów szkoleniowych przewidywanie ogrzewania ładowania lub chłodzenia ładowania.  Porównanie działania różnych regresji dowiaduje się przed efektywności energetycznej przewidywania zestawu danych dla zmiennej docelowej obciążenia Cooling jest wykres poniżej: 

|Model|Oznacza błędu bezwzględnego|Średnia głównego kwadratu błędu|Względne błędu bezwzględnego|Względna kwadrat błędu|Współczynnik wyznaczania
|---|---|---|---|---|---
|Algorytm zwiększane|0.930113|1.4239|0.106647|0.021662|0.978338
|(Zejścia gradientowego) linią regresji liniowej.|2.035693|2.98006|0.233414|0.094881|0.905119
|Neuronowych regresji|1.548195|2.114617|0.177517|0.047774|0.952226
|(Zwykłych najmniejszych kwadratów) linią regresji liniowej.|1.428273|1.984461|0.163767|0.042074|0.957926  

## <a name="key-takeaways"></a>Takeaways kluczowe 

Opisano wiele przez z regresji uruchomionego programu Excel i Azure maszynowego uczenia doświadczenia równolegle. Tworzenie modelu według planu bazowego w programie Excel i porównanie z modelami przy użyciu Azure ML [Regresji liniowej] [ linear-regression] pomógł nam informacje Azure ML i możemy wykryte szans sprzedaży, aby zwiększyć wydajność zaznaczenia i modelu danych.         

Także znaleźć zaleca umożliwia [Wybór funkcji filtr oparty] [ filter-based-feature-selection] do prognozowania przyszłych projektów.  Stosując wybór funkcji danych, możesz utworzyć model ulepszone w Azure ML z ogólnej wydajności. 

Możliwość przekazywania przewidywanych analityczny Prognozowanie z Azure ML do programu Excel systemically umożliwia znacznego zwiększenia możliwość pomyślnie wyświetlania wyników dla odbiorców użytkownika szeroki firm.     


## <a name="resources"></a>Zasoby
Aby uzyskać pomoc, z którymi współpracujesz regresji znajdują się niektóre zasoby:  

* Regresji w programie Excel.  Jeśli nie zostały wypróbowane regresji w programie Excel, ten samouczek ułatwi: [http://www.excel-easy.com/examples/regression.html](http://www.excel-easy.com/examples/regression.html)
* Regresji w stosunku do prognozowania  Tyler Chessman zapisano artykuł blogu wyjaśnieniem dotyczącym sposobu czasu serii prognozowania w programie Excel zawiera opis początkujących dobre regresji liniowej. [http://sqlmag.com/SQL-Server-Analysis-Services/Understanding-Time-Series-forecasting-Concepts](http://sqlmag.com/sql-server-analysis-services/understanding-time-series-forecasting-concepts)  
*   Zwykłe najmniejszych kwadratów regresji liniowej: Wady, problemów i problemów.  Wprowadzenie i dyskusji regresji: [http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/](http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/ )

[1]: ./media/machine-learning-linear-regression-in-azure/machine-learning-linear-regression-in-azure-1.png
[2]: ./media/machine-learning-linear-regression-in-azure/machine-learning-linear-regression-in-azure-2.png


<!-- Module References -->
[bayesian-linear-regression]: https://msdn.microsoft.com/library/azure/ee12de50-2b34-4145-aec0-23e0485da308/
[boosted-decision-tree-regression]: https://msdn.microsoft.com/library/azure/0207d252-6c41-4c77-84c3-73bdf1ac5960/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
 
