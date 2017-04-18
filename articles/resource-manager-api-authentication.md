<properties 
   pageTitle="Uwierzytelnianie usługi Active Directory i Menedżera zasobów | Microsoft Azure"
   description="Przewodnik po Deweloper uwierzytelnianie za pomocą interfejsu API Menedżera zasobów Azure i usługi Active Directory dla Integrowanie aplikacji z innych Azure subskrypcji."
   services="azure-resource-manager,active-directory"
   documentationCenter="na"
   authors="dushyantgill"
   manager="timlt"
   editor="tysonn" />
<tags 
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/31/2016"
   ms.author="dugill;tomfitz" />

# <a name="how-to-use-azure-active-directory-and-resource-manager-to-manage-a-customers-resources"></a>Jak używać usługi Azure Active Directory i Menedżera zasobów do zarządzania zasobami klienta

## <a name="introduction"></a>Wprowadzenie

Jeśli programiści, którzy będą tworzyli aplikację, która zarządza zasobami Azure klienta, w tym temacie przedstawiono sposób uwierzytelniania za pośrednictwem interfejsów API Menedżera zasobów Azure i uzyskać dostęp do zasobów w innych subskrypcji. 

Aplikacji można uzyskać dostęp do interfejsów API Menedżera zasobów na kilka sposobów:

1. **Użytkownik + aplikacji programu access**: w przypadku aplikacji mających dostęp do zasobów w imieniu zalogowany użytkownik. Ta metoda działa w przypadku aplikacji, takich jak aplikacje sieci web i narzędzi wiersza polecenia, które zajmują się tylko "interakcyjnych zarządzania" Azure zasobów.
1. **Tylko do aplikacji programu access**: w przypadku aplikacji uruchamianych demon usług i zaplanowanych zadań. Tożsamość aplikacji otrzymuje bezpośredni dostęp do tych zasobów. Ta metoda działa w przypadku aplikacji, które wymagają długoterminowe "dostęp w trybie offline" Azure.

Ten temat zawiera instrukcje krok po kroku, aby utworzyć aplikację, której użyto obie te metody autoryzacji. Jak wykonać poszczególne kroki z interfejsu API usługi REST lub C# jest wyświetlany. Zakończenie aplikacji ASP.NET MVC jest dostępna w [https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense](https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense).

