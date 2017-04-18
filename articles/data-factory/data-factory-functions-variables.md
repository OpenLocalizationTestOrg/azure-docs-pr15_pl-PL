<properties 
    pageTitle="Funkcje Factory danych i zmienne systemowe | Microsoft Azure" 
    description="Lista funkcji Factory danych Azure i zmienne systemowe" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"
    services="data-factory"
/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016" 
    ms.author="shlo"/>

# <a name="azure-data-factory---functions-and-system-variables"></a>Azure danych Factory - funkcje i zmienne systemowe
Ten artykuł zawiera informacje o funkcji i zmiennych obsługiwanych przez Azure danych Factory.
  
## <a name="data-factory-system-variables"></a>Zmienne systemowe Factory danych

Nazwa zmiennej | Opis | Zakres obiektu | Zakres JSON i przypadków użycia
------------- | ----------- | ------------ | ------------------------
WindowStart | Początek interwału bieżących działań uruchomić okno | działanie | <ol><li>Określ kwerend zaznaczenia danych. Zobacz artykuły łącznik zawarte w artykule [Działania przepływu danych](data-factory-data-movement-activities.md) .</li><li>Przekazywać parametry do gałęzi skryptu (przykład pokazano powyżej).</li>
WindowEnd | Koniec interwału bieżących działań uruchomić okno | działanie | jak wyżej
SliceStart | Początek interwału wycinek wyprodukowane | działanie<br/>zestaw danych | <ol><li>Określ ścieżki dynamiczne folderu i nazwy plików podczas pracy z [Obiektów Blob platformy Azure](data-factory-azure-blob-connector.md) i [zestawy danych w systemie plików](data-factory-onprem-file-system-connector.md).</li><li>Określ zależności wprowadzania danych przy użyciu funkcji factory danych w zbiorze wartości wejściowych aktywności.</li></ol>
SliceEnd | Koniec interwał czasu dla bieżącego wycinek wyprodukowane | działanie<br/>zestaw danych | działa tak samo jak powyżej. 

> [AZURE.NOTE] Obecnie factory danych musi być zgodna z harmonogramem określonym w udostępnianiu zestawu danych dane wyjściowe z harmonogramem dokładnie określonym w działaniu. Oznacza to, że WindowStart, WindowEnd i SliceStart oraz SliceEnd są zawsze mapowane na tym samym czasie i wycinek pojedynczy wynik.
 
## <a name="data-factory-functions"></a>Funkcje Factory danych 

Za pomocą funkcji factory danych razem z powyżej zmienne wymienione systemu dla następujących celów:

