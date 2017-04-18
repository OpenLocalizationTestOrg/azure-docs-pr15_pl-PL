<properties
    pageTitle="Azure AD 2.0 — iOS aplikacji | Microsoft Azure"
    description="Sposoby tworzenia aplikacji w systemie iOS znaki w użytkowników z obu osobistego konta Microsoft i konta służbowego, za pomocą bibliotek innych firm."
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/28/2016"
    ms.author="brandwe"/>

# <a name="add-sign-in-to-an-ios-app-using-a-third-party-library-with-graph-api-using-the-v20-endpoint"></a>Dodawanie logowania do aplikacji w systemie iOS biblioteki innych firm za pomocą interfejsu API wykresu za pomocą punkt końcowy 2.0

Platforma Microsoft tożsamości używa otwartych standardów, takich jak uwierzytelnianie OAuth2 i łączenie OpenID. Deweloperzy mogą używać dowolnej biblioteki, które mają być Integracja z naszych usług. Aby ułatwić programistom naszej platformy za pomocą innych bibliotek, możemy napisanych kilka instruktaży podobny do tego celu zademonstrowania sposobu konfigurowania bibliotek innych firm nawiązywania połączenia z platformy Microsoft tożsamości. Większość bibliotek implementujących [Specyfikacja uwierzytelnianie OAuth2 RFC6749](https://tools.ietf.org/html/rfc6749) można nawiązać platformy Microsoft tożsamości.

Z tą aplikacją, który tworzy w tym instruktażu użytkownicy mogą zalogować się do swojej organizacji i wyszukaj dla innych osób w organizacji za pomocą interfejsu API wykresu.

Jeśli znasz uwierzytelnianie OAuth2 lub połączyć OpenID duża część tej konfiguracji przykładowych może nie miały znaczenie dla Ciebie. Zalecamy, przeczytaj [protokoły 2.0 — OAuth 2.0 autoryzacji kod przepływ](active-directory-v2-protocols-oauth-code.md) tła.


> [AZURE.NOTE]
    Niektóre funkcje naszej platformy, które mają wyrażenia standardy uwierzytelnianie OAuth2 lub OpenID nawiązywanie połączenia, takie jak warunkowego dostępu i zarządzanie zasadami Intune, trzeba użyć naszych Otwórz źródło Microsoft Azure tożsamości bibliotek.

Punkt końcowy 2.0 nie obsługuje wszystkich scenariuszy usługi Azure Active Directory i funkcji.

> [AZURE.NOTE]
    Aby określić, należy użyć punktu końcowego 2.0, przeczytaj o [ograniczenia 2.0](active-directory-v2-limitations.md).

## <a name="download-code-from-github"></a>Pobieranie kodu z GitHub
Kod tego samouczka jest przechowywana [na GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-v2).  Aby wykonać opisane czynności, można [pobrać aplikację szkielet jako zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) lub klonowanie szkielet:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

Możesz też po prostu można pobrać próbki i razu rozpocząć pracę:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

## <a name="register-an-app"></a>Zarejestruj się w aplikacji
Tworzenie nowej aplikacji w [aplikacji portal rejestracji](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), lub wykonaj szczegółowe instrukcje opisane w temacie [jak zarejestrować aplikację z punktem końcowym 2.0](active-directory-v2-app-registration.md).  Upewnij się, że:

- Skopiuj **Identyfikator aplikacji** jest przypisany do aplikacji, ponieważ musisz je szybko.
- Dodawanie platformy **Mobile** dla aplikacji.
- Skopiuj **Przekieruj identyfikatora URI** z portalu. Należy użyć wartości domyślnej `urn:ietf:wg:oauth:2.0:oob`.


## <a name="download-the-third-party-nxoauth2-library-and-create-a-workspace"></a>Pobieranie biblioteki NXOAuth2 innych firm i tworzenie obszaru roboczego

W tym instruktażu powinny zawierać OAuth2Client z GitHub, czyli bibliotekę uwierzytelnianie OAuth2 dla systemu Mac OS X i iOS (kakao i kakao dotknij). Ta biblioteka jest oparty na projekt 10 Specyfikacja uwierzytelnianie OAuth2. Wykonuje profilu natywnej aplikacji, a obsługuje punkt końcowy autoryzacji użytkownika. Są to wszystkie zadania, które musisz Integracja z platformy Microsoft tożsamości.

### <a name="add-the-library-to-your-project-by-using-cocoapods"></a>Dodawanie biblioteki do projektu przy użyciu CocoaPods

CocoaPods jest menedżerem zależności dla projektów Xcode. Automatycznie zarządza poprzednie kroki instalacji.

```
$ vi Podfile
```
1. Dodaj następujący tekst na tym podfile:

    ```
     platform :ios, '8.0'

     target 'QuickStart' do

     pod 'NXOAuth2Client'

     end
    ```

2. Załaduj podfile przy użyciu CocoaPods. Spowoduje to utworzenie nowego obszaru roboczego Xcode, która będzie ładowana.

    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

## <a name="explore-the-structure-of-the-project"></a>Przegląd struktury projektu

Następującą strukturę konfigurowana jest dla naszych projektu szkielet:

- Widok wzorca przy użyciu wyszukiwania głównej nazwy użytkownika
- Widok szczegółów danych o wybranego użytkownika
- Widok logowania, gdzie użytkownik zalogować się do aplikacji do kwerendy na wykresie

Firma Microsoft zostaną przeniesione do różnych plików w szkielet Dodawanie uwierzytelniania. Inne części kod, na przykład kod visual nie dotyczą tożsamości, ale są dostarczane za Ciebie.

## <a name="set-up-the-settingsplst-file-in-the-library"></a>Konfigurowanie pliku settings.plst w bibliotece

-   W programie project Szybki Start, otwórz `settings.plist` pliku. Zamienianie wartości elementów w sekcji w celu odzwierciedlenia wartości, które używane w portalu Azure. Kod będzie odwoływać się do tych wartości przy każdym użyciu Active Directory Authentication Library.
    -   `clientId` Jest identyfikator klienta aplikacji, który został skopiowany w portalu.
    -   `redirectUri` Jest adres URL przekierowania podających portalu.

## <a name="set-up-the-nxoauth2client-library-in-your-loginviewcontroller"></a>Konfigurowanie biblioteki NXOAuth2Client w Twojej LoginViewController

Biblioteka NXOAuth2Client wymaga niektórych wartości w celu skonfigurowania. Po wykonaniu tego zadania służy nabytych token nawiązać połączenie z interfejsu API wykresu. Ponieważ `LoginView` będzie o nazwie dowolnej chwili trzeba służą do uwierzytelniania, jest to właściwe rozwiązanie, aby umieścić wartości w konfiguracji do tego pliku.

- Dodawanie niektórych wartości do `LoginViewController.m` plik, aby ustawić kontekst dla uwierzytelniania i autoryzacji. Szczegółowe informacje o wartości wykonaj kod.

    ```objc
    NSString *scopes = @"openid offline_access User.Read";
    NSString *authURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/authorize";
    NSString *loginURL = @"https://login.microsoftonline.com/common/login";
    NSString *bhh = @"urn:ietf:wg:oauth:2.0:oob?code=";
    NSString *tokenURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/token";
    NSString *keychain = @"com.microsoft.azureactivedirectory.samples.graph.QuickStart";
    static NSString * const kIDMOAuth2SuccessPagePrefix = @"session_state=";
    NSURL *myRequestedUrl;
    NSURL *myLoadedUrl;
    bool loginFlow = FALSE;
    bool isRequestBusy;
    NSURL *authcode;
    ```

Przyjrzyjmy się szczegółowe informacje o kodzie.

Pierwszy ciąg jest `scopes`.  `User.Read` Wartość pozwala na odczytywanie podstawowe profilu zalogowania użytkownika.

Więcej informacji o wszystkie dostępne zakresy na [zakresów uprawnień programu Microsoft Graph](https://graph.microsoft.io/docs/authorization/permission_scopes)można znaleźć.

Aby uzyskać `authURL`, `loginURL`, `bhh`, i `tokenURL`, należy używać wartości podane wcześniej. Jeśli używasz Otwórz źródło Microsoft Azure tożsamości bibliotek, firma Microsoft rozwijane te dane dla Ciebie za pomocą naszego punktu końcowego metadanych. Firma Microsoft już gotowe wytężonej pracy wyodrębniania tych wartości dla Ciebie.

`keychain` Wartość jest kontener, w którym biblioteki NXOAuth2Client będzie używane do tworzenia Pęk kluczy do przechowywania usługi tokenów. Jeśli chcesz uzyskać logowania jednokrotnego (SSO) w wielu aplikacjach, można określić samej Pęk kluczy w każdej aplikacji i żąda użycia tego Pęk kluczy w swojej uprawnień Xcode. Jest to wyjaśnione w dokumentacji firmy Apple.

Pozostałe wartości te są wymagane do używania biblioteki i Tworzenie miejsca, w których można wykonać wartości do kontekstu.

### <a name="create-a-url-cache"></a>Tworzenie pamięci podręcznej adresów URL

Wewnątrz `(void)viewDidLoad()`, zawsze nazywane po załadowaniu widoku, poniższy kod primes pamięci podręcznej naszych użytku.

Dodaj następujący kod:

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    self.loginView.delegate = self;
    [self setupOAuth2AccountStore];
    [self requestOAuth2Access];
    NSURLCache *URLCache = [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                                         diskCapacity:20 * 1024 * 1024
                                                             diskPath:nil];
    [NSURLCache setSharedURLCache:URLCache];

}
```

### <a name="create-a-webview-for-sign-in"></a>Tworzenie widoku sieci Web dla logowania

Widok sieci Web można wyświetli monit o dodatkowe czynników, takich jak wiadomości SMS (jeśli został skonfigurowany) lub zwrócić komunikaty o błędach do użytkownika. W tym miejscu skonfigurujesz widok sieci Web w górę, a następnie wpisz później kodu do obsługi zwrotne, wykonywanych w widoku sieci Web z usług tożsamości.

```objc
-(void)requestOAuth2Access {
    //to sign in to Microsoft APIs using OAuth2, we must show an embedded browser (UIWebView)
    [[NXOAuth2AccountStore sharedStore] requestAccessToAccountWithType:@"myGraphService"
                                   withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
                                       //navigate to the URL returned by NXOAuth2Client

                                       NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
                                       [self.loginView loadRequest:r];
                                   }];
}
```

### <a name="override-the-webview-methods-to-handle-authentication"></a>Zastępowanie metody widok sieci Web do obsługi uwierzytelniania

Aby sprawdzić widok sieci Web, co się dzieje, gdy użytkownik musi się zalogować, jak wcześniej wspomniano, można wkleić poniższy kod.

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {

    // We get the auth token from a redirect so we need to handle that in the webview.

    if (![NSThread isMainThread]) {
        [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:) withObject:URL waitUntilDone:YES];
        return;
    }

    NSURLRequest *hostnameURLRequest = [NSURLRequest requestWithURL:URL cachePolicy:NSURLRequestUseProtocolCachePolicy timeoutInterval:10.0f];
    isRequestBusy = YES;
    [self.loginView loadRequest:hostnameURLRequest];

    NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)", self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType {

    NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL, (long)navigationType);

    // The webview is where all the communication happens. Slightly complicated.

    myLoadedUrl = [webView.request mainDocumentURL];
    NSLog(@"***Loaded url: %@", myLoadedUrl);

    //if the UIWebView is showing our authorization URL or consent URL, show the UIWebView control
    if ([request.URL.absoluteString rangeOfString:authURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:loginURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:bhh options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = YES;
        [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }
    else {
        self.loginView.hidden = NO;
        //read the Location from the UIWebView, this is how Microsoft APIs is returning the
        //authentication code and relation information. This is controlled by the redirect URL we chose to use from Microsoft APIs
        //continue the OAuth2 flow
       // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }

    return YES;

}
```

