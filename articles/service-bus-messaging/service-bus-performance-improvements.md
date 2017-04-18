<properties 
    pageTitle="Najważniejsze wskazówki dotyczące zwiększania wydajności przy użyciu usługi Bus | Microsoft Azure"
    description="Informacje dotyczące używania Bus usługi Azure optymalizacji wydajności podczas wymiany brokered wiadomości."
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
    ms.date="10/25/2016"
    ms.author="sethm" />

# <a name="best-practices-for-performance-improvements-using-service-bus-messaging"></a>Najważniejsze wskazówki dotyczące ulepszenia dotyczące wydajności przy użyciu usługi Bus wiadomości

W tym temacie opisano, jak za pomocą wiadomości Bus usługi Azure optymalizacji wydajności, gdy wymiana brokered wiadomości. Pierwsza część w tym temacie opisano różne mechanizmy, które są dostępne dla w celu zwiększenia wydajności. Druga część zawiera wskazówki dotyczące używania usługi Bus w sposób, który można oferują najlepszą wydajność w danej sytuacji.

W tym temacie termin "klient" oznacza osobę, która uzyskuje dostęp do usług Bus. Klient może przejąć rolę nadawcy lub odbiorcy. Termin "nadawca" jest używany do Bus usługi kolejki lub temat klienta, który będzie wysyłać wiadomości do kolejki Bus usługi lub temat. Termin "odbiorca" odwołuje się do usługi Bus kolejki lub subskrypcji klienta, który odbiera wiadomości z kolejki Bus usługi lub subskrypcji.

Poniższe sekcje wprowadzić kilka pojęcia, które usługi Bus użyto, aby zwiększyć wydajność zwiększenie wydajności.

## <a name="protocols"></a>Protokoły

Usługa Bus umożliwia klientom wysyłać i odbierać wiadomości przy użyciu trzy protokoły

1. Zaawansowane kolejkowanie Protocol (AMQP)

2. Usługa Bus wiadomości Protocol (SBMP)

3. HTTP

Zarówno AMQP, jak i SBMP są bardziej efektywne, ponieważ zachowują połączenie usługi Bus, ile istnieje factory wiadomości. Wprowadza również tworzeniu partii oraz Odczyt z wyprzedzeniem. Chyba że jawnie wymienione cała zawartość w tym temacie założono stosowania AMQP lub SBMP.

## <a name="reusing-factories-and-clients"></a>Ponowne używanie fabryki i klientami

Obiekty klienta Bus usługi, takie jak [QueueClient][] lub [MessageSender][]są tworzone przez obiektu [MessagingFactory][] , który udostępnia wewnętrznych Zarządzanie połączeniami. Wiadomości fabryki lub kolejki, temacie i klienci subskrypcji należy zamknąć nie po wysłać wiadomość, a następnie ponownie utworzyć je, gdy wysyłasz następnej wiadomości. Zamknięcie wiadomości factory Usuwa połączenie z usługą Bus usługi, a nowe połączenie jest nawiązywane podczas odtwarzania fabryki. Nawiązywania połączenia jest drogich operację, której można uniknąć ponownego korzystając z samej fabryki i obiekty klienta dla wielu operacji. Obiekt [QueueClient][] może być bezpiecznie używany do wysyłania wiadomości z równoczesne operacje asynchroniczne i wiele wątków. 

## <a name="concurrent-operations"></a>Równoczesne wykonywanie operacji

Wykonywanie operacji (wysyłanie, odbieranie, usuwanie, itp.) wymaga pewnego czasu. Tym razem obejmuje przetwarzanie operacji przez usługę Bus usługi oprócz opóźnienie żądania i odpowiedzi. Aby zwiększyć liczbę operacji na czas, czynności należy wykonać jednocześnie. Można to zrobić na kilka sposobów:

