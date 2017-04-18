<properties
    pageTitle="Azure AD systemu iOS wprowadzenie | Microsoft Azure"
    description="Sposoby tworzenia aplikacji iOS można zintegrować z usługą Azure Active Directory do zalogowania się i połączeń Azure AD chronionych za pomocą OAuth interfejsów API."
    services="active-directory"
    documentationCenter="ios"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="integrate-azure-ad-into-an-ios-app"></a>Integrowanie Azure AD iOS aplikacji

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)] 

Azure AD Active Directory Authentication Library lub ADAL, przewiduje iOS klientów, którzy potrzebują dostępu do chronionych zasobów.  Osoby ADAL wyłącznie w celu w życiu jest ułatwiają aplikacji uzyskanie tokeny dostępu.  Wykazać się, jak łatwo jest, w tym miejscu będzie budowy aplikacji listy zadań do wykonania C celem którego:

-   Pobiera dostęp tokeny do nawiązywania połączeń z interfejsu API Azure AD wykresu przy użyciu [protokołu uwierzytelniania OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).
-   Wyszukiwanie w katalogu dla użytkowników z określonego aliasu.

Aby utworzyć pełną aplikację roboczych, musisz:

2. Zarejestruj się w aplikacji z usługą Azure Active Directory.
3. Instalowanie i konfigurowanie ADAL.
5. Umożliwia uzyskanie tokeny Azure AD ADAL.

Aby rozpocząć korzystanie, [Pobierz szkielet aplikacji](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) lub [Pobierz ukończonej przykładowej](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).  Konieczne będzie również dzierżawę Azure AD, w którym można tworzyć użytkowników i rejestrowania aplikacji.  Jeśli nie masz jeszcze dzierżawy, [Dowiedz się, jak kupić](active-directory-howto-tenant.md).

