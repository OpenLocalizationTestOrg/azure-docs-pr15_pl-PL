<properties
    pageTitle="Azure Active Directory B2C: Za pomocą Graph interfejsu API | Microsoft Azure"
    description="Jak połączenia interfejsu API wykres dla dzierżawy B2C przy użyciu tożsamości aplikacji aby zautomatyzować."
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-use-the-graph-api"></a>Azure AD B2C: Za pomocą wykresu interfejsu API

Azure dzierżaw B2C usługi Active Directory (Azure AD) mogą być bardzo duże. Oznacza to, że wiele typowych zadań zarządzania dzierżawy należy wykonać programowo. Przykład podstawowej jest zarządzanie użytkownikami. Może być konieczne migrację istniejącej magazynu użytkowników do dzierżawy B2C. Być może zechcesz hosta rejestracja użytkownika na własnej stronie i utworzyć konta użytkowników w Azure AD w tle. Poniższe typy zadań wymagać tworzenie, odczytywanie, aktualizowanie i usuwanie kont użytkowników. Te zadania można wykonywać przy użyciu interfejsu API Azure AD wykresu.

Dla dzierżawami B2C istnieją dwa podstawowe tryby komunikowania się z interfejsu API wykresu.

- W przypadku zadań interakcyjnych, jednokrotnego uruchamiania ma pełnić rolę administratora w dzierżawie B2C wykonywać zadania. W tym trybie wymaga administratora, aby zalogować się przy użyciu poświadczeń przed tym administrator może wykonywać żadnych wywołań API wykresu.
- W przypadku zadań automatyczną ciągłego, należy używać różnego rodzaju konta usługi dostarczać wystarczających uprawnień do wykonywania zadań zarządzania. W Azure AD możesz to zrobić rejestrowania aplikacji i uwierzytelniania Azure AD. Jest to zrobić przy użyciu **Identyfikatora aplikacji** używającej [udzielić OAuth 2.0 poświadczeń klienta](../active-directory/active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api). W tym przypadku aplikacja działa, pozbawiony opisanego wyżej działania użytkownika, aby nawiązać połączenie z interfejsu API wykres.

W tym artykule omówiono w sposób wykonywania przypadku użycia automatycznego. Aby udowodnić, możemy utworzyć 4,5 .NET `B2CGraphClient` który wykonuje użytkownikowi tworzenie, odczytywanie, aktualizowanie i usuwanie operacji (OBSŁUGIWAŁ). Klient uzyskuje interfejs wiersza polecenia systemu Windows (polecenie), który umożliwia wywołania różnych metod. Jednak kod jest zapisywany działają w sposób nieinteraktywnej, automatyczne.

## <a name="get-an-azure-ad-b2c-tenant"></a>Uzyskiwanie dzierżawy usługi Azure AD B2C

Zanim będzie można utworzyć aplikacji lub użytkowników i współdziałać z usługą Azure Active Directory w ogóle, konieczne będzie dzierżawę Azure AD B2C i konta administratora globalnego w dzierżawie. Jeśli nie masz już konto, dzierżawy, [wprowadzenie Azure AD B2C](active-directory-b2c-get-started.md).

## <a name="register-a-service-application-in-your-tenant"></a>Zarejestruj się w aplikacji usługi w dzierżawie

