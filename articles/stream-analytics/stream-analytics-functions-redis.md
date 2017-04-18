<properties
    pageTitle="Dane wyjściowe do Azure Redis pamięci podręcznej, za pomocą funkcji Azure, z analizy strumieniu Azure | Microsoft Azure"
    description="Dowiedz się, jak używać funkcji Azure połączony kolejki Bus usługi, można wypełnić Cache Redis Azure z wyników zadanie analizy strumieniu."
    keywords="strumień danych, redis pamięci podręcznej, usługa bus kolejki"
    services="stream-analytics"
    authors="ryancrawcour"
    manager="jhubbard"
    documentationCenter=""
    />

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/26/2016"
    ms.author="ryancraw"/>

# <a name="how-to-store-data-from-azure-stream-analytics-in-an-azure-redis-cache-using-azure-functions"></a>Jak zapisać dane z analizy strumieniu Azure w Azure Redis pamięci podręcznej za pomocą funkcji Azure

Azure analizy strumieniu pozwala szybko opracowywać i wdrażanie rozwiązań kosztach uzyskanie wniosków w czasie rzeczywistym z urządzenia, czujniki, infrastruktura i aplikacje lub strumieniem danych. Umożliwia różnych przypadków użycia, takich jak zarządzanie w czasie rzeczywistym i monitorowania, polecenia i kontroli, wykrywania oszustwa, samochody połączonego i wiele innych. W wielu takich scenariuszy może być przechowywane dane outputted przez analizy strumieniu Azure do magazynu danych rozłożone, takich jak Azure Redis pamięci podręcznej.

Załóżmy, że są częścią firmy telekomunikacyjnych. Próbujesz wykrywanie oszustwa SIM miejsce, w którym połączenia wielu pochodzące z tej samej tożsamości, w tym samym czasie, ale w różnych geograficznie lokalizacje. Z przechowywania wszystkich potencjalnych fałszywej rozmów telefonicznych w pamięci podręcznej Azure Redis są podobne zadanie przypisane. W tym blogu możemy umożliwiają uzyskanie informacji jak mogą łatwo wykonywać zadania. 

## <a name="prerequisites"></a>Wymagania wstępne
[W czasie rzeczywistym oszustwa wykrywania] [ fraud-detection] samouczek dla ASA

## <a name="architecture-overview"></a>Przegląd architektury
![Zrzut ekranu przedstawiający architektura](./media/stream-analytics-functions-redis/architecture-overview.png)

Jak pokazano na kolejnej ilustracji, analizy strumieniu umożliwia strumieniowe przesyłanie danych wejściowych proszeni i wysyłane do wyników. W zależności od wyniku, funkcje Azure następnie może wyzwolić różnego rodzaju zdarzenia. 

W tym blogu możemy skupić się na funkcje Azure część tego procesu lub bardziej szczegółowo powodujące zdarzenie, które są przechowywane fałszywej dane w pamięci podręcznej.
Po zakończeniu [Wykrywania oszustwa w czasie rzeczywistym] [ fraud-detection] samouczek, są dane wejściowe (koncentratora zdarzenia), kwerendy i wynik (magazyn obiektów blob) już skonfigurowany i działa. W tym blogu zmienimy dane wyjściowe do zamiast tego użyj kolejki Bus usługi. Po wykonaniu tej funkcji Azure firma Microsoft Połącz do niej. 

## <a name="create-and-connect-a-service-bus-queue-output"></a>Tworzenie i łączenie usługi Bus kolejki dane wyjściowe
Aby utworzyć kolejkę Bus usługi, wykonaj kroki 1 i 2 w sekcji .NET [Wprowadzenie kolejek Bus usług][servicebus-getstarted].
Teraz Przyjrzyjmy nawiązać kolejki zadania analizy strumieniu został utworzony we wcześniejszej samouczek wykrywania oszustwa.



