<properties 
    pageTitle="Usługa Bus sparowane nazw | Microsoft Azure"
    description="Szczegóły dotyczące implementacji iloczynów nazw i koszt"
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

# <a name="paired-namespace-implementation-details-and-cost-implications"></a>Skojarzone szczegóły implementacji nazw i koszt skutków

Metoda [PairNamespaceAsync][] przy użyciu wystąpieniem [SendAvailabilityPairedNamespaceOptions][] wykonuje zadania widoczne w Twoim imieniu. Ponieważ są koszty zagadnienia, podczas korzystania z funkcji, warto opis tych zadań, tak aby można się spodziewać zachowanie podczas tak się dzieje. Interfejs API uczestniczy o następujących zasadach automatycznych w Twoim imieniu:

-   Tworzenie kolejek zaległości.
-   Tworzenie obiektu [MessageSender][] z kolejki lub tematów.
-   Kiedy osoba prowadząca wiadomości jest niedostępny, wysyła ping wiadomości do obiektu w celu wykrywania tego obiektu stał się dostępny ponownie.
-   Opcjonalnie tworzy zestawu pomp"komunikat" który przenieść wiadomości z kolejek zaległości do podstawowego kolejek.
-   Współrzędne zamknięcia błąd wystąpień [MessagingFactory][] głównego i pomocniczego.

Na wysokim poziomie, ta funkcja działa w następujący sposób: gdy Encja podstawowa jest prawidłowy, występują żadne zmiany zachowania. Gdy czas trwania [FailoverInterval][] upłynie i Encja podstawowa widzi nie powiedzie wysyła po innych niż zmiennych [MessagingException][] lub [TimeoutException][]występuje o następujących zasadach:

1.  Wyślij operacje z obiektem podstawowym są wyłączone i system pinguje Encja podstawowa, aż pomyślnie dostarczone polecenia ping.

2.  Kolejka losową zaległości jest zaznaczone.

3.  Obiekty [BrokeredMessage][] są kierowane do kolejki wybranym zaległości.

1.  Jeśli operacja wysyłania do kolejki wybranym zaległości nie powiedzie się, kolejki są pobierane z obrót i nową kolejkę jest zaznaczone. Dowiedz się, że wszystkich nadawców na wystąpienie [MessagingFactory][] błędu.

Poniższej ilustracji przedstawić sekwencji. Najpierw nadawcy wysyła wiadomości.

![ILOCZYNÓW nazw][0]

W przypadku awarii wysyłanie do podstawowego kolejki nadawcy rozpocznie proces wysyłania wiadomości do kolejki losowo wybranych zaległości. Jednocześnie zostanie uruchomiony zadania ping.

![ILOCZYNÓW nazw][1]

W tym momencie wiadomości nadal znajdują się w kolejce pomocniczej, a nie zostały dostarczone do kolejki podstawowego. Gdy podstawowy kolejka jest prawidłowy ponownie, co najmniej jednego procesu powinna być uruchomiona syphon. Syphon udostępnia wiadomości ze wszystkich różnych kolejek zaległości do jednostek docelowego pisane z wielkiej litery (kolejek i tematy).

![ILOCZYNÓW nazw][2]

Pozostała część w tym temacie omówiono określonych szczegółach sposób działania tych elementów.

## <a name="creation-of-backlog-queues"></a>Tworzenie kolejek zaległości

Obiekt [SendAvailabilityPairedNamespaceOptions][] przekazany do metody [PairNamespaceAsync][] wskazuje liczbę kolejek zaległości, którego chcesz użyć. Każda kolejka zaległości zostanie utworzona o następujących właściwościach jawnie zestawu (wszystkie inne wartości są ustawione na wartości domyślne [QueueDescription][] ):

