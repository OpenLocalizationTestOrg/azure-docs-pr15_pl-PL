<properties 
    pageTitle="CENC z wieloma DRM i kontroli dostępu: Projektowanie odwołania i wdrożeniu Azure i Azure usługi multimediów | Microsoft Azure" 
    description="Dowiedz się, jak do zasad licencjonowania Microsoft® wygładzonymi Streaming klienta przenoszenia Kit." 
    services="media-services" 
    documentationCenter="" 
    authors="willzhan"  
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="willzhan;kilroyh;yanmf;juliako"/>

#<a name="cenc-with-multi-drm-and-access-control-a-reference-design-and-implementation-on-azure-and-azure-media-services"></a>CENC z wieloma DRM i kontroli dostępu: Projektowanie odwołania i wdrożeniu Azure i Azure usługi multimediów

##<a name="key-words"></a>Słowa kluczowe
 
Azure Active Directory, usługi multimediów Azure, Azure Media Player, dynamiczne szyfrowania licencji dostarczenia, PlayReady, Widevine, FairPlay, Encryption(CENC) typowych, wielokrotne DRM, Axinom, ŁĄCZNIKA, EME, MSE, JSON Token sieci Web (JWT), roszczeń, Nowoczesny przeglądarki, przerzucania klucza, symetrycznej klucza asymetrycznym klucza, połączyć OpenID, X509 certyfikatu. 

##<a name="in-this-article"></a>W tym artykule

W tym artykule omówione są następujące tematy:

- [Wprowadzenie](media-services-cenc-with-multidrm-access-control.md#introduction)
    - [Omówienie tego artykułu](media-services-cenc-with-multidrm-access-control.md#overview-of-this-article)
- [Projekt odwołań](media-services-cenc-with-multidrm-access-control.md#a-reference-design)
- [Mapowanie projektu na technologii do wykonania](media-services-cenc-with-multidrm-access-control.md#mapping-design-to-technology-for-implementation)
- [Implementacja](media-services-cenc-with-multidrm-access-control.md#implementation)
    - [Procedury wykonania](media-services-cenc-with-multidrm-access-control.md#implementation-procedures)
    - [Niektóre niespodzianki w celu wykonania](media-services-cenc-with-multidrm-access-control.md#some-gotchas-in-implementation)
- [Dodatkowe tematy do wykonania](media-services-cenc-with-multidrm-access-control.md#additional-topics-for-implementation)
    - [HTTP lub HTTPS](media-services-cenc-with-multidrm-access-control.md#http-or-https)
    - [Azure Active Directory podpisywania przerzucania klucza](media-services-cenc-with-multidrm-access-control.md#azure-active-directory-signing-key-rollover)
    - [Gdzie jest Token dostępu?](media-services-cenc-with-multidrm-access-control.md#where-is-the-access-token)
    - [Jak Live przesyłanie strumieniowe?](media-services-cenc-with-multidrm-access-control.md#what-about-live-streaming)
    - [Co z otwieraniem serwerów licencji poza usługi multimediów Azure?](media-services-cenc-with-multidrm-access-control.md#what-about-license-servers-outside-of-azure-media-services)
    - [Co zrobić, jeśli ma być użyty STS niestandardowe?](media-services-cenc-with-multidrm-access-control.md#what-if-i-want-to-use-a-custom-sts)
- [System ukończone i test](media-services-cenc-with-multidrm-access-control.md#the-completed-system-and-test)
    - [Identyfikator logowania użytkownika](media-services-cenc-with-multidrm-access-control.md#user-login)
    - [Przy użyciu rozszerzeń zaszyfrowanych Media dla PlayReady](media-services-cenc-with-multidrm-access-control.md#using-encrypted-media-extensipons-for-playready)
    - [Przy użyciu EME dla Widevine](media-services-cenc-with-multidrm-access-control.md#using-eme-for-widevine)
    - [Użytkownicy nie prawo](media-services-cenc-with-multidrm-access-control.md#not-entitled-users)
    - [Usługi niestandardowe bezpiecznego tokenu](media-services-cenc-with-multidrm-access-control.md#running-custom-secure-token-service)
- [Podsumowanie](media-services-cenc-with-multidrm-access-control.md#summary)

##<a name="introduction"></a>Wprowadzenie

Jest to dobrze znane jest złożone zadanie projektować i tworzyć podsystemu DRM dla OTT lub online streaming rozwiązanie. I typowych wskazówki dotyczące operatorów/online wideo dostawcom zewnętrzny tę część specjalistyczne DRM usługodawców. Celem tego dokumentu jest do prezentowania projekt odwołań i stosowania podsystemu DRM zakończenia do końca OTT lub online przesyłanie strumieniowe.

Docelowa czytelników tego dokumentu są Inżynierowie pracujący w podsystemu DRM OTT lub online streaming/wielu-screen rozwiązania lub dowolny czytników zainteresować podsystemu DRM. Zakłada się czytniki i zaznajomieni z co najmniej jednym z technologii DRM na rynku, takich jak PlayReady, Widevine, FairPlay lub Adobe dostępu.

Przez DRM możemy także zawierać CENC (typowe szyfrowanie) z wielu DRM. Główne trendu w online streaming i branżowe OTT jest za pomocą CENC wielu-native-DRM na różnych platform klienta, która jest shift z poprzedniego trendy przy użyciu jednego DRM i klienta SDK dla różnych platform klienta. Korzystając z wielu native DRM CENC, zarówno PlayReady i Widevine są szyfrowane na specyfikacji [Typowych szyfrowania (CENC ISO/IEC 23001-7)](http://www.iso.org/iso/home/store/catalogue_ics/catalogue_detail_ics.htm?csnumber=65271/) .

Zalety CENC z wieloma DRM są następujące:

1. Zmniejsza szyfrowania kosztów, ponieważ przetwarzanie jednego szyfrowania jest używany kierowanie różnych platform z jego natywnej DRMs;
1. Zmniejsza koszt zarządzanie zasobami zaszyfrowane, ponieważ jest potrzebna tylko jednej kopii zaszyfrowanych aktywów;
1. Eliminuje Licencjonowanie kosztów, ponieważ klientami DRM jest zazwyczaj wolnego miejsca na jego natywnej platformy klientów DRM.

Microsoft został aktywne promotor ŁĄCZNIKA i CENC razem z niektóre odtwarzacze głównych branżowe. Usługi multimediów Azure Microsoft obsługiwał pomocy technicznej ŁĄCZNIKA i CENC. Najnowsze anonsy, zobacz blogi i Mingfei: [informowanie o Google Widevine licencji dostarczania usług w usługi multimediów Azure](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/)i [usługi multimediów Azure dodaje opakowań Google Widevine dostarczania strumienia DRM wielokrotne](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).  

### <a name="overview-of-this-article"></a>Omówienie tego artykułu

Celem w tym artykule znajdują się następujące informacje:

1. Zawiera projekt odwołań podsystemu DRM CENC przy użyciu wielu DRM;
1. Udostępnia implementacji na platformie usługi multimediów Azure i Azure firmy Microsoft;
1. W tym artykule omówiono niektóre tematy projektowanie i wdrażanie.

W artykule "wielokrotne DRM" obejmuje następujące czynności:

1. Microsoft PlayReady
1. Google Widevine
1. FairPlay Apple (nie są jeszcze obsługiwane przez usługi multimediów Azure)

W poniższej tabeli podsumowano natywnych platformy macierzystego aplikację, a są obsługiwane przez każdego DRM.

**Platformy klienta**|**Obsługa macierzystych DRM**|**Przeglądarki i aplikacji**|**Przesyłanie strumieniowe formatów**
----|------|----|----
**Inteligentne telewizorów, operator dekoderami dekoderami OTT**|PlayReady przede wszystkim, i/lub Widevine i/lub inne|Linux, Opera, WebKit, inne|Różnych formatów
**Urządzenia systemu Windows 10 (Windows PC, tabletów z systemem Windows, Windows Phone, konsoli Xbox)**|PlayReady|MS krawędź IE11-EME<br/><br/><br/>UWP|KRESKA (dla HLS, PlayReady nie jest obsługiwany)<br/><br/>Pauza, gładkie Streaming (dla HLS, PlayReady nie jest obsługiwany) 
**Urządzeń z systemem android (telefon, tabletu, telewizji)**|Widevine|Chrome-EME|KRESKA
**iOS (telefon iPhone, iPad), OS X klientów i telewizji firmy Apple**|FairPlay|Safari 8 +/ EME|HLS
**Wtyczka: Adobe Primetime**|Primetime dostępu|Przeglądarki|OBR. / MIN, HLS

Uwzględniając bieżący stan wdrożenia dla każdego DRM usługi przeważnie warto zaimplementować DRMs 2 lub 3, aby upewnić się, że adres wszystkie typy punkty końcowe w najlepszy sposób.

Istnieje zależnościami między złożoność logiczny usługi i złożoność po stronie klienta do osiągnięcia określonego poziomu środowisko użytkownika na różnych komputerach klienckich.

Aby wprowadzić zaznaczenia, pamiętaj o następujących kwestiach:

- PlayReady oryginalnie jest zaimplementowana w każde urządzenie systemu Windows na kilka urządzeń z systemem Android, a dostępne za pośrednictwem SDK oprogramowania na niemal wszystkich platformach
- Widevine oryginalnie jest zaimplementowana w każdej urządzenie z systemem Android, Chrome i kilka innych urządzeń
- FairPlay jest dostępna tylko w systemie iOS i klientów systemu Mac OS lub za pośrednictwem usługi iTunes.

Aby typowe DRM wielokrotne mogą być następujące:

- Opcja 1: PlayReady i Widevine
- Opcja 2: PlayReady, Widevine i FairPlay


## <a name="a-reference-design"></a>Projekt odwołań

W tej sekcji firma Microsoft przedstawia projekt odwołań, czyli o niesprecyzowanym technologie używane do jego zaimplementowania.

Podsystemu DRM może zawierać następujące składniki:

1. Zarządzania kluczami
1. Pakowanie DRM
1. Dostarczenie licencji DRM
1. Sprawdź uprawnienia
1. Uwierzytelnianie i autoryzacja
1. Odtwarzacza
1. Origin-CDN

Na poniższym diagramie przedstawiono wysokiego poziomu interakcji między składnikami podsystemu DRM.

![DRM podsystemu CENC](./media/media-services-cenc-with-multidrm-access-control/media-services-generic-drm-subsystem-with-cenc.png)
 
Istnieją trzy podstawowe "warstwy" w projekcie:

1. Tworzenie kopii warstwy pakietu office (na czarnym tle), które nie są dostępne zewnętrznie;
1. Warstwy "Strefy Zdemilitaryzowanej" (niebieski) zawierającą wszystkie punkty końcowe przeciwległych publicznej;
1. Publiczny Internet warstwę (uproszczonej niebieski) zawierającą CDN i odtwarzacze z ruchem publicznego Internetu.
 
Powinny być narzędzia zarządzania zawartością zapewniające kontrolę nad ochrony DRM, bez względu na to statyczną lub dynamiczną szyfrowania. Wartości wejściowych szyfrowania DRM powinien zawierać:

1. Zawartość wideo MBR;
1. Klucz zawartości;
1. Adresy URL nabycia licencji.

W czasie odtwarzania wysokiego poziomu przepływ jest następujący:

1. Uwierzytelnieniu użytkownika;
1. Token autoryzacji zostanie utworzona dla użytkownika;
1. Zawartość DRM chroniony (manifest) są pobierane do odtwarzacza;
1. Odtwarzacz przesyła żądanie nabycia licencji do serwerów licencji razem z najważniejszych identyfikator i autoryzacji token.

Przed przejściem do następnego tematu kilka słów na temat projektu zarządzania kluczami.

**ContentKey — zawartości**|**Scenariusz**
------|---------------------------
1-do-1|Najprostszym przypadku. Udostępnia Najdrobniejsze kontrolki. Ale ogólnie powoduje najwyższy koszt dostarczenia licencji. Na minimalną jedną licencję żądania jest wymagane dla każdego chronionego trwałego.
1-do wielu|Można użyć tego samego klucza zawartości wielu zasobów. Na przykład dla wszystkich środków trwałych w grupie logicznych, takich jak gatunek lub podzestaw gatunek (lub genu filmu), możesz użyć jednego klucza zawartości.
Wiele do-1|Wiele kluczy zawartości są wymagane dla każdego środka trwałego. <br/><br/>Na przykład w razie potrzeby zastosować dynamiczną ochronę CENC z wieloma DRM dla MPEG-kreska i dynamiczne szyfrowanie AES-128 HLS wymaga dwóch oddzielnych klawiszy zawartości, każda z własną ContentKeyType. (Klucz zawartości na potrzeby dynamiczną ochronę CENC ContentKeyType.CommonEncryption powinien być używany, natomiast w przypadku zawartości klucz służący do szyfrowania AES 128 dynamiczne, ContentKeyType.EnvelopeEncryption powinien być używany.)<br/><br/>Inny przykład CENC ochrony zawartości ŁĄCZNIKA, teoretycznie jeden klucz zawartości mogą być używane w celu ochrony strumienia wideo i innej zawartości klucza ochrony strumieniu audio. 
Wiele-do - wielu|Kombinacja powyżej dwa scenariusze: jednego zestawu zawartości kluczy są używane dla każdego z wielu zasobów w tym samym składniku aktywów "Grupa".


Inny ważne czynniki do rozważenia jest użycie trwałe i nietrwałe licencji.

Dlaczego następujące zagadnienia są ważne? 

Mają bezpośredni wpływ na koszt dostarczenia licencji, jeśli korzystasz z publicznej chmury dostarczania licencji. Przypatrzmy następujących dwóch sytuacjach inny projekt, aby przedstawić:



1. Subskrypcja: za pomocą trwałych licencji, a 1-do wielu zawartości mapowania klucza do zawartości. Np. w przypadku wszystkich filmów dzieci firma Microsoft korzysta z jednego klucza zawartości na potrzeby szyfrowania. W tym przypadku: 

    Całkowita liczba licencji # zażądania wszystkich dzieci filmy na urządzeniu = 1

1. Subskrypcja: przy użyciu licencji nietrwałe i 1 do 1 mapowanie między klucz zawartości i środków trwałych. W tym przypadku:

    Całkowita liczba licencji # zażądania wszystkich dzieci filmy na urządzeniu = [obserwowane filmy #] x [# sesji]

Jak można łatwo zobaczyć, dwóch różnych projektów powodują bardzo różne licencji wezwanie na tym samym wzorców kosztów, jeśli jest dostępna usługa dostarczania licencji publicznej chmurze, takich jak usługi multimediów Azure dostarczania licencji.

## <a name="mapping-design-to-technology-for-implementation"></a>Mapowanie projektu na technologii do wykonania

Następnie możemy zamapuj naszych ogólnego projektu technologii na platformie usługi multimediów Azure i Azure firmy Microsoft, określając technologii dla każdego bloku konstrukcyjnego.

W poniższej tabeli przedstawiono mapowania:

**Blok konstrukcyjny**|**Technologia**
------|-------
**Odtwarzacza**|[Odtwarzacza multimediów Azure](https://azure.microsoft.com/services/media-services/media-player/)
**Dostawcy tożsamości (protokołu IDP)**|Azure Active Directory
**Zabezpieczanie usługi tokenu (STS)**|Azure Active Directory
**Przepływ pracy ochrony DRM**|Dynamiczne ochrony usługi multimediów Azure
**Dostarczenie licencji DRM**|1. usługi multimediów azure dostarczania licencji (PlayReady, Widevine, FairPlay), lub <br/>2. serwer licencji Axinom <br/>3. niestandardowe PlayReady serwera licencji
**Pochodzenia**|Przesyłanie strumieniowe punktu końcowego usługi multimediów Azure
**Zarządzania kluczami**|Niepotrzebne dla implementacji
**Zarządzanie zawartością**|Aplikacja konsoli C#

Innymi słowy zarówno dostawcy tożsamości (protokołu IDP), jak i zabezpieczanie tokenu usługi (STS) będzie Azure AD. Dla odtwarzacza użyjemy [Interfejs API Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/). Zarówno usługi multimediów Azure i Azure Media Player obsługuje ŁĄCZNIKA i CENC wielu DRM.

Na poniższym diagramie przedstawiono ogólna struktura i przepływ z powyższych mapowania technologii.

![CENC na AMS](./media/media-services-cenc-with-multidrm-access-control/media-services-cenc-subsystem-on-AMS-platform.png)

Aby skonfigurować dynamiczne szyfrowania CENC, narzędzie do zarządzania zawartością użyje następujące wartości wejściowych:

1. Otwieranie zawartości;
1. Klucz zawartości z klucza Generowanie i zarządzania;
1. Adresy URL nabycia licencji;
1. Lista informacji z usługi Azure Active Directory.

Dane wyjściowe narzędzia do zarządzania zawartością będą:

1. ContentKeyAuthorizationPolicy zawierające specyfikacji na sposób dostarczania licencji sprawdza tokenu JWT i specyfikacji licencji DRM;
1. AssetDeliveryPolicy zawierające specyfikacji streaming format, ochrony DRM i adresy URL nabycia licencji.

Podczas wykonywania, przepływ jest jak poniżej:

1. Po uwierzytelnianie użytkownika jest generowany tokenu JWT;
1. Jedną z roszczeń zawarte w JWT token jest zawierającej identyfikator obiektu grupy "EntitledUserGroup" zastrzeżenie "grupy". Wniosek ten będzie używana dla przekazywania "Sprawdź uprawnienia".
1. Odtwarzacza materiały do pobrania klient manifest CENC zawartości chronionej i "widzi", następujące czynności:
    1. Identyfikator, klucza 
    1. zawartość jest chroniony, CENC
    1. Adresy URL nabycia licencji.

1. Odtwarzacza sprawia, że żądanie nabycia licencji oparte na przeglądarce DRM obsługiwane. Identyfikator klucza żądanie nabycia licencji, a JWT token również zostaną przesłane. Usługa dostarczania licencji sprawdzi JWT token i roszczeń zawarte przed wydaniem potrzebnych licencji.

##<a name="implementation"></a>Implementacja

###<a name="implementation-procedures"></a>Procedury wykonania

Wykonanie obejmuje następujące czynności:

1. Przygotowywanie ich test: kodowanie pakiet wideo testu szybkości transmisji bitów wielokrotne pofragmentowane MP4 w usługi multimediów Azure. Tej zawartości nie jest chroniony DRM. Ochrony DRM zostanie wykonana przez dynamiczną ochronę później.
1. Tworzenie kluczowych identyfikator i zawartości klucza (opcjonalnie z zapasu klucza). W celu naszych systemu zarządzania kluczami nie jest potrzebna, ponieważ możemy zajmujących tylko jeden zestaw klucza identyfikator i klucz zawartości z kilku aktywów test;
1. Konfigurowanie usług dostarczania licencji DRM wielokrotne trwałego test za pomocą interfejsu API AMS. Jeśli są używane serwery niestandardowe licencji przez firmę lub dostawców firmy zamiast usługi licencji usługi multimediów Azure, należy pominąć ten krok i określić adresy URL nabycia licencji w kroku dotyczące konfigurowania dostarczania licencji. Interfejs API AMS jest potrzebny do określ, że niektóre szczegółowe konfiguracji, takich jak ograniczenia zasad autoryzacji, szablony odpowiedzi licencji dla różnych usługach licencji DRM itp. W tej chwili Azure portal nie jeszcze podaj potrzebne interfejsu użytkownika dla tej konfiguracji. Możesz znaleźć informacji o poziomie interfejsu API i przykładowy kod w dokumencie Julia Kornich: [za pomocą PlayReady i/lub Widevine dynamiczne typowych szyfrowania](media-services-protect-with-drm.md). 
1. Konfigurowanie zasad dostarczania zawartości dla trwałego test przy użyciu interfejsu API AMS. Możesz znaleźć informacji o poziomie interfejsu API i przykładowy kod w dokumencie Julia Kornich: [za pomocą PlayReady i/lub Widevine dynamiczne typowych szyfrowania](media-services-protect-with-drm.md).
1. Tworzenie i konfigurowanie dzierżawy usługi Azure Active Directory platformy Azure;
1. Tworzenie kilka kont użytkowników i grup w dzierżawie usługi Azure Active Directory: należy utworzyć co najmniej "EntitledUser" Grupowanie i dodać użytkownika do tej grupy. Użytkownicy w tej grupie przejdzie wyboru uprawnienia w pobieranie licencji i użytkownicy nie w tej grupie zakończy się niepowodzeniem w celu przekazania uwierzytelnianie i nie będą mogli uzyskać licencji. Członkostwa w tej grupie "EntitledUser" jest wymagane "grupy" roszczenia tokenu JWT wydany przez Azure AD. To wymaganie roszczeń należy określić podczas konfigurowania licencji DRM wielokrotnego dostawy usług — krok.
1. Tworzenie aplikacji programu ASP.NET MVC, który będzie obsługiwać odtwarzacza wideo. Ta aplikacja ASP.NET będą one chronione z uwierzytelniania użytkowników do dzierżawy usługi Azure Active Directory. Roszczeń pisane z wielkiej litery zostaną uwzględnione w tokeny dostępu uzyskane po uwierzytelnianie użytkownika. Interfejs API OpenID łączenie jest zalecane dla tego kroku. Należy zainstalować pakiety NuGet następujące czynności:
    - Pakiet instalacyjny Microsoft.Azure.ActiveDirectory.GraphClient
    - Pakiet instalacyjny Microsoft.Owin.Security.OpenIdConnect
    - Pakiet instalacyjny Microsoft.Owin.Security.Cookies
    - Pakiet instalacyjny Microsoft.Owin.Host.SystemWeb
    - Pakiet instalacyjny Microsoft.IdentityModel.Clients.ActiveDirectory
1. Tworzenie odtwarzaczu za pomocą [Interfejsu API odtwarzacza multimediów Azure](http://amp.azure.net/libs/amp/latest/docs/). [Interfejs API ProtectionInfo Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/) umożliwia określenie technologii DRM na innej platformie DRM.
1. Macierzy testów:

**DRM**|**Przeglądarka**|**Wynik uprawnienia użytkownika**|**Wynik nie uprawnienia użytkownika**
---|---|-----|---------
**PlayReady**|Krawędź MS lub IE11 w systemie Windows 10|Powiodła się|Fail (niepowodzenie)
**Widevine**|Chrome w systemie Windows 10|Powiodła się|Fail (niepowodzenie)
**FairPlay** |DO OPRACOWANIA PÓŹNIEJ||

Jerzy Trifonov zespołu usługi multimediów Azure został zapisany blogu udostępnia szczegółowe kroki w procesie konfigurowania usługi Azure Active Directory dla aplikacji odtwarzacza ASP.NET MVC: [Integracja MVC OWIN usługi multimediów Azure podstawie aplikacji z usługą Azure Active Directory, a także ograniczać dostarczania zawartości klucza oparte na oświadczeniach JWT](http://gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

Jerzy także został zapisany blogu na [JWT tokenu uwierzytelniania usługi multimediów Azure i szyfrowania dynamiczne](http://gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/). A Oto jego [próbki dotyczące integracji Azure AD przy użyciu usługi multimediów Azure klucza dostarczenia](https://github.com/AzureMediaServicesSamples/Key-delivery-with-AAD-integration/).

Aby uzyskać informacje dotyczące usługi Azure Active Directory:

- Informacje dla deweloperów można znaleźć w [przewodniku Azure Active Directory dewelopera](../active-directory/active-directory-developers-guide.md).
- Informacje dla administratorów można znaleźć w [Katalogu AD Azure i administrowanie](../active-directory/active-directory-administer.md).

### <a name="some-gotchas-in-implementation"></a>Niektóre niespodzianki w celu wykonania

Istnieje kilka "niespodzianki" w realizacji. Hopefully poniżej "niespodzianki" może ułatwić rozwiązywanie problemów w przypadku, gdy wystąpią problemy.

1. **Wystawcy** Adres URL powinien mieć **"/"**.  

    **Grupy odbiorców** powinno być identyfikator klienta aplikacji odtwarzacza i należy również dodać na końcu adresu URL wystawcy **"/"** .

        <add key="ida:audience" value="[Application Client ID GUID]" />
        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/" /> 

    W [Dekoder JWT](http://jwt.calebb.net/)powinny zostać wyświetlone **lub** i **firmy** jako poniżej tokenu JWT:
    
    ![1 gotcha](./media/media-services-cenc-with-multidrm-access-control/media-services-1st-gotcha.png)


2. Dodaj uprawnienia do aplikacji w AAD (na karcie Konfigurowanie aplikacji). Jest to wymagane dla każdej aplikacji (w wersji lokalnej i wdrożonym).

    ![2 gotcha](./media/media-services-cenc-with-multidrm-access-control/media-services-perms-to-other-apps.png)


3. Użyj prawo wystawcy w procesie konfigurowania dynamiczną ochronę CENC:

        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/"/>
    
    Nie zadziała następujące czynności:
    
        <add key="ida:issuer" value="https://willzhanad.onmicrosoft.com/" />
    
    Identyfikator GUID jest identyfikatora AAD dzierżawy. Identyfikator GUID znajdują się w okienku podręcznym punkty końcowe w Azure portal.

4. Udzielanie członkostwo w grupach oświadczeń uprawnienia. Upewnij się, w pliku manifestu AAD aplikacji i zawiera następujące czynności

    "groupMembershipClaims": "Wszystkie" (wartość domyślna to null)

5. Ustawianie TokenType pisane z wielkiej litery, podczas tworzenia wymagania ograniczeń.

        objTokenRestrictionTemplate.TokenType = TokenType.JWT;

    Ponieważ dodawanie obsługi JWT (AAD) oprócz SWT (ACS), domyślne TokenType jest TokenType.JWT. Jeśli używasz SWT-ACS, należy ustawić na TokenType.SWT.

## <a name="additional-topics-for-implementation"></a>Dodatkowe tematy do wykonania
Następnie firma Microsoft będzie dysku uss niektóre tematy dodatkowe w naszym projektowanie i wdrażanie.

###<a name="http-or-https"></a>HTTP lub HTTPS?

Aplikacja odtwarzacza ASP.NET MVC, które często musi obsługiwać następujących czynności:

1. Uwierzytelnianie użytkownika za pośrednictwem Azure AD, które muszą być w obszarze protokołu HTTPS.
1. Exchange token JWT między klientem a Azure AD, które muszą być w obszarze protokołu HTTPS.
1. Pobieranie licencji DRM przez klienta, który musi być w obszarze HTTPS, jeśli jest dostępna dostarczania licencji usługi multimediów Azure. Oczywiście zestawu produktów PlayReady nie przesądza HTTPS dostarczania licencji. Jeśli serwer licencji PlayReady wykracza poza usługi multimediów Azure, można używać protokołu HTTP lub HTTPS.

W związku z tym aplikacja odtwarzacza ASP.NET użyje HTTPS, zgodnie z zaleceniami dotyczącymi. Oznacza to, że będzie Azure Media Player na stronie w obszarze HTTPS. Jednak do przesyłania strumieniowego możemy wolisz HTTP, w związku z tym potrzebujemy do rozważenia mieszane problem zawartości.

1. Przeglądarka nie zezwala na zawartość mieszaną. Ale zezwalaj wtyczek, takich jak dodatek Silverlight i OSMF dla gładkie i ŁĄCZNIKA. Zawartość mieszana jest problem dotyczący zabezpieczeń — jest to z powodu zagrożenia funkcji do dodania złośliwy JS, co może spowodować danych klientów zagrożenie.  Przeglądarki zablokować to domyślne, a następnie pory jest jedynym sposobem obejścia go po stronie serwera (pochodzenie), aby umożliwić wszystkich domen (bez względu na to https lub http). To prawdopodobnie nie jest dobrym pomysłem albo.
1. Firma Microsoft należy unikać zawartość mieszana: zarówno za pomocą protokołu HTTP albo oba użycie protokołu HTTPS. Podczas odtwarzania zawartość mieszana silverlightSS tech wymaga wyczyszczenie mieszanych ostrzeżenie zawartości. flashSS tech obsługuje zawartość mieszana bez ostrzeżenia mieszane zawartości.
1. Jeśli utworzono punkt końcowy przesyłania strumieniowego przed sierpnia 2014 r, nie obsługuje protokół HTTPS. W tym przypadku Utwórz i używanie przesyłanie strumieniowe nowy punkt końcowy dla HTTPS.

W celu wykonania odniesienia dla zawartości DRM chronione aplikacji i przesyłanie strumieniowe będą w obszarze HTTTPS. Otwieranie zawartości odtwarzacza nie wymaga uwierzytelniania lub licencji, więc musisz swobodę za pomocą protokołu HTTP lub HTTPS.

### <a name="azure-active-directory-signing-key-rollover"></a>Azure Active Directory podpisywania przerzucania klucza

To należy wziąć pod uwagę implementacji. Jeśli nie uważasz to w implementacji, wykonanych system ostatecznie przestaną działać całkowicie w ciągu co najwyżej 6 tygodni.

Azure AD używa standardowego w celu ustanowienia zaufania między sobą i aplikacji przy użyciu Azure AD. W szczególności Azure AD używa klucza podpisywania składający się z pary kluczy publicznych i prywatnych. Gdy Azure AD tworzy tokenu zabezpieczającego, która zawiera informacje o użytkowniku, token ten został podpisany przez Azure AD przy użyciu kluczem prywatnym przed wysłaniem powrót do aplikacji. Aby sprawdzić, czy token jest prawidłowy i faktycznie udzielonych z usługi Azure Active Directory, aplikacja musi Sprawdź poprawność podpisu tokenu przy użyciu klucza publicznego ujawnionego przez Azure AD znajdującej się w dokumencie metadanych Federacji dzierżawy. Ten klucz publiczny — i klucza podpisywania, z której pochodzi — jest taki sam, jedną dla wszystkich dzierżaw w Azure AD.

Szczegółowe informacje o przerzucania klucza Azure AD znajdują się w dokumencie: [Ważne informacje o logowaniu przerzucania klucza w Azure AD](../active-directory/active-directory-signing-key-rollover.md).

Między [pary kluczy publicznych i prywatnych](https://login.windows.net/common/discovery/keys/) 

- Klucz prywatny jest używany przez usługi Azure Active Directory do wygenerowania tokenu JWT;
- Klucz publiczny jest używany przez aplikację, takich jak usługi dostarczania licencji DRM w AMS do weryfikowania tokenu JWT;
 
W celu zabezpieczenia usługi Azure Active Directory obraca ten certyfikat okresowo (co tydzień). W przypadku naruszenia bezpieczeństwa przerzucania klucza może wystąpić w dowolnym momencie. W związku z tym usług dostarczania licencji w AMS konieczne zaktualizowanie klucz publiczny używany Azure AD obraca pary kluczy, w przeciwnym razie token uwierzytelniania w AMS zakończy się niepowodzeniem i zostanie wydana bez licencji. 

W tym celu należy przez ustawienie TokenRestrictionTemplate.OpenIdConnectDiscoveryDocument podczas konfigurowania usług dostarczania licencji DRM.

Przepływ token JWT jest jako poniżej:

1.  Azure AD będzie wydać JWT token bieżący klucz prywatny dla użytkownika uwierzytelnionego;
2.  Po odtwarzaczu wykryje CENC z zawartością DRM wielokrotne, chronione, najpierw będą zlokalizować co token JWT wydany przez Azure AD.
3.  Odtwarzacz przesyła żądanie nabycia licencji przy użyciu tokenu JWT do usług dostarczania licencji w AMS;
4.  Przy użyciu bieżącego i prawidłowy klucz publiczny z Azure AD usług dostarczania licencji w AMS Sprawdź tokenu JWT przed wydaniem licencji.

Usługi dostarczania licencji DRM będzie zawsze wyszukiwanie bieżącego i prawidłowy klucz publiczny z Azure AD. Klucz publiczny prezentowany przez Azure AD będzie klucz służący do weryfikacji tokenu JWT wydany przez Azure AD.

Co zrobić, jeśli przerzucania klucza się dzieje, gdy AAD generuje JWT token, ale przed JWT token jest wysyłana przy odtwarzacze DRM licencji dostarczenia w usługach AMS weryfikujące? 

Ponieważ klucz może być podsumowywane w dowolnym momencie, że zawsze będzie więcej niż jeden nieprawidłowy klucz publiczny dostępne w dokumencie metadanych federacji. Dostarczenie licencji usługi multimediów Azure można użyć dowolnej z klawiszy określony w dokumencie, ponieważ jeden klawisz mogą być szybko wdrażania, innej mogą być jej duplikat i tak dalej.

### <a name="where-is-the-access-token"></a>Gdzie jest Token dostępu?

Jeśli możesz obejrzeć jak aplikacji sieci web wymaga aplikacji interfejsu API tożsamością [aplikacji z OAuth 2.0 klienta poświadczeń Udziel](active-directory-authentication-scenarios.md#web-application-to-web-api), przepływ uwierzytelniania jest jako poniżej:

1.  Użytkownik jest zalogowany Azure AD w aplikacji sieci web (zobacz [Przeglądarki sieci Web do aplikacji sieci Web](active-directory-authentication-scenarios.md#web-browser-to-web-application).
2.  Punkt końcowy autoryzacji Azure AD przekierowuje agenta użytkownika do aplikacji klienckiej z kodem autoryzacji. Agenta użytkownika zwraca kod autoryzacji z aplikacją kliencką przekierowania URI.
3.  Aplikacja sieci web musi uzyskać token dostępu, który można uwierzytelnić interfejsu API sieci web i pobrać żądanego zasobu. Jest to żądanie tokenu punktu końcowego Azure AD, podanie poświadczeń, identyfikator klienta i aplikacji sieci web interfejsu API identyfikator URI. Prezentowane przy jego użyciu kodu autoryzacji, aby udowodnić, że użytkownik ma zgodę.
4.  Azure AD uwierzytelnia aplikacji i zwraca token dostępu JWT służy do wywołania interfejsu API sieci web.
5.  Za pomocą protokołu HTTPS aplikacji sieci web używa zwracane token dostępu JWT dodać ciąg JWT z oznaczeniem "Okaziciela" w nagłówku autoryzacji żądania do interfejsu API sieci web. Interfejsu API sieci web sprawdza JWT token i jeśli sprawdzanie poprawności zakończyło się pomyślnie, zwraca żądanego zasobu.

W tym przepływie "tożsamości aplikacji" interfejsu API sieci web zaufania, że aplikacji sieci web uwierzytelnianie użytkownika. Z tego powodu tego wzorca nosi nazwę zaufanej podsystemu. [Diagram na tej stronie](http://msdn.microsoft.com/library/azure/dn645542.aspx/) opisano, jak kod autoryzacji udzielić przepływu działa.

W pobieranie licencji z tokenu ograniczeń możemy po takim samym wzorcem zaufanych podsystemu. I usługi dostarczania licencji w ramach usługi multimediów Azure zasobu interfejsu API, "zasobu wewnętrznej bazy danych" aplikacji sieci web musi mieć dostęp do sieci web. Gdzie jest więc token dostępu?

Firma Microsoft są w rzeczywistości uzyskiwanie token dostępu z usługi Azure Active Directory. Po uwierzytelnieniu użytkownika pomyślnie zwracany jest kod autoryzacji. Kod autoryzacji następnie umożliwia, razem z kluczy identyfikator i aplikacji klienta exchange dla token dostępu. A token dostępu do uzyskiwania dostępu do aplikacji "wskaźnik" wskazującą lub reprezentującą usługa dostarczania licencji usługi multimediów Azure.

Trzeba Zarejestruj i skonfiguruj aplikację "wskaźnik" w Azure AD, wykonując poniższe czynności:

1.  W dzierżawie Azure AD

    - Dodawanie aplikacji (zasobów) z adresem URL logowania jednokrotnego: 

    https://[resource_name].azurewebsites.NET/ i 

    - adres URL identyfikator aplikacji: 
    
    https://[aad_tenant_name].onmicrosoft.com/[resource_name]; 
2.  Dodaj nowy klucz dla zasobów aplikacji;
3.  Aktualizowanie pliku manifestu aplikacji tak, aby właściwość groupMembershipClaims ma następującą wartość: "groupMembershipClaims": "Wszystkie",  
4.  W aplikacji Azure AD wskazująca odtwarzacza aplikacji sieci web w sekcji "uprawnień do innych aplikacji" Dodawanie aplikacji zasobów, które zostało dodane w kroku 1. W obszarze "przypisane uprawnienia" Zaznacz znacznik wyboru "W programie Access [resource_name]". Dzięki temu uprawnień aplikacji sieci web do tworzenia tokeny dostępu do uzyskiwania dostępu do aplikacji zasobów. Należy to zrobić dla lokalnych i wdrożonym wersji aplikacji web app opracowywania aplikacji sieci web programu Visual Studio i Azure.
    
Dlatego token JWT wydany przez Azure AD jest faktycznie token dostępu do uzyskiwania dostępu do tego zasobu "wskaźnik".

### <a name="what-about-live-streaming"></a>Jak Live przesyłanie strumieniowe?

W przypadku powyższych naszych dyskusji ma zostały skoncentrowanie się na zasoby na żądanie. Jak live streaming?

Dobra wiadomość jest, której można dokładnie samej projektowanie i wdrażanie ochrony live streaming w usługi multimediów Azure, umieszczając zawartości skojarzonego z programem jako "trwały VOD".

W szczególności jest dobrze znane, że w live streaming w usługi multimediów Azure, należy utworzyć kanał, a następnie program w sekcji kanał. Aby utworzyć program, należy utworzyć zasób, zawierające live archiwum programu. Aby zapewnić CENC DRM wielokrotne ochrony zawartości na żywo, wszystko, czego należy wykonać, jest zastosowanie samej konfiguracji i przetwarzania elementu tak jakby była zasób"VOD" przed rozpoczęciem programu.

### <a name="what-about-license-servers-outside-of-azure-media-services"></a>Co z otwieraniem serwerów licencji poza usługi multimediów Azure?

Często klienci mogą mieć zainwestowanego w licencji farmy serwerów własnych danych Centrum lub hostowanej przez dostawców usług DRM. Na szczęście ochrony zawartości usługi multimediów Azure pozwala pracować w trybie hybrydowym: zawartość hostowanej i dynamicznie chronione usługi multimediów Azure, gdy licencji DRM są dostarczane przez serwery spoza usługi multimediów Azure. W tym przypadku są następujące kwestie zmiany:

1. Token usługi bezpiecznego musi problemu tokeny, które są dopuszczalne i mogą zostać sprawdzone przez licencji farmy serwerów. Na przykład serwery licencji Widevine podane przez Axinom wymaga określonego tokenu JWT zawierający "uprawnienia komunikat". W związku z tym musisz dysponować STS do wykonania tych token JWT. Autorów ukończono takie implementacja i szczegóły można znaleźć w następujących dokumentu w [Centrum dokumentacji Azure](https://azure.microsoft.com/documentation/): [Za pomocą Axinom do przeprowadzania Widevine licencje usługi multimediów Azure](media-services-axinom-integration.md). 
1. Nie trzeba skonfigurować usługę dostarczania licencji (ContentKeyAuthorizationPolicy) w usługi multimediów Azure. Co należy zrobić jest zapewnienie licencji adresy URL acquisition (PlayReady, Widevine i FairPlay) po skonfigurowaniu AssetDeliveryPolicy w procesie konfigurowania CENC z wieloma DRM.
 
### <a name="what-if-i-want-to-use-a-custom-sts"></a>Co zrobić, jeśli ma być użyty STS niestandardowe?

Może istnieć kilka przyczyn, które klienta może wybrać niestandardowe STS (Usługa tokenu bezpiecznego) umożliwiające tokeny JWT. Niektóre z nich są:

1.  Tożsamość dostawcy (protokołu IDP) używane przez klienta nie obsługuje usługi STS. W tym przypadku niestandardowej usługi STS może być odpowiednią opcję.
2.  Bardziej elastyczne ściślejsza kontrolki lub w integracji usługi STS z rozliczenia system subskrybentów klienta może być konieczne klienta. Na przykład MVPD operator mogą oferować wielu pakietów subskrybentów OTT takich jak premium podstawowe sports, itp. Operator może być konieczne odpowiadają oświadczeniach tokenu z pakietem subskrybentów, dzięki czemu tylko zawartość w pakiecie prawo staje się dostępny. W tym przypadku niestandardowej usługi STS znajdują się potrzebne elastyczność i kontrolę.

Dwie zmiany należy wykonać podczas korzystania z niestandardowego STS:

1.  Podczas konfigurowania usługi dostarczania licencji dla środka trwałego, musisz określić klucz zabezpieczeń na potrzeby weryfikacji przez STS niestandardowe (poniżej więcej szczegółów) zamiast bieżącego klucza z usługi Azure Active Directory.
2.  Podczas generowania tokenu JTW klucza zabezpieczeń określono zamiast klucz prywatny bieżącą X509 certyfikat usługi Azure Active Directory.

Istnieją dwa typy kluczy zabezpieczeń:

1.  Klucz symetrycznej: tego samego klucza służy do generowania, a także weryfikowanie tokenu JWT;
2.  Klucz asymetrycznym: parę kluczy publicznych i prywatnych w X509 klucz prywatny szyfrowania i generowania tokenu JWT i kluczem publicznym sprawdzania tokenu używanego certyfikatu.

####<a name="tech-note"></a>Uwaga techniczna

Jeśli korzystasz z programu .NET Framework i C# jako platformy projektowej, X509 certyfikat na potrzeby klucz zabezpieczeń asymetrycznym musi mieć długość co najmniej 2048. Jest to wymagane klasy System.IdentityModel.Tokens.X509AsymmetricSecurityKey w architekturze .NET Framework. W przeciwnym razie zostanie wygenerowany następujący wyjątek:

IDX10630: "System.IdentityModel.Tokens.X509AsymmetricSecurityKey" do podpisywania nie może być mniejszy niż bitów "2048". 

## <a name="the-completed-system-and-test"></a>System ukończone i test

Firma Microsoft prowadzi przez kilka scenariuszy w złożonym systemie zakończenia do końca tak, aby czytelnicy mogą mieć basic "obraz" zachowanie przed pobraniem konta logowania.

Aplikacja sieci web odtwarzacza i jego logowania można znaleźć [tutaj](https://openidconnectweb.azurewebsites.net/).

Jeśli potrzebne jest "wartością zintegrowanego" scenariusz: aktywa wideo obsługiwane w usługi multimediów Azure, które są albo bez ochrony lub chroniony DRM, ale bez token uwierzytelniania (wydawania licencji kto żąda go), testowanie bez logowania (według przechodzenia do protokołu HTTP, w przypadku usługi strumieniowe przesyłanie wideo za pomocą protokołu HTTP).

Jeśli co jest potrzebne jest kompleksowe — scenariusz zintegrowane: zasoby wideo znajduje się w obszarze dynamiczną ochronę DRM w usługi multimediów Azure, z tokenu uwierzytelniania i token JWT generowany przez Azure AD, musisz zalogować.

### <a name="user-login"></a>Identyfikator logowania użytkownika

Aby przetestować zintegrowanego systemu DRM end-to-end, musisz mieć "konto" utworzone lub dodane. 

Jakie konto?

Mimo że Azure pierwotnie dostęp tylko przez użytkowników, konta Microsoft, teraz umożliwia dostęp użytkowników z obu kierunków. To zostało wykonane przez wszystkie właściwości Azure Ufaj Azure AD dla uwierzytelniania o Azure AD uwierzytelniania użytkowników w organizacji, a przez utworzenie relacji Federacji miejsce, w którym Azure AD zaufania systemu Microsoft konta dla klientów indywidualnych tożsamości do uwierzytelniania użytkowników indywidualnych. W wyniku Azure AD jest mogli przeprowadzać uwierzytelniania konta serwera Microsoft "Gość", a także konta Azure AD "macierzystych".

Ponieważ Azure AD zaufania domeny konta Microsoft (MSA), można dodać dowolny kontom z jednej z następujących domen niestandardowych Azure AD dzierżawa i korzystania z konta do logowania:

**Nazwa domeny**|**Domeny**
-----------|----------
**Niestandardowe Azure AD dzierżawy domeny**|somename.onmicrosoft.com
**Domeny firmy**|witryny Microsoft.com
**Domeny konta Microsoft (MSA)**|Outlook.com, live.com, hotmail.com

Można skontaktować się z wymienionych autorów mam konto utworzone lub dodane. 

Poniżej znajdują się zrzuty ekranu przedstawiające strony logowania różnych kont innej domeny.

**Niestandardowe Azure AD konta domeny dzierżawy**: W tym przypadku zostanie wyświetlona strona logowania dostosowane niestandardowego Azure AD dzierżawy domeny.

![Konta domeny niestandardowe Azure AD dzierżawy](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain1.png)

**Konta domeny firmy Microsoft przy użyciu karty inteligentnej**: W tym przypadku zostanie wyświetlona strona logowania dostosowany przez firmę Microsoft corporate IT z uwierzytelnianie dwuskładnikowe.

![Konta domeny niestandardowe Azure AD dzierżawy](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain2.png)

**Konta Microsoft (MSA)**: W tym przypadku zostanie wyświetlona strona logowania Account Microsoft dla klientów indywidualnych.

![Konta domeny niestandardowe Azure AD dzierżawy](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain3.png)

### <a name="using-encrypted-media-extensions-for-playready"></a>Przy użyciu rozszerzeń zaszyfrowanych Media dla PlayReady

W przeglądarce nowoczesny z rozszerzeniem szyfrowane multimediów (EME) PlayReady pomoc techniczną, takich jak 11 programu Internet Explorer w systemie Windows 8.1, jak i i przeglądarki Microsoft Edge w systemie Windows 10 PlayReady będzie źródłowych DRM dla EME.

![Przy użyciu EME dla PlayReady](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready1.png)

Obszar ciemny odtwarzacza jest z faktu, że ochrona PlayReady zapobiega z wprowadzania przechwycony ekran wideo chroniony. 

Poniższym zrzucie ekranu przedstawiono Obsługa MSE-EME i dodatków plug-in odtwarzacza.

![Przy użyciu EME dla PlayReady](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready2.png)

EME w Microsoft Edge i programu Internet Explorer 11 w systemie Windows 10 umożliwia wywoływanie z [PlayReady SL3000](https://www.microsoft.com/playready/features/EnhancedContentProtection.aspx/) na urządzeniach z systemem Windows 10, obsługujące go. PlayReady SL3000 odblokowanie przepływu rozszerzonego specjalnej zawartości (4K HDR, itp.) i nowych modeli dostarczania zawartości (najwcześniejsze okno dla rozszerzonego zawartości).

Skup się na urządzenia z systemem Windows: PlayReady jest tylko DRM w sprzętowego dostępna na urządzenia z systemem Windows (PlayReady SL3000). Przesyłanie strumieniowe usługi za pomocą PlayReady przez EME lub za pośrednictwem aplikacji UWP i oferują wyższą jakość wideo przy użyciu PlayReady SL3000 niż innego DRM. Zazwyczaj zawartość 2K będzie przepływał przez Chrome i Firefox i 4K zawartości za pomocą Microsoft Edge-IE11 lub aplikację UWP na tym samym urządzeniu (w zależności od ustawień usługi i wykonania).


#### <a name="using-eme-for-widevine"></a>Przy użyciu EME dla Widevine

W przeglądarce nowoczesny z obsługą EME-Widevine, takich jak Chrome 41 + w systemie Windows 10, Windows 8.1, Mac OSX Yosemite i Chrome na Android w wersji 4.4.4 Google Widevine jest DRM za EME.

![Przy użyciu EME dla Widevine](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine1.png)

Zwróć uwagę, że Widevine nie uniemożliwia z wprowadzania przechwycony ekran wideo chroniony.

![Przy użyciu EME dla Widevine](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine2.png)

### <a name="not-entitled-users"></a>Użytkownicy nie prawo

Jeśli użytkownik nie jest członkiem grupy "Użytkownicy prawo", użytkownik nie będzie mógł zaliczone "Sprawdź uprawnienia" i usługa licencji DRM wielokrotne będzie odmówić wydania żądanej licencji, tak jak pokazano poniżej. Szczegółowy opis jest "uzyskiwanie licencji nie powiodło się", która jest zgodnie z przeznaczeniem.

![Użytkownicy nie prawo](./media/media-services-cenc-with-multidrm-access-control/media-services-unentitledusers.png)

### <a name="running-custom-secure-token-service"></a>Usługi niestandardowe bezpiecznego tokenu

Dla tego scenariusza uruchomienia niestandardowego bezpiecznego tokenu usługi (STS) JWT token zostanie wystawiony przez STS niestandardowych przy użyciu klucza symetrycznej lub asymetrycznym. 

W przypadku przy użyciu klucza symetrycznej (przy użyciu przeglądarki Chrome):

![Uruchamianie usługi STS niestandardowe](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts1.png)

W przypadku przy użyciu klucza asymetrycznym za pośrednictwem X509 certyfikatów (za pomocą przeglądarki Nowoczesna firmy Microsoft).

![Uruchamianie usługi STS niestandardowe](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts2.png)

W obu przypadkach powyżej uwierzytelnianie użytkownika nie zmienia — za pośrednictwem Azure AD. Jedyna różnica polega na tym, że tokeny JWT są wydawane przez niestandardowe STS zamiast Azure AD. Oczywiście, konfigurując dynamiczną ochronę CENC, to ograniczenie licencji usługi określa typ tokenu JWT symetrycznej lub asymetrycznym klucz.

## <a name="summary"></a>Podsumowanie

W tym dokumencie wspomniano CENC z wielu native DRM i kontroli dostępu za pośrednictwem uwierzytelniania token: jej projekt i jego wdrożenie przy użyciu Azure, usługi multimediów Azure i Azure Media Player.

- Projekt odwołań są prezentowane, który zawiera wszystkie niezbędne składniki podsystemu DRM-CENC;
- Implementacji Azure, usługi multimediów Azure i Azure Media Player.
- Ponadto omówiono niektóre tematy, które są bezpośrednio związane z projektem i implementacją.


##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

###<a name="acknowledgments"></a>Potwierdzenia 

Zhang łączy, Mingfei Yan, franka Le Roland, Kilroy Olecka, Julia Kornich