Po umieszczeniu dzierżawy B2C, musisz utworzyć aplikację usługi przy użyciu poleceń cmdlet środowiska AD PowerShell Azure.
Najpierw Pobierz i zainstaluj pakietu [Microsoft Online Services Asystenta logowania w](http://go.microsoft.com/fwlink/?LinkID=286152). Następnie pobrać i zainstalować [64-bitową modułu usługi Azure Active Directory dla programu Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).

> [AZURE.IMPORTANT]
Aby użyć interfejsu API wykres z Twojej dzierżawy B2C, będzie konieczne zarejestrować dedykowanej aplikacji przy użyciu programu PowerShell. Postępuj zgodnie z instrukcjami w tym artykule, aby to zrobić. Nie można ponownie użyć aplikacji B2C już istniejących zarejestrowane w portalu Azure.

Po zainstalowaniu modułu programu PowerShell, otwórz programu PowerShell i połącz się z dzierżawcą B2C. Po uruchomieniu `Get-Credential`, zostanie wyświetlony monit dla nazwy użytkownika i hasła, wprowadź nazwę użytkownika i hasło konta administratora dzierżawy B2C.

```
> $msolcred = Get-Credential
> Connect-MsolService -credential $msolcred
```

Przed utworzeniem aplikacja należy wygenerować nowy **klucz tajny klienta**.  Aplikacja użyje tajny klienta do uwierzytelniania Azure AD i uzyskanie tokeny dostępu. Istnieje możliwość wygenerowania prawidłowych hasła w programie PowerShell:

```
> $bytes = New-Object Byte[] 32
> $rand = [System.Security.Cryptography.RandomNumberGenerator]::Create()
> $rand.GetBytes($bytes)
> $rand.Dispose()
> $newClientSecret = [System.Convert]::ToBase64String($bytes)
> $newClientSecret
```

Ostatnim poleceniu wydrukować nowy klucz tajny klienta. Skopiuj go innym bezpieczne. Musisz go później. Teraz można utworzyć aplikację, dostarczając nowego hasła klienta jako poświadczenie aplikacji:

```
> New-MsolServicePrincipal -DisplayName "My New B2C Graph API App" -Type password -Value $newClientSecret

DisplayName           : My New B2C Graph API App
ServicePrincipalNames : {dd02c40f-1325-46c2-a118-4659db8a55d5}
ObjectId              : e2bde258-6691-410b-879c-b1f88d9ef664
AppPrincipalId        : dd02c40f-1325-46c2-a118-4659db8a55d5
TrustedForDelegation  : False
AccountEnabled        : True
Addresses             : {}
KeyType               : Password
KeyId                 : a261e39d-953e-4d6a-8d70-1f915e054ef9
StartDate             : 9/2/2015 1:33:09 AM
EndDate               : 9/2/2016 1:33:09 AM
Usage                 : Verify
```

Jeśli pomyślnie utworzyć aplikację, należy wydrukować właściwości aplikacji podobnych powyżej. Będą potrzebne `ObjectId` i `AppPrincipalId`, więc zbyt skopiuj te wartości.

Po utworzeniu aplikacji w dzierżawie B2C muszą przypisz mu uprawnienia niezbędne do wykonywania operacji OBSŁUGIWAŁ użytkownika. Przypisywanie ról trzy aplikacji: katalogu czytelnikom (zapoznaj się z użytkownikami), autorzy katalogu (na tworzenie i aktualizowanie użytkowników), a administratorem konta użytkownika (Aby usunąć użytkowników). Role te ma dobrze znane identyfikatory, więc możesz zastąpić `-RoleMemberObjectId` parametr `ObjectId` z góry i uruchom następujące polecenia. Aby wyświetlić listę wszystkich ról katalogu, spróbuj uruchomić `Get-MsolRole`.

```
> Add-MsolRoleMember -RoleObjectId 88d8e3e3-8f55-4a1e-953a-9b9898b8876b -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
> Add-MsolRoleMember -RoleObjectId 9360feb5-f418-4baa-8175-e2a00bac4301 -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
> Add-MsolRoleMember -RoleObjectId fe930be7-5e62-47db-91af-98c3a49a38b1 -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
```

Teraz masz aplikację, która ma uprawnienia do tworzenia, odczytu, aktualizowania, a usunąć użytkowników z dzierżawy usługi B2C.

## <a name="download-configure-and-build-the-sample-code"></a>Pobieranie, konfigurować i tworzenie przykładowy kod

Najpierw pobrać przykładowy kod i uruchomiony. Następnie możemy potrwa przyjrzeć się bliżej go.  Możesz [pobrać przykładowy kod jako pliku zip](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip). Można również klonowanie w katalogu wybranych przez użytkownika:

```
git clone https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet.git
```

Otwórz `B2CGraphClient\B2CGraphClient.sln` rozwiązania Visual Studio w programie Visual Studio. W `B2CGraphClient` projekt, otwórz plik `App.config`. Zastąp ustawienia aplikacji trzy własne wartości:

```
<appSettings>
    <add key="b2c:Tenant" value="contosob2c.onmicrosoft.com" />
    <add key="b2c:ClientId" value="{The AppPrincipalId from above}" />
    <add key="b2c:ClientSecret" value="{The client secret you generated above}" />
</appSettings>
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Następnie kliknij prawym przyciskiem myszy `B2CGraphClient` rozwiązanie i Odbuduj próbki. Jeśli jesteś pomyślnie, po wykonaniu `B2C.exe` plik wykonywalny znajduje się w `B2CGraphClient\bin\Debug`.

## <a name="build-user-crud-operations-by-using-the-graph-api"></a>Tworzenie użytkownika OBSŁUGIWAŁ operacje przy użyciu interfejsu API wykresu

Aby użyć B2CGraphClient, otwórz `cmd` poleceń systemu Windows monit, a następnie zmień katalog do `Debug` katalogu. Następnie uruchom `B2C Help` polecenia.

```
> cd B2CGraphClient\bin\Debug
> B2C Help
```

Spowoduje to wyświetlenie krótki opis każdego polecenia. Za każdym razem jednego z tych poleceń wywołania `B2CGraphClient` żąda API Azure AD wykresu.

### <a name="get-an-access-token"></a>Uzyskiwanie token dostępu

Dowolnego żądania API Graph wymaga token dostępu dla uwierzytelniania. `B2CGraphClient`używa Otwórz źródło Active Directory uwierzytelniania biblioteki (ADAL), aby ułatwić uzyskiwanie tokeny dostępu. ADAL ułatwia token nabycia, dostarczając prostego interfejsu API i przypadki, niektóre ważne informacje, takie jak buforowanie tokeny dostępu. Nie musisz uzyskać tokenów, ale przy użyciu ADAL. Można również uzyskać tokenów przez rzemieślniczy żądania HTTP.

> [AZURE.NOTE]
    W przykładzie kodu użyto ADAL w wersji 2 w celu komunikowanie się za pomocą interfejsu API wykres.  Aby można było uzyskiwać tokeny dostępu, które mogą być używane z interfejsem API Azure AD wykresu, należy użyć ADAL wersja 2 lub 3.

Po `B2CGraphClient` jest uruchomiony, który tworzy wystąpienie `B2CGraphClient` zajęć. Konstruktor dla tej klasy konfiguruje rusztowania ADAL uwierzytelniania:

```C#
public B2CGraphClient(string clientId, string clientSecret, string tenant)
{
    // The client_id, client_secret, and tenant are provided in Program.cs, which pulls the values from App.config
    this.clientId = clientId;
    this.clientSecret = clientSecret;
    this.tenant = tenant;

    // The AuthenticationContext is ADAL's primary class, in which you indicate the tenant to use.
    this.authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenant);

    // The ClientCredential is where you pass in your client_id and client_secret, which are
    // provided to Azure AD in order to receive an access_token by using the app's identity.
    this.credential = new ClientCredential(clientId, clientSecret);
}
```

Użyjemy `B2C Get-User` polecenia jako przykład. Gdy `B2C Get-User` jest wywoływana bez wszelkie dodatkowe dane wejściowe połączenia polecenie `B2CGraphClient.GetAllUsers(...)` metody. Ta metoda wymaga `B2CGraphClient.SendGraphGetRequest(...)`, która przesyła żądanie HTTP GET API wykresu. Przed `B2CGraphClient.SendGraphGetRequest(...)` wysyła żądania GET, najpierw staje się dostępu token przy użyciu ADAL:

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    // First, use ADAL to acquire a token by using the app's identity (the credential)
    // The first parameter is the resource we want an access_token for; in this case, the Graph API.
    AuthenticationResult result = authContext.AcquireToken("https://graph.windows.net", credential);

    ...

```

