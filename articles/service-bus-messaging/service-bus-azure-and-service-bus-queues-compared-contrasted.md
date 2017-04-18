<properties 
    pageTitle="Azure kolejki i Bus usługi kolejek — porównanie i odróżniające | Microsoft Azure"
    description="Analizuje podobieństwa między dwoma typami kolejek oferowanych przez Azure i."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="tysonn" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="tbd"
    ms.date="09/23/2016"
    ms.author="sethm" />

# <a name="azure-queues-and-service-bus-queues---compared-and-contrasted"></a>Azure kolejki i Bus usługi kolejek — porównanie i odróżniające

W tym artykule analizuje podobieństwa między tymi dwoma typami kolejek oferowaną dziś przez Microsoft Azure i: kolejek kolejek Azure i Bus usługi. Za pomocą tych informacji, możesz porównać i kontrastu odpowiednich technologii i mieć możliwość podjęcia decyzji więcej informacji o rozwiązania, które najlepiej odpowiada Twoim potrzebom.

## <a name="introduction"></a>Wprowadzenie

Microsoft Azure obsługuje dwa typy mechanizmy kolejki: **Kolejki Azure** i **Kolejki Bus usługi**.

**Azure kolejek**, które są częścią infrastruktury [Azure miejsca do magazynowania](https://azure.microsoft.com/services/storage/) , funkcja interfejsem oparte na pozostałych Get-położenie-wglądu proste, dostarczając niezawodnych i trwałych wiadomości w ramach i między usługami.

**Kolejki Bus usługi** są częścią szerszego infrastruktury [Azure wiadomości](https://azure.microsoft.com/services/service-bus/) , obsługującego Kolejkowanie oraz publikowania subskrypcji, zdalnych usług sieci Web i desenie integracji. Aby uzyskać więcej informacji o kolejkach Bus usługi, tematy i subskrypcje i przekaźniki zobacz [Omówienie Bus usługi wiadomości](service-bus-messaging-overview.md).

Gdy obie technologie Kolejkowanie istnieje jednocześnie, kolejek Azure wprowadzono najpierw mechanizm magazynowania dedykowane kolejki utworzoną na bieżąco usług Azure magazynu. Kolejki Bus usługi są dostępne u góry pełniejsza infrastruktury "brokered wiadomości" zaprojektowane do integracji aplikacji lub składników aplikacji, obejmujące wiele protokołów komunikacji, umów danych zaufania domen i/lub środowiskach sieci.

W tym artykule porównano technologii dwie kolejki oferowanych przez Azure według omawianie różnice w zachowanie i stosowania funkcji dostępnych na każdej. Artykuł również zawiera wskazówki dotyczące wybierania, które funkcje mogą być własnymi potrzebami opracowywania aplikacji.

## <a name="technology-selection-considerations"></a>Zagadnienia dotyczące zaznaczenia technologii

Zarówno kolejek Azure, jak i usługi Bus kolejki są implementacji wiadomości Kolejkowanie usługi Microsoft Azure dostępne. Każda ma zestaw funkcji różni, co oznacza wybierz jednego lub drugiego lub korzystanie z obu, w zależności od potrzeb danego rozwiązania lub są rozwiązywania problemu biznesowych i technicznych.

Podczas określania Kolejkowanie technologii ogólnej przeznaczenie dla danego rozwiązania, rozwiązanie Twórcy architektury i deweloperzy należy uwzględnić poniższe zalecenia. Aby uzyskać więcej informacji zobacz następną sekcję.

Jako rozwiązanie architektury dewelopera, **należy rozważyć użycie kolejek Azure** po:

- Aplikacja ponad 80 GB wiadomości muszą być przechowywane w kolejce, której wiadomości mają istnienia krótszym niż 7 dni.

- Aplikacja chce śledzenia postępu przetwarzania wiadomości w kolejce. To jest przydatne, jeśli pracownik przetwarzania wiadomości ulega awarii. Kolejne pracownika używać tych informacji chcesz kontynuować miejsce, w którym pracownik poprzednich przerwana.

- Wymaganie dzienniki po stronie serwera wszystkich transakcji wykonywane z kolejek.

Jako rozwiązanie architektury dewelopera, **należy rozważyć użycie usługi Bus kolejek** podczas:

- Rozwiązania muszą mieć możliwość odbierania wiadomości bez konieczności ankieta kolejki. Przy użyciu usługi Bus, można to osiągnąć przy użyciu ankieta długiej operacji za pomocą protokołów opartych na TCP/IP, obsługiwanych przez usługę Bus odbioru.

- Rozwiązanie wymaga kolejki o podanie gwarantowanej pierwszego w pierwszym wychodzący dostarczenia zamówiono (FIFO).

- Chcesz symetrycznej doświadczenia w Azure i w systemie Windows Server (chmury prywatnych). Aby uzyskać więcej informacji zobacz [Usługa Bus dla systemu Windows Server](https://msdn.microsoft.com/library/dn282144.aspx).

- Rozwiązania muszą mieć możliwość obsługuje automatycznego wykrywania duplikatów.

- Ma aplikacji do procesu wiadomości jako równoległe strumienie długim (wiadomości są skojarzone z strumienia przy użyciu właściwości [identyfikatora sesji](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx) w komunikacie). W tym modelu każdy węzeł w aplikacji dużo konkuruje dla strumieni, a nie w wiadomości. Strumień podano do węzła dużo, można sprawdzić stan stan strumienia aplikacji przy użyciu transakcji w węzeł.

- Rozwiązanie wymaga zachowanie transakcji i niepodzielność podczas wysyłania lub odbierania wielu wiadomości z kolejki.

- Time to live (TTL) cechy obciążenie pracą specyficzne dla aplikacji mogą przekraczać 7 dni.

- Aplikacja obsługuje wiadomości, które mogą przekraczać 64 KB, ale ograniczy nie mogą podejście 256 KB.

- Należy zająć się wymagane zapewnia model dostępu oparta na rolach, aby kolejki i inne prawa i uprawnienia do nadawcy i odbiorcy.

- Rozmiar kolejki nie będzie rosnąć większych niż 80 GB.

- Chcesz używać protokołu wiadomości zgodne ze standardami AMQP 1.0. Aby uzyskać więcej informacji o AMQP zobacz [Omówienie AMQP Bus usługi](./service-bus-amqp-overview.md).

- Można traktować ewentualne migracji z komunikacji typu punkt-punkt opartych na kolejkach wiadomości wzorca programu exchange, umożliwiającą integrację dodatkowe odbiorców (subskrybentów), z których każdy będzie otrzymuje niezależnych kopię albo niektóre lub wszystkie wiadomości wysłane do kolejki. Te ostatnie odwołuje się do możliwości publikowania/subskrypcji oryginalnie dostarczony przez usługę Bus.

- Rozwiązanie do obsługi wiadomości muszą mieć możliwość obsługi gwarancji dostawy "Większość-jednocześnie" bez konieczności do tworzenia elementów infrastruktury dodatkowe.

- Chcesz mieć możliwość publikowania i wykorzystywania partie wiadomości.

- Wymaganie pełnej integracji z stos komunikacji Windows Communication Foundation (WCF) dla programu .NET Framework.

## <a name="comparing-azure-queues-and-service-bus-queues"></a>Porównanie kolejek kolejek Azure i Bus usługi

Tabele w poniższych sekcjach udostępnia logiczna grupa funkcje kolejki i umożliwiają porównanie w skrócie, funkcje dostępne zarówno kolejek Azure i kolejek Bus usługi.

## <a name="foundational-capabilities"></a>Funkcje foundational

W tej sekcji porównano niektóre podstawowe możliwości Kolejkowanie dostarczony przez kolejki kolejek Azure i Bus usługi.

|Kryteria porównawcze|Azure kolejki|Usługa Bus kolejki|
|---|---|---|
|Kolejność gwarancji|**Brak** <br/><br>Aby uzyskać więcej informacji zobacz pierwszą notatkę w sekcji "Dodatkowe informacje".</br>|**Tak — pierwszego w pierwszym Out (FIFO)**<br/><br>(za pomocą wiadomości sesji)|
|Gwarancji dostarczenia|**Najmniej jednocześnie**|**Najmniej jednocześnie**<br/><br/>**Większość jednocześnie**|
|Obsługa Atomowej operacji|**Brak**|**Tak**<br/><br/>|
|Odbieranie zachowanie|**Bez blokowania**<br/><br/>(wykonuje bezpośrednio w przypadku nie nowej wiadomości)|**Blokowanie z lub bez przekroczenia limitu czasu**<br/><br/>(ofert długie ankieta lub ["Kometa metoda"](http://go.microsoft.com/fwlink/?LinkId=613759))<br/><br/>**Bez blokowania**<br/><br/>(za pomocą .NET zarządzany interfejs API tylko)|
|Interfejs API wypychanych stylu|**Brak**|**Tak**<br/><br/>[OnMessage](https://msdn.microsoft.com/library/azure/jj908682.aspx) i **OnMessage** sesje interfejsu API .NET.|
|Tryb odbierania|**Wgląd do & dzierżawy**|**Wgląd do & Zablokuj**<br/><br/>**Odbieranie i Usuń**|
|Tryb wyłączności|**Oparte na dzierżawę**|**Oparte na blokady**|
|Czas trwania dzierżawy i blokady|**30 sekund (ustawienie domyślne)**<br/><br/>**7 dni (maksymalną)** (Można odnowić lub Zwolnij dzierżawę wiadomości przy użyciu [UpdateMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage.aspx) interfejsu API.)|**60 sekund (ustawienie domyślne)**<br/><br/>Możesz odnowić blokada wiadomości przy użyciu [RenewLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.renewlock.aspx) interfejsu API.|
|Dzierżawy i Zablokuj precyzji|**Poziom wiadomości**<br/><br/>(każda wiadomość może mieć wartość inny limit, które można zaktualizować stosownie do potrzeb podczas przetwarzania wiadomości przy użyciu [UpdateMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage.aspx) interfejsu API)|**Poziom kolejki**<br/><br/>(Każda kolejka ma dokładność blokady zastosowane do wszystkich jej wiadomości, ale można odnowić blokady przy użyciu [RenewLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.renewlock.aspx) interfejsu API).|
|Przetwarzany wsadowo odbieranie|**Tak**<br/><br/>(jawne określenie wiadomości podczas pobierania wiadomości maksymalnie 32 wiadomości)|**Tak**<br/><br/>(niejawnie włączając właściwość wstępne pobrania lub jawnie za pomocą transakcji)|
|Wyślij wsadowej|**Brak**|**Tak**<br/><br/>(za pomocą transakcje lub tworzeniu partii po stronie klienta)|

### <a name="additional-information"></a>Dodatkowe informacje

- Wiadomości w kolejkach Azure są zwykle pierwszego w pierwszym wychodzący, ale czasami może być uszkodzona; na przykład wiadomość widoczność limit czasu wygaśnięcia (na przykład w wyniku aplikacją kliencką awarii podczas przetwarzania). Po wygaśnięciu limitu czasu widoczność wiadomości jest widoczna ponownie w kolejce dla innego pracownika do usuwania z kolejki go. W tym momencie nowo widoczne wiadomości mogą być umieszczane w kolejce (Aby zostać usunięty z kolejki ponownie) po wiadomość, którą pierwotnie został umieszczony w kolejce po nim.

- Jeśli już używasz blob miejsca do magazynowania Azure lub tabel i możesz rozpocząć korzystanie z kolejek, są zagwarantować dostępność 99,9%. Jeśli używasz obiektów blob lub tabele z kolejek usługi Bus masz dolnym dostępności.

- Gwarantowana deseniu FIFO w kolejkach Bus usługi wymaga korzystania z sesji komunikacji. W przypadku, gdy aplikacja ulega awarii podczas przetwarzania wiadomości odebranych w trybie **wglądu & Zablokuj** , przy następnym słuchawka kolejce akceptuje sesji wiadomości będzie zaczynać się nie powiodło się wiadomości po wygaśnięciu okresu time to live (TTL).

- Azure kolejek są przeznaczone do obsługuje standard Kolejkowanie scenariuszy, takich jak odsprzęgania składniki aplikacji można zwiększyć skalowalność i uszkodzenia dla błędów, załaduj bilansowania i tworzenie przepływów pracy procesu.

- Usługa Bus kolejki obsługują gwarancji dostarczenia *Najmniej-jednocześnie* . Ponadto *Większość-jednocześnie* semantyczny są obsługiwane przy użyciu stanu sesji w celu przechowywania stan aplikacji i przy użyciu transakcji atomically odbierać wiadomości i zaktualizować stan sesji.

- Azure kolejek udostępnianie jednolity i spójny model programowania w kolejkach, tabel i obiektów blob — dla deweloperów i zespołów operacji.

- Usługa Bus kolejek zapewniają obsługę transakcji lokalnych w kontekście pojedynczej kolejki.

- **Odbieranie i Usuń** trybu obsługiwanych przez usługę Bus umożliwia zmniejszenie wiadomości liczba operacji (i skojarzone koszt) zamian assurance obniżona dostarczenia.

- Azure kolejki umożliwiają dzierżawy rozszerzanie dzierżawy dla wiadomości. Dzięki temu pracowników, którzy mają Obsługa krótki dzierżawy w odniesieniu do wiadomości. W związku z tym przypadku awarii pracownikiem wiadomości mogą być szybko przetwarzane ponownie przez innego pracownika. Ponadto pracownika można rozszerzyć dzierżawy w wiadomości, gdy niezbędne do jego przetworzenia dłużej niż bieżący czas trwania dzierżawy.

- Azure kolejek oferują widoczność przekroczenie limitu czasu można ustawić na enqueueing lub usuwania wiadomości. Ponadto można zaktualizować wiadomości z dzierżawy różnych wartości w czasie wykonywania i aktualizowanie różnych wartości w wiadomości w tej samej kolejki. Limity czasu blokady Bus usługi są definiowane w kolejce metadanych; Jednak możesz odnowić blokady przez wywołanie metody [RenewLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.renewlock.aspx) .

- Maksymalny limit czasu operacji blokowania odbioru w kolejkach Bus usługi jest 24 dni. Limity czasu oparte na pozostałych jednak maksymalna wartość 55 sekundach.

- Po stronie klienta tworzeniu partii dostarczony przez usługę Bus umożliwia klientowi kolejki partii wielu wiadomości do operacji wysyłania pojedynczej. Tworzeniu partii jest dostępna tylko dla operacji wysyłania asynchroniczne.

- Funkcje, takie jak ceiling 200 TB Azure kolejkach (więcej po wirtualizacji konta) i nieograniczoną kolejek był idealny platformy dla władz akredytacji bezpieczeństwa dostawców.

- Azure kolejki udostępnia elastyczne i performant delegowanymi mechanizmu kontroli dostępu.

## <a name="advanced-capabilities"></a>Zaawansowane funkcje

W tej sekcji porównano zaawansowanych funkcji dostarczony przez kolejki kolejek Azure i Bus usługi.

|Kryteria porównawcze|Azure kolejki|Usługa Bus kolejki|
|---|---|---|
|Zaplanowane dostarczenia|**Tak**|**Tak**|
|Automatyczne braku litery|**Brak**|**Tak**|
|Zwiększenie wartości czasu wygaśnięcia kolejki|**Tak**<br/><br/>(za pomocą aktualizacji w miejscu widoczność Przekroczono limit czasu)|**Tak**<br/><br/>(udostępniana za pośrednictwem funkcji interfejsu API dedykowane)|
|Poison Obsługa wiadomości|**Tak**|**Tak**|
|Aktualizacja w miejscu|**Tak**|**Tak**|
|Dziennik transakcji po stronie serwera|**Tak**|**Brak**|
|Metryki magazynowania|**Tak**<br/><br/>**Minuta metryki**: znajdują się w czasie rzeczywistym metryki dla interfejsu API dostępność, TPS, połączenie liczników, liczniki błędu i nie tylko wszystkie w czasie rzeczywistym (agregacją minutę i zgłoszone w ciągu kilku minut od co tak się stało z produkcji. Aby uzyskać więcej informacji zobacz [Temat metryki analizy magazynowania](https://msdn.microsoft.com/library/azure/hh343258.aspx).|**Tak**<br/><br/>(zbiorcze zapytania, dzwoniąc [GetQueues](https://msdn.microsoft.com/library/azure/hh293128.aspx))|
|Zarządzania stanem|**Brak**|**Tak**<br/><br/>[Microsoft.ServiceBus.Messaging.EntityStatus.Active](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx), [Microsoft.ServiceBus.Messaging.EntityStatus.Disabled](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx) [Microsoft.ServiceBus.Messaging.EntityStatus.SendDisabled](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx), [Microsoft.ServiceBus.Messaging.EntityStatus.ReceiveDisabled](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx)|
|Automatyczne przekazywanie wiadomości|**Brak**|**Tak**|
|Czyszczenie kolejki, funkcja|**Tak**|**Brak**|
|Grupy wiadomości|**Brak**|**Tak**<br/><br/>(za pomocą wiadomości sesji)|
|Stan aplikacji dla każdej grupy wiadomości|**Brak**|**Tak**|
|Wykrywanie duplikatów|**Brak**|**Tak**<br/><br/>(można konfigurować na stronie nadawcy)|
|Integracja programu WCF|**Brak**|**Tak**<br/><br/>(oferuje w nowym polu WCF powiązań)|
|Integracja WF|**Niestandardowe**<br/><br/>(wymaga tworzenia niestandardowym działaniem WF)|**Natywne**<br/><br/>(oferuje działania WF w nowym polu)|
|Przeglądanie wiadomości grupy|**Brak**|**Tak**|
|Pobieranie wiadomości sesje według identyfikatorów|**Brak**|**Tak**|

### <a name="additional-information"></a>Dodatkowe informacje

- Obie technologie Kolejkowanie włączyć wiadomości zaplanowany dla dostarczania w późniejszym czasie.

- Automatyczne przekazywanie kolejki umożliwia tysiące kolejki, aby automatycznie Prześlij dalej wiadomości do pojedynczej kolejki, z której aplikacja korzysta z wiadomości. Za pomocą tego mechanizmu osiągnięcia bezpieczeństwa, sterowania przepływem i wyodrębnienia miejsca do magazynowania między każdego wydawcy wiadomości.

- Azure kolejek zapewniają obsługę aktualizowanie treści wiadomości. Można użyć tej funkcji utrwalanie informacje o stanie i aktualizacje postępu przyrostowe do wiadomości, dzięki czemu mogą być przetwarzane z ostatniego znane punktu kontrolnego, zamiast od podstaw. Z kolejki Bus usługi możesz włączyć obsługę tego samego scenariusza za pomocą sesji wiadomości. Sesje umożliwiają zapisywanie i pobieranie stan przetwarzania aplikacji (za pomocą [SetState](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.setstate.aspx) i [GetState](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.getstate.aspx)).

- [Braku litery](service-bus-dead-letter-queues.md), która jest obsługiwana tylko przez usługę Bus kolejki, może być przydatne w przypadku izolowanie wiadomości, które nie zostanie przetworzony pomyślnie, stosując odbierania lub wiadomości nie osiągnie swoje miejsce docelowe z powodu wygasłej time to live (TTL) właściwości. Wartość TTL Określa, jak długo wiadomość pozostaje w kolejce. Przy użyciu usługi Bus wiadomość zostanie przeniesiona do specjalnych kolejki o nazwie $DeadLetterQueue, po wygaśnięciu okresu TTL.

- Aby znaleźć "" wiadomości w kolejkach Azure podczas usuwania wiadomości aplikację sprawdza, czy właściwość **[DequeueCount](https://msdn.microsoft.com/library/azure/dd179474.aspx)** wiadomości. W przypadku **DequeueCount** powyżej określonej wartości progowej, aplikacja przenosi wiadomość do zdefiniowanych przez aplikację "kolejki utraconych wiadomości".

- Azure kolejek pozwalają uzyskać szczegółowy dziennik wszystkich transakcji wykonywane kolejki jako metryki oraz jak zagregowane. Obu tych opcji są przydatne do debugowania i opis, jak aplikacja używa kolejek Azure. Są one również przydatne w przypadku aplikacji Dostosowywanie wydajności i zmniejszenie kosztów użycia kolejki.

- Pojęcie "sesje komunikat" obsługiwane przez usługę Bus umożliwia wiadomości, które należą do określonej grupy logiczne mają być skojarzone z danym odbiorca, co z kolei spowoduje utworzenie koligację przypominających sesji między wiadomości i ich odpowiednich odbiorców. Możesz włączyć ten zaawansowanych funkcji w Bus usługi przez ustawienie właściwości [identyfikatora sesji](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx) w wiadomości. Odbiorców można odsłuchać z identyfikatorem określonej sesji i odbierać wiadomości, współużytkujących identyfikator określonej sesji.

- Funkcji wykrywania duplikatów obsługiwane przez usługę Bus kolejki automatycznie usuwa zduplikowane wiadomości wysłane do kolejki lub tematu, na podstawie wartości tej właściwości [komunikatu](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) .

## <a name="capacity-and-quotas"></a>Wydajność i przydziałów

W tej sekcji porównano kolejki Azure i Bus usługi kolejek z perspektywy [Wydajność i przydziałami](service-bus-quotas.md) które mogą być stosowane.

|Kryteria porównawcze|Azure kolejki|Usługa Bus kolejki|
|---|---|---|
|Maksymalny rozmiar kolejki|**200 TB**<br/><br/>(tylko jednego konta pojemności)|**1 GB do 80 GB**<br/><br/>(zdefiniowane po utworzeniu kolejki i [Włączanie podziału](service-bus-partitioning.md) — zobacz sekcję "Dodatkowe informacje")|
|Maksymalny rozmiar wiadomości|**64 KB**<br/><br/>(48 KB, gdy formacie **Base64** )<br/><br/>Azure obsługuje duże wiadomości, łącząc kolejek i obiektów blob — w chwili możesz kolejkowania wywołań zwrotnych maksymalnie 200GB dla pojedynczego elementu.|**256 KB** lub **1 MB**<br/><br/>(w tym zarówno nagłówka i treści nagłówka maksymalny rozmiar: 64 KB).<br/><br/>W zależności od [poziomu usług](service-bus-premium-messaging.md).|
|Maksymalny czas wygaśnięcia wiadomości|**7 dni**|**`TimeSpan.Max`**|
|Maksymalna liczba kolejki|**Nieograniczoną**|**10 000**<br/><br/>(dla usługi nazw, można zwiększyć)|
|Maksymalna liczba równoczesne klientów|**Nieograniczoną**|**Nieograniczoną**<br/><br/>(100 połączenia nawiązanego limit dotyczy tylko komunikacji opartej na protokołu TCP)|

### <a name="additional-information"></a>Dodatkowe informacje

- Usługa Bus wymusza ograniczenia rozmiaru kolejki. Maksymalny rozmiar kolejki określono po utworzeniu kolejki i nie może mieć wartość z zakresu od 1 do 80 GB. Po osiągnięciu wartość rozmiaru kolejki przy tworzeniu kolejki dodatkowe wiadomości przychodzące będą odrzucane i wyjątków będzie odbierana przez kod wywołujący. Aby uzyskać więcej informacji dotyczących przydziałów w Bus usługi zobacz [Usługa Bus przydziałów](service-bus-quotas.md).

- Możesz utworzyć kolejek usługi Bus powstawanie 1, 2, 3, 4 lub 5 GB (wartością domyślną jest 1 GB). Z podziału włączone (jest to ustawienie domyślne), usługi Bus tworzy 16 partycje dla każdego GB określone. Jako takie, jeśli tworzysz kolejkę, którą jest 5 GB rozmiar z 16 partycje maksymalny rozmiar kolejki staje się (5 * 16) = 80 GB. Maksymalny rozmiar kolejki podzielone na partycje lub tematu widać, sprawdzając wejścia w [Azure portal][].

- Z kolejki Azure Jeśli zawartość wiadomości nie jest bezpiecznych XML, następnie należy **Base64** zakodowany. Jeśli możesz **Base64**-kodowanie wiadomości, ładunek użytkownika może być maksymalnie 48 KB, zamiast 64 KB.

- Z kolejek usługi Bus każdej wiadomości przechowywanych w kolejce składa się z dwóch części: nagłówka i treści. Całkowity rozmiar wiadomości nie może przekraczać maksymalny rozmiar wiadomości obsługiwane przez warstwa usług.

- Gdy klienci komunikować się z kolejek usługi Bus przez protokołu TCP, maksymalną liczbę równoległych połączeń do pojedynczej kolejki Bus usługi jest ograniczone do 100. Liczba ta jest udostępniane między nadawcy i odbiorcy. Po osiągnięciu tego przydziału kolejne żądania związane z dodatkowych połączeń będą odrzucane i wyjątków będzie odbierana przez kod wywołujący. Ten limit nie jest nałożone na klientów łączących się z kolejek przy użyciu interfejsu API opartego na pozostałych.

- Jeśli potrzebujesz więcej niż 10 000 kolejek w jednym nazw Bus usługi, możesz skontaktuj się z zespołem pomocy technicznej Azure i poproś wzrost. Aby przeskalować ponad 10 000 kolejki z usługi Bus, można także tworzyć dodatkowe przestrzenie nazw za pomocą [Azure portal][].

## <a name="management-and-operations"></a>Zarządzanie i operacje

W tej sekcji porównano funkcje zarządzania oferowane przez kolejki kolejek Azure i Bus usługi.

|Kryteria porównawcze|Azure kolejki|Usługa Bus kolejki|
|---|---|---|
|Protokół zarządzania|**Umieść protokołu HTTP/HTTPS**|**Umieść za pomocą protokołu HTTPS**|
|Środowisko uruchomieniowe Protocol (protokół)|**Umieść protokołu HTTP/HTTPS**|**Umieść za pomocą protokołu HTTPS**<br/><br/>**AMQP 1.0 standardowe (TCP z TLS)**|
|Zarządzany interfejs API programu .NET|**Tak**<br/><br/>(.NET zarządzany interfejs API klienta miejsca do magazynowania)|**Tak**<br/><br/>(.NET zarządzane brokered API wiadomości)|
|Macierzystym C++|**Tak**|**Brak**|
|Interfejs API języka Java|**Tak**|**Tak**|
|INTERFEJS API PHP|**Tak**|**Tak**|
|Node.js interfejsu API|**Tak**|**Tak**|
|Obsługa dowolnego metadanych|**Tak**|**Brak**|
|Kolejka regułami nazewnictwa|**Maksymalnie 63 znaków**<br/><br/>(Liter w nazwie kolejki musi być mała).|**Maksymalnie 260 znaków.**<br/><br/>(Nazwy i ścieżki kolejki są bez uwzględniania wielkości liter).|
|Uzyskiwanie funkcja długość kolejki|**Tak**<br/><br/>(Przybliżona wartość, jeśli wiadomości wygasają poza wartość TTL bez usuwany.)|**Tak**<br/><br/>(Wartość dokładnie tak samo, w chwili.)|
|Funkcja wglądu|**Tak**|**Tak**|

### <a name="additional-information"></a>Dodatkowe informacje

- Azure kolejek zapewniają obsługę dowolnego atrybuty, które mogą być stosowane do opisu kolejki, w formie pary nazwa wartość.

- Obie technologie kolejki oferuje możliwość wgląd wiadomości bez konieczności zablokować, które mogą być przydatne podczas wykonywania narzędzi przeglądarki/explorer kolejki.

- .NET Bus usługi brokered wiadomości interfejsy API dźwigni pełny dupleks połączeń zwiększa wydajność w porównaniu z pozostałych za pomocą protokołu HTTP i obsługują protokół standardowy AMQP 1.0.

- Nazwy kolejek Azure może być długi 3-63 znaków, może zawierać małe litery, cyfry i łączniki. Aby uzyskać więcej informacji zobacz [kolejki nazewnictwa i metadanych](https://msdn.microsoft.com/library/azure/dd179349.aspx).

- Nazwy kolejek Bus usługi można maksymalnie 260 znaków i unikatowe regułami nazewnictwa. Nazwy kolejek Bus usługi może zawierać liter, cyfr, okresów, łączniki i podkreślenia.

## <a name="authentication-and-authorization"></a>Uwierzytelnianie i autoryzacja

W tej sekcji opisano funkcje uwierzytelniania i autoryzacji obsługiwane przez kolejki kolejek Azure i Bus usługi.

|Kryteria porównawcze|Azure kolejki|Usługa Bus kolejki|
|---|---|---|
|Uwierzytelnianie|**Klucz symetrycznej**|**Klucz symetrycznej**|
|Model zabezpieczeń|Delegowane dostęp za pośrednictwem tokeny skojarzeń zabezpieczeń.|SKOJARZENIA ZABEZPIECZEŃ|
|Federacja dostawcy tożsamości|**Brak**|**Tak**|

### <a name="additional-information"></a>Dodatkowe informacje

- Każdego żądania albo technologii Kolejkowanie muszą zostać uwierzytelnione. Kolejki publiczne pomocą dostępu anonimowego nie są obsługiwane. Przy użyciu [skojarzenia zabezpieczeń](service-bus-sas-overview.md), czy adres w tym scenariuszu przez opublikowanie tylko do zapisu skojarzeń zabezpieczeń, skojarzenia zabezpieczeń tylko do odczytu lub nawet skojarzeń zabezpieczeń pełnego dostępu.

- Schemat uwierzytelniania dostarczony przez kolejek Azure obejmuje użyj klawisza symetrycznej, który jest oparty na procesie mieszania wiadomości uwierzytelniania kod (HMAC), obliczane przy użyciu algorytmu SHA-256 i kodowane jako ciąg **Base64** . Aby uzyskać więcej informacji na temat odpowiednich protokołu zobacz [Uwierzytelnianie dla usługi Azure miejsca do magazynowania](https://msdn.microsoft.com/library/azure/dd179428.aspx). Usługa Bus kolejki obsługują modelu podobne za pomocą klawiszy symetrycznej. Aby uzyskać więcej informacji zobacz [Udostępnione Uwierzytelnianie podpisu programu Access z Bus usługi](service-bus-shared-access-signature-authentication.md).

## <a name="cost"></a>Koszt

W tej sekcji porównano kolejek kolejek Azure i Bus usługi z punktu widzenia kosztów.

|Kryteria porównawcze|Azure kolejki|Usługa Bus kolejki|
|---|---|---|
|Koszt transakcji w kolejce|**$0.0036**<br/><br/>(na 100 000 transakcji)|**Podstawowe warstwa**: **$0,05**<br/><br/>(na milionów operacji)|
|Operacje fakturowanego|**Wszystkie**|**Wyślij/Odbierz tylko**<br/><br/>(bezpłatne dla innych operacji)|
|Bezczynne transakcje|**Fakturowanego**<br/><br/>(Kwerendy pustej kolejki jest liczony jako transakcja fakturowanego.)|**Rozliczaniu**<br/><br/>(Odbierz przed pustej kolejki jest traktowany jako wiadomości fakturowanego.)|
|Koszt magazynowania|**$0.07**<br/><br/>(za GB/miesiąc)|**0,00 zł**|
|Koszty przeniesienia danych wychodzących|**$0.12 - $0.19**<br/><br/>(W zależności od geograficznych.)|**$0.12 - $0.19**<br/><br/>(W zależności od geograficznych.)|

### <a name="additional-information"></a>Dodatkowe informacje

- Przesyłanie danych są naliczane na podstawie sumy ilości danych, pozostawiając Azure centrach danych za pośrednictwem Internetu w danym okresie rozliczeniowym.

- Przeniesienia danych między usług Azure znajdujących się w tym samym regionie nie są objęte opłat.

- Z tego artykułu przesyłania danych przychodzących są naliczone opłaty.

- Mając pomocy technicznej dla długich ankieta, korzystanie z kolejek Bus usługi może być kosztów skuteczne w sytuacjach, gdy jest wymagane małym opóźnieniem dostarczenia.

>[AZURE.NOTE] Koszty mogą ulec zmianie. Ta tabela odzwierciedla bieżące ceny i nie zawiera żadnych promocyjnych ofert, które obecnie mogą być dostępne. Aby uzyskać aktualne informacje na temat Azure ceny zobacz stronę [Azure ceny](https://azure.microsoft.com/pricing/) . Aby uzyskać więcej informacji o cenach Bus usługi zobacz [ceny Bus usługi](https://azure.microsoft.com/pricing/details/service-bus/).

## <a name="conclusion"></a>Wnioski

Poprzez zdobywanie szczegółowego opis dwóch technologii, będzie można podjęcie decyzji więcej informacji o technologii kolejki i kiedy. Decyzji o tym, kiedy używać kolejek Azure lub usługi Bus kolejek wyraźnie zależy od kilku czynników. Następujących czynników może być intensywnie zależy od indywidualnych potrzeb i jego Architektura aplikacji. Jeśli aplikacja już używa możliwości core Microsoft Azure, możesz wybrać kolejki Azure, zwłaszcza jeśli wymagają podstawowe komunikacji i wiadomości między usługami lub kolejek potrzebna, które może być większy niż 80 GB w polu rozmiar.

Ponieważ kolejek Bus usługi zapewniają wiele zaawansowanych funkcji, takich jak sesje, transakcje, duplikowanie wykrywania automatycznego litery wiadomości i trwałe publikowania/subskrypcji funkcje mogą być preferowanym rozwiązaniem, jeśli tworzysz aplikację hybrydowego lub jeśli aplikacji wymaga tych funkcji.

## <a name="next-steps"></a>Następne kroki

Więcej wskazówek i informacji o używaniu kolejek Azure lub usługi Bus kolejek zawierają następujące artykuły.

- [Jak używać kolejek Bus usług](service-bus-dotnet-get-started-with-queues.md)
- [Jak korzystać z usługi Magazyn kolejki](../storage/storage-dotnet-how-to-use-queues.md)
- [Najważniejsze wskazówki dotyczące ulepszenia dotyczące wydajności przy użyciu usługi Bus brokered wiadomości](service-bus-performance-improvements.md)
- [Wprowadzenie do kolejek i tematy w Bus usługi Azure](http://www.code-magazine.com/article.aspx?quickid=1112041)
- [Przewodnik projektanta Bus usługi](http://www.cloudcasts.net/devguide/Default.aspx?id=11030)
- [Przy użyciu usługi Kolejkowanie platformy Azure](http://www.developerfusion.com/article/120197/using-the-queuing-service-in-windows-azure/)
- [Opis rozliczeń Azure magazynowania — przepustowości, transakcje i wydajność](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx)

[Azure portal]: https://portal.azure.com
 
