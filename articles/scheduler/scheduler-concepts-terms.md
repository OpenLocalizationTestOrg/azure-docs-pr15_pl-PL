<properties
 pageTitle="Harmonogram pojęcia, terminy i jednostki | Microsoft Azure"
 description="Azure pojęcia harmonogram, terminologia i hierarchia jednostki, w tym zadania i zbiory zadania.  Przedstawiono przykład Pełna zaplanowanego zadania."
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="get-started-article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="scheduler-concepts-terminology--entity-hierarchy"></a>Harmonogram pojęcia, terminologia + hierarchia jednostki

## <a name="scheduler-entity-hierarchy"></a>Hierarchia jednostki harmonogramu

W poniższej tabeli opisano głównym zasobów wyświetlane lub używane przez interfejs API harmonogramu:

|Zasób | Opis |
|---|---|
|**Kolekcja zadania**|Kolekcja zadania zawiera grupy zadań i przechowuje ustawienia, przydziałów i ograniczenia, które są udostępniane przez zadania w tym zbiorze. Kolekcja zadania jest tworzona przez właściciela subskrypcji i grupy zadania ze sobą według ograniczenia użycia lub aplikacji. Jest ograniczona do jednego regionu. Umożliwia także stosowania przydziałów, aby ograniczyć użycie wszystkie zadania w tej kolekcji. Kwoty obejmują MaxJobs i MaxRecurrence.|
|**Zadanie**|Zadanie Określa akcję powtarzającego się pojedynczego, przy użyciu prostej lub złożonej strategii wykonywania. Działania mogą obejmować HTTP, kolejki miejsca do magazynowania, kolejka bus serwisowych lub żądania tematu bus usług.|
|**Historię zatrudnienia**|Historię zatrudnienia reprezentuje szczegóły dotyczące wykonania zadania. Zawiera on sukcesu a błąd, a także wszystkie szczegóły odpowiedzi.|

## <a name="scheduler-entity-management"></a>Zarządzania harmonogramem jednostki

Na wysokim poziomie harmonogram i zarządzanie usługą interfejsu API uwidaczniane następujące operacje na zasoby:

|Funkcja|Opis i identyfikator URI adresu|
|---|---|
|**Zarządzanie zbiorem zadania**|Uzyskiwanie Umieść, a następnie usuń obsługę tworzenia i modyfikowania zbiorów zadania i zadania zawartych w niej. Kolekcja zadania jest kontenera zadań i mapy i przydziałami i wspólnych ustawień. Przykłady przydziałów opisanych później, maksymalną liczbę zadań i Najkrótszy interwał cyklu. <p>UMIESZCZANIE i usuwanie:`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</p><p>Pobierz:`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</p>
|**Zarządzanie zadaniami**|Uzyskiwanie Umieść, PUBLIKOWAĆ, poprawki i usuwanie obsługę tworzenie i modyfikowanie zadań. Wszystkie zadania musi należeć do zbioru zadań, który już istnieje, dlatego nie tworzenie niejawnych. <p>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}`</p>|
|**Zarządzanie zadaniami historii**|Uzyskiwanie pomocy technicznej pobierania 60 dni historii wykonywania zadań, takich jak czas pracy i wyniki wykonania zadania. Dodaje obsługę parametru ciągu kwerendy do filtrowania, na podstawie stanu i stanu. <P>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}/history`</p>|

## <a name="job-types"></a>Typy zadań

Istnieje wiele typów zadań: zadania HTTP (w tym HTTPS zadania, które obsługują protokołu SSL), zadania w kolejce miejsca do magazynowania, zadania w kolejce bus usługi i usługa bus temat zadania. HTTP zadania są idealnym rozwiązaniem, jeśli masz punktu końcowego obciążenie pracą lub usług. Za pomocą zadania w kolejce miejsca do magazynowania ogłaszać wiadomości do kolejek przestrzeni dyskowej, aby te zadania doskonale nadają się do obciążeniami, które korzystają z kolejek miejsca do magazynowania. Podobnie usługa bus zadania są idealne rozwiązanie w przypadku obciążenia korzystających z usługi bus kolejki i tematy.

## <a name="the-job-entity-in-detail"></a>Jednostki "zadania" szczegółowo

Na poziomie podstawowym zaplanowanego zadania zawiera kilka segmenty:

- Akcję do wykonania po zadania czasomierza  

- (Opcjonalnie) Czas wykonywania zadania  

- (Opcjonalnie) Kiedy i jak często należy powtórzyć zadania  

- (Opcjonalnie) AKCJA Jeśli główną akcję kończy się niepowodzeniem  

Wewnętrznie zaplanowanego zadania zawiera również system danych, takich jak następnym wykonanie według harmonogramu.

