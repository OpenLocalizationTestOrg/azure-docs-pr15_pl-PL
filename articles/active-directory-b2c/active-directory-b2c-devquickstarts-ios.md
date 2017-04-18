<properties
    pageTitle="Azure Active Directory B2C: Połączenie interfejsu API sieci web z aplikacji iOS korzystania z bibliotek innych firm | Microsoft Azure"
    description="W tym artykule opisano, jak utworzyć aplikację "listy zadań do wykonania" iOS wywołującego Node.js interfejsu API sieci web przy użyciu tokenów okaziciela OAuth 2.0 przy użyciu biblioteki innej firmy"
    services="active-directory-b2c"
    documentationCenter="ios"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

< tagi ms.service= "ruch b2c active directory" ms.workload="identity" ms.tgt_pltfrm="na" ms.devlang="objectivec" ms.topic= "główny artykuł"

    ms.date="07/26/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c--call-a-web-api-from-an-ios-application-using-a-third-party-library"></a>Azure AD B2C: Połączenie interfejsu API sieci web z aplikacji iOS przy użyciu biblioteki innej firmy

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Platforma Microsoft tożsamości używa otwartych standardów, takich jak uwierzytelnianie OAuth2 i łączenie OpenID. Dzięki temu deweloperzy mogą korzystać z dowolnej bibliotece, które chcą Integracja z naszych usług. Do pomocy deweloperów w naszej platformy za pomocą innych bibliotek napisanych możemy kilka instruktaży podobny do tego celu demonstate sposobu konfigurowania bibliotek innych firm nawiązywania połączenia z platformy Microsoft tożsamości. Większość bibliotek implementujących [Specyfikacja uwierzytelnianie OAuth2 RFC6749](https://tools.ietf.org/html/rfc6749) będzie można nawiązać połączenia z platformy Microsoft Identity.


Jeśli jesteś nowym użytkownikiem uwierzytelnianie OAuth2 lub połączyć OpenID duża część tej konfiguracji przykładowych może nie miały znaczenie ilości dla Ciebie. Zalecamy zapoznanie przeglądać krótkie [Omówienie protokół, którego została firma Microsoft opisanych tutaj](active-directory-b2c-reference-protocols.md).

> [AZURE.NOTE]
    Niektóre funkcje naszej platformy, które mają wyrażenia w standardy, takich jak zarządzanie zasadami dostępu warunkowego i Intune trzeba użyć naszych Otwórz źródło Microsoft Azure tożsamości bibliotek. 
   
Nie wszystkie scenariusze usługi Azure Active Directory i funkcje są obsługiwane przez platformę B2C.  Aby określić, należy użyć platformy B2C, przeczytaj o [B2C ograniczenia](active-directory-b2c-limitations.md).


## <a name="get-an-azure-ad-b2c-directory"></a>Pobieranie katalogu Azure AD B2C

Zanim będzie można używać Azure AD B2C, należy utworzyć katalogu lub dzierżawa. Katalog jest kontenerem dla wszystkich użytkowników, aplikacje, grupy i. Jeśli nie masz już konto, [Utwórz katalog B2C](active-directory-b2c-get-started.md) przed Kontynuuj.

## <a name="create-an-application"></a>Tworzenie aplikacji

Następnie należy utworzyć aplikację w katalogu B2C. Dzięki temu Azure AD informacji wymaganych do bezpiecznego komunikowania się z aplikacji. Zarówno aplikacji, jak i interfejsu API sieci web są reprezentowane przez pojedynczy **Identyfikator aplikacji** w tym przypadku ponieważ one obejmować jedną aplikację logiczne. Aby utworzyć aplikację, wykonaj [następujące czynności](active-directory-b2c-app-registration.md). Pamiętaj, aby:

- Dołączanie z **urządzenia przenośnego** w aplikacji.
- Skopiuj **Identyfikator aplikacji** jest przypisana do aplikacji. Musisz także to później.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Tworzenie zasad

W Azure AD B2C co środowisko użytkownika jest zdefiniowany przez [zasady](active-directory-b2c-reference-policies.md). Ta aplikacja zawiera jeden obsługi tożsamości: Scalonej logowania w i tworzenia konta. Musisz utworzyć tych zasad każdego typu, zgodnie z opisem w [artykule referencyjnym zasad](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Po utworzeniu zasady, należy:

- Wybierz **nazwę wyświetlaną** i atrybuty zapisów w zasad.
- Wybierz **nazwę wyświetlaną** i **Identyfikator obiektu** oświadczeń aplikacji w każdej zasady. Możesz także inne roszczenia.
- Skopiuj **nazwy** każdej zasady po jego utworzeniu. Prefiks powinien mieć `b2c_1_`.  Nazwa zasady będzie potrzebna później.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Po utworzeniu zasad możesz już przystąpić do tworzenia aplikacji.


## <a name="download-the-code"></a>Pobieranie kodu

Kod tego samouczka jest przechowywana [na GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c).  Aby wykonać opisane czynności, możesz [pobrać aplikację jako zip](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c)/archive/master.zip) lub klonowanie go:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

Lub pobrać złożonym kodu i rozpocząć pracę od razu: 

```
git clone --branch complete git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

## <a name="download-the-third-party-library-nxoauth2-and-launch-a-workspace"></a>Pobierz nxoauth2 biblioteki innej firmy i uruchamianie obszaru roboczego

Dla tego instruktażu użyjemy OAuth2Client z GitHub, biblioteką uwierzytelnianie OAuth2 dla systemu Mac OS X i iOS (kakao i kakao dotknij). Ta biblioteka jest oparty na projekt 10 Specyfikacja uwierzytelnianie OAuth2. Wykonuje profilu natywnej aplikacji, a obsługuje punkt końcowy autoryzacji użytkowników końcowych. Są to wszystkie czynności, które będą potrzebne w celu integrat przy użyciu programu Microsoft tożsamości platformy.

### <a name="adding-the-library-to-your-project-using-cocoapods"></a>Dodawanie biblioteki do projektu przy użyciu CocoaPods

CocoaPods jest menedżerem zależności dla projektów Xcode. Powyższe kroki instalacji zarządza automatycznie.

```
$ vi Podfile
```
Dodaj następujący tekst na tym podfile:

```
 platform :ios, '8.0'
 
 target 'SampleforB2C' do
 
 pod 'NXOAuth2Client'
 
 end
```

Teraz załadować podfile przy użyciu cocoapods. Spowoduje to utworzenie nowego obszaru roboczego XCode ładowania.

```
$ pod install
...
$ open SampleforB2C.xcworkspace

```

## <a name="the-structure-of-the-project"></a>Struktura projektu

Firma Microsoft ma następującą strukturę skonfigurować dla naszych projektu szkielet:

* **Widok wzorca slajdów** z okienka zadań
* **Dodawanie widoku zadań** , aby dane dotyczące zaznaczonego zadania
* **Widok logowania** , który umożliwia użytkownikowi logowania do aplikacji.

Firma Microsoft spowoduje przejście do różnych plików w programie project, aby dodać uwierzytelniania. Inne części kodu, takich jak kod visual jest germane tożsamości i są dostępne dla Ciebie.

## <a name="create-the-settingsplist-file-for-your-application"></a>Tworzenie `settings.plist` plik aplikacji

Jest łatwiejsze skonfigurować aplikację, jeśli mamy scentralizowanej lokalizacji w celu umieszczenia naszych wartości konfiguracji. Pomaga także zrozumieć, co oznacza każdego ustawienia w aplikacji. Firma Microsoft będzie używać *Listy właściwości* jako sposobem udostępnienia te wartości do aplikacji.

* Tworzenie lub otwieranie `settings.plist` pliku w obszarze `Supporting Files` w obszarze roboczym aplikacji

* Wprowadź następujące wartości (zostały one opisane przez nich szczegółowo wkrótce)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>accountIdentifier</key>
    <string>B2C_Acccount</string>
    <key>clientID</key>
    <string><client ID></string>
    <key>clientSecret</key>
    <string></string>
    <key>authURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/authorize?p=<policy name></string>
    <key>loginURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/login</string>
    <key>bhh</key>
    <string>urn:ietf:wg:oauth:2.0:oob</string>
    <key>tokenURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/token?p=<policy name></string>
    <key>keychain</key>
    <string>com.microsoft.azureactivedirectory.samples.graph.QuickStart</string>
    <key>contentType</key>
    <string>application/x-www-form-urlencoded</string>
    <key>taskAPI</key>
    <string>https://aadb2cplayground.azurewebsites.net</string>
</dict>
</plist>
```

Przyjrzyjmy w do nich szczegółowo.


Aby uzyskać `authURL`, `loginURL`, `bhh`, `tokenURL` można zauważyć, co jest potrzebne do wypełnienia nazwy dzierżawy. To nazwa dzierżawy dzierżawy usługi B2C przypisany do Ciebie. Na przykład `kidventusb2c.onmicrosoft.com`. Jeśli korzystasz z naszych Otwórz źródło Microsoft Azure tożsamości bibliotek firma Microsoft może rozwijane te dane przy użyciu naszych końcowy metadanych. Firma Microsoft już gotowe wytężonej pracy wyodrębniania tych wartości dla Ciebie.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

`keychain` Wartość jest kontener, w którym Biblioteka NXOAuth2Client będzie używane do tworzenia Pęk kluczy do przechowywania usługi tokenów. Jeśli chcesz uzyskać logowania jednokrotnego w aplikacjach wiele możesz można określić samej Pęk kluczy w poszczególnych aplikacji a także żąda użycia tego Pęk kluczy w swojej entitements XCode. Jest to opisane w dokumentacji firmy Apple.

`<policy name>` Na końcu każdego adresu URL są miejsca miejsce, w którym chcesz umieścić zasady utworzone powyżej. Aplikacja będzie połączenia te zasady, w zależności od przepływu.

`taskAPI` Punkt końcowy pozostałych nazywamy z token B2C albo dodać zadania lub zadania istniejących zapytań. To ustawiono specjalnie dla tego przykładu. Nie musisz zmienić Aby przykład działał.

Pozostałe wartości te są wymagane do używania biblioteki i po prostu utworzyć miejsca, w których można wykonać wartości do kontekstu.

Teraz gdy mamy już `settings.plist` pliku utworzonego, potrzebujemy kod, aby ją przeczytać.

## <a name="set-up-a-appdata-class-to-read-our-settings"></a>Konfigurowanie klasy AppData odczytać naszych ustawień

Można wprowadzić prosty plik, po prostu analizowania naszych `settngs.plist` pliku możemy utworzony w poprzednim przykładzie i wprowadź te ustawienia avaialble w przyszłości dla każdej klasy. Ponieważ firma Microsoft nie chcesz, aby utworzyć nową kopię danych każdorazowo klasy szuka go, firma Microsoft będzie użyć deseniu pojedynczych i zwrócić tylko tego samego wystąpienia utworzone każdym razem, gdy pojawi się żądanie ustawień

* Tworzenie `AppData.h` pliku:

```objc
#import <Foundation/Foundation.h>

@interface AppData : NSObject

@property(strong) NSString *accountIdentifier;
@property(strong) NSString *taskApiString;
@property(strong) NSString *authURL;
@property(strong) NSString *clientID;
@property(strong) NSString *loginURL;
@property(strong) NSString *bhh;
@property(strong) NSString *keychain;
@property(strong) NSString *tokenURL;
@property(strong) NSString *clientSecret;
@property(strong) NSString *contentType;

+ (id)getInstance;

@end
```

* Tworzenie `AppData.m` pliku:

```objc
#import "AppData.h"

@implementation AppData

+ (id)getInstance {
  static AppData *instance = nil;
  static dispatch_once_t onceToken;

  dispatch_once(&onceToken, ^{
    instance = [[self alloc] init];

    NSDictionary *dictionary = [NSDictionary
        dictionaryWithContentsOfFile:[[NSBundle mainBundle]
                                         pathForResource:@"settings"
                                                  ofType:@"plist"]];
    instance.accountIdentifier = [dictionary objectForKey:@"accountIdentifier"];
    instance.clientID = [dictionary objectForKey:@"clientID"];
    instance.clientSecret = [dictionary objectForKey:@"clientSecret"];
    instance.authURL = [dictionary objectForKey:@"authURL"];
    instance.loginURL = [dictionary objectForKey:@"loginURL"];
    instance.bhh = [dictionary objectForKey:@"bhh"];
    instance.tokenURL = [dictionary objectForKey:@"tokenURL"];
    instance.keychain = [dictionary objectForKey:@"keychain"];
    instance.contentType = [dictionary objectForKey:@"contentType"];
    instance.taskApiString = [dictionary objectForKey:@"taskAPI"];

  });

  return instance;
}
@end
```

Teraz możemy ułatwić sobie dostęp w naszych danych, po prostu dzwoniąc `  AppData *data = [AppData getInstance];` jeden z naszych klas podczas widoczne poniżej.



## <a name="set-up-the-nxoauth2client-library-in-your-appdelegate"></a>Konfigurowanie biblioteki NXOAuth2Client w Twojej AppDelegate

Biblioteka NXOAuthClient wymaga niektórych wartości w celu skonfigurowania. Jeden raz, czyli pełną możesz użyć tokenu, który jest pobrane połączenie interfejsu API usługi REST. Ponieważ firma Microsoft wiedzieć, że `AppDelegate` zostanie wywołana dowolnej chwili możemy ładowanie aplikacji jest to właściwe rozwiązanie, możemy umieścić wartości w naszym konfiguracji do tego pliku.
* Otwórz `AppDelegate.m` pliku

* Importowanie niektóre pliki nagłówek, który użyjemy później.

```objc
#import "NXOAuth2.h" // the Identity library we are using
#import "AppData.h" // the class we just created we will use to load the settings of our application
```

* Dodawanie `setupOAuth2AccountStore` metody AppDelegate

Trzeba utworzyć AccountStore i umieść go po danych możemy po prostu Odczyt w `settings.plist` pliku.

Istnieje kilka rzeczy, które należy pamiętać o dotyczące B2C usługi na tym etapie wprowadzająca kod bardziej zrozumiałe:


1. Azure AD B2C używa *zasad* zgodnie z parametrami kwerendy do obsługi żądania. Dzięki temu usługi Azure Active Directory ma pełnić rolę usługi niezależnych tylko dla aplikacji. W celu zapewnienia kwerendy te dodatkowe parametry należy podać `kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:` metody z parametrami naszych zasad niestandardowych. 

2. Azure AD B2C używa zakresów w większości tak samo jako inny serwer uwierzytelnianie OAuth2. Jednak ponieważ użycie B2C jest tyle o uwierzytelnianiu użytkownika jako uzyskiwanie dostępu do zasobów niektórych zakresów są naprawdę wymagane dla przepływu pracy poprawnie. Jest to `openid` zakres. Nasze tożsamości Microsoft SDK automatycznie zapewniają `openid` zakres dla użytkownika nie widzisz go w naszym konfiguracji SDK. Ponieważ użyto biblioteki innej firmy, jednak należy określić ten zakres.

```objc
- (void)setupOAuth2AccountStore {
  AppData *data = [AppData getInstance]; // The singleton we use to get the settings

  NSDictionary *customHeaders =
      [NSDictionary dictionaryWithObject:@"application/x-www-form-urlencoded"
                                  forKey:@"Content-Type"];

  // Azure B2C needs
  // kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters for
  // sending policy to the server,
  // therefore we use -setConfiguration:forAccountType:
  NSDictionary *B2cConfigDict = @{
    kNXOAuth2AccountStoreConfigurationClientID : data.clientID,
    kNXOAuth2AccountStoreConfigurationSecret : data.clientSecret,
    kNXOAuth2AccountStoreConfigurationScope :
        [NSSet setWithObjects:@"openid", data.clientID, nil],
    kNXOAuth2AccountStoreConfigurationAuthorizeURL :
        [NSURL URLWithString:data.authURL],
    kNXOAuth2AccountStoreConfigurationTokenURL :
        [NSURL URLWithString:data.tokenURL],
    kNXOAuth2AccountStoreConfigurationRedirectURL :
        [NSURL URLWithString:data.bhh],
    kNXOAuth2AccountStoreConfigurationCustomHeaderFields : customHeaders,
    //      kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:customAuthenticationParameters
  };

  [[NXOAuth2AccountStore sharedStore] setConfiguration:B2cConfigDict
                                        forAccountType:data.accountIdentifier];
}
```
Następnie upewnij się, nadaj jej AppDelegate w obszarze `didFinishLaunchingWithOptions:` metody. 

```
[self setupOAuth2AccountStore];
```


## <a name="create-a-loginviewcontroller-class-that-we-will-use-to-handle-authentication-requests"></a>Tworzenie `LoginViewController` klasy będzie używany do obsługi uwierzytelniania żądań

Firma Microsoft korzysta z widoku sieci Web dla logowania konta. Pozwala na monit o dodatkowe czynników, takich jak wiadomości SMS (jeśli został skonfigurowany) lub zwrot komunikaty o błędach do użytkownika. W tym miejscu będzie możemy zdefiniować widok sieci Web w górę, a następnie wpisz później kodu do obsługi zwrotne, wykonywanych w widoku sieci Web w usłudze Microsoft tożsamości.

* Tworzenie `LoginViewController.h` zajęć

```objc
@interface LoginViewController : UIViewController <UIWebViewDelegate>
@property(weak, nonatomic) IBOutlet UIWebView *loginView; // Our webview that we will use to do authentication

- (void)handleOAuth2AccessResult:(NSURL *)accessResult; // Allows us to get a token after we've received an Access code.
- (void)setupOAuth2AccountStore; // We will need to add to our OAuth2AccountStore we setup in our AppDelegate
- (void)requestOAuth2Access; // This is where we invoke our webview.
```

Zostanie utworzony każdej z tych metod poniżej.

> [AZURE.NOTE] 
    Upewnij się, że powiąże `loginView` do rzeczywisty widok sieci Web, która znajduje się wewnątrz z serii ujęć. W przeciwnym razie nie będzie miał widok sieci Web, którą można wyskakujące w terminie do uwierzytelnienia.

* Tworzenie `LoginViewController.m` zajęć

* Dodawanie niektóre zmienne przeprowadzenie stan, jak firma Microsoft uwierzytelniania

```objc
NSURL *myRequestedUrl; \\ The URL request to Azure Active Directory 
NSURL *myLoadedUrl; \\ The URL loaded for Azure Active Directory
bool loginFlow = FALSE; 
bool isRequestBusy; \\ A way to give status to the thread that the request is still happening
NSURL *authcode; \\ A placeholder for our auth code.
```

* Zastępowanie metody widok sieci Web do obsługi uwierzytelniania

Trzeba określić zachowanie, które będą Jeżeli użytkownik musi się zalogować, zgodnie z powyższym opisem widok sieci Web. Po prostu można wyciąć i wkleić kod poniżej.

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {
  // We get the auth token from a redirect so we need to handle that in the
  // webview.

  if (![NSThread isMainThread]) {
    [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:)
                           withObject:URL
                        waitUntilDone:YES];
    return;
  }

  NSURLRequest *hostnameURLRequest =
      [NSURLRequest requestWithURL:URL
                       cachePolicy:NSURLRequestUseProtocolCachePolicy
                   timeoutInterval:10.0f];
  isRequestBusy = YES;
  [self.loginView loadRequest:hostnameURLRequest];

  NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)",
        self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView
    shouldStartLoadWithRequest:(NSURLRequest *)request
                navigationType:(UIWebViewNavigationType)navigationType {
  AppData *data = [AppData getInstance];

  NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL,
        (long)navigationType);

  // The webview is where all the communication happens. Slightly complicated.

  myLoadedUrl = [webView.request mainDocumentURL];
  NSLog(@"***Loaded url: %@", myLoadedUrl);

  // if the UIWebView is showing our authorization URL or consent URL, show the
  // UIWebView control
  if ([request.URL.absoluteString rangeOfString:data.authURL
                                        options:NSCaseInsensitiveSearch]
          .location != NSNotFound) {
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.loginURL
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.bhh
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = YES;
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  } else {
    self.loginView.hidden = NO;
    // read the Location from the UIWebView, this is how Microsoft APIs is
    // returning the
    // authentication code and relation information. This is controlled by the
    // redirect URL we chose to use from Microsoft APIs
    // continue the OAuth2 flow
    // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  }

  return YES;
}

