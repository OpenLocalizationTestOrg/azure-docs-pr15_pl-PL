<properties 
    pageTitle="Usługa Bus asynchroniczne wiadomości | Microsoft Azure"
    description="Opis usługi Bus asynchroniczne wiadomości."
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

# <a name="asynchronous-messaging-patterns-and-high-availability"></a>Asynchroniczne wzorców wiadomości i wysokiej dostępności

Asynchroniczne wiadomości można zaimplementować na wiele różnych sposobów. Przy użyciu kolejki, tematy i subskrypcje Bus usługi Azure obsługuje asynchrony za pośrednictwem sklepu i mechanizm do przodu. W normalnych warunkach (obraz) można wysyłać wiadomości do kolejki i tematy i odbierać wiadomości z subskrypcji i kolejek. Aplikacje, którą piszesz zależą od tych obiektów zawsze będzie dostępna. Prawidłowość jednostki zmian ze względu na różnych okoliczności, konieczne jest sposobem udostępnienia jednostki możliwości ograniczona, która spełnia większość potrzeb.

Aplikacje zwykle umożliwia asynchroniczne desenie wiadomości włączyć wiele scenariusze komunikacji. Można tworzyć aplikacje, w których klienci mogą wysyłać wiadomości do usług, nawet wtedy, gdy usługa nie działa. Dla aplikacji tego środowiska, który seria komunikacja kolejki może pomóc w poziomie Załaduj, dostarczając miejsce do komunikacji buforu. Ponadto możesz uzyskać równoważenia obciążenia prostą, ale skutecznych do rozpowszechniania wiadomości na wielu komputerach.

Aby zachować dostępność dowolnej z tych obiektów, należy rozważyć, czy wiele różnych sposobów, w którym tych obiektów mogą być wyświetlane niedostępny dla trwałego systemu wiadomości. Ogólnie mówiąc widzimy jednostki stają się niedostępne dla aplikacji, które możemy pisanie w następujących sposobów:

- Nie można wysłać wiadomości.

- Nie można odbierać wiadomości.

- Nie można zarządzać jednostek (Tworzenie, pobieranie, aktualizowanie i usuwanie jednostki).

- Nie można skontaktować się z usługą.

Dla każdego z tych błędów Tryby awaryjne różnych istnieją włączyć aplikację na kontynuowanie pracy na poziomie kilka możliwości ograniczona. Na przykład systemu, w którym można wysyłać wiadomości, ale nie ich odbierać nadal może otrzymywać zamówienia od klientów, ale nie może przetworzyć tych zamówień. W tym temacie omówiono potencjalne problemy, które mogą wystąpić i jak są nieco ograniczane tych problemów. Usługa Bus została wprowadzona liczba czynniki, które należy wybrać do, a w tym temacie omówiono także zasady stosowania tych Wyraź zgodę na czynniki.

## <a name="reliability-in-service-bus"></a>Niezawodność w Bus usługi

Istnieje kilka sposobów do obsługi wiadomości i jednostki problemy, a istnieje wskazówek dotyczących zastosowania te czynniki. Aby uzyskać wskazówki, należy zrozumieć, co może się nie powieść, w Bus usługi. Ze względu na projekt systemów Azure wszystkie te problemy zwykle mają postać krótkotrwałe. Na wysokim poziomie różne przyczyny niedostępności wyglądać następująco:

-   Ograniczanie z systemem zewnętrznym, od którego zależy Bus usług. Ograniczanie występuje z interakcji z zasobami magazynowania i obliczeń.

-   Problemy dotyczące systemu, od którego zależy Bus usługi. Na przykład w określonej części miejsca do magazynowania mogą występować problemy.

-   Błąd usługi Bus na jednym podsystemu. W tej sytuacji węzeł obliczeń może uzyskać dostęp do niespójna i należy ponownie uruchomić, powoduje wszystkie obiekty, które służy do równoważenia do innych węzłów obciążenia. To z kolei może spowodować krótkim przetwarzania powolne wiadomości.

