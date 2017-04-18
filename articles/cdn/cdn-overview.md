<properties
    pageTitle="Omówienie Azure CDN | Microsoft Azure"
    description="Dowiedz się, co to jest sieć dostarczania zawartości Azure (CDN) i jak go używać do dostarczania zawartości wysokiej przepustowości przez buforowanie obiektów blob i zawartość statyczną."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/30/2016"
    ms.author="casoper"/>

# <a name="overview-of-the-azure-content-delivery-network-cdn"></a>Omówienie sieci Azure dostarczania zawartości (CDN)

> [AZURE.NOTE] W tym dokumencie opisano, co to jest sieci dostarczania zawartości (CDN) Azure, jak to działa, a funkcje każdego produktu Azure CDN.  Jeśli chcesz pominąć te informacje i przejść bezpośrednio do samouczek na temat tworzenia punkt końcowy CDN, zobacz [Przy użyciu sieci CDN Azure](cdn-create-new-endpoint.md).  Jeśli chcesz wyświetlić listę bieżącej lokalizacji węzeł CDN, zobacz [Azure CDN POP lokalizacje](cdn-pop-locations.md).

Sieci dostarczania zawartości Azure (CDN) buforowanie zawartości sieci web statyczne w lokalizacjach strategicznie umieszczony zapewnienie maksymalnej przepustowości dostarczania zawartości dla użytkowników.  Sieci CDN oferuje deweloperów rozwiązania globalnego dostarczania zawartości wysokiej przepustowości przez buforowanie zawartości w węzłach fizycznie w dowolnym miejscu na świecie. 

Korzyści wynikające z używania CDN do elementy zawartości witryny sieci web pamięci podręcznej obejmują:

- Poprawić ich wydajność i użytkownika możliwości obsługi użytkowników końcowych, zwłaszcza w przypadku, gdy przy użyciu aplikacji miejsce, w którym wielu round-trips są wymagane do załadowania treści.
- Duży skalowania, aby lepiej uchwyt chwilową wysokie obciążenie, takich jak na początku zdarzeń uruchamiania produktu.
- Rozpowszechnianie żądania użytkowników i określanie zawartości z serwery graniczne, mniej ruchu są wysyłane do punktu początkowego.


## <a name="how-it-works"></a>Jak to działa

![Omówienie sieci CDN](./media/cdn-overview/cdn-overview.png)

1. Użytkownik (Alicja) żąda pliku (zwanych również środka trwałego) przy użyciu adresu URL z nazwą domeny specjalnych, takich jak `<endpointname>.azureedge.net`.  DNS kieruje żądania do najbardziej położenie punktu o obecności (POP).  Zazwyczaj jest to POP, geograficznie zbliżony do użytkownika.

2. Jeśli serwery graniczne w punktach obecności nie ma pliku w pamięci podręcznej, serwera granicznego żąda pliku od punktu początkowego.  Pochodzenie może być aplikacji sieci Web programu Azure, usługa w chmurze Azure, konto Azure miejsca do magazynowania lub dowolny serwer sieci web publicznie.

3. Pochodzenie zwraca plik na serwerze krawędzi, w tym opcjonalne nagłówki HTTP opisem pliku Time-to-Live (TTL).

4. Serwera granicznego buforowanie plik i zwraca plik do oryginalnej żądającego (Alicja).  Plik pozostanie pamięci podręcznej na serwerze granicznym, dopóki nie wygaśnie wartość TTL.  Jeśli pochodzenie nie określi TTL, domyślny czas TTL jest siedem dni.

5. Dodatkowych użytkowników może zażądać tego samego pliku przy użyciu tego samego adresu URL, a także mogą być kierowane do tego samego protokołu POP.

6. Jeśli wartość TTL pliku nie wygasła, serwera granicznego zwraca plik z pamięci podręcznej.  Powoduje szybszą i bardziej odpowiednie użytkownikom.


## <a name="azure-cdn-features"></a>Funkcje Azure CDN

Istnieją trzy produkty Azure CDN: **Azure CDN standardowy z Akamai**, **Azure CDN standardowy z Verizon**i **Premium CDN Azure z Verizon**.  W poniższej tabeli wymieniono funkcje dostępne w przypadku każdego produktu.