### <a name="write-code-to-handle-the-result-of-the-oauth2-request"></a>Pisanie kodu do obsługi wynik żądania uwierzytelnianie OAuth2

Poniższy kod będzie obsługiwany redirectURL, która zwraca widok sieci Web. Jeśli uwierzytelnianie nie powiedzie, kod będzie spróbuj ponownie. W międzyczasie biblioteki zawiera błąd, który można wyświetlić w konsoli lub obsługiwać asynchroniczne.

```objc
- (void)handleOAuth2AccessResult:(NSString *)accessResult {

    AppData* data = [AppData getInstance];

    //parse the response for success or failure
     if (accessResult)
    //if success, complete the OAuth2 flow by handling the redirect URL and obtaining a token
     {
         [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
    } else {
        //start over
        [self requestOAuth2Access];
    }
}
```

### <a name="set-up-the-oauth-context-called-account-store"></a>Konfigurowanie kontekstu OAuth (nazywanych magazynu kont)

W tym miejscu możesz zadzwonić `-[NXOAuth2AccountStore setClientID:secret:authorizationURL:tokenURL:redirectURL:forAccountType:]` magazynu udostępnionego konta dla każdej usługi, którą chcesz mieć dostęp do aplikacji. Typ konta jest ciąg, który jest używany jako identyfikator niektórych usługi. Ponieważ uzyskują dostęp do interfejsu API wykresu, kod odwołuje się do go jako `"myGraphService"`. Następnie skonfigurowaniu obserwatora informujący, kiedy zmienią się jakiekolwiek założenia przy użyciu tokenu. Po pobraniu tokenu powrocie użytkownika z powrotem do `masterView`.