1. W portalu Azure przejdź do karta **wyjściowe** zadania i wybierz pozycję **Dodaj** w górnej części strony.

    ![Dodawanie wyjściowe](./media/stream-analytics-functions-redis/adding-outputs.png)

2. Wybierz pozycję **Usługa Bus kolejki** jako **zlew** i postępuj zgodnie z instrukcjami na ekranie. Należy wybrać przestrzeń nazw kolejki Bus usługi zostało utworzone w [Wprowadzenie kolejek Bus usług][servicebus-getstarted]. Gdy skończysz, kliknij przycisk "baczki".
3. Określ następujące wartości:
    - **Format serializatora zdarzenia**: JSON
    - **Kodowanie**: UTF8
    - **FORMAT**: linii oddzielone

4. Kliknij przycisk **Utwórz** , aby dodać to źródło i sprawdzić, czy analizy strumieniu pomyślnie łączy do rachunku miejsca do magazynowania.

5. Na karcie **Query** Zamień bieżącej kwerendy z następujących czynności. Zamień *[Nazwa i BUS usługi]* nazwy wyjściowego utworzony w kroku 3. 

    ```    

        SELECT 
            System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1, 
            CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2

        INTO [YOUR SERVICE BUS NAME]
    
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
            ON CS1.CallingIMSI = CS2.CallingIMSI AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
    
        WHERE CS1.SwitchNum != CS2.SwitchNum
    
    ```

## <a name="create-an-azure-redis-cache"></a>Tworzenie pamięci podręcznej Azure Redis
Utwórz Azure Redis pamięci podręcznej, wykonując sekcji .NET [sposobu użycia Azure Redis w pamięci podręcznej] [ use-rediscache] do momentu sekcji o nazwie ***Konfigurowanie klientów pamięci podręcznej***.
Po zakończeniu masz nową pamięć podręczną Redis. W obszarze **wszystkie ustawienia**wybierz **klawiszy dostępu** i zanotuj w ***ciągu podstawowego połączenia***.

![Zrzut ekranu przedstawiający architektura](./media/stream-analytics-functions-redis/redis-cache-keys.png)

