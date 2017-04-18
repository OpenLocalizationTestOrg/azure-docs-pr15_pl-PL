<properties
   pageTitle="Analiza tryb błędu | Microsoft Azure"
   description="Wytyczne dotyczące analizowania tryb błąd dla chmury rozwiązań opartych na Azure."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="mwasson"/>

# <a name="azure-resiliency-guidance-failure-mode-analysis"></a>Wskazówki Azure elastyczność: analiza tryb błędu

Analiza tryb błędu (FMA) jest procesem tworzenia elastyczność do systemu, wskazując punkty możliwe niepowodzenie w systemie. FMA powinny być częścią fazy architektury i projektowania, tak aby awarii można tworzyć w systemie od początku.

Poniżej przedstawiono ogólne proces prowadzenia FMA: 

1. Identyfikowanie wszystkie składniki w systemie. Dołączanie zależności zewnętrznych, takich jak jako dostawcy tożsamości, usług innych firm i tak dalej.   

2. Dla każdego składnika zidentyfikować potencjalne błędy, które mogą wystąpić. Pojedynczy składnik może mieć więcej niż jeden trybu awaryjnego. Na przykład należy rozważyć błędy odczytu i oddzielnie, zakończone niepowodzeniem, ponieważ czynniki wpływ i możliwych będą inne.

3. Ocenianie każdego trybu awaryjnego zgodnie z jej ogólną ryzyka. Należy rozważyć następujące kwestie:  

    - Co to jest prawdopodobieństwo wystąpienia awarii. Jest to stosunkowo typowych? Extrememly rzadkich? Nie ma potrzeby dokładnie liczbami dodatnimi. celem jest pomoc klasyfikowanie priorytet.

    - Jaki jest wpływ na pasku aplikacji, pod względem dostępności, utratą danych koszcie i zakłócenia firm? 

4. Dla każdego trybu awaryjnego Określanie sposobu aplikacji będą odpowiadać i odzyskać. Należy rozważyć kompromisów w złożoność kosztów i aplikacji.   

Jako punktu startowego procesu FMA ten artykuł zawiera wykaz potencjalnych Tryby awaryjne i ich czynniki. Wykaz jest zorganizowane według technologii lub usługa Azure oraz kategorii Ogólne dla projektu na poziomie aplikacji. Wykaz nie jest pełna, ale obejmuje wiele podstawową Azure usług. 


## <a name="app-service"></a>Aplikacji usługi

### <a name="app-service-app-shuts-down"></a>Zamknięcie aplikacji usługi aplikacji.

**Wykrywanie**. Możliwe przyczyny:

- Oczekiwany zamknięcia
    
    - Operator zamknięcie aplikacji; na przykład za pomocą portalu Azure.

    - Aplikacja została zwolnić powodu bezczynne. (Tylko wtedy, gdy `Always On` ustawienie jest wyłączone.)

- Nieoczekiwane zamknięcie

    - Powoduje awarię aplikacji.

    - Wystąpienie maszyn wirtualnych usługi aplikacji staje się niedostępna.


Rejestrowanie Application_End będzie przechwytywać zamknięcia domeny aplikacji (ulegać awarii wygładzone proces) i jest jedynym sposobem efektywnej zamknięcia domeny aplikacji.

**Odzyskiwanie**

- Jeśli zamknięcie oczekiwano, należy użyć zdarzenia zamknięcia aplikacji opcję łagodnego zamknięcia. Na przykład w programie ASP.NET, należy użyć `Application_End` metody.

- Jeśli aplikacja została usunięta z pamięci, gdy bezczynne, jest automatycznie ponownie przy następnym żądaniu. Jednak będzie powodowało kosztów "zimnej start". 

- Aby uniemożliwić aplikacji jest dozwolony podczas bezczynności, Włącz `Always On` Ustawianie w aplikacji sieci web. Zobacz [Konfigurowanie aplikacji sieci web w usłudze Azure aplikacji][app-service-configure].

- Aby zapobiec operatora zamykania aplikacji, ustaw Blokada zasobu z `ReadOnly` poziom. Zobacz [zasoby blokady przy użyciu Menedżera zasobów Azure][rm-locks].

- Jeśli powoduje awarię aplikacji lub maszyn wirtualnych usługi aplikacji staje się niedostępna, aplikacji usługi powoduje automatyczne ponowne uruchomienie aplikacji. 


**Narzędzia diagnostyczne**. Dzienniki aplikacji i dzienniki serwera sieci web. Zobacz [Włączanie diagnostyki rejestrowania aplikacji sieci web w usłudze Azure aplikacji][app-service-logging].


### <a name="a-particular-user-repeatedly-makes-bad-requests-or-overloads-the-system"></a>Dany użytkownik wielokrotnie sprawia, że żądania nieprawidłowe lub obciążone zasoby systemowe. 

**Wykrywanie**. Uwierzytelniania użytkowników i Uwzględnij identyfikator użytkownika w aplikacji.

**Odzyskiwanie**

- Za pomocą [Usługi zarządzania interfejsu API Azure] [ api-management] na żądania przepustowości przez użytkownika. Zobacz [Zaawansowane żądanie ograniczania za pomocą usługi Zarządzanie interfejsu API platformy Azure][api-management-throttling]

- Blokuj użytkownika.

**Narzędzia diagnostyczne**. Zaloguj się wszystkie żądania uwierzytelniania.


### <a name="a-bad-update-was-deployed"></a>Nieprawidłowe aktualizacji został wdrożony.

**Wykrywanie**. Monitorowanie kondycji aplikacji przez Azure Portal (zobacz [Azure monitorowanie wydajności aplikacji sieci web][app-insights-web-apps]) lub zaimplementowania [deseniem monitorowania kondycji punktu końcowego][health-endpoint-monitoring-pattern].