```

* Pisanie kodu do obsługi wynik żądania uwierzytelnianie OAuth2

Potrzebujemy kod, który będzie obsługiwał redirectURL, które pochodzą widok sieci Web. Jeśli nie została pomyślnie, firma Microsoft będzie spróbuj ponownie. W międzyczasie biblioteki zapewni błędu można wyświetlić w konsoli lub obsługiwać asyncronously. 

```objc
- (void)handleOAuth2AccessResult:(NSURL *)accessResult {
  // parse the response for success or failure
  if (accessResult)
  // if success, complete the OAuth2 flow by handling the redirect URL and
  // obtaining a token
  {
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
  } else {
    // start over
    [self requestOAuth2Access];
  }
}
```

* Konfigurowanie fabryki powiadomienie.

Tworzymy ten sam sposób, jak firma Microsoft w `AppDelegate` powyżej, ale tym razem zostanie dodana niektóre `NSNotification`s, aby Powiedz nam, co się dzieje w naszej usługi. Konfigurowanie możemy obserwatora, który będzie Powiedz nam, kiedy zmienią się jakiekolwiek założenia przy użyciu tokenu. Gdy będziemy korzystać z tokenu zwróconych użytkownika do `masterView`.



```objc
- (void)setupOAuth2AccountStore {
  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                if (aNotification.userInfo) {
                  // account added, we have access
                  // we can now request protected data
                  NSLog(@"Success!! We have an access token.");
                  dispatch_async(dispatch_get_main_queue(), ^{

                    MasterViewController *masterViewController =
                        [self.storyboard
                            instantiateViewControllerWithIdentifier:@"master"];
                    [self.navigationController
                        pushViewController:masterViewController
                                  animated:YES];
                  });
                } else {
                  // account removed, we lost access
                }
              }];

  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                NSError *error = [aNotification.userInfo
                    objectForKey:NXOAuth2AccountStoreErrorKey];
                NSLog(@"Error!! %@", error.localizedDescription);
              }];
}