## <a name="create-an-azure-function"></a>Tworzenie Azure funkcji
[Tworzenie pierwszego funkcja Azure] wykonaj[ functions-getstarted] samouczka, aby rozpocząć pracę z funkcjami Azure. Jeśli masz już funkcję Azure, którego chcesz użyć, następnie przejdź do [pisania Redis pamięci podręcznej](#Writing-to-Redis-Cache)

1. W portalu wybierz aplikację usługi z nawigacji po lewej stronie, a następnie kliknij nazwę aplikacji Azure funkcji uzyskiwania dostępu do funkcji aplikacji witryny sieci Web.
    ![Zrzut ekranu przedstawiający listę funkcji usług aplikacji](./media/stream-analytics-functions-redis/app-services-function-list.png)

2. Kliknij pozycję **nową funkcję > ServiceBusQueueTrigger — C#**. W następujących polach wykonaj następujące instrukcje:
    - **Nazwa kolejki**: taką samą nazwę jak wprowadzona podczas tworzenia kolejki w [Wprowadzenie kolejek Bus usług] nazwa[ servicebus-getstarted] (nie nazwę bus usługi). Upewnij się, że używasz kolejki podłączonego do wyników analizy strumieniu.
    - **Połączenie usługi Bus**: wybierz pozycję **Dodaj parametry połączenia**. Aby znaleźć parametry połączenia, przejdź do portalu klasyczny wybierz pozycję **Usługa Bus**, bus usługi, który został utworzony i **Informacje o połączeniu** u dołu ekranu. Upewnij się, że jesteś na ekranie głównym na tej stronie. Kopiowanie i wklejanie parametry połączenia. Zachęcamy wprowadzić dowolną nazwę połączenia.
    
        ![Zrzut ekranu: połączenie bus usługi](./media/stream-analytics-functions-redis/servicebus-connection.png)
    - **AccessRights**: wybierz pozycję **Zarządzanie**


3. Kliknij przycisk **Utwórz**

## <a name="writing-to-redis-cache"></a>Zapisywanie do Redis pamięci podręcznej
Funkcja Azure, który odczytuje z kolejki Bus usługi teraz został utworzony. Wszystkie opcje, które jest pozostałej do wykonania jest funkcja naszych zapisać te dane w pamięci podręcznej Redis. 

1. Wybierz pozycję do nowo utworzonego **ServiceBusQueueTrigger**, a następnie kliknij **funkcji aplikacji ustawienia** w prawym górnym rogu. Wybierz pozycję **Przejdź do pozycji Ustawienia usługi aplikacji > Ustawienia > Ustawienia aplikacji**

2. W sekcji ciągów połączenia Utwórz nazwę w sekcji **Nazwa** . Wklej parametry połączenia podstawowego, który został znaleziony w kroku **Utwórz pamięci podręcznej Redis** do sekcji **wartość** . Wybierz **Niestandardowy** napisem **Bazy danych SQL**.

3. U góry kliknij przycisk **Zapisz** .

    ![Zrzut ekranu: połączenie bus usługi](./media/stream-analytics-functions-redis/function-connection-string.png)

4. Teraz przejdź do ustawień usługi aplikacji i wybierz pozycję **Narzędzia > Edytor usługi aplikacji (wersja Preview) > na > Przejdź**.

    ![Zrzut ekranu: połączenie bus usługi](./media/stream-analytics-functions-redis/app-service-editor.png)

5. W edytorze wyboru Utwórz JSON pliku o nazwie **project.json** z następujących czynności, a następnie zapisz ją na lokalnym dysku.

        {
            "frameworks": {
                "net46": {
                    "dependencies": {
                        "StackExchange.Redis":"1.1.603",
                        "Newtonsoft.Json": "9.0.1"
                    }
                }
            }
        }

6. Przekazać tego pliku w katalogu głównym funkcji (nie WWWROOT). Powinny być widoczne są wyświetlane w pliku o nazwie **project.lock.json** automatycznie potwierdzenia, że Nuget pakietów "StackExchange.Redis" i "Newtonsoft.Json" zostały zaimportowane.

7. W pliku **run.csx** zastąpić wstępnie wygenerowanych poniższy kod. W funkcji lazyConnection Zamień "POŁ nazwa" na nazwę utworzonego w kroku 2 **Przechowuj dane w pamięci podręcznej Redis**.

````

    using System;
    using System.Threading.Tasks;
    using StackExchange.Redis;
    using Newtonsoft.Json;
    using System.Configuration;
    
    public static void Run(string myQueueItem, TraceWriter log)
    {
        log.Info($"Function processed message: {myQueueItem}");

        // Connection refers to a property that returns a ConnectionMultiplexer
        IDatabase db = Connection.GetDatabase();
        log.Info($"Created database {db}");
    
        // Parse JSON and extract the time
        var message = JsonConvert.DeserializeObject<dynamic>(myQueueItem);
        string time = message.time;
        string callingnum1 = message.callingnum1;

        // Perform cache operations using the cache object...
        // Simple put of integral data types into the cache
        string key = time + " - " + callingnum1;
        db.StringSet(key, myQueueItem);
        log.Info($"Object put in database. Key is {key} and value is {myQueueItem}");

        // Simple get of data types from the cache
        string value = db.StringGet(key);
        log.Info($"Database got: {value}"); 
    }

    // Connect to the Service Bus
    private static Lazy<ConnectionMultiplexer> lazyConnection = 
        new Lazy<ConnectionMultiplexer>(() =>
            {
                var cnn = ConfigurationManager.ConnectionStrings["CONN NAME"].ConnectionString
                return ConnectionMultiplexer.Connect();
            });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

````

## <a name="start-the-stream-analytics-job"></a>Uruchom zadanie analizy strumieniu

1. Uruchom aplikację telcodatagen.exe. Jest w następujący sposób:````telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]````