```objc
- (void)setupOAuth2AccountStore {


        AppData* data = [AppData getInstance];

    [[NXOAuth2AccountStore sharedStore] setClientID:data.clientId
                                             secret:data.secret
                                              scope:[NSSet setWithObject:scopes]
                                   authorizationURL:[NSURL URLWithString:authURL]
                                           tokenURL:[NSURL URLWithString:tokenURL]
                                        redirectURL:[NSURL URLWithString:data.redirectUriString]
                                      keyChainGroup: keychain
                                     forAccountType:@"myGraphService"];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      if (aNotification.userInfo) {
                                                          //account added, we have access
                                                          //we can now request protected data
                                                          NSLog(@"Success!! We have an access token.");
                                                          dispatch_async(dispatch_get_main_queue(),^ {

                                                              MasterViewController* masterViewController = [self.storyboard instantiateViewControllerWithIdentifier:@"masterView"];
                                                              [self.navigationController pushViewController:masterViewController animated:YES];
                                                          });
                                                      } else {
                                                          //account removed, we lost access
                                                      }
                                                  }];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      NSError *error = [aNotification.userInfo objectForKey:NXOAuth2AccountStoreErrorKey];
                                                      NSLog(@"Error!! %@", error.localizedDescription);
                                                  }];
}
```