-   **Operacje asynchroniczne**: klient planuje operacji, wykonując operacje asynchroniczne. Przy następnym żądaniu jest uruchamiany przed zakończeniem poprzedniego żądania. Oto przykład operacji wysyłania asynchroniczne:

    ```
    BrokeredMessage m1 = new BrokeredMessage(body);
    BrokeredMessage m2 = new BrokeredMessage(body);
    
    Task send1 = queueClient.SendAsync(m1).ContinueWith((t) => 
      {
        Console.WriteLine("Sent message #1");
      });
    Task send2 = queueClient.SendAsync(m2).ContinueWith((t) => 
      {
        Console.WriteLine("Sent message #2");
      });
    Task.WaitAll(send1, send2);
    Console.WriteLine("All messages sent");
    ```

    Przykład asynchroniczne operacji odbioru:
    
    ```
    Task receive1 = queueClient.ReceiveAsync().ContinueWith(ProcessReceivedMessage);
    Task receive2 = queueClient.ReceiveAsync().ContinueWith(ProcessReceivedMessage);
    
    Task.WaitAll(receive1, receive2);
    Console.WriteLine("All messages received");
    
    async void ProcessReceivedMessage(Task<BrokeredMessage> t)
    {
      BrokeredMessage m = t.Result;
      Console.WriteLine("{0} received", m.Label);
      await m.CompleteAsync();
      Console.WriteLine("{0} complete", m.Label);
    }
    ```

-   **Wiele fabryki**: wszystkich klientów (nadawców oprócz odbiorców), które są tworzone przez tej samej fabryki udostępnianie jednego połączenia TCP. Przepustowość maksymalna liczba wiadomości jest ograniczona przez liczbę operacji, które można przejść przez to połączenie TCP. Przepustowość, które można uzyskać przy użyciu tego samego zakładu znacznie zależy od czasami opóźnienia TCP i rozmiar wiadomości. Aby uzyskać wyższy przepustowość, należy użyć wielu fabryki wiadomości.

## <a name="receive-mode"></a>Tryb odbierania

Podczas tworzenia kolejki lub subskrypcji klienta, można określić tryb odbierania: *Zablokuj wglądu* lub *Odbieranie i Usuń*. Odbieranie domyślny tryb jest [PeekLock][]. Podczas pracy w tym trybie, klient wysyła żądanie komunikat o błędzie z Bus usługi. Po otrzymaniu wiadomości klient wysyła żądanie do wykonania wiadomości.

Po ustawieniu trybu Odbierz na [ReceiveAndDelete][], obu kroków są łączone w pojedyncze żądanie. Zmniejsza ogólne liczbę operacji i może zwiększyć ogólną wydajność obsługi wiadomości. Jest to wzmocnienia wydajności ryzyko utraty wiadomości.

Usługa Bus nie obsługuje transakcji dla operacji odbieranie i usuwania. Ponadto Zablokuj wglądu do znaczeń właściwych są wymagane scenariusze, w których chce odroczyć klienta lub [utraconych](service-bus-dead-letter-queues.md) wiadomości.

## <a name="client-side-batching"></a>Tworzeniu partii po stronie klienta

Po stronie klienta tworzeniu partii umożliwia kolejki lub temat klientowi opóźnienie wysyłania wiadomości przez pewien okres czasu. Jeśli klient wysyła dodatkowe wiadomości w tym okresie, przesyła wiadomości w ramach jednej partii. Po stronie klienta tworzeniu partii również powoduje, że klient kolejki/subskrypcji partii wiele żądań **wykonane** do pojedynczego żądania. Tworzeniu partii jest dostępna tylko dla operacji **wysyłania** i **Wykonano** asynchronicznych. Obraz operacji natychmiast są przesyłane do usługi Bus usługi. Tworzeniu partii nie występują dla wglądu ani otrzymywać operacji nie występuje tworzeniu partii wielu klientów.

Jeśli partii przekracza maksymalny rozmiar wiadomości, ostatnią wiadomość zostanie usunięte z partię, a klient wysyła natychmiast partii. Ostatnią wiadomość staje się pierwszą wiadomość następnej partii. Domyślnie klient używa przedział partii 20 MS. Możesz zmienić interwał partii przez ustawienie właściwości [BatchFlushInterval][] przed utworzeniem factory wiadomości. To ustawienie ma wpływ na wszystkich klientów, którzy są tworzone przez ten factory.

Aby wyłączyć tworzeniu partii, ustawić właściwość [BatchFlushInterval][] **TimeSpan.Zero**. Na przykład:

```
MessagingFactorySettings mfs = new MessagingFactorySettings();
mfs.TokenProvider = tokenProvider;
mfs.NetMessagingTransportSettings.BatchFlushInterval = TimeSpan.FromSeconds(0.05);
MessagingFactory messagingFactory = MessagingFactory.Create(namespaceUri, mfs);
```

