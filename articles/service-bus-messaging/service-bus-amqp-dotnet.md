<properties 
    pageTitle="Usługi Bus z .NET i AMQP 1.0 | Microsoft Azure"
    description="Usługa Bus z .NET przy użyciu AMQP"
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
    ms.date="10/03/2016"
    ms.author="sethm" />

# <a name="using-service-bus-from-net-with-amqp-10"></a>Usługa Bus z .NET przy użyciu AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

## <a name="downloading-the-service-bus-sdk"></a>Pobieranie Bus usługi SDK

Obsługa AMQP 1.0 jest dostępna w usługi Bus SDK w wersji 2.1 lub nowszej. Upewnij się, że masz najnowszą wersję, pobierając bitów Bus usług z [NuGet][].

## <a name="configuring-net-applications-to-use-amqp-10"></a>Konfigurowanie aplikacji .NET umożliwia AMQP 1.0

Domyślnie biblioteka klienta usługi Bus .NET komunikuje się z usługą Bus usługi przy użyciu dedykowane protokołu opartego na protokole SOAP. Używać AMQP 1.0 zamiast domyślny protokół wymaga jawne konfigurację w parametrach połączenia Bus usługi opisane w następnej sekcji. Inną niż ta zmiana kod aplikacji jest zmieniany podczas korzystania z AMQP 1.0.

