<properties 
    pageTitle="Pisanie aplikacje, które korzystają z usługi Bus kolejek | Microsoft Azure"
    description="Jak pisać prostej aplikacji opartych na kolejkach, która korzysta z usługi Bus."
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

# <a name="create-applications-that-use-service-bus-queues"></a>Tworzenie aplikacji, które korzystają z usługi Bus kolejek

W tym temacie opisano kolejek Bus usługi i przedstawiono sposób tworzenia prostej aplikacji opartych na kolejkach, która korzysta z usługi Bus.

Rozważmy scenariusz ze świata detalicznym, w którym dane dotyczące sprzedaży w poszczególnych terminale Point-of-Sale (punkt sprzedaży) muszą być kierowane do systemu zarządzania zapasami, oparty na danych, aby określić, kiedy ma być uzupełniona zasobów. To rozwiązanie wymaga usługi Bus wiadomości dla komunikacji między terminale i systemu zarządzania zapasami, jak pokazano na poniższym rysunku:

![Obraz kolejki Bus usługi 1](./media/service-bus-create-queues/IC657161.gif)

Terminal w każdym punkcie sprzedaży raportów jego dane dotyczące sprzedaży przez wysyłanie wiadomości do **DataCollectionQueue**. Te wiadomości pozostają w kolejce do momentu ich pobraniu z systemu zarządzania zapasami. Ten wzorzec jest często określane jako *asynchroniczne wiadomości*, ponieważ nie ma czekać na odpowiedzi od systemu zarządzania zapasami kontynuować przetwarzanie terminal w punkcie sprzedaży.

## <a name="why-queuing"></a>Dlaczego Kolejkowanie?

Zanim przyjrzymy kod, który jest wymagane do skonfigurowania tej aplikacji, należy rozważyć, czy zalety korzystania z kolejki w tym scenariuszu zamiast terminale punktu sprzedaży porozmawiać bezpośrednio (synchronicznie) do systemu zarządzania zapasami.

### <a name="temporal-decoupling"></a>Rozdzielenie czasowy

Z wzorcem wiadomości asynchroniczne producentów i konsumentów nie trzeba być w trybie online w tym samym czasie. Infrastruktury obsługi wiadomości niezawodne przechowuje wiadomości, aż dużo strona jest gotowa do ich odbierać. Oznacza to, że części aplikacji rozproszonej mogą być rozłączone, albo dobrowolnie; na przykład obsługi lub z powodu awarii składnika, bez wpływu na całego systemu. Ponadto dużo aplikacji tylko może być konieczne być w trybie online pewnych porach dnia. Na przykład w tym scenariuszu detalicznej systemu zarządzania zapasami tylko mają do trybu online po zakończeniu dnia roboczego.

### <a name="load-leveling"></a>Wyrównywanie obciążenia

Wiele aplikacji obciążenia systemu różni się z czasem przetwarzania czas dla każdej jednostki pracy jest zazwyczaj stałej. Intermediating producentów wiadomości i konsumentów z kolejki oznacza, że dużo aplikacji (pracowników) ma być tylko ma ma zostać zastrzeżona do obsługi średnie obciążenie zamiast ładowania Szczyt. Głębokość kolejki będą powiększanie i umowy podczas ładowania przychodzących zmienia się. Zapisuje bezpośrednio pieniędzy w odniesieniu do kwoty infrastruktury wymagane do obsługi obciążenia aplikacji.

![Obraz kolejki Bus usługi 2](./media/service-bus-create-queues/IC657162.gif)

### <a name="load-balancing"></a>Równoważenia obciążenia

Miarę wzrostu obciążenia, aby odczytać z kolejki pracownika można dodać więcej procesy. Każda wiadomość jest przetwarzana przez tylko jeden procesy. Ponadto ten równoważenia obciążenia opartego na pobieraj umożliwia do użytku optymalnej komputerów pracownika nawet w przypadku komputerów pracownik różnią się w odniesieniu do potęgi przetwarzanie, wprowadzi wiadomości własne maksymalna szybkość. Ten wzorzec jest często określane jako konkurujących deseniu dla klientów indywidualnych.

![Obraz kolejki Bus usługi 3](./media/service-bus-create-queues/IC657163.gif)

### <a name="loose-coupling"></a>Swobodna sprzężenia

Używanie Kolejkowanie do między producentów wiadomości i konsumentów zapewnia wewnętrzne sprzężenie luźno składniki. Ponieważ producentów i konsumentów nie są znane od siebie, można uaktualnić konsumenta bez wpływu na producenta. Ponadto można rozwijać topologii wiadomości, bez wpływu na istniejące punkty końcowe. Omówiono to bardziej po porozmawiamy o publikowania subskrypcji.

## <a name="show-me-the-code"></a>Pokaż kod

Sekcji poniżej pokazano, jak używać usługi Bus do utworzenia tej aplikacji.

### <a name="sign-up-for-an-azure-account"></a>Załóż konto Azure