Tworzeniu partii nie wpływa na liczbę fakturowanego operacji wiadomości i jest dostępna tylko dla protokołu Bus usługi klienta. Protokół HTTP nie obsługuje tworzeniu partii.

## <a name="batching-store-access"></a>Tworzeniu partii dostęp do sklepu

Aby zwiększyć przepustowość kolejki tematu i subskrypcji, Bus usługi partii wiele wiadomości, czy zapisuje w magazynie wewnętrzny. Jeśli włączony w kolejce lub tematu, pisanie wiadomości w magazynie będzie można przetwarzany wsadowo. Jeśli włączony w kolejce lub innej subskrypcji, usuwanie wiadomości z magazynu będzie można przetwarzany wsadowo. Po włączeniu dostęp do sklepu wsadowej obiektu usługi Bus opóźnienia operacji zapisu magazynu dotyczące tego obiektu przez maksymalnie 20 MS. Operacje dodatkowe sklepu, występujących w tym zakresie zostaną dodane do partii. Dostęp do sklepu wsadowej dotyczy tylko operacji **wysyłania** i **Wykonano** ; odbieranie operacje problem nie występuje. Dostęp do sklepu wsadowej jest właściwością na obiekt. Tworzeniu partii występuje przez wszystkie obiekty, które umożliwiają dostęp do sklepu wsadowej.

Podczas tworzenia nowej kolejki, tematu lub subskrypcji, dostęp do sklepu wsadowej jest domyślnie włączona. Aby wyłączyć dostęp do sklepu wsadowej, należy ustawić właściwość [EnableBatchedOperations][] na **wartość false** przed utworzeniem jednostki. Na przykład:

```
QueueDescription qd = new QueueDescription();
qd.EnableBatchedOperations = false;
Queue q = namespaceManager.CreateQueue(qd);
```

Dostęp do sklepu wsadowej nie wpływa na liczbę fakturowanego operacji wiadomości i jest właściwością kolejki, tematu lub subskrypcji. Nie zależy od sposobu odbierania i protokół, który jest używany między klientem a usługą Bus usługi.

## <a name="prefetching"></a>Odczyt z wyprzedzeniem

Odczyt z wyprzedzeniem umożliwia klienta kolejki lub subskrypcji, aby załadować dodatkowe wiadomości przez usługę, podczas wykonywania operacji odbioru. Klient przechowuje tych wiadomości w lokalnej pamięci podręcznej. Rozmiar pamięci podręcznej zależy od właściwości [QueueClient.PrefetchCount][] lub [SubscriptionClient.PrefetchCount][] . Każdy klient, która umożliwia odczyt z wyprzedzeniem zachowuje swojej własnej pamięci podręcznej. Pamięci podręcznej nie są udostępniane przez klientów. Jeśli klient inicjuje operację Odbierz i pamięci podręcznej jest pusta, usługa przesyła partię wiadomości. Rozmiar partii jest równa rozmiar pamięci podręcznej lub 256KB, zależnie od jest mniejszy. Jeśli klient inicjuje operację Odbierz i pamięci podręcznej zawiera wiadomości, wiadomość jest pobierana z pamięci podręcznej.

Gdy wiadomość jest prefetched, usługa blokuje prefetched wiadomości. Dzięki temu prefetched wiadomości nie można otrzymać innego odbiorcy. Jeśli odbiorca nie można ukończyć wiadomość, przed wygaśnięciem blokady, wiadomość staje się dostępna dla innych odbiorców. Prefetched kopia wiadomości jest przechowywana w pamięci podręcznej. Odbiorca korzystającym wygasłe buforowanej kopii otrzymają wyjątku podczas próby wykonania tej wiadomości. Blokowanie wiadomości domyślnie Dezaktualizuje się po 60 sekund. Ta wartość może zostać wydłużony 5 minut. Aby zapobiec zużycie wygasłe wiadomości, rozmiar pamięci podręcznej zawsze powinien być mniejszy niż liczba wiadomości, które mogą być wykorzystane przez klienta w interwał limitu czasu blokady.