## <a name="set-up-the-master-view-to-search-and-display-the-users-from-the-graph-api"></a>Konfigurowanie widoku wzorca do wyszukiwania i wyświetlania użytkowników z interfejsu API wykresu

Aplikację wzorzec-widok-kontroler (MVC), która zawiera zwrócone dane w siatce wykracza poza zakres tego instruktażu, a wiele podręczniki online wyjaśniono, jak utworzyć. Ten kod znajduje się w pliku szkielet. Jednak należy zająć się kilka rzeczy w tej aplikacji MVC:

* ODCIĘTA, jeśli użytkownik wpisze czegoś w polu wyszukiwania
* Udostępnić obiektu danych powrót do MasterView, aby go wyświetlić wyniki w siatce

Firma Microsoft, wykonaj te poniżej.

### <a name="add-a-check-to-see-if-youre-logged-in"></a>Dodaj znacznik wyboru, aby wyświetlić, jeśli Cię zalogowano

Aplikacja ma małą, jeśli użytkownik nie jest zalogowany, dlatego warto Sprawdź, czy jest już tokenu w pamięci podręcznej. W przeciwnym razie przekierowanie do LoginView dla użytkownika do zalogowania się. Jeśli odwołać, najlepszym sposobem aby zrobić coś podczas ładowania widoku jest za pomocą `viewDidLoad()` metodę Apple znajdują się z nami.

