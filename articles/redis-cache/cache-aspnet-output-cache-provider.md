<properties
    pageTitle="Dostawca pamięci podręcznej wyjścia w środowisku ASP.NET pamięci podręcznej"
    description="Dowiedz się, jak pamięci podręcznej wyniki strony ASP.NET przy użyciu Azure Redis w pamięci podręcznej"
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
    ms.date="09/27/2016"
    ms.author="sdanie" />

# <a name="aspnet-output-cache-provider-for-azure-redis-cache"></a>Dostawca pamięci podręcznej wyjścia w środowisku ASP.NET dla Azure Redis pamięci podręcznej

Dostawca Redis pamięci podręcznej danych wyjściowych jest mechanizm out of process miejsca do magazynowania dla danych z pamięci podręcznej danych wyjściowych. Te dane są specjalnie dla pełnego odpowiedzi HTTP (strona buforowania stron wyjściowych). Dostawca podłączyć do nowego dane wyjściowe pamięci podręcznej dostawcy rozszerzeń punkt, który został wprowadzony w ASP.NET 4.

Aby użyć dostawcy Redis pamięci podręcznej danych wyjściowych, najpierw skonfigurować pamięć podręczną, a następnie skonfiguruj aplikację ASP.NET za pomocą pakietu Redis NuGet dostawcy pamięci podręcznej danych wyjściowych. Ten temat zawiera wskazówki na temat konfigurowania aplikacji do używania dostawcy Redis pamięci podręcznej danych wyjściowych. Aby uzyskać więcej informacji na temat tworzenia i konfigurowania wystąpienie Azure Redis w pamięci podręcznej zobacz [Tworzenie pamięci podręcznej](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

## <a name="store-aspnet-page-output-in-the-cache"></a>Przechowywanie wyniki strony ASP.NET w pamięci podręcznej

Aby skonfigurować aplikację klienta w programie Visual Studio przy użyciu pakietu Redis NuGet dostawcy pamięci podręcznej danych wyjściowych, kliknij prawym przyciskiem myszy projektu w **Eksploratorze rozwiązań** i wybierz pozycję **Zarządzaj NuGet pakietów**.

![Pamięć podręczna Azure Redis Zarządzanie pakietów NuGet](./media/cache-aspnet-output-cache-provider/redis-cache-manage-nuget-menu.png)

Wpisz **RedisOutputCacheProvider** w polu tekstowym wyszukiwania, wybierz go z listy wyników, a następnie kliknij przycisk **Zainstaluj**.

![Azure Redis pamięci podręcznej danych wyjściowych pamięci podręcznej dostawcy](./media/cache-aspnet-output-cache-provider/redis-cache-page-output-provider.png)

Pakiet Redis NuGet dostawcy pamięci podręcznej danych wyjściowych ma zależność na opakowaniu StackExchange.Redis.StrongName. Pakiet StackExchange.Redis.StrongName nie znajduje się w projekcie zostanie zainstalowana. Zauważ, że oprócz pakietu StackExchange.Redis.StrongName znaczący o nazwie jest również wersji bez silnych-o nazwach StackExchange.Redis. Jeśli projekt korzysta z wartością znaczący — o nazwach StackExchange.Redis wersji musisz odinstalować, przed lub po zainstalowaniu pakietu Redis NuGet dostawcy pamięci podręcznej danych wyjściowych, w przeciwnym razie można będzie uzyskać konfliktów nazw w projekcie. Aby uzyskać więcej informacji na temat tych pakietów zobacz [Konfigurowanie .NET pamięci podręcznej klientów](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

Pakiet NuGet do pobrania i dodaje odwołania wymaganego zestawu i dodaje następną sekcję do pliku web.config, która zawiera wymagane konfiguracji aplikacji ASP.NET do używania dostawcy Redis pamięci podręcznej danych wyjściowych.

    <caching>
      <outputCachedefault Provider="MyRedisOutputCache">
        <providers>
          <!--
          <add name="MyRedisOutputCache"
            host = "127.0.0.1" [String]
            port = "" [number]
            accessKey = "" [String]
            ssl = "false" [true|false]
            databaseId = "0" [number]
            applicationName = "" [String]
            connectionTimeoutInMilliseconds = "5000" [number]
            operationTimeoutInMilliseconds = "5000" [number]
          />
          -->
          <add name="MyRedisOutputCache" type="Microsoft.Web.Redis.RedisOutputCacheProvider" host="127.0.0.1" accessKey="" ssl="false"/>
        </providers>
      </outputCache>
    </caching>

W sekcji komentarze znajdują się z przykładem atrybuty i ustawienia próbki dla każdego atrybutu.

Konfigurowanie atrybuty z wartościami z Twojej karta pamięci podręcznej w portalu Microsoft Azure i skonfigurować inne wartości w razie potrzeby. Aby uzyskać instrukcje dotyczące uzyskiwania dostępu do właściwości z pamięci podręcznej zobacz [Konfigurowanie Redis ustawienia pamięci podręcznej](cache-configure.md#configure-redis-cache-settings).

-   **host** — Określ punkt końcowy pamięci podręcznej.
-   **port** — Użyj portów port bez użycia protokołu SSL lub ustawienia zabezpieczeń SSL, w zależności od ustawień protokołu ssl.
-   **accessKey** — używanie klucz podstawowy i pomocniczy dla pamięci podręcznej.
-   **ssl** — wartość PRAWDA, jeśli chcesz zabezpieczyć komunikacja pamięci podręcznej i klienta za pomocą protokołu ssl; przeciwnym razie wartość false. Pamiętaj określić poprawny port.
    -   Port SSL nie jest domyślnie wyłączone dla nowej pamięci podręcznej. Określ wartość PRAWDA, to ustawienie korzysta z portu SSL. Aby uzyskać więcej informacji na temat włączania funkcji portu bez użycia protokołu SSL zobacz sekcję [Porty dostępu](cache-configure.md#access-ports) w temacie [Konfigurowanie pamięci podręcznej](cache-configure.md) .
-   **databaseId** — określonego bazę danych, która służących do pamięci podręcznej danych wyjściowych. Jeśli nie zostanie określony, jest używana wartość domyślna 0.
-   **applicationName** — klawiszy są przechowywane w redis jako <AppName>_<SessionId>_Data. Dzięki temu wielu aplikacjom współużytkowanie tego samego klucza. Ten parametr jest opcjonalny i jeśli nie zostanie określona wartość domyślną będzie używany.
-   **connectionTimeoutInMilliseconds** — to ustawienie umożliwia zastąpienie ustawienia connectTimeout w kliencie StackExchange.Redis. Jeśli nie zostanie określony, jest używane domyślne ustawienie connectTimeout 5000. Aby uzyskać więcej informacji zobacz [model konfiguracji StackExchange.Redis](http://go.microsoft.com/fwlink/?LinkId=398705).
-   **operationTimeoutInMilliseconds** — to ustawienie umożliwia zastąpienie ustawienia syncTimeout w kliencie StackExchange.Redis. Jeśli nie zostanie określony, jest używane domyślne ustawienie syncTimeout 1000. Aby uzyskać więcej informacji zobacz [model konfiguracji StackExchange.Redis](http://go.microsoft.com/fwlink/?LinkId=398705).

Dodawanie dyrektywy OutputCache na każdej stronie, do której chcesz pamięci podręcznej danych wyjściowych.

    <%@ OutputCache Duration="60" VaryByParam="*" %>

W tym przykładzie strony pamięci podręcznej danych pozostanie w pamięci podręcznej 60 sekund, a innej wersji strony są buforowane dla każdej kombinacji parametrów. Aby uzyskać więcej informacji na temat dyrektywy OutputCache, zobacz [@OutputCache](http://go.microsoft.com/fwlink/?linkid=320837).

Po wykonywane są następujące kroki, aplikacja jest skonfigurowany do używania dostawcy Redis pamięci podręcznej danych wyjściowych.

## <a name="next-steps"></a>Następne kroki

Zapoznaj się z [Dostawcą stan sesji programu ASP.NET Azure Redis w pamięci podręcznej](cache-aspnet-session-state-provider.md).