> [AZURE.TIP] Wypróbuj Podgląd programu nasz nowy [dzięki portalowi deweloperów](https://identity.microsoft.com/Docs/iOS) , która ułatwi rozpoczęcie pracy z usługą Azure Active Directory w ciągu kilku minut!  Dzięki portalowi deweloperów przeprowadzi Cię przez proces rejestracji aplikację i integracji Azure AD do kodu.  Po zakończeniu, konieczne będzie prostej aplikacji uwierzytelnieni użytkownicy w Twojej dzierżawy i wewnętrznej bazie danych, które można zaakceptować tokeny sprawdzana poprawność. 

## <a name="1-determine-what-your-redirect-uri-will-be-for-ios"></a>*1. Określanie swojego identyfikatora URI przekierowywać będzie dla systemu iOS*

Aby można było bezpieczne uruchamianie aplikacji w niektórych scenariuszach logowania jednokrotnego wymagana Tworzenie identyfikatora **URI Przekieruj** w określonym formacie. Identyfikator URI przekierowywać służy do upewnij się, że tokeny powrócić do poprawne aplikacji, która zostanie wyświetlony monit dla nich.

Format iOS dla identyfikatora URI przekierowywanie jest:

```
<app-scheme>://<bundle-id>
```

-   **schemat aap** — to jest zarejestrowana w projekcie XCode. Jest jak inne aplikacje zadzwonić do Ciebie. Można znaleźć, to w obszarze Info.plist -> typy adresu URL -> identyfikator adresu URL. Należy utworzyć jeden, jeśli nie masz jeszcze jedną lub więcej skonfigurowane.
-   **identyfikator pakietu** — jest to identyfikator pakietu znajdujący się w obszarze "tożsamości" Wyczyść ustawień projektu w XCode.

Oto przykład dla tego kodu Szybki Start: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***

## <a name="2-register-the-directorysearcher-application"></a>*2. rejestr aplikacji DirectorySearcher*
Aby włączyć aplikację uzyskać tokenów, najpierw musisz go zarejestrować w dzierżawie usługi Azure AD i udzielać jej uprawnień dostępu do interfejsu API Azure AD wykresu:

-   Zaloguj się do portalu zarządzania Azure
-   W nawigacji po lewej kliknij w **Usłudze Active Directory**
-   Wybierz pozycję dzierżawy, w której chcesz zarejestrować tę aplikację.
-   Kliknij kartę **aplikacje** , a następnie kliknij przycisk **Dodaj** w szufladzie dołu.
-   Postępuj zgodnie z monitami i Utwórz nową **Natywnej aplikacji**.
    -   **Nazwa** aplikacji opisując aplikacji dla użytkowników końcowych
    -   **Przekierowywanie Uri** to połączenie schemat i ciąg używające Azure AD zwraca token odpowiedzi.  Wprowadź wartość określonej aplikacji na podstawie podanych informacji.
-   Po zakończeniu rejestracji AAD przypisze aplikacji unikatowego identyfikatora klienckiego.  Musisz tę wartość w następnej sekcji, aby skopiować go na karcie **Konfigurowanie** .
- Ponadto na karcie **Konfigurowanie** Odszukaj sekcję "Uprawnienia do innych aplikacji".  Na podstawie "Usługi Azure Active Directory" Dodaj uprawnień **dostępu i katalogu organizacji** w obszarze **Delegowane uprawnienia**.  Spowoduje to włączenie aplikacji kwerendy API wykres dla użytkowników.

## <a name="3-install--configure-adal"></a>*3. Instalowanie i konfigurowanie ADAL*
Teraz, gdy masz aplikację w Azure AD, można zainstalować ADAL i wpisz swój kod dotyczące tożsamości.  Aby ADAL mieć możliwość komunikowania się z usługą Azure Active Directory musisz udostępnić niektóre informacje o rejestracji aplikacji.
-   Rozpocznij, dodając ADAL do projektu DirectorySearcher przy użyciu Cocapods.

```
$ vi Podfile
```
Dodaj następujący tekst na tym podfile:

```
source 'https://github.com/CocoaPods/Specs.git'
link_with ['QuickStart']
xcodeproj 'QuickStart'

pod 'ADALiOS'
```

Teraz załadować podfile przy użyciu cocoapods. Spowoduje to utworzenie nowego obszaru roboczego XCode ładowania.

```
$ pod install
...
$ open QuickStart.xcworkspace
```

-   W programie project Szybki Start, otwórz plik plist `settings.plist`.  Zamienianie wartości elementów w sekcji w celu odzwierciedlenia wartości wprowadzane w Azure Portal.  Kod będzie odwoływać się do tych wartości przy każdym użyciu ADAL.
    -   `tenant` Jest domeną Twojej dzierżawy Azure AD contoso.onmicrosoft.com np.
    -   `clientId` Jest clientId aplikacji skopiowany z portalu.
    -   `redirectUri` Jest przekierowanie adres url zarejestrowany w portalu.

## <a name="4--use-adal-to-get-tokens-from-aad"></a>*4. Używanie ADAL, aby uzyskać tokenów z AAD*
Podstawowe zasady za ADAL to, że zawsze, gdy aplikacji wymaga token dostępu, po prostu wywołuje completionBlock `+(void) getToken : `, i ADAL reszta.  

-   W `QuickStart` projektu, otwórz `GraphAPICaller.m` i Znajdź `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` komentarz u góry.  Jest to miejsce, w którym należy przekazać ADAL współrzędne za pośrednictwem CompletionBlock do komunikowania się z usługą Azure Active Directory i poinstruuj go jak pamięci podręcznej tokenów.

```ObjC
+(void) getToken : (BOOL) clearCache
           parent:(UIViewController*) parent
completionHandler:(void (^) (NSString*, NSError*))completionBlock;
{
    AppData* data = [AppData getInstance];
    if(data.userItem){
        completionBlock(data.userItem.accessToken, nil);
        return;
    }

    ADAuthenticationError *error;
    authContext = [ADAuthenticationContext authenticationContextWithAuthority:data.authority error:&error];
    authContext.parentController = parent;
    NSURL *redirectUri = [[NSURL alloc]initWithString:data.redirectUriString];

    [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
    [authContext acquireTokenWithResource:data.resourceId
                                 clientId:data.clientId
                              redirectUri:redirectUri
                           promptBehavior:AD_PROMPT_AUTO
                                   userId:data.userItem.userInformation.userId
                     extraQueryParameters: @"nux=1" // if this strikes you as strange it was legacy to display the correct mobile UX. You most likely won't need it in your code.
                          completionBlock:^(ADAuthenticationResult *result) {

                              if (result.status != AD_SUCCEEDED)
                              {
                                  completionBlock(nil, result.error);
                              }
                              else
                              {
                                  data.userItem = result.tokenCacheStoreItem;
                                  completionBlock(result.tokenCacheStoreItem.accessToken, nil);
                              }
                          }];
}

```

- Teraz trzeba wyszukać użytkowników na wykresie za pomocą ten token. Znajdowanie `// TODO: implement SearchUsersList` commentThis metoda pozwala żądania GET API Azure AD wykres do kwerendy dla użytkowników, których UPN zaczyna się od określonej wyszukiwany termin.  Jednak aby kwerenda interfejsu API wykres, trzeba uwzględnić access_token w `Authorization` nagłówka żądania — jest to, skąd pochodzą ADAL.

```ObjC
+(void) searchUserList:(NSString*)searchString
                parent:(UIViewController*) parent
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    AppData* data = [AppData getInstance];

    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users?api-version=%@&$filter=startswith(userPrincipalName, '%@')", data.taskWebApiUrlString, data.tenant, data.apiversion, searchString];


    [self craftRequest:[self.class trimString:graphURL]
                parent:parent
     completionHandler:^(NSMutableURLRequest *request, NSError *error) {

         if (error != nil)
         {
             completionBlock(nil, error);
         }
         else
         {

             NSOperationQueue *queue = [[NSOperationQueue alloc]init];

             [NSURLConnection sendAsynchronousRequest:request queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {

                 if (error == nil && data != nil){

                     NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:data options:0 error:nil];

                     // We can grab the top most JSON node to get our graph data.
                     NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                     // Don't be thrown off by the key name being "value". It really is the name of the
                     // first node. :-)

                     //each object is a key value pair
                     NSDictionary *keyValuePairs;
                     NSMutableArray* Users = [[NSMutableArray alloc]init];

                     for(int i =0; i < graphDataArray.count; i++)
                     {
                         keyValuePairs = [graphDataArray objectAtIndex:i];

                         User *s = [[User alloc]init];
                         s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                         s.name =[keyValuePairs valueForKey:@"givenName"];

                         [Users addObject:s];
                     }

                     completionBlock(Users, nil);
                 }
                 else
                 {
                     completionBlock(nil, error);
                 }

             }];
         }
     }];

}

```
- Gdy aplikacji zażąda tokenu, dzwoniąc `getToken(...)`, ADAL spróbuje zwracana tokenu bez monitowania użytkownika o poświadczenia.  Jeśli ADAL Określa, że użytkownik musi się zalogować do uzyskać token, go będzie wyświetlane okno dialogowe Logowanie, zbieranie poświadczenia użytkownika i zwrócić token po pomyślnym uwierzytelnieniu.  W przypadku nie można zwrócić tokenu dowolnego powodu ADAL spowoduje zgłoszenie `AdalException`.
- Należy zauważyć, że `AuthenticationResult` obiekt zawiera `tokenCacheStoreItem` obiekt, który może być używany na potrzeby zbierania informacji o aplikacji może być konieczne.  W szybki start `tokenCacheStoreItem` służy do określania, czy authenitcation już pojawił się.


## <a name="step-5-build-and-run-the-application"></a>Krok 5: Tworzenie i uruchamianie aplikacji



Gratulacje! Teraz masz aplikacją iOS pracy, która ma możliwość uwierzytelniania użytkowników, bezpieczne połączenie API sieci Web przy użyciu OAuth 2.0 i uzyskanie podstawowych informacji o użytkowniku.  Jeśli jeszcze tego nie zrobiono, nadszedł czas, aby wypełnić dzierżawy usługi z niektórzy użytkownicy.  Uruchom aplikację Szybki Start i zaloguj się przy użyciu jednego z tych użytkowników.  Wyszukiwanie innych użytkowników według ich głównej nazwy użytkownika.  Zamknij aplikację, a następnie uruchom go ponownie.  Zwróć uwagę, jak sesji użytkownika pozostanie niezmieniona.

ADAL ułatwia wdrażanie wszystkich tych typowych funkcji tożsamości do aplikacji.  Trwa istotnych pracy dirty dla Ciebie — zarządzania pamięcią podręczną, obsługa protokołu OAuth, prezentowanie użytkownika z identyfikatora interfejsu użytkownika, odświeżanie wygasłe tokeny i innych.  Naprawdę wiedzieć wystarczy połączenie jednego interfejsu API `getToken`.

W przypadku odwołania, ukończony przykładowy (bez wartości konfiguracji) jest dostępna [w tym miejscu](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).  

## <a name="additional-scenarios"></a>Dodatkowe scenariusze
Możesz teraz można przejść do dodatkowych scenariuszach.  Warto wypróbować:

- [Zabezpieczanie Node.JS interfejsu API sieci Web z usługą Azure Active Directory](active-directory-devquickstarts-webapi-nodejs.md)
- Dowiedz się, [jak włączyć logowania jednokrotnego krzyżowe aplikacji w systemie iOS przy użyciu ADAL](active-directory-sso-ios.md)  

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