-   Błąd usługi Bus w Azure centrum danych. To, w którym system jest nieosiągalny za kilka minut lub kilka godzin "losowych błąd".

> [AZURE.NOTE] Określenie **miejsca do magazynowania** może oznaczać zarówno magazyn Azure i platformy SQL Azure.

Usługa Bus zawiera liczbę czynniki tych problemów. W poniższych sekcjach omówiono każdego problemu oraz ich odpowiednich czynniki.

### <a name="throttling"></a>Ograniczanie

Z Bus usługi ograniczania umożliwia zarządzanie stopa współpracy wiadomości. Każdy poszczególnych węzeł Bus usługi zawiera wiele jednostek. W systemie Procesora, pamięci, miejsca do magazynowania i inne aspekty każdej z tych jednostek służy do potrzeb. Jeśli dowolny z tych aspekty wykryje zastosowania przekraczającej progi zdefiniowane, odmówić danego żądania usługi Bus. Wywołujący otrzyma [ServerBusyException][] i prób po 10 sekund.

Jako łagodzenia kod należy Przeczytaj komunikat o błędzie i zatrzymanie wszelkie ponowne próby wiadomości dla co najmniej 10 sekund. Ponieważ błąd może wystąpić w części aplikacji klienta, oczekuje się, że każda niezależna wykonuje ponów próbę logicznych. Kod można zmniejszyć prawdopodobieństwo jest ograniczenie, umożliwiając podziału w kolejce lub temat.

### <a name="issue-for-an-azure-dependency"></a>Problem dotyczący zależność Azure

Inne składniki w Azure czasami może mieć problemy z usługą. Na przykład po uaktualnieniu systemu, która korzysta z usługi Bus systemu tymczasowo mogą wystąpić możliwości ograniczona. W celu obejścia tego typu problemów, usługa Bus regularnie bada i wykonuje czynniki. Efekty stronie następujące czynniki są wyświetlane. Na przykład obsługiwać przejściowych problemów z miejsca do magazynowania, usługa Bus wykonuje systemu, który umożliwia pracy spójne operacji wysyłania wiadomości. Ze względu na rodzaj łagodzenia wysłana wiadomość może zająć do 15 minut wyświetlane w kolejce dotyczy lub subskrypcji i gotowy do użycia odbierania. Ogólnie mówiąc, większości obiektów będzie ten problem nie występuje. Jednak podana liczba jednostek w Bus usługi w Azure, takie rozwiązanie jest czasami wymagane przez mały podzbiór klientów usługi Bus.

### <a name="service-bus-failure-on-a-single-subsystem"></a>Błąd usługi Bus jednego podsystemu

Z dowolną aplikacją okolicznościach mogą powodować wewnętrznych część usługi Bus spójności. Gdy usługa Bus wykryje to, zbiera dane z poziomu aplikacji do pomocy do diagnozowania, co się stało. Gdy dane są zbierane, ponownym uruchomieniu aplikacji w celu powraca do stanu spójne. Ten proces przebiega dość szybko, a wyniki w jednostce wyświetlane jako niedostępny dla maksymalnie kilka minut, chociaż typowe dół godziny są dużo krótsza.

W tych przypadkach z aplikacją kliencką generuje wyjątek [System.TimeoutException][] lub [MessagingException][] . Usługa Bus zawiera łagodzenia tego problemu w formularzu klienta automatyczną ponów próbę logicznych. Po wyczerpaniu okresu ponów próbę i wiadomość nie została dostarczona, można sprawdzić za pomocą innych funkcji, takich jak [iloczynów nazw][]. ILOCZYNÓW przestrzenie nazw mają inne ostrzeżenia, które zostały omówione w tym artykule.

### <a name="failure-of-service-bus-within-an-azure-datacenter"></a>Błąd usługi Bus w Azure centrum danych