**Odzyskiwanie**. Używanie wielu [gniazda wdrożenia] [ app-service-slots] i przywracać wdrożenia ostatniej znanej dobrej. Aby uzyskać więcej informacji, zobacz [podstawowe web architektura odwołanie][ra-web-apps-basic].


## <a name="azure-active-directory"></a>Azure Active Directory

### <a name="openid-connect-oidc-authentication-fails"></a>Uwierzytelnianie OpenID nawiązywanie połączenia (OIDC) nie powiedzie się.

**Wykrywanie**. Ewentualne sposoby uszkodzenia obejmują:

1. Azure AD nie jest dostępna lub nie można połączyć się ze względu na problem z siecią. Przekierowanie do punktu końcowego uwierzytelniania zakończy się niepowodzeniem, a pośredniczącym OIDC zgłasza wyjątek.
2. Azure AD dzierżawy nie istnieje. Przekierowanie do punktu końcowego uwierzytelniania zwraca kod błędu protokołu HTTP, a pośredniczącym OIDC zgłasza wyjątek.
3. Nie można uwierzytelnić użytkownika. Nie strategii wykrywania jest konieczne; Azure AD obsługuje błędy logowania.

**Odzyskiwanie**

1. Efektywnej nieobsługiwanego wyjątki od pośredniczącym.
2. Uchwyt `AuthenticationFailed` zdarzenia.
3. Przekierowywanie użytkownika do strony o błędzie.
4. Ponowne próby użytkownika.


## <a name="azure-search"></a>Azure wyszukiwania

### <a name="writing-data-to-azure-search-fails"></a>Zapisywanie danych w celu wyszukiwania Azure kończy się niepowodzeniem.

**Wykrywanie**. Efektywnej `Microsoft.Rest.Azure.CloudException` błędy.

**Odzyskiwanie**

[Wyszukiwanie .NET SDK] [ search-sdk] automatycznie ponawia próby po przejściowych błędy. Wyjątki generowane przez klienta SDK powinny być traktowane jako błędne bez zmiennych.

Używa domyślnych zasad ponów próbę wykładniczego wstecz wył. Aby użyć zasady różnych ponów próbę, połączenie `SetRetryPolicy` na `SearchIndexClient` lub `SearchServiceClient` zajęć. Aby uzyskać więcej informacji, zobacz [Automatyczne prób][auto-rest-client-retry].

**Narzędzia diagnostyczne**. Za pomocą [analizy ruchu wyszukiwania][search-analytics].


### <a name="reading-data-from-azure-search-fails"></a>Odczytywanie danych z wyszukiwania Azure kończy się niepowodzeniem.

**Wykrywanie**. Efektywnej `Microsoft.Rest.Azure.CloudException` błędy.

**Odzyskiwanie**

[Wyszukiwanie .NET SDK] [ search-sdk] automatycznie ponawia próby po przejściowych błędy. Wyjątki generowane przez klienta SDK powinny być traktowane jako błędne innych niż zmiennych.

Używa domyślnych zasad ponów próbę wykładniczego wstecz wył. Aby użyć zasady różnych ponów próbę, połączenie `SetRetryPolicy` na `SearchIndexClient` lub `SearchServiceClient` zajęć. Aby uzyskać więcej informacji, zobacz [Automatyczne prób][auto-rest-client-retry].

**Narzędzia diagnostyczne**. Za pomocą [analizy ruchu wyszukiwania][search-analytics].



## <a name="cassandra"></a>Cassandra


### <a name="reading-or-writing-to-a-node-fails"></a>Odczytu lub zapisu do węzła zakończy się niepowodzeniem.

