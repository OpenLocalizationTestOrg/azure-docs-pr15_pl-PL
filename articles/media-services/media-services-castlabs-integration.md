<properties 
    pageTitle="Dostarczanie Widevine licencji usługi multimediów Azure za pomocą castLabs | Microsoft Azure" 
    description="W tym artykule opisano, jak usługi multimediów Azure (AMS) umożliwia przedstawianie strumień dynamicznie są szyfrowane za pomocą AMS PlayReady i Widevine DRMs. Licencja PlayReady pochodzi z serwera licencji PlayReady usługi multimediów i licencji Widevine został dostarczony przez serwer licencji castLabs." 
    services="media-services" 
    documentationCenter="" 
    authors="Mingfeiy" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="Mingfeiy;willzhan;Juliako"/>


#<a name="using-castlabs-to-deliver-widevine-licenses-to-azure-media-services"></a>Dostarczanie Widevine licencji usługi multimediów Azure za pomocą castLabs

> [AZURE.SELECTOR]
- [Axinom](media-services-axinom-integration.md)
- [castLabs](media-services-castlabs-integration.md)

##<a name="overview"></a>Omówienie

W tym artykule opisano, jak usługi multimediów Azure (AMS) umożliwia przedstawianie strumień dynamicznie są szyfrowane za pomocą AMS PlayReady i Widevine DRMs. Licencja PlayReady pochodzi z serwera licencji PlayReady usługi multimediów i licencji Widevine został dostarczony przez serwer licencji **castLabs** .

Aby odtwarzanie strumieniowego przesyłania zawartości chronionej przez CENC (PlayReady lub Widevine) możesz użyć [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html). Aby uzyskać szczegółowe informacje, zobacz [AMP dokumentu](http://amp.azure.net/libs/amp/latest/docs/) .

Poniższy diagram przedstawia wysokiego poziomu usługi multimediów Azure i Architektura integracji castLabs.

![Integracja z programem](./media/media-services-castlabs-integration/media-services-castlabs-integration.png)

##<a name="typical-system-set-up"></a>Konfigurowanie systemowe, zwykle

- Zawartość multimedialną są przechowywane w AMS.
- Identyfikatory klucza zawartości kluczy są przechowywane w castLabs i AMS.
- castLabs i AMS mają token uwierzytelniania wbudowana. W poniższych sekcjach omówiono tokenów uwierzytelniania. 
- Podczas żądania klienta do strumienia wideo, zawartości jest dynamicznie zaszyfrowanych za pomocą **Typowych szyfrowania** (CENC) i dynamicznie spakowany przy AMS Streaming wygładzonymi i ŁĄCZNIKA. Możemy również udostępniać PlayReady M2TS szkół podstawowych strumienia szyfrowania protokołu przesyłanie strumieniowe HLS.
- Licencja PlayReady jest pobierana z serwera licencji AMS i licencji Widevine jest pobierana z serwera licencji castLabs. 
- Odtwarzacz automatycznie zdecyduje licencji do pobierania według możliwości platformy klienta. 

##<a name="authentication-token-generation-for-getting-a-license"></a>Generowanie token uwierzytelniania uzyskiwania licencji

Zarówno castLabs, jak i AMS obsługuje token JWT (JSON Web Token) w formacie używanym Aby autoryzować licencji. 

###<a name="jwt-token-in-ams"></a>Token JWT na AMS 

W poniższej tabeli opisano token JWT na AMS. 

Wystawcy|Ciąg wystawcy z wybranego bezpiecznego tokenu usługi (STS)
---|---
Grupy odbiorców|Ciąg odbiorców z używanych STS
Roszczeń|Zestaw oświadczeń
Nie wcześniej niż|Rozpoczynanie ważności tokenu
Wygasa|Koniec ważności tokenu
Elementu SigningCredentials|Klucz współużytkowany PlayReady serwera licencji, castLabs serwera licencji i usługi STS, może to być symetrycznej lub asymetrycznym klucza.

###<a name="jwt-token-in-castlabs"></a>Token JWT na castLabs

W poniższej tabeli opisano token JWT na castLabs. 

Nazwa|Opis
---|---
optData|Ciąg JSON, zawierający informacje o użytkowniku. 
CRT|Ciąg JSON zawierający informacje na temat zawartości, jego prawa informacje i odtwarzanie licencji.
IAT|Bieżącej daty i godziny w epoch.
jti|Unikatowy identyfikator o ten token (każdy token można używać tylko raz w systemie castLabs).

##<a name="sample-solution-set-up"></a>Konfigurowanie przykładowe rozwiązanie 

[Przykładowe rozwiązanie](https://github.com/AzureMediaServicesSamples/CastlabsIntegration) obejmuje dwa projekty:

-   Aplikacja konsoli, który może być używany w celu ustawienia ograniczeń DRM na zasób już zasysanego zarówno PlayReady i Widevine.
-   Aplikacja sieci Web, która przekazuje się tokeny, które mogą być widoczne jako bardzo uproszczony wersji usługi STS.


Aby użyć aplikacji konsoli:

1.  Zmienianie app.config Konfigurowanie poświadczeń AMS poświadczeń castLabs, STS i Konfigurowanie klucza udostępnionego.
2.  Przekaż środka trwałego do AMS.
3.  Zmienianie linii 32 w pliku Plik Program.cs i uzyskać UUID z przekazywanych elementów zawartości:

         var objIAsset = _context.Assets.Where(x => x.Id == "nb:cid:UUID:dac53a5d-1500-80bd-b864-f1e4b62594cf").FirstOrDefault();

4.  Za pomocą IDzasobu nazewnictwa środka trwałego w systemie castLabs (44 wiersza w pliku Plik Program.cs).

    Należy ustawić IDzasobu dla **castLabs**; wymagane jest unikatowy ciąg alfanumeryczny.

5.  Uruchom program.


Aby użyć aplikacji sieci Web (STS):

1.  Zmienianie pliku web.config sprzedawcy castlabs ustawienia Identyfikatora i konfigurowanie usługi STS klucz udostępniony.
2.  Wdrażanie Azure witryn sieci Web.
3.  Przejdź do witryny sieci Web.

##<a name="playing-back-a-video"></a>Odtwarzanie klipu wideo

Odtwarzanie klipu wideo zaszyfrowanych za pomocą typowych szyfrowania (PlayReady lub Widevine) umożliwia [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html). Podczas uruchamiania aplikacji konsoli, identyfikator klucza zawartości oraz adres URL pojawiają są wyświetlane.

1.  Otwórz nową kartę i uruchamianie usługi STS: http://[yourStsName].azurewebsites.net/api/token/assetid/[yourCastLabsAssetId]/contentkeyid/[thecontentkeyid].
2.  Przejdź do [odtwarzania multimediów Azure](http://amsplayer.azurewebsites.net/azuremediaplayer.html).
3.  Wklej adres URL przesyłanie strumieniowe.
4.  Kliknij pole wyboru **Opcje zaawansowane** .
5.  Na liście rozwijanej **Ochrona** zaznacz PlayReady i/lub Widevine.
6.  Wklej tokenu uzyskanego od usługi STS w polu tekstowym Token. 
    
    Nie jest konieczne serwera licencji castLab "okaziciela =" prefiks przed tokenu. Tak, Usuń który przed przesłaniem tokenu.
7.  Aktualizowanie odtwarzacza.
8.  Należy odtwarzanie klipu wideo.


##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