Gdy używasz po upływie blokady domyślny 60 sekund, warto wartość [SubscriptionClient.PrefetchCount][] jest 20 godzin stawki maksymalna przetwarzanie wszystkich odbiorców fabryki. Na przykład fabryki tworzy odbiorców 3, a każdy odbiorca może przetworzyć maksymalnie 10 wiadomości na sekundę. Liczba pobrane z wyprzedzeniem nie może przekraczać 20\*3\*10 = 600. Domyślnie [QueueClient.PrefetchCount][] jest równa 0, co oznacza, że żadnych dodatkowych wiadomości są pobierane z usługi.

Odczyt z wyprzedzeniem wiadomości zwiększa ogólną wydajność kolejki lub subskrypcji, ponieważ jej zmniejsza ogólną liczbę operacji wiadomości lub przesłania. Pobieranie pierwszej wiadomości, jednak wydłuży (ze względu na rozmiar wiadomości lepszą). Odbieranie wiadomości prefetched będzie przebiegać szybciej, ponieważ te wiadomości zostały pobrane przez klienta.

Właściwość time to live (TTL), wiadomości jest zaznaczone pole wyboru przez serwer w czasie serwer wysyła wiadomość do klienta. Klient nie sprawdza właściwości TTL wiadomości po odebraniu wiadomości. Zamiast tego mogą być odbierane wiadomości, nawet wtedy, gdy minął TTL wiadomości, gdy wiadomość została buforowane przez klienta.

Odczyt z wyprzedzeniem nie wpływa na liczbę fakturowanego operacji wiadomości i jest dostępna tylko dla protokołu Bus usługi klienta. Protokół HTTP nie obsługuje odczyt z wyprzedzeniem. Odczyt z wyprzedzeniem jest dostępna dla synchroniczne i asynchroniczne operacje odbioru.

## <a name="express-queues-and-topics"></a>Express kolejki i tematy

Jednostki Express Włącz wysokiej wydajności i zmniejszenia opóźnienie scenariuszy. Z osobami ekspresowe Jeśli wiadomość jest wysyłana do kolejki lub temat wiadomości nie natychmiast znajduje się w magazynie wiadomości. Zamiast tego go w pamięci podręcznej. Jeśli wiadomość jest przechowywana w kolejce więcej niż kilka sekund, automatycznie są zapisywane na stabilny miejscem do magazynowania, więc ochrona przed utratą z powodu awarii. Pisanie wiadomości w pamięci podręcznej zwiększa przepustowość i ogranicza czas zwłoki, ponieważ nie ma dostępu do stabilny miejsca do magazynowania w chwili, gdy wiadomość jest wysyłana. Wiadomości, które są używane w ciągu kilku sekund nie są zapisane w magazynie wiadomości. Poniższy przykład tworzy express tematu.

```
TopicDescription td = new TopicDescription(TopicName);
td.EnableExpress = true;
namespaceManager.CreateTopic(td);
```

Jeśli wiadomość zawierającą krytyczne informacje, które nie musi zostać utracone są wysyłane do express jednostki, nadawcy można wymusić Bus usługi, aby od razu wiadomości stabilny miejsca do magazynowania, ustawiając właściwość [ForcePersistence][] na **wartość true**.

## <a name="use-of-partitioned-queues-or-topics"></a>Stosowanie kolejki podzielone na partycje lub tematów

Wewnętrznie Bus usługi korzysta z tym samym węźle i wiadomości przechowywane w celu przetwarzania i przechowywania wszystkich wiadomości SMS jednostki (kolejki lub temat). Kolejka podzielone na partycje lub tematów, z drugiej strony, jest rozkładana w wielu węzły i Sklepy wiadomości. Podzielone na partycje kolejki i tematy nie tylko rentowność wyższej przepustowości niż zwykłe kolejki i tematy, są również powodować wystąpienie najwyższej dostępności. Aby utworzyć obiekt podzielone na partycje, należy ustawić właściwość [EnablePartitioning][] na **wartość PRAWDA**, jak pokazano w poniższym przykładzie. Aby uzyskać więcej informacji na temat jednostki podzielone na partycje Zobacz [partycją podmioty wiadomości][].

```
// Create partitioned queue.
QueueDescription qd = new QueueDescription(QueueName);
qd.EnablePartitioning = true;
namespaceManager.CreateQueue(qd);
```

## <a name="use-of-multiple-queues"></a>Używanie wielu kolejek

