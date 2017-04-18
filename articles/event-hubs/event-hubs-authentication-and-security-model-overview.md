<properties 
    pageTitle="Omówienie koncentratory zdarzeń uwierzytelniania i model zabezpieczeń | Microsoft Azure"
    description="Zdarzenia koncentratory uwierzytelniania i zabezpieczeń omówienie modelu."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/16/2016"
    ms.author="sethm;clemensv" />

# <a name="event-hubs-authentication-and-security-model-overview"></a>Zdarzenia koncentratory uwierzytelniania i zabezpieczeń omówienie modelu

Model zabezpieczeń koncentratory zdarzenia spełnia następujące wymagania:

- Tylko urządzenia, które są dostępne prawidłowe poświadczenia przesyłać dane do koncentratora zdarzenia.
- Urządzenie nie personifikacji innego urządzenia.
- Możliwe jest zablokowanie urządzenia rozpoczęcie wysyłanie danych do koncentratora zdarzenia.

## <a name="device-authentication"></a>Uwierzytelnianie urządzenia

Model zabezpieczeń koncentratory zdarzenie jest oparty na kombinacji tokeny [Udostępniane podpis programu Access (SA)](../service-bus-messaging/service-bus-shared-access-signature-authentication.md) i *wydawcy zdarzenia*. Zdarzenie programu publisher definiuje wirtualną punkt końcowy dla Centrum zdarzenia. Wydawcy tylko może służyć do wysyłania wiadomości do koncentratora zdarzenia. Nie istnieje możliwość otrzymywania wiadomości od wydawcy.

Zazwyczaj koncentratora zdarzenia wykorzystuje jednego wydawcy na urządzenie. Wszystkich wiadomości, które są wysyłane do dowolnego z tych wydawców koncentratora zdarzenia są dodawane do kolejki w koncentratora zdarzenia. Wydawcy włączyć kontrola dostępu szerokiego i ograniczanie.

Każde urządzenie jest przypisany tokenu unikatowe przekazaniu do urządzenia. Tokeny wyprodukowano w taki sposób, że każdy token unikatowe udziela dostępu do innego wydawcy unikatowe. Urządzenie, które ma token można wysyłać tylko do jednego wydawcy, ale nie programu publisher. Jeśli wielu urządzeń Udostępnij niż, każde z tych urządzeń udostępnia wydawcy.

Chociaż nie jest to zalecane, prawdopodobnie wyposażenie urządzeń z tokeny udzielanie bezpośredni dostęp do Centrum zdarzenia. Dowolnego urządzenia, w którym znajduje się ten token może wysyłać wiadomości bezpośrednio do koncentratora zdarzenia. Urządzenie takie nie podlega ograniczania. Ponadto urządzenie nie może być na liście zabronionych numerów wysyłanie do koncentratora zdarzenia.

Wszystkie tokeny są zalogowani przy użyciu klucza skojarzeń zabezpieczeń. Zazwyczaj wszystkie tokeny są zalogowani za pomocą tego samego klucza. Urządzenia nie wiedzą o istnieniu istotnych klucza; Dzięki temu urządzeń produkcyjnych tokeny.

### <a name="create-the-sas-key"></a>Tworzenie skojarzeń zabezpieczeń klucza

Podczas tworzenia nazw koncentratory zdarzenia, koncentratory zdarzenia Azure generuje klucz skojarzeń zabezpieczeń 256-bitowy o nazwie **RootManageSharedAccessKey**. Ten klawisz dotacji wysyłanie, odsłuchać i Zarządzanie prawami do obszaru nazw. Możesz utworzyć dodatkowe klawisze. Zalecane jest generowane klucz, że dotacje wysyłać uprawnienia do określonych Centrum zdarzenia. Do końca tematu, to zakłada, że o nazwie ten klucz `EventHubSendKey`.

W poniższym przykładzie jest tworzony klucz tylko do wysyłania podczas tworzenia Centrum zdarzeń:

```
// Create namespace manager.
string serviceNamespace = "YOUR_NAMESPACE";
string namespaceManageKeyName = "RootManageSharedAccessKey";
string namespaceManageKey = "YOUR_ROOT_MANAGE_SHARED_ACCESS_KEY";
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, string.Empty);
TokenProvider td = TokenProvider.CreateSharedAccessSignatureTokenProvider(namespaceManageKeyName, namespaceManageKey);
NamespaceManager nm = new NamespaceManager(namespaceUri, namespaceManageTokenProvider);

// Create Event Hub with a SAS rule that enables sending to that Event Hub
EventHubDescription ed = new EventHubDescription("MY_EVENT_HUB") { PartitionCount = 32 };
string eventHubSendKeyName = "EventHubSendKey";
string eventHubSendKey = SharedAccessAuthorizationRule.GenerateRandomKey();
SharedAccessAuthorizationRule eventHubSendRule = new SharedAccessAuthorizationRule(eventHubSendKeyName, eventHubSendKey, new[] { AccessRights.Send });
ed.Authorization.Add(eventHubSendRule); 
nm.CreateEventHub(ed);
```