2. Karta zadanie analizy strumieniu w portalu kliknij **Start** w górnej części strony.

    ![Zrzut ekranu przedstawiający zadania rozpoczęcia](./media/stream-analytics-functions-redis/starting-job.png)

3. W kartę **rozpoczęcia zadania** , która jest wyświetlana wybierz pozycję **teraz** , a następnie kliknij przycisk **Start** , w dolnej części ekranu. Zmiany statusu zadania, aby początkowy, a po niektóre zmiany czasu pracy.
 
    ![Zrzut ekranu przedstawiający wyboru strefy czasowej rozpoczęcia zadania](./media/stream-analytics-functions-redis/start-job-time.png)

## <a name="run-solution-and-check-results"></a>Uruchom rozwiązanie i sprawdź wyniki
Rozważmy ponownie do strony **ServiceBusQueueTrigger** , powinien zostać wyświetlony instrukcji dziennika. Dzienniki te wskazują, że masz coś w kolejce Bus usługi umieszcza ją w bazie danych i pobrane go się przy użyciu czas jako klucz!

Aby sprawdzić, czy dane znajdują się w pamięci podręcznej Redis, przejdź do strony pamięci podręcznej Redis w portalu nowego (jak pokazano w poprzednim kroku [Utwórz pamięci podręcznej Azure Redis](#Create-an-Azure-Redis-Cache) ), a następnie wybierz pozycję Konsola.

Możesz teraz napisać Redis poleceń, aby potwierdzić, że dane są w rzeczywistości w pamięci podręcznej.

![Zrzut ekranu przedstawiający konsoli Redis](./media/stream-analytics-functions-redis/redis-console.png)

## <a name="next-steps"></a>Następne kroki
Emocje o nowościach funkcje Azure analizy strumieniu możliwości razem i firma Microsoft Życzymy dla Ciebie spowoduje to odblokowanie nowe możliwości. Jeśli masz opinię na następny ma zachęcamy do korzystania z [możliwości Azure witryny](https://feedback.azure.com/forums/270577-stream-analytics).

Jeśli masz nowy Microsoft Azure zachęcamy wypróbować przez zarejestrowanie [bezpłatne konto wersji próbnej Azure](https://azure.microsoft.com/pricing/free-trial/). Jeśli jesteś nowym użytkownikiem analizy strumieniu, następnie zachęcamy do [utworzenia pierwszego zadania analizy strumieniu](stream-analytics-create-a-job.md).

Jeśli potrzebujesz żadnych pomocy lub publikowanie masz pytania na forum [w witrynie MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) lub [zdarzeń Stackoverflow](http://stackoverflow.com/questions/tagged/azure-stream-analytics) . 

Możesz też zobaczyć następujące zasoby:

- [Dokumentacja dewelopera funkcje Azure](../azure-functions/functions-reference.md)
- [Azure Dokumentacja dewelopera funkcje C#](../azure-functions/functions-reference-csharp.md)
- [Azure Dokumentacja dewelopera funkcji F #](../azure-functions/functions-reference-fsharp.md)
- [Dokumentacja dewelopera NodeJS funkcje Azure](../azure-functions/functions-reference.md)
- [Azure wyzwalaczy funkcje i powiązania](../azure-functions/functions-triggers-bindings.md)
- [Jak można monitorować Azure Redis w pamięci podręcznej](../redis-cache/cache-how-to-monitor.md)

Aby otrzymywać aktualne na wszystkie najnowsze informacje i funkcje, wykonaj [@AzureStreaming](https://twitter.com/AzureStreaming) serwisie Twitter.


[fraud-detection]: stream-analytics-real-time-fraud-detection.md
[servicebus-getstarted]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[use-rediscache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md
[functions-getstarted]: ../azure-functions/functions-create-first-azure-function.md
