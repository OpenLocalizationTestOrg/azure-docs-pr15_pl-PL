<properties 
    pageTitle="Migrowanie pamięci podręcznej do Redis | Microsoft Azure"
    description="Dowiedz się, jak przeprowadzić migrację aplikacji usługi zarządzanych pamięci podręcznej Azure Redis w pamięci podręcznej"
    services="redis-cache"
    documentationCenter="na"
    authors="steved0x"
    manager="douge"
    editor="tysonn" />
<tags 
    ms.service="cache"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="09/30/2016"
    ms.author="sdanie" />

# <a name="migrate-from-managed-cache-service-to-azure-redis-cache"></a>Migrowanie z pamięci podręcznej zarządzane usługi Azure Redis pamięci podręcznej

Migrowanie aplikacji korzystających z usługi zarządzanych pamięci podręcznej Azure Azure Redis w pamięci podręcznej można wykonywać z minimalnymi zmian w aplikacji, w zależności od funkcji usługi zarządzanych pamięci podręcznej w pamięci podręcznej aplikacji. Gdy za pośrednictwem interfejsów API nie są dokładnie tak samo są podobne, a duża część istniejący kod korzystającego usługi zarządzanych pamięci podręcznej, aby uzyskać dostęp do pamięci podręcznej może nastąpić z minimalnymi zmiany. W tym temacie pokazano, jak wprowadzić zmiany aplikacji do migrowania aplikacji usługi zarządzanych pamięci podręcznej umożliwia Azure Redis w pamięci podręcznej i niezbędne konfiguracji i pokazuje, jak niektóre funkcje Azure Redis w pamięci podręcznej używane do implementowania pamięci podręcznej usługi zarządzanych pamięci podręcznej.

## <a name="migration-steps"></a>Kroków migracji

Poniższe czynności są wymagane do migrowania aplikację usługi zarządzanych pamięci podręcznej umożliwia Azure Redis w pamięci podręcznej.

-   Mapowanie funkcje usługi zarządzanych pamięci podręcznej Azure Redis w pamięci podręcznej
-   Wybierz pozycję oferująca pamięci podręcznej
-   Tworzenie pamięci podręcznej
-   Konfigurowanie klientów pamięci podręcznej
    -   Usuwanie konfiguracji usługi zarządzanych pamięci podręcznej
    -   Konfigurowanie pamięci podręcznej klienta przy użyciu pakietu NuGet StackExchange.Redis
-   Migrowanie kod zarządzany usługi pamięci podręcznej
    -   Nawiązywanie połączenia z pamięci podręcznej, za pomocą klasy ConnectionMultiplexer
    -   Dostęp podstawowych typów danych w pamięci podręcznej
    -   Praca z obiektami .NET w pamięci podręcznej
-   Migrowanie stanu sesji ASP.NET i buforowania Azure Redis w pamięci podręcznej stron wyjściowych 

## <a name="map-managed-cache-service-features-to-azure-redis-cache"></a>Mapowanie funkcje usługi zarządzanych pamięci podręcznej Azure Redis w pamięci podręcznej

Azure usługi zarządzanych pamięci podręcznej i Azure Redis pamięci podręcznej są podobne, ale wykonania niektórych ich funkcji na różne sposoby. W tej sekcji opisano niektóre różnice i zawiera wskazówki dotyczące implementacji funkcji usługi zarządzanych pamięci podręcznej w pamięci podręcznej Azure Redis.