Poniższy kod zawiera pełna przykład zaplanowanego zadania. Szczegóły można znaleźć w kolejnych sekcjach.

    {
        "startTime": "2012-08-04T00:00Z",               // optional
        "action":
        {
            "type": "http",
            "retryPolicy": { "retryType":"none" },
            "request":
            {
                "uri": "http://contoso.com/foo",        // required
                "method": "PUT",                        // required
                "body": "Posting from a timer",         // optional
                "headers":                              // optional

                {
                    "Content-Type": "application/json"
                },
            },
           "errorAction":
           {
               "type": "http",
               "request":
               {
                   "uri": "http://contoso.com/notifyError",
                   "method": "POST",
               },
           },
        },
        "recurrence":                                   // optional
        {
            "frequency": "week",                        // can be "year" "month" "day" "week" "minute"
            "interval": 1,                              // optional, how often to fire (default to 1)
            "schedule":                                 // optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]
            },
            "count": 10,                                 // optional (default to recur infinitely)
            "endTime": "2012-11-04",                     // optional (default to recur infinitely)
        },
        "state": "disabled",                           // enabled or disabled
        "status":                                       // controlled by Scheduler service
        {
            "lastExecutionTime": "2007-03-01T13:00:00Z",
            "nextExecutionTime": "2007-03-01T14:00:00Z ",
            "executionCount": 3,
                                                "failureCount": 0,
                                                "faultedCount": 0
        },
    }

Jak było widać w zadaniu zaplanowanym próbki powyżej, definicji zadania składa się z kilku segmentów:

- Czas rozpoczęcia ("czas rozpoczęcia")  

- Akcji (""), która zawiera błąd czynności ("errorAction")

- Cykl ("cyklu")  

- State "(stan)  

- Status "(stan)  

- Spróbuj ponownie zasad ("retryPolicy")  

Przeanalizujmy każdy z tych szczegółowo:

## <a name="starttime"></a>czas rozpoczęcia