Jeśli nie jest możliwe korzystanie z kolejki podzielone na partycje lub tematu lub oczekiwanych obciążenia nie są obsługiwane przez kolejka podzielone na partycje lub temat, należy użyć wiele jednostek wiadomości. Gdy używasz wiele jednostek, Utwórz dedykowane klienta dla każdej jednostki, zamiast tego samego klienta dla wszystkich obiektów.

## <a name="development--testing-features"></a>Opracowywania i testowania funkcji

Usługa Bus ma jedną funkcję, która jest używana specjalnie dla rozwoju, które **nie mogą być używane w konfiguracji produkcji**.

[TopicDescription.EnableFilteringMessagesBeforePublishing](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.enablefilteringmessagesbeforepublishing.aspx)
- Po dodaniu nowej reguły lub filtry do tematu, EnableFilteringMessagesBeforePublishing można sprawdzić, czy nowe wyrażenie filtru działa zgodnie z oczekiwaniami.

## <a name="scenarios"></a>Scenariusze

W poniższych sekcjach opisano typowe scenariusze wiadomości i konspekt preferowane ustawienia Bus usługi. Przepustowość są klasyfikowane jako małe (mniej niż 1 wiadomości na sekundę), średni (sekundę wiadomości 1 lub większy, ale mniej niż 100 wiadomości na sekundę) oraz wysokiego (100 wiadomości/drugiej lub większych). Liczba klientów należą do małych umiarkowane (5 lub mniej), (więcej niż 5, ale mniejsza niż lub równa 20), a duży (więcej niż 20).

### <a name="high-throughput-queue"></a>Kolejka wysokiej przepustowości

Cel: Maksymalizowanie przepustowość pojedynczej kolejki. Liczba nadawcy i odbiorcy jest mały.

-   Zwiększona wydajność i dostępność za pomocą kolejkę podzielone na partycje.

-   Aby zwiększyć ogólną szybkość wysyłania w kolejce, należy utworzyć nadawców wielu fabryki wiadomości. Do każdego nadawcy użyj operacji asynchronicznych lub wiele wątków.

-   Aby zwiększyć ogólną szybkość odbierania z kolejki, należy utworzyć odbiorców wielu fabryki wiadomości.

-   Użyj operacji asynchronicznych skorzystać z tworzeniu partii po stronie klienta.

-   Ustawianie interwału łączenia we wsady do 50ms, aby zmniejszyć liczbę Bus usługi transmisji protokołu klienta. Użycie wielu nadawców zwiększyć łączenia we wsady interwał do 100ms.

-   Pozostaw włączony dostęp wsadowej magazynu. Zwiększa ogólną szybkość, w którym można zapisywać wiadomości w kolejce.

-   Ustawianie liczby pobrane z wyprzedzeniem 20 godzin stawki maksymalna przetwarzanie wszystkich odbiorców fabryki. Zmniejsza liczbę Bus usługi transmisji protokołu klienta.

### <a name="multiple-high-throughput-queues"></a>Wielu kolejek wysokiej przepustowości

Cel: Maksymalizowanie ogólną wydajność wielu kolejek. Przepustowość pojedynczej kolejki jest średni lub wysoki.

Uzyskanie maksymalna przepustowość u wielu kolejek, za pomocą ustawień konspekt maksymalizować przepustowość pojedynczej kolejki. Ponadto za pomocą różnych fabryki tworzy klientów, które wysyłać lub odbierać z różnych kolejek.

### <a name="low-latency-queue"></a>Krótki czas oczekiwania kolejki

Cel: Zminimalizować opóźnienie zakończenia do końca kolejki lub temat. Liczba nadawcy i odbiorcy jest mały. Przepustowość kolejki jest mała lub średnia.

-   Ulepszone dostępności za pomocą kolejkę podzielone na partycje.

-   Wyłącz tworzeniu partii po stronie klienta. Klient wysyła od razu wiadomości.

-   Wyłącz dostęp do sklepu wsadowej. Usługa natychmiast zapisuje wiadomość w magazynie.

