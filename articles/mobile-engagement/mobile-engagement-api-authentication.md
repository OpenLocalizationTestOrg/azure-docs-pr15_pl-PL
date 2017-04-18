<properties 
    pageTitle="Uwierzytelnianie przy użyciu urządzeń przenośnych zaangażowania interfejsy API pozostałych"
    description="Informacje dotyczące uwierzytelniania Azure Mobile zaangażowania pozostałych API" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="10/05/2016"
    ms.author="wesmc;ricksal"/>

# <a name="authenticate-with-mobile-engagement-rest-apis"></a>Uwierzytelnianie przy użyciu urządzeń przenośnych zaangażowania interfejsy API pozostałych

## <a name="overview"></a>Omówienie

Tym dokumencie opisano, jak uzyskać prawidłową Oauth AAD token do uwierzytelniania API pozostałych zaangażowania Mobile. 

Zakłada się, że masz ważna subskrypcja Azure i utworzeniu aplikacji zaangażowania Mobile, przy użyciu jednej z naszych [Samouczki Deweloper](mobile-engagement-windows-store-dotnet-get-started.md).

## <a name="authentication"></a>Uwierzytelnianie

Microsoft Azure Active Directory podstawie OAuth token jest używany do uwierzytelniania. 

W celu żądania uwierzytelnienia interfejs API nagłówkiem autoryzacji musi zostać dodana do każdego żądania, czyli następującą postać:

    Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGmJlNmV2ZWJPamg2TTNXR1E...

>[AZURE.NOTE] Tokeny Azure Active Directory wygaśnie w ciągu godziny.

Istnieje kilka sposobów uzyskiwania tokenu. Ponieważ za pośrednictwem interfejsów API zazwyczaj są nazywane z usługi w chmurze, którego chcesz użyć klucz interfejsu API. Klucz interfejsu API w terminologii Azure nosi nazwę hasło głównej usługi. W poniższej procedurze opisano jednym ze sposobów ręcznego konfigurowania.

### <a name="one-time-setup-using-script"></a>Jednorazowej konfiguracji (za pomocą skryptu)

Należy postępować zgodnie z zestawem instrukcji poniżej do wykonywania za pomocą skryptu programu PowerShell minimalny czas instalacji, ale używa najbardziej dopuszczalne wartości domyślnych ustawień. Opcjonalnie można też postępuj zgodnie z instrukcjami w [Ręczna konfiguracja](mobile-engagement-api-authentication-manual.md) spowoduje to bezpośrednio z poziomu portalu Azure i wykonaj precyzyjniej zarządzać konfiguracji. 