1.  Określanie kwerend zaznaczenia danych (zobacz artykuły łącznik odwołuje się artykuł [Działania przepływu danych](data-factory-data-movement-activities.md) .

    Składnia do wywołania funkcji factory danych jest: ** $$ ** dla kwerend zaznaczenia danych i inne właściwości w aktywności i zestawy danych.  
2. Określanie zależności wprowadzania danych przy użyciu funkcji factory danych w działaniu wejść zbioru (zobacz przykładowy powyżej).

    $ nie jest potrzebna do określania współzależności wprowadzania wyrażeń.   

W następującym przykładzie właściwość **sqlReaderQuery** w pliku JSON przydzielono wartość zwracana przez funkcję **Text.Format** . W tym przykładzie użyto także zmienna o nazwie **WindowStart**, która reprezentuje czas rozpoczęcia działalności Uruchom okno.
    
    {
        "Type": "SqlSource",
        "sqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
    }

### <a name="functions"></a>Funkcje

W poniższej tabeli wymieniono wszystkie funkcje w Azure Factory danych:

Kategoria | Funkcja | Parametry | Opis
-------- | -------- | ---------- | ----------- 
Czas | AddHours(X,Y) | X: daty i godziny <br/><br/>Y: int | Dodaje godzin Y do danej chwili X. <br/><br/>Przykład: 2013 9-5 godzina 12:00:00 + 2 godzin = 9/5/2013 2:00:00 PM
Czas | AddMinutes(X,Y) | X: daty i godziny <br/><br/>Y: int | Sumuje X Y minut.<br/><br/>Przykład: 2013 9-15 godzina 12:00:00 + 15 minut = 9-15/2013 12:15:00 PM
Czas | StartOfHour(X) | X: daty i godziny | Pobiera godzinę rozpoczęcia przez godzinę reprezentowany przez godzinę część X. <br/><br/>Przykład: StartOfHour 2013-9-15 05:22:23: 00 jest 2013-9-15 05:00:00 PM
Data | AddDays(X,Y) | X: daty i godziny<br/><br/>Y: int | Dodaje dni Y do X.<br/><br/>Przykład: 2013 9-15 godzina 12:00:00 + 2 dni = 9-17/2013 12:00:00 PM
Data | AddMonths(X,Y) | X: daty i godziny<br/><br/>Y: int | Dodaje miesięcy Y x.<br/><br/>Przykład: 2013 9-15 godzina 12:00:00 + 1 miesiąc = 10-15/2013 12:00:00 PM 
Data | AddQuarters(X,Y) | X: daty i godziny <br/><br/>Y: int | Dodaje Y * 3 miesiące x.<br/><br/>Przykład: 2013 9-15 godzina 12:00:00 + 1 kwartał = 12-15/2013 12:00:00 PM
Data | AddWeeks(X,Y) | X: daty i godziny<br/><br/>Y: int | Dodaje Y * 7 dni X<br/><br/>Przykład: 2013 9-15 godzina 12:00:00 + 1 tydzień = 9-22/2013 12:00:00 PM
Data | AddYears(X,Y) | X: daty i godziny<br/><br/>Y: int | Dodaje lat Y x.<br/><br/>Przykład: 2013 9-15 godzina 12:00:00 + 1 rok = 9-15/2014 12:00:00 PM
Data | Day(X) | X: daty i godziny | Otrzymuje dnia część X.<br/><br/>Przykład: Dzień 2013-9-15 12:00:00 PM jest 9. 
Data | DayOfWeek(X) | X: daty i godziny | Otrzymuje dnia tygodnia część X.<br/><br/>Przykład: DayOfWeek 2013-9-15 12:00:00 PM jest niedziela.
Data | DayOfYear(X) | X: daty i godziny | Otrzymuje dnia w roku reprezentowana przez część roku X.<br/><br/>Przykłady:<br/>2015-12-1: dzień 335 2015 r.<br/>2015-12-31: dzień 365 2015 r.<br/>2016-12-31: dzień 366 2016 (rok przestępny)
Data | DaysInMonth(X) | X: daty i godziny | Otrzymuje dni w miesiącu reprezentowany przez składnik miesiąc parametr X.<br/><br/>Przykład: DaysInMonth 2013-9-15 są 30, ponieważ zawiera 30 dni miesiąca września.
Data | EndOfDay(X) | X: daty i godziny | Otrzymuje daty i godziny zakończenia dnia X (składnik dnia).<br/><br/>Przykład: EndOfDay 2013-9-15 05:22:23: 00 jest 2013-9-15 11:59:59 PM.
Data | EndOfMonth(X) | X: daty i godziny | Otrzymuje na koniec miesiąca reprezentowany przez składnik miesiąc parametr X. <br/><br/>Przykład: EndOfMonth 2013-9-15 05:22:23: 00 jest 2013-9-30 11:59:59 PM (Data Godzina zakończenia dnia miesiąca września)
Data | StartOfDay(X) | X: daty i godziny | Otrzymuje rozpoczęcia dnia reprezentowany przez składnik dzień parametr X.<br/><br/>Przykład: StartOfDay 2013-9-15 05:22:23: 00 jest 2013-9-15 12:00:00 AM.
Daty i godziny | FROM(X) | X: ciąg | Przeanalizować ciągu X na czas daty.
Daty i godziny | Ticks(X) | X: daty i godziny | Otrzymuje znaczniki właściwość parametru X. Jeden osi jest równa 100 nanosekundach. Wartości tej właściwości oznacza liczbę znaczników, jaka upłynęła od północy 12:00:00 1 stycznia 0001. 
Tekst | Format(X) | X: zmiennej będącej ciągiem | Formatuje tekst.

#### <a name="textformat-example"></a>Przykład Text.Format

    "defines": { 
        "Year" : "$$Text.Format('{0:yyyy}',WindowStart)",
        "Month" : "$$Text.Format('{0:MM}',WindowStart)",
        "Day" : "$$Text.Format('{0:dd}',WindowStart)",
        "Hour" : "$$Text.Format('{0:hh}',WindowStart)"
    }

Zobacz [niestandardowe daty i godziny ciągi formatów](https://msdn.microsoft.com/library/8kb3ddd4.aspx) tematu w tym artykule opisano różne opcje formatowania możesz użyć (na przykład: RR a rrrr). 

> [AZURE.NOTE] Podczas korzystania z funkcji wewnątrz innej funkcji, nie trzeba używać **$$** prefiks dla funkcji wewnętrzne. Na przykład: $$Text.Format ("PartitionKey eq \\" my_pkey_filter_value\\"i stronę RowKey \\" {0:yyyy-MM-dd HH: mm:}\\", Time.AddHours (SliceStart -6)). W tym przykładzie należy zauważyć, że **$$** prefiks nie jest używany dla funkcji **Time.AddHours** . 
  