```objc
- (void)viewDidLoad {
    [super viewDidLoad];


    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];

        if (accounts.count == 0) {

        dispatch_async(dispatch_get_main_queue(),^ {

            LoginViewController* userSelectController = [self.storyboard instantiateViewControllerWithIdentifier:@"LoginUserView"];
            [self.navigationController pushViewController:userSelectController animated:YES];
        });
        }
```

### <a name="update-the-table-view-when-data-is-received"></a>Aktualizowanie widoku tabeli po odebraniu danych

Po interfejsie API wykresu zwraca dane, należy wyświetlić dane. Dla uproszczenia Oto cały kod, aby zaktualizować tabelę. W kodzie standardowy MVC można wkleić tylko właściwych wartości.

```objc
#pragma mark - Table View

- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView {
    return 1;
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {

        return [upnArray count];
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {

    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"TaskPrototypeCell" forIndexPath:indexPath];

    if ( cell == nil ) {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:@"TaskPrototypeCell"];
    }

    User *user = nil;
     user = [upnArray objectAtIndex:indexPath.row];


    // Configure the cell
    cell.textLabel.text = user.name;
    [cell setAccessoryType:UITableViewCellAccessoryDisclosureIndicator];

    return cell;
}

```

### <a name="provide-a-way-to-call-the-graph-api-when-someone-types-in-the-search-field"></a>Umożliwia połączenie interfejsu API wykres, gdy użytkownik wpisze w polu wyszukiwania

Jeśli użytkownik wpisze wyszukiwania w polu, należy shove która API wykresu. `GraphAPICaller` Klasy, która zostanie utworzona poniższy kod, oddziela funkcję wyszukiwania z prezentacji. Na obecnym Przejdźmy wpisz kod, którego kanałów znaków wyszukiwania interfejsu API wykresu. Firma Microsoft wykonaj następujące czynności, dostarczając metodę o nazwie `lookupInGraph`, która przyjmuje ciąg, który chcemy wyszukać.

```objc

-(void)lookupInGraph:(NSString *)searchText {
if (searchText.length > 0) {

    };



        [GraphAPICaller searchUserList:searchText completionBlock:^(NSMutableArray* returnedUpns, NSError* error) {
            if (returnedUpns) {


                upnArray = returnedUpns;


            }
            else
            {
                UIAlertView *alertView = [[UIAlertView alloc]initWithTitle:nil message:[[NSString alloc]initWithFormat:@"Error : %@", error.localizedDescription] delegate:nil cancelButtonTitle:@"Retry" otherButtonTitles:@"Cancel", nil];

                [alertView setDelegate:self];

                dispatch_async(dispatch_get_main_queue(),^ {
                    [alertView show];
                });
            }


        }];


}
```

## <a name="write-a-helper-class-to-access-the-graph-api"></a>Pisanie klasy pomocy, aby uzyskać dostęp do interfejsu API wykresu

To jest podstawową naszych aplikacji. Należy pozostałych została wstawiania kodu w domyślnym wzorcem MVC firmy Apple, poniżej pisania kodu do wykonywania kwerend wykresu, gdy użytkownik wpisuje i zwróć dane. Oto kod i następuje szczegółowy opis.

