<properties 
    pageTitle="Konfigurowanie zasad ochrony zawartości za pomocą portalu Azure | Microsoft Azure" 
    description="W tym artykule przedstawiono sposób użycia Azure portal, aby skonfigurować zasady ochrony zawartości. Artykuł zawiera także jak włączyć dynamiczne szyfrowania trwałych." 
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
    ms.date="10/24/2016"    
    ms.author="juliako"/>

# <a name="configuring-content-protection-policies-using-the-azure-portal"></a>Konfigurowanie zasad ochrony zawartości za pomocą portalu Azure

> [AZURE.NOTE] Aby użyć tego samouczka, potrzebne jest konto Azure. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).

## <a name="overview"></a>Omówienie

Microsoft Azure multimediów usług (AMS) umożliwia bezpieczne multimediów od momentu opuszczenia tego komputera za pomocą przechowywania, przetwarzania i dostarczania. Usługi multimediów pozwala na przedstawianie zawartości szyfrowane dynamicznie z szyfrowania AES (Advanced Standard) (za pomocą klawiszy szyfrowania 128-bitowego), typowe szyfrowanie (CENC) przy użyciu PlayReady i/lub Widevine DRM i FairPlay firmy Apple. 

AMS udostępnia usługę dostarczania licencji DRM i AES wyczyść klawiszy autoryzowanych klientów. Azure portal umożliwia tworzenie jeden **klawisz/licencji zasady autoryzacji** dla wszystkich typów skonfigurowała.

W tym artykule przedstawiono sposób konfigurowania zasady ochrony zawartości Portal Azure. Artykuł zawiera także stosowania dynamiczne szyfrowania do aktywów.