```
* Dodaj kod, który obsługuje użytkownika, gdy żądanie została zainicjowana dla macierzystego logowania

Tworzenie metodę, która zostanie wywołana zawsze, gdy mamy żądania uwierzytelniania. Są to metody, który tworzy widok sieci Web

```objc
- (void)requestOAuth2Access {
  AppData *data = [AppData getInstance];

  // in order to login to Mircosoft APIs using OAuth2 we must show an embedded
  // browser (UIWebView)
  [[NXOAuth2AccountStore sharedStore]
           requestAccessToAccountWithType:data.accountIdentifier
      withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
        // navigate to the URL returned by NXOAuth2Client

        NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
        [self.loginView loadRequest:r];
      }];
}
```

* Na koniec załóżmy połączenie tych metod napisanych pracujemy nad każdym `LoginViewController` ładowania. Firma Microsoft to zrobić przez dodanie tych sposobów naszych `viewDidLoad` daje metody firmy Apple

```objc
  [super viewDidLoad];
  // Do any additional setup after loading the view.

  // OAuth2 Code

  self.loginView.delegate = self;
  [self requestOAuth2Access];
  [self setupOAuth2AccountStore];
  NSURLCache *URLCache =
      [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                    diskCapacity:20 * 1024 * 1024
                                        diskPath:nil];
  [NSURLCache setSharedURLCache:URLCache];