### <a name="create-a-new-objective-c-header-file"></a>Tworzenie nowego pliku nagłówka cel C

Nadaj plikowi nazwę `GraphAPICaller.h`i Dodaj następujący kod.

```objc
@interface GraphAPICaller : NSObject<NSURLConnectionDataDelegate>

+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray*, NSError* error))completionBlock;

@end
```

W tym miejscu pojawi się ten metodę określony ciąg znaków i zwraca completionBlock. Ten completionBlock w możesz może mieć odgadnąć, będzie aktualizowany tabeli za pomocą obiektu z wypełnione dane w czasie rzeczywistym jako wyszukiwanie użytkowników.


### <a name="create-a-new-objective-c-file"></a>Tworzenie nowego pliku cel C

Nadaj plikowi nazwę `GraphAPICaller.m`i dodaj następujące metody.

```objc
+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    AppData* data = [AppData getInstance];

    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];

    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSDictionary* params = [self convertParamsToDictionary:searchString];

    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

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
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


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

```

Przyjrzyjmy się za pomocą tej metody można użyć szczegółowo.

Podstawową kod znajduje się w `NXOAuth2Request`, metodę, która pobiera parametry, które już zdefiniowany przez użytkownika w pliku settings.plist.

Pierwszym krokiem jest sporządzenie prawo połączenia interfejsu API wykresu. Ponieważ dzwonisz `/users`, możesz określić, że dołączając do zasobu interfejsu API wykresu wraz z wersji. Jest to właściwe rozwiązanie, aby umieścić w pliku ustawienia zewnętrznego, ponieważ można to zmienić API rozwoju.


```objc
NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];
```

Następnie należy określić parametry, które będą również umożliwiają połączenie interfejsu API wykresu. Jest *bardzo ważne,* że nie umieszczać parametry w punkt końcowy zasobów, ponieważ jest który wyczyszczonym dla wszystkich innych niż URI zgodnych znaków w czasie rzeczywistym. Cały kod kwerendy należy podać w parametrach.

```objc

NSDictionary* params = [self convertParamsToDictionary:searchString];
```

Może się okazać wymaga `convertParamsToDictionary` metodę, która jeszcze nie napisano jeszcze. Załóżmy to teraz zrobić na końcu pliku:

```objc
+(NSDictionary*) convertParamsToDictionary:(NSString*)searchString
{
    NSMutableDictionary* dictionary = [[NSMutableDictionary alloc]init];

        NSString *query = [NSString stringWithFormat:@"startswith(givenName, '%@')", searchString];

           [dictionary setValue:query forKey:@"$filter"];



    return dictionary;
}

```
Następnie można użyć `NXOAuth2Request` metoda odzyskać danych z interfejsu API w formacie JSON.

```objc
NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];
```

Na koniec Przyjrzyjmy się jak przekazać je do MasterViewController. Dane zwraca jako seryjny i musi być rozszeregować i załadować obiektu, który może wykorzystać MainViewController. W tym celu ma szkielet `User.m/h` pliku, który tworzy obiekt użytkownika. Wypełnij ten obiekt użytkownika na podstawie informacji z wykresu.

```objc
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
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


                               [Users addObject:s];
```


## <a name="run-the-sample"></a>Uruchamianie próbki

Jeśli używany szkielet lub obserwowane wraz z Instruktaż aplikacji powinna teraz działać. Uruchom simulator i kliknij przycisk **Zaloguj się** do korzystania z aplikacji.

## <a name="get-security-updates-for-our-product"></a>Uzyskiwanie aktualizacji zabezpieczeń systemu nasze produkty

Zachęcamy do otrzymywać powiadomienia o kiedy bezpieczeństwem występują, odwiedź witrynę [TechCenter zabezpieczeń](https://technet.microsoft.com/security/dd252948) i subskrybowania doradcze alertów zabezpieczeń.
