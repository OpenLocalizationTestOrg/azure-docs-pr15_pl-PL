<properties
    pageTitle="Tematy Bus usługi za pomocą programu .NET | Microsoft Azure"
    description="Dowiedz się, jak korzystać z programu .NET w Azure tematy Bus usługi i subskrypcje. Przykłady kodu są zapisywane aplikacji .NET."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="09/16/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Jak używać usługi Bus tematy i subskrypcji

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Ten artykuł zawiera opis sposobu używania tematy Bus usługi i subskrypcje. Przykłady są zapisywane w języku C# i przy użyciu interfejsów API .NET. Scenariusze, w których zawierać tworzenia tematy i subskrypcje, tworzenia filtrów subskrypcji, wysyłanie wiadomości na temat, odbierania wiadomości z subskrypcji i usuwanie tematów i subskrypcji. Aby uzyskać więcej informacji o subskrypcji i tematów zobacz sekcję [Następne kroki](#next-steps) .

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="configure-the-application-to-use-service-bus"></a>Konfigurowanie aplikacji w celu za pomocą usługi Bus

Podczas tworzenia aplikacji, która korzysta z usługi Bus, należy dodać odwołanie do zestawu Bus usługi i dołączyć odpowiadające im nazw. Najprostszym sposobem na tym jest Pobierz odpowiednie [NuGet](https://www.nuget.org) .

## <a name="get-the-service-bus-nuget-package"></a>Pobierz pakiet NuGet Bus usługi

[Pakiet NuGet Bus usługi](https://www.nuget.org/packages/WindowsAzure.ServiceBus) jest najprostszym sposobem, aby uzyskać interfejs API Bus usługi i konfigurowanie aplikacji z wszystkie niezbędne zależności Bus usługi. Aby zainstalować pakiet NuGet Bus usługi w projekcie, wykonaj następujące czynności:

1.  W oknie Eksplorator rozwiązań kliknij prawym przyciskiem myszy **odwołania**, a następnie kliknij pozycję **Zarządzaj pakiety NuGet**.
2.  Wyszukaj "Usługi Bus" i wybierz element, **Microsoft Azure usługi Bus** . Kliknij przycisk **Zainstaluj** , aby ukończyć instalację, a następnie zamknij okno dialogowe następujące czynności:

    ![][7]

Teraz możesz przystąpić do pisania kodu dla Bus usługi.

## <a name="create-a-service-bus-connection-string"></a>Tworzenie parametrów połączenia Bus usługi

Usługa Bus przechowuje parametry połączenia punkty końcowe i poświadczenia. Ciąg połączenia można umieścić w pliku konfiguracji, zamiast słabo kodowania go:

- Podczas korzystania z usług Azure, zalecane jest, aby przechowywać ciąg połączenia przy użyciu usługi Azure konfiguracji systemu (pliki .csdef i .cscfg).
- Podczas korzystania z witryn sieci Web Azure lub maszyn wirtualnych Azure, zalecane jest, aby przechowywać ciąg połączenia w systemie .NET konfiguracji (na przykład plik Web.config).

W obu przypadkach można podjąć użyciu parametrów połączenia `CloudConfigurationManager.GetSetting` metody, jak pokazano w dalszej części tego artykułu.

### <a name="configure-your-connection-string"></a>Konfigurowanie usługi parametry połączenia

Mechanizm Konfiguracja usługi umożliwia dynamicznie zmieniać ustawienia konfiguracji z [Azure portal][] bez ponownego wdrożenia aplikacji. Na przykład dodać `Setting` etykiety do usługi pliku definicji (**.csdef**), jak pokazano w przykładzie dalej.

```
<ServiceDefinition name="Azure1">
...
    <WebRole name="MyRole" vmsize="Small">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString" />
        </ConfigurationSettings>
    </WebRole>
...
</ServiceDefinition>
```

Następnie określ wartości w pliku konfiguracji (.cscfg) usługi.

```
<ServiceConfiguration serviceName="Azure1">
...
    <Role name="MyRole">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString"
                     value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
        </ConfigurationSettings>
    </Role>
...
</ServiceConfiguration>
```

Użyj nazwy klucza podpisu udostępnionych programu Access (SA) i wartości klucza pobrane z portalem opisany powyżej.

### <a name="configure-your-connection-string-when-using-azure-websites-or-azure-virtual-machines"></a>Konfigurowanie ciąg połączenia podczas korzystania z witryn sieci Web Azure lub maszyn wirtualnych Azure

Podczas korzystania z witryn sieci Web lub maszyn wirtualnych, zaleca się, że używasz system konfiguracji .NET (na przykład Web.config). Przechowywanie, przy użyciu parametrów połączenia `<appSettings>` elementu.

```
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
    </appSettings>
</configuration>
```

Użyj nazwy skojarzeń zabezpieczeń i wartości klucza, które pobrane z [Azure portal][]opisany powyżej.

## <a name="create-a-topic"></a>Tworzenie tematu

Można wykonywać operacje zarządzania tematy Bus usługi i subskrypcje za pomocą klasy [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) . Ta klasa daje sposoby tworzenia, wyliczanie i usuwanie tematów.

Poniższy przykład tworzy `NamespaceManager` obiekt przy użyciu Azure `CloudConfigurationManager` zajęć przy użyciu parametrów połączenia, składający się z adres bazowy odpowiednie skojarzeń zabezpieczeń i nazw usługi Bus poświadczenia z uprawnieniami do zarządzania nim. Ten ciąg połączenia jest następującą postać:

```
Endpoint=sb://<yourNamespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<yourKey>
```

Za pomocą następującym przykładzie podane ustawienia konfiguracji w poprzedniej sekcji.

```
// Create the topic if it does not exist already.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic("TestTopic");
}
```

Występują nadmierne obciążenia metody [CreateTopic](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.createtopic.aspx) , które umożliwiają ustawianie właściwości tematu; na przykład aby ustawić domyślny czas wygaśnięcia (TTL) wartości mają być stosowane do wiadomości wysłane do tematu. Te ustawienia są stosowane przy użyciu klasy [TopicDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.aspx) . W poniższym przykładzie pokazano, jak utworzyć tematu o nazwie **TestTopic** o maksymalnym rozmiarze 5 GB i komunikat domyślny TTL 1 minuty.

```
// Configure Topic Settings.
TopicDescription td = new TopicDescription("TestTopic");
td.MaxSizeInMegabytes = 5120;
td.DefaultMessageTimeToLive = new TimeSpan(0, 1, 0);

// Create a new Topic with custom settings.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic(td);
}
```

> [AZURE.NOTE] Metoda [TopicExists](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.topicexists.aspx) obiektom [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) służy do sprawdzenia, czy tematu o określonej nazwie już istnieje w obszarze nazw.

## <a name="create-a-subscription"></a>Tworzenie subskrypcji

Można także tworzyć subskrypcji tematu przy użyciu klasy [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) . Subskrypcje są nazywane i nie może mieć opcjonalne filtr, który ogranicza zestaw komunikaty przesyłane do kolejki wirtualnych subskrypcji.

> [AZURE.IMPORTANT] Aby wiadomości, aby otrzymać subskrypcji możesz utworzyć tej subskrypcji przed wysłaniem komunikaty do tematu. W przypadku żadnych subskrypcji na temat temat powoduje odrzucenie tych wiadomości.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Tworzenie subskrypcji z filtr domyślny (MatchAll)

Jeśli określono bez filtru po utworzeniu nowej subskrypcji, filtr **MatchAll** jest filtr domyślny, który jest używany. Podczas używania funkcji Filtruj **MatchAll** , wszystkie wiadomości opublikowany w tym temacie są umieszczane w kolejce wirtualnych subskrypcji. Poniższy przykład tworzy subskrypcji o nazwie "AllMessages" i używa filtrowanie **MatchAll** domyślne.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.SubscriptionExists("TestTopic", "AllMessages"))
{
    namespaceManager.CreateSubscription("TestTopic", "AllMessages");
}
```

### <a name="create-subscriptions-with-filters"></a>Tworzenie subskrypcji przy użyciu filtrów

Możesz również skonfigurować filtry, które pozwalają określić, które wiadomości wysyłane do tematu należy są wyświetlane w ramach subskrypcji określonego tematu.

Najbardziej elastyczna typ filtru obsługiwane w przypadku subskrypcji jest klasy [SqlFilter][] , która wykonuje podzbiór standardzie SQL92. Filtry SQL działają na właściwości wiadomości, które są publikowane w tym temacie. Aby uzyskać więcej informacji na temat wyrażeń, których można używać z filtrem SQL zobacz Składnia [SqlFilter.SqlExpression][] .

Poniższy przykład tworzy subskrypcji o nazwie **HighMessages** z obiektem [SqlFilter][] wybierające tylko wiadomości, które mają właściwość niestandardowa składnika **MessageNumber** większa niż 3.

```
// Create a "HighMessages" filtered subscription.
SqlFilter highMessagesFilter =
   new SqlFilter("MessageNumber > 3");

namespaceManager.CreateSubscription("TestTopic",
   "HighMessages",
   highMessagesFilter);
```

Podobnie poniższym przykładzie jest tworzona subskrypcji o nazwie **LowMessages** z [SqlFilter][] wybierające tylko wiadomości, które mają **MessageNumber** właściwość mniejsze niż lub równe 3.

```
// Create a "LowMessages" filtered subscription.
SqlFilter lowMessagesFilter =
   new SqlFilter("MessageNumber <= 3");

namespaceManager.CreateSubscription("TestTopic",
   "LowMessages",
   lowMessagesFilter);
```

Teraz, gdy wiadomość jest wysyłana do `TestTopic`, zawsze będą dostarczane do odbiorców subskrybuje subskrypcji tematu **AllMessages** i selektywne dostarczane do odbiorców subskrybuje subskrypcji tematu **HighMessages** i **LowMessages** (w zależności od zawartości wiadomości).

## <a name="send-messages-to-a-topic"></a>Wysyłanie wiadomości na temat

Aby wysłać wiadomość na temat usługi Bus, aplikacja tworzy obiekt [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) przy użyciu parametrów połączenia.

Poniższy kod przedstawia sposób tworzenia obiektu [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) tematu **TestTopic** utworzony wcześniej przy użyciu [`CreateFromConnectionString`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.createfromconnectionstring.aspx) interfejsu API połączenia.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

TopicClient Client =
    TopicClient.CreateFromConnectionString(connectionString, "TestTopic");

Client.Send(new BrokeredMessage());
```

Wiadomości wysyłane do tematów Bus usługi są wystąpienia klasy [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) . Obiekty [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) mają pewien zestaw właściwości standardowe (na przykład [etykiety](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) i [licznika TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)), słownik, który służy do przechowywania niestandardowej właściwości specyficzne dla aplikacji i treści danych dowolnego aplikacji. Aplikacji można ustawić w treści wiadomości, przekazanie można obiekty do konstruktora obiektu [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) , a następnie służy odpowiednie **DataContractSerializer** szeregować obiektu. Możesz też **System.IO.Stream** może być udostępniana.

Poniższy przykład ilustruje wysyłać wiadomości pięć do obiektu [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) **TestTopic** uzyskane w poprzednim przykładzie. Należy zauważyć, że wartość właściwości [MessageNumber](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.properties.aspx) każdej wiadomości zmienia się w zależności od iteracji pętli (określa, które subskrypcje odebraniu).

```
for (int i=0; i<5; i++)
{
  // Create message, passing a string message for the body.
  BrokeredMessage message = new BrokeredMessage("Test message " + i);

  // Set additional custom app-specific property.
  message.Properties["MessageNumber"] = i;

  // Send message to the topic.
  Client.Send(message);
}
```

Usługa Bus tematy pomocy technicznej maksymalny rozmiar wiadomości 256 KB w [standardowej warstwy](service-bus-premium-messaging.md) i 1 MB w [warstwie Premium](service-bus-premium-messaging.md). Nagłówek, który zawiera właściwości aplikacji standardowych i niestandardowych, może mieć maksymalny rozmiar 64 KB. Istnieje limit liczby znajdujące się na temat wiadomości, ale jest zakończenie na całkowity rozmiar wiadomości według tematu. Rozmiar tego tematu jest zdefiniowany w czasie tworzenia z maksymalnie 5 GB. Po włączeniu podziału górną granicę zostanie zwiększona. Aby uzyskać więcej informacji zobacz [Partitioned wiadomości podmiotów](service-bus-partitioning.md).

## <a name="how-to-receive-messages-from-a-subscription"></a>Jak otrzymywać wiadomości z subskrypcji

Zalecany sposób otrzymywania wiadomości od subskrypcji jest użycie obiektu [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) . Obiekty [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) można pracować w dwóch różnych trybach: [ *ReceiveAndDelete* i *PeekLock*](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx).

Podczas korzystania z trybu **ReceiveAndDelete** odbieranie jest operacji zrzut pojedyncze; oznacza to, że po Bus usługa odbiera żądania odczytu wiadomości w subskrypcji, go oznacza wiadomość jako są używane i zwraca go do aplikacji. Tryb **ReceiveAndDelete** jest najprostszym model i sprawdza się najlepiej w przypadku scenariuszy, w których aplikacja przeszkadzają, nie przetwarzania wiadomości w przypadku awarii. Aby zrozumieć, to, warto rozważyć scenariusza, w którym problemy żądania odbioru konsumenta, a następnie ulega awarii przed jego przetworzenia. Ponieważ usługa Bus oznaczył wiadomość jako zużyte, gdy aplikacja uruchomieniu i rozpoczyna się ponownie przez inne wiadomości, będą zostały odebrane wiadomości, która została wykorzystana przed awarii.

W trybie **PeekLock** (jest to domyślny tryb) proces Odbierz staje się operację dwuetapowy, co umożliwia obsługuje aplikacji, nie można tolerować brakujące wiadomości. Gdy usługa Bus otrzymuje żądanie, umożliwia znalezienie wyrazów następnej wiadomości do zużycia, blokuje go, aby uniemożliwić innym konsumentów go odbiera i zwraca go do aplikacji. Po aplikacji zakończeniu przetwarzania wiadomości (lub zapisuje go w wiarygodny sposób przetwarzania przyszłych) wykonuje drugiego procesu odbierania, dzwoniąc [wykonane](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) w otrzymanej wiadomości. Gdy usługa Bus wykryje **Zakończ** połączenie, oznacza wiadomość jako są używane i powoduje usunięcie go z subskrypcji.

W poniższym przykładzie pokazano, jak wiadomości mogą być odbierane i przetwarzane przy użyciu domyślny tryb **PeekLock** . Aby określić inną wartość [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) , umożliwiają innym przeciążeń [CreateFromConnectionString](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.createfromconnectionstring.aspx). W tym przykładzie użyto zwrotnego [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.onmessage.aspx) w celu przetwarzania wiadomości przysłanych subskrypcję **HighMessages** .

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

SubscriptionClient Client =
    SubscriptionClient.CreateFromConnectionString
            (connectionString, "TestTopic", "HighMessages");

// Configure the callback options.
OnMessageOptions options = new OnMessageOptions();
options.AutoComplete = false;
options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

Client.OnMessage((message) =>
{
    try
    {
        // Process message from subscription.
        Console.WriteLine("\n**High Messages**");
        Console.WriteLine("Body: " + message.GetBody<string>());
        Console.WriteLine("MessageID: " + message.MessageId);
        Console.WriteLine("Message Number: " +
            message.Properties["MessageNumber"]);

        // Remove message from subscription.
        message.Complete();
    }
    catch (Exception)
    {
        // Indicates a problem, unlock message in subscription.
        message.Abandon();
    }
}, options);
```

W tym przykładzie konfiguruje zwrotnego [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.onmessage.aspx) przy użyciu obiektu [OnMessageOptions](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.aspx) . [Autouzupełnianie](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autocomplete.aspx) jest ustawiona na **false** , aby włączyć ręczną kontrolę nad tym, kiedy należy nawiązać połączenie [wykonane](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) w otrzymanej wiadomości. [AutoRenewTimeout](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autorenewtimeout.aspx) jest ustawiona na 1 minutę, co powoduje, że klient czekać na jedną minutę przed zakończeniem funkcji automatycznego odnawiania i klient dzwoni nowe sprawdzania wiadomości. Wartość tej właściwości zmniejsza liczbę powtórzeń klienta sprawia, że odpłatne połączeń, które nie pobierać wiadomości.

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Jak obsługiwać awarie aplikacji i nie można odczytać wiadomości

Usługa Bus zawiera funkcje ułatwiające przywrócenie bezpiecznie z błędami w aplikacji lub trudności przetwarzania wiadomości. Jeśli aplikacja odbierająca nie może przetworzyć komunikatu jakiegoś powodu, następnie go może wywołać [zrezygnować](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.abandon.aspx) metody otrzymanej wiadomości (zamiast metody [Wykonano](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) ). Ta opcja powoduje Bus usługi odblokować wiadomości w ramach subskrypcji i udostępnić otrzymać ponownie przez tę samą aplikację dużo lub w innej aplikacji dużo.

Jest również limit czasu skojarzone z komunikatem zablokowane w ramach subskrypcji, a jeśli aplikacji nie powiedzie się przetwarzania wiadomości przed blokady wygaśnięciu limitu czasu (na przykład jeśli powoduje awarię aplikacji), następnie Bus usługi automatycznie odblokowanie wiadomości i udostępnia będzie odebrana ponownie.

W przypadku, gdy aplikacja ulega awarii po przetworzeniu wiadomości, ale przed wydaniem żądania [wykonania](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) , wiadomości będą się przed przeniesieniem, do aplikacji po jego ponownym uruchomieniu. Jest to często nazywane, *co najmniej raz przetwarzania*; oznacza to, że każda wiadomość jest przetwarzana co najmniej raz, ale w niektórych przypadkach może być przed przeniesieniem tego samego komunikatu. Jeśli tego scenariusza nie można tolerować zduplikowane przetwarzanie, deweloperzy aplikacji należy dodać dodatkowe logiki do ich aplikacji do obsługi dostarczania zduplikowane wiadomości. Często można to osiągnąć przy użyciu właściwości [komunikatu](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) wiadomości, która pozostaje stała wielu prób dostarczenia.

## <a name="delete-topics-and-subscriptions"></a>Usuwanie tematów i subskrypcji

Poniższy przykład pokazuje, jak usuwanie tematu **TestTopic** z nazw usługi **HowToSample** .

```
// Delete Topic.
namespaceManager.DeleteTopic("TestTopic");
```

Usuwanie tematu powoduje także usunięcie żadnej subskrypcji, które są zarejestrowane wraz z tematem. Subskrypcje mogą być również usuwane niezależnie. Poniższy kod przedstawia sposób usunięcia subskrypcji o nazwie **HighMessages** w temacie **TestTopic** .

```
namespaceManager.DeleteSubscription("TestTopic", "HighMessages");
```

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy tematy Bus usługi i subskrypcje, skorzystaj z poniższych łączy, aby dowiedzieć się więcej.

-   [Kolejki, tematy i subskrypcji][].
-   [Przykładowe filtry tematu][]
-   Opis interfejsu API dla [SqlFilter][].
-   Tworzenie aplikacji pracy, który wysyła i odbiera wiadomości przy użyciu kolejki Bus usługi: [Usługa Bus brokered wiadomości samouczek .NET][].
-   Przykłady Bus usługi: Pobierz z [próbki Azure][] lub zobacz [Omówienie](service-bus-samples.md).

  [Azure portal]: https://portal.azure.com

  [7]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/getting-started-multi-tier-13.png

  [Kolejki, tematy i subskrypcji]: service-bus-queues-topics-subscriptions.md
  [Przykładowe filtry tematu]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx
  [SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Usługa Bus brokered wiadomości samouczek .NET]: service-bus-brokered-tutorial-dotnet.md
  [Przykłady Azure]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
