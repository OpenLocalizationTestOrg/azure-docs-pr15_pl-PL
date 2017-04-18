<properties 
    pageTitle="Ochrona zawartości omówienie | Microsoft Azure" 
    description="W tym artykule nadać Omówienie ochrony zawartości z usługi multimediów." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/27/2016" 
    ms.author="juliako"/>

#<a name="protecting-content-overview"></a>Ochrona zawartości — omówienie


Usługi multimediów Azure firmy Microsoft umożliwia bezpieczne multimediów od momentu opuszczenia tego komputera za pomocą przechowywania, przetwarzania i dostarczania. Usługi multimediów pozwala na przedstawianie żywo i na żądanie zawartości zaszyfrowanych dynamicznie za pomocą szyfrowania AES (Advanced Standard) (za pomocą klawiszy szyfrowania 128-bitowego) lub na dowolnej stronie głównych DRMs: Microsoft PlayReady, Google Widevine i FairPlay firmy Apple. Usługi multimediów są także usługą dostarczania kluczy AES i DRM (PlayReady, Widevine i FairPlay) licencje autoryzowanych klientów. 

Poniższa ilustracja przedstawia przepływów pracy ochrony zawartości, które obsługuje AMS. 

![Ochrona z PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

>[AZURE.NOTE]Aby można było używać szyfrowania dynamiczne, możesz uzyskać co najmniej jedną jednostkę zastrzeżone przesyłanie strumieniowe przesyłanie strumieniowe końcowego, z której chcesz przesyłać strumieniowo zaszyfrowanej zawartości.

W tym temacie omówiono [pojęcia i terminologię](media-services-content-protection-overview.md) dotyczącym Opis ochrony zawartości z AMS. Temat zawiera również [łącza](media-services-content-protection-overview.md#common-scenarios) do tematów, pokazujące, jak osiągnąć zadania ochrony zawartości. 

##<a name="dynamic-encryption"></a>Dynamiczne szyfrowania

Usługi multimediów Azure firmy Microsoft umożliwia dostarczanie zawartości szyfrowane dynamicznie AES klucza czyszczenia lub szyfrowania DRM: Microsoft PlayReady, Google Widevine i FairPlay firmy Apple.

Obecnie możesz szyfrowanie następujących formatów strumieniowych: HLS, MPEG ŁĄCZNIKA i wygładzonymi Streaming. Nie można zaszyfrować obr. / min streaming format lub stopniowe do pobrania.

Usługi multimediów do szyfrowania środka trwałego, należy skojarzyć klucza szyfrowania (CommonEncryption lub EnvelopeEncryption) do zawartości, a także skonfigurować zasady autoryzacji klucza.

Trzeba również skonfigurować zasady dostarczania zawartości. Jeśli chcesz strumienia zasób zaszyfrowanych miejsca do magazynowania, pamiętaj określić sposób przedstawiania go przez skonfigurowanie zasad dostarczania zawartości.

W przypadku zażądania strumienia przez odtwarzacz, usługi multimediów używa określonego klucza dynamicznie szyfrowanie do zawartości za pomocą klucza czyszczenia AES lub szyfrowania DRM. Aby odszyfrować strumienia, odtwarzacza zażąda klucz z usługi klucza dostarczenia. Określanie, czy użytkownik jest autoryzowany do uzyskania klucza, usługa oblicza zasady autoryzacji, które określone dla klucza.

>[AZURE.NOTE]Aby skorzystać z dynamicznego szyfrowania, możesz uzyskać co najmniej jedną jednostkę strumieniowych na żądanie przesyłanie strumieniowe punktu końcowego, z której ma zostać dostarczania zawartości zaszyfrowanej. Aby uzyskać więcej informacji zobacz [jak skala usługi multimediów](media-services-portal-manage-streaming-endpoints.md).

##<a name="storage-encryption"></a>Szyfrowanie miejsca do magazynowania

Szyfrowanie Wyczyść zawartość lokalnie, przy użyciu szyfrowania bitowego AES 256, a następnie przekazanie go do magazynu Azure miejsce, w którym jest przechowywany szyfrowane na pozostałych za pomocą szyfrowania miejsca do magazynowania. Zasoby chronione za pomocą szyfrowania miejsca do magazynowania automatycznie bez szyfrowania i umieszczone w systemie zaszyfrowanego pliku przed kodowanie i opcjonalnie ponownie szyfrowane przed przekazaniem jako nowy trwały dane wyjściowe. W przypadku użycia podstawowego szyfrowania miejsca do magazynowania jest, gdy chcesz zabezpieczyć wysokiej jakości plików multimedialnych wprowadzania danych przy użyciu silnego szyfrowania spoczynku na dysku.

Dostarczenia zasób zaszyfrowanych miejsca do magazynowania, należy skonfigurować zasady dostarczenia zasobu, aby usługi multimediów wie, jak ma być dostarczania zawartości. Przed można przesłać strumieniowo do zawartości, serwera strumieniowych usuwa szyfrowania miejsca do magazynowania i umożliwia strumieniowe przesyłanie zawartości za pomocą zasad określonego odbiorcy (na przykład AES, typowych szyfrowania lub bez szyfrowania).

## <a name="common-encryption-cenc"></a>Typowe szyfrowania (CENC)

Typowe szyfrowania jest używane do szyfrowania zawartości z PlayReady lub / i Widewine.

## <a name="using-cbcs-aapl-encryption"></a>Przy użyciu szyfrowania cbcs aapl

Cbcs aapl jest używane do szyfrowania zawartości z FairPlay.

## <a name="envelope-encryption"></a>Koperta szyfrowania 

Użyj tej opcji, jeśli chcesz ochronić zawartość kluczem wyczyść AES-128. Jeśli chcesz zabezpieczyć opcji, wybierz jedną z DRMs wymienionych w tym temacie. 

##<a name="licenses-and-keys-delivery-service"></a>Usługa licencje i klucze

Usługi multimediów udostępnia usługę dostarczania licencji DRM (PlayReady, Widevine, FairPlay) i AES wyczyść klawiszy autoryzowanych klientów. [Azure portal](media-services-portal-protect-content.md), interfejsu API usługi REST lub SDK usługi multimediów dla środowiska .NET umożliwia konfigurowanie zasad autoryzacji i uwierzytelniania dla klawiszy i licencji.

##<a name="token-restriction"></a>Ograniczenie tokenu

Zasady zawartości autoryzacji klucza może mieć jeden lub więcej ograniczeń autoryzacji: Otwórz lub token ograniczeń. Token zasady ograniczeniami towarzyszy token wystawiony przez bezpiecznego tokenu usługi (STS). Usługi multimediów obsługuje tokeny w JSON Token sieci Web (JWT) a formatem prosty tokeny sieci Web (SWT). Usługi multimediów nie umożliwia zabezpieczanie usług tokenu. Można tworzyć niestandardowe STS lub korzystać z usługi Microsoft Azure ACS do tokenów problem. Usługi STS musi być skonfigurowany do tworzenia tokenu zalogowani za pomocą określonego oświadczeniach problem i klucza, określone w konfiguracji token ograniczeń. Dostarczenie klucza usługi multimediów zwróci żądany klucz (lub licencji) do klienta, jeśli tokenu jest prawidłowe i roszczeń w dopasowanie tokenu tych skonfigurowane dla klucz (lub licencji).

Podczas konfigurowania token ograniczony zasad, musisz określić klucz podstawowy weryfikacji, wystawcy i parametrów odbiorców. Klucz podstawowy weryfikacji zawiera klucz token został podpisany przy, wystawcy jest usługę tokenu bezpiecznego problemy tokenu. Grupy odbiorców (nazywane zakresu) w tym artykule opisano przeznaczenie tokenu lub zasób tokenu zezwala na dostęp do. Dostarczenie klucza usługi multimediów sprawdza, czy te wartości tokenu zgodne z wartościami w szablonie.

##<a name="streaming-urls"></a>Przesyłanie strumieniowe adresy URL

Jeśli do zawartości zaszyfrowanych z więcej niż jeden DRM, należy użyć tag szyfrowania w adresie URL przesyłanie strumieniowe: (format = "aapl m3u8", szyfrowania = 'xxx').

Następujące kwestie:

- Można określić tylko zero lub jeden typ szyfrowania.
- Typ szyfrowania nie ma określonego w adresie url, jeśli tylko jeden szyfrowania została zastosowana do środka.
- Typ szyfrowania jest uwzględniana wielkość liter.
- Można określić następujące typy szyfrowania:  
    - **cenc**: typowe szyfrowanie (Playready lub Widevine)
    - **cbcs aapl**: Fairplay
    - **CBC**: szyfrowania koperty AES.

##<a name="common-scenarios"></a>Typowe scenariusze

W poniższych tematach pokazano, jak ochrona zawartości w magazynie, przeprowadzania dynamicznie zaszyfrowanych multimediów strumieniowych, użyj AMS klucza licencji usługi

- [Ochrona z AES](media-services-protect-with-aes128.md) 
- [Ochrona z PlayReady i/lub Widevine](media-services-protect-with-drm.md)
- [Przesyłanie strumieniowe usługi HLS chronione zawartości z Apple FairPlay i/lub PlayReady](media-services-protect-hls-with-fairplay.md)

### <a name="additional-scenarios"></a>Dodatkowe scenariusze

- [Jak zintegrować usługę Azure PlayReady licencji z własnych szyfrujący streaming server](http://mingfeiy.com/integrate-azure-playready-license-service-encryptorstreaming-server).
- [Dostarczanie DRM licencji usługi multimediów Azure za pomocą castLabs](media-services-castlabs-integration.md)
 
##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-links"></a>Łącza pokrewne

[Informowanie o PlayReady jako usługa i dynamiczne szyfrowania AES z usługi multimediów Azure](http://mingfeiy.com/playready)

[Omówienie ceny dostarczania licencji PlayReady usługi multimediów Azure](http://mingfeiy.com/playready-pricing-explained-in-azure-media-services)

[Sposób debugowania dla AES strumienia zaszyfrowane w usługi multimediów Azure](http://mingfeiy.com/debug-aes-encrypted-stream-azure-media-services)

[JWT token authenitcation](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Integracja MVC OWIN usługi multimediów Azure podstawie aplikacji z usługą Azure Active Directory, a także ograniczać dostarczania zawartości klucza oparte na oświadczeniach JWT](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[Użyj Azure ACS do tokenów problem](http://mingfeiy.com/acs-with-key-services).

[content-protection]: ./media/media-services-content-protection-overview/media-services-content-protection.png