|       | Standardowy Akamai | Standardowy Verizon | Premium Verizon |
|-------|-----------------|------------------|-----------------|
| Łatwe integrację z Azure usług takich jak [miejsca do magazynowania](cdn-create-a-storage-account-with-cdn.md), [Usług w chmurze](cdn-cloud-service-with-cdn.md), [Aplikacje sieci Web](../app-service-web/cdn-websites-with-cdn.md)i [Usługi multimediów](../media-services/media-services-portal-manage-streaming-endpoints.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;**|
| Zarządzanie za pośrednictwem [interfejsu API usługi REST](https://msdn.microsoft.com/library/mt634456.aspx), [.NET](./cdn-app-dev-net.md), [Node.js](./cdn-app-dev-node.md)lub [programu PowerShell](./cdn-manage-powershell.md). | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| Obsługa protokołu HTTPS | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| Równoważenia obciążenia | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| Ochrona [DDOS](https://www.us-cert.gov/ncas/tips/ST04-015) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| Stos narożnej IPv4 i IPv6 | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [Obsługa nazwę domeny niestandardowej](cdn-map-content-to-custom-domain.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [Buforowanie ciągu kwerendy](cdn-query-string.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [Filtrowanie Geo](cdn-restrict-access-by-country.md) |  | **& #x 2713;** | **& #x 2713;** |
| [Szybkie wyczyść](cdn-purge-endpoint.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [Obciążenia wstępnego składników majątku](cdn-preload-endpoint.md) |  | **& #x 2713;** | **& #x 2713;** |
| [Podstawowe analizy](cdn-analyze-usage-patterns.md) |  | **& #x 2713;** | **& #x 2713;** |
| [Obsługa protokołu HTTP/2](https://msdn.microsoft.com/library/mt762901.aspx) | **& #x 2713;**  |  |  |
| [Zaawansowane raporty protokołu HTTP](cdn-advanced-http-reports.md) | | | **& #x 2713;** |
| [Statystyki w czasie rzeczywistym](cdn-real-time-stats.md) | | | **& #x 2713;** |
| [Alerty w czasie rzeczywistym](cdn-real-time-alerts.md) | | | **& #x 2713;** |
| [Aparat można dostosować, na podstawie reguł dostarczania zawartości](cdn-rules-engine.md) | | | **& #x 2713;** |
| Ustawienia pamięci podręcznej i nagłówek (za pomocą [aparatu reguł](cdn-rules-engine.md))  | | | **& #x 2713;** |
| Adres URL przekierowania i zmiana (za pomocą [aparatu reguł](cdn-rules-engine.md)) | | | **& #x 2713;** |
| Reguły urządzenia przenośnego (za pomocą [aparatu reguł](cdn-rules-engine.md))  | | | **& #x 2713;** |

>[AZURE.TIP] Czy istnieje funkcji, którą chcesz wyświetlić w sieci CDN Azure?  [Przekaż nam swoją opinię](https://feedback.azure.com/forums/169397-cdn)! 

## <a name="next-steps"></a>Następne kroki

Aby rozpocząć pracę z sieci CDN, zobacz [Używanie CDN Azure](./cdn-create-new-endpoint.md).

Jeśli jesteś klientem CDN mogą teraz zarządzać punktów końcowych sieci CDN za pośrednictwem [portalu Microsoft Azure](https://portal.azure.com) lub przy użyciu [programu PowerShell](cdn-manage-powershell.md).

Aby wyświetlić CDN w działaniu, zapoznaj się z [wideo nasz sesji 2016 kompilacji](https://azure.microsoft.com/documentation/videos/build-2016-leveraging-the-new-azure-cdn-apis-to-build-wicked-fast-applications/).

Dowiedz się, jak zautomatyzować CDN Azure za pomocą [.NET](./cdn-app-dev-net.md) lub [Node.js](./cdn-app-dev-node.md).

Aby uzyskać informacje o cenach, zobacz [CDN ceny](https://azure.microsoft.com/pricing/details/cdn/).