Możesz uzyskać dostęp tokenu dla interfejsu API wykresu, dzwoniąc ADAL `AuthenticationContext.AcquireToken(...)` metody. Zwraca ADAL `access_token` reprezentujący tożsamości aplikacji.

### <a name="read-users"></a>Więcej użytkowników

Jeśli chcesz wyświetlić listę użytkowników, lub uzyskaj danego użytkownika z interfejsu API wykres, można wysłać HTTP `GET` żądanie `/users` punktu końcowego. Żądanie dla wszystkich użytkowników w dzierżawie wygląda następująco:

```
GET https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Aby wyświetlić tę prośbę, uruchom polecenie:

 ```
 > B2C Get-User
 ```

Istnieją dwa ważnych uwag:

- Token dostępu nabyte za pośrednictwem ADAL jest dodawana do `Authorization` nagłówka za pomocą `Bearer` systemu.
- Dla B2C dzierżawami, możesz użyć parametru zapytania `api-version=1.6`.

Obie informacje te są obsługiwane w `B2CGraphClient.SendGraphGetRequest(...)` metody:

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    ...

    // For B2C user management, be sure to use the 1.6 Graph API version.
    HttpClient http = new HttpClient();
    string url = "https://graph.windows.net/" + tenant + api + "?" + "api-version=1.6";
    if (!string.IsNullOrEmpty(query))
    {
        url += "&" + query;
    }

    // Append the access token for the Graph API to the Authorization header of the request by using the Bearer scheme.
    HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, url);
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
    HttpResponseMessage response = await http.SendAsync(request);

    ...
```