```

Po zakończeniu teraz tworzyć główny sposób możemy będzie interakcję z naszych aplikacji do zalogowania się. Po możemy po zalogowaniu się potrzebujemy za pomocą naszego tokeny, które otrzymano firma Microsoft. W tym utworzymy niektórych Pomocnik kod, który będzie połączenia pozostałych interfejsy API dla nas, używając tej biblioteki.


## <a name="create-a-graphapicaller-class-to-handle-our-requests-to-a-rest-api"></a>Tworzenie `GraphAPICaller` klasy obsługi naszych żądania interfejsu API usługi REST

Mamy konfiguracji załadowane każdorazowo możemy ładowanie naszych aplikacji. Teraz trzeba z nim coś zrobić, gdy mamy tokenu. 

* Tworzenie `GraphAPICaller.h` pliku

```objc
@interface GraphAPICaller : NSObject <NSURLConnectionDataDelegate>

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock;

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *error))completionBlock;

@end
```

Z tego kodu zobaczysz, że firma Microsoft będą tworzyli dwie metody: jeden-do-pobieranie zadań z interfejsem API i drugi dodawanie zadań do API.

Teraz, gdy możemy skonfigurowaną Nasz interfejs, możesz dodać rzeczywista implementacja:

* Tworzenie`GraphAPICaller.m file`

```objc
@implementation GraphAPICaller

