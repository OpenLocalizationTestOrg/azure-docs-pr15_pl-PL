<properties
 pageTitle="Jak zarządzać wygasania zawartości usługi aplikacji-chmura Azure sieci Web, programu ASP.NET i usług IIS w sieci CDN Azure | Microsoft Azure"
 description="Opisano, jak zarządzać wygasaniem chmury usługi zawartości w sieci CDN Azure"
 services="cdn"
 documentationCenter=".NET"
 authors="camsoper"
 manager="erikre"
 editor=""/>
<tags
 ms.service="cdn"
 ms.workload="media"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="09/19/2016"
 ms.author="casoper"/>

# <a name="how-to-manage-expiration-of-azure-web-appscloud-services-aspnet-or-iis-content-in-azure-cdn"></a>Jak zarządzać wygasania zawartości usługi aplikacji-chmura Azure sieci Web, programu ASP.NET lub usług IIS w sieci CDN Azure

> [AZURE.SELECTOR]
- [Usługi aplikacji chmura azure sieci Web, programu ASP.NET lub usług IIS](cdn-manage-expiration-of-cloud-service-content.md)
- [Azure magazyn obiektów blob usługi](cdn-manage-expiration-of-blob-content.md)

Mogą być buforowane pliki z dowolnym serwerze sieci web publicznie pochodzenia w sieci CDN Azure, aż po jego time to live (TTL).  Wartość TTL jest określona przez [ *Cache-Control* nagłówka](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) w odpowiedzi HTTP z serwera pochodzenia.  W tym artykule opisano, jak ustawić `Cache-Control` nagłówki dla aplikacji sieci Web Azure, usług w chmurze Azure aplikacji ASP.NET i witryny internetowe usługi informacyjne, które zostały skonfigurowane podobnie.

>[AZURE.TIP] Można ustawić nie TTL w pliku.  W tym przypadku Azure CDN automatycznie stosuje domyślne TTL siedem dni.
>
>Aby uzyskać więcej informacji na temat współdziałania sieci CDN Azure aby przyspieszyć dostęp do plików i innych zasobów zobacz [Omówienie CDN Azure](./cdn-overview.md).

## <a name="setting-cache-control-headers-in-configuration"></a>Ustawianie nagłówki kontroli pamięci podręcznej w konfiguracji

Na zawartość statyczną, takie jak obrazy i arkusze stylów możesz sterować częstotliwość aktualizacji przez zmianę plików **applicationHost.config** lub **web.config** dla aplikacji sieci web.  Element **system.webServer\staticContent\clientCache** w pliku konfiguracji zostanie ustawiona `Cache-Control` nagłówka dla zawartości. Dla **web.config**ustawienia konfiguracji wpłynie na wszystkie elementy w folderze wraz ze wszystkimi podfolderami, chyba że zastąpić na poziomie podfolderu.  Można na przykład Ustaw domyślne time to live w katalogu głównym mają cała zawartość statyczną w pamięci podręcznej dla 3 dni, ale masz podfolderu zawierającej więcej zmiennych zawartości za pomocą ustawienia pamięci podręcznej, 6 godzin.  **ApplicationHost.config**wszystkich aplikacji w witrynie zostaną zmienione, ale może zostać zastąpiona w plikach **web.config** aplikacji.

Następujące programy XML i przykład ustawienie **clientCache** , aby określić maksymalny wiek 3 dni:  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

Określanie **UseMaxAge** dodaje `Cache-Control: max-age=<nnn>` nagłówka do odpowiedzi na podstawie wartości określonej w atrybucie **CacheControlMaxAge** . Format przedziału czasu jest dla atrybutu **cacheControlMaxAge** jest <days>. <hours>:<min>:<sec>. Aby uzyskać więcej informacji w węźle **clientCache** , zobacz [pamięci podręcznej klienta <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).  

## <a name="setting-cache-control-headers-in-code"></a>Ustawianie nagłówki kontroli pamięci podręcznej w kodzie

W przypadku aplikacji ASP.NET można ustawić CDN pamięci podręcznej zachowanie programowo przez ustawienie właściwości **HttpResponse.Cache** . Aby uzyskać więcej informacji na temat właściwości **HttpResponse.Cache** zobacz [Właściwość HttpResponse.Cache](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) i [HttpCachePolicy klasy](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

Jeśli chcesz programowo pamięci podręcznej zawartości aplikacji w programie ASP.NET, upewnij się, że zawartość nie została oznaczona jako buforowalne przez ustawienie HttpCacheability *publiczną*. Upewnij się również, że jest wybrana wartość sprawdzania pamięci podręcznej. Moduł sprawdzania poprawności pamięci podręcznej może być Ostatnia modyfikacja sygnatur czasowych określonych przez wywołanie SetLastModified lub wartość etag Ustawianie dzwoniąc SetETag. Opcjonalnie można też określić czas wygaśnięcia pamięci podręcznej, dzwoniąc SetExpires lub polega na heurystykę pamięci podręcznej domyślne opisane wcześniej w tym dokumencie.  

Na przykład aby buforować zawartości przez godzinę, należy dodać:  

```csharp
// Set the caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="next-steps"></a>Następne kroki

- [Przeczytaj informacje o elemencie **clientCache**](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
- [Zapoznaj się z dokumentacją dla właściwości **HttpResponse.Cache**](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
- [Przeczytaj dokumentację dla **Klasy HttpCachePolicy**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  