| Ścieżka                                   | [podstawowy nazw] / x servicebus przesyłania / [index] [indeksu w przypadku wartości [0, BacklogQueueCount) |
|----------------------------------------|------------------------------------------------------------------------------------------------------|
| MaxSizeInMegabytes                     | 5120                                                                                                 |
| MaxDeliveryCount                       | wewnętrznej Wartość MaxValue                                                                                         |
| DefaultMessageTimeToLive               | TimeSpan.MaxValue                                                                                    |
| AutoDeleteOnIdle                       | TimeSpan.MaxValue                                                                                    |
| LockDuration                           | 1 minuta                                                                                             |
| EnableDeadLetteringOnMessageExpiration | wartość PRAWDA.                                                                                                 |
| EnableBatchedOperations                | wartość PRAWDA.                                                                                                 |

Na przykład z pierwszej kolejki zaległości utworzone na nazwie nazw **contoso** `contoso/x-servicebus-transfer/0`.

Podczas tworzenia kolejek, kod najpierw sprawdza, czy istnieje takiej kolejki. Jeśli kolejka nie istnieje, zostanie utworzony kolejki. Kod nie Oczyszczanie kolejek zaległości "dodatkowe". W szczególności wniosek aplikacji z nazw podstawowego **contoso** pięć zaległości kolejek, ale kolejki zaległości ścieżką `contoso/x-servicebus-transfer/7` istnieje, kolejki dodatkowe zaległości jest nadal zainstalowany, ale nie jest używane. System umożliwia jawnie kolejek dodatkowe zaległości istnieje, które nie będą używane. Jako właściciel nazw odpowiadasz za czyszczenie wszystkich kolejek nieużywane niechciane zaległości. Przyczyna tę decyzję jest Bus usługi nie wiesz, jakiego celów istnieje dla wszystkich kolejek w obszarze nazw. Ponadto jeśli istnieje o podanej nazwie kolejki, ale nie spełnia założonej [QueueDescription][], z powodów, dla których to własne zmienić domyślne zachowanie. Nie gwarancji zostały wprowadzone do modyfikacji do kolejek zaległości w kodzie. Upewnij się dokładnie sprawdzić wprowadzone zmiany.

## <a name="custom-messagesender"></a>Niestandardowe MessageSender

Podczas wysyłania, wszystkie wiadomości obsłużone obiektu [MessageSender][] wewnętrzny, który zachowuje się zwykle, kiedy wszystko działa, a przekierowuje do kolejek zaległości podczas czynności "przerwać." Po otrzymaniu awarii nie zmiennych, zostanie uruchomiony czasomierza. Po upływie [przedziału czasu][] składający się z wartości właściwości [FailoverInterval][] , w którym są wysyłane wiadomości nie powiedzie tym przełączeniu jest włączone. W tym momencie wystąpić, następujące czynności dla każdego obiektu:

- Zadania ping wykonuje każdy [PingPrimaryInterval][] , aby sprawdzić, czy jednostka jest dostępna. Po pomyślnym to zadanie cały kod klienta korzystającego z jednostką natychmiast rozpoczyna, wysyłanie nowych wiadomości do podstawowego nazw.

- Przyszłe żądania, aby wysłać do tego samego obiektu od innych nadawców spowoduje [BrokeredMessage][] wysyłana do zmodyfikowania usiąść w kolejce zaległości. Zmiana usuwa niektóre właściwości z obiektu [BrokeredMessage][] i zapisuje je w innym miejscu. Poniższe właściwości są wyczyszczone i dodany w obszarze Nowy alias, umożliwiając Bus usługi i SDK równomiernie przetwarzania wiadomości:

| Stara nazwa właściwości       | Nowa nazwa właściwości |
|-------------------------|-------------------|
| Identyfikator sesji               | x-ms-identyfikator sesji    |
| Licznika TimeToLive              | x-ms-licznika timetolive   |
| ScheduledEnqueueTimeUtc | x-ms-path         |

Oryginalny ścieżka docelowa także będzie przechowywana w wiadomości jako właściwość o nazwie x-ms-path. Ten projekt umożliwia wiadomości dla wielu obiektów współistnienie w kolejce pojedynczy zaległości. Właściwości są przekształcane przez syphon.

Niestandardowy obiekt [MessageSender][] mogą występować problemy z wiadomości dojechać limit 256 KB i prowadzi pracy awaryjnej. Niestandardowy obiekt [MessageSender][] przechowuje wiadomości dla wszystkich kolejek i tematy razem w kolejce zaległości. Ten obiekt miesza wiadomości z wielu kolory podstawowe razem w kolejkach zaległości. Obsługę równoważenia spośród wielu klientów, którzy nie znają siebie, zestawu SDK losowo wybiera jedną kolejkę zaległości dla każdego [QueueClient][] lub [TopicClient][] , możesz utworzyć w kodzie.

## <a name="pings"></a>Polecenia ping

Wiadomość ping jest pusty [BrokeredMessage][] z jego właściwość [Typ zawartości][] , ustaw aplikacji/vnd.ms-servicebus-ping i wartość [licznika TimeToLive][] 1 sekundę. To polecenie ping ma jednej właściwości specjalne w Bus usługi: gdy dowolny rozmówcy zażąda [BrokeredMessage][]serwer nigdy nie zapewnia polecenie ping w czasie. W ten sposób nie trzeba Dowiedz się, jak i odbierania zignorować te wiadomości. Będzie można wykonywać polecenia ping każdej jednostki (kolejki unikatowe lub temat) dla każdego wystąpienia [MessagingFactory][] na klienta, gdy są one uznawane za niedostępny. Domyślnie w takim przypadku raz na minutę. Polecenie ping wiadomości są uznawane za zwykłych wiadomości Bus usługi, a może spowodować opłaty za przepustowości i wiadomości. Jak klienci wykryje, że system jest dostępne, Zatrzymaj wiadomości.

## <a name="the-syphon"></a>Syphon

Co najmniej jeden program wykonywalny w aplikacji powinna być aktywnie uruchomiona syphon. Syphon wykonuje typu Liczba długa odbieranie ankiety, które trwa 15 minut. Gdy wszystkie obiekty są dostępne i masz 10 kolejek zaległości, aplikacji, która obsługuje syphon połączeń operacji odbioru 40 godzin na godzinę, 960 godzin dziennie, 28800 razy w 30 dni. Gdy syphon jest aktywnie przenoszenie wiadomości od zaległości do podstawowego kolejki, każda wiadomość występują następujące opłaty (standardowe rozmiaru wiadomości i przepustowość opłata we wszystkich etapach):

1.  Wyślij do zaległości.

2.  Otrzymać od zaległości.

3.  Wyślij do podstawowej.

4.  Odbieranie z serwera podstawowego.

## <a name="closefault-behavior"></a>Zamknij i błędów zachowanie

W aplikacji, która obsługuje syphon, raz błędów [MessagingFactory][] podstawowy i pomocniczy lub jest zamknięty bez partnera również są umieszczone zamknięte i syphon wykryje ten stan działania syphon. Jeśli inne [MessagingFactory][] nie jest zamknięte w 5 sekund, syphon będzie błąd nadal otwarty [MessagingFactory][].

## <a name="next-steps"></a>Następne kroki

Szczegółowe omówienie usługi Bus asynchroniczne wiadomości, zobacz [asynchroniczne wzorców wiadomości i wysokiej dostępności][] . 

  [PairNamespaceAsync]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.pairnamespaceasync.aspx
  [SendAvailabilityPairedNamespaceOptions]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.aspx
  [MessageSender]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx
  [MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [FailoverInterval]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.pairednamespaceoptions.failoverinterval.aspx
  [MessagingException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
  [TimeoutException]: https://msdn.microsoft.com/library/azure/system.timeoutexception.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  [QueueDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx
  [Przedziału czasu]: https://msdn.microsoft.com/library/azure/system.timespan.aspx
  [PingPrimaryInterval]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.pingprimaryinterval.aspx
  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx
  [Typ zawartości]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx
  [Licznika TimeToLive]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx
  [Asynchroniczne wzorców wiadomości i wysokiej dostępności]: service-bus-async-messaging.md
  [0]: ./media/service-bus-paired-namespaces/IC673405.png
  [1]: ./media/service-bus-paired-namespaces/IC673406.png
  [2]: ./media/service-bus-paired-namespaces/IC673407.png