### <a name="create-consumer-user-accounts"></a>Utwórz konta użytkowników dla klientów indywidualnych

Po utworzeniu konta użytkowników w dzierżawie B2C możesz wysłać HTTP `POST` żądanie `/users` punkt końcowy:

```
POST https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 338

{
    // All of these properties are required to create consumer users.

    "accountEnabled": true,
    "signInNames": [                            // controls which identifier the user uses to sign in to the account
        {
            "type": "emailAddress",             // can be 'emailAddress' or 'userName'
            "value": "joeconsumer@gmail.com"
        }
    ],
    "creationType": "LocalAccount",            // always set to 'LocalAccount'
    "displayName": "Joe Consumer",              // a value that can be used for displaying to the end user
    "mailNickname": "joec",                     // an email alias for the user
    "passwordProfile": {
        "password": "P@ssword!",
        "forceChangePasswordNextLogin": false   // always set to false
    },
    "passwordPolicies": "DisablePasswordExpiration"
}
```

Większość z tych właściwości w tym żądaniu są wymagane do utworzenia dla klientów indywidualnych użytkowników. Aby uzyskać więcej informacji, kliknij [tutaj](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser). Należy zauważyć, że `//` komentarze zostały włączone dla ilustracji. Nie powinno obejmować ich rzeczywistej wezwanie.

Aby wyświetlić żądania, uruchom jeden z następujących poleceń:

```
> B2C Create-User ..\..\..\usertemplate-email.json
> B2C Create-User ..\..\..\usertemplate-username.json
```

`Create-User` Polecenia trwa pliku .json jako parametru wejściowego. Ta strona zawiera reprezentację JSON obiektu użytkownika. Istnieją dwa przykładowe pliki .json w kodzie przykładowych: `usertemplate-email.json` i `usertemplate-username.json`. Możesz zmienić te pliki do własnych potrzeb. Oprócz wymagane pola powyżej kilka pola opcjonalne, które są dostępne są uwzględniane w tych plików. Szczegółowych informacji na temat pola opcjonalne znajdują się w [odwołania do jednostki interfejsu API Azure AD wykresu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity).

Widać, jak żądanie POST jest tworzona w `B2CGraphClient.SendGraphPostRequest(...)`.

- Dołącza token dostępu do `Authorization` nagłówka żądania.
- Ustawia `api-version=1.6`.
- Zawiera obiekt użytkownika JSON w treści wezwania.

