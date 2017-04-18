<properties 
    pageTitle="Stan sesji z Azure Redis pamięci podręcznej w usłudze Azure aplikacji" 
    description="Dowiedz się, jak korzystać z usługi pamięci podręcznej Azure do obsługi pamięci podręcznej stan sesji programu ASP.NET." 
    services="app-service\web" 
    documentationCenter=".net" 
    authors="Rick-Anderson" 
    manager="wpickett" 
    editor="none"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="get-started-article" 
    ms.date="06/27/2016" 
    ms.author="riande"/>


# <a name="session-state-with-azure-redis-cache-in-azure-app-service"></a>Stan sesji z Azure Redis pamięci podręcznej w usłudze Azure aplikacji


W tym temacie wyjaśniono, jak za pomocą usługi Azure pamięci podręcznej Redis dla stanu sesji.

Jeśli aplikacji sieci web programu ASP.NET używa stanu sesji, należy skonfigurować dostawcę stan sesji zewnętrznych (usługi pamięci podręcznej Redis lub dostawcę stan sesji programu SQL Server). Jeśli używasz stan sesji, a nie za pomocą zewnętrznego dostawcy, będzie ograniczona do jednego wystąpienia aplikacji sieci web. Usługa pamięci podręcznej Redis jest najszybszy i najprostszy umożliwiające.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

##<a id="createcache"></a>Tworzenie pamięci podręcznej
Wykonaj [te wskazówki](../cache-dotnet-how-to-use-azure-redis-cache.md#create-cache) utworzyć pamięci podręcznej.

##<a id="configureproject"></a>Dodawanie pakietu RedisSessionStateProvider NuGet do aplikacji sieci web
Instalowanie NuGet `RedisSessionStateProvider` pakiet.  Użyj następującego polecenia, aby zainstalować z konsoli Menedżera pakietu (**Narzędzia** > **Menedżera pakietów NuGet** > **Konsoli Menedżera pakietów**):

  `PM> Install-Package Microsoft.Web.RedisSessionStateProvider`
  
Aby zainstalować z **Narzędzia** > **Menedżera pakietów NuGet** > **Zarządzanie pakietów NugGet rozwiązanie**, wyszukaj `RedisSessionStateProvider`.

Aby uzyskać więcej informacji, zobacz [NuGet RedisSessionStateProvider strony](http://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider/ ) i [Konfigurowanie klienta pamięci podręcznej](../cache-dotnet-how-to-use-azure-redis-cache.md#NuGet).

##<a id="configurewebconfig"></a>Modyfikowanie pliku Web.Config
Oprócz tego, że odwołania do zestawów w pamięci podręcznej, pakietu NuGet dodaje element zastępczy wpisy w pliku *web.config* . 

1. Otwórz *web.config* i Znajdź **sessionState** element.

1. Wpisz wartości dla `host`, `accessKey`, `port` (SSL port powinna być 6380) i ustaw `SSL` do `true`. Te wartości można uzyskać od karta [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715) wystąpienia pamięci podręcznej. Aby uzyskać więcej informacji zobacz [Nawiązywanie połączenia z pamięci podręcznej](../cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-cache). Należy zauważyć, że port SSL nie jest domyślnie wyłączone dla nowej pamięci podręcznej. Aby uzyskać więcej informacji na temat włączania funkcji portu bez użycia protokołu SSL zobacz sekcję [Porty dostępu](https://msdn.microsoft.com/library/azure/dn793612.aspx#AccessPorts) w temacie [Konfigurowanie pamięci podręcznej w pamięci podręcznej Azure Redis](https://msdn.microsoft.com/library/azure/dn793612.aspx) . Następujące Adiustacja pokazuje zmiany w pliku *web.config* , w szczególności zmiany do *portu*, *hosta*accessKey*, i *ssl *.

          <system.web>;
            <customErrors mode="Off" />;
            <authentication mode="None" />;
            <compilation debug="true" targetFramework="4.5" />;
            <httpRuntime targetFramework="4.5" />;
            <sessionState mode="Custom" customProvider="RedisSessionProvider">;
              <providers>;  
                  <!--<add name="RedisSessionProvider" 
                    host = "127.0.0.1" [String]
                    port = "" [number]
                    accessKey = "" [String]
                    ssl = "false" [true|false]
                    throwOnError = "true" [true|false]
                    retryTimeoutInMilliseconds = "0" [number]
                    databaseId = "0" [number]
                    applicationName = "" [String]
                  />;-->;
                 <add name="RedisSessionProvider" 
                      type="Microsoft.Web.Redis.RedisSessionStateProvider" 
                      port="6380"
                      host="movie2.redis.cache.windows.net" 
                      accessKey="m7PNV60CrvKpLqMUxosC3dSe6kx9nQ6jP5del8TmADk=" 
                      ssl="true" />;
              <!--<add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false" />;-->;
              </providers>;
            </sessionState>;
          </system.web>;


##<a id="usesessionobject"></a>Używanie obiektów sesji w kodzie
Ostatnim krokiem jest rozpoczęcie korzystania z obiektu sesji w kodzie programu ASP.NET. Dodawanie obiektów do stanu sesji odbywa się przy użyciu metody **Session.Add** . Ta metoda używa par klucz wartość do przechowywania elementów w pamięci podręcznej stan sesji.

    string strValue = "yourvalue";
    Session.Add("yourkey", strValue);

Poniższy kod pobiera wartość z stan sesji.

    object objValue = Session["yourkey"];
    if (objValue != null)
       strValue = (string)objValue; 

Za pomocą Redis pamięci podręcznej do pamięci podręcznej obiektów w aplikacji sieci web. Aby uzyskać więcej informacji zobacz [MVC filmu aplikacji z pamięci podręcznej Redis Azure w ciągu 15 minut](https://azure.microsoft.com/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/).
Aby uzyskać więcej informacji na temat używania stanu sesji ASP.NET zobacz [Omówienie stan sesji programu ASP.NET][].

>[AZURE.NOTE] Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=523751), którym natychmiast można utworzyć aplikację sieci web krótkotrwałe starter w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.

## <a name="whats-changed"></a>Informacje o zmianach
* Przewodnika do zmiany z witryn sieci Web do usługi aplikacji Zobacz: [Usługa Azure aplikacji i jego wpływ na istniejące usługi Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

  *Przez [Anderson mającej](https://twitter.com/RickAndMSFT)*
  
  [installed the latest]: http://www.windowsazure.com/downloads/?sdk=net  
  [Omówienie stanu sesji ASP.NET]: http://msdn.microsoft.com/library/ms178581.aspx

  [NewIcon]: ./media/web-sites-dotnet-session-state-caching/CacheScreenshot_NewButton.png
  [NewCacheDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CreateOptions.png
  [CacheIcon]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheIcon.png
  [NuGetDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_NuGet.png
  [OutputConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_OC_WebConfig.png
  [CacheConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheConfig.png
  [EndpointURL]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_EndpointURL.png
  [ManageKeys]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_ManageAccessKeys.png
 
