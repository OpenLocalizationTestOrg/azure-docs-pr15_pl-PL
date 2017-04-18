<properties
    pageTitle="Sieci CDN pamięci podręcznej zasad w rozszerzeniu usług multimediów"
    description="Ten temat zawiera omówienie CDN pamięci podręcznej zasad w rozszerzeniu usług multimediów."
    services="media-services,cdn"
    documentationCenter=".NET"
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="juliako"/>
 
#<a name="cdn-caching-policy-in-media-services-extension"></a>Sieci CDN pamięci podręcznej zasad w rozszerzeniu usług multimediów

Usługi multimediów Azure zapewnia HTTP adaptacyjne przesyłanie strumieniowe i pobieranie stopniowe. HTTP podstawie streaming jest wysoce skalowalna z zalet pamięci podręcznej w serwera proxy i warstw CDN, jak również buforowanie po stronie klienta. Przesyłanie strumieniowe punkty końcowe zawiera ogólne możliwości przesyłanie strumieniowe, a także konfiguracji nagłówków HTTP pamięci podręcznej. Przesyłanie strumieniowe punkty końcowe ustawia kontroli pamięci podręcznej HTTP: maksymalny wiek oraz Dezaktualizuje się nagłówki i stopki. Więcej informacji o usłudze nagłówków pamięci podręcznej HTTP można przejść z [adresem W3.org](http://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html).

##<a name="default-caching-headers"></a>Domyślne buforowanie nagłówków

Domyślnie streaming punkty końcowe Zastosuj nagłówki 3 dni pamięci podręcznej danych strumieniowych na żądanie (rzeczywiste fragmenty fragmentów) i manifest(playlist). Na żywo przesyłania strumieniowego przesyłanie strumieniowe punkty końcowe zastosować 3 dni nagłówki pamięci podręcznej danych (rzeczywiste fragmenty fragmentów) i dwie sekundy pamięci podręcznej nagłówek dla żądania manifest(playlist). Jeśli live program zmieni się na żądanie (live archiwum) na żądanie przesyłanie strumieniowe pamięci podręcznej nagłówków zastosować.

##<a name="azure-cdn-integration"></a>Integracja CDN Azure

Usługi multimediów Azure udostępnia [zintegrowane CDN](https://azure.microsoft.com/updates/azure-media-services-now-fully-integrated-with-azure-cdn/) dla strumieniowego przesyłania punktów końcowych. Nagłówki kontroli pamięci podręcznej ma zastosowanie w taki sam sposób, jako strumieniowych punktów końcowych do sieci CDN włączone przesyłanie strumieniowe punktów końcowych. Azure zastosowania CDN streaming punkt końcowy skonfigurowany wartości pamięci podręcznej w celu zdefiniowania czasu życia wewnętrznie pamięci podręcznej obiektów, a także używa tej wartości, aby ustawić dostarczania pamięci podręcznej nagłówków. Włączenie przy użyciu sieci CDN przesyłanie strumieniowe punkty końcowe nie zaleca się ustawiania wartości małych pamięci podręcznej. Ustawianie wartości małych zmniejszyć wydajność i zmniejszenia korzyści CDN. Ustaw nagłówki pamięci podręcznej mniejsze niż 600 sekund CDN włączone przesyłanie strumieniowe punkty końcowe jest niedozwolone.

>[AZURE.IMPORTANT] Azure integracji usługi multimediów z sieci CDN Azure jest zaimplementowana w **Sieci CDN Azure z Verizon**.  Jeśli chcesz używać **CDN Azure z Akamai** dla usługi multimediów Azure, należy [ręcznie skonfigurować punkt końcowy](cdn-create-new-endpoint.md).  Aby uzyskać więcej informacji o funkcjach Azure CDN zobacz [Omówienie CDN](cdn-overview.md).

##<a name="configuring-cache-headers-with-azure-media-services"></a>Konfigurowanie pamięci podręcznej nagłówków przy użyciu usługi multimediów Azure

Za pomocą portalu zarządzania Azure lub interfejsów API usługi multimediów Azure skonfigurowanie wartości nagłówka pamięci podręcznej.

1. Aby skonfigurować pamięci podręcznej nagłówków przy użyciu zarządzania portalu można znaleźć do sekcji [jak zarządzanie punkty końcowe Streaming](../media-services/media-services-portal-manage-streaming-endpoints.md) Konfigurowanie punktu końcowego Streaming.
2. Usługi multimediów Azure interfejsu API usługi REST, [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx#StreamingEndpointCacheControl).
3. Usługi multimediów Azure SDK .NET, [StreamingEndpointCacheControl właściwości](http://go.microsoft.com/fwlink/?LinkId=615302).

##<a name="cache-configuration-precedence-order"></a>Pamięć podręczną konfiguracji ich hierarchia ważności

1. Wartość usługi multimediów Azure w pamięci podręcznej zastępuje wartości domyślnej.
2. W przypadku nie konfiguracji ręcznej dotyczy wartości domyślne.
3. Przez dwie sekundy domyślne w pamięci podręcznej nagłówków dotyczy live przesyłanie strumieniowe manifest(playlist) niezależnie od konfiguracji multimediów Azure lub magazyn Azure i zastępowanie tej wartości nie jest dostępna.