1. Pobierz najnowszą wersję programu Azure PowerShell [tutaj](http://aka.ms/webpi-azps). Aby uzyskać więcej informacji na instrukcje pobierania widać tego [łącza](../powershell-install-configure.md).  

2. Po zainstalowaniu programu PowerShell Azure, aby upewnić się, że masz zainstalowany **moduł Azure** należy użyć następujących poleceń:

    . Upewnij się, że modułu programu PowerShell usługi Azure jest dostępna na liście dostępnych modułów. 
    
        Get-Module –ListAvailable 

    ![Dostępne moduły Azure][1]
        
    b. Jeśli nie zostanie znaleziony modułu programu PowerShell usługi Azure na powyższej liście, a następnie należy uruchomić następujące czynności:
        
        Import-Module Azure 
        
3. Logowanie do Menedżera zasobów Azure z programu PowerShell, uruchamiając następujące polecenie i podając swoją nazwę użytkownika i hasło dla konta Azure: 
        
        Login-AzureRmAccount

4. Jeśli masz wiele subskrypcji należy uruchomić następujące czynności:

    . Wyświetlić listę wszystkich subskrypcji i skopiuj SubscriptionId subskrypcji, którą chcesz użyć. Upewnij się, że tej subskrypcji jest taki sam, który ma aplikacji zaangażowania Mobile, które mają zostać interakcję za pomocą interfejsów API. 

        Get-AzureRmSubscription

    b. Uruchom następujące polecenie dostarczanie SubscriptionId skonfigurowanie subskrypcji do użytku.

        Select-AzureRmSubscription –SubscriptionId <subscriptionId>

5. Kopiowanie tekstu skryptu [Nowy AzureRmServicePrincipalOwner.ps1](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) na komputerze lokalnym, a następnie zapisz go jako polecenia cmdlet programu PowerShell (np. `APIAuth.ps1`), a następnie wykonaj je `.\APIAuth.ps1`. 
    
6. Skrypt wyświetli monit o podanie danych wejściowych dla **principalName**. Podaj odpowiednią nazwę chcesz używać do tworzenia aplikacji usługi Active Directory (na przykład APIAuth). 

7. Po ukończeniu działania skryptu, są wyświetlane następujące cztery wartości, które trzeba będzie uwierzytelnienia programowy z usługą Active Directory dlatego upewnij się je skopiować. 
        
    **TenantId**, **SubscriptionId**, **Identyfikator aplikacji**i **hasło**.

    Użyje TenantId jako `{TENANT_ID}`, identyfikator aplikacji jako `{CLIENT_ID}` i tajny jako `{CLIENT_SECRET}`.

    > [AZURE.NOTE] Zasady dotyczące zabezpieczeń domyślne blokują uruchamianie skryptów programu PowerShell. Jeśli tak, możesz tymczasowo skonfigurować zasady wykonanie, aby pozwolić na wykonanie skryptu przy użyciu następującego polecenia:

        > Set-ExecutionPolicy RemoteSigned

8. Poniżej przedstawiono, jak będzie wyglądać zestawu poleceń cmdlet PS. 

    ![][3]

9. Zaznacz w portalu zarządzania Azure utworzenia nowej aplikacji AD o nazwie podany skrypt o nazwie **principalName** w obszarze **Pokaż aplikacji firmowych właścicielem**.

    ![][4]

#### <a name="steps-to-get-a-valid-token"></a>Czynności, aby uzyskać prawidłową tokenu

1. Interfejs API z poniższych parametrów połączenia i upewnij się zamienić dzierżawy\_identyfikator, klienta\_identyfikator i klienta\_HASŁA:

    - **Żądanie adres URL** jako *https://login.microsoftonline.com/ {dzierżawy\_identyfikator}-uwierzytelnianie oauth2-token*
    - **Typ zawartości HTTP nagłówek** jako *aplikacja/x-www-form-urlencoded*
    - **Treść żądania HTTP** *udzielić\_typ = klienta\_poświadczenia i client_id = {klienta\_identyfikator} & client_secret = {klienta\_TAJNY} & resource=https%3A%2F%2Fmanagement.core.windows.net%2F*

    Oto przykład żądania:

        POST /{TENANT_ID}/oauth2/token HTTP/1.1
        Host: login.microsoftonline.com
        Content-Type: application/x-www-form-urlencoded
        grant_type=client_credentials&client_id={CLIENT_ID}&client_secret={CLIENT_SECRET}&reso
        urce=https%3A%2F%2Fmanagement.core.windows.net%2F

    Oto przykład odpowiedzi:

        HTTP/1.1 200 OK
        Content-Type: application/json; charset=utf-8
        Content-Length: 1234
    
        {"token_type":"Bearer","expires_in":"3599","expires_on":"1445395811","not_before":"144
        5391911","resource":"https://management.core.windows.net/","access_token":{ACCESS_TOKEN}}

    W tym przykładzie uwzględnione kodowanie adresów URL parametrów WPIS `resource` wartość jest faktycznie `https://management.core.windows.net/`. Pamiętaj, aby również adres URL kodowanie `{CLIENT_SECRET}` jako może zawierać znaków specjalnych.

    > [AZURE.NOTE] Do testowania, można użyć narzędzia klienta HTTP takie jak [Fiddler](http://www.telerik.com/fiddler) lub [rozszerzenie Chrome Postman](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop) 

2. Teraz w każdej połączenia interfejsu API obejmują nagłówku żądania autoryzacji:

        Authorization: Bearer {ACCESS_TOKEN}

    Jeśli zostanie wyświetlony kodu stanu 401 zwrócone, sprawdź treść odpowiedzi go mogą informować o wygasł tokenu. W takim przypadku uzyskać nowy token.

##<a name="using-the-apis"></a>Używanie interfejsów API

Teraz, gdy masz prawidłowy token, możesz przystąpić do nawiązywania połączeń interfejsu API.

1. W każdym żądaniu interfejsu API należy przekazać prawidłowy, pozostały tokenu uzyskaną w poprzedniej sekcji.

2. Konieczne będzie Podłącz niektórych parametrów do wezwania identyfikator URI, który identyfikuje aplikację. Żądanie URI wygląda następująco

        https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/
        providers/Microsoft.MobileEngagement/appcollections/{app-collection}/apps/{app-resource-name}/

    Aby pobrać parametrów kliknij swoją nazwę aplikacji, a pulpitu nawigacyjnego, a zostanie wyświetlona strona jak następujących 3 parametry.

    - **1**`{subscription-id}`
    - **2**`{app-collection}`
    - **3**`{app-resource-name}`
    - **4** nazwy i grupa zasobów ma być **MobileEngagement** , chyba że tworzony nowy. 

    ![Identyfikator URI interfejsu API zaangażowania Mobile parametrów][2]

>[AZURE.NOTE] <br/>
>1. Ignoruj adres głównego interfejsu API, jak to dla poprzedniego interfejsów API.<br/>
>2. Jeśli została utworzona za pomocą portalu Azure klasycznym należy użyć nazwy zasobu aplikacji, która jest inna niż sama nazwa aplikacji. Jeśli aplikacja została utworzona w Azure Portal należy używać nazwę aplikacji, sam (jest rozróżnienia między nazwę zasobu aplikacji i aplikacji dla aplikacji utworzonych w portalu nowy).  

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication/azure-module.png
[2]: ./media/mobile-engagement-api-authentication/mobile-engagement-api-uri-params.png
[3]: ./media/mobile-engagement-api-authentication/ps-cmdlets.png
[4]: ./media/mobile-engagement-api-authentication/ad-app-creation.png