W bieżącej wersji istnieje kilka funkcji interfejsu API, które nie są obsługiwane w przypadku korzystania z AMQP. Te funkcje nieobsługiwane są widoczne później w sekcji [nieobsługiwane funkcje i ograniczenia różnice funkcjonalne](#unsupported-features-restrictions-and-behavioral-differences). Niektóre zaawansowane ustawienia konfiguracji mieć także inne znaczenie podczas korzystania z AMQP.

### <a name="configuration-using-appconfig"></a>Konfiguracja przy użyciu App.config

Jest zalecane dla aplikacji, aby użyć tego pliku App.config do przechowywania ustawień. W przypadku aplikacji usługi Bus App.config służy do przechowywania ustawień wartość Bus usługi **ConnectionString** . Przykładowy plik App.config jest następująca:

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <appSettings>
            <add key="Microsoft.ServiceBus.ConnectionString"
                 value="Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
        </appSettings>
    </configuration>

Wartość `Microsoft.ServiceBus.ConnectionString` ustawiono opcję Parametry połączenia Bus usług, które umożliwiają skonfigurowanie połączenia usługi Bus. Format jest w następujący sposób:

    Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp

Miejsce, w którym `[namespace]` i `SharedAccessKey` pochodzą z [Azure portal][]. Aby uzyskać więcej informacji zobacz [Wprowadzenie do usługi Bus kolejek][].

Gdy używasz AMQP, Dołącz parametry połączenia z `;TransportType=Amqp`. Ten zapis informuje Biblioteka klienta, aby nawiązać połączenie przy użyciu AMQP 1.0 Bus usługi.

## <a name="message-serialization"></a>Szeregowania wiadomości

Podczas korzystania z protokołem domyślnym domyślne zachowanie szeregowania Biblioteka klienta .NET jest typ [DataContractSerializer][] umożliwia szeregować wystąpieniem [BrokeredMessage][] transportu między Biblioteka klienta a usługą Bus usługi. Podczas korzystania z trybu transportu AMQP, Biblioteka klienta używa system typów AMQP szeregowania [wiadomości brokered][BrokeredMessage] do wiadomości AMQP. Ten szeregowania umożliwia wiadomość odebrana i interpretowany przez aplikację odbiorczą, potencjalnie uruchomionym na innej platformie, na przykład aplikacji Java, która korzysta z interfejsu API JMS w celu uzyskania dostępu Bus usługi.

Utworzenia wystąpienia [BrokeredMessage][] można udostępniać obiektu .NET parametru do konstruktora służyć jako treści wiadomości. Dla obiektów, które mogą być mapowane na typy pierwotne AMQP treści jest seryjny do typów danych AMQP. Jeśli obiekt nie można mapować bezpośrednio do pierwotnych typu AMQP; oznacza to, że typ niestandardowy zdefiniowane przez aplikację, a następnie obiekt jest seryjny przy użyciu [DataContractSerializer][]i bajtów seryjne są wysyłane w wiadomości AMQP danych.

W celu ułatwienia współdziałania z innymi klientami, użyj tylko te typy .NET, które można szeregować bezpośrednio na typy AMQP dla treści wiadomości. W poniższej tabeli przedstawiono te typy i odpowiadające im mapowanie systemu typu AMQP.

| Typ obiektu treści .NET          | Typ zamapowanych AMQP                          | Typ sekcji treści AMQP                                                                                                                                    |
|--------------------------------|-------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| wartość logiczna                           | wartość logiczna                                   | Wartość AMQP                                                                                                                                                |
| bajt                           | ubyte                                     | Wartość AMQP                                                                                                                                                |
| ushort                         | ushort                                    | Wartość AMQP                                                                                                                                                |
| uint                           | uint                                      | Wartość AMQP                                                                                                                                                |
| ulong                          | ulong                                     | Wartość AMQP                                                                                                                                                |
| sbyte                          | bajt                                      | Wartość AMQP                                                                                                                                                |
| krótki                          | krótki                                     | Wartość AMQP                                                                                                                                                |
| int                            | int                                       | Wartość AMQP                                                                                                                                                |
| długie                           | długie                                      | Wartość AMQP                                                                                                                                                |
| Przestaw                          | Przestaw                                     | Wartość AMQP                                                                                                                                                |
| podwójne                         | podwójne                                    | Wartość AMQP                                                                                                                                                |
| dziesiętna                        | decimal128                                | Wartość AMQP                                                                                                                                                |
| znak                           | znak                                      | Wartość AMQP                                                                                                                                                |
| Daty i godziny                       | Sygnatura czasowa                                 | Wartość AMQP                                                                                                                                                |
| Identyfikator GUID                           | UUID                                      | Wartość AMQP                                                                                                                                                |
| bajt]                         | format binarny                                    | Wartość AMQP                                                                                                                                                |
| ciąg                         | ciąg                                    | Wartość AMQP                                                                                                                                                |
| System.Collections.IList       | Lista                                      | Wartość AMQP: elementy zawarte w kolekcji można tylko te, które są definiowane w tej tabeli.                                                             |
| System.Array                   | Tablica                                     | Wartość AMQP: elementy zawarte w kolekcji można tylko te, które są definiowane w tej tabeli.                                                             |
| System.Collections.IDictionary | Mapa                                       | Wartość AMQP: elementy zawarte w kolekcji można tylko te, które są definiowane w tej tabeli. Uwaga: obsługiwane są tylko klucze ciągu.                        |
| Identyfikator URI                            | Opisane ciąg (zobacz poniższej tabeli) | Wartość AMQP                                                                                                                                                |
| DateTimeOffset                 | Opisane długi (zobacz poniższej tabeli)   | Wartość AMQP                                                                                                                                                |
| Przedziału czasu                       | Opisane długi (zapoznać się z następującymi)         | Wartość AMQP                                                                                                                                                |
| Strumień                         | format binarny                                    | Dane AMQP (może być wiele). Sekcje dane zawierają nieprzetworzonych Bajty odczytane z obiektu strumienia.                                                           |
| Innego obiektu                   | format binarny                                    | Dane AMQP (może być wiele). Zawiera seryjne binarną obiektu korzystającego z DataContractSerializer lub serializatora dostarczonym przez aplikację. |

