<properties
    pageTitle="Dostawca stanu sesji ASP.NET pamięć podręczną | Microsoft Azure"
    description="Dowiedz się, jak przechowywanie stanu sesji ASP.NET przy użyciu Azure Redis w pamięci podręcznej"
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
    ms.date="09/01/2016"
    ms.author="sdanie" />

# <a name="aspnet-session-state-provider-for-azure-redis-cache"></a>Dostawca stanu sesji ASP.NET dla Azure Redis pamięci podręcznej

Azure Redis pamięci podręcznej zapewnia dostawca stan sesji, który służy do przechowywania swój stan sesji w pamięci podręcznej zamiast w pamięci lub w bazie danych programu SQL Server. Aby użyć dostawcy buforowania stan sesji, najpierw skonfigurować pamięć podręczną, a następnie skonfiguruj aplikację ASP.NET w pamięci podręcznej za pomocą pakietu Redis NuGet stan sesji pamięci podręcznej.

Często nie jest praktyczne w aplikacji dla chmury rzeczywisty unikać przechowywanie niektórych formularz stan sesji użytkownika, ale kilka metod mieć wpływ na wydajność i skalowalność więcej niż inne. Jeśli masz do przechowywania stan, najlepszym rozwiązaniem jest zwiększać ilość stan i zapisać go w pliki cookie. Jeśli, która nie jest możliwe, następnym najlepszym rozwiązaniem jest używanie stanu sesji ASP.NET z dostawcą dla rozłożone, w pamięci podręcznej. Rozwiązanie najgorszego z wydajność i skalowalność punktu widzenia jest korzystanie z bazy danych kopii dostawcy stan sesji. Ten temat zawiera wskazówki na temat korzystania z dostawcy stan sesji programu ASP.NET Azure Redis w pamięci podręcznej. Aby uzyskać informacje na temat innych opcji stan sesji zobacz [Opcje stanu sesji ASP.NET](#aspnet-session-state-options).

## <a name="store-aspnet-session-state-in-the-cache"></a>Przechowywanie stanu sesji ASP.NET w pamięci podręcznej

Aby skonfigurować aplikację klienta w programie Visual Studio przy użyciu pakietu Redis NuGet stan sesji pamięci podręcznej, kliknij prawym przyciskiem myszy projektu w **Eksploratorze rozwiązań** i wybierz pozycję **Zarządzaj NuGet pakietów**.

![Pamięć podręczna Azure Redis Zarządzanie pakietów NuGet](./media/cache-aspnet-session-state-provider/redis-cache-manage-nuget-menu.png)

**RedisSessionStateProvider** należy wpisać w polu tekstowym wyszukiwania, wybierz go z listy wyników, a następnie kliknij przycisk **Zainstaluj**.

>[AZURE.IMPORTANT] Jeśli używasz funkcji klastrów z warstwy premium, należy użyć [RedisSessionStateProvider](https://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider) 2.0.1 lub nowszy albo wyjątku jest generowany. Jest to zmiana podziału; Aby uzyskać więcej informacji, zobacz [v2.0.0 przerywanie Zmień szczegóły](https://github.com/Azure/aspnet-redis-providers/wiki/v2.0.0-Breaking-Change-Details).

![Stan sesji pamięci podręcznej Redis Azure dostawcy](./media/cache-aspnet-session-state-provider/redis-cache-session-state-provider.png)

Pakiet Redis NuGet dostawcy stan sesji ma zależność na opakowaniu StackExchange.Redis.StrongName. Pakiet StackExchange.Redis.StrongName nie znajduje się w projekcie zostanie zainstalowana. Zauważ, że oprócz pakietu StackExchange.Redis.StrongName znaczący o nazwie jest również wersji bez silnych-o nazwach StackExchange.Redis. Jeśli projekt używa bez silnych-o nazwach StackExchange.Redis wersji musisz odinstalować, przed lub po zainstalowaniu pakietu Redis NuGet dostawcy stan sesji, w przeciwnym razie można będzie uzyskać konfliktów nazw w projekcie. Aby uzyskać więcej informacji na temat tych pakietów zobacz [Konfigurowanie .NET pamięci podręcznej klientów](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

Pakiet NuGet do pobrania i dodaje wymaganego zestawu odwołuje się i dodaje następujące dodaje następną sekcję do pliku web.config, która zawiera wymagane konfiguracji aplikacji ASP.NET do używania Redis pamięci podręcznej sesji stan dostawcy.

    <sessionState mode="Custom" customProvider="MySessionStateStore">
        <providers>
        <!--
        <add name="MySessionStateStore"
            host = "127.0.0.1" [String]
            port = "" [number]
            accessKey = "" [String]
            ssl = "false" [true|false]
            throwOnError = "true" [true|false]
            retryTimeoutInMilliseconds = "0" [number]
            databaseId = "0" [number]
            applicationName = "" [String]
            connectionTimeoutInMilliseconds = "5000" [number]
            operationTimeoutInMilliseconds = "5000" [number]
        />
        -->
        <add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false"/>
        </providers>
    </sessionState>

W sekcji komentarze znajdują się z przykładem atrybuty i ustawienia próbki dla każdego atrybutu.

Konfigurowanie atrybuty z wartościami z Twojej karta pamięci podręcznej w portalu Microsoft Azure i skonfigurować inne wartości w razie potrzeby. Aby uzyskać instrukcje dotyczące uzyskiwania dostępu do właściwości z pamięci podręcznej zobacz [Konfigurowanie Redis ustawienia pamięci podręcznej](cache-configure.md#configure-redis-cache-settings).

-   **host** — Określ punkt końcowy pamięci podręcznej.
-   **port** — Użyj portów port bez użycia protokołu SSL lub ustawienia zabezpieczeń SSL, w zależności od ustawień protokołu ssl.
-   **accessKey** — używanie klucz podstawowy i pomocniczy dla pamięci podręcznej.
-   **ssl** — wartość PRAWDA, jeśli chcesz zabezpieczyć komunikacja pamięci podręcznej i klienta za pomocą protokołu ssl; przeciwnym razie wartość false. Pamiętaj określić poprawny port.
    -   Port SSL nie jest domyślnie wyłączone dla nowej pamięci podręcznej. Określ wartość PRAWDA, to ustawienie korzysta z portu SSL. Aby uzyskać więcej informacji na temat włączania funkcji portu bez użycia protokołu SSL zobacz sekcję [Porty dostępu](cache-configure.md#access-ports) w temacie [Konfigurowanie pamięci podręcznej](cache-configure.md) .
-   **throwOnError** — wartość PRAWDA, jeśli wyjątek zostanie wygenerowany w przypadku awarii, lub FAŁSZ, jeśli chcesz, aby operacja kończy się niepowodzeniem w trybie cichym. Możesz sprawdzić dla błąd, zaznaczając pole wyboru statyczna właściwość Microsoft.Web.Redis.RedisSessionStateProvider.LastException. Wartość domyślna to PRAWDA.
-   **retryTimeoutInMilliseconds** — operacji, które nie są wykonana ponownie w tym czasie określony (w milisekundach). Pierwsza próba występuje po 20 milisekund, a następnie wykonywane co sekundę, dopóki nie wygaśnie interwał retryTimeoutInMilliseconds. Natychmiast po tym okresie operacja zostanie wykonana ponownie jeden raz ostatni. Jeśli nadal kończy się niepowodzeniem, wyjątku powrót do rozmówcy, zależnie od ustawienia throwOnError. Wartość domyślna jest równy 0, co oznacza bez ponawiania.
-   **databaseId** — Określa bazę danych, która służących do pamięci podręcznej wyprowadzenia danych. Jeśli nie zostanie określony, jest używana wartość domyślna 0.
-   **applicationName** — klawiszy są przechowywane w redis jako `{<Application Name>_<Session ID>}_Data`. Dzięki temu wielu aplikacjom współużytkowanie tego samego klucza. Ten parametr jest opcjonalny i jeśli nie zostanie określona wartość domyślną będzie używany.
-   **connectionTimeoutInMilliseconds** — to ustawienie umożliwia zastąpienie ustawienia connectTimeout w kliencie StackExchange.Redis. Jeśli nie zostanie określony, jest używane domyślne ustawienie connectTimeout 5000. Aby uzyskać więcej informacji zobacz [model konfiguracji StackExchange.Redis](http://go.microsoft.com/fwlink/?LinkId=398705).
-   **operationTimeoutInMilliseconds** — to ustawienie umożliwia zastąpienie ustawienia syncTimeout w kliencie StackExchange.Redis. Jeśli nie zostanie określony, jest używane domyślne ustawienie syncTimeout 1000. Aby uzyskać więcej informacji zobacz [model konfiguracji StackExchange.Redis](http://go.microsoft.com/fwlink/?LinkId=398705).

Aby uzyskać więcej informacji na temat tych właściwości Zobacz oryginalny wpis w blogu, zawiadomienie o informowanie o ASP.NET sesji stan dla u [Dostawcy Redis](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx).

Nie zapomnij komentarz standardowy InProc sesji stan dostawcy sekcji w swojej web.config.

    <!-- <sessionState mode="InProc"
         customProvider="DefaultSessionProvider">
         <providers>
            <add name="DefaultSessionProvider"
                  type="System.Web.Providers.DefaultSessionStateProvider,
                        System.Web.Providers, Version=1.0.0.0, Culture=neutral,
                        PublicKeyToken=31bf3856ad364e35"
                  connectionStringName="DefaultConnection" />
          </providers>
    </sessionState> -->

Gdy wykonywane są następujące kroki, aplikacja jest skonfigurowany do używania Redis pamięci podręcznej sesji stan dostawcy. Gdy używasz stan sesji w aplikacji będą przechowywane w pamięci podręcznej Azure Redis wystąpienie.

>[AZURE.NOTE] Należy zauważyć, że dane przechowywane w pamięci podręcznej musi być można, w odróżnieniu od dane, które mogą być przechowywane w domyślnym sesji programu ASP.NET w pamięci Województwo dostawcy. Dostawca stan sesji dla Redis jest używana, należy pamiętać, że typów danych, które są przechowywane w stanie sesji są można.

## <a name="aspnet-session-state-options"></a>Opcje stanu sesji ASP.NET

- W polu Dostawca stan sesji pamięci - tego dostawcy przechowuje stan sesji w pamięci. Zaletą za pomocą tego dostawcy jest, że jest proste i szybkie. Nie można jednak skalowanie aplikacji sieci Web programu używasz przez dostawcę pamięci, ponieważ nie jest rozdzielana.

- Dostawca stan sesji programu SQL Server — ten dostawca przechowuje stan sesji w programie Sql Server. Jeśli chcesz utrzymują stan sesji w magazynie trwałych, należy użyć tego dostawcy. Można skalować aplikacji sieci Web, ale przy użyciu programu Sql Server dla sesji będą mieć wpływ na wydajność w aplikacji sieci Web.

- Distributed w pamięci sesji stan dostawca na przykład Redis pamięci podręcznej stan dostawcy sesji - tego dostawcy umożliwia najlepszych stron obu rozwiązań. Aplikacji sieci Web może zawierać proste, szybkie i skalowalna dostawcy stan sesji. Jednak trzeba pamiętać uwagę, że ten dostawca przechowuje stan sesji w pamięci podręcznej i aplikacji musi zostać w uwagę wszystkie właściwości skojarzone podczas rozmówcy Distributed w pamięci podręcznej takich jak awarie sieci. Aby najważniejsze wskazówki na temat korzystania z pamięci podręcznej zobacz [wskazówki buforowanie](../best-practices-caching.md) z Patterns firmy Microsoft i wskazówki dotyczące [implementacji wskazówki i projekt aplikacji chmura Azure](https://github.com/mspnp/azure-guidance).

Aby uzyskać więcej informacji o stanie sesji i innych najważniejszych wskazówek zobacz [Web rozwoju najważniejsze wskazówki (konstrukcyjnych rzeczywisty chmury aplikacje z Azure)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices).

## <a name="next-steps"></a>Następne kroki

Zapoznaj się z [Dostawcą pamięci podręcznej wyjścia ASP.NET Azure Redis w pamięci podręcznej](cache-aspnet-output-cache-provider.md).