"Czas rozpoczęcia" jest godzina rozpoczęcia i pozwala rozmówcy określić przesunięcie strefy czasowej w sieci w [formacie ISO 8601](http://en.wikipedia.org/wiki/ISO_8601).

## <a name="action-and-erroraction"></a>Akcja i errorAction

"Działania" jest akcja wywoływane w przypadku każdego wystąpienia i opisano rodzaj wywołania usługi. Akcja jest, co można wykonać podane zgodnie z harmonogramem. Harmonogram obsługuje HTTP, kolejki miejsca do magazynowania, usługa bus tematu i akcji w kolejce bus usługi.

Akcja w powyższym przykładzie jest akcji HTTP. Poniżej przedstawiono przykładowy akcji kolejki miejsca do magazynowania:

    {
            "type": "storageQueue",
            "queueMessage":
            {
                "storageAccount": "myStorageAccount",  // required
                "queueName": "myqueue",                // required
                "sasToken": "TOKEN",                   // required
                "message":                             // required
                    "My message body",
            },
    }

Poniżej przedstawiono przykładowy akcji tematu bus usługi.

  "działania": {"typ": "serviceBusTopic", "serviceBusTopicMessage": {"topicPath": "t1",  
      "przestrzeń nazw": "mySBNamespace", "atrybut transportType": "netMessaging" / / może być netMessaging lub AMQP "uwierzytelniania": {"sasKeyName": "QPolicy", "typ": "sharedAccessKey"}, "komunikat": "Wiadomość", "brokeredMessageProperties": {}, "customMessageProperties": {"argumentu": "FromScheduler"}},}

Poniżej przedstawiono przykładowy akcji kolejki bus usługi:


  "działania": {"serviceBusQueueMessage": {"NazwaKolejki": "1"  
      "przestrzeń nazw": "mySBNamespace", "atrybut transportType": "netMessaging" / / może być netMessaging lub AMQP "uwierzytelniania": {  
        "sasKeyName": "QPolicy", "typ": "sharedAccessKey"}, "komunikat": "Niektóre wiadomości"  
      "brokeredMessageProperties": {}, "customMessageProperties": {"argumentu": "FromScheduler"}}, "typ": "serviceBusQueue"}

"errorAction" jest obsługi błędów, akcja wywoływane w przypadku niepowodzenia główną akcję. Za pomocą tej zmiennej do punktu końcowego obsługi błędów zadzwonić lub wysłać powiadomienie użytkownika. To może być używany do osiągnięcia pomocniczej punktu końcowego w przypadku podstawowego nie jest dostępna (np. w przypadku awarii w witrynie punkt końcowy) lub może być używany do powiadamianie o błąd obsługi punktu końcowego. Tak samo jak akcja podstawowego Akcja błędu może być prostego lub złożonego logiczny oparte na inne akcje. Aby dowiedzieć się, jak utworzyć tokenu skojarzeń zabezpieczeń, zobacz [Tworzenie i używanie podpisu dostępu do udostępnionych](https://msdn.microsoft.com/library/azure/jj721951.aspx).

## <a name="recurrence"></a>Cykl

Cykl składa się z kilku segmentów:

- Częstotliwość: Jeden z minuta, godzina, dzień, tydzień, miesiąc, rok  

- : Interwał dla danej częstotliwości cyklu  

- Harmonogram określonej: Określ minut, godzin, dni tygodnia, miesięcy i monthdays cyklu  

- Liczba: Liczba wystąpień  

- Czas zakończenia: Brak zadań spowoduje wykonanie określony czas zakończenia  

Zadanie jest cykliczne, jeśli został cyklicznego obiektu, określonego w jego definicji JSON. Jeśli określono zarówno liczby, jak i zakończenia uznane jest reguła zakończenia nastąpi wcześniej.

## <a name="state"></a>Województwo

Stan zadania jest jednym z czterech wartości: włączone, wyłączone, ukończony lub wystąpił błąd. Możesz umieścić lub zadań poprawki tak, aby zaktualizuje ich tak, aby stan włączony lub wyłączony. Jeśli zadanie zostało wykonane lub wystąpił błąd, to stan końcowy, której nie można aktualizować (choć nadal można usunąć zadanie). Przykład właściwość stan jest następująca:


        "state": "disabled", // enabled, disabled, completed, or faulted
Zadania wykonane i błędowi są usuwane po 60 dni.

## <a name="status"></a>Stan

Po rozpoczęciu zadanie harmonogramu, zostaną zwrócone informacje o bieżącym stanie zadania. Ten obiekt nie jest można ustawić przez użytkownika — jest ustawiona przez system. Jednak go znajduje się w obiekcie Zadanie (a nie osobnych zasobów połączonych) tak, aby jedną można łatwo uzyskać stanu zadania.

Stan zadania zawiera czas wykonywania (jeśli istnieje), podczas następnego planowane wykonanie (w przypadku zadań w trakcie wykonywania) i liczba wykonanie zadania.

## <a name="retrypolicy"></a>retryPolicy

Jeśli zadanie harmonogramu nie powiedzie się, jest możliwe określenie zasad ponów próbę, aby określić, czy i w jaki sposób akcja jest po ponownych próbach. To zależy od obiektu **retryType** — jest ustawiony na **Brak** Jeśli zasady nie ponów próbę, jak pokazano powyżej. Ustaw ją na **stałe** , jeśli zasady ponów próbę.

Aby ustawić zasady ponów próbę, można określić dwa dodatkowe ustawienia: interwał ponawiania (**retryInterval**) i liczba prób (**retryCount**).

Interwał ponawiania określony z obiektem **retryInterval** jest interwał między kolejnymi próbami. Jego wartość domyślna to 30 sekund, można skonfigurować jego minimalny to 15 sekund, a wartość maksymalna jest 18 miesięcy. Zadania w obszarze zbiory bezpłatne zadania mają minimalną wartość można konfigurować 1 godzina.  Zdefiniowanej w formacie ISO 8601. Podobnie wartość argumentu liczba prób określono wraz z obiektem **retryCount** ; jest liczba powtórzeń próbie ponowienie próby. Wartość domyślną jest 4, a wartość maksymalna wynosi 20\. zarówno **retryInterval** i **retryCount** są opcjonalne. Podane są one domyślnie wartości, jeśli **retryType** jest ustawiona na **stałe** i nie są określone jawnie.

## <a name="see-also"></a>Zobacz też

 [Co to jest harmonogram?](scheduler-intro.md)

 [Wprowadzenie do korzystania z harmonogramu w portalu Azure](scheduler-get-started-portal.md)

 [Plany i rozliczenia w harmonogramie Azure](scheduler-plans-billing.md)

 [Sposoby tworzenia złożonych harmonogramów i zaawansowane cyklu z harmonogramem Azure](scheduler-advanced-complexity.md)

 [Odwołanie interfejsu API usługi REST harmonogram Azure](https://msdn.microsoft.com/library/mt629143)

 [Dokumentacja dotycząca Azure poleceń cmdlet środowiska PowerShell harmonogramu](scheduler-powershell-reference.md)

 [Azure Harmonogram dostępności i niezawodności](scheduler-high-availability-reliability.md)

 [Azure limity harmonogram, wartości domyślne i kody błędów](scheduler-limits-defaults-errors.md)

 [Azure uwierzytelniania ruchu wychodzącego harmonogramu](scheduler-outbound-authentication.md)
