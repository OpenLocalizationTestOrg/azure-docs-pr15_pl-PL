<properties 
    pageTitle="Jak rejestrowanie zdarzeń w Azure koncentratory zdarzenia w zarządzaniu interfejsu API Azure | Microsoft Azure" 
    description="Dowiedz się, jak rejestrowanie zdarzeń w Azure koncentratory zdarzenia w zarządzaniu interfejsu API Azure." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-log-events-to-azure-event-hubs-in-azure-api-management"></a>Jak rejestrowanie zdarzeń w Azure koncentratory zdarzenia w zarządzaniu interfejsu API platformy Azure

Azure koncentratory zdarzenia to usługa ingress wysoce skalowalna danych, która może mogły zjeść tej ostatniej miliony wydarzeń na sekundę, aby mogła procesu i analizowanie dużych ilości danych tworzone przez połączenia urządzeń i aplikacji. Koncentratory zdarzenie działa jako "drzwi wejściowe" dla zdarzenia planowana i gdy dane są zbierane do koncentratora zdarzenia, może zostać zamieniony i przechowywane za pomocą dowolnego dostawcy analizy w czasie rzeczywistym lub karty tworzeniu partii magazynowania. Koncentratory zdarzenia oddzielono produkcji strumienia zdarzeń ze zużycia te zdarzenia, aby umożliwić dostęp wydarzeń na własne harmonogram odbiorców zdarzeń.

Ten artykuł jest companion do pliku wideo [Integracja zarządzania interfejsu API Azure za pomocą koncentratorów zdarzenia](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) i opisano sposób logowania zdarzeń interfejsu API zarządzania za pomocą koncentratorów zdarzenia Azure.

## <a name="create-an-azure-event-hub"></a>Tworzenie Centrum Azure zdarzenia