Najbardziej prawdopodobne przyczyny błędu Azure centrum danych jest niepowodzenie wdrożenia uaktualnienia usługi Bus lub zależne systemu. Jak leżakowany platformę, ma zmniejszony prawdopodobieństwa tego typu błędu. Błąd centrum danych również może się zdarzyć, powodów, które zawierają następujące czynności:

-   Awaria elektrycznych (zasilania i generowania power znikają).
-   Łączność (internet podziału między klientami i Azure).

W obu przypadkach awarii fizycznych lub chemicznych spowodowały problem. Aby obejścia tego problemu i upewnij się, że nadal można wysyłać wiadomości, umożliwia [iloczynów nazw][] wiadomości były wysyłane do innej lokalizacji, podczas gdy lokalizacja podstawowa jest prawidłowy ponownie aktywować. Aby uzyskać więcej informacji zobacz [Najważniejsze wskazówki dotyczące izolacji aplikacji przed dostawie Bus usługi i awarii][].

## <a name="paired-namespaces"></a>ILOCZYNÓW nazw

Funkcja [iloczynów nazw][] obsługuje scenariusze, w którym jednostki Bus usługi lub rozmieszczania w centrum danych staje się niedostępna. Gdy to zdarzenie występuje rzadko, systemów dystrybucji nadal muszą mieć możliwość obsługi scenariusz najgorszego przypadku. Zazwyczaj zdarzenie określonego elementu, od którego zależy Bus usługi występują krótkoterminowy problem. Aby zachować dostępność aplikacji podczas awarii, użytkownicy usługi Bus umożliwia dwóch oddzielnych nazw, najlepiej w osobnych danych roboczych, udostępniać ich jednostek wiadomości. Pozostała część tej sekcji jest używane następujące terminologia:

-   Podstawowy nazw: nazw, z którym aplikacja interakcji, Wyślij i odbieranie operacji.

-   Pomocnicza nazw: przestrzeń nazw działająca jako kopii zapasowej do podstawowego nazw. Logikę aplikacji nie współdziała z tego obszaru nazw.

-   Interwał pracy awaryjnej: ilość czasu, aby zaakceptować normalny porażek przed aplikacji zmienia się z podstawowego nazw na pomocniczym nazw.

Sparowany, *Wysyłanie informacji o dostępności*pomocy technicznej przestrzenie nazw. Wysyłanie dostępność zachowuje możliwość wysyłania wiadomości. Aby użyć Wyślij dostępność, aplikacji musi spełniać następujące wymagania:

1.  Wiadomości są odbierane tylko z podstawowego nazw.

2.  Wiadomości wysyłane do danej kolejki lub tematu może otrzymują kolejności.

3.  Jeśli aplikacja używa sesje, może być nadchodzących kolejność wiadomości w sesji. To jest podział od normalnego funkcji sesji. Oznacza to, że aplikacja używa sesje logicznego wiadomości grupy. Stan sesji jest obsługiwana tylko w obszarze podstawowy nazw.

4.  Wiadomości w ramach sesji może być dostarczone kolejności. To jest podział od normalnego funkcji sesji. Oznacza to, że aplikacja używa sesje logicznego wiadomości grupy.

5.  Stan sesji jest obsługiwana tylko w obszarze podstawowy nazw.

6.  Podstawowy kolejki można do trybu online i uruchomić akceptowanie wiadomości, zanim pomocniczej kolejki udostępnia wszystkie wiadomości w kolejce podstawowego.

W poniższych sekcjach omówiono za pośrednictwem interfejsów API, jak są wykonywane za pośrednictwem interfejsów API i zawiera przykładowy kod, który używa funkcji. Zauważ, że są konsekwencje rozliczeń skojarzonego z tą funkcją.

### <a name="the-messagingfactorypairnamespaceasync-api"></a>MessagingFactory.PairNamespaceAsync interfejsu API

