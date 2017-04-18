<properties 
    pageTitle="Pamięć podręczna Azure Redis — często zadawane pytania | Microsoft Azure" 
    description="Dowiedz się odpowiedzi na często zadawane pytania, wzorców i najlepszych rozwiązań dla Azure Redis w pamięci podręcznej" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="sdanie"/>

# <a name="azure-redis-cache-faq"></a>Pamięć podręczna Azure Redis — często zadawane pytania

Więcej odpowiedzi na często zadawane pytania, wzorców i najlepszych rozwiązań Azure Redis w pamięci podręcznej. 


## <a name="what-if-my-question-isnt-answered-here"></a>Co zrobić, jeśli Moja nie ma odpowiedzi na pytanie w tym miejscu?

Jeśli na liście nie ma swoje pytanie, napisz nam, a pomożemy Ci znaleźć odpowiedzi.

-   Możesz Zadaj pytanie w [wątku Disqus](#comments) na końcu często zadawanych Pytaniach i współpracować z pamięci podręcznej Azure zespołu i inni członkowie społeczności dotyczący tego artykułu.
-   Aby uzyskać dostęp do większej liczby osób, możesz Zadaj pytanie na [Forum w witrynie MSDN pamięci podręcznej Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=azurecache) i współpracować z pamięci podręcznej Azure zespołu i inni członkowie społeczności.
-   Jeśli chcesz wprowadzić żądanie funkcji można przesłać żądania i pomysłów na [Głos użytkownika pamięci podręcznej Redis Azure](https://feedback.azure.com/forums/169382-cache).
-   Możesz też wysyłać wiadomości e-mail z nami u [Azure pamięci podręcznej zewnętrznych opinii](mailto:azurecache@microsoft.com).

## <a name="azure-redis-cache-basics"></a>Azure podstawy Redis pamięci podręcznej

Często zadawane pytania w tej sekcji omówiono niektóre podstawowe informacje o Azure Redis w pamięci podręcznej.

-    [Co to jest Azure Redis w pamięci podręcznej?](#what-is-azure-redis-cache)
-    [Jak mogę rozpocząć z pamięci podręcznej Azure Redis?](#how-can-i-get-started-with-azure-redis-cache)

Następujące często zadawane pytania dotyczą podstawowych pojęć i pytania dotyczące Azure Redis w pamięci podręcznej i są odbierane w innej sekcji często zadawane pytania.

-   [Jakie oferująca Redis pamięci podręcznej i rozmiar należy używać?](#what-redis-cache-offering-and-size-should-i-use)
-   [Jakie Redis pamięci podręcznej klienci mogą używać?](#what-redis-cache-clients-can-i-use)
-   [Czy istnieje lokalne emulatora Azure Redis w pamięci podręcznej?](#is-there-a-local-emulator-for-azure-redis-cache)
-   [Jak monitorowanie kondycji i wydajności Moje pamięci podręcznej](#how-do-i-monitor-the-health-and-performance-of-my-cache)



## <a name="planning-faqs"></a>Planowanie często zadawane pytania

-   [Jakie oferująca Redis pamięci podręcznej i rozmiar należy używać?](#what-redis-cache-offering-and-size-should-i-use)
-   [Azure wydajności Redis pamięci podręcznej](#azure-redis-cache-performance)
-   [W jakich regionu powinna Znajdź Moje pamięci podręcznej?](#in-what-region-should-i-locate-my-cache)
-   [Jak mam wystawiona Azure Redis w pamięci podręcznej?](#how-am-i-billed-for-azure-redis-cache)
-   [Czy można używać Azure Redis w pamięci podręcznej z chmury dla instytucji rządowych Azure lub w chmurze Chinach Azure?](#can-i-use-azure-redis-cache-with-azure-government-cloud-or-azure-china-cloud)



## <a name="development-faqs"></a>Często zadawane pytania dotyczące opracowywania

-   [Co zrobić z opcji konfiguracji StackExchange.Redis?](#what-do-the-stackexchangeredis-configuration-options-do)
-   [Jakie Redis pamięci podręcznej klienci mogą używać?](#what-redis-cache-clients-can-i-use)
-   [Czy istnieje lokalne emulatora Azure Redis w pamięci podręcznej?](#is-there-a-local-emulator-for-azure-redis-cache)
-   [Jak można uruchamiać polecenia Redis?](#how-can-i-run-redis-commands)
-   [Dlaczego Azure Redis w pamięci podręcznej nie ma w witrynie MSDN informacje dotyczące biblioteki klas takich jak niektóre z tych usług Azure?](#why-doesnt-azure-redis-cache-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services)
-   [Czy mogę używać Azure Redis w pamięci podręcznej jako pamięci podręcznej sesji PHP?](#can-i-use-azure-redis-cache-as-a-php-session-cache)


## <a name="security-faqs"></a>Często zadawane pytania dotyczące zabezpieczeń

-   [Kiedy należy włączyć port bez użycia protokołu SSL do łączenia się z Redis?](#when-should-i-enable-the-non-ssl-port-for-connecting-to-redis)


## <a name="production-faqs"></a>Często zadawane pytania produkcji

-   [Jakie są niektóre z najważniejszych wskazówek produkcji?](#what-are-some-production-best-practices)
-   [Jakie są niektóre z zagadnień podczas korzystania z popularnych poleceń Redis?](#what-are-some-of-the-considerations-when-using-common-redis-commands)
-   [Jak testu wydajności i testowanie wydajności Moje pamięci podręcznej?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
-   [Istotnych kwestiach dotyczących wzrostu puli](#important-details-about-threadpool-growth)
-   [Włączanie serwera ° c uzyskać więcej przepustowość na kliencie podczas korzystania z StackExchange.Redis](#enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis)


## <a name="monitoring-and-troubleshooting-faqs"></a>Monitorowania i rozwiązywania problemów — często zadawane pytania

Często zadawane pytania w tej sekcji omówiono typowe monitorowania i pytania dotyczące rozwiązywania problemów. Aby uzyskać więcej informacji na temat monitorowania i rozwiązywania problemów z wystąpienia Azure Redis w pamięci podręcznej zobacz [jak monitorowanie Azure Redis w pamięci podręcznej](cache-how-to-monitor.md) i [Jak rozwiązywać problemy z pamięci podręcznej Azure Redis](cache-how-to-troubleshoot.md).

-   [Jak monitorowanie kondycji i wydajności Moje pamięci podręcznej](#how-do-i-monitor-the-health-and-performance-of-my-cache)
-   [Moje ustawienia pamięci podręcznej diagnostyki miejsca do magazynowania konta zmienione, co się stało?](#my-cache-diagnostics-storage-account-settings-changed-what-happened)
-   [Dlaczego diagnostyki jest włączone dla niektórych nowych pamięci podręcznej, ale nie inne?](#why-is-diagnostics-enabled-for-some-new-caches-but-not-others)
-   [Dlaczego widzę przekroczenia limitu czasu?](#why-am-i-seeing-timeouts)
-   [Dlaczego moje klienta został odłączony z pamięci podręcznej](#why-was-my-client-disconnected-from-the-cache)


## <a name="prior-cache-offering-faqs"></a>Często zadawane pytania dotyczące poprzednich oferująca pamięci podręcznej

-   [Które oferująca pamięci podręcznej Azure jest dla mnie odpowiednia?](#which-azure-cache-offering-is-right-for-me)


### <a name="what-is-azure-redis-cache"></a>Co to jest Azure Redis w pamięci podręcznej?

Azure Redis pamięci podręcznej jest oparty na popularne Otwórz — źródło [Redis pamięci podręcznej](http://redis.io). Daje dostęp do bezpiecznego, dedykowane Redis pamięci podręcznej, zarządzane przez firmę Microsoft i dostępne z dowolnej aplikacji w Azure. Aby uzyskać bardziej szczegółowe omówienie zobacz stronę produktu [Azure Redis w pamięci podręcznej](https://azure.microsoft.com/services/cache/) na Azure.com.


### <a name="how-can-i-get-started-with-azure-redis-cache"></a>Jak mogę rozpocząć z pamięci podręcznej Azure Redis?

Istnieje kilka sposobów, które ułatwi Ci rozpoczęcie pracy z Azure Redis w pamięci podręcznej.

-    Możesz sprawdzić się z jednym z naszych samouczki dostępne dla [.NET](cache-dotnet-how-to-use-azure-redis-cache.md), [ASP.NET](cache-web-app-howto.md), [Java](cache-java-get-started.md), [Node.js](cache-nodejs-get-started.md)i [Python](cache-python-get-started.md). 
-    Możesz obejrzeć, [jak tworzyć wysokiej wydajności aplikacji przy użyciu programu Microsoft Azure Redis pamięci podręcznej](https://azure.microsoft.com/documentation/videos/how-to-build-high-performance-apps-using-microsoft-azure-cache/).
-    Możesz zapoznaj się z dokumentacją klienta dla klientów, którzy zgodny z językiem rozwoju projekcie, aby dowiedzieć się, jak używać Redis. Istnieje wielu klientów Redis, których można używać z Azure Redis w pamięci podręcznej. Aby uzyskać listę klientów Redis zobacz [http://redis.io/clients](http://redis.io/clients).


Jeśli nie masz jeszcze konta usługi Azure, możesz wykonać następujące czynności:

-    [Otwórz konto Azure bezpłatnie](/pricing/free-trial/?WT.mc_id=redis_cache_hero). Uzyskaj środków używane wypróbować płatnych usług Azure. Nawet w przypadku, gdy środki są używane w górę, możesz zachować konta i za pomocą bezpłatnej usługi Azure i funkcje.
-    [Aktywowanie Visual Studio subskrybentów korzyści](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). Twoja subskrypcja MSDN umożliwia środków co miesiąc, używanego do płatnej usługi Azure.


<a name="cache-size"></a>
### <a name="what-redis-cache-offering-and-size-should-i-use"></a>Jakie oferująca Redis pamięci podręcznej i rozmiar należy używać?
Każdy Azure Redis w pamięci podręcznej oferuje różne poziomy **rozmiar**, **przepustowości** **wysokiej dostępności**i opcje **Umowa dotycząca poziomu usług** .

Poniżej przedstawiono informacje dotyczące wybierania oferująca pamięci podręcznej.

-   **Pamięć**: podstawowe i standardowe poziomów oferują 250 MB – 53 GB. Poziom Premium oferuje do 530 GB z bardziej dostępne [na żądanie](mailto:wapteams@microsoft.com?subject=Redis%20Cache%20quota%20increase). Aby uzyskać więcej informacji zobacz [Azure Redis ceny pamięci podręcznej](https://azure.microsoft.com/pricing/details/cache/).
-   **Wydajność sieci**: Jeśli masz obciążenie pracą, która wymaga wysokiej wydajności warstwie Premium oferuje więcej przepustowości w porównaniu do standardowego lub Basic. W każdej warstwy większych rozmiarów pamięci podręcznej mieć większą przepustowość ze względu na podstawowej maszyn wirtualnych, który obsługuje pamięci podręcznej. Można znaleźć w [poniższej tabeli](#cache-performance) , aby uzyskać więcej informacji.
-   **Przepustowość**: warstwa Premium oferuje maksymalna dostępna przepustowość. Jeśli pamięć podręczną serwera lub klienta osiągnie ograniczenia przepustowości, otrzymasz limity czasu po stronie klienta. Aby uzyskać więcej informacji zobacz poniższej tabeli.
-   **Wysoka dostępność i SLA**: Azure Redis w pamięci podręcznej gwarantuje, że pamięci podręcznej Standard i Premium będzie dostępny co najmniej 99,9% czasu. Aby dowiedzieć się więcej na temat naszych Umowa dotycząca poziomu usług, zobacz [Azure Redis ceny pamięci podręcznej](https://azure.microsoft.com/support/legal/sla/cache/v1_0/). Umowa dotycząca poziomu usług obejmuje wyłącznie łączności z pamięci podręcznej punktów końcowych. Umowa dotycząca poziomu usług nie obejmuje ochrony przed utratą danych. Zaleca się za pomocą funkcji utrzymywanie danych Redis w warstwie Premium zwiększanie ochrony przed utratą danych.
-   **Utrzymywanie danych redis**: warstwa Premium pozwala zachować dane pamięci podręcznej na koncie magazyn Azure. W pamięci podręcznej Basic i standardowe wszystkie dane są przechowywane tylko w pamięci. W przypadku podstawowej infrastruktury problemów może być utracie danych. Zaleca się za pomocą funkcji utrzymywanie danych Redis w warstwie Premium zwiększanie ochrony przed utratą danych. Azure Redis pamięci podręcznej oferuje RDB i AOF (już wkrótce) opcji w utrzymywanie Redis. Aby uzyskać więcej informacji zobacz [jak skonfigurować pozostawanie na Premium Azure Redis potrzeby pamięci podręcznej](cache-how-to-premium-persistence.md).
-   **Klaster redis**: tworzenie buforowanie większa niż 53 GB lub aby shard dane w różnych węzłach Redis, możesz użyć Redis klaster, który jest dostępny w warstwie Premium. Każdy węzeł składa się z głównego replice para pamięci podręcznej wysokiej dostępności. Aby uzyskać więcej informacji zobacz [jak skonfigurować klaster na Premium Azure Redis potrzeby pamięci podręcznej](cache-how-to-premium-clustering.md).
-   **Ulepszone zabezpieczenia i izolacji sieci**: wdrożenie Azure wirtualna sieć (VNET) zawiera ulepszone zabezpieczenia i izolacji Azure Redis w pamięci podręcznej, jak również podsieci, zasad kontroli dostępu, i inne funkcje dodatkowe ograniczenia dostępu. Aby uzyskać więcej informacji zobacz [jak skonfigurować wirtualną sieć obsługę bufor Redis Azure Premium](cache-how-to-premium-vnet.md).
-   **Konfigurowanie Redis**: W standardowym i Premium poziomów, można skonfigurować Redis Keyspace powiadomienia.
-   **Maksymalna liczba połączeń klientów**: warstwa Premium oferuje maksymalną liczbę klientów, łączących się Redis z większej liczby połączeń dla większej wielkości pamięci podręcznej. [Tekst można znaleźć na stronie cennik, aby uzyskać szczegółowe informacje](https://azure.microsoft.com/pricing/details/cache/).
-   **Dedykowane Core Redis serwera**: W warstwie Premium wszystkich wielkości pamięci podręcznej mają dedykowane core Redis. Basic-Standard poziomów C1 rozmiar i powyżej mają dedykowane podstawowa dla serwera Redis.
-   **Redis jest jednym z wątkami** , więc masz więcej niż dwa rdzenie nie oferuje dodatkowe korzyści nad mające tylko dwa rdzenie, ale większych rozmiarów maszyn wirtualnych powodują przepustowości więcej niż mniejsze rozmiary. Jeśli pamięć podręczną serwera lub klienta osiągnie ograniczenia przepustowości, następnie otrzymasz limity czasu po stronie klienta.
-   **Ulepszenia dotyczące wydajności**: pamięć podręczną w warstwie Premium są wdrożone na sprzęt, który jest szybszy procesorów i zapewnia większą wydajność w porównaniu z poziomu Basic lub standardowe. Pamięć podręczną warstwa Premium mają wyższej przepustowości i opóźnienia dolnym.

<a name="cache-performance"></a>
### <a name="azure-redis-cache-performance"></a>Azure wydajności Redis pamięci podręcznej

W poniższej tabeli przedstawiono wartości maksymalnej przepustowości obserwowanych podczas testowania różnych rozmiarach Standard i Premium buforowanie, za pomocą `redis-benchmark.exe` z maszyn wirtualnych Iaas przed punkt końcowy Azure Redis w pamięci podręcznej. Zauważ, że te wartości nie są gwarancji i jest umowa dotycząca poziomu usług dla tych liczb, ale powinna być typowe. Powinny być pobierane przetestować aplikację do określenia rozmiaru prawo pamięci podręcznej aplikacji.

Z tej tabeli możemy rysować następujących wniosków.

-   Przepustowości dla pamięci podręcznej, które mają taki sam rozmiar jest większy w warstwie Premium porównaniu warstwie standardowy. Na przykład z 6 GB pamięci podręcznej, przepustowości P1 jest 140K RPS porównaniu 49 K dla C3.
-   Z Redis klaster przepustowość zwiększa liniowo zwiększenie liczby odłamki (węzłów) w klastrze. Na przykład jeśli utworzysz klastrze P4 10 odłamki, następnie dostępna przepustowość jest 250K * 10 = 2,5 miliona RPS.
-   Przepustowości dla większych rozmiarów klucza jest wyższy w warstwie Premium porównaniu warstwie standardowy.

| Cennik warstwy             | Rozmiar   | Rdzenie Procesora | Dostępna przepustowość                                    | Rozmiar klucza 1 KB                            |
|--------------------------|--------|-----------|--------------------------------------------------------|------------------------------------------|
| **Rozmiary standardowe pamięci podręcznej** |        |           | **MB na sekundę (Mb/s)-megabajtów na sekundę (MB/s)** | **Żądania na sekundę (RPS)**            |
| C0                       | 250 MB | Udostępnione    | 5 / 0.625                                              | 600                                      |
| C1                       | 1 GB   | 1         | 100 / 12,5                                             | 12200                                    |
| C2                       | 2,5 GB | 2         | 200 / 25                                               | 24000                                    |
| C3                       | 6 GB   | 4         | 400 / 50                                               | 49000                                    |
| C4                       | 13 GB  | 2         | 500 / 62,5                                             | 61000                                    |
| C5                       | 26 GB  | 4         | 1000 / 125                                             | 115000                                   |
| C6                       | 53 GB  | 8         | 2000 / 250                                             | 150000                                   |
| **Rozmiar pamięci podręcznej Premium**  |        | **Rdzenie Procesora na shard**  |                                         | **Żądania na sekundę (RPS) na shard** |
| P1                       | 6 GB   | 2         | 1000 / 125                                             | 140000                                   |
| P2                       | 13 GB  | 4         | 2000 / 250                                             | 220000                                   |
| P3                       | 26 GB  | 4         | 2000 / 250                                             | 220000                                   |
| P4                       | 53 GB  | 8         | 4000 / 500                                             | 250000                                   |


Aby uzyskać instrukcje dotyczące pobierania narzędzia Redis, takich jak `redis-benchmark.exe`, zobacz [sposobu uruchamiania polecenia Redis?](#cache-commands) sekcji.

<a name="cache-region"></a>
### <a name="in-what-region-should-i-locate-my-cache"></a>W jakich regionu powinna Znajdź Moje pamięci podręcznej?

Dla uzyskania najlepszej wydajności i opóźnienie najniższych Znajdź pamięć podręczną Redis Azure w tym samym regionie jako aplikacji klienckiej pamięci podręcznej.

<a name="cache-billing"></a>
### <a name="how-am-i-billed-for-azure-redis-cache"></a>Jak mam wystawiona Azure Redis w pamięci podręcznej?

Azure ceny Redis pamięci podręcznej jest [tutaj](https://azure.microsoft.com/pricing/details/cache/). Na stronie cennik lista, ceny za godzinę. Pamięci podręcznej są wystawiona na zasadzie na minutę od momentu tworzona w pamięci podręcznej czasu usuniętego pamięci podręcznej. Istnieje opcja zatrzymywanie lub wstrzymywanie rozliczenie pamięci podręcznej.


## <a name="can-i-use-azure-redis-cache-with-azure-government-cloud-or-azure-china-cloud"></a>Czy można używać Azure Redis w pamięci podręcznej z chmury dla instytucji rządowych Azure lub w chmurze Chinach Azure?

Tak, Azure Redis w pamięci podręcznej jest dostępna w chmurze dla instytucji rządowych Azure i w chmurze Chinach Azure. Zauważ, że adresy URL do uzyskiwania dostępu i zarządzanie Azure Redis w pamięci podręcznej są różne w chmurze dla instytucji rządowych Azure i chmury Chinach Azure w porównaniu z chmury publicznej Azure. Aby uzyskać więcej informacji na zagadnienia, korzystając z chmury dla instytucji rządowych Azure i Azure Chinach chmura Azure Redis w pamięci podręcznej zobacz [Bazach danych dla instytucji rządowych Azure - Azure Redis w pamięci podręcznej](../azure-government/documentation-government-services-database.md#azure-redis-cache) i [Azure Chinach Cloud - Azure Redis w pamięci podręcznej](https://www.azure.cn/documentation/services/redis-cache/).

Aby informacji na temat Azure Redis w pamięci podręcznej za pomocą programu PowerShell w chmurze dla instytucji rządowych Azure i chmura Chin Azure zobacz [jak nawiązywanie połączenia z chmury dla instytucji rządowych Azure lub w chmurze Chin Azure](cache-howto-manage-redis-cache-powershell.md#how-to-connect-to-azure-government-cloud-or-azure-china-cloud).


<a name="cache-configuration"></a>
### <a name="what-do-the-stackexchangeredis-configuration-options-do"></a>Co zrobić z opcji konfiguracji StackExchange.Redis?

StackExchange.Redis zawiera wiele opcji. Ta sekcja zawiera informacje o niektóre typowe ustawienia. Aby uzyskać bardziej szczegółowe informacje na temat opcji StackExchange.Redis zobacz [Konfiguracja StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md).

ConfigurationOptions|Opis|Zalecenia
---|---|---
AbortOnConnectFail|Jeśli wartość true, połączenie nie nawiąże połączenie po awarii sieci.|Wartość false i umożliwić StackExchange.Redis automatycznie ponownie nawiązać połączenie.
ConnectRetry|Liczba powtórzeń próby nawiązania połączenia podczas początkowego połączenia.| Znajdziesz następujące wskazówki. |
ConnectTimeout|Limit czasu w ms na łączenie operacji.| Znajdziesz następujące wskazówki. |

W większości przypadków domyślne wartości klienta są wystarczające. Można dostosować opcje na podstawie swojej obciążenia.

-   **Ponowne próby**
    -   ConnectRetry i ConnectTimeout ogólne wskazówki jest szybkie kończą się niepowodzeniem i spróbuj ponownie. To jest oparty na obciążenia systemu i ile czasu na średnie ma dla klienta w odniesieniu do wydaj polecenie Redis i otrzymujesz odpowiedź.
    -   Poinformuj StackExchange.Redis automatycznie ponownie zamiast sprawdzanie stanu połączenia i ponowne łączenie samodzielnie. **Unikaj przy użyciu właściwości ConnectionMultiplexer.IsConnected**.
    -   Snowballing — czasami może wystąpić problem są ponawianie to snowballs i nigdy nie odzyskuje. W tym przypadku należy rozważyć przy użyciu algorytmu ponów próbę wykładniczego wycofywania, zgodnie z opisem w [ponawiania próby ogólne wskazówki](best-practices-retry-general.md) opublikowane przez grupie Patterns firmy Microsoft i wskazówki.
-   **Wartości limitu czasu**
    -   Należy rozważyć, czy usługi Obciążenie pracą, a następnie odpowiednio ustawić wartości. Jeśli przechowujesz duże wartości ustaw wyższa wartość limitu czasu.
    -   Ustawianie `AbortOnConnectFail` do StackExchange.Redis FAŁSZ oraz pozwalają ponownie nawiązać połączenie za Ciebie.
    -   Za pomocą jednego wystąpienia ConnectionMultiplexer dla aplikacji. LazyConnection umożliwia tworzenie jedno wystąpienie, który został zwrócony przez właściwości połączenia, jak pokazano na [Nawiązywanie połączenia z pamięci podręcznej, za pomocą klasy ConnectionMultiplexer](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).
    -   Ustawianie `ConnectionMultiplexer.ClientName` właściwość do aplikacji wystąpienie unikatową nazwę w celach diagnostycznych.
    -   Używanie wielu `ConnectionMultiplexer` wystąpienia obciążenia niestandardowe.
    -   Jeśli masz o różnych szerokościach obciążenia w aplikacji, można wykonać ten model. Na przykład:
    -   Można mieć jeden multiplekser dotyczące dużych kluczy.
    -   Można mieć jeden multiplekser na sposób postępowania z małych klawiszy.
    -   Można ustawić różne wartości limitów czasu połączeń i ponawiania dla każdego ConnectionMultiplexer, którego używasz.
    -   Ustawianie `ClientName` właściwość na każdej multiplekser ułatwiające diagnostyki.
    -   Spowoduje to przejście do więcej Usprawniona opóźnienie na `ConnectionMultiplexer`.

### <a name="what-redis-cache-clients-can-i-use"></a>Jakie Redis pamięci podręcznej klienci mogą używać?

Jedną z najważniejszych funkcji Redis zakłada, że wielu klientów obsługi wielu języków różnych rozwoju. Aby uzyskać bieżącą listę klientów zobacz [Redis klientów](http://redis.io/clients). Samouczki, obejmujące wiele różnych językach i klientów Dowiedz się, [jak używać Azure Redis w pamięci podręcznej](cache-dotnet-how-to-use-azure-redis-cache.md) , a następnie kliknij odpowiedni język z przełącznika języka w górnej części tego artykułu.

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

<a name="cache-emulator"></a>
### <a name="is-there-a-local-emulator-for-azure-redis-cache"></a>Czy istnieje lokalne emulatora Azure Redis w pamięci podręcznej?

Nie ma żadnych lokalnych emulator Azure Redis w pamięci podręcznej, ale można uruchomić wersję MSOpenTech redis server.exe z [Redis narzędzi wiersza polecenia](https://github.com/MSOpenTech/redis/releases/) na komputerze lokalnym i połączyć się z nim uzyskiwania dostępu obsługi podobne do emulatora lokalnej pamięci podręcznej, jak pokazano w poniższym przykładzie.


    private static Lazy<ConnectionMultiplexer>
        lazyConnection = new Lazy<ConnectionMultiplexer>
        (() =>
        {
            // Connect to a locally running instance of Redis to simulate a local cache emulator experience.
            return ConnectionMultiplexer.Connect("127.0.0.1:6379");
        });
    
        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }


Opcjonalnie można skonfigurować pliku [redis.conf](http://redis.io/topics/config) lepiej zgodnie z [domyślnych ustawień pamięci podręcznej](cache-configure.md#default-redis-server-configuration) usługi online Azure Redis pamięci podręcznej, w razie potrzeby.

<a name="cache-commands"></a>
### <a name="how-can-i-run-redis-commands"></a>Jak można uruchamiać polecenia Redis?

Możesz użyć dowolnej z poleceń na [Redis polecenia](http://redis.io/commands#) z wyjątkiem poleceń wymieniony w artykule [Redis polecenia nie są obsługiwane w pamięci podręcznej Azure Redis](cache-configure.md#redis-commands-not-supported-in-azure-redis-cache). Aby uruchomić polecenia Redis dostępnych jest kilka możliwości.

-   Jeśli masz Standard lub Premium pamięci podręcznej, możesz uruchomić polecenia Redis za pomocą [Konsoli Redis](cache-configure.md#redis-console). Zapewnia bezpieczny sposób, aby uruchomić polecenia Redis w portalu Azure.
-   Można również użyć narzędzi wiersza polecenia Redis. Aby korzystać z nich, wykonaj następujące czynności.
-   Pobierz [Redis narzędzi wiersza polecenia](https://github.com/MSOpenTech/redis/releases/).
-   Nawiązywanie połączenia z pamięci podręcznej, za pomocą `redis-cli.exe`. Przekazać w pamięci podręcznej punktu końcowego za pomocą których przełączyć -h i klucza na podstawie jak pokazano w poniższym przykładzie.
-   `redis-cli -h <your cache="" name="">
.redis.cache.windows.net -a <key>
  `
  - Uwaga SSL port nie działają narzędzia wiersza polecenia Redis, że można użyć narzędzia, takie jak `stunnel` aby zapewnić bezpieczne połączenie w obszarze narzędzia do portu SSL, postępując zgodnie z instrukcjami wyświetlanymi w ogłoszeniu w blogu [O ASP.NET stan dostawcy sesji dla Redis wersji Preview](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) .

<a name="cache-reference"></a>
### <a name="why-doesnt-azure-redis-cache-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services"></a>Dlaczego Azure Redis w pamięci podręcznej nie ma w witrynie MSDN informacje dotyczące biblioteki klas takich jak niektóre z tych usług Azure?

Microsoft Azure Redis w pamięci podręcznej jest oparty na popularne Otwórz źródło Redis pamięci podręcznej i można uzyskać do nich dostęp przez szeroką gamę [Redis klientów](http://redis.io/clients) , które są dostępne w wielu językach programowania. Każdy komputer kliencki ma własny interfejs API, który wywołań wystąpieniu pamięci podręcznej Redis za pomocą [polecenia Redis](http://redis.io/commands).

Ponieważ różni każdego klienta w witrynie MSDN; jest nie jedno odwołanie scentralizowane zajęć Zamiast tego każdy komputer kliencki zachowuje dokumentacji odwołania. Oprócz dokumentacji istnieje kilka samouczki przedstawiający, jak rozpocząć pracę z Azure Redis pamięci podręcznej za pomocą różnych językach i klienci pamięci podręcznej. Aby uzyskać dostęp do tych samouczkach, Dowiedz się, [jak używać Azure Redis w pamięci podręcznej](cache-dotnet-how-to-use-azure-redis-cache.md) i kliknij odpowiedni język z przełącznika języka w górnej części tego artykułu.


### <a name="can-i-use-azure-redis-cache-as-a-php-session-cache"></a>Czy mogę używać Azure Redis w pamięci podręcznej jako pamięci podręcznej sesji PHP?

Tak, aby użyć Azure Redis w pamięci podręcznej jako pamięci podręcznej sesji PHP, określ parametry połączenia do wystąpienia Azure Redis w pamięci podręcznej w `session.save_path`.

>[AZURE.IMPORTANT] Podczas używania Azure Redis w pamięci podręcznej jako pamięci podręcznej sesji PHP, musisz adresu URL kodowanie klucz zabezpieczeń używany do łączenia z pamięci podręcznej, jak pokazano w poniższym przykładzie.
>
>`session.save_path = "tcp://mycache.redis.cache.windows.net:6379?auth=<url encoded primary or secondary key here>";`
>
>Jeśli klucz nie jest zakodowany w adresie URL, może zostać wyświetlony wyjątków podobny do następującego:`Failed to parse session.save_path`

Aby uzyskać więcej informacji o używaniu Redis pamięci podręcznej jako pamięci podręcznej sesji PHP z klientem PhpRedis zobacz [obsługi PHP sesji](https://github.com/phpredis/phpredis#php-session-handler).



<a name="cache-ssl"></a>
### <a name="when-should-i-enable-the-non-ssl-port-for-connecting-to-redis"></a>Kiedy należy włączyć port bez użycia protokołu SSL do łączenia się z Redis?

Redis serwer nie obsługuje protokołu SSL poza pole, ale nie Azure Redis w pamięci podręcznej. Jeśli łączysz się Azure Redis w pamięci podręcznej, a klient obsługuje protokół SSL, takich jak StackExchange.Redis, należy użyć protokołu SSL.

Należy zauważyć, że port SSL nie jest domyślnie wyłączone dla nowych wystąpień Azure Redis w pamięci podręcznej. Jeśli klient nie obsługuje protokołu SSL, należy włączyć port bez użycia protokołu SSL, postępując zgodnie z instrukcjami w sekcji [porty dostępu](cache-configure.md#access-ports) artykuł [Konfigurowanie pamięci podręcznej w pamięci podręcznej Azure Redis](cache-configure.md) .

Redis narzędzi, takich jak `redis-cli` nie działa z portem SSL, ale można użyć narzędzia, takie jak `stunnel` aby zapewnić bezpieczne połączenie w obszarze narzędzia do portu SSL, postępując zgodnie z instrukcjami wyświetlanymi w ogłoszeniu w blogu [O ASP.NET stan dostawcy sesji dla Redis wersji Preview](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) .

Aby uzyskać instrukcje dotyczące pobierania narzędzia Redis, zobacz [sposobu uruchamiania polecenia Redis?](#cache-commands) sekcji.



### <a name="what-are-some-production-best-practices"></a>Jakie są niektóre z najważniejszych wskazówek produkcji?

-   [StackExchange.Redis najważniejsze wskazówki](#stackexchangeredis-best-practices)
-   [Konfiguracja i pojęć](#configuration-and-concepts)
-   [Testowanie wydajności](#performance-testing)

#### <a name="stackexchangeredis-best-practices"></a>StackExchange.Redis najważniejsze wskazówki

-   Ustawianie `AbortConnect` ma wartość FAŁSZ, a następnie umożliwić ConnectionMultiplexer automatycznie ponownie nawiązać połączenie. [Zobacz tutaj, aby uzyskać szczegółowe informacje](https://gist.github.com/JonCole/36ba6f60c274e89014dd#file-se-redis-setabortconnecttofalse-md).
-   Ponowne używanie ConnectionMultiplexer — nie utworzyć nową dla każdego żądania. `Lazy<ConnectionMultiplexer>` Wzór [pokazano w przykładzie](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache) zdecydowanie zaleca się.
-   Redis działa najlepiej z mniejszymi wartościami, dlatego warto rozważyć siekanie zapasowych większy danych w wielu kluczy. W [tym Redis dyskusji](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ)100 kb jest traktowany jako "duże". Przeczytaj [Ten artykuł](https://gist.github.com/JonCole/db0e90bedeb3fc4823c2#large-requestresponse-size) , na przykład problemu, który może być spowodowany duże wartości.
-   Konfigurowanie [ustawień puli](#important-details-about-threadpool-growth) , aby uniknąć przekroczenia limitu czasu.
-   Użyj co najmniej connectTimeout domyślne 5 sekund. Pozwoli to uzyskać StackExchange.Redis wystarczająco dużo czasu, aby ponownie nawiązać połączenie, w przypadku blip sieci.
-   Należy pamiętać o wydajności kosztów związanych z różnych operacji, którego używasz. Na przykład `KEYS` polecenie to operacji O(n) i należy unikać. [Witryny redis.io](http://redis.io/commands/) zawiera szczegóły dotyczące złożoności czasu dla każdej operacji, która obsługuje. Kliknij pozycję kolejnych poleceń, aby wyświetlić złożoności dla każdej operacji.

#### <a name="configuration-and-concepts"></a>Konfiguracja i pojęć

-   Za pomocą standardowy lub warstwa Premium systemów produkcji. Podstawowe warstwa jest pojedynczy węzeł systemie nie replikacji danych nie Umowa dotycząca poziomu usług. Ponadto za pomocą co najmniej pamięci podręcznej C1. Pamięć podręczną C0 naprawdę są przeznaczone dla scenariuszy prosty deweloperów/testowania.
-   Pamiętaj, że Redis jest magazynu danych **W pamięci** . [Ten](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) artykuł, aby pamiętać o scenariuszy, w którym mogą zostać utracone dane.
-   Opracowanie systemu tak, aby może obsługiwać połączenia blips [ze względu na poprawki i pracy awaryjnej](https://gist.github.com/JonCole/317fe03805d5802e31cfa37e646e419d#file-azureredis-patchingexplained-md).

#### <a name="performance-testing"></a>Testowanie wydajności

-   Uruchom przy użyciu `redis-benchmark.exe` do oswój się wcześniej z przepustowości możliwe przed zapisaniem własne wydajności testów. Należy zauważyć, że tego testu redis nie obsługuje protokołu SSL, więc będzie [włączyć port SSL nie za pośrednictwem portalu Azure](cache-configure.md#access-ports) przed uruchomieniu test. Przykłady można znaleźć [jak testu wydajności i testowanie wydajności Moje pamięci podręcznej?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
-   W tym samym regionie jako wystąpienia pamięci podręcznej Redis powinny być klienta maszyn wirtualnych do testowania.
-   Zalecamy używanie Dv2 maszyn wirtualnych serii klienta lepiej sprzętu i będzie dają najlepsze wyniki.
-   Upewnij się, klient maszyn wirtualnych, możesz wybrać ma co najmniej tyle przetwarzania i badania możliwości przepustowości jako pamięci podręcznej. 
-   Na komputerze klienckim, należy włączyć VRSS, jeśli korzystasz z systemu Windows. [Zobacz tutaj, aby uzyskać szczegółowe informacje](https://technet.microsoft.com/library/dn383582.aspx).
-   Wystąpienia Redis Premium warstwa będzie mieć lepiej sieci opóźnienie i przepustowość ponieważ są uruchamiane na lepsze sprzętu dla Procesora i sieci.

<a name="cache-redis-commands"></a>
### <a name="what-are-some-of-the-considerations-when-using-common-redis-commands"></a>Jakie są niektóre z zagadnień podczas korzystania z popularnych poleceń Redis?

-   Niektóre polecenia Redis, które zająć dużo czasu, aby zakończyć bez opis wpływu te polecenia nie powinna działać.
    -   Na przykład nie należy używać polecenia [klawiszy](http://redis.io/commands/keys) produkcji, jak może zająć dużo czasu, aby zwrócić w zależności od liczby klawiszy. Redis jest serwerem pojedynczym wątku i przetwarza polecenia jednym naraz. Jeśli masz inne polecenia wydawane po klawiszy, ta osoba nie będą przetwarzane do momentu Redis przetwarzanie polecenia klawiszy. [Witryny redis.io](http://redis.io/commands/) zawiera szczegóły dotyczące złożoności czasu dla każdej operacji, która obsługuje. Kliknij pozycję kolejnych poleceń, aby wyświetlić złożoności dla każdej operacji.
-   Rozmiary kluczy — należy używać małych klucza/wartości lub duże wartości klucza? Na ogół zależy od tego scenariusza. Jeśli rozwiązania wymaga większych kluczy następnie można dostosować ConnectionTimeout i spróbuj ponownie wartości i dostosować logiki ponów próbę. Z perspektywy serwera Redis mniejszymi wartościami są obserwowane mają lepszą wydajność.
-   Oznacza to, że nie można przechowywać większe wartości w Redis; należy pamiętać o następujących zagadnieniach. Opóźnienia będzie wyższa. Jeśli masz jeden zestaw danych, który jest większy i jedną, która jest mniejsza, możesz użyć wielu wystąpień ConnectionMultiplexer, każdego skonfigurowanego z innego zestawu wartości limitu czasu i spróbuj ponownie, zgodnie z opisem w poprzedniej sekcji [Opcje konfiguracji StackExchange.Redis czego](#cache-configuration) .



<a name="cache-benchmarking"></a>
### <a name="how-can-i-benchmark-and-test-the-performance-of-my-cache"></a>Jak testu wydajności i testowanie wydajności Moje pamięci podręcznej?

-   [Włączanie diagnostyki pamięci podręcznej](cache-how-to-monitor.md#enable-cache-diagnostics) , dzięki czemu można [Monitorowanie](cache-how-to-monitor.md) kondycji pamięci podręcznej. Możesz wyświetlać metryki w portalu Azure i można [pobrać i przeglądanie](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) za pomocą narzędzia wyboru.
-   Umożliwia redis benchmark.exe ładowanie test serwera Redis.
-   Upewnij się, czy testowania klienta i pamięci podręcznej Redis obciążenia są w tym samym regionie.
-   Użyj redis cli.exe i monitorowanie pamięci podręcznej, za pomocą polecenia informacje.
-   Jeśli do ładowania powoduje fragmentacji pamięci wysokiej następnie możesz należy rozmiarów rozmiar pamięci podręcznej.
-   Aby uzyskać instrukcje dotyczące pobierania narzędzia Redis, zobacz [sposobu uruchamiania polecenia Redis?](#cache-commands) sekcji.

Poniżej przedstawiono przykład zastosowania redis benchmark.exe. Aby uzyskać dokładne wyniki Uruchom to polecenie z maszyn wirtualnych w tym samym regionie jako pamięci podręcznej.

-   Za pomocą 1 ładunku k żądań potokowa SET testowych

    redis benchmark.exe - h **yourcache**. redis.cache.windows.net **yourAccesskey** -t Ustawianie - n 1000000 -d 1024 - P 50
    
-   Test potokowa uzyskiwanie żądań używających 1 ładunku k. 
    Uwaga: Uruchom zestaw testowanie przedstawiony powyżej, aby wypełnić pamięci podręcznej
    
    redis benchmark.exe - h **yourcache**. redis.cache.windows.net **yourAccesskey** -t uzyskiwanie -n 1000000 - d 1024 - P 50

<a name="threadpool"></a>
### <a name="important-details-about-threadpool-growth"></a>Istotnych kwestiach dotyczących wzrostu puli

Puli CLR występują dwa typy threads - "Pracownika" i "Port We/Wy zakończenia" (czyli IOCP) wątków. 

-   Wątków są używane podczas dla elementów takich jak przetwarzanie `Task.Run(…)` lub `ThreadPool.QueueUserWorkItem(…)` metod. Tych wątków są również używane przez różne składniki w CLR podczas pracy wymaganym w wątku tła.
-   Wątki IOCP są używane sytuacji Jo asynchroniczne (np. do czytania z sieci). 

Puli wątków udostępnia nowe wątków lub wątki zakończenia We/Wy na żądanie (bez dowolnego ograniczania) aż osiągnie ona ustawienie "Minimalny" dla każdego typu wątku. Minimalna liczba wątków domyślnie do liczby procesorów w systemie. 

Gdy liczba istniejących (zajęty) wątki trafień "minimalna" Liczba wątków, puli będzie ograniczenia szybkości jest wstawia go nowych wątków jeden wątek na 500 milisekund. Oznacza to, że jeśli systemu serii pracy wymagających wątku IOCP, go będzie przetwarzał działające bardzo szybko. Jednak jeśli serii pracy jest większa niż ustawień "Minimum", będzie pewne opóźnienie w przetwarzającym część pracy puli oczekuje na jedną z dwóch czynności nastąpić.

1. Istniejące wątku zwolnieniu podczas pracy.
1. Nie istniejącego wątku staje się bezpłatnie przez 500ms, dzięki utworzeniu nowego wątku.

Zasadniczo oznacza to, że gdy liczba wątków zajęty jest większa niż wątków Min, prawdopodobnie realizujesz 500ms opóźnienie przed ruch sieciowy jest przetwarzana przez aplikację. Ponadto należy pamiętać, że jeśli istniejący wątku pozostaje bezczynne przez okres dłuższy niż 15 sekundach (w oparciu o I pamiętaj), zostaną wyczyszczone w górę i cyklu wzrostu i kurczenia można powtórzyć

Jeśli przyjrzymy się przykładowy komunikat o błędzie z StackExchange.Redis (Tworzenie 1.0.450 lub nowsza), pojawi się teraz wydrukowaniu statystyki puli (zobacz informacje o IOCP i pracownik poniżej).

    System.TimeoutException: Timeout performing GET MyKey, inst: 2, mgr: Inactive, 
    queue: 6, qu: 0, qs: 6, qc: 0, wr: 0, wq: 0, in: 0, ar: 0, 
    IOCP: (Busy=6,Free=994,Min=4,Max=1000), 
    WORKER: (Busy=3,Free=997,Min=4,Max=1000)

W powyższym przykładzie widać wątku IOCP są 6 wątków zajęty, a system jest skonfigurowany do zezwalania 4 wątków minimalne. W tym przypadku klient będzie prawdopodobnie umieścić dwa opóźnienia 500 ms ponieważ 6 > 4.

Uwaga StackExchange.Redis można trafienie limity czasu jeśli otrzymuje ograniczenie wzrostu IOCP lub pracownika wątków.

### <a name="recommendation"></a>Zalecenia

Uwagę powyższych informacji, zalecamy że klienci wartość minimalna konfiguracja wątków IOCP i pracowników coś większa niż wartość domyślna. Firma Microsoft nie można nadać uniwersalny wytyczne dotyczące co ta wartość powinna być ponieważ prawo wartość dla jednej aplikacji będzie zbyt niskiej/wysokiej dla innej aplikacji. To ustawienie może również wpływ na wydajność innych części aplikacji skomplikowane, więc należy dostosować to ustawienie do określonych potrzeb każdego z klientów. Dobre miejsce rozpoczęcia 200 lub 300, a następnie przetestuj i dostosować a stosownie do potrzeb.

Jak skonfigurować to ustawienie:

-   W programie ASP.NET [Ustawienia konfiguracji "minIoThreads"][] w obszarze służy `<processModel>` element konfiguracji w pliku web.config. Jeśli używasz umieszczona Azure witryn sieci Web, to ustawienie nie jest dostępne za pośrednictwem opcje konfiguracji. Jednak nadal można ustawić to programowy (patrz poniżej) z metodę Application_Start w global.asax.cs.

> **Ważna uwaga:** wartości określonej w tym element konfiguracji to ustawienie *na podstawowe* . Na przykład jeśli maszyna 4 podstawowych i chcesz ustawienia minIOThreads być 200 w czasie rzeczywistym, możesz użyć `<processModel minIoThreads="50"/>`.

-   Poza ASP.NET za pomocą [ThreadPool.SetMinThreads(...)](https://msdn.microsoft.com/library/system.threading.threadpool.setminthreads.aspx) INTERFEJS API.

<a name="server-gc"></a>
### <a name="enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis"></a>Włączanie serwera ° c uzyskać więcej przepustowość na kliencie podczas korzystania z StackExchange.Redis

Włączanie ° c server można zoptymalizować klienta i zapewnić lepszą wydajność i przepustowość podczas korzystania z StackExchange.Redis. Aby uzyskać więcej informacji na serwerze ° c i jak je włączyć zobacz następujące artykuły.

-   [Aby włączyć serwer ° c](https://msdn.microsoft.com/library/ms229357.aspx)
-   [Podstawy śmieci](https://msdn.microsoft.com/library/ee787088.aspx)
-   [Wydajność i śmieci](https://msdn.microsoft.com/library/ee851764.aspx)







<a name="cache-monitor"></a>
### <a name="how-do-i-monitor-the-health-and-performance-of-my-cache"></a>Jak monitorowanie kondycji i wydajności Moje pamięci podręcznej

Pamięć podręczna Microsoft Azure Redis wystąpienia można monitorować w [Azure portal](https://portal.azure.com). Można wyświetlić metryki, przypiąć metryki wykresy Startboard, dostosowywanie zakres dat i godzin monitorowania wykresy, dodać i usunąć metryki z wykresów i Ustaw alerty, gdy są spełnione określone warunki. Aby uzyskać więcej informacji zobacz [Monitorowanie Azure Redis w pamięci podręcznej](cache-how-to-monitor.md).

W sekcji **Obsługa + Rozwiązywanie problemów z** karta Redis pamięci podręcznej **Ustawienia** zawiera również kilka narzędzi do monitorowania i rozwiązywania problemów z pamięci podręcznej. 

-   **Rozwiązywanie problemów z** zawiera informacje dotyczące typowych problemów i strategie dotyczące rozwiązywania tych problemów.
-   **Dzienniki inspekcji** zawiera informacje dotyczące akcje wykonywane w pamięci podręcznej. Za pomocą filtrowania można również rozwinąć ten widok, aby uwzględnić inne zasoby.
-   **Kondycja zasobów** oczekuje zasobu i wskazuje, czy jest uruchomiony w zgodnie z oczekiwaniami. Aby uzyskać więcej informacji o usłudze kondycji zasobu Azure zobacz [Omówienie kondycji Azure zasobów](../resource-health/resource-health-overview.md).
-   **Nowy obsługuje żądania** udostępnia opcje, aby otworzyć prośbę o pomoc techniczną dla pamięci podręcznej.

Te narzędzia umożliwiają monitorowanie kondycji usługi wystąpień Azure Redis w pamięci podręcznej i ułatwiają zarządzanie pamięci podręcznej aplikacji. Aby uzyskać więcej informacji zobacz [Pomoc techniczna i rozwiązywanie problemów z ustawieniami](cache-configure.md#support-amp-troubleshooting-settings).

### <a name="my-cache-diagnostics-storage-account-settings-changed-what-happened"></a>Moje ustawienia pamięci podręcznej diagnostyki miejsca do magazynowania konta zmienione, co się stało?

Pamięć podręczną w tym samym region i subskrypcji mają te same ustawienia miejsca do magazynowania diagnostyki, a w przypadku konfiguracji zmienione (diagnostyki włączone/wyłączone lub zmienianie konta miejsca do magazynowania) dotyczy wszystkich pamięci podręcznej w subskrypcji, które znajdują się w danym regionie. Jeśli zmieniono ustawienia diagnostyki pamięci podręcznej, sprawdź, jeśli zmieniono ustawień diagnostycznych dla innego pamięci podręcznej w tej samej subskrypcji i regionu. Jednym ze sposobów sprawdzenia jest wyświetlanie dzienników inspekcji dla pamięć podręczną dla `Write DiagnosticSettings` zdarzenia. Aby uzyskać więcej informacji o pracy z dzienników inspekcji Zobacz [Wyświetlanie zdarzeń i dzienników inspekcji](../monitoring-and-diagnostics/insights-debugging-with-events.md) i [Inspekcja operacji przy użyciu Menedżera zasobów](../resource-group-audit.md). Aby uzyskać więcej informacji dotyczących monitorowania zdarzenia Azure Redis w pamięci podręcznej zobacz [Operacje i alerty](cache-how-to-monitor.md#operations-and-alerts).

### <a name="why-is-diagnostics-enabled-for-some-new-caches-but-not-others"></a>Dlaczego diagnostyki jest włączone dla niektórych nowych pamięci podręcznej, ale nie inne?

Pamięć podręczną w ustawieniach regionalnych i subskrypcji mają te same ustawienia miejsca do magazynowania diagnostyki. Czy można utworzyć nową pamięć podręczną w ustawieniach regionalnych i subskrypcji jako innej pamięci podręcznej, które ma diagnostyki włączone, diagnostyki jest włączony na nową pamięć podręczną, przy użyciu tych samych ustawień.


<a name="cache-timeouts"></a>
### <a name="why-am-i-seeing-timeouts"></a>Dlaczego widzę przekroczenia limitu czasu?

Limity czasu programów klienta, którego używasz, aby porozmawiać z Redis. W większości przypadków serwer Redis wykonuje nie limitu czasu. Gdy polecenia jest wysyłana do serwera Redis, polecenie zostanie umieszczone w kolejce i serwera Redis po pewnym czasie wybiera polecenie i go uruchomi. Jednak klienta można limit czasu w trakcie tego procesu i jeśli nie wyjątków jest uruchamiany po stronie połączeń. Aby uzyskać więcej informacji na temat rozwiązywania problemów przekroczenia limitu czasu zobacz [Rozwiązywanie problemów po stronie klienta](cache-how-to-troubleshoot.md#client-side-troubleshooting) i [StackExchange.Redis limit czasu wyjątki](cache-how-to-troubleshoot.md#stackexchangeredis-timeout-exceptions).


<a name="cache-disconnect"></a>
### <a name="why-was-my-client-disconnected-from-the-cache"></a>Dlaczego moje klienta został odłączony z pamięci podręcznej

Poniżej przedstawiono niektóre typowe przyczyny rozłączenia pamięci podręcznej.

-   Powoduje, że po stronie klienta
    -   Z aplikacją kliencką została ponownego rozmieszczania serwera.
    -   Z aplikacją kliencką wykonać operację skalowania.
        -   W przypadku usług w chmurze lub aplikacji sieci Web może to być spowodowane automatyczne skalowanie.
    -   Zmienić warstwy sieci po stronie klienta.
    -   Wystąpiły błędy przejściowych w kliencie lub węzły sieci między klientem a serwerem.
    -   Ograniczenia przepustowości próg były dostępne.
    -   Procesor powiązana operacje trwała zbyt długo.
-   Powoduje, że po stronie serwera
    -   Na standardowy pamięci podręcznej, oferowanie usługi Azure Redis w pamięci podręcznej zainicjowane przełączaniem z podstawowego węzła do węzła pomocniczą.
    -   Azure została poprawki wystąpienia, w której został wdrożony pamięci podręcznej
        -   Może to być aktualizacji serwera Redis lub ogólne konserwacji maszyn wirtualnych.







### <a name="which-azure-cache-offering-is-right-for-me"></a>Które oferująca pamięci podręcznej Azure jest dla mnie odpowiednia?

>[AZURE.IMPORTANT]Jak ostatni rok [anons](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)usługa Azure zarządzania usługą pamięci podręcznej i pamięci podręcznej w roli Azure będzie można usunąć na 30 listopada 2016. Nasze zaleca się użycie [Azure Redis w pamięci podręcznej](https://azure.microsoft.com/services/cache/). Aby uzyskać informacje na temat przeprowadzania migracji zobacz [Migrowanie z usługi Azure Redis w pamięci podręcznej zarządzanych pamięci podręcznej](cache-migrate-to-redis.md).

### <a name="azure-redis-cache"></a>Azure Redis pamięci podręcznej
Azure Redis pamięci podręcznej są powszechnie dostępny w o rozmiarze do 53 GB i ma dostępność SLA 99,9%. Nowy [poziom premium](cache-premium-tier-intro.md) oferuje do rozmiarów 530 GB i obsługę klaster VNET i utrzymywanie z SLA 99,9%.

Azure Redis pamięci podręcznej zapewnia klientom możliwość używania funkcji bezpieczne, dedykowane Redis pamięci podręcznej, zarządzane przez firmę Microsoft. Z tej oferty możesz wyświetlić korzystać z zaawansowanych funkcji zestawu i ekosystemu dostarczony przez Redis, zaufanego hostingu i monitorowanie od firmy Microsoft.

W przeciwieństwie do tradycyjnych pamięci podręcznej, które dotyczą tylko z par klucz wartość Redis jest popularne jego wysoce performant typów danych. Redis również obsługuje długotrwałych Atomowej operacji dotyczących tych typów, takie jak dołączanie do ciąg. zwiększanie wartość skrótu; Naciśnięcie na listę; Obliczanie przecięcia zestawu, Unii i różnica; lub wprowadzenie członka o najwyższej klasyfikacji w zestawie posortowany. Inne funkcje obejmują obsługę transakcje, pub-sub, Lua skryptów kluczy z ograniczony czas wygaśnięcia i ustawienia konfiguracji nawiązać Redis zachowują się podobnie tradycyjnych pamięci podręcznej.

Innym aspektem klucza efektywnego Redis jest ekosystemie prawidłowy, dynamiczne Otwórz źródło wbudowany wokół niego. To jest widoczny w różnych grupy klientów Redis dostępne w wielu językach. Dzięki temu mają być używane przez niemal dowolnym obciążenie pracą, którego chcesz utworzyć umieszczona Azure. 

Aby uzyskać więcej informacji na temat rozpoczynania pracy z Azure Redis w pamięci podręcznej zobacz [sposób użycia Azure Redis w pamięci podręcznej](cache-dotnet-how-to-use-azure-redis-cache.md) i [dokumentacji Azure Redis w pamięci podręcznej](https://azure.microsoft.com/documentation/services/redis-cache/).

### <a name="managed-cache-service"></a>Usługa zarządzanych pamięci podręcznej
[Zarządza usługa jest ustawiona na być wycofana 30 listopada 2016 pamięci podręcznej.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)

### <a name="in-role-cache"></a>Pamięć podręczną w roli
[Pamięć podręczną w roli ustawiono być wycofana 30 listopada 2016.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)





[Ustawienie konfiguracji "minIoThreads"]: https://msdn.microsoft.com/library/vstudio/7w2sway1(v=vs.100).aspx