| Typ .NET      | AMQP zamapowanych opisem typu                                                                                              | Notatki                   |
|----------------|-------------------------------------------------------------------------------------------------------------------------|-------------------------|
| Identyfikator URI            | `<type name=”uri” class=restricted source=”string”> <descriptor name=”com.microsoft:uri” /></type>`                       | Uri.AbsoluteUri         |
| DateTimeOffset | `<type name=”datetime-offset” class=restricted source=”long”> <descriptor name=”com.microsoft:datetime-offset” /></type>` | DateTimeOffset.UtcTicks |
| Przedziału czasu       | `<type name=”timespan” class=restricted source=”long”> <descriptor name=”com.microsoft:timespan” /></type> `              | TimeSpan.Ticks          |

## <a name="unsupported-features-restrictions-and-behavioral-differences"></a>Nieobsługiwane funkcje, ograniczenia i różnice funkcjonalne

Następujące funkcje interfejsu API usługi Bus .NET nie są obecnie obsługiwane podczas korzystania z AMQP:

-   Transakcje

-   Wysyłanie pocztą docelowy przeniesienia

Istnieją również pewne niewielkie różnice w zachowaniu interfejs API .NET Bus usługi, używając AMQP w porównaniu z protokołem domyślnym:

-   Właściwość [OperationTimeout][] jest ignorowana.

-   `MessageReceiver.Receive(TimeSpan.Zero)`jest zaimplementowana jako `MessageReceiver.Receive(TimeSpan.FromSeconds(10))`.

-   Wypełnić wiadomości tokeny lock jest możliwe tylko przez odbiorców wiadomości, które początkowo odebrane wiadomości.

## <a name="controlling-amqp-protocol-settings"></a>Kontrolowanie ustawień protokołu AMQP

Interfejsy API .NET uwidaczniane kilka ustawień, aby kontrolować zachowanie protokołu AMQP:

-   **MessageReceiver.PrefetchCount**: określa początkowe kredytu zastosowane do łącza. Wartość domyślna to 0.

-   **MessagingFactorySettings.AmqpTransportSettings.MaxFrameSize**: kontrolki maksymalnego rozmiaru ramki AMQP oferowanych podczas uzgadniania połączenia Otwórz czasu. Wartość domyślna to 65 536 bajtów.

-   **MessagingFactorySettings.AmqpTransportSettings.BatchFlushInterval**: Jeśli przeniesienia są batchable, ta wartość określa maksymalne opóźnienie wysyłania przepisy. Dziedziczona przez nadawcy i odbiorcy domyślnie. Poszczególne nadawcy i odbiorcy można zmienić domyślnego, czyli 20 milisekund.

-   **MessagingFactorySettings.AmqpTransportSettings.UseSslStreamSecurity**: Określa, czy połączenia AMQP zostały ustanowione za pośrednictwem połączenia SSL. Wartość domyślna to **PRAWDA**.

## <a name="next-steps"></a>Następne kroki

Chcesz dowiedzieć się więcej? Skorzystaj z poniższych łączy:

- [Omówienie usług Bus AMQP]
- [Obsługa AMQP 1.0 kolejki Bus usługi na partycje i tematy]
- [AMQP w Bus usługi dla systemu Windows Server]

  [Wprowadzenie do usługi Bus kolejki]: service-bus-dotnet-get-started-with-queues.md
  [DataContractSerializer]: https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  [Microsoft.ServiceBus.Messaging.MessagingFactory.AcceptMessageSession]: https://msdn.microsoft.com/library/azure/jj657638.aspx
  [Microsoft.ServiceBus.Messaging.MessagingFactory.CreateMessageSender(System.String,System.String)]: https://msdn.microsoft.com/library/azure/jj657703.aspx
  [OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
[NuGet]: http://nuget.org/packages/WindowsAzure.ServiceBus/
[Azure portal]: https://portal.azure.com
[Omówienie usług Bus AMQP]: service-bus-amqp-overview.md
[Obsługa AMQP 1.0 kolejki Bus usługi na partycje i tematy]: service-bus-partitioned-queues-and-topics-amqp-overview.md
[AMQP w Bus usługi dla systemu Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx
