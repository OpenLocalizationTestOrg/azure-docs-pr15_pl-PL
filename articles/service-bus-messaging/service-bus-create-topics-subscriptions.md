<properties 
    pageTitle="Tworzenie aplikacji używających tematy Bus usługi i subskrypcje | Microsoft Azure"
    description="Wprowadzenie do publikowania — subskrypcja możliwości oferowane przez tematy Bus usługi i subskrypcje."
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
    ms.date="10/04/2016"
    ms.author="sethm" />

# <a name="create-applications-that-use-service-bus-topics-and-subscriptions"></a>Tworzenie aplikacji korzystających z usługi Bus tematy i subskrypcji

Azure Bus usługi obsługuje zestaw technologii pośredniczącym opartej na chmurze, przesyłania wiadomości, łącznie z zaufanego usługi i trwałe publikowania/subskrypcji wiadomości. Ten artykuł opiera się na informacji podanych w [aplikacjach tworzenie, które korzystają z usługi Bus kolejek](service-bus-create-queues.md) i oferuje wprowadzenie do publikowania/subskrypcji możliwości oferowane przez tematy Bus usługi.

## <a name="evolving-retail-scenario"></a>Scenariusz detalicznej zmieniających się

W tym artykule nadal scenariusza detalicznym, używanego w [aplikacjach tworzenie, które korzystają z usługi Bus kolejek](service-bus-create-queues.md). Odwoływanie tej dane dotyczące sprzedaży w poszczególnych punktu sprzedaży (punkcie sprzedaży) terminale muszą być kierowane do systemu zarządzania zapasami, używający tych danych, aby określić, kiedy ma być uzupełniona giełdowy. Terminal w każdym punkcie sprzedaży raportów jego dane dotyczące sprzedaży przez wysłanie wiadomości do kolejki **DataCollectionQueue** miejsce, w którym pozostają do momentu ich odbierania przez system zarządzania zapasami, jak pokazano poniżej:

![Usługa Bus 1](./media/service-bus-create-topics-subscriptions/IC657161.gif)

Do są obsługiwane w tym scenariuszu, nowych wymogów został dodany do systemu: właściciel Sklepu chce monitorować informacje dotyczące wydajności magazynie w czasie rzeczywistym.

Aby rozwiązać ten wymóg, system musi "naciśnij" strumienia dane dotyczące sprzedaży. Każda wiadomość wysłana przez terminale punktu sprzedaży były wysyłane do systemu zarządzania zapasami jako przed będą nadal, ale potrzebne jest inna kopia każdej wiadomości, które pomagają prezentowanie widoku pulpitu nawigacyjnego do właściciela magazynu.

W sytuacji, takich jak, w której jest wymagane każda wiadomość ma zostać zużyta przez wiele stron, można użyć usługi Bus *tematów*. Tematy zawierają wzorzec publikowania/subskrypcji, w którym każdej wiadomości opublikowanych były dostępne dla jedną lub więcej subskrypcji zarejestrowanych w temacie. Natomiast z kolejek każda wiadomość jest odebrana przez jednego dla klientów indywidualnych.

Wiadomości są wysyłane do tematu w taki sam sposób, jak są one są wysyłane do kolejki. Jednak nie odbiera wiadomości z tematu bezpośrednio; są one odbierane z subskrypcji. Możesz myśleć subskrypcji na temat jako kolejka wirtualnych otrzymuje kopie wiadomości, które są wysyłane do tego tematu. Wiadomości są odbierane z subskrypcji taki sam sposób są odbierane z kolejki.

Rozważmy ponownie dotyczący tego scenariusza detalicznym, kolejki zastępuje tematu, a zostanie dodany subskrypcji, którego można używać składnika systemu zarządzania zapasami. System wygląda teraz następująco:

![Usługa Bus 2](./media/service-bus-create-topics-subscriptions/IC657165.gif)

Konfiguracji wykonuje identyczne do poprzedniego opartych na kolejkach projektu. Oznacza to, że wiadomości wysłane do tematu są kierowane do subskrypcji **zapasów** , z którego **Systemu zarządzania zapasami** korzysta z nich.

W celu wsparcia pulpit nawigacyjny zarządzania, tworzymy drugiego subskrypcji na temat, jak pokazano poniżej:

![Usługa Bus 3](./media/service-bus-create-topics-subscriptions/IC657166.gif)

W tej konfiguracji każdej wiadomości z terminale punktu sprzedaży jest udostępniana subskrypcje zarówno **pulpitu nawigacyjnego** i **zapasów** .

## <a name="show-me-the-code"></a>Pokaż kod

