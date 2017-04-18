<properties 
    pageTitle="Wprowadzenie do usługi MFA Server Mobile aplikacji sieci Web"
    description="Aplikacja uwierzytelnianie wieloskładnikowe Azure oferuje opcję dodatkowego uwierzytelniania w nowym oknie grupy.  Umożliwia używanie powiadomień wypychanych dla użytkowników na serwerze MFA."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="getting-started-the-mfa-server-mobile-app-web-service"></a>Wprowadzenie do usługi MFA Server Mobile aplikacji sieci Web

Aplikacja uwierzytelnianie wieloskładnikowe Azure oferuje opcję dodatkowego uwierzytelniania w nowym oknie grupy. Zamiast umieszczenie automatyczną rozmowy telefonicznej lub wiadomości SMS użytkownikowi podczas logowania, uwierzytelnianie wieloskładnikowe Azure umieszcza powiadomienie do aplikacji uwierzytelnianie wieloskładnikowe Azure na użytkownika smartphone lub tablecie. Użytkownik po prostu naciska "Uwierzytelniania" (lub wprowadza numer PIN i naciska "Uwierzytelniania") w aplikacji, aby się zalogować.

Aby można było używać aplikacji uwierzytelnianie wieloskładnikowe Azure, następujące czynności są wymagane aplikacja może pomyślnie połączyć się z usługi sieci Web aplikacji mobilnej:

- Zobacz temat sprzęt i wymagania dotyczące oprogramowania dla wymagania dotyczące sprzętu i oprogramowania
- Należy używać 6.0 lub nowszym serwera uwierzytelnianie wieloskładnikowe Azure
- Usługi sieci Web aplikacji mobilnej musi być zainstalowana na serwerze sieci web z Internetu systemem Microsoft® Internet Information Services (IIS) usług IIS 7.x lub nowszy.  Aby uzyskać więcej informacji na temat usług IIS zobacz [IIS.NET](http://www.iis.net/).
- Upewnij się, ASP.NET v4.0.30319 jest zainstalowany, zarejestrowanych i dozwolonych
- Usługi roli wymagane obejmują ASP.NET i zgodność z metabazą usług IIS 6
- Usługi sieci Web aplikacji mobilnej muszą być dostępne za pośrednictwem publicznego adresu URL
- Usługi sieci Web aplikacji mobilnej muszą być zabezpieczone przy użyciu certyfikatu SSL.
- Zestaw SDK usługi sieci Web uwierzytelnianie wieloskładnikowe Azure musi być zainstalowana w programie IIS 7.x lub nowszy na serwerze który serwer uwierzytelniania wieloskładnikowego Azure
- Zestaw SDK usługi sieci Web uwierzytelnianie wieloskładnikowe Azure muszą być zabezpieczone przy użyciu certyfikatu SSL.
- Usługi sieci Web aplikacji mobilnej muszą mieć możliwość nawiązywania połączenia z zestawu SDK usługi sieci Web uwierzytelnianie wieloskładnikowe Azure przez SSL
- Usługi sieci Web aplikacji mobilnej muszą mieć możliwość uwierzytelnienia do Azure wieloskładnikowe uwierzytelniania sieci Web usługi SDK przy użyciu poświadczeń konta usługi, który jest członkiem grupy zabezpieczeń o nazwie "PhoneFactor administratorów". To konto usługi i grupy istnieje w usłudze Active Directory, jeśli serwer uwierzytelnianie wieloskładnikowe Azure działa na serwerze domeny. Na serwerze uwierzytelnianie wieloskładnikowe Azure istnieje lokalnie tego konta usługi i grupy, ponieważ nie jest dołączony do domeny.


Instalowanie portalu użytkownika na serwerze niż serwer uwierzytelniania wieloskładnikowego Azure wymaga trzech kroków:

1. Instalowanie usługi sieci web SDK
2. Instalowanie aplikacji dla urządzeń przenośnych usługi sieci web
3. Konfigurowanie ustawień aplikacji dla urządzeń przenośnych na serwerze uwierzytelnianie wieloskładnikowe Azure
4. Aktywowanie aplikacji uwierzytelnianie wieloskładnikowe Azure dla użytkowników końcowych

## <a name="install-the-web-service-sdk"></a>Instalowanie usługi sieci web SDK

Jeśli zestaw SDK usługi sieci Web uwierzytelnianie wieloskładnikowe Azure nie jest już zainstalowany na serwerze uwierzytelnianie wieloskładnikowe Azure, przejdź do tego serwera i Otwórz serwer uwierzytelniania wieloskładnikowego Azure. Kliknij ikonę SDK usługi sieci Web, kliknij pozycję SDK Instalowanie usługi sieci Web... przycisk i postępuj zgodnie z instrukcjami prezentowane. Muszą być zabezpieczone SDK usługi sieci Web przy użyciu certyfikatu SSL. Certyfikat z podpisem własnym jest poprawny w tym celu, ale musi być importowane do magazynu "Zaufane główne urzędy certyfikacji" komputera lokalnego konta na serwerze sieci web portalu użytkownika tak, aby zaufała ten certyfikat podczas inicjowania połączenia SSL.

<center>![Konfiguracja](./media/multi-factor-authentication-get-started-server-webservice/sdk.png)</center>

## <a name="install-the-mobile-app-web-service"></a>Instalowanie aplikacji dla urządzeń przenośnych usługi sieci web
Przed zainstalowaniem aplikacji dla urządzeń przenośnych usługi sieci web, należy pamiętać o następujących czynności:

- Jeśli Portal użytkownika uwierzytelnianie wieloskładnikowe Azure jest już zainstalowany na serwerze z Internetu, nazwę użytkownika, hasło i adres URL do zestawu SDK usługi sieci Web mogą być kopiowane z pliku web.config portalu użytkownika.
- Warto Otwórz przeglądarkę sieci web na serwerze sieci web z Internetu i przejdź do adresu URL wprowadzony w pliku web.config SDK usługi sieci Web. Jeśli w przeglądarce można uzyskać dostęp do usługi sieci web pomyślnie, jej możesz Monituj o poświadczenia. Wprowadź nazwę użytkownika i hasło, które zostały wprowadzone w pliku web.config dokładnie tak, jak wygląda w pliku. Upewnij się, są wyświetlane nie ostrzeżenia certyfikatu lub błędy.
- Jeśli odwrotnej serwera proxy lub zapory znajduje się przed serwer sieci web usługi sieci Web aplikacji mobilnej i wykonywania odciążanie protokołu SSL, można edytować plik web.config usługi sieci Web aplikacji mobilnej i Dodaj następujący klucz do <appSettings> sekcji tak, aby usługi sieci Web aplikacji mobilnej zamiast https, można użyć protokołu http. Jednak SSL jest nadal potrzebny w aplikacji Mobile do serwera proxy zapory i odwrotnie. <add key="SSL_REQUIRED" value="false"/>

### <a name="to-install-the-mobile-app-web-service"></a>Aby zainstalować usługę sieci web aplikacji dla urządzeń przenośnych

<ol>
<li>Otwórz Eksploratora Windows na serwerze uwierzytelnianie wieloskładnikowe Azure i przejdź do folderu, w którym zainstalowano serwer uwierzytelniania wieloskładnikowego Azure (przykład C:\Program Files\Azure uwierzytelnianie wieloskładnikowe). Wybór 32-bitowej lub 64-bitowej wersji pakietu AuthenticationPhoneAppWebServiceSetup wieloskładnikowe Azure instalacji odpowiednio dla serwera usługi sieci Web aplikacji mobilnej zostanie zainstalowana na. Skopiuj plik instalacji na serwerze w Internecie.</li>

<li>Na serwerze sieci web w Internecie to plik Instalatora muszą być uruchamiane z uprawnieniami administratora. Najprostszym sposobem na tym jest Otwórz wiersz polecenia jako administrator i przejdź do lokalizacji, w której został skopiowany plik instalacji.</li>  

<li>Uruchom plik instalacji AuthenticationMobileAppWebServiceSetup wieloskładnikowe, w razie potrzeby zmienić witryny i zmień katalogów wirtualnych na krótką nazwę, takie jak "PA". Nazwa krótki katalogu wirtualnego jest to zalecane, ponieważ użytkownik musi wprowadzić adres URL usługi Mobile aplikacji sieci Web do urządzenia przenośnego w trakcie procesu aktywacji.</li>

<li>Po zakończeniu instalacji AuthenticationMobileAppWebServiceSetup wieloskładnikowe Azure, przejdź do C:\inetpub\wwwroot\PA (lub odpowiednim katalogu na podstawie nazwy katalogu wirtualnego) i edytowania pliku web.config.</li>  

<li>Odszukaj klawiszy WEB_SERVICE_SDK_AUTHENTICATION_USERNAME i WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD i ustaw wartości na nazwę użytkownika i hasło konta usługi, które należy do grupy zabezpieczeń Administratorzy PhoneFactor grupy (zobacz sekcję wymagania powyżej). Może to być to samo konto używane z tożsamością Portal użytkownika uwierzytelnianie wieloskładnikowe Azure który był już zainstalowany. Pamiętaj wprowadzić nazwę użytkownika i hasło między znaki cudzysłowu na końcu wiersza (wartość = "" / >). Zaleca się używanie kwalifikowaną nazwę użytkownika (na przykład domena\nazwa_użytkownika lub machine\username).</li>  

<li>Znajdź ustawienie aplikacji sieci Web Service_pfwssdk_PfWsSdk pfMobile i zmień wartość "http://localhost:4898/PfWsSdk.asmx" do określonego adresu URL SDK usługi sieci Web jest uruchomiony na serwerze uwierzytelnianie wieloskładnikowe Azure (np. https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx). Ponieważ SSL jest używany dla tego połączenia, ponieważ certyfikat SSL zostanie wystawiony nazwa serwera i adres URL używany musi odpowiadać nazwie certyfikatu musi odwoływać się SDK usługi sieci Web, nazwa serwera, a nie adres IP. Jeśli nazwa serwera nie rozwiąże adresu IP na serwerze z Internetu, należy dodać wpis do pliku hosts na tym serwerze do zamapować nazwa serwera uwierzytelnianie wieloskładnikowe Azure na adres IP. Zapisz plik web.config, gdy zostaną wprowadzone zmiany.</li>  

<li>Jeśli witryny sieci Web, czy usługi sieci Web aplikacji mobilnej została zainstalowana w obszarze (np. Domyślna witryna sieci Web) nie został już binded z certyfikatem publicznie podpisane, zainstalować certyfikat na serwerze Jeśli nie jest już zainstalowany, otwórz Menedżera IIS i powiązać certyfikat z witryny sieci Web.</li>  

<li>Otwórz przeglądarkę sieci web z dowolnego komputera i przejdź do adresu URL, w którym zainstalowano usługi sieci Web aplikacji mobilnej (np. https://www.publicwebsite.com/PA). Upewnij się, są wyświetlane nie ostrzeżenia certyfikatu lub błędy.</li>

### <a name="configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>Konfigurowanie ustawień aplikacji dla urządzeń przenośnych na serwerze uwierzytelnianie wieloskładnikowe Azure
Teraz, gdy jest zainstalowana usługa sieci web aplikacji dla urządzeń przenośnych, trzeba skonfigurować serwer uwierzytelniania wieloskładnikowego Azure do pracy z portalem.

#### <a name="to-configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>Aby skonfigurować ustawienia aplikacji dla urządzeń przenośnych na serwerze uwierzytelnianie wieloskładnikowe Azure

1. W polu Serwer uwierzytelnianie wieloskładnikowe Azure kliknij ikonę portalu użytkownika. Jeśli użytkownicy mogą kontrolować ich metody uwierzytelniania na karcie Ustawienia w obszarze Zezwalaj użytkownikom na wybieranie metody, sprawdź aplikacji Mobile. Bez ta funkcja jest włączona użytkownicy końcowi będą musieli skontaktować się z pomoc techniczna do ukończenia aktywacji dla aplikacji urządzeń przenośnych.
2. Zaznacz pozycję Zezwalaj użytkownikom, aby aktywować pole w aplikacji Mobile.
3. Zaznacz pole Zezwalaj na rejestrowania użytkownika.
4. Kliknij ikonę aplikacji Mobile.
5. Wprowadź adres URL używany z katalogu wirtualnego, który został utworzony podczas instalowania AuthenticationMobileAppWebServiceSetup wieloskładnikowe Azure. W tym miejscu można wprowadzić nazwę konta. Ta nazwa firmy są wyświetlane w aplikacji mobilnej. Jeśli puste, pojawi się nazwa dostawcy uwierzytelnianie wieloskładnikowe utworzony w portalu zarządzania Azure.



<center>![Konfiguracja](./media/multi-factor-authentication-get-started-server-webservice/mobile.png)</center>
