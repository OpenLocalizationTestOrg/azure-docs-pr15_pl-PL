<properties 
    pageTitle="Azure próbki Redis pamięć podręczną | Microsoft Azure" 
    description="Dowiedz się, jak używać Azure Redis w pamięci podręcznej" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="sdanie"/>

# <a name="azure-redis-cache-samples"></a>Azure próbki Redis pamięci podręcznej 

Ten temat zawiera listę próbki Azure Redis w pamięci podręcznej, scenariusze, takie jak nawiązywanie połączeń z pamięci podręcznej, czytania i pisania dane do i z pamięci podręcznej i za pomocą dostawców ASP.NET Redis w pamięci podręcznej. Przykłady należą do pobrania projektów, a niektóre wskazówki krok po kroku i zawiera fragmenty kodu, ale nie są połączone z projektu do pobrania.

## <a name="hello-world-samples"></a>Witaj świecie próbki

Przykłady w tej sekcji Pokaż podstawowe zagadnienia dotyczące nawiązywania połączenia z wystąpieniem Azure Redis w pamięci podręcznej i czytania i pisania dane w pamięci podręcznej za pomocą różnych języków i Redis klientów.

[Witaj świecie](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) pokazano się, jak wykonywać różne operacje pamięci podręcznej przy użyciu klienta programu .NET [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) .

W tym przykładzie pokazano sposób:

-   Za pomocą różnych opcji połączenia
-   Odczytywanie i zapisywanie obiektów do i z pamięci podręcznej, za pomocą operacji synchroniczne i asynchroniczne
-   Użyj poleceń MGET-MSET Redis celu zwrócenia wartości określonych kluczach
-   Wykonywanie operacji transakcji Redis
-   Praca z listami Redis i posortowany zestawy
-   Przechowywanie obiektów .NET przy użyciu serializatorów JsonConvert obiektów
-   Użyj Redis ustawia zaimplementować znakowanie
-   Praca z klastrem Redis

Aby uzyskać więcej informacji zapoznaj się z dokumentacją [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) na github, a dla scenariuszy zastosowania testy jednostek [StackExchange.Redis.Tests](https://github.com/StackExchange/StackExchange.Redis/tree/master/StackExchange.Redis.Tests) .

[Jak używać Azure Redis pamięci podręcznej Python](cache-python-get-started.md) pokazano, jak rozpocząć pracę z pamięci podręcznej Azure Redis przy użyciu klienta [Korekta redis](https://github.com/andymccurdy/redis-py) i Python.

[Praca z obiektami .NET w pamięci podręcznej](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache) przedstawia sposób szeregować obiekty .NET, aby można było ich do zapisywania i Odczyt wystąpienie Azure Redis w pamięci podręcznej. 

## <a name="use-redis-cache-as-a-scale-out-backplane-for-aspnet-signalr"></a>Używanie Redis pamięci podręcznej jako skalę się tablica połączeń dla SignalR programu ASP.NET

W przykładzie [Za pomocą Redis pamięci podręcznej jako skalę się tablica połączeń dla ASP.NET SignalR](https://github.com/rustd/RedisSamples/tree/master/RedisAsSignalRBackplane) pokazano, korzystaniem Azure Redis w pamięci podręcznej jako tablica SignalR połączeń. Aby uzyskać więcej informacji na temat tablica połączeń zobacz [SignalR Scaleout z Redis](http://www.asp.net/signalr/overview/performance/scaleout-with-redis).

## <a name="redis-cache-customer-query-sample"></a>Redis pamięci podręcznej klienta kwerend próbki

W przykładzie pokazano wydajności porównanie między uzyskiwania dostępu do danych z pamięci podręcznej i uzyskiwanie dostępu do danych z magazynu trwałe. W tym przykładzie zawiera dwa projekty.

-   [Pokaz, jak zwiększyć wydajność przez buforowanie danych Redis pamięci podręcznej](https://github.com/rustd/RedisSamples/tree/master/RedisCacheCustomerQuerySample)
-   [Początkowy bazy danych i pamięci podręcznej dla pokaz](https://github.com/rustd/RedisSamples/tree/master/SeedCacheForCustomerQuerySample)

## <a name="aspnet-session-state-and-output-caching"></a>Stan sesji programu ASP.NET i buforowania stron wyjściowych

[Za pomocą Azure Redis pamięci podręcznej w celu przechowywania ASP.NET SessionState i OutputCache](https://github.com/rustd/RedisSamples/tree/master/SessionState_OutputCaching) przykładzie pokazano, jak użycie Azure Redis w pamięci podręcznej do przechowywania sesji programu ASP.NET i za pomocą dostawców SessionState i OutputCache dla Redis pamięci podręcznej danych wyjściowych.

## <a name="manage-azure-redis-cache-with-maml"></a>Zarządzanie Azure Redis pamięci podręcznej MAML

W [pamięci podręcznej Zarządzanie Redis Azure za pomocą biblioteki zarządzania Azure](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) przykładzie pokazano, jak można używać biblioteki zarządzania Azure Zarządzanie - (Utwórz / Aktualizuj / Usuń) pamięci podręcznej. 

## <a name="custom-monitoring-sample"></a>Niestandardowe monitorowania próbki

Przykładowe [dane programu Access Redis pamięci podręcznej monitorowania](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) pokazano, jak można pobrać dane z monitorowania dla pamięci podręcznej Redis Azure poza Azure Portal.

## <a name="a-twitter-style-clone-written-using-php-and-redis"></a>Klonowanie styl Twitter, napisanym w języku PHP i Redis

Przykładowe [Retwis](https://github.com/SyntaxC4-MSFT/retwis) jest Redis Witaj świecie. Jest napisane przy użyciu Redis i PHP przy użyciu klienta [Predis](https://github.com/nrk/predis) minimalnego klonowanie sieci społecznościowej Twitter styl. Kod źródłowy ma być bardzo proste i w tym samym czasie, aby wyświetlić inny Redis struktury danych.

## <a name="bandwidth-monitor"></a>Monitorowanie przepustowości

Przykładowe [monitor przepustowości](https://github.com/JonCole/SampleCode/tree/master/BandWidthMonitor) umożliwia monitorowanie przepustowość na komputerze klienckim. Zmierz przepustowości, uruchomić przykład na komputerze klienckim, pamięci podręcznej, nawiązywania połączeń z pamięci podręcznej i obserwować przepustowości zgłoszone przez próbkę monitor przepustowości.
