<properties 
    pageTitle="Jak używać pamięci podręcznej Azure Redis | Microsoft Azure" 
    description="Dowiedz się, jak zwiększyć wydajność Azure aplikacji przy użyciu Azure Redis w pamięci podręcznej" 
    services="redis-cache,app-service" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="dotnet" 
    ms.topic="hero-article" 
    ms.date="08/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache"></a>Jak używać Azure Redis pamięci podręcznej

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [PROGRAMU ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Ten przewodnik pokazano, jak rozpocząć korzystanie z **Platformy Azure Redis w pamięci podręcznej**. Pamięć podręczna Microsoft Azure Redis jest oparty na popularne Otwórz źródło Redis pamięci podręcznej. Daje dostęp do bezpiecznego, dedykowane Redis pamięci podręcznej, zarządzane przez firmę Microsoft. Pamięci podręcznej utworzony przy użyciu Azure Redis w pamięci podręcznej jest dostępna z dowolnej aplikacji w programie Microsoft Azure.

Pamięć podręczna Microsoft Azure Redis jest dostępny w następujących poziomów:

-   **Podstawowe** — jeden węzeł. Wielokrotności o rozmiarze do 53 GB.
-   **Standardowe** — dwóch węzłach podstawowa replice. Wielokrotności o rozmiarze do 53 GB. 99,9 UMOWA DOTYCZĄCA POZIOMU USŁUG %.
-   **Premium** — dwóch węzłach podstawowa-replice z odłamki maksymalnie 10. Różnych rozmiarach z 6 GB do 530 GB (kontakt z nami dłużej). Wszystkie funkcje standardowego warstwy i więcej obsługę tym [Redis klaster](cache-how-to-premium-clustering.md), [Redis trwałe](cache-how-to-premium-persistence.md)i [Azure wirtualnej sieci](cache-how-to-premium-vnet.md). 99,9 UMOWA DOTYCZĄCA POZIOMU USŁUG %.

Każdą różni się pod względem funkcji i cennik. Aby uzyskać informacje dotyczące cen zobacz [Szczegóły ceny pamięci podręcznej][].

Ten przewodnik pokazano, jak za pomocą klienta usługi [StackExchange.Redis][] za pomocą C\# kodu. Scenariusze, w których obejmują **Tworzenie i konfigurowanie pamięci podręcznej**, **Konfigurowanie klientów pamięci podręcznej**i **Dodawanie i usuwanie obiektów z pamięci podręcznej**. Aby uzyskać więcej informacji na temat korzystania z platformy Azure Redis w pamięci podręcznej zajrzyj do sekcji [Następne kroki][] . Samouczek krok po kroku tworzenia MVC ASP.NET aplikacji sieci web z Redis pamięci podręcznej zobacz [Tworzenie aplikacji sieci Web z Redis pamięci podręcznej](cache-web-app-howto.md).

<a name="getting-started-cache-service"></a>
## <a name="get-started-with-azure-redis-cache"></a>Rozpoczynanie pracy z pamięci podręcznej Azure Redis

Wprowadzenie do programu Azure Redis w pamięci podręcznej jest łatwe. Aby rozpocząć pracę, obsługi administracyjnej i konfigurowanie pamięci podręcznej. Następnie skonfiguruj możesz klientów pamięci podręcznej, więc można uzyskać dostęp do pamięci podręcznej. Skonfigurowane klientów pamięci podręcznej można rozpocząć pracę z nimi.

-   [Tworzenie pamięci podręcznej][]
-   [Konfigurowanie klientów pamięci podręcznej][]

<a name="create-cache"></a>
## <a name="create-a-cache"></a>Tworzenie pamięci podręcznej

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

### <a name="to-access-your-cache-after-its-created"></a>Aby uzyskać dostęp do zawartości pamięci podręcznej po jego utworzeniu

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Aby uzyskać więcej informacji o konfigurowaniu pamięć podręczną zobacz [jak skonfigurować Azure Redis w pamięci podręcznej](cache-configure.md).

<a name="NuGet"></a>
## <a name="configure-the-cache-clients"></a>Konfigurowanie klientów pamięci podręcznej

[AZURE.INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

Po skonfigurowaniu projektu klienta w pamięci podręcznej, można użyć techniki opisane w poniższych sekcjach dotyczące pracy z pamięci podręcznej.

<a name="working-with-caches"></a>
## <a name="working-with-caches"></a>Praca z pamięci podręcznej

W tej sekcji opisano sposób wykonywania typowych zadań z pamięci podręcznej.

-   [Nawiązywanie połączenia z pamięci podręcznej][]
-   [Dodawanie i pobrać z pamięci podręcznej obiektów][]
-   [Praca z obiektami .NET w pamięci podręcznej](#work-with-net-objects-in-the-cache)

<a name="connect-to-cache"></a>
## <a name="connect-to-the-cache"></a>Nawiązywanie połączenia z pamięci podręcznej

Aby można było programowy pracować z pamięci podręcznej, potrzebne odwołanie do pamięci podręcznej. Dodaj następujący tekst na początku dowolnego pliku, z którego chcesz uzyskać dostęp do pamięci podręcznej Azure Redis za pomocą klienta usługi StackExchange.Redis.

    using StackExchange.Redis;

>[AZURE.NOTE] Klient StackExchange.Redis wymaga .NET Framework 4 lub nowszym.

Połączenie z pamięci podręcznej Redis Azure jest zarządzane przez `ConnectionMultiplexer` zajęć. Ta klasa jest przeznaczona do udostępnionego i ponownie wykorzystywać całej aplikacji klienta i nie muszą zostać utworzone na podstawie operacji na. 

Nawiązać połączenie z pamięci podręcznej Azure Redis i zwracaną wystąpienie połączonego `ConnectionMultiplexer`, połączenie statycznego `Connect` metody i przekazania w pamięci podręcznej punktu końcowego i klucza, takich jak poniższym przykładzie. Użyj klawisza wynikiem Portal Azure jako parametr hasła.

    ConnectionMultiplexer connection = ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");

>[AZURE.IMPORTANT] Ostrzeżenie: Nigdy nie magazynu poświadczeń w kodzie źródłowym. Aby zachować w tym przykładzie prosty, I jest wyświetlania ich w kodu źródłowego. Aby uzyskać informacje na temat przechowywania poświadczeń, zobacz [jak ciągi aplikacji i pracy ciągów połączenia][] .

Jeśli nie chcesz używać protokołu SSL, albo ustawić `ssl=false` lub pominąć `ssl` parametru.

>[AZURE.NOTE] Port SSL nie jest domyślnie wyłączone dla nowej pamięci podręcznej. Aby uzyskać instrukcje dotyczące włączania portu bez użycia protokołu SSL zobacz [Porty dostępu](cache-configure.md#access-ports).

Jeden ze sposobów udostępniania `ConnectionMultiplexer` wystąpienie w aplikacji ma statyczny właściwość, która zwraca wystąpienie połączonego podobny do następującego przykładu. Dzięki temu wątku bezpieczne do zainicjowania tylko jeden połączony `ConnectionMultiplexer` wystąpienie. W tych przykładach `abortConnect` jest ustawiona na wartość FAŁSZ, co oznacza, że połączenie powiedzie się, nawet jeśli nie nawiązano połączenia z pamięci podręcznej Redis Azure. Jedną z najważniejszych funkcji programu `ConnectionMultiplexer` zostaną automatycznie przywrócić łączność w pamięci podręcznej raz problemu sieć lub inne przyczyny są rozwiązany.

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

Aby uzyskać więcej informacji na temat opcji konfiguracji Zaawansowane połączenie zobacz [model konfiguracji StackExchange.Redis][].

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

Po nawiązaniu połączenia zwraca odwołanie do bazy danych z pamięci podręcznej redis, dzwoniąc `ConnectionMultiplexer.GetDatabase` metody. Obiekt zwrócony przez `GetDatabase` metoda jest obiektem przekazująca najprostsze i nie musi być przechowywane.

    // Connection refers to a property that returns a ConnectionMultiplexer
    // as shown in the previous example.
    IDatabase cache = Connection.GetDatabase();

    // Perform cache operations using the cache object...
    // Simple put of integral data types into the cache
    cache.StringSet("key1", "value");
    cache.StringSet("key2", 25);

    // Simple get of data types from the cache
    string key1 = cache.StringGet("key1");
    int key2 = (int)cache.StringGet("key2");

Teraz, gdy wiesz, jak połączyć się z wystąpieniem Azure Redis w pamięci podręcznej i zwraca odwołanie do bazy danych pamięci podręcznej, Spójrzmy w pracy z pamięci podręcznej.

<a name="add-object"></a>
## <a name="add-and-retrieve-objects-from-the-cache"></a>Dodawanie i pobrać z pamięci podręcznej obiektów

Elementy mogą być przechowywane w, a pobrane z pamięci podręcznej za pomocą `StringSet` i `StringGet` metod.

    // If key1 exists, it is overwritten.
    cache.StringSet("key1", "value1");

    string value = cache.StringGet("key1");

Redis sklepy większość danych jako ciągi Redis, ale tych ciągów może zawierać wiele typów danych, w tym seryjnych dane binarne, które mogą być używane podczas przechowywania .NET obiektów w pamięci podręcznej.

Podczas połączenia `StringGet`, jeśli istnieje obiekt, jest zwracana, a jeśli nie, `null` jest zwracana. W takim przypadku możesz pobrać wartości ze źródła danych odpowiednie i zapisać go w pamięci podręcznej do późniejszego użycia. Jest to określane jako deseń odłogowania pamięci podręcznej.

    string value = cache.StringGet("key1");
    if (value == null)
    {
        // The item keyed by "key1" is not in the cache. Obtain
        // it from the desired data source and add it to the cache.
        value = GetValueFromDataSource();

        cache.StringSet("key1", value);
    }

Aby określić ważności elementu w pamięci podręcznej, użyj `TimeSpan` parametr `StringSet`.

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

## <a name="work-with-net-objects-in-the-cache"></a>Praca z obiektami .NET w pamięci podręcznej

Azure Redis pamięci podręcznej może buforować obiekty .NET, a także podstawowych typów danych, ale przed obiektu .NET mogą być buforowane musi być seryjnych. Spoczywa Deweloper aplikacji i zapewnia elastyczność Deweloper przy wyborze serializatora.

Najprostszy sposób szeregować obiektów jest użycie `JsonConvert` metod szeregowania w [Newtonsoft.Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/8.0.1-beta1) i szeregować do i z JSON. W poniższym przykładzie pokazano get i zestaw przy użyciu `Employee` wystąpienie obiektu.


    class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }
    
        public Employee(int EmployeeId, string Name)
        {
            this.Id = EmployeeId;
            this.Name = Name;
        }
    }

    // Store to cache
    cache.StringSet("e25", JsonConvert.SerializeObject(new Employee(25, "Clayton Gragg")));

    // Retrieve from cache
    Employee e25 = JsonConvert.DeserializeObject<Employee>(cache.StringGet("e25"));

<a name="next-steps"></a>
## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy, skorzystaj z poniższych łączy, aby dowiedzieć się więcej na temat Azure Redis w pamięci podręcznej.

-   Zapoznaj się z dostawcami ASP.NET Azure Redis w pamięci podręcznej.
    -   [Stan sesji Redis Azure dostawcy](cache-aspnet-session-state-provider.md)
    -   [Azure Redis pamięci podręcznej wyjścia w środowisku ASP.NET pamięci podręcznej dostawcy](cache-aspnet-output-cache-provider.md)
-   [Włączanie diagnostyki pamięci podręcznej](cache-how-to-monitor.md#enable-cache-diagnostics) , dzięki czemu można [Monitorowanie](cache-how-to-monitor.md) kondycji pamięci podręcznej. Można wyświetlić metryki Azure Portal i można [pobrać i przeglądanie](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) za pomocą narzędzia wyboru.
-   Zapoznaj się z [StackExchange.Redis pamięci podręcznej klienta dokumentacji][].
    -   Azure Redis pamięci podręcznej są dostępne z wielu klientów Redis i języki projektowania. Aby uzyskać więcej informacji zobacz [http://redis.io/clients][].
-   Azure Redis pamięci podręcznej można również za pomocą narzędzi, takich jak Redsmin i Redis Desktop Manager i usług innych firm.
    -   Aby uzyskać więcej informacji o Redsmin zobacz [Pobieranie parametrów połączenia Azure Redis i używania go Redsmin][].
    -   Dostęp i sprawdź dane w pamięci podręcznej Redis Azure przy użyciu graficznego interfejsu użytkownika przy użyciu [RedisDesktopManager](https://github.com/uglide/RedisDesktopManager).
-   Zapoznaj się z dokumentacją [redis][] i informacje o [redis typów danych][] i [wprowadzenie 15 minut do Redis typy danych][].



<!-- INTRA-TOPIC LINKS -->
[Następne kroki]: #next-steps
[Introduction to Azure Redis Cache (Video)]: #video
[What is Azure Redis Cache?]: #what-is
[Create an Azure Cache]: #create-cache
[Which type of caching is right for me?]: #choosing-cache
[Prepare Your Visual Studio Project to Use Azure Caching]: #prepare-vs
[Configure Your Application to Use Caching]: #configure-app
[Get Started with Azure Redis Cache]: #getting-started-cache-service
[Tworzenie pamięci podręcznej]: #create-cache
[Configure the cache]: #enable-caching
[Konfigurowanie klientów pamięci podręcznej]: #NuGet
[Working with Caches]: #working-with-caches
[Nawiązywanie połączenia z pamięci podręcznej]: #connect-to-cache
[Dodawanie i pobrać z pamięci podręcznej obiektów]: #add-object
[Specify the expiration of an object in the cache]: #specify-expiration
[Store ASP.NET session state in the cache]: #store-session

  
<!-- IMAGES -->


[StackExchangeNuget]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-stackexchange-redis.png

[NuGetMenu]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-nuget-menu.png

[CacheProperties]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-properties.png

[ManageKeys]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-keys.png

[SessionStateNuGet]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-session-state-provider.png

[BrowseCaches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-browse-caches.png

[Caches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-caches.png






   
<!-- LINKS -->
[http://redis.IO/clients]: http://redis.io/clients
[Develop in other languages for Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn690470.aspx
[Jak pobrać parametry połączenia Azure Redis i używać go z Redsmin]: https://redsmin.uservoice.com/knowledgebase/articles/485711-how-to-connect-redsmin-to-azure-redis-cache
[Azure Redis Session State Provider]: http://go.microsoft.com/fwlink/?LinkId=398249
[How to: Configure a Cache Client Programmatically]: http://msdn.microsoft.com/library/windowsazure/gg618003.aspx
[Session State Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320835
[Azure AppFabric Cache: Caching Session State]: http://www.microsoft.com/showcase/details.aspx?uuid=87c833e9-97a9-42b2-8bb1-7601f9b5ca20
[Output Cache Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320837
[Azure Shared Caching]: http://msdn.microsoft.com/library/windowsazure/gg278356.aspx
[Team Blog]: http://blogs.msdn.com/b/windowsazure/
[Azure Caching]: http://www.microsoft.com/showcase/Search.aspx?phrase=azure+caching
[How to Configure Virtual Machine Sizes]: http://go.microsoft.com/fwlink/?LinkId=164387
[Azure Caching Capacity Planning Considerations]: http://go.microsoft.com/fwlink/?LinkId=320167
[Azure Caching]: http://go.microsoft.com/fwlink/?LinkId=252658
[How to: Set the Cacheability of an ASP.NET Page Declaratively]: http://msdn.microsoft.com/library/zd1ysf1y.aspx
[How to: Set a Page's Cacheability Programmatically]: http://msdn.microsoft.com/library/z852zf6b.aspx
[Configure a cache in Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn793612.aspx

[Model konfiguracji StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md

[Work with .NET objects in the cache]: http://msdn.microsoft.com/library/dn690521.aspx#Objects


[NuGet Package Manager Installation]: http://go.microsoft.com/fwlink/?LinkId=240311
[Pamięć podręczna szczegółowe informacje o cenach]: http://www.windowsazure.com/pricing/details/cache/
[Azure Portal]: https://portal.azure.com/

[Overview of Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=320830
[Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=398247

[Migrate to Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=317347
[Azure Redis Cache Samples]: http://go.microsoft.com/fwlink/?LinkId=320840
[Using Resource groups to manage your Azure resources]: ../azure-resource-manager/resource-group-overview.md

[StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis
[StackExchange.Redis pamięci podręcznej klienta dokumentacji]: http://github.com/StackExchange/StackExchange.Redis#documentation

[Redis]: http://redis.io/documentation
[Redis typy danych]: http://redis.io/topics/data-types
[wprowadzenie 15 minut do typów danych Redis]: http://redis.io/topics/data-types-intro

[Jak działają ciągi aplikacji i parametry połączenia]: http://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/