|Funkcja pamięci podręcznej usługi zarządzanych|Obsługa usługi pamięci podręcznej zarządzanych|Obsługa Azure Redis pamięci podręcznej|
|---|---|---|
|Nazwana pamięci podręcznej|Domyślne pamięci podręcznej są skonfigurowane tak, a w pamięci podręcznej Standard i Premium można skonfigurować ofertą maksymalnie 9 dodatkowe o nazwie pamięć podręczną w razie potrzeby.|Azure Redis pamięci podręcznej mają można konfigurować liczba baz danych (domyślnie 16), które mogą być używane do wykonania podobne funkcje do nazwanych pamięci podręcznej. Aby uzyskać więcej informacji zobacz [domyślne Redis konfiguracji serwera](cache-configure.md#default-redis-server-configuration).|
|Wysoka dostępność|Zapewnia wysoką dostępność dla elementów w pamięci podręcznej w ofertach pamięci podręcznej Standard i Premium. W przypadku utraty ze względu na błąd elementów kopie zapasowe elementów w pamięci podręcznej są nadal dostępne. Zapisy do pomocniczej pamięci podręcznej, są wykonywane synchronicznie.|Wysoka dostępność jest dostępna w Standard i Premium pamięci podręcznej ofert, których konfiguracji podstawowej-replice dwóch węzeł (para podstawowa replice ma każdego shard w pamięci podręcznej Premium). Zapisy replice są wykonywane asynchroniczne. Aby uzyskać więcej informacji zobacz [ceny Azure Redis w pamięci podręcznej](https://azure.microsoft.com/pricing/details/cache/).|
|Powiadomienia|Umożliwia klientom otrzymywanie powiadomień o asynchroniczne różnych operacji pamięci podręcznej występują w nazwanym pamięci podręcznej.|Aplikacje klienckie umożliwia Redis pub-sub lub [powiadomienia Keyspace](cache-configure.md#keyspace-notifications-advanced-settings) osiągnięcia podobną funkcję powiadomień.|
|Lokalnej pamięci podręcznej|Przechowywana kopia pamięci podręcznej obiektów lokalnie na komputerze klienckim dla bardzo szybki dostęp.|Aplikacje klienckie należy zaimplementować tę funkcję przy użyciu słownika lub podobne struktury danych.|
|Zasady eksmisji zwiększany|Brak lub LRU. Zasady domyślne są LRU.|Azure Redis pamięci podręcznej obsługuje następujących zasad eksmisji zwiększany: lru lotnych, allkeys lru, losowe lotnych, allkeys losowe ttl lotnych, noeviction. Zasady domyślne są volatile lru. Aby uzyskać więcej informacji zobacz [domyślne Redis konfiguracji serwera](cache-configure.md#default-redis-server-configuration).|
|Zasady wygasania|Domyślną zasadę wygaśnięcia jest bezwzględne i domyślny interwał wygasania wynosi dziesięć minut. Przesuwanie i nigdy zasady są również dostępne.|Domyślnie nie wygasają elementów w pamięci podręcznej, ale wygaśnięcia można skonfigurować dla zapisu na przy użyciu overloads zestawu pamięci podręcznej. Aby uzyskać więcej informacji zobacz [Dodawanie i Pobierz obiektów z pamięci podręcznej](cache-dotnet-how-to-use-azure-redis-cache.md#add-and-retrieve-objects-from-the-cache).|
|Regionów i znakowanie|Regiony są podgrup dla elementów pamięci podręcznej. Regiony obsługują także adnotacji pamięci podręcznej elementów z dodatkowych ciągów opisowy o nazwie znaczniki. Regiony obsługuje możliwość wykonywania operacji wyszukiwania na wszystkie elementy oznakowane w danym regionie. Wszystkie elementy w obszarze znajdują się w jednym węźle klaster pamięci podręcznej.|Pamięci podręcznej Redis składa się z jednego węzła (chyba że klaster Redis jest włączona), nie ma zastosowania koncepcji usługi zarządzanych pamięć podręczną regionów. Podczas pobierania klawiszy, aby opisowe tagi można osadzone w nazw kluczy i używana do pobierania elementów później redis operacji symboli i obsługa wyszukiwania. Na przykład stosowania znakowania rozwiązanie przy użyciu Redis zobacz [implementacji pamięci podręcznej znakowania z Redis](http://stackify.com/implementing-cache-tagging-redis/).|
|Szeregowania|Zarządzane pamięci podręcznej obsługuje NetDataContractSerializer, elementu i stosowania serializatorów niestandardowych obiektów. Wartość domyślna to NetDataContractSerializer.|Jest odpowiedzialny za aplikacji klienckiej szeregować obiekty .NET przed umieszczanie ich w pamięci podręcznej, wybrać serializatora maksymalnie Deweloper aplikacji klienta. Aby uzyskać więcej informacji i przykładowy kod zobacz [Praca z obiektami .NET w pamięci podręcznej](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).|
| Emulator pamięci podręcznej | Zarządzane pamięci podręcznej udostępnia emulatora lokalnej pamięci podręcznej. | Azure Redis pamięci podręcznej nie ma emulatora, ale możesz [uruchomić kompilację MSOpenTech redis-server.exe lokalnie](cache-faq.md#cache-emulator) , aby zapewnić obsługi emulatora. |

## <a name="choose-a-cache-offering"></a>Wybierz pozycję oferująca pamięci podręcznej

Pamięć podręczna Microsoft Azure Redis jest dostępny w następujących poziomów:

-   **Podstawowe** — jeden węzeł. Wielokrotności o rozmiarze do 53 GB.
-   **Standardowe** — dwóch węzłach podstawowa replice. Wielokrotności o rozmiarze do 53 GB. 99,9 UMOWA DOTYCZĄCA POZIOMU USŁUG %.
-   **Premium** — dwóch węzłach podstawowa-replice z odłamki maksymalnie 10. Różnych rozmiarach z 6 GB do 530 GB (kontakt z nami dłużej). Wszystkie funkcje standardowego warstwy i więcej obsługę tym [Redis klaster](cache-how-to-premium-clustering.md), [Redis trwałe](cache-how-to-premium-persistence.md)i [Azure wirtualnej sieci](cache-how-to-premium-vnet.md). 99,9 UMOWA DOTYCZĄCA POZIOMU USŁUG %.

Każdą różni się pod względem funkcji i cennik. Funkcje zostały omówione w dalszej części tego przewodnika i uzyskać więcej informacji na temat ceny, zobacz [Szczegóły ceny pamięci podręcznej](https://azure.microsoft.com/pricing/details/cache/).

Rozpoczęcie migracji jest wybierz rozmiar, która odpowiada rozmiarowi zawartości poprzedniej pamięci podręcznej usługi zarządzanych pamięci podręcznej, a następnie skalowanie w górę lub w dół w zależności od potrzeb aplikacji. Więcej porad dotyczących wybierania prawo oferująca Azure Redis w pamięci podręcznej [jakie oferująca Redis pamięci podręcznej i rozmiar należy używać?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="create-a-cache"></a>Tworzenie pamięci podręcznej

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="configure-the-cache-clients"></a>Konfigurowanie klientów pamięci podręcznej

Gdy pamięci podręcznej zostanie utworzona i skonfigurowana, następnym krokiem jest usunięcie Konfiguracja usługi zarządzanych pamięci podręcznej i dodawanie Dodaj konfiguracji Azure Redis w pamięci podręcznej i odwołań, tak aby pamięci podręcznej klienci mogą uzyskać dostęp do pamięci podręcznej.

-   Usuwanie konfiguracji usługi zarządzanych pamięci podręcznej
-   Konfigurowanie pamięci podręcznej klienta przy użyciu pakietu NuGet StackExchange.Redis

### <a name="remove-the-managed-cache-service-configuration"></a>Usuwanie konfiguracji usługi zarządzanych pamięci podręcznej

Przed aplikacje klienckie można skonfigurować dla Azure Redis pamięci podręcznej, istniejącą konfigurację usługi zarządzanych pamięci podręcznej i odwołania do zestawów muszą zostać usunięte przez odinstalowanie pakietu zarządzania NuGet usługi pamięci podręcznej.

Aby odinstalować pakiet zarządzanych NuGet usługi pamięci podręcznej, kliknij prawym przyciskiem myszy projektu klienta w **Eksploratorze rozwiązań** i wybierz pozycję **Zarządzaj NuGet pakietów**. Wybierz węzeł **zainstalowano pakiety** , a następnie wpisz W**indowsAzure.Caching** w polu wyszukiwania zainstalowanych pakietów. Zaznacz **systemu Windows** **Azure pamięci podręcznej** (lub **systemu Windows** **Azure pamięci podręcznej** w zależności od wersję pakietu NuGet), kliknij przycisk **Odinstaluj**, a następnie kliknij przycisk **Zamknij**.

![Odinstalowywanie pakietu NuGet usługi Azure zarządzanych pamięci podręcznej](./media/cache-migrate-to-redis/IC757666.jpg)

Odinstalowywanie pakietu zarządzania NuGet usługi pamięci podręcznej usuwa zespoły usługi zarządzanych pamięci podręcznej i wpisy usługi zarządzanych pamięci podręcznej app.config lub web.config aplikacji klienta. Ponieważ nie można usunąć niektóre ustawienia niestandardowe, po odinstalowaniu pakietu NuGet, otwórz web.config lub app.config i upewnij się, że następujące elementy zostaną całkowicie usunięte.

Upewnij się, że `dataCacheClients` zostaje on usunięty z `configSections` elementu. Nie usuwaj całej `configSections` element; wystarczy usunąć `dataCacheClients` wpis, jeśli jest zainstalowany.

    <configSections>
      <!-- Existing sections omitted for clarity. -->
      <section name="dataCacheClients"type="Microsoft.ApplicationServer.Caching.DataCacheClientsSection, Microsoft.ApplicationServer.Caching.Core" allowLocation="true" allowDefinition="Everywhere"/>
    </configSections>

Upewnij się, że `dataCacheClients` sekcja zostanie usunięty. `dataCacheClients` Sekcja będzie podobny do następującego przykładu.

    <dataCacheClients>
      <dataCacheClientname="default">
        <!--To use the in-role flavor of Azure Cache, set identifier to be the cache cluster role name -->
        <!--To use the Azure Managed Cache Service, set identifier to be the endpoint of the cache cluster -->
        <autoDiscoverisEnabled="true"identifier="[Cache role name or Service Endpoint]"/>

        <!--<localCache isEnabled="true" sync="TimeoutBased" objectCount="100000" ttlValue="300" />-->
        <!--Use this section to specify security settings for connecting to your cache. This section is not required if your cache is hosted on a role that is a part of your cloud service. -->
        <!--<securityProperties mode="Message" sslEnabled="true">
          <messageSecurity authorizationInfo="[Authentication Key]" />
        </securityProperties>-->
      </dataCacheClient>
    </dataCacheClients>

Po konfiguracji usługi zarządzanych pamięci podręcznej zostanie usunięty, można skonfigurować klienta pamięci podręcznej, zgodnie z opisem w poniższej sekcji.

### <a name="configure-a-cache-client-using-the-stackexchangeredis-nuget-package"></a>Konfigurowanie pamięci podręcznej klienta przy użyciu pakietu NuGet StackExchange.Redis

[AZURE.INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

## <a name="migrate-managed-cache-service-code"></a>Migrowanie kod zarządzany usługi pamięci podręcznej

Interfejs API klienta pamięci podręcznej StackExchange.Redis jest podobna do usługi zarządzanych pamięci podręcznej. W tej sekcji omówiono różnic między nimi.

### <a name="connect-to-the-cache-using-the-connectionmultiplexer-class"></a>Nawiązywanie połączenia z pamięci podręcznej, za pomocą klasy ConnectionMultiplexer

W usłudze pamięci podręcznej w zarządzaniu połączenia z pamięci podręcznej były obsługiwane przez `DataCacheFactory` i `DataCache` klasy. W pamięci podręcznej Redis Azure, połączenia te są zarządzane przez `ConnectionMultiplexer` zajęć.

Dodaj następujący za pomocą instrukcji do początku dowolnego pliku, z którego chcesz uzyskać dostęp do pamięci podręcznej.

    using StackExchange.Redis
                                
Jeśli nie rozwiąże ten obszar nazw, upewnij się, że dodano pakietu StackExchange.Redis NuGet zgodnie z opisem w [Konfigurowanie klientów pamięci podręcznej](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

>[AZURE.NOTE] Należy zauważyć, że klient StackExchange.Redis wymaga .NET Framework 4 lub nowszym.

Aby połączyć się z wystąpieniem Azure Redis w pamięci podręcznej, zadzwoń do statycznego `ConnectionMultiplexer.Connect` metody i przebiegu w punktu końcowego i klucza. Jeden ze sposobów udostępniania `ConnectionMultiplexer` wystąpienie w aplikacji ma statyczny właściwość, która zwraca wystąpienie połączonego podobny do następującego przykładu. Dzięki temu wątku bezpieczne do zainicjowania tylko jeden połączony `ConnectionMultiplexer` wystąpienie. W tym przykładzie `abortConnect` jest ustawiona na wartość FAŁSZ, co oznacza, że połączenie powiedzie się, nawet jeśli nie nawiązano połączenia z pamięci podręcznej. Jedną z najważniejszych funkcji programu `ConnectionMultiplexer` zostaną automatycznie przywrócić łączność w pamięci podręcznej raz problemu sieć lub inne przyczyny są rozwiązany.

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

Punkt końcowy pamięci podręcznej, klucze i porty można uzyskać od karta **Podręcznej Redis** wystąpienia pamięci podręcznej. Aby uzyskać więcej informacji zobacz [Właściwości Redis pamięci podręcznej](cache-configure.md#properties).

Po nawiązaniu połączenia zwraca odwołanie do bazy danych z pamięci podręcznej Redis, dzwoniąc `ConnectionMultiplexer.GetDatabase` metody. Obiekt zwrócony przez `GetDatabase` metoda jest obiektem przekazująca najprostsze i nie musi być przechowywane.

    IDatabase cache = Connection.GetDatabase();
    
    // Perform cache operations using the cache object...
    // Simple put of integral data types into the cache
    cache.StringSet("key1", "value");
    cache.StringSet("key2", 25);
    
    // Simple get of data types from the cache
    string key1 = cache.StringGet("key1");
    int key2 = (int)cache.StringGet("key2");

Klient StackExchange.Redis używa `RedisKey` i `RedisValue` typy do uzyskiwania dostępu i przechowywanie elementów w pamięci podręcznej. Następujące typy zamapować na najbardziej pierwotne typy języka, w tym ciągu znaków, a często nie są używane bezpośrednio. Redis ciągi są najbardziej podstawowe rodzaju wartość Redis i mogą zawierać wiele typów danych, w tym seryjnych strumieni binarnych, a gdy nie mogą używać tego typu bezpośrednio, którego użyjesz metod, które zawierają `String` w nazwie. Dla typów danych najbardziej pierwotnych przechowywanie i pobieranie elementów z pamięci podręcznej, za pomocą `StringSet` i `StringGet` metod, chyba że zbiory lub innymi typami danych Redis jest przechowywana w pamięci podręcznej. 

`StringSet`i `StringGet` są bardzo podobne do usługi zarządzanych pamięci podręcznej `Put` i `Get` metod, z jednym główne różnice że przed możesz ustawić i uzyskać obiektu .NET w pamięci podręcznej możesz musi szeregować je najpierw. 

Podczas połączenia `StringGet`, jeśli istnieje obiekt, jest zwracany i jeśli nie jest dostępne, zwracana jest wartość null. W takim przypadku możesz pobrać wartości ze źródła danych odpowiednie i zapisać go w pamięci podręcznej do późniejszego użycia. Jest to określane jako deseń odłogowania pamięci podręcznej.

Aby określić ważności elementu w pamięci podręcznej, użyj `TimeSpan` parametr `StringSet`.

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

Azure Redis pamięci podręcznej można pracować z obiekty .NET, a także podstawowych typów danych, ale przed obiektu .NET mogą być buforowane musi być seryjnych. Jest to odpowiedzialność Deweloper aplikacji. W wyborze serializatora zapewnia elastyczność Deweloper. Aby uzyskać więcej informacji i przykładowy kod zobacz [Praca z obiektami .NET w pamięci podręcznej](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).

## <a name="migrate-aspnet-session-state-and-output-caching-to-azure-redis-cache"></a>Migrowanie stanu sesji ASP.NET i buforowania Azure Redis w pamięci podręcznej stron wyjściowych

Azure Redis pamięci podręcznej ma dostawców dla stanu sesji ASP.NET i wyniki strony pamięci podręcznej. Aby przeprowadzić migrację aplikacji, który korzysta z wersji usługi zarządzanych pamięci podręcznej z tych dostawców, najpierw usuń niezawierającego sekcji z usługi web.config, a następnie skonfiguruj wersji Azure Redis w pamięci podręcznej dostawców. Aby uzyskać instrukcje na temat korzystania z dostawcami Azure Redis pamięci podręcznej ASP.NET zobacz [Dostawcy stan sesji programu ASP.NET Azure Redis w pamięci podręcznej](cache-aspnet-session-state-provider.md) i [ASP.NET dane wyjściowe pamięci podręcznej dostawcy Azure Redis w pamięci podręcznej](cache-aspnet-output-cache-provider.md).

## <a name="next-steps"></a>Następne kroki

Zapoznaj się z [dokumentacją Azure Redis w pamięci podręcznej](https://azure.microsoft.com/documentation/services/cache/) samouczków, próbki, klipów wideo i.