**Wykrywanie**. Efektywnej wyjątek. W przypadku klientów .NET, zazwyczaj są to `System.Web.HttpException`. Inne klient może mieć inne typy wyjątek.  Aby uzyskać więcej informacji zobacz [obsługi błędów Cassandra gotowe w prawo](http://www.datastax.com/dev/blog/cassandra-error-handling-done-right).

**Odzyskiwanie**

- Każdego [klienta Cassandra](https://wiki.apache.org/cassandra/ClientOptions) ma własne zasady ponów próbę i funkcje. Aby uzyskać więcej informacji, zobacz [obsługi błędów Cassandra gotowe bezpośrednio][cassandra-error-handling].
- Wdrożeniu pamiętać o stojaków za pomocą węzłów danych rozkładany domen błędów.
- Rozmieszczanie do wielu regionów za pomocą spójności kworum lokalnego. Jeśli wystąpi błąd nie zmiennych, nie przez innego regionu.

**Narzędzia diagnostyczne**. Dzienniki aplikacji


## <a name="cloud-service"></a>Usługa w chmurze

### <a name="web-or-worker-roles-are-unexpectedlybeing-shut-down"></a>Role w sieci Web lub pracownika są nieoczekiwanie zamykana.

**Wykrywanie**. [RoleEnvironment.Stopping] [ RoleEnvironment.Stopping] zdarzenia.

**Odzyskiwanie**. Zastępowanie [RoleEntryPoint.OnStop] [ RoleEntryPoint.OnStop] metody bezpiecznie oczyszczania. Aby uzyskać więcej informacji, zobacz [Sposób prawo do obsługi zdarzeń OnStop Azure] [ onstop-events] (blog).


## <a name="documentdb"></a>DocumentDB

### <a name="reading-data-from-documentdb-fails"></a>Odczyt danych z DocumentDB zakończy się niepowodzeniem.

**Wykrywanie**. Efektywnej `System.Net.Http.HttpRequestException` lub `Microsoft.Azure.Documents.DocumentClientException`. 

**Odzyskiwanie**

- Zestaw SDK prób automatycznie niepomyślne. Aby ustawić liczbę prób i maksymalny czas oczekiwania, skonfigurować `ConnectionPolicy.RetryOptions`. Wyjątki zgłasza klienta są poza zasad ponów próbę lub nie są przejściowych błędy. 

- Jeśli DocumentDB ogranicza klienta, zwracany jest błąd HTTP 429. Ewidencjonowanie kodu stanu `DocumentClientException`. Jeśli jest wyświetlany błąd 429 spójne, warto rozważyć zwiększenie przepustowości wartości w zbiorze DocumentDB.

- Replikować DocumentDB bazy danych w dwóch lub większej liczbie regionów. Wszystkie repliki są czytelne. Za pomocą klienta SDK określ `PreferredLocations` parametru. To uporządkowana lista regionów Azure. Wszystkie operacje odczytu będą wysyłane do pierwszej region dostępnych na liście. Jeśli żądanie nie powiedzie się, klient będzie próbowała pozostałe regiony na liście w kolejności. Aby uzyskać więcej informacji, zobacz [rozwijających się z kontami DocumentDB w przypadku][docdb-multi-region].

**Narzędzia diagnostyczne**. Zaloguj się wszystkie błędy po stronie klienta.


### <a name="writing-data-to-documentdb-fails"></a>Zapisywanie danych w DocumentDB kończy się niepowodzeniem.

**Wykrywanie**. Efektywnej `System.Net.Http.HttpRequestException` lub `Microsoft.Azure.Documents.DocumentClientException`. 

**Odzyskiwanie**

- Zestaw SDK prób automatycznie niepomyślne. Aby ustawić liczbę prób i maksymalny czas oczekiwania, skonfigurować `ConnectionPolicy.RetryOptions`. Wyjątki zgłasza klienta są poza zasad ponów próbę lub nie są przejściowych błędy. 

- Jeśli DocumentDB ogranicza klienta, zwracany jest błąd HTTP 429. Ewidencjonowanie kodu stanu `DocumentClientException`. Jeśli jest wyświetlany błąd 429 spójne, warto rozważyć zwiększenie przepustowości wartości w zbiorze DocumentDB.

- Replikować DocumentDB bazy danych w dwóch lub większej liczbie regionów. Jeśli podstawowy regionu kończy się niepowodzeniem, innego regionu rola zostanie podwyższona do pisania. Można też ręcznie wyzwalanie trybie awaryjnym. Zestawu SDK powoduje automatyczne wykrywanie i routingu, więc kod aplikacji będzie nadal działać po przełączeniu. W okresie pracy awaryjnej (zazwyczaj minut) operacji zapisu będzie miał wyższą opóźnienie zgodnie z zestawu SDK umożliwia znalezienie wyrazów nowy region zapisu. Aby uzyskać więcej informacji, zobacz [rozwijających się z kontami DocumentDB w przypadku][docdb-multi-region].

- Jako powrót utrzymują dokumentu do kolejki kopii zapasowej, a później procesu kolejki.

**Narzędzia diagnostyczne**. Zaloguj się wszystkie błędy po stronie klienta.


## <a name="elasticsearch"></a>Elasticsearch

### <a name="reading-data-from-elasticsearch-fails"></a>Odczyt danych z Elasticsearch zakończy się niepowodzeniem.

**Wykrywanie**. Efektywnej odpowiedni wyjątek dla określonego [klienta Elasticsearch] [ elasticsearch-client] używany. 

**Odzyskiwanie**

- Korzysta z mechanizmu ponów próbę. Każdy komputer kliencki ma własne zasady ponów próbę. 

- Wdrażanie wiele węzłów Elasticsearch i replikacji wysokiej dostępności.

Aby uzyskać więcej informacji, zobacz [Uruchamianie Elasticsearch Azure][elasticsearch-azure].

**Narzędzia diagnostyczne**. Możesz używać narzędzi do monitorowania dla Elasticsearch lub rejestruje wszystkie błędy po stronie klienta ładunek. Zobacz sekcję "Monitorowanie" w [Uruchomiony Elasticsearch Azure][elasticsearch-azure].


### <a name="writing-data-to-elasticsearch-fails"></a>Zapisywanie danych w Elasticsearch kończy się niepowodzeniem.

**Wykrywanie**. Efektywnej odpowiedni wyjątek dla określonego [klienta Elasticsearch] [ elasticsearch-client] używany.  

**Odzyskiwanie**

- Korzysta z mechanizmu ponów próbę. Każdy komputer kliencki ma własne zasady ponów próbę. 
 
- Jeśli aplikacja przeszkadzają poziomu spójności ograniczona, rozważ pisania z `write_consistency` ustawienie `quorum`.

Aby uzyskać więcej informacji, zobacz [Uruchamianie Elasticsearch Azure][elasticsearch-azure].

**Narzędzia diagnostyczne**. Możesz używać narzędzi do monitorowania dla Elasticsearch lub rejestruje wszystkie błędy po stronie klienta ładunek. Zobacz sekcję "Monitorowanie" w [Uruchomiony Elasticsearch Azure][elasticsearch-azure].


## <a name="queue-storage"></a>Magazyn kolejek

### <a name="writing-a-message-to-azure-queue-storage-fails-consistently"></a>Spójne zapisywanie wiadomości do miejsca do magazynowania w kolejce Azure kończy się niepowodzeniem.

**Wykrywanie**. Po ponowienia *N* operacji zapisu nadal kończy się niepowodzeniem.

**Odzyskiwanie**

- Przechowywanie danych w lokalnej pamięci podręcznej i przesyłanie ich dalej zapisywania miejsca do magazynowania później, usługa stał się dostępny.
- Utwórz kolejkę pomocniczej, a do tej kolejki zapisu, jeśli podstawowy kolejki jest niedostępny.

**Narzędzia diagnostyczne**. Metryki [magazynowania][storage-metrics].


### <a name="the-application-cannot-process-a-particular-message-from-the-queue"></a>Aplikacja nie może przetworzyć konkretną wiadomość z kolejki. 

**Wykrywanie**. Określone dla aplikacji. Na przykład wiadomość zawiera nieprawidłowe dane lub warunków logicznych kończy się niepowodzeniem jakiegoś powodu. 

**Odzyskiwanie**

Przenoszenie wiadomości do oddzielnych kolejki. Uruchamianie oddzielnego procesu wiadomości z tej kolejki przyjrzenie się.

Należy rozważyć użycie wiadomości Bus usługi Azure kolejki, który zapewnia [kolejki utraconych] [ sb-dead-letter-queue] funkcji w tym celu.

Uwaga: Jeśli korzystasz z kolejek miejsca do magazynowania z WebJobs, zestaw SDK WebJobs zawiera obsługi wbudowanych uszkodzonych komunikatów. Dowiedz się, [jak używać magazyn kolejek Azure z zestawu SDK WebJobs][sb-poison-message].

**Narzędzia diagnostyczne**. Za pomocą rejestrowania aplikacji.


## <a name="redis-cache"></a>Redis pamięci podręcznej

### <a name="reading-from-the-cache-fails"></a>Odczyt z pamięci podręcznej nie powiedzie się.

**Wykrywanie**. Efektywnej `StackExchange.Redis.RedisConnectionException`.

**Odzyskiwanie**

1. Spróbuj ponownie na przejściowych błędy. Azure Redis pamięci podręcznej obsługuje wbudowane ponów próbę za pośrednictwem zobacz [wskazówki ponów próbę Redis pamięci podręcznej][redis-retry].
2. Traktuj błędy nie zmiennych jako Brak trafienia pamięci podręcznej i powrotu do oryginalnego źródła danych.

**Narzędzia diagnostyczne**. Za pomocą [Narzędzia diagnostyczne Redis pamięci podręcznej][redis-monitor].


### <a name="writing-to-the-cache-fails"></a>Zapisywanie w pamięci podręcznej nie powiedzie się.

**Wykrywanie**. Efektywnej `StackExchange.Redis.RedisConnectionException`.

**Odzyskiwanie**

1. Spróbuj ponownie na przejściowych błędy. Azure Redis pamięci podręcznej obsługuje wbudowane ponów próbę za pośrednictwem zobacz [wskazówki ponów próbę Redis pamięci podręcznej][redis-retry].
2. Jeśli komunikat o błędzie jest wartością zmiennych, należy ją zignorować i umożliwić inne transakcje zapisywanie w pamięci podręcznej później.

**Narzędzia diagnostyczne**. Za pomocą [Narzędzia diagnostyczne Redis pamięci podręcznej][redis-monitor].


## <a name="sql-database"></a>Baza danych SQL

### <a name="cannot-connect-to-the-database-in-the-primary-region"></a>Nie można połączyć z bazą danych w regionie podstawowego.

**Wykrywanie**. Połączenie kończy się niepowodzeniem.

**Odzyskiwanie**

Wymagania wstępne: Bazy danych należy skonfigurować dla aktywnego replikacji geo. Zobacz [SQL Replikacja bazy danych Active Geo -][sql-db-replication].

- W przypadku kwerend odczyt replice pomocniczą. 
- Wstawia i aktualizacji ręcznie nie w replice pomocniczą. Zobacz [zainicjować planowane lub niezaplanowane awaryjnego bazy danych SQL Azure][sql-db-failover].

Replice używa ciągu inne połączenie, więc należy zaktualizować parametry połączenia w aplikacji.


### <a name="client-runs-out-of-connections-in-the-connection-pool"></a>Klient uruchamia się z połączenia w puli połączeń.

**Wykrywanie**. Efektywnej `System.InvalidOperationException` błędy. 

**Odzyskiwanie**

- Spróbuj jeszcze raz.
- Jako plan łagodzenia wyodrębnianie pul połączeń dla poszczególnych przypadków użycia, aby jeden przypadek użycia nie dominuj wszystkie połączenia.
- Zwiększanie pul maksymalna liczba połączeń.

**Narzędzia diagnostyczne**. Dzienniki aplikacji.


### <a name="database-connection-limit-is-reached"></a>Osiągnięto limit połączenia bazy danych. 

**Wykrywanie**. Baza danych SQL Azure ogranicza liczbę pracowników równoczesne, logowania i sesji. Limity zależy od poziomu usług. Aby uzyskać więcej informacji, zobacz [limity zasobów bazy danych SQL Azure][sql-db-limits].

Wykrywanie tych błędów, efektywnej `System.Data.SqlClient.SqlException` i sprawdź wartość `SqlException.Number` dla kodu błędu SQL. Aby uzyskać listę kodów błędów odpowiednich, zobacz [SQL kodów błędów aplikacji klienckich bazy danych SQL: błąd połączenia i innych problemów z bazy danych][sql-db-errors].

**Odzyskiwanie**. Te błędy są traktowane jako przejściowych, aby ponawianie próby może rozwiązać problem. Jeśli trafisz spójne tych błędów, należy rozważyć możliwość skalowania bazy danych.

**Narzędzia diagnostyczne**. [Sys.event_log] [ sys.event_log] kwerenda zwraca połączenia z bazą danych pomyślnie, błędy połączenia i zakleszczenia.

- Tworzenie [alertu] [ azure-alerts] dla nie powiodło się połączenia.

- Włączanie [inspekcji bazy danych SQL] [ sql-db-audit] i sprawdź, czy logowania nie powiodło się.


## <a name="service-bus-messaging"></a>Usługi Bus wiadomości

### <a name="reading-a-message-from-a-service-bus-queue-fails"></a>Odczytywanie wiadomości z kolejki Bus usługi zakończy się niepowodzeniem.

**Wykrywanie**. Przechwytywać wyjątków w kliencie SDK. Klasa podstawowa dla wyjątki Bus usługi jest [MessagingException][sb-messagingexception-class]. Jeśli komunikat o błędzie jest przejściowych, `IsTransient` właściwość ma wartość true. 

Aby uzyskać więcej informacji, zobacz [Usługa Bus wiadomości wyjątki][sb-messaging-exceptions].

**Odzyskiwanie**

1. Spróbuj ponownie na przejściowych błędy. Zobacz [Bus usługi ponów próbę wskazówki][sb-retry].

2. Wiadomości, których nie można dostarczyć dowolnego odbiorcy są umieszczane w *kolejce utraconych wiadomości*. Umożliwia wyświetlanie, nie można odebrać wiadomości, które tej kolejki. Istnieje nie automatycznego oczyszczania kolejki utraconych wiadomości. Wiadomości są zachowywane do czasu ich jawnie pobierania. Zobacz [Omówienie usługi Bus o nazwie utraconych][sb-dead-letter-queue].


### <a name="writing-a-message-to-a-service-bus-queue-fails"></a>Zapisywanie wiadomości do Bus usługi kolejki zakończy się niepowodzeniem.

**Wykrywanie**. Przechwytywać wyjątków w kliencie SDK. Klasa podstawowa dla wyjątki Bus usługi jest [MessagingException][sb-messagingexception-class]. Jeśli komunikat o błędzie jest przejściowych, `IsTransient` właściwość ma wartość true. 

Aby uzyskać więcej informacji, zobacz [Usługa Bus wiadomości wyjątki][sb-messaging-exceptions].

**Odzyskiwanie**

1. Usługa Bus klient będzie ponawiał próbę automatycznie po przejściowych błędy. Domyślnie używa wykładniczego wstecz wył. Po maksymalna liczba powtórzeń lub maksymalny limit czasu klient zgłasza wyjątek. Aby uzyskać więcej informacji, zobacz [Bus usługi ponów próbę wskazówki][sb-retry].

2. Jeśli przekroczenia limitu kolejki klient zgłasza [QuotaExceededException][QuotaExceededException]. Komunikat wyjątku zawiera szczegółowe informacje. Opróżnij niektóre wiadomości z kolejki przed ponawianie próby i warto rozważyć użycie deseniu rozmieszczenie, aby uniknąć ciągłego ponowne próby podczas Przekroczono przydział. Upewnij się również, że właściwość [BrokeredMessage.TimeToLive] nie jest ustawiona zbyt duży. 

3. W obszarze, można zwiększyć elastyczność przy użyciu [kolejki podzielone na partycje lub tematy][sb-partition]. Partycją kolejki lub temat zostanie przypisany do jednego magazynu wiadomości. Jeśli ten magazyn wiadomości jest niedostępna, wszystkie operacje w tym kolejki lub tematu nie powiedzie się. Kolejka podzielone na partycje lub tematu jest podzielona przez wielu magazynów wiadomości. 

4. Tworzenie dwóch nazw Bus usługi w różnych regionów dodatkowej elastyczności i odtworzyć wiadomości. Za pomocą albo replikacji aktywne lub pasywne.

    - Replikacja usługi Active: klient wysyła każdej wiadomości do obu kolejek. Odbiorca wykrywa na obu kolejek. Oznaczanie wiadomości z unikatowym identyfikatorem, więc klienta można odrzucić zduplikowanych wiadomości.

    - Pasywne replikacji: klient wysyła wiadomość do jednej kolejki. W przypadku błędu, Klient przechodzi do innych kolejki. Odbiorca wykrywa na obu kolejek. Tej metody zmniejsza liczbę zduplikowanych wiadomości, które są wysyłane. Jednak odbiorca nadal musi obsługiwać zduplikowanych wiadomości.

    Aby uzyskać więcej informacji, zobacz [Przykładowy GeoReplication] [ sb-georeplication-sample] i [Najważniejsze wskazówki dotyczące izolacji aplikacji przed dostawie Bus usługi i awarii][sb-outages].


### <a name="duplicate-message"></a>Zduplikowany komunikat.

**Wykrywanie**. Sprawdzenie `MessageId` i `DeliveryCount` właściwości wiadomości.

**Odzyskiwanie**

- Jeśli to możliwe projektowanie wiadomości przetwarzania być idempotent. W przeciwnym razie zawierają identyfikatory wiadomości wiadomości, które już są przetwarzane i Sprawdź identyfikator przed przetwarzania wiadomości.

- Włącz wykrywanie duplikatów, tworząc kolejki z `RequiresDuplicateDetection` ma wartość true. To ustawienie, Bus usługi automatycznie usuwa każdej wiadomości wysyłanej z tym samym `MessageId` jako poprzedniej wiadomości.  Pamiętaj o następujących kwestiach:

    -  To ustawienie zapobiega zduplikowanych wiadomości są umieszczone w kolejce. Słuchawka nie uniemożliwia przetwarzanie tego samego komunikatu więcej niż jeden raz.

    - Wykrywanie duplikatów ma przedział czasu. Jeśli duplikat poza tego okna, nie zostanie wykryty.

**Narzędzia diagnostyczne**. Zaloguj się zduplikowane wiadomości.


### <a name="the-application-cannot-process-a-particular-message-from-the-queue"></a>Aplikacja nie może przetworzyć konkretną wiadomość z kolejki. 

**Wykrywanie**. Określone dla aplikacji. Na przykład wiadomość zawiera nieprawidłowe dane lub warunków logicznych kończy się niepowodzeniem jakiegoś powodu. 

**Odzyskiwanie**

Istnieją dwa tryby awaryjne, które należy rozważyć, czy. 

- Odbiorca wykrywa błąd. W tym przypadku należy przenieść wiadomość do kolejki utraconych wiadomości. Później Uruchom oddzielnego procesu przyjrzenie się wiadomości w kolejce utraconych wiadomości.

- Odbiorca kończy się niepowodzeniem w trakcie przetwarzania wiadomości &mdash; na przykład z powodu nieobsługiwanego wyjątku. Do obsługi sprawy, za pomocą `PeekLock` tryb. W tym trybie po wygaśnięciu blokady, wiadomość staje się dostępna dla innych odbiorców. Jeśli wiadomość jest większa niż liczba maksymalna dostarczenia lub czas wygaśnięcia, wiadomość zostanie automatycznie przeniesiona do kolejki utraconych wiadomości.

Aby uzyskać więcej informacji, zobacz [Omówienie usługi Bus o nazwie utraconych][sb-dead-letter-queue].

**Narzędzia diagnostyczne**. Gdy aplikacja przenosi wiadomość do kolejki utraconych, Zapisz zdarzenie do Dzienniki aplikacji.


## <a name="service-fabric"></a>Usługa tkaninie

### <a name="a-request-to-a-service-fails"></a>Żądanie usługi zakończy się niepowodzeniem.

**Wykrywanie**. Usługa zwraca błąd.

**Odzyskiwanie**

- Zlokalizuj serwer proxy ponownie (`ServiceProxy` lub `ActorProxy`) i ponownie wywołaj metodę usługi aktora. 

- **Usługa stanowe**. Zawijanie operacji na niezawodne zbiory w transakcji. W przypadku błędu transakcji zostanie wycofana. Żądanie, jeśli pobierane z kolejki, będą przetwarzane ponownie. 
 
- **Bezstanowa usługi**. Jeśli dane do zewnętrznych magazynu usługi będzie nadal występował, wszystkie operacje muszą być idempotent.


**Narzędzia diagnostyczne**. Dziennik aplikacji

### <a name="service-fabric-node-is-shut-down"></a>Węzeł tkaninie usługi jest zamknięty.

**Wykrywanie**. Token anulowania są przekazywane do usługi `RunAsync` metody. Usługa tkaninie anuluje zadania przed zamknięciem węzeł.

**Odzyskiwanie**. Korzystanie z tokenu anulowania wykrywanie zamknięcia. Gdy usługa tkaninie żąda anulowania, zakończenie pracy i opuszczanie `RunAsync` tak szybko, jak to możliwe.

**Narzędzia diagnostyczne**. Dzienniki aplikacji


## <a name="storage"></a>Miejsca do magazynowania

### <a name="writing-data-to-azure-storage-fails"></a>Zapisywanie danych do magazynowania Azure kończy się niepowodzeniem.

**Wykrywanie**. Klient odbiera błędy podczas pisania.

**Odzyskiwanie**

1. Spróbuj jeszcze raz, aby naprawić błędy przejściowych. [Spróbuj ponownie zasad] [ Storage.RetryPolicies] w kliencie SDK obsługuje to automatycznie.
2. Zaimplementowania deseniem rozmieszczenie unikać utrudnione miejsca do magazynowania.
3. Jeśli N ponawiania próby kończą się niepowodzeniem, należy przeprowadzić bezpieczne powrót. Na przykład:

    - Przechowywanie danych w lokalnej pamięci podręcznej i przesyłanie ich dalej zapisywania miejsca do magazynowania później, usługa stał się dostępny.
    - Jeśli czynność zapisu w zakresie transakcji, zadośćuczynienia transakcji.

**Narzędzia diagnostyczne**. Metryki [magazynowania][storage-metrics].


### <a name="reading-data-from-azure-storage-fails"></a>Odczytywanie danych z magazynu Azure kończy się niepowodzeniem.

**Wykrywanie**. Klient odbiera błędy podczas czytania.

**Odzyskiwanie**

1. Spróbuj jeszcze raz, aby naprawić błędy przejściowych. [Spróbuj ponownie zasad] [ Storage.RetryPolicies] w kliencie SDK obsługuje to automatycznie.
2. Do przechowywania GRS pomoc Zdalna Jeśli czytania od podstawowego punktu końcowego nie powiedzie się, spróbuj odczytu z pomocniczym punktu końcowego. Klient SDK można rozwiązać automatycznie. Zobacz [Replikacja magazyn Azure][storage-replication].
3. Jeśli *N* ponownych prób nie, podejmowanie alternatywnych obniżyć bezpiecznie. Na przykład jeśli obraz produktu nie można pobrać z miejsca do magazynowania, Pokaż obraz zastępczy ogólne.

**Narzędzia diagnostyczne**. Metryki [magazynowania][storage-metrics].


## <a name="virtual-machine"></a>Maszyn wirtualnych

### <a name="connection-to-a-backend-vm-fails"></a>Połączenie do wewnętrznej bazy danych maszyn wirtualnych kończy się niepowodzeniem.

**Wykrywanie**. Błędy połączenia sieciowego.

**Odzyskiwanie**

- Wdrażanie maszyny wirtualne co najmniej dwa wewnętrznej bazy danych w zestawie dostępności za usługi równoważenia obciążenia.

- Jeśli błąd połączenia jest przejściowych, czasami TCP zostanie pomyślnie ponów próbę wysłania wiadomości. 

- Implementowanie zasad ponów próbę w aplikacji. 

- Trwałe lub w innych niż zmiennych błędy, zaimplementować [rozmieszczenie] [ circuit-breaker] deseniem.

- Jeśli wywołujący maszyn wirtualnych przekracza limit wyjściowego sieci, kolejki wychodzącej wstawi w górę. Jeśli kolejki wychodzącej jest zawsze pełne, warto rozważyć Skalowanie zewnętrzne. 

**Narzędzia diagnostyczne**. Dziennik zdarzeń na granicach usługi.

### <a name="vm-instance-becomes-unavailable-or-unhealthy"></a>Wystąpienia maszyn wirtualnych stają się niedostępne lub nieprawidłowe.

**Wykrywanie**. Konfigurowanie usługi równoważenia obciążenia [sondy kondycji] [ lb-probe] którego sygnalizuje, czy wystąpienie maszyn wirtualnych jest uszkodzony. Sonda należy sprawdzić, czy odpowiada funkcje krytyczne. 

**Odzyskiwanie**. Dla każdej warstwy aplikacji wprowadzenia wielu wystąpień maszyn wirtualnych ten sam zestaw dostępność i umieść równoważenia obciążenia przed maszyny wirtualne. Jeśli sonda kondycji kończy się niepowodzeniem, równoważenia obciążenia zatrzymuje wysyłanie nowych połączeń do Nieprawidłowe wystąpienie.

**Narzędzia diagnostyczne**. — Za pomocą usługi równoważenia obciążenia [analizy dziennika][lb-monitor].
- Skonfiguruj system monitorowania do monitorowania wszystkich kondycji monitorowania punktów końcowych.


### <a name="operator-accidentally-shuts-down-a-vm"></a>Operator przypadkowo wyłączy maszyny.

**Wykrywanie**. N/D!

**Odzyskiwanie**. Ustawianie Blokada zasobu z `ReadOnly` poziom. Zobacz [zasoby blokady przy użyciu Menedżera zasobów Azure][rm-locks].

**Narzędzia diagnostyczne**. [Dzienniki aktywności Azure]za pomocą[azure-activity-logs].


## <a name="webjobs"></a>WebJobs

### <a name="continuous-job-stops-running-when-the-scm-host-is-idle"></a>Ciągły zadanie zatrzymane, kiedy host Menedżer sterowania usługami jest bezczynny.

**Wykrywanie**. Przekaż token odwołania do funkcji WebJob. Aby uzyskać więcej informacji, zobacz [łagodnego zamykania][web-jobs-shutdown]. 

**Odzyskiwanie**. Włączanie `Always On` Ustawianie w aplikacji sieci web. Aby uzyskać więcej informacji, zobacz [Uruchamianie tła zadań za pomocą WebJobs][web-jobs].


## <a name="application-design"></a>Projekt aplikacji

### <a name="application-cant-handle-a-spike-in-incoming-requests"></a>Aplikacja nie obsługuje kolekcji w przychodzących żądań.

**Wykrywanie**. W zależności od aplikacji. Typowe objawy:

- Witryny sieci Web powoduje uruchomienie, zwracanie kody błędów 5xx HTTP.
- Zależne usługi, takie jak bazy danych lub miejscem do magazynowania, Rozpocznij, aby ograniczyć żądania. Poszukaj błędów HTTP, takich jak 429 HTTP (zbyt wiele żądań), w zależności od usługi.
- Długość kolejki HTTP rozwoju.

**Odzyskiwanie**

- Możliwość skalowania obsługę lepszą ładowania. 

- Ograniczanie błędów, aby uniknąć błędów kaskadowych przerwać całej aplikacji. Strategie łagodzenia obejmują:

    - Zaimplementowania [Deseniem ograniczania] [ throttling-pattern] w celu uniknięcia utrudnione systemów wewnętrznej bazy danych.
    - Za pomocą [opartych na kolejkach ładowania bilansowania] [ queue-based-load-leveling] buforu żądania i przetwarzać je w odpowiedniej tempie.
    - Określanie priorytetów niektórych klientów. Na przykład jeśli aplikacja ma bezpłatnych i płatnych warstwy, ograniczania klientów w warstwie bezpłatne, ale nie zapłacono klientów. Zobacz [priorytet kolejki wzorca][priority-queue-pattern].

**Narzędzia diagnostyczne**. Rejestrowanie [Diagnostyczne w aplikacji usługi][app-service-logging]. Za pomocą usługi, takie jak [Azure analizy dziennika][azure-log-analytics], [Wniosków aplikacji][app-insights], lub [Nowy Relic] [ new-relic] ułatwiające zrozumienie dzienniki diagnostyczne.


### <a name="one-of-the-operations-in-a-workflow-or-distributed-transaction-fails"></a>Jedna z operacji przepływu pracy lub transakcji rozproszonej zakończy się niepowodzeniem.

**Wykrywanie**. Po ponowienia *N* ją nadal kończy się niepowodzeniem.

**Odzyskiwanie**

- Jako plan łagodzenia zaimplementować [Opiekuna agenta harmonogram] [ scheduler-agent-supervisor] deseń do zarządzania całego przepływu pracy. 
- Nie ponów próbę przekroczenia limitu czasu. Istnieje stawki niskim sukcesu dla tego problemu. 
- Kolejki pracy, aby można było bazę danych.

**Narzędzia diagnostyczne**. Zaloguj się wszystkie operacje (pomyślnego i niepowodzeniem), w tym wyrównywania akcje. Za pomocą korelacji identyfikatory, tak aby można było śledzić wszystkie operacje w obrębie tej samej transakcji.


### <a name="a-call-to-a-remote-service-fails"></a>Połączenie z usługą zdalnego zakończy się niepowodzeniem.

**Wykrywanie**. Kod błędu protokołu HTTP.

**Odzyskiwanie**

1. Spróbuj ponownie na przejściowych błędy. 
2. Jeśli połączenie kończy się niepowodzeniem po *N* prób, wykonywanie akcji alternatywnych. (Aplikacja określone).
3. Zaimplementowania [deseniem rozmieszczenie] [ circuit-breaker] Aby uniknąć błędów kaskadowych. 

**Narzędzia diagnostyczne**. Zaloguj się wszystkie błędy połączenia zdalnego.


## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji o procesie FMA, zobacz [odporność zamierzone dla usług w chmurze] [ resilience-by-design-pdf] (plik PDF do pobrania).

<!-- links -->

[api-management]: https://azure.microsoft.com/documentation/services/api-management/
[api-management-throttling]: ../api-management/api-management-sample-flexible-throttling.md
[app-insights]: ../application-insights/app-insights-overview.md
[app-insights-web-apps]: ../application-insights/app-insights-azure-web-apps.md
[app-service-configure]: ../app-service-web/web-sites-configure.md
[app-service-logging]: ../app-service-web/web-sites-enable-diagnostic-log.md
[app-service-slots]: ../app-service-web/web-sites-staged-publishing.md
[auto-rest-client-retry]: https://github.com/Azure/autorest/blob/master/Documentation/clients-retry.md
[azure-activity-logs]: ../monitoring-and-diagnostics/monitoring-overview-activity-logs.md
[azure-alerts]: ../monitoring-and-diagnostics/insights-alerts-portal.md
[azure-log-analytics]: ../log-analytics/log-analytics-overview.md
[BrokeredMessage.TimeToLive]: https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx
[cassandra-error-handling]: http://www.datastax.com/dev/blog/cassandra-error-handling-done-right
[circuit-breaker]: https://msdn.microsoft.com/library/dn589784.aspx
[docdb-multi-region]: ../documentdb/documentdb-developing-with-multiple-regions.md
[elasticsearch-azure]: guidance-elasticsearch-running-on-azure.md
[elasticsearch-client]: https://www.elastic.co/guide/en/elasticsearch/client/index.html
[health-endpoint-monitoring-pattern]: https://msdn.microsoft.com/library/dn589789.aspx
[onstop-events]: https://azure.microsoft.com/blog/the-right-way-to-handle-azure-onstop-events/
[lb-monitor]: ../load-balancer/load-balancer-monitor-log.md
[lb-probe]: ../load-balancer/load-balancer-custom-probe-overview.md#learn-about-the-types-of-probes
[new-relic]: https://newrelic.com/
[priority-queue-pattern]: https://msdn.microsoft.com/library/dn589794.aspx
[queue-based-load-leveling]: https://msdn.microsoft.com/library/dn589783.aspx
[QuotaExceededException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.quotaexceededexception.aspx
[ra-web-apps-basic]: guidance-web-apps-basic.md
[redis-monitor]: ../redis-cache/cache-how-to-monitor.md
[redis-retry]: ../best-practices-retry-service-specific.md#cache-redis-retry-guidelines
[resilience-by-design-pdf]: http://download.microsoft.com/download/D/8/C/D8C599A4-4E8A-49BF-80EE-FE35F49B914D/Resilience_by_Design_for_Cloud_Services_White_Paper.pdf
[RoleEntryPoint.OnStop]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[RoleEnvironment.Stopping]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.stopping.aspx
[rm-locks]: ../resource-group-lock-resources.md
[sb-dead-letter-queue]: ../service-bus-messaging/service-bus-dead-letter-queues.md
[sb-georeplication-sample]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/GeoReplication
[sb-messagingexception-class]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
[sb-messaging-exceptions]: ../service-bus-messaging/service-bus-messaging-exceptions.md
[sb-outages]: ../service-bus/service-bus-outages-disasters.md#protecting-queues-and-topics-against-datacenter-outages-or-disasters
[sb-partition]: ../service-bus-messaging/service-bus-partitioning.md
[sb-poison-message]: ../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#poison
[sb-retry]: ../best-practices-retry-service-specific.md#service-bus-retry-guidelines
[search-sdk]: https://msdn.microsoft.com/library/dn951165.aspx
[scheduler-agent-supervisor]: https://msdn.microsoft.com/library/dn589780.aspx
[search-analytics]: ../search/search-traffic-analytics.md
[sql-db-audit]: ../sql-database/sql-database-auditing-get-started.md
[sql-db-errors]: ../sql-database/sql-database-develop-error-messages.md#resource-governanance-errors
[sql-db-failover]: ../sql-database/sql-database-geo-replication-failover-portal.md
[sql-db-limits]: ../sql-database/sql-database-resource-limits.md
[sql-db-replication]: ../sql-database/sql-database-geo-replication-overview.md
[storage-metrics]: https://msdn.microsoft.com/library/dn782843.aspx
[storage-replication]: ../storage/storage-redundancy.md
[Storage.RetryPolicies]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.retrypolicies.aspx
[sys.event_log]: https://msdn.microsoft.com/library/dn270018.aspx
[throttling-pattern]: https://msdn.microsoft.com/library/dn589798.aspx
[web-jobs]: ../app-service-web/web-sites-create-web-jobs.md
[web-jobs-shutdown]: ../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful

