<properties 
    pageTitle="Wiadomości usługi Bus próbki omówienie | Microsoft Azure"
    description="Kategoryzuje i opisano Bus usługi próbek z łączami do wszystkich wiadomości."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/07/2016"
    ms.author="sethm" />

# <a name="service-bus-messaging-samples"></a>Bus usługi SMS próbki

Przykłady wiadomości Bus usługi wykazać najważniejszych funkcji w [wiadomości Service Bus](https://azure.microsoft.com/services/service-bus/) (usługa w chmurze) i [Bus usługi dla systemu Windows Server](https://msdn.microsoft.com/library/dn282144.aspx). W tym artykule kategoryzuje i opisano dostępne z łączami do każdej próbki.

>[AZURE.NOTE] Przykłady Bus usługa nie są instalowane z zestawu SDK. Aby uzyskać próbki, odwiedź [strony próbki Azure SDK](https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=5).
>
>Ponadto jest zaktualizowany zbiór Bus usługi wiadomości próbki [poniżej](https://github.com/Azure-Samples/azure-servicebus-messaging-samples) (z tego artykułu, ta osoba nie zostały opisane w tym artykule).  

Dla próbki przekazywania zobacz [Usługa Bus przekazywania próbki](../service-bus-relay/service-bus-relay-samples.md).

## <a name="service-bus-messaging"></a>Usługa Bus wiadomości

Poniższych przykładach przedstawiono jak napisać aplikacje, które używają Bus usługi wiadomości.

Należy zauważyć, że wiadomości przykłady wymagają parametry połączenia, aby uzyskać dostęp do nazw Bus usługi.

### <a name="to-obtain-a-connection-string-for-azure-service-bus"></a>Aby uzyskać parametry połączenia dla Bus usługi Azure

1. Zaloguj się do [portalu Azure](http://portal.azure.com).

1. W kolumnie po lewej stronie kliknij **Bus usługi**.

1. Kliknij nazwę obszaru nazw na liście.

3. W karta nazw kliknij **zasady dostępu udostępnione**.

4. Karta **zasady dostępu udostępnione** kliknij **RootManageSharedAccessKey**.

6. Skopiuj parametry połączenia do Schowka.

### <a name="to-obtain-a-connection-string-for-service-bus-for-windows-server"></a>Aby uzyskać parametry połączenia dla usługi Bus dla systemu Windows Server

1. Uruchom następujące polecenie cmdlet programu PowerShell:

    ```
    get-sbClientConfiguration
    ```

2. Wklej parametry połączenia w pliku App.config dla próbki.

### <a name="getting-started-samples"></a>Wprowadzenie wprowadzenie próbki

Te przykłady opisano podstawowe funkcje wiadomości.

|Przykładowa nazwa|Opis|Zestaw SDK minimalna wersja|Dostępność|
|---|---|---|---|
|[Wprowadzenie: Wiadomości z kolejki](http://code.msdn.microsoft.com/Getting-Started-Brokered-aa7a0ac3)|Pokazuje, jak za pomocą programu Microsoft Azure usługi Bus wysyłania i odbierania wiadomości z kolejki.|1.8|Bus usługi Microsoft Azure; Bus usługi dla systemu Windows Server|
|[Wprowadzenie: Wiadomości z tematów](http://code.msdn.microsoft.com/Getting-Started-Brokered-614d42e5)|Pokazuje, jak za pomocą programu Microsoft Azure usługi Bus wysyłanie i odbieranie wiadomości z tematu zawierającego wiele subskrypcji.|1.8|Bus usługi Microsoft Azure; Bus usługi dla systemu Windows Server|

### <a name="exploring-features"></a>Poznawanie funkcji

Następujące przykładów pokazano różne funkcje usługi Bus.

|Przykładowa nazwa|Opis|Zestaw SDK minimalna wersja|Dostępność|
|---|---|---|---|
|[Dostawców HTTP Token](http://code.msdn.microsoft.com/Service-Bus-HTTP-Token-38f2cfc5)|Pokazano różne sposoby uwierzytelniania protokołu HTTP/RESZTA klientowi Bus usługi.|2.1|Bus usługi Microsoft Azure; Bus usługi dla systemu Windows Server|
|[Usługa Bus HTTP klienta](http://code.msdn.microsoft.com/Service-Bus-HTTP-client-fe7da74a)|Pokazuje, jak wiadomości, aby wysyłać i odbierać wiadomości z usługi Bus za pośrednictwem protokołu HTTP/pozostałych.|2.3|Bus usługi Microsoft Azure; Bus usługi dla systemu Windows Server|
|[Usługa Bus Autoforwarding](http://code.msdn.microsoft.com/Service-Bus-Autoforwarding-b9df470b)|Pokazuje, jak automatyczne przesyłanie dalej wiadomości z kolejki, subskrypcji lub kolejka utraconych wiadomości do innej kolejki lub temat. Ilustruje też sposób wysłać wiadomość do kolejki lub tematu za pośrednictwem kolejki przełączania.|2.3|Bus usługi Microsoft Azure; Bus usługi dla systemu Windows Server|
|[Brokered wiadomości: Przykładowe sesji kanału WCF](http://code.msdn.microsoft.com/Brokered-Messaging-WCF-0a526451)|Przedstawiono sposób użycia Bus usługi Microsoft Azure z za pomocą kanałów Windows Communication Foundation (WCF). Pokazano stosowania WCF kanałów, aby wysyłać i odbierać wiadomości przy użyciu kolejki Bus usługi. Próbki przedstawia zarówno sesji i innych niż sesji komunikacji za pośrednictwem Bus usługi.|1.8|Bus usługi Microsoft Azure; Bus usługi dla systemu Windows Server|
|[Brokered wiadomości: transakcje](http://code.msdn.microsoft.com/Brokered-Messaging-8cd41d1e)|Przedstawiono sposób użycia Bus usługi Microsoft Azure wiadomości funkcje w zakresie transakcji w celu zapewnienia, że partii wiadomości operacji są Projekt zatwierdzony — atomically.|1.8|Bus usługi Microsoft Azure; Bus usługi dla systemu Windows Server|
|[Brokered wiadomości: Za pomocą usługi REST operacji zarządzania](http://code.msdn.microsoft.com/Brokered-Messaging-569cff88)|Przedstawiono sposób wykonywania operacji zarządzania na Bus usługi za pomocą usługi REST.|1.8|Bus usługi Microsoft Azure; Bus usługi dla systemu Windows Server|
|[Dostawcy zasobów interfejsy API REST](http://code.msdn.microsoft.com/Service-Bus-Resource-5d887203)|Pokazuje, jak przy użyciu nowych usług Bus RDFE pozostałych interfejsów API Zarządzanie nazw i jednostki wiadomości.|1.8|Bus usługi Microsoft Azure; Bus usługi dla systemu Windows Server|
|[Brokered wiadomości: Przykładowe sesji usługa WCF](http://code.msdn.microsoft.com/Brokered-Messaging-WCF-db4262c2)|Przedstawiono sposób użycia Bus usługi Azure firmy Microsoft przy użyciu modelu Usługa WCF. Pokazano stosowania modelu Usługa WCF do realizacji komunikacji za pośrednictwem kolejki Bus usługi opartej na sesji.|1.8|Bus usługi Microsoft Azure; Bus usługi dla systemu Windows Server|
|[Brokered wiadomości: Odpowiedź żądania](http://code.msdn.microsoft.com/Brokered-Messaging-Request-2b4ff5d8)|Pokazuje, jak używać Bus usługi Microsoft Azure i funkcji żądanie/odpowiedź. Próbka zawiera proste klienci i serwery komunikowanie się za pośrednictwem kolejki Bus usługi.|1.8|Bus usługi Microsoft Azure; Bus usługi dla systemu Windows Server|
|[Brokered wiadomości: kolejka utraconych wiadomości](http://code.msdn.microsoft.com/Brokered-Messaging-Dead-22536dd8)|Pokazuje, jak używać programu Microsoft Azure usługi Bus i funkcji "kolejki utraconych wiadomości" wiadomości. Próbka zawiera proste nadawcy i odbiorcy komunikowanie się za pośrednictwem kolejki Bus usługi.|1.8|Bus usługi Microsoft Azure; Bus usługi dla systemu Windows Server|
|[Brokered wiadomości: Wstrzymana wiadomości](http://code.msdn.microsoft.com/Brokered-Messaging-ccc4f879)|Pokazuje, jak korzystać z funkcji odroczenie wiadomości programu Microsoft Azure usługi Bus. Próbka zawiera proste nadawcy i odbiorcy komunikowanie się za pośrednictwem kolejki Bus usługi.|1.8|Bus usługi Microsoft Azure; Bus usługi dla systemu Windows Server|
|[Brokered wiadomości: Wiadomości sesji](http://code.msdn.microsoft.com/Brokered-Messaging-Session-41c43fb4)|Pokazuje, jak używać programu Microsoft Azure usługi Bus i funkcji sesji wiadomości. Próbka zawiera proste nadawcy i odbiorcy komunikowanie się za pośrednictwem kolejki Bus usługi.|1.8|Bus usługi Microsoft Azure; Bus usługi dla systemu Windows Server|
|[Brokered wiadomości: Temat odpowiedź żądania](http://code.msdn.microsoft.com/Brokered-Messaging-Request-6759a36e)|Przedstawiono sposób zaimplementowania deseniem żądanie/odpowiedź za pomocą Microsoft Azure usługi Bus tematy i subskrypcje. Próbka zawiera proste klientów i serwery komunikacja za pośrednictwem tematu Bus usługi.|1.8|Bus usługi Microsoft Azure; Bus usługi dla systemu Windows Server|
|[Brokered wiadomości: Kolejka odpowiedzi żądania](http://code.msdn.microsoft.com/Brokered-Messaging-Request-0ce8fcaf)|Pokazuje, jak używać programu Microsoft Azure usługi Bus i funkcji żądanie/odpowiedź. Próbka zawiera proste klientów i serwerów komunikowania się przez dwie kolejki Bus usługi.|1.8|Bus usługi Microsoft Azure; Bus usługi dla systemu Windows Server|
|[Brokered wiadomości: Wykrywanie duplikatów](http://code.msdn.microsoft.com/Brokered-Messaging-c0acea25)|Przedstawiono sposób użycia Microsoft Azure usługi Bus wykrywania zduplikowanych wiadomości z kolejek. Tworzy dwie kolejki, osoba używająca wykrywania duplikatów włączone i drugi bez wykrywania duplikatów.|1.8|Bus usługi Microsoft Azure; Bus usługi dla systemu Windows Server|
|[Brokered wiadomości: Wiadomości asynchroniczne](http://code.msdn.microsoft.com/Brokered-Messaging-Async-211c1e74)|Pokazuje, jak za pomocą programu Microsoft Azure usługi Bus wysyłanie i odbieranie wiadomości asynchroniczne z kolejki. Kolejka znajdują się rozłączone, asynchroniczny komunikacji między nadawcy i dowolnej liczby odbiorców (tutaj pojedynczego odbiorcy).|1.8|Bus usługi Microsoft Azure; Bus usługi dla systemu Windows Server|
|[Brokered wiadomości: Filtry zaawansowane](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749)|Pokazuje, jak używać programu Microsoft Azure usługi Bus publikowania/subskrypcji filtry zaawansowane. Tworzy tematu i subskrypcje 3 z definicjami inny filtr, wysyła wiadomości do tematu i odbiera wszystkich wiadomości z subskrypcji.|1.8|Bus usługi Microsoft Azure; Bus usługi dla systemu Windows Server|
|[Brokered wiadomości: Pobieranie z wyprzedzeniem wiadomości](http://code.msdn.microsoft.com/Brokered-Messaging-be2dac1d)|Pokazuje, jak korzystać z funkcji Microsoft Azure usługi Bus wiadomości pobrane z wyprzedzeniem. Go pokazuje, jak korzystać z funkcji pobrane z wyprzedzeniem wiadomości na Odbierz.|1.8|Bus usługi Microsoft Azure; Bus usługi dla systemu Windows Server|

## <a name="service-bus-reference-tools"></a>Usługa Bus narzędzi odwołań

Następujące przykładów pokazano różne funkcje usługi.

|Przykładowa nazwa|Opis|Zestaw SDK minimalna wersja|Dostępność|
|---|---|---|---|
|[Usługa Bus Eksploratora](http://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)|Eksplorator Bus usługi umożliwia użytkownikom nawiązywanie połączenia z nazw usługi Bus usługi i zarządzanie nimi podmioty wiadomości w łatwy sposób. Narzędzie udostępnia zaawansowanych funkcji, takich jak funkcje Importuj/Eksportuj i możliwość przetestowania podmioty wiadomości i usług przekazywania.|1.8|Bus usługi Microsoft Azure; Bus usługi dla systemu Windows Server|
|[Autoryzacja: SBAzTool](http://code.msdn.microsoft.com/Authorization-SBAzTool-6fd76d93)|W przykładzie pokazano sposób tworzenia i Zarządzanie tożsamościami usługi w Microsoft Azure Active Directory kontroli dostępu (nazywane także usługa Kontrola dostępu lub ACS) do użytku z Bus usługi.|N/D!|Bus usługi Microsoft Azure|

## <a name="next-steps"></a>Następne kroki

Zobacz następujące tematy dla koncepcyjny omówienia Bus usługi.

- [Omówienie wiadomości Bus usługi](service-bus-messaging-overview.md)
- [Architektura Bus usługi](service-bus-architecture.md)
- [Podstawy Bus usługi](service-bus-fundamentals-hybrid-solutions.md)
