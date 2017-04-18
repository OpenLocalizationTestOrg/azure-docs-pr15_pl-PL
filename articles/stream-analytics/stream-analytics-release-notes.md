<properties 
    pageTitle="Informacje o wersji analizy strumieniu | Microsoft Azure" 
    description="Informacje o wersji analizy strumieniu" 
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

#<a name="stream-analytics-release-notes"></a>Informacje o wersji analizy strumieniu

## <a name="notes-for-04152016-release-of-stream-analytics"></a>Informacje o wersji 2016-04-15 analizy strumieniu ##

Ta wersja zawiera następującą aktualizację.

Tytuł | Opis
---|---
Wyświetla ogólnodostępną usługi Power BI  | [Power BI Wyświetla](stream-analytics-power-bi-dashboard.md) teraz są powszechnie dostępne. Usunięto wygaśnięcia autoryzacji 90 dni w usłudze Power BI. Więcej informacji na temat scenariusze, w którym autoryzacji konieczności odnowienia nazwy zobacz sekcję [autoryzacji Odnów](stream-analytics-power-bi-dashboard.md#Renew-authorization) tworzenia pulpitu nawigacyjnego programu Power BI.

## <a name="notes-for-03032016-release-of-stream-analytics"></a>Informacje o wersji 2016-03-03 analizy strumieniu ##

Ta wersja zawiera następującą aktualizację.

Tytuł | Opis
---|---
Nowe elementy języka kwerend analizy strumieniu  | SAQL zawiera teraz [GetType](https://msdn.microsoft.com/library/azure/mt643890.aspx "Strony w witrynie MSDN GetType"), [TRY_CAST](https://msdn.microsoft.com/library/azure/mt643735.aspx "Strony w witrynie MSDN TRY_CAST") i [REGEXMATCH](https://msdn.microsoft.com/library/azure/mt643891.aspx "REGEXMATCH strony w witrynie MSDN").

## <a name="notes-for-12102015-release-of-stream-analytics"></a>Informacje o wersji 2015-12-10 analizy strumieniu ##

Ta wersja zawiera następującą aktualizację.

Tytuł | Opis
---|---
Aktualizacja wersji interfejsu API usługi REST | Wersja interfejsu API usługi REST została zaktualizowana do 2015-10-01. Szczegóły można znaleźć w witrynie MSDN [Strumienia analizy zarządzania pozostałych API Reference](https://msdn.microsoft.com/library/azure/dn835031.aspx) i [integracji maszynowego uczenia analizy strumieniu](stream-analytics-how-to-configure-azure-machine-learning-endpoints-in-stream-analytics.md).
Integracja nauki Azure komputera | W tej wersji pochodzi z pomocą techniczną usługi Azure maszynowego uczenia funkcje zdefiniowane przez użytkownika. Zobacz [Samouczek](stream-analytics-machine-learning-integration-tutorial.md) więcej informacji, jak również [Zawiadomienie o głównej blogu](http://blogs.technet.com/b/machinelearning/archive/2015/12/10/apply-azure-ml-as-a-function-in-azure-stream-analytics.aspx).

## <a name="notes-for-11122015-release-of-stream-analytics"></a>Informacje o wersji 2015-11-12 analizy strumieniu ##

Ta wersja zawiera następującą aktualizację.

Tytuł | Opis
---|---
Nowe zachowanie wybierz pozycję | Wybierz pozycję analizy strumieniu została rozszerzona umożliwia * jako udostępniający właściwości zagnieżdżonych rekordu. Aby uzyskać więcej informacji zapoznaj się [http://msdn.microsoft.com/library/mt622759.aspx](http://msdn.microsoft.com/library/mt622759.aspx "Złożonych typów danych").

## <a name="notes-for-10222015-release-of-stream-analytics"></a>Informacje o wersji analizy strumieniu 2015-10-22 ##

Ta wersja zawiera następujące aktualizacje.

Tytuł | Opis
---|---
Funkcje dla języków dodatkowych kwerend | Analizy strumieniu został rozwinięty języka kwerend, dołączając następujące funkcje: [moduł.liczby](https://msdn.microsoft.com/library/azure/mt574054.aspx), [CEILING](https://msdn.microsoft.com/library/azure/mt605286.aspx) [EXP](https://msdn.microsoft.com/library/azure/mt605289.aspx), [powierzchnia](https://msdn.microsoft.com/library/azure/mt605240.aspx), [POWER](https://msdn.microsoft.com/library/azure/mt605287.aspx), [logowania](https://msdn.microsoft.com/library/azure/mt605290.aspx), [KWADRATOWY](https://msdn.microsoft.com/library/azure/mt605288.aspx)i [pierwiastek](https://msdn.microsoft.com/library/azure/mt605238.aspx).
Usunięte ograniczenia agregacji  | Ta wersja usuwa ograniczenia 15 agregacji w kwerendzie. Teraz jest nie ograniczenia liczby agregacji w kwerendzie.
Dodano funkcję grupy przez System.Timestamp | Funkcja [Grupuj według](https://msdn.microsoft.com/library/azure/dn835023.aspx) teraz umożliwia window_type lub [System.Timestamp](https://msdn.microsoft.com/library/azure/mt598501.aspx).
Dodano przesunięcie Tumbling i skaczące systemu windows | Domyślnie system windows [Tumbling](https://msdn.microsoft.com/library/azure/dn835055.aspx) i [Hopping](https://msdn.microsoft.com/library/azure/dn835041.aspx) są wyrównane względem czas wynosi zero (1-1-0001 UTC 12:00:00 AM). Nowy parametr (opcjonalnie) "offsetsize" umożliwia określenie niestandardowej przesunięcie (lub wyrównanie).


## <a name="notes-for-09292015-release-of-stream-analytics"></a>Informacje o wersji 2015-09-29 analizy strumieniu ##

Ta wersja zawiera następujące aktualizacje.

Tytuł | Opis
---|---
Podgląd publicznej pakietu Azure IoT | Analizy strumieniu znajduje się w podglądzie publicznej pakietu IoT Azure.
Integracja Portal Azure | Oprócz ciągłej obecności w portalu zarządzania Azure analizy strumieniu jest teraz zintegrowany w [Azure Portal](https://azure.microsoft.com/overview/preview-portal/). Uwaga, że funkcja w portalu Preview jest aktualnie analizy strumieniu podzestaw funkcji oferowane w portalu zarządzania Azure bez pomocy technicznej do kwerendy w przeglądarce testowania, Power BI wyjściowy konfiguracji i przeglądania lub tworząc nowe dane wejściowe i wyjściowe zasobów w subskrypcje, do których masz dostęp do.
Obsługa wyjścia DocumentDB | Zadania analizy strumieniu można teraz wyjściowy do [DocumentDB](https://azure.microsoft.com/services/documentdb/).
Obsługa IoT centrum danych wejściowych | Zadania analizy strumieniu można teraz mogły zjeść tej ostatniej danych z koncentratorów IoT.
Sygnatura CZASOWA przez niejednorodnymi zdarzenia | Gdy jeden strumień zawiera wiele typów zdarzeń o sygnatury czasowe w różnych polach, teraz umożliwia [Sygnatury czasowej,](http://msdn.microsoft.com/library/mt573293.aspx) przy użyciu wyrażeń określenie pól różnych sygnatury czasowej w każdym przypadku.

## <a name="notes-for-09102015-release-of-stream-analytics"></a>Informacje o wersji 2015-09-10 analizy strumieniu ##

Ta wersja zawiera następujące aktualizacje.

Tytuł|Opis
---|---
Obsługa grup PowerBI|Aby włączyć udostępnianie danych innym użytkownikom usługi Power BI, analizy strumieniu zadania można teraz napisać grupom [PowerBI](stream-analytics-define-outputs.md#power-bi) wewnątrz konta usługi Power BI.

## <a name="notes-for-08202015-release-of-stream-analytics"></a>Informacje o wersji 2015-08-20 analizy strumieniu ##

Ta wersja zawiera następujące aktualizacje.

Tytuł|Opis
---|---
Dodano funkcja LAST |Funkcja [LAST](http://msdn.microsoft.com/library/mt421186.aspx) teraz jest dostępna w miejscu pracy analizy strumieniu, umożliwiając pobrać najnowsze zdarzenia w strumieniu zdarzenia w danym czasie.
Nowe funkcje tablicy|Funkcje tablicy [GetArrayElement](http://msdn.microsoft.com/library/mt270218.aspx), [GetArrayElements](http://msdn.microsoft.com/library/mt298451.aspx) i [GetArrayLength](http://msdn.microsoft.com/library/mt270226.aspx) są teraz dostępne.
Nowe funkcje rekordu|Funkcje rekordu [GetRecordProperties](http://msdn.microsoft.com/library/mt270221.aspx) i [GetRecordPropertyValue](http://msdn.microsoft.com/library/mt270220.aspx) są teraz dostępne.

## <a name="notes-for-07302015-release-of-stream-analytics"></a>Informacje o wersji 2015-07-30 analizy strumieniu ##

Ta wersja zawiera następujące aktualizacje.

Tytuł|Opis
---|---
Power BI Org Id odłączona od identyfikatora Azure|Ta funkcja umożliwia [Wyjście Power BI](stream-analytics-power-bi-dashboard.md) dla zadań ASA w obszarze dowolnego typu konto Azure (identyfikator Live Id lub identyfikator organizacji). Ponadto można mieć jeden identyfikator organizacji dla Twojego konta Azure i używać inny autoryzowanie wyjściowe Power BI.
Obsługa kolejek Bus usług wyjścia|Wyjściowe [Kolejek Bus usług](stream-analytics-define-outputs.md#service-bus-queues) są teraz dostępne w zadaniach analizy strumieniu.
Obsługa danych wyjściowych tematy Bus usługi|[Tematy Bus usługi](stream-analytics-define-outputs.md#service-bus-topics) wyjściowe są teraz dostępne w zadaniach analizy strumieniu.

## <a name="notes-for-07092015-release-of-stream-analytics"></a>Informacje o wersji 2015-07-09 analizy strumieniu ##

Ta wersja zawiera następujące aktualizacje.


Tytuł|Opis
---|---
Niestandardowe Blob podziału wyjścia|Wyjściowe magazyn obiektów blob teraz opcję Zezwalaj na określenie częstotliwości tego dane wyjściowe, które są zapisywane obiektów blob i strukturę i formatowanie struktura folderów ścieżka dane wyjściowe. 

## <a name="notes-for-05032015-release-of-stream-analytics"></a>Informacje o wersji 2015-05-03 analizy strumieniu ##

Ta wersja zawiera następujące aktualizacje.


Tytuł|Opis
---|---
Zwiększona maksymalną wartość porządku w nowym oknie uszkodzenia|Maksymalny rozmiar okna uszkodzenia w nowym porządku jest teraz 59:59 (mm: ss)
Format docelowy JSON: Linii rozdzielając lub tablicy|Obecnie ma opcji dla magazyn obiektów Blob lub Centrum zdarzeń wyjściowy jako albo tablica JSON obiektów lub rozdzielając JSON obiektów z nowego wiersza. 

## <a name="notes-for-04162015-release-of-stream-analytics"></a>Informacje o wersji 2015-04-16 analizy strumieniu ##


Tytuł|Opis
---|---
Opóźnienie w konfiguracji konta magazynu platformy Azure|Podczas tworzenia zadania analizy strumieniu w regionie po raz pierwszy, zostanie wyświetlony monit o Utwórz nowe konto miejsca do magazynowania lub Określ istniejące konto monitorowania analizy strumieniu zadania w danym regionie. Ze względu na czas oczekiwania w konfigurowaniu monitorowania tworzenie zadanie analizy strumieniu w tym samym regionie w 30 minut wyświetli monit o określenie drugie konto miejsca do magazynowania zamiast przedstawiający ostatnio skonfigurowaną jedną z listy rozwijanej konta monitorowanie miejsca do magazynowania. Aby uniknąć tworzenia konta niepotrzebne miejsca do magazynowania, poczekaj 30 minut po utworzeniu zadanie w regionie po raz pierwszy przed dodatkowych zadań w danym regionie inicjowania obsługi administracyjnej.
Uaktualnianie zadania|W tej chwili analizy strumieniu nie obsługuje live edycji definicji i konfiguracji uruchomione zadanie. W celu zmiany danych wejściowych, dane wyjściowe, kwerendy, skali lub konfiguracji uruchomione zadanie, należy zatrzymać zadania.
Typy danych wnioskowane ze źródła danych wejściowych|Jeśli instrukcji CREATE TABLE nie jest używany, typ danych wejściowych pochodzi z formatu wprowadzania danych, na przykład wszystkie pola z pliku CSV są ciąg. Pola muszą przekonwertowane jawnie odpowiedniego typu przy użyciu funkcji CAST, aby uniknąć błędów niezgodności typów.
Brakujące pola są outputted jako wartości null|Odwoływanie się do pola, którego nie ma w źródle wprowadzania spowoduje wartości null w przypadku danych wyjściowych.
W instrukcjach musi poprzedzać instrukcji SELECT|W kwerendzie instrukcji SELECT muszą obserwować podkwerend zdefiniowane przy użyciu instrukcji.
Problem brakiem pamięci|Przesyłanie strumieniowe zadania analizy dużych uszkodzenia dla zdarzenia w nowym oknie kolejności i/lub złożonych kwerend z zachowaniem dużej ilości stan może spowodować zadania brakować pamięci, uzyskując zadanie ponownie. Operacje uruchomieniu i zatrzymaniu będą widoczne w dzienniki operacji zadania. Aby uniknąć tego problemu, skalowanie kwerendy się między wieloma partycje. W przyszłej wersji to ograniczenie zostaną uwzględnione w obniżeniu wydajności na ryzyko zadań zamiast ponownego uruchomienia je.
Wartości wejściowych dużych obiektów blob bez sygnatury czasowej ładunku mogą powodować problemu brakiem pamięci|Używające dużych plików z magazynem obiektów Blob może spowodować zadań analizy strumieniu awarie, gdy pole sygnatury czasowej nie zostało określone przez sygnatura CZASOWA przez. Aby uniknąć tego problemu, pamiętaj rozmiar poszczególnych obiektów blob w obszarze 10MB.
Ograniczenie głośność zdarzenia bazy danych SQL|Podczas używania bazy danych SQL jako obiekt docelowy format wyjściowy, bardzo dużych ilości danych wyjściowych może spowodować zadanie analizy strumieniu limitu czasu. Aby rozwiązać ten problem, zmniejszanie głośności wyjścia przy użyciu agregacji lub operatorów filtru lub zamiast tego wybrać magazyn obiektów Blob platformy Azure lub koncentratorów zdarzenia jako obiekt docelowy format wyjściowy.
Zestawy danych PowerBI może zawierać tylko jedną tabelę|PowerBI nie obsługuje więcej niż jednej tabeli w danym zestawie danych.

## <a name="get-help"></a>Uzyskiwanie pomocy
Aby uzyskać dodatkową pomoc spróbuj nasze [forum analizy strumieniu Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Następne kroki

- [Wprowadzenie do analizy strumieniu Azure](stream-analytics-introduction.md)
- [Wprowadzenie do korzystania z analizy strumieniu Azure](stream-analytics-get-started.md)
- [Skalowanie Azure analizy strumieniu zadania](stream-analytics-scale-jobs.md)
- [Dokumentacja języka kwerend analizy strumieniu Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Strumień Azure analizy zarządzania pozostałych interfejsu API odwołania](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 