> [AZURE.NOTE]
Jeśli konta, które chcesz przeprowadzić migrację z istniejącego magazynu użytkownika ma dolnym siły hasła niż [nałożonych przez Azure AD B2C siły silnego hasła](https://msdn.microsoft.com/library/azure/jj943764.aspx), możesz wyłączyć za pomocą Wymaganie silnych haseł `DisableStrongPassword` wartość w `passwordPolicies` właściwości. Na przykład można modyfikować żądanie użytkownika utworzenia podanego powyżej w następujący sposób: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.

### <a name="update-consumer-user-accounts"></a>Aktualizowanie konta użytkowników dla klientów indywidualnych

Po zaktualizowaniu obiektów użytkowników proces jest podobny do tego, który będzie używany do tworzenia obiektów użytkowników. Ale ten proces używa HTTP `PATCH` metody:

```
PATCH https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 37

{
    "displayName": "Joe Consumer",              // this request updates only the user's displayName
}
```

Spróbuj zaktualizować użytkownika przez aktualizowanie plików JSON nowymi danymi. Możesz użyć `B2CGraphClient` na jeden z następujących poleceń:

```
> B2C Update-User <user-object-id> ..\..\..\usertemplate-email.json
> B2C Update-User <user-object-id> ..\..\..\usertemplate-username.json
```

Przeprowadzanie inspekcji `B2CGraphClient.SendGraphPatchRequest(...)` metody, aby uzyskać szczegółowe informacje na temat wysyłania to żądanie.

### <a name="search-users"></a>Wyszukaj użytkowników

Możesz wyszukać użytkowników w dzierżawie B2C na kilka sposobów. Obiekt ją za pomocą użytkownika identyfikator lub dwa, za pomocą której logowania użytkownika (to znaczy `signInNames` właściwości).

Uruchom jedną z następujących poleceń, aby wyszukać określonego użytkownika:

```
> B2C Get-User <user-object-id>
> B2C Get-User <filter-query-expression>
```

Oto kilka przykładów:

```
> B2C Get-User 2bcf1067-90b6-4253-9991-7f16449c2d91
> B2C Get-User $filter=signInNames/any(x:x/value%20eq%20%27joeconsumer@gmail.com%27)
```

### <a name="delete-users"></a>Usuwanie użytkowników

Proces usuwania użytkownika jest proste. Użyj protokołu HTTP `DELETE` metody i konstrukcji adres URL z odpowiedniego identyfikator obiektu:

```
DELETE https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Aby zobaczyć przykład, wprowadź następujące polecenie i wyświetlanie żądanie usunięcia drukowanego do konsoli:

```
> B2C Delete-User <object-id-of-user>
```

Przeprowadzanie inspekcji `B2CGraphClient.SendGraphDeleteRequest(...)` metody, aby uzyskać szczegółowe informacje na temat wysyłania to żądanie.

Można wykonywać wielu innych czynności przy użyciu interfejsu API Azure AD wykresu oprócz Zarządzanie użytkownikami. [Odwołanie interfejsu API Azure AD wykresu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) podano szczegółowe informacje dotyczące każdej akcji, wraz z żądania próbki.

## <a name="use-custom-attributes"></a>Używanie atrybutów niestandardowych

Większość aplikacji dla klientów indywidualnych należy przechowywać niektóre typu informacji w profilu użytkownika niestandardowego. Jednym ze sposobów możesz to zrobić jest określenie atrybutu niestandardowego w dzierżawie B2C. Można następnie traktować atrybutu tak samo jak inne właściwości są traktowane w obiekcie użytkownika. Możesz zaktualizować atrybut, usuń atrybut, kwerendy przez atrybut, wysyłać atrybut jako oświadczeń w tokeny logowania i nie tylko.

Aby zdefiniować atrybutu niestandardowego w dzierżawie B2C, zobacz [B2C niestandardowy atrybut odwołania](active-directory-b2c-reference-custom-attr.md).

Można wyświetlić atrybuty niestandardowe zdefiniowane w dzierżawie B2C przy użyciu `B2CGraphClient`:

```
> B2C Get-B2C-Application
> B2C Get-Extension-Attribute <object-id-in-the-output-of-the-above-command>
```

Dane wyjściowe tych funkcji powoduje wyświetlenie szczegółów każdego niestandardowy atrybut, takich jak:

```JSON
{
      "odata.type": "Microsoft.DirectoryServices.ExtensionProperty",
      "objectType": "ExtensionProperty",
      "objectId": "cec6391b-204d-42fe-8f7c-89c2b1964fca",
      "deletionTimestamp": null,
      "appDisplayName": "",
      "name": "extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number",
      "dataType": "Integer",
      "isSyncedFromOnPremises": false,
      "targetObjects": [
        "User"
      ]
}
```

Można użyć pełnej nazwy, takie jak `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, jako właściwość obiekty użytkownika.  Zaktualizować plik .json z nowej właściwości i wartości właściwości, a następnie uruchom:

```
> B2C Update-User <object-id-of-user> <path-to-json-file>
```

Za pomocą `B2CGraphClient`, masz aplikacją usługi, które mogą zarządzać użytkownicy dzierżawy B2C programowy. `B2CGraphClient`używa tożsamości aplikacji do uwierzytelniania API Azure AD wykresu. Za pomocą hasła klienta są również uzyskuje tokeny. Zgodnie z tej funkcji można dołączyć do aplikacji, należy pamiętać o kilku najważniejszych punktów w przypadku aplikacji B2C:

- Należy udzielić odpowiednich uprawnień w dzierżawie aplikacji.
- Teraz musisz uzyskać tokeny dostępu za pomocą ADAL w wersji 2. (Pliki możesz też wysyłać wiadomości protokołu bezpośrednio, bez używania biblioteki.)
- Podczas połączenia z interfejsu API wykresu za pomocą `api-version=1.6`.
- Podczas tworzenia i aktualizowania użytkowników dla klientów indywidualnych, kilka właściwości są wymagane, zgodnie z powyższym opisem.

Jeśli masz pytania lub żądania do wykonywania czynności, że chcesz wykonać za pomocą interfejsu API wykres na Twojej dzierżawy B2C, zostaw komentarz w tym artykule, lub plików problem w repozytorium przykładowy kod GitHub.