-   Jeśli z jednego klienta, ustaw 20 godzin przetwarzanie stopień odbiorca statystykę pobrane z wyprzedzeniem. Jeśli wiele wiadomości przychodzących kolejki w tym samym czasie, protokół klienta usługi Bus przesyła je wszystkie jednocześnie. Gdy klient odbierze następnej wiadomości, wiadomości, jest już lokalnej pamięci podręcznej. Pamięć podręczną powinna być niewielka.

-   Jeśli używasz wielu klientów, równa 0 statystykę pobrane z wyprzedzeniem. Dzięki temu drugi klient może komunikat drugi podczas pierwszego klienta jest przetwarzana pierwszą wiadomość.

### <a name="queue-with-a-large-number-of-senders"></a>Kolejki z dużą liczbą nadawców

Cel: Maksymalizowanie przepustowość kolejki lub tematu z dużą liczbą nadawców. Każdemu nadawcy wysyła wiadomości o średnim częstotliwości. Liczba odbiorców jest mała.

Usługa Bus umożliwia do 1000 równoczesne połączenia z podmiotem wiadomości (lub 5000 przy użyciu AMQP). Ten limit są wymuszane na poziomie nazw i kolejek tematy i subskrypcje są ograniczone przez limit połączenia równoczesne według nazw. W przypadku kolejek tylu są udostępniane między nadawcy i odbiorcy. Jeśli wszystkie połączenia 1000 są wymagane do nadawców, należy zastąpić kolejki z tematu i pojedynczej subskrypcji. Temat akceptuje do 1000 równoczesne połączenia z nadawców, dlatego subskrypcji przyjmuje dodatkowe połączenia równoczesne 1000 z odbiorców. Jeśli więcej niż 1000 nadawców równoczesne są wymagane, nadawców należy wysyłać wiadomości do protokołu Bus usługi za pośrednictwem protokołu HTTP.

Aby zmaksymalizować przepustowość, wykonaj następujące czynności:

-   Zwiększona wydajność i dostępność za pomocą kolejkę podzielone na partycje.

-   Jeśli każdego nadawcy znajduje się w inny proces, za pomocą jednego factory na proces.

-   Użyj operacji asynchronicznych skorzystać z tworzeniu partii po stronie klienta.

-   Zmniejsz liczbę Bus usługi transmisji protokołu klienta za pomocą domyślnego tworzeniu partii interwału 20 MS.

-   Pozostaw włączony dostęp wsadowej magazynu. Zwiększa ogólną szybkość, w którym można zapisywać wiadomości w kolejce lub temat.

-   Ustawianie liczby pobrane z wyprzedzeniem 20 godzin stawki maksymalna przetwarzanie wszystkich odbiorców fabryki. Zmniejsza liczbę Bus usługi transmisji protokołu klienta.

### <a name="queue-with-a-large-number-of-receivers"></a>Kolejki z dużej liczby odbiorców

Cel: Maksymalizowanie stawkę odbierania kolejka lub subskrypcję z dużej liczby odbiorców. Każdy odbiorca otrzyma wiadomości stawki średnim. Liczba nadawców jest mała.

Usługa Bus umożliwia do 1000 równoczesne połączenia do jednostki. Jeśli kolejki wymaga więcej niż 1000 odbiorców, należy zastąpić kolejki z tematu i wiele subskrypcji. Każdej subskrypcji w stanie obsługiwać maksymalnie 1000 równoczesne połączenia. Możesz też odbiorców dostęp do kolejki za pośrednictwem protokołu HTTP.

Aby zmaksymalizować przepustowość, wykonaj następujące czynności:

-   Zwiększona wydajność i dostępność za pomocą kolejkę podzielone na partycje.

-   Każdy odbiorca znajduje się w inny proces, należy użyć jednej fabrycznych na proces.

-   Odbiorców można używać operacji obraz lub asynchroniczna. Mieć stopa średnim Odbierz poszczególnych odbiorcy, po stronie klienta tworzeniu partii żądania wykonania nie wpływa na przepustowość odbiorcy.

-   Pozostaw włączony dostęp wsadowej magazynu. Zmniejsza całkowite obciążenie jednostki. Zmniejsza ogólnego wskaźnika, w którym można zapisywać wiadomości w kolejce lub temat.

-   Ustawianie liczby prefetch małej wartości (na przykład PrefetchCount = 10). Zapobiega to bezczynności, podczas gdy innych odbiorców dużej liczby wiadomości przechowywanych w pamięci podręcznej odbiorców.