Aby rozpocząć pracę z usługi Bus konieczne będzie konto Azure. Jeśli nie masz już jeden, możesz zalogować się w górę bezpłatne konto [tutaj](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A85619ABF).

### <a name="create-a-namespace"></a>Tworzenie obszaru nazw

Po umieszczeniu subskrypcji, możesz [utworzyć nowy obszar nazw](service-bus-create-namespace-portal.md). Każdy obszar nazw działa jako zakresu kontenera dla zestawu obiektów Bus usługi. Określ unikatową nazwę nowego obszaru nazw dla wszystkich kont usługi Bus. 

### <a name="install-the-nuget-package"></a>Zainstaluj pakiet NuGet

Aby użyć nazw usługi Bus, aplikacja muszą odwoływać się do zestawu Bus usługę, w szczególności Microsoft.ServiceBus.dll. Możesz znaleźć tego zestawu w ramach pakietu Microsoft Azure i pobierania jest dostępna na [stronie pobierania Azure SDK](https://azure.microsoft.com/downloads/). Jednak [pakiet NuGet Bus usługi](https://www.nuget.org/packages/WindowsAzure.ServiceBus) jest najprostszym sposobem, aby uzyskać interfejs API Bus usługi i skonfigurować aplikację ze wszystkimi zależności Bus usługi.

### <a name="create-the-queue"></a>Tworzenie kolejki

Operacji zarządzania Bus usługi wiadomości jednostek (tematy kolejki i publikowania subskrypcji) są wykonywane za pośrednictwem klasy [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) . Usługa Bus korzysta z modelu zabezpieczenia oparte [Udostępnione podpis programu Access (SA)](service-bus-sas-overview.md) . Klasa [Dostawca TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) reprezentuje dostawcy tokenu zabezpieczeń z metod wbudowanych factory zwracanie niektórych znanych dostawców token. Metoda [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) będzie używana do przechowywania poświadczeń skojarzeń zabezpieczeń. Wystąpienia [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) następnie jest tworzona przy użyciu podstawowego adresu nazw Bus usługi i Dostawca tokenu.

Klasa [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) udostępnia sposoby tworzenia, wyliczanie i usuwania wiadomości podmiotów. Kod, który jest wyświetlany poniżej pokazano sposobu tworzenia i użyte do utworzenia kolejki **DataCollectionQueue** wystąpienia [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) .

```
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", 
                "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";
 
TokenProvider tokenProvider = 
    TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = 
    new NamespaceManager(uri, tokenProvider);
namespaceManager.CreateQueue("DataCollectionQueue");
```

Należy zauważyć, że występują nadmierne obciążenia metody [CreateQueue](https://msdn.microsoft.com/library/azure/hh322663.aspx) umożliwiające właściwości kolejki, aby być dostosowanych. Na przykład można ustawić time to live (TTL) wartość domyślna wiadomości wysłane do kolejki.

### <a name="send-messages-to-the-queue"></a>Wysyłanie wiadomości do kolejki

Do wykonywania operacji na podmioty Bus usługi; na przykład wysyłanie i odbieranie wiadomości, aplikacja najpierw musisz utworzyć obiektu [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) . Podobnie jak w klasie [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) , wystąpienie [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) zostanie utworzona na adres podstawowy obszar nazw usługi i Dostawca tokenu.

```
 BrokeredMessage bm = new BrokeredMessage(salesData);
 bm.Label = "SalesReport";
 bm.Properties["StoreName"] = "Redmond";
 bm.Properties["MachineID"] = "POS_1";
```

Wiadomości wysyłane do, a uzyskane od usługi Bus kolejki są wystąpienia klasy [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) . Ta klasa składa się z zestawu właściwości standardowe (na przykład [etykiety](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) i [licznika TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)), słownik, który służy do przechowywania właściwości aplikacji i treści danych dowolnego aplikacji. Aplikacja można ustawić jednostkę przez przekazanie dowolnego obiektu można (poniższy przykład przekazuje w obiekcie **SalesData** reprezentujący dane sprzedaży z terminal w punkcie sprzedaży), która będzie korzystać [DataContractSerializer](https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx) szeregować obiektu. Możesz też może być udostępniana obiektu [strumienia](https://msdn.microsoft.com/library/azure/system.io.stream.aspx) .

Najprostszym sposobem na wysyłanie wiadomości do danej kolejki w naszym przypadku **DataCollectionQueue**jest umożliwia tworzenie obiektu [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) bezpośrednio z wystąpienia [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) [CreateMessageSender](https://msdn.microsoft.com/library/azure/hh322659.aspx) .

```
MessageSender sender = factory.CreateMessageSender("DataCollectionQueue");
sender.Send(bm);
```

### <a name="receiving-messages-from-the-queue"></a>Odbieranie wiadomości z kolejki

Aby odbierać wiadomości z kolejki, możesz użyć obiektu [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) , które są tworzone bezpośrednio na [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) przy użyciu [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx). Odbiorcy wiadomości można pracować w dwóch różnych trybach: **ReceiveAndDelete** i **PeekLock**. [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) ustawiono po utworzeniu odbiorca wiadomości jako parametr połączenia [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx) .


W trybie **ReceiveAndDelete** odbioru jest operacji zrzut pojedyncze; oznacza to, że po Bus usługi odbiera żądanie, jej oznacza wiadomość jako są używane i zwraca go do aplikacji. Tryb **ReceiveAndDelete** jest najprostszym model i najlepiej pasuje do scenariusze, w których aplikacja przeszkadzają, nie przetwarzania wiadomości w przypadku wystąpienia awarii. Aby zrozumieć, to, warto rozważyć scenariusza, w którym problemy żądania odbioru konsumenta, a następnie ulega awarii przed jego przetworzenia. Ponieważ usługa Bus oznaczone wiadomości jako są używane po uruchomieniu aplikacji i uruchamia ponownie przez inne wiadomości, będą zostały odebrane wiadomości, która została wykorzystana przed awarii.

W trybie **PeekLock** odbierania staje się operację dwuetapowy, dzięki czemu możliwe do obsługi aplikacji, nie można tolerować brakujące wiadomości. Gdy usługa Bus odbiera żądania, umożliwia znalezienie wyrazów następnej wiadomości do zużycia, blokuje go, aby uniemożliwić innym konsumentów go odbiera i zwraca go do aplikacji. Po aplikacji zakończeniu przetwarzania wiadomości (lub zapisuje go w wiarygodny sposób przetwarzania przyszłych) wykonuje drugiego procesu odbierania, dzwoniąc [wykonane](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) w otrzymanej wiadomości. Gdy usługa Bus wykryje [Zakończ](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) połączenie, oznacza wiadomość jako są używane.

Możliwe są dwa inne wyniki. Najpierw Jeśli aplikacji nie może przetworzyć komunikatu jakiegoś powodu, go można wywołać [zrezygnować](https://msdn.microsoft.com/library/azure/hh181837.aspx) w otrzymanej wiadomości (zamiast [Wykonano](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx)). Ta opcja powoduje Bus usługi odblokować wiadomości i udostępnić otrzymać ponownie konsumenta tym samym lub innym Kończenie dla klientów indywidualnych. Drugi, jest skojarzone z blokady limit czasu i jeśli aplikacji nie może przetworzyć wiadomości przed blokady limitu czasu wygaśnięcia (na przykład jeśli powoduje awarię aplikacji), a następnie Bus usługi będzie odblokować wiadomości i udostępnić będzie odebrana ponownie (zasadniczo operacji [zrezygnować](https://msdn.microsoft.com/library/azure/hh181837.aspx) domyślnie).

Należy zauważyć, że jeśli aplikacja ulega awarii po przetwarza wiadomości, ale przed [Wykonano](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) żądanie został wydany, wiadomości będzie się przed przeniesieniem do aplikacji po jego ponownym uruchomieniu. Jest to często określane jako *Co najmniej raz* przetwarzania. Oznacza to, że każda wiadomość zostanie przetworzony co najmniej raz, ale w niektórych przypadkach może być przed przeniesieniem tego samego komunikatu. Jeśli tego scenariusza nie można tolerować zduplikowanych przetwarzanie, w aplikacji wykrywanie duplikatów jest wymagane dodatkowe logicznych. Można to osiągnąć na podstawie właściwości [komunikatu](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) wiadomości. Wartości tej właściwości pozostanie niezmieniona wielu prób dostarczenia. To jest określane jako przetwarzanie *Tylko raz* .

Kod, który jest wyświetlany poniżej odbiera i przetwarza wiadomości przy użyciu trybu **PeekLock** , w którym to ustawienie domyślne, jeśli jest jawnie podana żadna wartość [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) .

```
MessageReceiver receiver = factory.CreateMessageReceiver("DataCollectionQueue");
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

### <a name="use-the-queue-client"></a>Za pomocą klienta usługi kolejki

Przykłady wcześniej w tej sekcji utworzony [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) i [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) obiektów bezpośrednio z [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) do wysyłania i odbierania wiadomości z kolejki, odpowiednio. Podejściem alternatywnym jest użycie klasy [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) , która obsługuje zarówno wysyłania i odbierania operacje oprócz bardziej zaawansowane funkcje, takie jak sesji.

```
QueueClient queueClient = factory.CreateQueueClient("DataCollectionQueue");
queueClient.Send(bm);
            
BrokeredMessage message = queueClient.Receive();

try
{
    ProcessMessage(message);
    message.Complete();
}
catch (Exception e)
{
    message.Abandon();
} 
```

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy kolejek, zobacz [Tworzenie aplikacji używających tematy Bus usługi i subskrypcje](service-bus-create-topics-subscriptions.md) , aby kontynuować tej dyskusji za pomocą funkcji publikowania/subskrypcji tematów Bus usługi i subskrypcje.