Artykuł [Tworzenie aplikacji używających kolejek usługi Bus](service-bus-create-queues.md) opisano Załóż konto Azure i tworzenie nazw usługi. Aby użyć nazw usługi Bus, aplikacja muszą odwoływać się do zestawu Bus usługę, w szczególności Microsoft.ServiceBus.dll. Najłatwiejszym sposobem odwołanie zależności Bus usługi jest zainstalować usługę Bus [pakiet Nuget](https://www.nuget.org/packages/WindowsAzure.ServiceBus/). Możesz również znaleźć ten zestaw jako część zestawu SDK Azure. Pobieranie jest dostępna na [stronie pobierania Azure SDK](https://azure.microsoft.com/downloads/).

### <a name="create-the-topic-and-subscriptions"></a>Tworzenie tematu i subskrypcji

Operacji zarządzania Bus usługi wiadomości jednostek (tematy kolejki i publikowania subskrypcji) są wykonywane za pośrednictwem klasy [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) . Odpowiednie poświadczenia są wymagane w celu utworzenia wystąpienia [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) dla określonego obszaru nazw. Usługa Bus korzysta z modelu zabezpieczenia oparte [Udostępnione podpis programu Access (SA)](service-bus-sas-overview.md) . Klasa [Dostawca TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) reprezentuje dostawcy tokenu zabezpieczeń z metod wbudowanych factory zwracanie niektórych znanych dostawców token. Metoda [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) będzie używana do przechowywania poświadczeń skojarzeń zabezpieczeń. Wystąpienia [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) następnie jest tworzona przy użyciu podstawowego adresu nazw Bus usługi i Dostawca tokenu.

Klasa [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) udostępnia sposoby tworzenia, wyliczanie i usuwania wiadomości podmiotów. Kod, który jest wyświetlany poniżej pokazano sposobu tworzenia i umożliwia tworzenie tematu **DataCollectionTopic** wystąpienia [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) .

```
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";
     
TokenProvider tokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = new NamespaceManager(uri, tokenProvider);
 
namespaceManager.CreateTopic("DataCollectionTopic");
```

Należy zauważyć, że występują nadmierne obciążenia metody [CreateTopic](https://msdn.microsoft.com/library/azure/hh293080.aspx) , które umożliwiają ustawianie właściwości tematu. Na przykład można ustawić time to live (TTL) wartość domyślna wiadomości wysłane do tematu. Następnie dodaj subskrypcje **zasobów** i **pulpit nawigacyjny** .

```
namespaceManager.CreateSubscription("DataCollectionTopic", "Inventory");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard");
```

### <a name="send-messages-to-the-topic"></a>Wysyłanie wiadomości do tematu

Do wykonywania operacji na podmioty Bus usługi; na przykład wysyłanie i odbieranie wiadomości, aplikacja najpierw musisz utworzyć obiektu [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) . Podobnie jak w klasie [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) , wystąpienie [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) zostanie utworzona na adres podstawowy obszar nazw usługi i Dostawca tokenu.

```
MessagingFactory factory = MessagingFactory.Create(uri, tokenProvider);
```

Wiadomości wysyłane do i odebrana tematy Bus usługi, są wystąpienia klasy [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) . Ta klasa składa się z zestawu właściwości standardowe (na przykład [etykiety](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) i [licznika TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)), słownik, który służy do przechowywania właściwości aplikacji i treści danych dowolnego aplikacji. Aplikacja można ustawić jednostkę przez przekazanie dowolnego obiektu można (poniższy przykład przekazuje w obiekcie **SalesData** reprezentujący dane sprzedaży z terminal w punkcie sprzedaży), która będzie korzystać [DataContractSerializer](https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx) szeregować obiektu. Możesz też może być udostępniana obiektu [strumienia](https://msdn.microsoft.com/library/azure/system.io.stream.aspx) .

```
BrokeredMessage bm = new BrokeredMessage(salesData);
bm.Label = "SalesReport";
bm.Properties["StoreName"] = "Redmond";
bm.Properties["MachineID"] = "POS_1";
```

Najprostszym sposobem na wysyłanie wiadomości do tematu jest umożliwia tworzenie obiektu [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) bezpośrednio z wystąpienia [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) [CreateMessageSender](https://msdn.microsoft.com/library/azure/hh322659.aspx) .

```
MessageSender sender = factory.CreateMessageSender("DataCollectionTopic");
sender.Send(bm);
```

### <a name="receive-messages-from-a-subscription"></a>Odbieranie wiadomości z subskrypcji

Podobnie jak korzystanie z kolejek, aby otrzymywać wiadomości z subskrypcji obiekt [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) , które są tworzone bezpośrednio na [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) przy użyciu [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx)może być używany. Możesz użyć jednego z dwóch różnych odbieranie tryby (**ReceiveAndDelete** i **PeekLock**), zgodnie z opisem w [aplikacjach tworzenie, które korzystają z usługi Bus kolejek](service-bus-create-queues.md).

Należy zauważyć, że po utworzeniu [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) subskrypcji parametr *entityPath* formularza `topicPath/subscriptions/subscriptionName`. W związku z tym, aby utworzyć [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) subskrypcji **zapasów** temat **DataCollectionTopic** , *entityPath* musi być równa `DataCollectionTopic/subscriptions/Inventory`. Kod wygląda następująco:

```
MessageReceiver receiver = factory.CreateMessageReceiver("DataCollectionTopic/subscriptions/Inventory");
BrokeredMessage receivedMessage = receiver.Receive();
try
{
    ProcessMessage(receivedMessage);
    receivedMessage.Complete();
}
catch (Exception e)
{
    receivedMessage.Abandon();
}
```

## <a name="subscription-filters"></a>Filtry subskrypcji

Pory w tym scenariuszu wszystkie wiadomości wysłane do tematu będą dostępne dla wszystkich subskrypcji zarejestrowane. Tutaj najważniejszych frazę "udostępniono." Podczas subskrypcji usługi Bus wyświetlić wszystkie wiadomości wysłane do tematu, możesz skopiować tylko niektóre tych wiadomości do kolejki wirtualnych subskrypcji. To jest wykonywane przy użyciu subskrypcji *Filtry*. Podczas tworzenia subskrypcji należy podać wyrażenia filtru w postaci predykatu styl standardzie SQL92 działającą za pośrednictwem właściwości wiadomości, zarówno właściwości systemu (na przykład [etykiety](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx)) i właściwości aplikacji, takich jak **pole Nazwasklepu** w poprzednim przykładzie.

Rozwijających się tego scenariusza, aby przedstawić to, drugi magazynu ma zostać dodany do scenariusza naszych detalicznej. Dane dotyczące sprzedaży ze wszystkich terminale punktu sprzedaży z obu magazynów jeszcze kierowane do systemu zarządzania zapasami scentralizowane, ale kierownik sklepu przy użyciu narzędzia do pulpitu nawigacyjnego jest tylko w wydajności tego magazynu. Umożliwia filtrowanie, aby osiągnąć ten cel subskrypcji. Należy zauważyć, że terminale punktu sprzedaży publikowania wiadomości, ustawione **pole Nazwasklepu** właściwości aplikacji w wiadomości. Podane dwa sklepy, na przykład **Redmond** i **Seattle**, terminale punktu sprzedaży w Redmond zawierają stempel wiadomościach dane dotyczące sprzedaży z **pole Nazwasklepu** równe **Redmond**, terminale punktu sprzedaży sklepu Seattle stosowania **pole Nazwasklepu** równe **Seattle**. Kierownik sklepu magazynu Redmond tylko chce wyświetlić dane z jego terminale punktu sprzedaży. System wygląda następująco:

![Usługa Bus4](./media/service-bus-create-topics-subscriptions/IC657167.gif)

Aby skonfigurować ten routingu, tworzenia subskrypcji **pulpitu nawigacyjnego** w następujący sposób:

```
SqlFilter dashboardFilter = new SqlFilter("StoreName = 'Redmond'");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard", dashboardFilter);
```

Z tym filtrem subskrypcji tylko wiadomości, które mają **pole Nazwasklepu** ustawiona do **Redmond** zostaną skopiowane do wirtualnego kolejki dla subskrypcji **pulpitu nawigacyjnego** . Istnieje wiele więcej subskrypcji filtrowania, jednak. Aplikacje mogą zawierać wiele reguł filtru na subskrypcję oprócz możliwość modyfikowania właściwości wiadomości przesyłanych do wirtualnego kolejki subskrypcji.

## <a name="summary"></a>Podsumowanie

Wszystkie powody do korzystania z Kolejkowanie opisano w sekcji [Tworzenie aplikacji, które korzystają z usługi Bus kolejek](service-bus-create-queues.md) również zastosować do tematów, specjalnie:

- Czasowy rozdzielenie — producentów wiadomości i konsumentów nie trzeba być w trybie online w tym samym czasie.

- Wyrównywanie obciążenia — wartości w obciążenia są wygładzania według tematu Włączanie dużo aplikacji ma zostać zastrzeżona dla średnie obciążenie zamiast ładowania Szczyt.

- Równoważenia obciążenia — podobnie jak w kolejce, możesz mieć wiele konkurencyjnych konsumentów nasłuchują na jednym subskrypcji, do każdej wiadomości przekazane do określonego odbiorcy, w związku z tym równoważenia obciążenia.

- Swobodna sprzężenia — są obsługiwane sieci wiadomości bez wpływu na istniejące punkty końcowe; na przykład dodawanie subskrypcji lub zmiana filtrów na temat, aby umożliwić dla nowych klientów.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać informacje o używaniu kolejek w scenariuszu punktu sprzedaży detalicznej, zobacz [Tworzenie aplikacji, które korzystają z usługi Bus kolejek](service-bus-create-queues.md) .