Cały kod w tym temacie jest uruchomiona jako aplikacji sieci web, które można wypróbować na [http://vipswapper.azurewebsites.net/cloudsense](http://vipswapper.azurewebsites.net/cloudsense). 

## <a name="what-the-web-app-does"></a>Co oznacza aplikacji sieci web

Aplikacji sieci web:

1. Znaki w Azure użytkownika.
2. Zapytanie użytkownika do udzielania dostępu aplikacji sieci web do Menedżera zasobów.
3. Otrzymuje użytkownika + token dostępu aplikacji do uzyskiwania dostępu do Menedżera zasobów.
4. Używa tokenu (w kroku 3), aby połączenie Menedżera zasobów i przypisać wystawcy usługi aplikacji do roli w subskrypcji, która zapewnia dostęp długoterminowe aplikacji z subskrypcją.
5. Otrzymuje token dostępu tylko do aplikacji.
6. Używa tokenu (z kroku 5) w celu zarządzania zasobami subskrypcji za pomocą Menedżera zasobów.

Oto przepływu zakończenia do końca aplikacji sieci web.

![Przepływ uwierzytelniania Menedżera zasobów](./media/resource-manager-api-authentication/Auth-Swim-Lane.png)

Dla subskrypcji, który ma być wyświetlany jako użytkownik, podane identyfikator subskrypcji:

![Podaj identyfikator subskrypcji](./media/resource-manager-api-authentication/sample-ux-1.png)

Wybierz konto, aby użyć do logowania.

![Wybierz konto](./media/resource-manager-api-authentication/sample-ux-2.png)

Podaj swoje poświadczenia.

![poświadczenia](./media/resource-manager-api-authentication/sample-ux-3.png)

Udzielanie dostępu aplikacji do subskrypcji Azure:
 
![Udzielanie dostępu](./media/resource-manager-api-authentication/sample-ux-4.png)
 
Zarządzanie subskrypcjami połączenia:

![Łączenie subskrypcji](./media/resource-manager-api-authentication/sample-ux-7.png)


## <a name="register-application"></a>Zarejestruj się w aplikacji

Przed rozpoczęciem kodowania Zarejestruj się w aplikacji sieci web z Azure Active Directory (AD). Rejestracja aplikacji tworzy centralnej tożsamości dla aplikacji w Azure AD. Przechowuje podstawowe informacje o aplikacji, takich jak identyfikator klienta OAuth, odpowiedź adresy URL i poświadczenia używane do uwierzytelniania i uzyskać dostęp do interfejsów API Menedżera zasobów Azure aplikacji. Rejestracja aplikacji rejestruje także różne delegowanych uprawnień, które aplikacja wymaga podczas uzyskiwania dostępu do programu Microsoft APIs w imieniu użytkownika. 

Ponieważ aplikacji uzyskuje dostęp do innej subskrypcji, musisz skonfigurować go jako aplikacja wielu dzierżawy. Aby przekazać sprawdzanie poprawności, podaj domeny skojarzone z usługi Active Directory. Aby wyświetlić domen skojarzone z usługi Active Directory, zaloguj się do [portalu klasyczny](https://manage.windowsazure.com). Wybierz pozycję usługi Active Directory, a następnie wybierz pozycję **domeny**.

W poniższym przykładzie pokazano, jak zarejestrować aplikacji przy użyciu programu PowerShell Azure. Musisz mieć najnowszej wersji (sierpień 2016) Azure programu PowerShell dla tego polecenia do pracy. 

    $app = New-AzureRmADApplication -DisplayName "{app name}" -HomePage "https://{your domain}/{app name}" -IdentifierUris "https://{your domain}/{app name}" -Password "{your password}" -AvailableToOtherTenants $true
    
Aby zalogować się jako aplikacja AD, potrzebujesz aplikacji id i hasło. Aby wyświetlić identyfikator aplikacji, który został zwrócony z poprzedniego polecenia, należy użyć:

    $app.ApplicationId

W poniższym przykładzie pokazano, jak zarejestrować aplikację za pomocą interfejsu wiersza polecenia Azure. 

    azure ad app create --name {app name} --home-page https://{your domain}/{app name} --identifier-uris https://{your domain}/{app name} --password {your password} --available true

Wyniki obejmują identyfikator aplikacji, która jest potrzebna podczas uwierzytelniania jako aplikacja.

### <a name="optional-configuration---certificate-credential"></a>Konfiguracja opcjonalne — poświadczenia certyfikatu

Azure AD obsługuje także certyfikat poświadczeń dla aplikacji: Tworzenie certyfikatu podpisem, zachowaj klucz prywatny i Dodaj klucz publiczny do rejestracji aplikacji Azure AD. W przypadku uwierzytelniania aplikacji wysyła małych ładunku do Azure AD zalogowani przy użyciu klucz prywatny, a Azure AD sprawdza podpis za pomocą klucza publicznego, który zostanie zarejestrowany.

Dla informacji na temat tworzenia aplikacji AD przy użyciu certyfikatu zobacz [Używanie Azure programu PowerShell do utworzenia usługi kapitału dostępu do zasobów](resource-group-authenticate-service-principal.md#create-service-principal-with-certificate) lub [Użyj Azure interfejsu wiersza polecenia do utworzenia usługi kapitału dostępu do zasobów](resource-group-authenticate-service-principal-cli.md#create-service-principal-with-certificate).

## <a name="get-tenant-id-from-subscription-id"></a>Pobieranie identyfikatora dzierżawy z identyfikator subskrypcji

Aby zażądać tokenu, który może służyć do połączeń Menedżera zasobów, aplikacja musi wiedzieć identyfikator dzierżawy dzierżawy Azure AD obsługującego Azure subskrypcji. Najprawdopodobniej wiedzieć użytkownicy identyfikatorami subskrypcji, ale nie będzie wiadomo, ich dzierżawy identyfikatorów dla usługi Active Directory. Aby uzyskać użytkownik dzierżawy, poproś użytkownika identyfikator subskrypcji. Wprowadź identyfikator tej subskrypcji, wysyłając wniosek o subskrypcji:

    https://management.azure.com/subscriptions/{subscription-id}?api-version=2015-01-01

Żądanie zakończy się niepowodzeniem, ponieważ użytkownik nie jest zalogowany jeszcze, ale możesz pobrać identyfikator dzierżawy na odpowiedź. W tym wyjątku pobrać identyfikator dzierżawy z wartość nagłówka odpowiedzi dla **Uwierzytelnienia WWW**. Zobacz tej implementacji metody [GetDirectoryForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L20) .

## <a name="get-user--app-access-token"></a>Uzyskiwanie użytkownika + token dostępu aplikacji

Aplikacja przekierowuje użytkownika do Azure AD przy użyciu OAuth 2.0 autoryzować żądanie — do uwierzytelniania poświadczeń użytkownika oraz wrócić kodu autoryzacji. Aplikacja używa kodu autoryzacji uzyskanie dostępu do tokenu dla Menedżera zasobów. Metoda [ConnectSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/Controllers/HomeController.cs#L42) tworzy żądanie autoryzacji.

W tym temacie przedstawiono żądania interfejsu API usługi REST do uwierzytelnienia użytkownika. Za pomocą bibliotek Pomocnik do uwierzytelniania w kodzie. Aby uzyskać więcej informacji na temat tych bibliotek zobacz [Azure Active Directory uwierzytelniania bibliotek](./active-directory/active-directory-authentication-libraries.md). Aby uzyskać wskazówki dotyczące integracji Zarządzanie tożsamościami w aplikacji zobacz [Przewodnik programisty usługi Azure Active Directory](./active-directory/active-directory-developers-guide.md).

### <a name="auth-request-oauth-20"></a>Żądanie uwierzytelniania (OAuth 2.0)

Problemu Otwórz identyfikator Połącz-OAuth2.0 autoryzować żądanie do punktu końcowego Azure AD Autoryzuj:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize

Parametry ciągu kwerendy, które są dostępne dla tego żądania opisano w temacie [żądanie kodu autoryzacji](./active-directory/active-directory-protocols-oauth-code.md#request-an-authorization-code) .

W poniższym przykładzie pokazano, jak zażądać autoryzacji OAuth2.0:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=query&response_type=code&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&domain_hint=live.com

Azure AD uwierzytelnia użytkownika, a następnie w razie potrzeby z monitem o udzielić odpowiednich uprawnień do aplikacji. Zwraca kod autoryzacji do adresu URL odpowiedzi aplikacji. W zależności od żądanego response_mode, Azure AD albo odsyła dane w ciągu kwerendy lub jako wpisu danych.

    code=AAABAAAAiL****FDMZBUwZ8eCAA&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="auth-request-open-id-connect"></a>Żądanie uwierzytelniania (Otwórz połączyć identyfikator)

Jeśli nie tylko chcesz uzyskać dostęp do Menedżera zasobów Azure w imieniu użytkownika, ale również umożliwia użytkownikowi logowanie się do aplikacji przy użyciu swojego konta Azure AD, problemów, otwórz identyfikator autoryzować żądania połączenia. Z otwartej połączyć identyfikator aplikacja otrzymuje id_token z Azure AD używaną przez aplikację do zalogowania się użytkownika.

Parametry ciągu kwerendy, które są dostępne dla tego żądania opisano w temacie [wysłać żądanie logowania](./active-directory/active-directory-protocols-openid-connect-code.md#send-the-sign-in-request) .

Na przykład połączyć identyfikator Otwórz zaproszenie do wymiany jest:

     https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=form_post&response_type=code+id_token&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&scope=openid+profile&nonce=63567Dc4MDAw&domain_hint=live.com&state=M_12tMyKaM8

Azure AD uwierzytelnia użytkownika, a następnie w razie potrzeby z monitem o udzielić odpowiednich uprawnień do aplikacji. Zwraca kod autoryzacji do adresu URL odpowiedzi aplikacji. W zależności od żądanego response_mode, Azure AD albo odsyła dane w ciągu kwerendy lub jako wpisu danych.

Przykład otwórz połączyć identyfikator odpowiedzi jest:

    code=AAABAAAAiL*****I4rDWd7zXsH6WUjlkIEQxIAA&id_token=eyJ0eXAiOiJKV1Q*****T3GrzzSFxg&state=M_12tMyKaM8&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="token-request-oauth20-code-grant-flow"></a>Żądania tokenu (OAuth2.0 kod przepływ Udziel)

Teraz, gdy aplikacja otrzymała kod autoryzacji z usługi Azure Active Directory, nadszedł czas uzyskanie dostępu do tokenu dla Menedżera zasobów Azure.  Publikowanie OAuth2.0 kod udzielanie Token żądanie Azure AD Token punkt końcowy: 

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

Parametry ciągu kwerendy, które są dostępne dla tego żądania opisano w temacie [przy użyciu kodu autoryzacji](./active-directory/active-directory-protocols-oauth-code.md#use-the-authorization-code-to-request-an-access-token) .

W poniższym przykładzie pokazano na żądanie kodu udzielanie token przy użyciu poświadczeń hasła:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Podczas pracy z poświadczeniami certyfikatu, utworzyć JSON Token sieci Web (JWT) i znak (RSA SHA256) przy użyciu klucza prywatnego poświadczeń certyfikatu aplikacji. Typy oświadczeń dla tokenu są wyświetlane w [oświadczeniach tokenu JWT](./active-directory/active-directory-protocols-oauth-code.md#jwt-token-claims). Aby informacje zobacz [Kod biblioteki Auth Active Directory (.NET)](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/blob/dev/src/ADAL.PCL.Desktop/CryptographyHelper.cs) do podpisania tokeny JWT potwierdzenia klienta.

Zobacz [Specyfikacja Otwórz połączyć identyfikator](http://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication) , aby uzyskać szczegółowe informacje na uwierzytelnianie klienta. 

W poniższym przykładzie pokazano żądanie tokenu udzielanie kod przy użyciu poświadczeń certyfikatu:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1
    
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012
    
    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbG*****Y9cYo8nEjMyA

Przykład odpowiedź tokenu udzielanie kodu: 

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039858","not_before":"1432035958","resource":"https://management.core.windows.net/","access_token":"eyJ0eXAiOiJKV1Q****M7Cw6JWtfY2lGc5A","refresh_token":"AAABAAAAiL9Kn2Z****55j-sjnyYgAA","scope":"user_impersonation","id_token":"eyJ0eXAiOiJKV*****-drP1J3P-HnHi9Rr46kGZnukEBH4dsg"}

#### <a name="handle-code-grant-token-response"></a>Uchwyt kod udzielanie token odpowiedzi

Pomyślną odpowiedź token zawiera (użytkownika + aplikacji) token dostępu dla Menedżera zasobów Azure. Aplikacja używa ten token dostępu, aby uzyskać dostęp do Menedżera zasobów w imieniu użytkownika. Ważności tokeny dostępu wydawany przez Azure AD jest godzinę. Prawdopodobnie aplikacji sieci web należy odnowić (użytkownika + aplikacji) token dostępu. Jeśli trzeba ją odnowić token dostępu, za pomocą aplikacji otrzyma w odpowiedzi token token odświeżania. Publikowanie OAuth2.0 Token żądanie Azure AD Token punkt końcowy: 

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

Parametry używane z żądania odświeżenia opisano odświeżania [token dostępu](./active-directory/active-directory-protocols-oauth-code.md#refreshing-the-access-tokens).

W poniższym przykładzie pokazano sposób używania odświeżanie token:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=refresh_token&refresh_token=AAABAAAAiL9Kn2Z****55j-sjnyYgAA&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Odświeżanie tokeny umożliwia uzyskiwanie nowych tokeny dostępu dla Menedżera zasobów Azure, nie są one odpowiednie dla dostęp w trybie offline przez aplikację. Czas użytkowania tokeny odświeżania jest ograniczona, i tokeny odświeżania są powiązane użytkownikowi. Jeśli użytkownik odchodzi z organizacji, aplikacji przy użyciu tokenu odświeżania traci dostęp. Ta metoda nie jest odpowiednie dla aplikacji, które są używane przez członkom zespołu Zarządzanie Azure zasobów.

## <a name="check-if-user-can-assign-access-to-subscription"></a>Sprawdzanie, jeśli użytkownik można przypisać dostęp do subskrypcji

Teraz aplikacja ma token, aby uzyskać dostęp do Menedżera zasobów Azure w imieniu użytkownika. Następnym krokiem jest łączenie aplikacji z subskrypcją. Po nawiązaniu połączenia aplikacji można zarządzać te subskrypcje, nawet wtedy, gdy użytkownik nie jest obecne (długoterminowe dostęp w trybie offline). 

Dla każdej subskrypcji nawiązywania połączeń [uprawnienia listy Menedżera zasobów](https://msdn.microsoft.com/library/azure/dn906889.aspx) interfejsu API w celu określenia, czy użytkownik ma uprawnienia zarządzania dla subskrypcji.

Metoda [UserCanManagerAccessForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L44) aplikacji przykładowych ASP.NET MVC wykonuje to połączenie.

W poniższym przykładzie pokazano, jak zażądać uprawnień użytkownika na subskrypcji. 83cfe939-2402-4581-b761-4f59b0a041e4 jest identyfikatorem subskrypcji.

    GET https://management.azure.com/subscriptions/83cfe939-2402-4581-b761-4f59b0a041e4/providers/microsoft.authorization/permissions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiLC***lwO1mM7Cw6JWtfY2lGc5A

Przykład odpowiedzi na uzyskanie uprawnień użytkownika dla subskrypcji jest:

    HTTP/1.1 200 OK

    {"value":[{"actions":["*"],"notActions":["Microsoft.Authorization/*/Write","Microsoft.Authorization/*/Delete"]},{"actions":["*/read"],"notActions":[]}]}

Uprawnienia interfejsu API zwraca wiele uprawnień. Wszystkie uprawnienia składa się z dozwolone akcje (Akcje) i niedozwolone akcje (notactions). W przypadku akcji na liście dozwolone akcje dowolnego uprawnień i nie znajdują się na liście notactions uprawnienie, użytkownik może wykonać tę akcję. **Microsoft.Authorization/RoleAssignments/Write** jest akcja zarządzania prawami którego udziela dostępu. Aplikacja musi przeanalizować wyniku uprawnienia, aby wyszukać odpowiednik regex, w tym ciągu akcji w menu Akcje, a notactions każdego uprawnienia.

## <a name="get-app-only-access-token"></a>Uzyskiwanie token dostępu tylko do aplikacji

Teraz wiesz, jeśli użytkownik można przypisać dostęp do Azure subskrypcji. Następne kroki są:

1. Przypisz odpowiednią rolę RBAC do tożsamości aplikacji dla tej subskrypcji.
2. Sprawdź poprawność przydziału dostęp przez wyszukiwanie aplikacji uprawnień dla tej subskrypcji lub uzyskiwanie dostępu do Menedżera zasobów przy użyciu tokenu tylko do aplikacji.
1. Rekord połączenia w strukturze danych aplikacji "połączonego subskrypcji" - utrzymuje identyfikator subskrypcji.

Przyjrzyjmy się dostępnym dokładniejsze w pierwszym kroku. Aby przypisać odpowiednią rolę RBAC tożsamości aplikacji, należy ustalić:

- Identyfikator aplikacji tożsamości w użytkownika usługi Azure Active Directory
- Identyfikator roli RBAC, które aplikacja wymaga subskrypcji

Gdy aplikacja uwierzytelnia użytkownika z Azure AD, tworzy obiektem głównej usługi aplikacji w tym Azure AD. Azure umożliwia role RBAC ma być przypisany do głównych usługi udzielenia bezpośredni dostęp do odpowiednich wniosków Azure zasobów. Ta akcja jest dokładnie Życzymy robić. Interfejs API Azure AD wykres zapytania, aby określić identyfikator wystawcy usługi aplikacji w zalogowany użytkownik przez Azure AD.

Wystarczy, że token dostępu dla Menedżera zasobów Azure — potrzebne nowe token dostępu, aby nawiązać połączenie z interfejsu API Azure AD wykresu. Każdej aplikacji w Azure AD ma uprawnienia do kwerendy własnej kapitału obiekt usługi, aby wystarcza token dostępu tylko do aplikacji.

<a id="app-azure-ad-graph">
### <a name="get-app-only-access-token-for-azure-ad-graph-api"></a>Uzyskać dostęp tylko do aplikacji token interfejsu API Azure AD wykresu

Do uwierzytelniania aplikacji i uzyskać token API Azure AD wykresu, zgłosić żądanie token przepływu OAuth2.0 udzielanie poświadczeń klienta do punktu końcowego tokenu Azure AD (**https://login.microsoftonline.com/ {directory_domain_name}-uwierzytelnianie OAuth2-Token**).

Metoda [GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs) ASP.net MVC przykładowej aplikacji otrzymuje dostęp tylko do aplikacji tokenu dla interfejsu API wykresu za pomocą Active Directory Authentication Library dla środowiska .NET.

Parametry ciągu kwerendy, które są dostępne dla tego żądania opisano w temacie [żądanie Token dostępu](./active-directory/active-directory-protocols-oauth-service-to-service.md#request-an-access-token) .

Żądanie przykład token udzielanie poświadczeń klienta: 

    POST https://login.microsoftonline.com/62e173e9-301e-423e-bcd4-29121ec1aa24/oauth2/token HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 187</pre>
    <pre>grant_type=client_credentials&client_id=a0448380-c346-4f9f-b897-c18733de9394&resource=https%3A%2F%2Fgraph.windows.net%2F &client_secret=olna8C*****Og%3D

Przykład odpowiedzi na token udzielanie poświadczeń klienta: 

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039862","not_before":"1432035962","resource":"https://graph.windows.net/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRv****G5gUTV-kKorR-pg"}

### <a name="get-objectid-of-application-service-principal-in-user-azure-ad"></a>Uzyskaj identyfikator obiektu głównej usługi aplikacji użytkownika Azure AD

Teraz umożliwia token dostępu tylko do aplikacji kwerendy [głównych usługi Azure AD wykresu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) interfejsu API do określania identyfikatora obiektu usługi aplikacji kapitału w katalogu.

Metoda [GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs#) ASP.net MVC przykładowej aplikacji wykonuje to połączenie.

W poniższym przykładzie pokazano, jak zażądać wystawcy usługi aplikacji. a0448380-c346-4f9f-b897-c18733de9394 jest identyfikator klienta aplikacji.

    GET https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/servicePrincipals?api-version=1.5&$filter=appId%20eq%20'a0448380-c346-4f9f-b897-c18733de9394' HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJK*****-kKorR-pg

W poniższym przykładzie pokazano główne odpowiedź na żądanie usługi aplikacji 

    HTTP/1.1 200 OK

    {"odata.metadata":"https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/$metadata#directoryObjects/Microsoft.DirectoryServices.ServicePrincipal","value":[{"odata.type":"Microsoft.DirectoryServices.ServicePrincipal","objectType":"ServicePrincipal","objectId":"9b5018d4-6951-42ed-8a92-f11ec283ccec","deletionTimestamp":null,"accountEnabled":true,"appDisplayName":"CloudSense","appId":"a0448380-c346-4f9f-b897-c18733de9394","appOwnerTenantId":"62e173e9-301e-423e-bcd4-29121ec1aa24","appRoleAssignmentRequired":false,"appRoles":[],"displayName":"CloudSense","errorUrl":null,"homepage":"http://www.vipswapper.com/cloudsense","keyCredentials":[],"logoutUrl":null,"oauth2Permissions":[{"adminConsentDescription":"Allow the application to access CloudSense on behalf of the signed-in user.","adminConsentDisplayName":"Access CloudSense","id":"b7b7338e-683a-4796-b95e-60c10380de1c","isEnabled":true,"type":"User","userConsentDescription":"Allow the application to access CloudSense on your behalf.","userConsentDisplayName":"Access CloudSense","value":"user_impersonation"}],"passwordCredentials":[],"preferredTokenSigningKeyThumbprint":null,"publisherName":"vipswapper"quot;,"replyUrls":["http://www.vipswapper.com/cloudsense","http://www.vipswapper.com","http://vipswapper.com","http://vipswapper.azurewebsites.net","http://localhost:62080"],"samlMetadataUrl":null,"servicePrincipalNames":["http://www.vipswapper.com/cloudsense","a0448380-c346-4f9f-b897-c18733de9394"],"tags":["WindowsAzureActiveDirectoryIntegratedApp"]}]}

### <a name="get-azure-rbac-role-identifier"></a>Uzyskaj identyfikator roli Azure RBAC

Aby przypisać odpowiednią rolę RBAC do głównej usługi, należy określić identyfikator roli Azure RBAC.

Prawy rola RBAC aplikacji:

- Jeśli aplikacja tylko monitoruje subskrypcji, bez wprowadzania zmian, wymaga tylko czytnika uprawnienia dla tej subskrypcji. Przypisywanie roli **czytnika** .
- Jeśli aplikacja zarządza Azure subskrypcji, tworzenie i modyfikowanie i usuwanie obiektów wymaga jednego z uprawnień współautorów.
  - Aby zarządzać określonego typu zasobów, przypisywanie ról współautorów specyficzne dla zasobów (maszyn wirtualnych współautorów, wirtualną sieć współautorów, współautorów konta miejsca do magazynowania itp.)
  - Aby zarządzać dowolny typ zasobu, przypisywanie roli **współautora** .

Przypisanie roli aplikacji jest widoczny dla użytkowników, dlatego wybierz wymagany najmniejszych uprawnień.

Zadzwoń do [definicji roli Menedżera zasobów interfejsu API](https://msdn.microsoft.com/library/azure/dn906879.aspx) lista wszystkich ról Azure RBAC i wyszukiwania, a następnie przejść przez wynik, aby znaleźć odpowiednią rolę według nazwy.

Metoda [GetRoleId](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L246) aplikacji przykładowych ASP.net MVC wykonuje to połączenie.

W poniższym przykładzie żądanie pokazano, jak uzyskać identyfikator roli Azure RBAC. 09cbd307-aa71-4aca-b346-5f253e6e3ebb jest identyfikatorem subskrypcji.

    GET https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV*****fY2lGc5

Odpowiedź znajduje się w następującym formacie: 

    HTTP/1.1 200 OK

    {"value":[{"properties":{"roleName":"API Management Service Contributor","type":"BuiltInRole","description":"Lets you manage API Management services, but not access to them.","scope":"/","permissions":[{"actions":["Microsoft.ApiManagement/Services/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/312a565d-c81f-4fd8-895a-4e21e48d571c","type":"Microsoft.Authorization/roleDefinitions","name":"312a565d-c81f-4fd8-895a-4e21e48d571c"},{"properties":{"roleName":"Application Insights Component Contributor","type":"BuiltInRole","description":"Lets you manage Application Insights components, but not access to them.","scope":"/","permissions":[{"actions":["Microsoft.Insights/components/*","Microsoft.Insights/webtests/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/ae349356-3a1b-4a5e-921d-050484c6347e","type":"Microsoft.Authorization/roleDefinitions","name":"ae349356-3a1b-4a5e-921d-050484c6347e"}]}

Nie musisz wywołania tego interfejsu API na bieżąco. Po po określeniu znanego GUID definicji roli, można utworzyć identyfikator definicji roli jako:

    /subscriptions/{subscription_id}/providers/Microsoft.Authorization/roleDefinitions/{well-known-role-guid}

Poniżej przedstawiono znane identyfikatory GUID często używane wbudowane ról:

| Rola | Identyfikator GUID |
| ----- | ------ |
| Czytnik | acdd72a7-3385-48ef-bd42-f606fba81ae7
| Trybu współautora | b24988ac-6180-42a0-ab88-20f7382dd24c
| Maszyn wirtualnych trybu współautora | d73bb868-a0df-4d4d-bd69-98a00b01fccb
| Wirtualna sieć współautorów | b34d265f-36f7-4a0d-a4d4-e158ca92e90f
| Współautor konta miejsca do magazynowania | 86e8f5dc-a6e9-4c67-9d15-de283e8eac25
| Współautor witryny sieci Web | de139f84-1756-47ae-9be6-808fbbe84772
| Współautor Plan sieci Web | 2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b
| Program SQL Server współautorów | 6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437
| Współautor bazy danych SQL | 9b7fa17d-e63e-47B0-bb0a-15c516ac86ec

### <a name="assign-rbac-role-to-application"></a>Przypisanie roli RBAC aplikacji

Masz wszystko należy przypisać odpowiednią rolę RBAC do głównej usługi przy użyciu [Menedżera zasobów tworzenie przypisanie roli](https://msdn.microsoft.com/library/azure/dn906887.aspx) interfejsu API.

Metoda [GrantRoleToServicePrincipalOnSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L170) aplikacji przykładowych ASP.net MVC wykonuje to połączenie.

Na przykład zaproszenie do wymiany przypisanie roli RBAC aplikacji: 

    PUT https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/microsoft.authorization/roleassignments/4f87261d-2816-465d-8311-70a27558df4c?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiL*****FlwO1mM7Cw6JWtfY2lGc5
    Content-Type: application/json
    Content-Length: 230

    {"properties": {"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2"}}

W wezwaniu na używane są następujące wartości:

| Identyfikator GUID | Opis |
| ------ | --------- |
| 09cbd307-aa71-4aca-b346-5f253e6e3ebb | Identyfikator subskrypcji
| c3097b31-7309-4c59-b4e3-770f8406bad2 | Identyfikator wystawcy usługi aplikacji
| acdd72a7-3385-48ef-bd42-f606fba81ae7 | Identyfikator roli reader
| 4f87261d-2816-465d-8311-70a27558df4c | Nowy identyfikator guid utworzoną dla nowe przypisanie roli

Odpowiedź znajduje się w następującym formacie: 

    HTTP/1.1 201 Created

    {"properties":{"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2","scope":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb"},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleAssignments/4f87261d-2816-465d-8311-70a27558df4c","type":"Microsoft.Authorization/roleAssignments","name":"4f87261d-2816-465d-8311-70a27558df4c"}

### <a name="get-app-only-access-token-for-azure-resource-manager"></a>Uzyskiwanie dostępu tylko do aplikacji tokenu dla Menedżera zasobów Azure

Aby sprawdzić poprawność tej aplikacji ma odpowiednie dostęp dla tej subskrypcji, w przypadku zadań test subskrypcji przy użyciu tokenu tylko do aplikacji.

Aby uzyskać dostęp tylko do aplikacji token, wykonaj instrukcje z sekcji [dostęp tylko do aplikacji tokenu dla interfejsu API Azure AD wykresu](#app-azure-ad-graph), podając inną wartość parametru zasobu: 

    https://management.core.windows.net/

Metoda [ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) ASP.NET MVC przykładowej aplikacji uzyskuje dostęp tylko do aplikacji tokenu dla Menedżera zasobów Azure za pomocą Active Directory Authentication Library dla środowiska .net.

#### <a name="get-applications-permissions-on-subscription"></a>Uzyskiwanie uprawnień aplikacji dla subskrypcji

Aby sprawdzić, czy aplikacja ma odpowiednią dostęp na subskrypcji usługi Azure, można również zadzwonić [Uprawnienia menedżera zasobów](https://msdn.microsoft.com/library/azure/dn906889.aspx) interfejsu API. Ta metoda jest podobne do jak można stwierdzić, czy użytkownik ma prawa Zarządzanie dostępem dla subskrypcji. Jednak tym razem połączeń uprawnienia interfejsu API przy użyciu tokenu dostępu tylko do aplikacji, otrzymany w poprzednim kroku.

Metoda [ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) aplikacji przykładowych ASP.NET MVC wykonuje to połączenie.

## <a name="manage-connected-subscriptions"></a>Zarządzanie subskrypcjami połączonego

Odpowiednią rolę RBAC przypisany do głównej w subskrypcji usługi aplikacji, aplikacja może zachować monitorowania i zarządzania tokeny dostępu tylko do aplikacji dla Menedżera zasobów Azure.

Tylko właściciel subskrypcji usuwa przypisanie roli aplikacji za pomocą portalu klasyczny lub narzędzia wiersza polecenia, aplikacja jest już mieli dostęp do tej subskrypcji. W tym przypadku należy powiadomić użytkownika, którego połączenie z subskrypcją został oddzielone od poza aplikacją i nadać im opcję "Napraw" połączenie. "Napraw" po prostu chcesz ponownie utworzyć przypisanie roli, który został usunięty w trybie offline.

Tak, jak zostanie włączona użytkownikowi połączyć się z aplikacją subskrypcje, musisz umożliwiają użytkownikom zbyt odłączyć subskrypcji. Z programu access zarządzania punktu widzenia Odłącz oznacza usunięcie przypisanie roli, które ma wystawcy usługi aplikacji w swojej subskrypcji. Opcjonalnie każde Państwo w aplikacji dla subskrypcji mogą zostać usunięte również. Tylko użytkownicy mający uprawnienie Zarządzanie subskrypcji będą mogli odłączyć subskrypcji.

[Metoda RevokeRoleFromServicePrincipalOnSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L200) aplikacji przykładowych ASP.net MVC wykonuje to połączenie.

To wszystko — użytkowników teraz łatwo połączyć oraz zarządzać nimi Azure subskrypcji z aplikacją.

