<properties 
    pageTitle="Bus usługi przekazywania próbki omówienie | Microsoft Azure"
    description="Kategoryzuje i opisano Bus usługi przekazywania próbki z łączami do wszystkich."
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

# <a name="service-bus-relay-samples"></a>Przykłady przekazywania Bus usługi

Przykłady przekazywania Bus usługi wykazać kluczowe funkcje [przekazywania Bus usługi](https://azure.microsoft.com/services/service-bus/). W tym artykule kategoryzowania i opisano dostępne z łączami do każdej próbki.

>[AZURE.NOTE] Przykłady Bus usługa nie są instalowane z zestawu SDK. Aby uzyskać próbki, odwiedź [strony próbki Azure SDK](https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=5).
>
>Ponadto jest zaktualizowany zbiór Bus usługi przekazywania próbki [poniżej](https://github.com/Azure-Samples/azure-servicebus-relay-samples) (z tego artykułu, ta osoba nie zostały opisane w tym artykule).  

Dla wiadomości próbkach zobacz [Usługa Bus wiadomości próbki](../service-bus-messaging/service-bus-samples.md).

## <a name="service-bus-relay"></a>Usługa Bus przekazywania

Następujące próbki przedstawiają sposób pisać aplikacje, które korzystają z usługi przekazywania Bus usługi.

Należy zauważyć, że przykłady przekazywania wymagają parametry połączenia, aby uzyskać dostęp do nazw Bus usługi.

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

## <a name="service-bus-relay"></a>Usługa Bus przekazywania

Przykłady, ilustrujące przekazywania Bus usługi.

### <a name="getting-started"></a>Wprowadzenie

|Przykładowa nazwa|Opis|Zestaw SDK minimalna wersja|Dostępność|
|---|---|---|---|
|[Przekazywanie wiadomości: Azure](http://code.msdn.microsoft.com/Relayed-Messaging-Windows-0d2cede3)|Pokazuje, jak przeprowadzić klienta usługi Bus i usługi Azure. W tym przykładzie programowy konfiguruje Bus usługi. Tylko informacje środowiska i bezpieczeństwa są przechowywane w plikach konfiguracji.|1.8|Bus usługi Microsoft Azure|
|[Odnośnie uwierzytelnianie wiadomości: Wspólne hasło](http://code.msdn.microsoft.com/Relayed-Messaging-92b04c02)|Przedstawiono sposób użycia Nazwa wystawcy i wystawcy hasło do uwierzytelniania Bus usługi.|1.8|Bus usługi Microsoft Azure|
|[Przekazywanie wiadomości uwierzytelniania: WebNoAuth](http://code.msdn.microsoft.com/Relayed-Messaging-a4f0b831)|Przedstawiono sposób udostępnienia usługi HTTP, która nie wymaga uwierzytelniania użytkowników klienta.|1.8|Bus usługi Microsoft Azure|
|[Przekazywanie wiadomości powiązań: WebHttp](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-a6477ba0)|Przedstawiono sposób użycia powiązanie **WebHttpRelayBinding** zwraca dane binarne korzystanie z sieci Web, model programowania.|1.8|Bus usługi Microsoft Azure|
|[Odnośnie powiązania wiadomości: Przekazywanie NetTcp](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-2dec7692)|Przedstawiono sposób użycia powiązanie **NetTcpRelayBinding** .|1.8|Bus usługi Microsoft Azure|

### <a name="exploring-features"></a>Poznawanie funkcji

Przykładów pokazujących, jak różne funkcje przekazywania Bus usługi.

|Przykładowa nazwa|Opis|Zestaw SDK minimalna wersja|Dostępność|
|---|---|---|---|
|[Odnośnie uwierzytelnianie wiadomości: Proste WebToken](http://code.msdn.microsoft.com/Relayed-Messaging-32c74392)|Przedstawiono sposób użycia poświadczenie token prostej sieci web do uwierzytelniania Bus usługi. Próbka jest podobna do próbki Echo kilka zmian. W szczególności w tym przykładzie dodaje zachowanie w aplikacjach ChannelFactory (klient) i ServiceHost (usługa).|1.8|Bus usługi Microsoft Azure|
|[Odnośnie wiadomości: Równoważenia obciążenia](http://code.msdn.microsoft.com/Relayed-Messaging-Load-bd76a9f8)|Pokazuje, jak za pomocą programu Microsoft Azure usługi Bus przesyłania wiadomości do wielu odbiorców. Jest wyświetlany w wielu wystąpień usługi proste komunikowanie się za pomocą klienta za pośrednictwem powiązanie **NetTcpRelayBinding**|1.8|Bus usługi Microsoft Azure|
|[Odnośnie powiązania wiadomości: Zdarzenia netto](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-c0176977)|Pokazuje, za pomocą tego powiązania **NetEventRelayBinding** na Bus usługi Azure firmy Microsoft.|1.8|Bus usługi Microsoft Azure|
|[Odnośnie powiązania wiadomości: Sesji WS2007Http](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-ef1f1fcb)|Zaprezentowano, za pomocą tego powiązania **WS2007HttpRelayBinding** z sesjami niezawodne włączone. Pokazuje także określania poświadczeń usługi Bus w pliku konfiguracji zamiast programowy.|1.8|Bus usługi Microsoft Azure|
|[Odnośnie powiązania wiadomości: MsgSecCertificate WS2007Http](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-f29c9da5)|Pokazuje, jak za pomocą powiązanie **WS2007HttpRelayBinding** z zabezpieczeniami wiadomości i zabezpieczanie wiadomości zakończenia do końca podczas nadal wymagające klientom uwierzytelnianie przy użyciu usługi Bus.|1.8|Bus usługi Microsoft Azure|
|[Odnośnie wiadomości: Wymiany metadanych](http://code.msdn.microsoft.com/Relayed-Messaging-Metadata-f122312e)|Przedstawiono sposób udostępniania punkt końcowy metadanych, korzystającego z powiązanie przekazywania. **MetadataExchange** jest obsługiwana w następujących powiązań przekazywania: **NetTcpRelayBinding**, **NetOnewayRelayBinding**, **BasicHttpRelayBinding**i **WS2007HttpRelayBinding**.|1.8|Bus usługi Microsoft Azure|
|[Odnośnie powiązania wiadomości: Bezpośrednie NetTcp](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-ca039161)|Pokazuje, jak skonfigurować powiązanie **NetTcpRelayBinding** do obsługi **hybrydowych** tryb połączenia, który najpierw nawiąże połączenie odnośnie i, jeśli to możliwe, przełączy się automatycznie do bezpośredniego połączenia między klientem a usługą.|1.8|Bus usługi Microsoft Azure|
|[Odnośnie powiązania wiadomości: Nazwa użytkownika MsgSec NetTcp](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-30542392)|Przedstawiono sposób użycia powiązanie **NetTcpRelayBinding** z zabezpieczeniami wiadomości.|1.8|Bus usługi Microsoft Azure|
|[Odnośnie powiązania wiadomości: Jednokierunkowej netto](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-bb5b813a)|Przedstawiono sposób udostępniania i używanie punktu końcowego usługi za pomocą tego powiązania **NetOnewayRelayBinding** .|1.8|Bus usługi Microsoft Azure|
|[Odnośnie powiązania wiadomości: Proste WS2007Http](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-aa4b793a)|Pokazuje, za pomocą tego powiązania **WS2007HttpRelayBinding** . Jednak przedstawia prosty usługa, która korzysta z Brak opcji zabezpieczeń i nie wymaga klientom uwierzytelnianie.|1.8|Bus usługi Microsoft Azure|

## <a name="next-steps"></a>Następne kroki

Zobacz następujące tematy dla koncepcyjny omówienia Bus usługi.

- [Omówienie przekaźnika Bus usługi](service-bus-relay-overview.md)
- [Architektura Bus usługi](../service-bus-messaging/service-bus-architecture.md)
- [Podstawy Bus usługi](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)