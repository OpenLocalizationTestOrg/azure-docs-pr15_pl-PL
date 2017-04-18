<properties 
    pageTitle="Aplikacja nauki komputera: usługa wykrywania anomalii | Microsoft Azure" 
    description="Interfejs API wykrywania anomalii jest na przykład utworzona za pomocą Microsoft Azure maszynowego Learning wykrywa różnic w odniesieniu w czasie serii danych za pomocą wartości liczbowe, które są równomiernie rozmieszczone w czasie." 
    services="machine-learning" 
    documentationCenter="" 
    authors="alokkirpal" 
    manager="jhubbard"
    editor="cgronlun" /> 

<tags 
    ms.service="machine-learning" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="multiple" 
    ms.date="10/11/2016" 
    ms.author="alokkirpal"/>


# <a name="machine-learning-anomaly-detection-service"></a>Usługi wykrywania anomalii szkoleniowe#

##<a name="overview"></a>Omówienie

[Interfejs API wykrywania anomalii](https://datamarket.azure.com/dataset/aml_labs/anomalydetection) jest na przykład skonstruowane za pomocą nauki komputera Azure, który wykrywa różnic w odniesieniu w czasie serii danych za pomocą wartości liczbowe, które są równomiernie rozmieszczone w czasie. 

Ten interfejs API może wykryć następujących typów anomalous wzorców w czasie serie danych:

* **Dodatnie i ujemne trendów**: na przykład gdy monitorowania użycie pamięci w przypadku komputerów trendów mogą być zainteresowania może być wskazujące przeciek pamięci

* **Zmiany w dynamiczny zakres wartości**: na przykład podczas monitorowania wyjątki generowane przez usługi w chmurze, wszelkie zmiany w dynamiczny zakres wartości można wskazać niestabilność kondycji usługi, i

* **Wzrósł i spadnie**: na przykład podczas monitorowania liczba błędów logowania w usłudze lub liczba wyewidencjonowania w witrynie elektronicznego, tych najwyższych wartościach lub spadku może oznaczać nietypowego działania.

Te wykrywacze nauki komputera Śledzenie takie zmiany wartości przez godzinę i raport trwających zmian w ich wartości jako wyniki anomalii. Nie wymaga Dostosowywanie próg ad hoc i wyniki umożliwiają sterowanie FAŁSZ dodatniej stawki. Wykrywania anomalii interfejsu API jest przydatna w kilka scenariuszy, takich jak monitorowanie usługi przez śledzenie wskaźniki KPI z czasem zastosowania monitorowania za pośrednictwem metryki, takie jak Liczba wyszukiwań, liczbę kliknięć, monitorowanie wydajności za pośrednictwem liczników, takich jak pamięć, CPU, odczytuje plik itp w czasie.

Oferta wykrywania anomalii dostępne przydatne narzędzia ułatwiające rozpoczęcie pracy. 

* [Aplikacja sieci web](http://anomalydetection-aml.azurewebsites.net/) ułatwia ocenić i wizualizowanie wyniki wykrywania anomalii interfejsy API na danych. 

* [Przykładowy kod](http://adresultparser.codeplex.com/) pokazano, jak programowy dostęp do API i przeanalizować wyniki w C#.

>[AZURE.NOTE]
>Wypróbuj **rozwiązania informatycznego anomalii wniosków** obsługiwane przez [Ten interfejs API](https://datamarket.azure.com/dataset/aml_labs/anomalydetection)
>
>Aby uzyskać ten kompleksowe rozwiązania używany do subskrypcji usługi Azure <a href="https://gallery.cortanaintelligence.com/Solution/Anomaly-Detection-Pre-Configured-Solution-1" target="_blank"> **Rozpocznij tutaj >**</a>


##<a name="api-definition"></a>Definicja interfejsu API

Udostępnia interfejsu API opartego na pozostałych za pomocą protokołu HTTPS, która może być używana na różne sposoby, łącznie z sieci web lub w aplikacji mobilnej, R, Python, Excel itp.  Wyślij czasu serii danych do tej usługi za pośrednictwem połączenia interfejsu API usługi REST, a wykonywania kombinacją trzech anomalii typu opisanego powyżej. Usługa działa na platformie Azure maszynowego uczenia, które bezproblemowo skale do potrzeb firmy, a także zwiększany.

###<a name="headers"></a>Nagłówki
Upewnij się, uwzględnić poprawne nagłówków w wezwaniu powinna wyglądać następująco:

    Authorization: Basic <creds>
    Accept: application/json
               
    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

Możesz znaleźć swój klucz konta z konta usługi [Azure Data Market](https://datamarket.azure.com/account/keys). 

###<a name="score-api"></a>Interfejs API wynik

Interfejs API wynik jest używana do uruchamiania wykrywania anomalii na czas — sezonowy serii danych. Interfejs API wykonuje szereg wykrywacze anomalii w danych i zwraca wyniki anomalii. Na poniższym rysunku przedstawiono przykład różnic w odniesieniu, które może wykryć interfejs API wynik. Ten szeregu czasowego ma 2 zmiany na poziomie odrębnych i 3 tych najwyższych wartościach. Czerwone kropki Pokaż czas, w którym zostanie wykryta zmiany poziomu, gdy czarne kropki Pokaż wykryto tych najwyższych wartościach.

![Interfejs API wynik][1]
    
**ADRES URL**

    https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/Score 

**Przykład treści żądania**

W wezwaniu na poniżej wyślemy do szeregu czasowego (jak pokazano obcięte) z punktów danych z 3-1-2016 za pośrednictwem 2016-3-10 i parametrów wykrywacze kolekcji API wykrywania, jeśli dowolny z tych punktów danych jest anomalous. 

    {
      "data": [
        [ "9/21/2014 11:05:00 AM", "3" ],
        [ "9/21/2014 11:10:00 AM", "9.09" ],
        [ "9/21/2014 11:15:00 AM", "0" ]
      ],
      "params": {
        "tspikedetector.sensitivity": "3",
        "zspikedetector.sensitivity": "3",
        "trenddetector.sensitivity": "3.25",
        "bileveldetector.sensitivity": "3.25",
        "postprocess.tailRows": "2"
      }
    }

Odpowiedzi na ten będzie: 

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/$metadata#AnomalyDetection.FrontEndService.Models.AnomalyDetectionResult",
      "ADOutput":"{
        "ColumnNames":["Time","Data","TSpike","ZSpike","rpscore","rpalert","tscore","talert"],
        "ColumnTypes":["DateTime","Double","Double","Double","Double","Int32","Double","Int32"],
        "Values":[
          ["9/21/2014 11:10:00 AM","9.09","0","0","-1.07030497733224","0","-0.884548154298423","0"],
          ["9/21/2014 11:15:00 AM","0","0","0","-1.05186237440962","0","-1.173800281031","0"]
        ]
      }"
    }

###<a name="scorewithseasonality-api"></a>ScoreWithSeasonality interfejsu API

Interfejs API ScoreWithSeasonality jest używana do uruchomionych wykrywania anomalii szeregu czasowego, której mają sezonowy wzorców. Ten interfejs API jest przydatna do wykrywania odchylenia w sezonowy wzorców.  

Na poniższej ilustracji pokazano przykład różnic w odniesieniu wykryty szeregu czasowego sezonowy. Szeregu czasowego ma jedną kolekcji (1st czarna kropka), dwa spadku (2 czarna kropka i jedną na końcu) i jedną zmianę poziomu (czerwona kropka). Należy zauważyć, że dip na środku szeregu czasowego oraz zmiany poziomu tylko discernable po usunięciu sezonowy składniki z serii.

![Sezonowości interfejsu API][2]

**ADRES URL**

    https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/ScoreWithSeasonality 

**Przykład treści żądania**

W wezwaniu na poniżej wyślemy do szeregu czasowego (jak pokazano obcięte) z punktów danych z 3-1-2016 za pośrednictwem 2016-3-10 i parametrów API wykrywania, jeśli dowolny z tych punktów danych jest anomalous. 

    {
      "data": [
        [ "9/21/2014 11:10:00 AM", "9" ],
        [ "9/21/2014 11:15:00 AM", "12" ],
        [ "9/21/2014 11:20:00 AM", "10" ]
      ],
      "params": {
        "preprocess.aggregationInterval": "0",
        "preprocess.aggregationFunc": "mean",
        "preprocess.replaceMissing": "lkv",
        "postprocess.tailRows": "1",
        "zspikedetector.sensitivity": "3",
        "tspikedetector.sensitivity": "3",
        "upleveldetector.sensitivity": "3.25",
        "bileveldetector.sensitivity": "3.25",
        "trenddetector.sensitivity": "3.25",
        "detectors.historywindow": "500",
        "seasonality.enable": "true",
        "seasonality.transform": "deseason",
        "seasonality.numSeasonality": "1"
      }
    }

Odpowiedzi na ten będzie: 

    {
        "odata.metadata": "https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/$metadata#AnomalyDetection.FrontEndService.Models.AnomalyDetectionResult",
        "ADOutput": "{
            "ColumnNames":["Time","OriginalData","ProcessedData","TSpike","ZSpike","PScore","PAlert","RPScore","RPAlert","TScore","TAlert"],
            "ColumnTypes":["DateTime","Double","Double","Double","Double","Double","Int32","Double","Int32","Double","Int32"],
            "Values":[
                ["9/21/201411: 20: 00AM","10","10","0","0","-1.30229513613974","0","-1.30229513613974","0","-1.173800281031","0"]
            ]
        }"
    }

###<a name="detectors"></a>Wykrywacze

Wykrywania anomalii interfejsu API obsługuje wykrywacze w 3 kategorie. Szczegółowe informacje o określone parametry wejściowe i wyjściowe dla każdego wykrywanie znajdują się w poniższej tabeli.

|Wykrywanie kategorii|Wykrywanie|Opis|Parametrów wejściowych|Wyjściowe
|---|---|---|---|---|
|Wykrywacze kolekcji|Wykrywanie TSpike|Wykrywanie tych najwyższych wartościach i spadku na podstawie skrajnej wartości są z pierwszym i trzecim Kwartyle|*tspikedetector.sensitivity:* trwa całkowitą z zakresu od 1-10, domyślne: 3; Wyższe wartości będzie przechwytywać więcej wartości skrajne, więc co mniej poufnych|TSpike: wartości binarnych — "1", jeśli zostanie wykryta kolekcji dip, "0" inaczej|
||Wykrywanie ZSpike|Wykrywanie tych najwyższych wartościach i spadku oparte na wydrukowanym są punktów danych od ich wartości średniej|*zspikedetector.sensitivity:* wykonać całkowitą z zakresu od 1-10, domyślne: 3; Wyższe wartości będzie przechwytywać więcej wartości skrajne, dzięki czemu mniej poufnych|ZSpike: wartości binarnych — "1", jeśli zostanie wykryta kolekcji dip, "0" inaczej|
|Wykrywanie powolne trendu.|Wykrywanie powolne trendu.|Wykrywanie powolne trendu dodatnia zgodnie czułości zestawu|*trenddetector.sensitivity:* próg na wykrywanie wynik (domyślny: 3,25, 3,25 — 5 jest rozsądne zakres, aby zaznaczyć; Wyższa mniej wielkość liter)|TScore: opadające liczbę reprezentującą wynik anomalii na trendu.|
|Zmienianie poziomu wykrywacze|Wykrywanie jednokierunkowe zmiany poziomu|Wykrywanie górę poziom zmienić zgodnie czułości zestawu|*upleveldetector.sensitivity:* próg na wykrywanie wynik (domyślny: 3,25, 3,25 — 5 jest rozsądne zakres, aby zaznaczyć; Wyższa mniej wielkość liter)|PScore: Liczba przedstawiająca wynik anomalii na górę przestawny Zmień poziom|
||Wykrywanie Zmień poziom dwukierunkowy|Wykrywanie zarówno w górę, jak i w dół zmiany poziomu zgodnie czułości zestawu|*bileveldetector.sensitivity:* próg na wykrywanie wynik (domyślny: 3,25, 3,25 — 5 jest rozsądne zakres, aby zaznaczyć; Wyższa mniej wielkość liter)|RPScore: opadające liczb przedstawiających anomalii, których wynik w górę i w dół poziom zmienić

###<a name="parameters"></a>Parametry

Bardziej szczegółowe informacje dotyczące tych parametrów wejściowych jest wymienione w poniższej tabeli:

|Parametrów wejściowych|Opis|Ustawienie domyślne|Typ|Prawidłowe zakresy|Sugerowany zakres|
|---|---|---|---|---|---|
|preprocess.aggregationInterval|Interwał agregacji w sekundach dla agregacji wprowadzania szeregu czasowego|0 (nie jest agregację)|Liczba całkowita|0: w przeciwnym razie przejdź agregacji, > 0|5 minut na 1 dzień, zależne od szeregu czasowego
|preprocess.aggregationFunc|Funkcja używana do agregowania danych do określonej AggregationInterval|Średnia|wyliczane|Średnia, Suma, długość|N/D!|
|preprocess.replaceMissing|Wartości umożliwia naliczenie brakujących danych|lkv (określanym ostatnia wartość)|wyliczane|zero, lkv, średnia|N/D!|
|detectors.historyWindow|Historia (w liczba punktów danych) na potrzeby anomalii wynik obliczeń|500|Liczba całkowita|10-2000|Zależne od szeregu czasowego|
|upleveldetector.sensitivity|Uwzględnianie dotyczącego górę zmienić wykrywanie. |3,25|podwójne|Brak|5(Lesser values mean more sensitive) 3,25|
|bileveldetector.sensitivity|Uwzględnianie dotyczącego dwukierunkowy zmienić wykrywanie. |3,25|podwójne|Brak|5(Lesser values mean more sensitive) 3,25|
|trenddetector.sensitivity|Uwzględnianie dla wykrywanie dodatnie trendu. |3,25|podwójne|Brak|5(Lesser values mean more sensitive) 3,25|
|tspikedetector.sensitivity |Uwzględnianie dla wykrywanie TSpike|3|Liczba całkowita|1-10|3 5(Lesser values mean more sensitive)|
|zspikedetector.sensitivity |Uwzględnianie dla wykrywanie ZSpike|3|Liczba całkowita|1-10 |3 5(Lesser values mean more sensitive)|
|seasonality.enable|Czy ma zostać wykonane sezonowości analizy|wartość PRAWDA.|wartość logiczna|PRAWDA, FAŁSZ|Zależne od szeregu czasowego|
|seasonality.numSeasonality |Maksymalna liczba cykli okresowych wykrycie|1|Liczba całkowita|1, 2|1-2|
|seasonality.transform |Czy sezonowy trendu części należy usunąć przed zastosowaniem wykrywania anomalii (i)|deseason|wyliczane|Brak, deseason deseasontrend|N/D!|
|postprocess.tailRows |Liczba punktów najnowszych danych mają być przechowywane w wynik|0|Liczba całkowita|0 (zachować wszystkie punkty danych), lub określić liczbę punktów w celu zachowania wyników|N/D!|

###<a name="output"></a>Wynik
Interfejs API wykonuje wszystkie detektory w czasie serii danych i zwraca wyniki anomalii, jak i wskaźniki kolekcji binarne dla każdego punktu w czasie. W poniższej tabeli wymieniono wyjść z interfejsu API. 

|Wyjściowe|Opis|
|---|---|
|Czas|Sygnatury czasowe od nieprzetworzonych danych lub danych zagregowane (lub) kalkulacyjne Jeśli agregacji (lub) Brak zastosowano przypisywania danych|
|OriginalData|Wartości od nieprzetworzonych danych lub zagregowane (lub) kalkulacyjne danych, jeśli agregacji (lub) Brak zastosowano przypisywania danych|
|ProcessedData|Jedną z następujących czynności: <ul><li>Jeśli znaczną sezonowości Wykryto i deseason opcji; sezonowo szeregu czasowego</li><li>okresowo dostosowane i Beztrendowy szeregu czasowego, jeśli wykryto znaczną sezonowości i zaznaczoną opcją deseasontrend</li><li>w przeciwnym razie jest taka sama, jak OriginalData</li>|
|TSpike|Wskaźnik binarny, aby wskazać, czy kolekcji zostanie wykryte przez wykrywanie TSpike|
|ZSpike|Wskaźnik binarny, aby wskazać, czy kolekcji zostanie wykryte przez wykrywanie ZSpike|
|Pscore|Przestawny Liczba przedstawiająca wynik anomalii na poziomie górę Zmienianie|
|Palert|wartość 1 i 0 wskazuje, że istnieje górę poziom Zmienianie anomalii według wprowadzania charakteru wiadomości|
|RPScore|Przestawny anomalii reprezentujące liczba wyników na zmiany poziomu dwukierunkowy|
|RPAlert|wartość 1 i 0 wskazuje, że istnieje poziom dwukierunkowy Zmienianie anomalii według wprowadzania charakteru wiadomości|
|TScore|Przestawny anomalii reprezentujące liczb wynik na dodatnią trendu.|
|TAlert|wartość 1 i 0 wskazującą, jest anomalii dodatnia trendu, oparte na wprowadzania charakteru wiadomości|


Tego raportu można analizować, przy użyciu [prostej parsera](https://adresultparser.codeplex.com/) — zawiera przykładowy kod, który pokazuje, jak połączyć API i analizowania danych wyjściowych. Wykryto różnic w odniesieniu można zwizualizować na pulpicie nawigacyjnym i/lub przekazywane do ludzi ekspertów dotyczące działań naprawczych lub zintegrowany biletów systemów.


[1]: ./media/machine-learning-apps-anomaly-detection/anomaly-detection-score.png
[2]: ./media/machine-learning-apps-anomaly-detection/anomaly-detection-seasonal.png

 

 
