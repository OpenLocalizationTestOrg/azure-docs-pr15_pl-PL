<properties 
    pageTitle="Konfigurowanie zasad autoryzacji klucza zawartości za pomocą portalu Azure | Microsoft Azure" 
    description="Dowiedz się, jak skonfigurować zasady autoryzacji klucza zawartości." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/12/2016" 
    ms.author="juliako"/>



#<a name="configure-content-key-authorization-policy"></a>Konfigurowanie zasad zawartości autoryzacji klucza
[AZURE.INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]


##<a name="overview"></a>Omówienie

Usługi multimediów Azure firmy Microsoft umożliwia przedstawianie strumienie MPEG-kreska, gładkie Streaming i HTTP-Live — Streaming (HLS) chronione za pomocą szyfrowania AES (Advanced Standard) (za pomocą klawiszy szyfrowania 128-bitowego) lub [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/). AMS umożliwia przedstawianie strumienie ŁĄCZNIKA zaszyfrowanych za pomocą Widevine DRM. Zarówno PlayReady i Widevine są szyfrowane na specyfikacji typowych szyfrowania (ISO/IEC CENC 23001-7).

Usługi multimediów udostępnia również **Klucza-licencji usługi** , z którego klienci mogą uzyskiwać PlayReady-Widevine licencji do odtwarzania zawartości zaszyfrowane lub kluczy AES.

W tym temacie przedstawiono sposób Azure portal umożliwia skonfigurowanie zasady autoryzacji klucza zawartości. Klucz można później dynamicznie szyfrowanie zawartości. Uwaga obecnie można szyfrowanie streaming następujące formaty: HLS, MPEG ŁĄCZNIKA i wygładzonymi Streaming. Nie można zaszyfrować obr. / min streaming format lub stopniowe do pobrania.

Gdy odtwarzaczu żąda strumienia, który jest ustawiona na dynamicznie szyfrowanie, usługi multimediów używa klucza skonfigurowanych dynamicznie szyfrowanie treści przy użyciu szyfrowania AES lub DRM. Aby odszyfrować strumienia, odtwarzacza zażąda klucz z usługi klucza dostarczenia. Określanie, czy użytkownik jest autoryzowany do uzyskania klucza, usługa oblicza zasady autoryzacji, które określone dla klucza.


Jeśli zamierzasz wielu klawiszy zawartości lub chcesz określić adres URL **Klucza-licencji usługi** niż klucza dostarczenia usługi multimediów za pomocą multimediów usług .NET SDK lub interfejsów API pozostałych.

[Konfigurowanie zasad autoryzacji klucza zawartości przy użyciu zestawu SDK .NET usługi multimediów](media-services-dotnet-configure-content-key-auth-policy.md)

[Konfigurowanie zasad autoryzacji klucza zawartości za pomocą interfejsu API usługi REST usługi multimediów](media-services-rest-configure-content-key-auth-policy.md)

###<a name="some-considerations-apply"></a>Niektóre kwestie:

- Aby można było używać dynamiczne opakowania i szyfrowania dynamiczne, należy się upewnić mieć co najmniej jeden streaming zastrzeżone jednostek. Aby uzyskać więcej informacji zobacz [jak skalowanie usługi multimediów](media-services-portal-manage-streaming-endpoints.md).
- Do zawartości musi zawierać zestaw adaptacyjne szybkość transmisji bitów MP4s lub adaptacyjne szybkość transmisji bitów wygładzonymi strumieniowego przesyłania plików. Aby uzyskać więcej informacji zobacz [kodowanie środka trwałego](media-services-encode-asset.md).
- Usługa dostarczania klucza buforowanie ContentKeyAuthorizationPolicy i jej powiązanych obiektów (opcje zasad i ograniczeń) na 15 minut.  Jeśli tworzysz ContentKeyAuthorizationPolicy i określ umożliwia ograniczenie "Tokenu", przetestowanie i następnie zaktualizować zasady ograniczeń "Otwórz", trwa około 15 minut, zanim zasady przełączy się do wersji "Otwarte" zasady.