Funkcja iloczynów nazw zawiera metodę [PairNamespaceAsync][] klasy [Microsoft.ServiceBus.Messaging.MessagingFactory][] :

```
public Task PairNamespaceAsync(PairedNamespaceOptions options);
```

Po zakończeniu zadania sparowania nazw jest również gotowy do pracy z dla dowolnego [MessageReceiver][], [QueueClient][]lub [TopicClient][] utworzone za pomocą wystąpienia [MessagingFactory][] . [Microsoft.ServiceBus.Messaging.PairedNamespaceOptions][] jest klasą podstawową dla różnych typów sparowania, które są dostępne z obiektem [MessagingFactory][] . Obecnie jedyną klasą pochodnych jest jeden nazwany [SendAvailabilityPairedNamespaceOptions][], który wykonuje wymagań dotyczących dostępności Wyślij. [SendAvailabilityPairedNamespaceOptions][] ma zestaw konstruktorów, które tworzyć od siebie. Przeglądająca konstruktora z parametrami większości, mogą zrozumieć działanie innych konstruktorów.

```
public SendAvailabilityPairedNamespaceOptions(
    NamespaceManager secondaryNamespaceManager,
    MessagingFactory messagingFactory,
    int backlogQueueCount,
    TimeSpan failoverInterval,
    bool enableSyphon)
```

Parametry te mają następujące znaczenie:

-   *secondaryNamespaceManager*: wystąpienia [NamespaceManager][] zainicjowany dla nazw pomocnicza dla metody [PairNamespaceAsync][] służy do ustawiania pomocniczej nazw. Menedżer nazw jest używana, aby uzyskać listę kolejek w obszarze nazw i upewnij się, że istnieje kolejek zaległości wymagane. Jeśli te kolejki nie istnieje, są tworzone. [NamespaceManager][] wymaga możliwość tworzenia tokenu z roszczeń **Zarządzaj** .

-   *messagingFactory*: wystąpienie [MessagingFactory][] dla pomocniczej nazw. Obiekt [MessagingFactory][] jest używany do wysyłania i, jeśli właściwość [EnableSyphon][] jest ustawiona na **wartość PRAWDA**, odbierać wiadomości z kolejek zaległości.

-   *backlogQueueCount*: Liczba kolejek zaległości do utworzenia. Wartość ta musi mieć co najmniej 1. Podczas wysyłania wiadomości do zaległości, jest jedną z tych kolejek losowo wybranych. Jeśli wartość jest ustawiona na 1, następnie tylko jedna kolejka kiedykolwiek można używać. Gdy dzieje i kolejki jeden zaległości generuje błędy, klient jest nie będą mogli spróbuj kolejki różnych zaległości i może się nie powieść wysłać wiadomość. Zaleca się ustawienie wartości niektóre większe wartości i domyślną wartość 10. Można to zmienić, wyższych lub niższych wartości w zależności od ilości przetwarzanych danych aplikacji wysyła dziennie. Każda kolejka zaległości może zawierać maksymalnie 5 GB wiadomości.

-   *failoverInterval*: ilość czasu, w którym można zaakceptować błędy podstawowego nazw przed przełączeniem dowolnego całość do nazw pomocniczym. Praca awaryjna odbywa się na podstawie jednostki jednostki. Podmioty w jednym nazw są często przechowywane w różnych węzłów na Bus usługi. Błąd podczas jednego obiektu nie oznacza awarii w innym. Można ustawić tę wartość do [System.TimeSpan.Zero][] przełączanie awaryjne do pomocniczej natychmiast po awarii do pierwszej, bez zmiennych. Błędy wywołujących czasomierza pracy awaryjnej są wszystkie [MessagingException][] , w którym właściwość [IsTransient][] ma wartość FAŁSZ lub [System.TimeoutException][]. Inne wyjątki, takie jak [UnauthorizedAccessException][] nie spowoduje awaryjnym przeniesieniu, ponieważ wskazują, że klient jest skonfigurowany niepoprawnie. Nie powoduje [ServerBusyException][] pracy awaryjnej ponieważ poprawnego wzorca jest czekać 10 sekund, następnie ponownie wysłać wiadomość.