Aby utworzyć nowe Centrum zdarzenia, zaloguj się do [portal Azure klasycznym](https://manage.windowsazure.com) i kliknij pozycję **Nowy**->**Aplikacji usług**->**Bus usługi**->**Centrum zdarzenia**->**Szybkiego tworzenia**. Wprowadź nazwę zdarzenia Centrum regionu, wybierz subskrypcję, a następnie wybierz pozycję przestrzeń nazw. Jeśli nie utworzono jeszcze przestrzeń nazw można utworzyć, wpisując nazwę w polu tekstowym **Namespace** . Skonfigurowane wszystkie właściwości kliknij pozycję **Utwórz nowe Centrum zdarzeń** Tworzenie Centrum zdarzeń.

![Tworzenie Centrum zdarzenia][create-event-hub]

Następnie przejdź do karty **Konfiguruj** dla swojego nowego koncentratora zdarzenia i Utwórz dwie **zasady dostępu udostępnione**. Nadaj nazwę pierwszy z nich **Wysyłanie** i nadaj mu uprawnień **Wyślij** .

![Wysyłanie zasad][sending-policy]

Nadaj nazwę drugiemu **otrzymywania**, nadaj uprawnienia **odsłuchać** i kliknij przycisk **Zapisz**.

![Odbiera zasady][receiving-policy]

Każdej zasady udostępniania umożliwia aplikacjom na wysyłanie i odbieranie zdarzeń do i z poziomu Centrum zdarzenia. Aby wyświetlić parametry połączenia dla tych zasad, przejdź na kartę **pulpit nawigacyjny** Centrum zdarzenia, a następnie kliknij przycisk **informacje o połączeniu**.

![Parametry połączenia][event-hub-dashboard]

**Wysyłanie** parametry połączenia jest używana podczas rejestrowania zdarzeń, a parametry połączenia **otrzymywania** jest używana podczas pobierania wydarzenia z poziomu Centrum zdarzenia.

![Parametry połączenia][event-hub-connection-string]

## <a name="create-an-api-management-logger"></a>Tworzenie rejestratora interfejsu API zarządzania

Teraz, gdy masz koncentratora zdarzenia, następnym krokiem jest skonfigurowanie [rejestratora](https://msdn.microsoft.com/library/azure/mt592020.aspx) w usłudze zarządzania interfejsu API tak, aby go rejestrowanie zdarzeń w Centrum zdarzeń.

Interfejs API zarządzania rejestratory są skonfigurowane przy użyciu [Interfejsu API usługi REST zarządzania interfejsu API](http://aka.ms/smapi). Przed użyciem interfejsu API usługi REST po raz pierwszy, przejrzyj [Warunki wstępne](https://msdn.microsoft.com/library/azure/dn776326.aspx#Prerequisites) , a następnie upewnij się, że masz [włączony dostęp do interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/dn776326.aspx#EnableRESTAPI).

Aby utworzyć rejestratora, należy żądania HTTP umieszczenie przy użyciu następującego szablonu adresu URL.

    https://{your service}.management.azure-api.net/loggers/{new logger name}?api-version=2014-02-14-preview

-   Zamienianie `{your service}` o nazwie wystąpienia usługi zarządzania interfejsu API.
-   Zamienianie `{new logger name}` z nazwą odpowiedniej dla Twojej nowego rejestratora. Po skonfigurowaniu zasad [dziennika do eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) będzie odwoływać się ta nazwa

Dodaj następujące nagłówki na żądanie.

-   Typ zawartości: aplikacja json
-   Zezwolenie: SharedAccessSignature uid =...
    -   Aby uzyskać instrukcje dotyczące generowania `SharedAccessSignature` zobacz [Azure interfejsu API zarządzania pozostałych interfejsu API uwierzytelniania](https://msdn.microsoft.com/library/azure/dn798668.aspx).

Określanie zawartości żądania przy użyciu następującego szablonu.

    {
      "type" : "AzureEventHub",
      "description" : "Sample logger description",
      "credentials" : {
        "name" : "Name of the Event Hub from the Azure Classic Portal",
        "connectionString" : "Endpoint=Event Hub Sender connection string"
        }
    }

-   `type`musi być ustawiona na `AzureEventHub`.
-   `description`zawiera opcjonalny opis rejestratora i może być ciągiem o zerowej długości, jeśli to konieczne.
-   `credentials`zawiera `name` i `connectionString` z Twoim Centrum zdarzeń Azure.

Po dokonaniu żądanie, jeśli rejestratora został utworzony kod stanu `201 Created` jest zwracana. 

>[AZURE.NOTE] Dla innych możliwych kodów zwrotnych oraz ich powodów zobacz [Tworzenie rejestratora](https://msdn.microsoft.com/library/azure/mt592020.aspx#PUT). Aby sprawdzić, jak wykonywać inne zadania, takie jak listy, aktualizowanie i usuwanie, zapoznaj się z dokumentacją jednostki [rejestratora](https://msdn.microsoft.com/library/azure/mt592020.aspx) .

## <a name="configure-log-to-eventhubs-policies"></a>Konfigurowanie zasad dziennika do eventhubs

Po skonfigurowaniu usługi rejestratora w zarządzaniu interfejsu API można skonfigurować zasad dziennika do eventhubs do rejestrowania zdarzeń odpowiednie. Zasady dziennika do eventhubs może służyć w sekcji zasady ruchu przychodzącego lub sekcji zasady ruchu wychodzącego.

Aby skonfigurować zasady, zaloguj się do [portalu klasyczny Azure](https://manage.windowsazure.com)Przejdź usługi zarządzania interfejsu API i kliknij **portalu programu publisher** lub **Zarządzaj** , aby uzyskać dostęp do portalu programu publisher.

![Portal programu Publisher][publisher-portal]

W menu zarządzania interfejsu API po lewej stronie kliknij polecenie **zasady** , wybierz żądany produkt i interfejsu API i kliknij pozycję **Dodaj zasady**. W tym przykładzie możemy dodawania zasady **Interfejsu API Echo** w produkcie **bez ograniczeń** .

![Dodawanie zasad][add-policy]

Umieść kursor w `inbound` zasad sekcji, a następnie kliknij pozycję zasad **dziennika EventHub** , aby wstawić `log-to-eventhub` szablonu deklaracji zasad.

![Edytor zasad][event-hub-policy]

    <log-to-eventhub logger-id ='logger-id'>
      @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
    </log-to-eventhub>

Zamienianie `logger-id` o nazwie rejestratora interfejsu API zarządzania skonfigurowany w poprzednim kroku.

Można użyć dowolnego wyrażenia, która zwraca ciąg jako wartość `log-to-eventhub` elementu. W tym przykładzie ciąg zawierający daty i godziny, nazwa usługi, identyfikator żądania, adres ip żądania i nazwa operacji jest rejestrowany.

Kliknij przycisk **Zapisz** zapisywania konfiguracji zasad zaktualizowane. Zaraz po zapisaniu go zasadę jest aktywna, a zdarzenia są rejestrowane w wyznaczonych Centrum zdarzenia.

## <a name="next-steps"></a>Następne kroki

-   Dowiedz się więcej o koncentratory zdarzenia Azure
    -   [Wprowadzenie do koncentratorów zdarzenia Azure](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
    -   [Odbieranie wiadomości z EventProcessorHost](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost)
    -   [Koncentratory zdarzenia programowania przewodnika](../event-hubs/event-hubs-programming-guide.md)
-   Dowiedz się więcej o integracji interfejsu API zarządzania i koncentratory zdarzenia
    -   [Odwołanie do rejestratora jednostki](https://msdn.microsoft.com/library/azure/mt592020.aspx)
    -   [Odwołanie do zasad dziennika do eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
    -   [Monitorowanie usługi interfejsy API zarządzania Azure interfejsu API, koncentratory zdarzenia i Runscope](api-management-log-to-eventhub-sample.md)    

## <a name="watch-a-video-walkthrough"></a>Obejrzyj klip wideo Instruktaż

> [AZURE.VIDEO integrate-azure-api-management-with-event-hubs]


[publisher-portal]: ./media/api-management-howto-log-event-hubs/publisher-portal.png
[create-event-hub]: ./media/api-management-howto-log-event-hubs/create-event-hub.png
[event-hub-connection-string]: ./media/api-management-howto-log-event-hubs/event-hub-connection-string.png
[event-hub-dashboard]: ./media/api-management-howto-log-event-hubs/event-hub-dashboard.png
[receiving-policy]: ./media/api-management-howto-log-event-hubs/receiving-policy.png
[sending-policy]: ./media/api-management-howto-log-event-hubs/sending-policy.png
[event-hub-policy]: ./media/api-management-howto-log-event-hubs/event-hub-policy.png
[add-policy]: ./media/api-management-howto-log-event-hubs/add-policy.png