##<a name="how-to-configure-the-key-authorization-policy"></a>Jak: Konfigurowanie zasad autoryzacji klucza

Aby skonfigurować zasady autoryzacji klucza, zaznacz stronę **Ochrony zawartości** .

Usługi multimediów obsługuje wiele metod uwierzytelniania użytkowników, którzy żądania klucza. Zasady zawartości autoryzacji klucza może mieć **Otwieranie**, **token**lub ograniczenia dotyczące autoryzacji **adresów IP** (**IP** można skonfigurować z pozostałych lub .NET SDK).

###<a name="open-restriction"></a>Otwieranie ograniczeń

**Otwórz** ograniczeń oznacza, że system będzie przedstawiana klucz dla każdego, kto wykonuje żądanie klucza. To ograniczenie może być przydatne podczas testowania.

![OpenPolicy][open_policy]

###<a name="token-restriction"></a>Ograniczenie tokenu

Aby wybrać token zasady ograniczeniami, naciśnij przycisk **TOKEN** .

Zasady **token** ograniczone towarzyszy token wystawiony przez **Usługę tokenu bezpiecznego** (STS). Usługi multimediów obsługuje tokeny w **Prostej tokeny sieci Web** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) a formatem **JSON Web Token** (JWT). Aby uzyskać informacje zobacz [JWT token uwierzytelniania](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).

Usługi multimediów nie umożliwia **Zabezpieczanie usług tokenu**. Można tworzyć niestandardowe STS lub korzystać z usługi Microsoft Azure ACS do tokenów problem. Usługi STS musi być skonfigurowany do tworzenia tokenu zalogowani za pomocą określonego oświadczeniach problem i klucza, określone w konfiguracji token ograniczeń. Dostarczenie klucza usługi multimediów zwróci klucza szyfrowania do klienta, jeśli tokenu jest ważna, a oświadczeniach tokenu pasują do zawartości klucza. Aby uzyskać więcej informacji zobacz [Używanie Azure ACS do tokenów problem](http://mingfeiy.com/acs-with-key-services).

Podczas konfigurowania **TOKEN** ograniczony zasad, należy ustawić wartości **klucza weryfikacji**, **wystawcy** i **odbiorców**. Klucz podstawowy weryfikacji zawiera klucz token został podpisany przy, wystawcy jest usługę tokenu bezpiecznego problemy tokenu. Grupy odbiorców (nazywane zakresu) w tym artykule opisano przeznaczenie tokenu lub zasób tokenu zezwala na dostęp do. Dostarczenie klucza usługi multimediów sprawdza, czy te wartości tokenu zgodne z wartościami w szablonie.

###<a name="playready"></a>PlayReady

Gdy Ochrona zawartości z **PlayReady**, jedną z czynności, które należy określić w zasad autoryzacji jest ciąg XML, który definiuje szablon licencji PlayReady. Domyślnie jest ustawiony następujących zasad:

<PlayReadyLicenseResponseTemplate xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1">
      <LicenseTemplates>
        <PlayReadyLicenseTemplate><AllowTestDevices>true</AllowTestDevices>
          <ContentKey i:type="ContentEncryptionKeyFromHeader" />
          <LicenseType>Nonpersistent</LicenseType>
          <PlayRight>
            <AllowPassingVideoContentToUnknownOutput>Allowed</AllowPassingVideoContentToUnknownOutput>
          </PlayRight>
        </PlayReadyLicenseTemplate>
      </LicenseTemplates>
    </PlayReadyLicenseResponseTemplate>

Można kliknij przycisk **Importuj zasady xml** i podaj różnych XML, które spełniają do schematu XML, zdefiniowane [w tym miejscu](https://msdn.microsoft.com/library/azure/dn783459.aspx).


##<a name="next-step"></a>Następny krok

Przejrzyj ścieżki szkoleniowe dla użytkowników usługi multimediów.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





[open_policy]: ./media/media-services-portal-configure-content-key-auth-policy/media-services-protect-content-with-open-restriction.png
[token_policy]: ./media/media-services-key-authorization-policy/media-services-protect-content-with-token-restriction.png