-   *enableSyphon*: wskazuje, że określony sparowania należy również syphon wiadomości z nazw pomocniczej powrót do podstawowego nazw. Na ogół aplikacje, które wiadomości należy skonfigurować tę wartość **FAŁSZ**; aplikacje, które komunikaty należy skonfigurować tę wartość **PRAWDA**. Przyczyną tego są często dostępne mniejszej liczby odbiorców wiadomości od nadawców wiadomości. W zależności od liczby odbiorców można obsługiwać obowiązków syphon wystąpienie jednej aplikacji. Przy użyciu wielu odbiorców ma znaczenie rozliczeń dla każdej kolejki zaległości.

Aby przy użyciu kodu, Utwórz podstawowego wystąpienia [MessagingFactory][] , pomocniczej wystąpienia [MessagingFactory][] pomocniczej wystąpienia [NamespaceManager][] i w przypadku wystąpienia [SendAvailabilityPairedNamespaceOptions][] . Połączenie może być tak proste jak następujące czynności:

```
SendAvailabilityPairedNamespaceOptions sendAvailabilityOptions = new SendAvailabilityPairedNamespaceOptions(secondaryNamespaceManager, secondary);
primary.PairNamespaceAsync(sendAvailabilityOptions).Wait();
```

Po zakończeniu zadania zwrócony przez metodę [PairNamespaceAsync][] wszystko jest ustawione i gotowe do użycia. Zanim zadanie zostanie zwrócona, mogą nie została ukończona niezbędne do sparowania działają prawidłowo pracy tła. W wyniku nie powinny zacząć wysyłania wiadomości do momentu zwraca zadania. W przypadku wystąpienia błędów, takich jak nieprawidłowe poświadczenia lub nie można utworzyć kolejek zaległości tych wyjątków będzie generowany po wykonaniu zadania. Po zwraca zadania, należy sprawdzić, czy kolejek zostały odnalezione lub utworzone przez sprawdzanie właściwości [BacklogQueueCount][] wystąpienia [SendAvailabilityPairedNamespaceOptions][] . Poprzednim kodzie tej operacji wygląda następująco:

```
if (sendAvailabilityOptions.BacklogQueueCount < 1)
{
    // Handle case where no queues were created.
}
```

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy asynchroniczne wiadomości w Bus usługi, przeczytaj więcej informacji na temat [nazw iloczynów][].

  [ServerBusyException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.serverbusyexception.aspx
  [System.TimeoutException]: https://msdn.microsoft.com/library/system.timeoutexception.aspx
  [MessagingException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
  [Najważniejsze wskazówki dotyczące izolacji aplikacji przed dostawie Bus usługi i awarii]: service-bus-outages-disasters.md
  [Microsoft.ServiceBus.Messaging.MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [MessageReceiver]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx
  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx
  [Microsoft.ServiceBus.Messaging.PairedNamespaceOptions]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.pairednamespaceoptions.aspx
  [MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [SendAvailabilityPairedNamespaceOptions]:https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.aspx
  [NamespaceManager]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx
  [PairNamespaceAsync]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.pairnamespaceasync.aspx
  [EnableSyphon]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.enablesyphon.aspx
  [System.TimeSpan.Zero]: https://msdn.microsoft.com/library/azure/system.timespan.zero.aspx
  [IsTransient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.istransient.aspx
  [UnauthorizedAccessException]: https://msdn.microsoft.com/library/azure/system.unauthorizedaccessexception.aspx
  [BacklogQueueCount]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.backlogqueuecount.aspx
  [ILOCZYNÓW nazw]: service-bus-paired-namespaces.md