// 
// Gets the tasks from our REST endpoint we specified in settings
//

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *))completionBlock

{
  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSMutableArray *Tasks = [[NSMutableArray alloc] init];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"GET"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:nil
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (!error) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          if ([dataReturned count] != 0) {

            for (NSMutableDictionary *theTask in dataReturned) {

              Task *t = [[Task alloc] init];
              t.name = [theTask valueForKey:@"Text"];

              [Tasks addObject:t];
            }
          }

          completionBlock(Tasks, nil);
        } else {
          completionBlock(nil, error);
        }

      }];
}

// 
// Adds a task from our REST endpoint we specified in settings
//

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock {

  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSDictionary *params = [self convertParamsToDictionary:task.name];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"POST"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:params
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (responseData) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          completionBlock(TRUE, nil);
        } else {
          completionBlock(FALSE, error);
        }

      }];
}

+ (NSDictionary *)convertParamsToDictionary:(NSString *)task {
  NSMutableDictionary *dictionary = [[NSMutableDictionary alloc] init];

  [dictionary setValue:task forKey:@"Text"];

  return dictionary;
}

@end
```

## <a name="run-the-sample-app"></a>Uruchom aplikację próbki

Na koniec tworzenie i uruchamianie aplikacji w Xcode. Utwórz konto lub zaloguj się do aplikacji, a tworzenie zadań dla zalogowany użytkownik. Wyloguj się i zaloguj się ponownie jako inny użytkownik i tworzenie zadań dla tego użytkownika.

Zadania są przechowywane dla każdego użytkownika w interfejsie API, ponieważ API wyodrębnia tożsamość użytkownika z token dostępu otrzyma powiadomienie.


## <a name="next-steps"></a>Następne kroki

Teraz można przenieść na tematy B2C bardziej zaawansowane. Możesz spróbować:

[Połączenie Node.js interfejsu API sieci web za pomocą aplikacji sieci web Node.js]()

[Dostosowywanie interfejsu użytkownika dla aplikacji B2C]()