> [AZURE.NOTE]  Klasyczny portalu Azure umożliwia tworzenie zasad ochrony, zasad mogą nie być wyświetlane w [Azure portal](https://portal.azure.com/). Jednak wszystkie stare zasady nadal istnieje. Można sprawdzić za pomocą SDK .NET usługi multimediów Azure lub [Azure-Media--Eksploratora usług](https://github.com/Azure/Azure-Media-Services-Explorer/releases) (Aby wyświetlić zasad, kliknij prawym przyciskiem myszy trwałego -> Wyświetl informacje (F4) -> kliknij kartę zawartości klucze -> kliknij klucz). 
> 
> Szyfrowanie do zawartości za pomocą nowych zasad je skonfigurować Portal Azure, kliknij przycisk Zapisz i ponowne stosowanie szyfrowania dynamiczne. 

## <a name="start-configuring-content-protection"></a>Rozpocznij Konfigurowanie ochrony zawartości

Aby rozpocząć konfigurowanie Ochrona zawartości za pomocą portalu, globalnej z Twoim kontem AMS, wykonaj następujące czynności:

1. W [portalu Azure](https://portal.azure.com/)wybierz swoje konto usługi multimediów Azure.
2. Wybierz pozycję **Ustawienia** > **ochrony zawartości**.

![Ochrona zawartości](./media/media-services-portal-content-protection/media-services-content-protection001.png)
 

## <a name="keylicense-authorization-policy"></a>Zasady autoryzacji klucza i licencji

AMS obsługuje wiele metod uwierzytelniania użytkowników, którzy żądania klucza lub licencji. Zasady zawartości autoryzacji klucza należy skonfigurować przez Ciebie i spełnione przez klienta w kolejności klucza-licencji do można delived do klienta. Zasady zawartości autoryzacji klucza może mieć jeden lub więcej ograniczeń autoryzacji: **Otwórz** lub **token** ograniczeń.

Azure portal umożliwia tworzenie jeden **klawisz/licencji zasady autoryzacji** dla wszystkich typów skonfigurowała.

###<a name="open"></a>Otwieranie 

Otwieranie ograniczeń oznacza, że system będzie przedstawiana klucz dla każdego, kto wykonuje żądanie klucza. To ograniczenie może być przydatne do celów testowych. 

### <a name="token"></a>Tokenu

Token zasady ograniczeniami towarzyszy token wystawiony przez bezpiecznego tokenu usługi (STS). Usługi multimediów obsługuje tokeny w JSON Token sieci Web (JWT) a formatem prosty tokeny sieci Web (SWT). Usługi multimediów nie umożliwia zabezpieczanie usług tokenu. Można tworzyć niestandardowe STS lub korzystać z usługi Microsoft Azure ACS do tokenów problem. Usługi STS musi być skonfigurowany do tworzenia tokenu zalogowani za pomocą określonego oświadczeniach problem i klucza, określone w konfiguracji token ograniczeń. Dostarczenie klucza usługi multimediów zwróci żądany klucz (lub licencji) do klienta, jeśli tokenu jest prawidłowe i roszczeń w dopasowanie tokenu tych skonfigurowane dla klucz (lub licencji).

Podczas konfigurowania token ograniczony zasad, musisz określić klucz podstawowy weryfikacji, wystawcy i parametrów odbiorców. Klucz podstawowy weryfikacji zawiera klucz token został podpisany przy, wystawcy jest usługę tokenu bezpiecznego problemy tokenu. Grupy odbiorców (nazywane zakresu) w tym artykule opisano przeznaczenie tokenu lub zasób tokenu zezwala na dostęp do. Dostarczenie klucza usługi multimediów sprawdza, czy te wartości tokenu zgodne z wartościami w szablonie.

![Ochrona zawartości](./media/media-services-portal-content-protection/media-services-content-protection002.png)

## <a name="playready-rights-template"></a>Szablon praw PlayReady

Uzyskać szczegółowe informacje na temat szablon praw PlayReady zobacz [Omówienie szablonu licencji PlayReady usługi multimediów](media-services-playready-license-template-overview.md).

### <a name="non-persistent"></a>Użytkownicy niebędący trwałych

Po skonfigurowaniu licencję jako nietrwałe go tylko odbywa się w pamięci podczas odtwarzacza korzysta z licencją.  

![Ochrona zawartości](./media/media-services-portal-content-protection/media-services-content-protection003.png)

### <a name="persistent"></a>Trwałe

Jeśli skonfigurujesz licencji jako trwałych, zostanie on zapisany w wygenerowanie na komputerze klienckim.

![Ochrona zawartości](./media/media-services-portal-content-protection/media-services-content-protection004.png)

## <a name="widevine-rights-template"></a>Szablon praw Widevine

Aby uzyskać szczegółowe informacje dotyczące szablon praw Widevine zobacz [Omówienie szablonu licencji Widevine](media-services-widevine-license-template-overview.md).

### <a name="basic"></a>Podstawowe

Po zaznaczeniu **podstawowe**, szablon zostanie utworzony przy użyciu wszystkich wartości domyślne.

### <a name="advanced"></a>Zaawansowane

Aby uzyskać szczegółowe informacje o opcja Zaawansowane ustawienia konfiguracji Widevine zobacz [w tym](media-services-widevine-license-template-overview.md) temacie.

![Ochrona zawartości](./media/media-services-portal-content-protection/media-services-content-protection005.png)

## <a name="fairplay-configuration"></a>Konfiguracja FairPlay

Aby włączyć szyfrowanie FairPlay, musisz podać certyfikat aplikacji i klucz tajny aplikacji (ASK) za pomocą opcji konfiguracji FairPlay. Aby uzyskać szczegółowe informacje dotyczące konfiguracji FairPlay i wymagania zobacz [w tym](media-services-protect-hls-with-fairplay.md) artykule.

![Ochrona zawartości](./media/media-services-portal-content-protection/media-services-content-protection006.png)

## <a name="apply-dynamic-encryption-to-your-asset"></a>Stosowanie dynamiczne szyfrowania do swojej zawartości

Aby skorzystać z dynamicznego szyfrowania, należy wykonać następujące czynności:

- Kodowanie pliku źródłowego do zestawu plików MP4 adaptacyjne szybkość transmisji bitów.
- Uzyskaj co najmniej jedną jednostkę strumieniowych na żądanie przesyłanie strumieniowe punktu końcowego, z której ma zostać dostarczania zawartości. Aby uzyskać więcej informacji zobacz [jak skalowanie streaming zastrzeżone jednostki na żądanie](media-services-portal-manage-streaming-endpoints.md).

### <a name="select-an-asset-that-you-want-to-encrypt"></a>Wybierz zasób, który chcesz zaszyfrować

Aby wyświetlić wszystkie elementy zawartości, wybierz pozycję **Ustawienia** > **aktywów**.

![Ochrona zawartości](./media/media-services-portal-content-protection/media-services-content-protection007.png)

### <a name="encrypt-with-aes-or-drm"></a>Szyfruj przy użyciu AES lub DRM

Po naciśnięciu **Szyfrowanie** aktywów dostępne są dwie opcje: **AES** lub **DRM**. 

#### <a name="aes"></a>AES

Wyczyść szyfrowania będą dostępne na wszystkich protokołów AES: wygładzonymi Streaming, HLS i MPEG-kreska.

![Ochrona zawartości](./media/media-services-portal-content-protection/media-services-content-protection008.png)

#### <a name="drm"></a>DRM

Po wybraniu na karcie DRM dostępne są różne opcje zasad ochrony zawartości (który należy skonfigurować już) + zestawu protokoły przesyłania strumieniowego.

- **PlayReady i Widevine na PAUZY MPEG** - dynamicznie są szyfrowane strumienia MPEG-kreska z PlayReady i Widevine DRMs.
- **PlayReady i Widevine z MPEG-kreska + FairPlay z HLS** - są dynamicznie szyfrowane możesz strumienia MPEG-kreska z PlayReady i Widevine DRMs. Są również szyfrowane strumienie HLS z FairPlay.
- **PlayReady tylko z wygładzonymi Streaming, HLS i ŁĄCZNIKA MPEG** - są dynamicznie szyfrowane wygładzonymi Streaming HLS, MPEG-kreska strumienie z PlayReady DRM.
- **Widevine tylko z ŁĄCZNIKA MPEG** - dynamicznie Zaszyfruj możesz MPEG-kreska z Widevine DRM.
- **FairPlay tylko z HLS** - dynamicznie są szyfrowane strumienia HLS z FairPlay.

Aby włączyć szyfrowanie FairPlay, musisz podać certyfikat aplikacji i klucz tajny aplikacji (ASK) za pomocą opcji konfiguracji FairPlay karta Ustawienia ochrony zawartości.

![Ochrona zawartości](./media/media-services-portal-content-protection/media-services-content-protection009.png)

Po wprowadzeniu wybór szyfrowania, naciśnij przycisk **Zastosuj**.

##<a name="next-steps"></a>Następne kroki

Przejrzyj ścieżki szkoleniowe dla użytkowników usługi multimediów.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