### <a name="generate-tokens"></a>Generowanie tokenów

Istnieje możliwość wygenerowania tokeny przy użyciu klucza skojarzeń zabezpieczeń. Musi mieć tylko jeden token na urządzenie. Tokeny można następnie wyprodukowano, korzystając z poniższej metody. Wszystkie tokeny są generowane przy użyciu klucza **EventHubSendKey** . Każdy token zostanie przypisany unikatowy identyfikator URI.

```
public static string SharedAccessSignatureTokenProvider.GetSharedAccessSignature(string keyName, string sharedAccessKey, string resource, TimeSpan tokenTimeToLive)
```

W wywołaniu tej metody, identyfikator URI powinny być określone jako `//<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`. Dla wszystkich tokenów identyfikator URI jest identyczny z wyjątkiem produktów `PUBLISHER_NAME`, powinny być różne na poszczególnych token. Najlepiej, jeśli `PUBLISHER_NAME` to identyfikator urządzenia, które odbiera token.

Ta metoda generuje token z następującą strukturę:

```
SharedAccessSignature sr={URI}&sig={HMAC_SHA256_SIGNATURE}&se={EXPIRATION_TIME}&skn={KEY_NAME}
```

W sekundach od 1 stycznia 1970 jest określony czas wygaśnięcia tokenu. Oto przykład tokenu:

```
SharedAccessSignature sr=contoso&sig=nPzdNN%2Gli0ifrfJwaK4mkK0RqAB%2byJUlt%2bGFmBHG77A%3d&se=1403130337&skn=RootManageSharedAccessKey
```

Zazwyczaj tokeny mają rozpiętość czasu, która przypomina lub przekracza rozpiętość czasu urządzenia. Jeśli urządzenie ma możliwość uzyskania nowy token, można używać tokenów z krótsze rozpiętość czasu.

### <a name="devices-sending-data"></a>Wysyłanie danych urządzeń

Po utworzeniu tokeny każde urządzenie jest obsługi administracyjnej z jej własne unikatowe token.

Gdy urządzenie dane są wysyłane do koncentratora zdarzenia, urządzenie tagi jego token z żądania wysłania. Aby blokowanie z podsłuchiwanie i kradzieżom tokenu, komunikacji między urządzeniem i Centrum zdarzenia musi przypadać nad zaszyfrowanego kanału.

### <a name="blacklisting-devices"></a>Wpisywanie na czarną listę urządzeń

Jeśli token jest kradzież intruz, tej może dokonać personifikacji urządzenie, którego token został kradzież. Blacklisting urządzenia są renderowane za pomocą urządzenia nie będzie można używać do momentu urządzenie znajduje się nowy token, która korzysta z innego programu publisher.

## <a name="authentication-of-back-end-applications"></a>Uwierzytelniania aplikacji wewnętrznej

Aby uwierzytelnić wewnętrznej aplikacje, które używają danych generowanych przez urządzenia, koncentratory zdarzenia wykorzystuje model zabezpieczeń, który jest podobny do modelu, który jest używany do tematów Bus usługi. Grupy dla klientów indywidualnych koncentratory zdarzenie jest równoznaczne z subskrypcji na temat usługi Bus. Klienta można utworzyć grupę dla klientów indywidualnych, jeśli żądanie tak, aby utworzyć grupę dla klientów indywidualnych towarzyszy tokenu dotacji Zarządzanie uprawnieniami, koncentratora zdarzenie lub nazw, do którego należy koncentratora zdarzenia. Klienta mogą być zużyte danych z grupy dla klientów indywidualnych, jeśli żądania odbioru towarzyszy token udziela praw Odbierz na tę grupę dla klientów indywidualnych, Centrum zdarzenie lub nazw, do którego należy Centrum zdarzenia.

Bieżącej wersji usługi Bus nie obsługuje reguł skojarzeń zabezpieczeń dla poszczególnych subskrypcji. To samo dla grup dla klientów indywidualnych koncentratory zdarzenia. Obsługa skojarzeń zabezpieczeń zostanie dodana do obu funkcji w przyszłości.

W przypadku braku uwierzytelniania skojarzeń zabezpieczeń dla grup poszczególnych dla klientów indywidualnych umożliwia kluczy skojarzeń zabezpieczeń bezpiecznego wszystkich grup dla klientów indywidualnych przy użyciu klucza typowych. Ta metoda umożliwia aplikacji wykorzystywania danych z jedną z grup dla klientów indywidualnych koncentratora zdarzenia.

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat koncentratory zdarzenia, odwiedź następujące tematy:

- [Omówienie koncentratory zdarzenia]
- [Rozwiązanie do obsługi wiadomości w kolejce] korzystanie z usługi Bus kolejek.
- Ukończono [przykładowej aplikacji, która korzysta z koncentratorów zdarzenia].

[Omówienie koncentratory zdarzenia]: event-hubs-overview.md
[Przykładowa aplikacja korzystającego z koncentratorów zdarzenia]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[rozwiązanie do obsługi wiadomości w kolejce]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
 
