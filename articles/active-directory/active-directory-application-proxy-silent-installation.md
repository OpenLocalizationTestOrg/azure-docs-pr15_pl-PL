<properties
    pageTitle="Jak bezgłośną zainstalować łącznik serwera Proxy aplikacji Azure AD | Microsoft Azure"
    description="Opisano, jak wykonać instalację odbiorcze Azure AD aplikacji serwera Proxy łącznika bezpieczny dostęp zdalny do aplikacji lokalnej."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="how-to-silently-install-the-azure-ad-application-proxy-connector"></a>Jak zainstalować bezgłośną Azure AD łącznik serwera Proxy aplikacji

Chcesz mieć możliwość wysyłania skrypt instalacji na wielu serwerach systemu Windows lub na serwerach systemu Windows, które nie mają włączony interfejs użytkownika. W tym temacie wyjaśniono, jak utworzyć skrypt programu Windows PowerShell, który umożliwia instalacji zainstalować i zarejestrować wybrany łącznik Azure AD aplikacji serwera Proxy.

## <a name="enabling-access"></a>Włączanie dostępu
Serwer Proxy aplikacji polega na instalowanie cienki usługi Windows Server o nazwie łącznika w Twojej sieci. Dla łącznika serwera Proxy aplikacji do pracy go musi być zarejestrowana w katalogu Azure AD przy użyciu administratora globalnego i hasła. Zwykle zostanie wprowadzona podczas instalacji łącznika w podręcznym okno dialogowe. Możesz też programu Windows PowerShell umożliwia tworzenie obiektu poświadczeń, aby wprowadzić informacje o rejestracji lub można tworzyć własne token i używać go do wprowadź informacje o rejestracji.

## <a name="step-1--install-the-connector-without-registration"></a>Krok 1: Instalowanie łącznik bez rejestracji


Zainstaluj msi łącznik bez rejestrowania łącznika w następujący sposób:


1. Otwórz wiersz polecenia.
2. Uruchom następujące polecenie, w którym/q oznacza cichy Instalacja — instalacja nie poprosi o Zaakceptuj Umowę licencyjną użytkownika końcowego.

        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

## <a name="step-2-register-the-connector-with-azure-active-directory"></a>Krok 2: Zarejestrować łącznik usługi Azure Active Directory
Można to osiągnąć przy użyciu jednej z następujących metod:


- Zarejestruj się łącznik za pomocą obiektu poświadczeń programu Windows PowerShell
- Zarejestruj się łącznik za pomocą tokenu utworzony w trybie offline

### <a name="register-the-connector-using-a-windows-powershell-credential-object"></a>Zarejestruj się łącznik za pomocą obiektu poświadczeń programu Windows PowerShell


1. Tworzenie obiektu poświadczeń systemu Windows PowerShell, uruchamiając następujące polecenie, gdzie "<username>"i"<password>" zastępuje się za pomocą nazwy użytkownika i hasła dla katalogu:

        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword

2. Przejdź do **Łącznik C:\Program Files\Microsoft AAD aplikacji serwera Proxy** i uruchomić skrypt za pomocą utworzony obiekt poświadczeń programu PowerShell, gdzie $cred to nazwa programu PowerShell poświadczeń utworzony obiekt:

        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred


### <a name="register-the-connector-using-a-token-created-offline"></a>Zarejestruj się łącznik za pomocą tokenu utworzony w trybie offline

1. Tworzenie token trybu offline za pomocą klasy AuthenticationContext, używając wartości wstawkę kodu:


        using System;
        using System.Diagnostics;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

        class Program
        {
        #region constants
        /// <summary>
        /// The AAD authentication endpoint uri
        /// </summary>
        static readonly Uri AadAuthenticationEndpoint = new Uri("https://login.windows.net/common/oauth2/token?api-version=1.0");

        /// <summary>
        /// The application ID of the connector in AAD
        /// </summary>
        static readonly string ConnectorAppId = "55747057-9b5d-4bd4-b387-abf52a8bd489";

        /// <summary>
        /// The reply address of the connector application in AAD
        /// </summary>
        static readonly Uri ConnectorRedirectAddress = new Uri("urn:ietf:wg:oauth:2.0:oob");

        /// <summary>
        /// The AppIdUri of the registration service in AAD
        /// </summary>
        static readonly Uri RegistrationServiceAppIdUri = new Uri("https://proxy.cloudwebappproxy.net/registerapp");

        #endregion

        #region private members
        private string token;
        private string tenantID;
        #endregion

        public void GetAuthenticationToken()
        {
            AuthenticationContext authContext = new AuthenticationContext(AadAuthenticationEndpoint.AbsoluteUri);

            AuthenticationResult authResult = authContext.AcquireToken(RegistrationServiceAppIdUri.AbsoluteUri,
                ConnectorAppId,
                ConnectorRedirectAddress,
                PromptBehavior.Always);

            if (authResult == null || string.IsNullOrEmpty(authResult.AccessToken) || string.IsNullOrEmpty(authResult.TenantId))
            {
                Trace.TraceError("Authentication result, token or tenant id returned are null");
                throw new InvalidOperationException("Authentication result, token or tenant id returned are null");
            }

            token = authResult.AccessToken;
            tenantID = authResult.TenantId;
        }





2. Po umieszczeniu tokenu utworzyć SecureString przy użyciu tokenu: <br>
`$SecureToken = $Token | ConvertTo-SecureString -AsPlainText -Force`
3. Uruchom następujące polecenia środowiska Windows PowerShell, gdzie SecureToken to nazwa tokenu utworzoną wcześniej, a tenantID jest identyfikator GUID dzierżawcy usługi: <br>
`RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Token -Token $SecureToken -TenantId <tenant GUID>`



## <a name="see-also"></a>Zobacz też

- [Włączanie serwera Proxy aplikacji dla usługi Azure Active Directory](active-directory-application-proxy-enable.md)
- [Publikowanie aplikacji przy użyciu własnej nazwy domeny](active-directory-application-proxy-custom-domains.md)
- [Włączyć logowania jednokrotnego](active-directory-application-proxy-sso-using-kcd.md)
- [Rozwiązywanie problemów, jakie masz z serwera Proxy aplikacji](active-directory-application-proxy-troubleshoot.md)

Aby uzyskać najnowsze informacje i aktualizacje zapoznaj się z [serwera Proxy aplikacji blogu](http://blogs.technet.com/b/applicationproxyblog/)