### <a name="topic-with-a-small-number-of-subscriptions"></a>Temat z niewielką liczbą subskrypcji

Cel: Maksymalizowanie przepustowość temat z niewielką liczbą subskrypcji. Komunikat otrzymania przez wiele subskrypcji, co oznacza, że stawki Scalonej Odbierz dla wszystkich subskrypcji jest większy niż szybkość wysyłania. Liczba nadawców jest mała. Liczba odbiorców na subskrypcję jest mała.

Aby zmaksymalizować przepustowość, wykonaj następujące czynności:

-   Zwiększona wydajność i dostępność za pomocą podzielone na partycje tematu.

-   Aby zwiększyć ogólną szybkość wysyłania do tematu, należy utworzyć nadawców wielu fabryki wiadomości. Do każdego nadawcy użyj operacji asynchronicznych lub wiele wątków.

-   Aby zwiększyć ogólną szybkość odbierania z subskrypcji, należy utworzyć odbiorców wielu fabryki wiadomości. Dla każdego adresata użyj operacji asynchronicznych lub wiele wątków.

-   Użyj operacji asynchronicznych skorzystać z tworzeniu partii po stronie klienta.

-   Zmniejsz liczbę Bus usługi transmisji protokołu klienta za pomocą domyślnego tworzeniu partii interwał 20 MS.

-   Pozostaw włączony dostęp wsadowej magazynu. Zwiększa ogólnego wskaźnika, w którym można zapisywać wiadomości w tym temacie.

-   Ustawianie liczby pobrane z wyprzedzeniem 20 godzin stawki maksymalna przetwarzanie wszystkich odbiorców fabryki. Zmniejsza liczbę Bus usługi transmisji protokołu klienta.

### <a name="topic-with-a-large-number-of-subscriptions"></a>Temat z dużą liczbą subskrypcji

Cel: Maksymalizowanie przepustowość tematu z dużą liczbą subskrypcji. Wiadomość zostanie odebrana przez wiele subskrypcji, co oznacza, że stawka Scalonej Odbierz dla wszystkich subskrypcji jest znacznie większy niż szybkość wysyłania. Liczba nadawców jest mała. Liczba odbiorców na subskrypcję jest mała.

Tematy z dużą liczbą subskrypcji zwykle uwidaczniają niskiej przepustowości ogólnego, jeśli wszystkie wiadomości są kierowane do wszystkich subskrypcji. Jest to spowodowane przez fakt, że każda wiadomość jest odebrana tyle razy, a wszystkie wiadomości, które są zawarte w temacie i wszystkich jej subskrypcji są przechowywane w tym samym magazynie. Przyjmowana jest wartość numeru nadawców oraz liczby odbiorców na subskrypcję jest mała. Usługa Bus obsługuje maksymalnie 2000 subskrypcji na temat.

Aby zmaksymalizować przepustowość, wykonaj następujące czynności:

-   Zwiększona wydajność i dostępność za pomocą podzielone na partycje tematu.

-   Użyj operacji asynchronicznych skorzystać z tworzeniu partii po stronie klienta.

-   Zmniejsz liczbę Bus usługi transmisji protokołu klienta za pomocą domyślnego tworzeniu partii interwału 20 MS.

-   Pozostaw włączony dostęp wsadowej magazynu. Zwiększa ogólnego wskaźnika, w którym można zapisywać wiadomości w tym temacie.

-   Ustawianie liczby pobrane z wyprzedzeniem 20 godzin na szybkość odbierania oczekiwanych w sekundach. Zmniejsza liczbę Bus usługi transmisji protokołu klienta.

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat optymalizacji wydajności usługi Bus, zobacz [Partitioned wiadomości podmiotów][].

  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [MessageSender]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx
  [MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [PeekLock]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx
  [ReceiveAndDelete]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx
  [BatchFlushInterval]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.netmessagingtransportsettings.batchflushinterval.aspx
  [EnableBatchedOperations]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablebatchedoperations.aspx
  [QueueClient.PrefetchCount]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.prefetchcount.aspx
  [SubscriptionClient.PrefetchCount]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.prefetchcount.aspx
  [ForcePersistence]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.forcepersistence.aspx
  [EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx
  [Podzielony na partycje podmioty wiadomości]: service-bus-partitioning.md
  