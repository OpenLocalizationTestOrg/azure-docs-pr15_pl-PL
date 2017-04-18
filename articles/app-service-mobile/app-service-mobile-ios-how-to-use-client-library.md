<properties
    pageTitle="Jak iOS Użyj SDK dla aplikacji Mobile Azure"
    description="Jak iOS Użyj SDK dla aplikacji Mobile Azure"
    services="app-service\mobile"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="how-to-use-ios-client-library-for-azure-mobile-apps"></a>Jak iOS Użyj Biblioteka klienta dla aplikacji Mobile Azure

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Ten przewodnik przedstawiono wykonywanie typowych scenariuszy przy użyciu najnowszych [aplikacji Mobile Azure iOS SDK][1]. Jeśli jesteś nowym użytkownikiem aplikacji Mobile Azure, pierwszego wykonania [Azure Mobile aplikacje Szybki Start] tworzenie wewnętrznej bazie danych, utworzyć tabelę, a pobieranie gotowych iOS Xcode projektu. W tym przewodniku możemy skupić się na iOS po stronie klienta SDK. Aby dowiedzieć się więcej na temat SDK po stronie serwera dla wewnętrznej bazy danych, zobacz HOWTOs SDK serwera.

## <a name="reference-documentation"></a>Dokumentacji

Dokumentacji dla klienta iOS SDK znajduje się tutaj: [aplikacje Mobile Azure iOS odwołanie klienta][2].

## <a name="supported-platforms"></a>Platformy obsługiwane

IOS SDK obsługuje celem-C projektów, projektami Swift 2.2 oraz projektów Swift 2.3 dla systemu iOS wersji 8.0 lub nowszy.

Uwierzytelnianie "serwer przepływu" za pomocą widoku sieci Web prezentowana interfejsu użytkownika.  Jeśli urządzenie nie jest w stanie do prezentowania interfejs użytkownika widoku sieci Web, a następnie należy użyć innej metody uwierzytelniania się poza zakresem produktu.  
Zestaw SDK nie jest więc odpowiedni typ czujki, lub podobnie ograniczeniami urządzeń.

##<a name="Setup"></a>Instalacja i wymagania wstępne

Ten przewodnik przyjęto założenie, że utworzono wewnętrznej bazie danych z tabeli. Ten przewodnik przyjęto założenie, że tabela zawiera ten sam schemat jako tabele w tych samouczkach. Ten przewodnik założono, że w kodzie odwołanie `MicrosoftAzureMobile.framework` i zaimportować `MicrosoftAzureMobile/MicrosoftAzureMobile.h`.

##<a name="create-client"></a>Jak: Tworzenie klienta

Aby uzyskać dostęp do wewnętrznej bazy danych w aplikacji Mobile Azure w projekcie, należy utworzyć `MSClient`. Zamienianie `AppUrl` z adresem URL aplikacji. Można pozostawić `gatewayURLString` i `applicationKey` puste. Jeśli skonfigurujesz bramy uwierzytelniania wypełnić `gatewayURLString` z adresem URL bramy.

**Cel C**:

```
MSClient *client = [MSClient clientWithApplicationURLString:@"AppUrl"];
```

**Swift**:

```
let client = MSClient(applicationURLString: "AppUrl")
```


##<a name="table-reference"></a>Jak: utworzyć odwołanie do tabeli

Do programu access lub aktualizowanie danych tworzenie odwołania do tabeli wewnętrznej bazy danych. Zamienianie `TodoItem` o nazwie tabeli

**Cel C**:

```
MSTable *table = [client tableWithName:@"TodoItem"];
```

**Swift**:

```
let table = client.tableWithName("TodoItem")
```


##<a name="querying"></a>Jak: kwerendy danych

Aby utworzyć kwerendę bazy danych, kwerendy `MSTable` obiektu. Poniższa kwerenda pobiera wszystkie elementy `TodoItem` i rejestruje tekst każdego elementu.

**Cel C**:

```
[table readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) { // error is nil if no error occured
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) { // items is NSArray of records that match query
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```
table.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

##<a name="filtering"></a>Jak: Filtr zwrócone dane

Aby filtrować wyniki, istnieje wiele dostępnych opcji.

Aby filtrować przy użyciu predykatu, użyj `NSPredicate` i `readWithPredicate`. Następujące filtry zwrócił dane, aby znaleźć tylko niepełne zadań do wykonania.

**Cel C**:

```
// Create a predicate that finds items where complete is false
NSPredicate * predicate = [NSPredicate predicateWithFormat:@"complete == NO"];
// Query the TodoItem table
[table readWithPredicate:predicate completion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```
// Create a predicate that finds items where complete is false
let predicate =  NSPredicate(format: "complete == NO")
// Query the TodoItem table
table.readWithPredicate(predicate) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

##<a name="query-object"></a>Jak: używanie MSQuery

Wykonywanie złożonych kwerend (włącznie z sortowaniem i stronicowania), utworzyć `MSQuery` obiektu, bezpośrednio lub przy użyciu predykatu:

**Cel C**:

```
MSQuery *query = [table query];
MSQuery *query = [table queryWithPredicate: [NSPredicate predicateWithFormat:@"complete == NO"]];
```

**Swift**:

```
let query = table.query()
let query = table.queryWithPredicate(NSPredicate(format: "complete == NO"))
```

`MSQuery`Pozwala kontrolować kilka zachowań kwerendy.

* Określanie kolejności wyników
* Ograniczanie pola, które zwraca
* Ograniczanie liczby rekordów zwraca
* Określ łączną liczbę elementów w odpowiedzi
* Parametry ciągu kwerendy niestandardowej w wezwaniu
* Stosowanie dodatkowe funkcje

Wykonywanie `MSQuery` kwerendy, dzwoniąc `readWithCompletion` obiektu.

## <a name="sorting"></a>Jak: sortowanie danych za pomocą MSQuery

Aby posortować wyniki, Spójrzmy na przykład. Aby posortować dane według pola "tekst" w kolejności rosnącej, a następnie według malejącej "wykonania", wywołania `MSQuery` tak jak:

**Cel C**:

```
[query orderByAscending:@"text"];
[query orderByDescending:@"complete"];
[query readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```
query.orderByAscending("text")
query.orderByDescending("complete")
query.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```


## <a name="selecting"></a><a name="parameters"></a>Jak: ograniczyć pola, a następnie rozwiń listę parametrów w ciągu kwerendy z MSQuery

Aby ograniczyć pola, które mają zostać zwrócone w kwerendzie, określ nazw pól we właściwości **selectFields** . W tym przykładzie zwraca tylko tekst i pola złożonym:

**Cel C**:

```
query.selectFields = @[@"text", @"complete"];
```

**Swift**:

```
query.selectFields = ["text", "complete"]
```

Aby uwzględniać parametry ciągu kwerendy dodatkowe w wezwaniu na serwerze (na przykład niestandardowego skryptu po stronie serwera z nich korzysta), wypełnij `query.parameters` tak jak:

**Cel C**:

```
query.parameters = @{
    @"myKey1" : @"value1",
    @"myKey2" : @"value2",
};
```

**Swift**:

```
query.parameters = ["myKey1": "value1", "myKey2": "value2"]
```

## <a name="paging"></a>Jak: Konfigurowanie rozmiar strony

Przy użyciu aplikacji Mobile Azure rozmiar strony określa liczbę rekordów, które są pobierane jednocześnie z tabel wewnętrznej bazy danych. Numer telefonu, aby `pull` zapasowych danych, na podstawie tej strony rozmiaru, dopóki nie ma żadnych więcej rekordów, aby uwzględniał następnie partii danych.

Istnieje możliwość skonfigurowania rozmiaru strony przy użyciu **MSPullSettings** , tak jak pokazano poniżej. Domyślny rozmiar strony jest 50, a w poniższym przykładzie zmieni go na 3.

Można skonfigurować inny rozmiar strony ze względu na wydajność. Jeśli masz dużej liczby rekordów danych małych duży rozmiar zmniejsza liczbę round-trips serwera. 

To ustawienie określa tylko rozmiar strony po stronie klienta. Jeśli klient żąda strony większe niż obsługuje aplikacje Mobile wewnętrznej bazy danych, rozmiar strony jest ograniczona do maksimum, które wewnętrznej bazy danych jest skonfigurowana do obsługi. 

To ustawienie jest również _liczbę_ rekordów nie _rozmiar w bajtach_.

Zwiększenie rozmiaru strony klienta, [należy również zwiększyć rozmiar strony na serwerze](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#_how-to-adjust-the-table-paging-size).

**Cel C**:

```
  MSPullSettings *pullSettings = [[MSPullSettings alloc] initWithPageSize:3];
  [table  pullWithQuery:query queryId:@nil settings:pullSettings
                        completion:^(NSError * _Nullable error) {
                               if(error) {
                    NSLog(@"ERROR %@", error);
                } 
                           }];
```


**Swift**:

```
let pullSettings = MSPullSettings(pageSize: 3)
table.pullWithQuery(query, queryId:nil, settings: pullSettings) { (error) in
    if let err = error {
        print("ERROR ", err)
    } 
}
```

##<a name="inserting"></a>Jak: wstawianie danych

Aby wstawić nowy wiersz tabeli, należy utworzyć `NSDictionary` i wywołania `table insert`. Po włączeniu [Dynamicznego schematu] przenośnych Azure aplikacji usługi wewnętrznej bazy danych umożliwia automatyczne generowanie nowych kolumn na podstawie `NSDictionary`.

Jeśli `id` jest nie są udostępnione, wewnętrznej bazy danych umożliwia automatyczne generowanie nowy unikatowy identyfikator. Podaj własne `id` korzystanie z poczty e-mail adresy, nazwy użytkowników, lub własny wartości jako identyfikatora. Dostarczanie własnego Identyfikatora może ułatwić sprzężeń i układy logiczne firmowe bazy danych.

`result` Zawiera nowy element, który został wstawiony. W zależności od logiki serwera może mieć dodatkowe lub zmienione dane w porównaniu z co przekazano do serwera.

**Cel C**:

```
NSDictionary *newItem = @{@"id": @"custom-id", @"text": @"my new item", @"complete" : @NO};
[table insert:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```
let newItem = ["id": "custom-id", "text": "my new item", "complete": false]
table.insert(newItem) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

##<a name="modifying"></a>Jak: zmodyfikować dane

Aby zaktualizować istniejących wierszy, modyfikowanie elementu i połączenia `update`:

**Cel C**:

```
NSMutableDictionary *newItem = [oldItem mutableCopy]; // oldItem is NSDictionary
[newItem setValue:@"Updated text" forKey:@"text"];
[table update:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```
if let newItem = oldItem.mutableCopy() as? NSMutableDictionary {
    newItem["text"] = "Updated text"
    table2.update(newItem as [NSObject: AnyObject], completion: { (result, error) -> Void in
        if let err = error {
            print("ERROR ", err)
        } else if let item = result {
            print("Todo Item: ", item["text"])
        }
    })
}
```

Możesz również podać identyfikator wiersza i zaktualizowane pole:

**Cel C**:

```
[table update:@{@"id":@"custom-id", @"text":"my EDITED item"} completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```
table.update(["id": "custom-id", "text": "my EDITED item"]) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

Co najmniej `id` należy ustawić atrybut, podczas tworzenia aktualizacji.

##<a name="deleting"></a>Jak: usuwanie danych

Aby usunąć element, wywołania `delete` z elementem:

**Cel C**:

```
[table delete:item completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**Swift**:

```
table.delete(newItem as [NSObject: AnyObject]) { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Możesz również usunąć, dostarczając identyfikator wiersza:

**Cel C**:

```
[table deleteWithId:@"37BBF396-11F0-4B39-85C8-B319C729AF6D" completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**Swift**:

```
table.deleteWithId("37BBF396-11F0-4B39-85C8-B319C729AF6D") { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Co najmniej `id` należy ustawić atrybut, podczas tworzenia usuwa.

##<a name="customapi"></a>Jak: połączenie niestandardowego interfejsu API

Przy użyciu niestandardowego interfejsu API pozwala udostępnić wszystkie funkcje wewnętrznej bazy danych. Nie zawiera on do zamapować na operacja tabeli. Nie tylko można uzyskać większą kontrolę nad wiadomości, można nawet odczytu i Ustaw nagłówki i zmienić format treści odpowiedzi. Aby dowiedzieć się, jak tworzyć niestandardowe interfejsu API do wewnętrznej bazy danych, przeczytaj [Niestandardowe interfejsy API](app-service-mobile-node-backend-how-to-use-server-sdk.md#work-easy-apis)

Aby zadzwonić do niestandardowego interfejsu API, połączenie `MSClient.invokeAPI`. Żądanie i odpowiedź zawartości są traktowane jako JSON. Aby użyć innych typów multimediów [za pomocą innych przeciążeń z `invokeAPI` ] [ 5].  Aby wprowadzić `GET` żądanie zamiast `POST` zażądać, ustaw parametr `HTTPMethod` do `"GET"` i parametrem `body` do `nil` (ponieważ żądania GET nie być treść wiadomości). Jeśli do niestandardowego interfejsu API obsługuje inne zlecenia HTTP, zmienić `HTTPMethod` prawidłowo.

**Cel C**:

```
[self.client invokeAPI:@"sendEmail"
                  body:@{ @"contents": @"Hello world!" }
            HTTPMethod:@"POST"
            parameters:@{ @"to": @"bill@contoso.com", @"subject" : @"Hi!" }
               headers:nil
            completion: ^(NSData *result, NSHTTPURLResponse *response, NSError *error) {
                if(error) {
                    NSLog(@"ERROR %@", error);
                } else {
                    // Do something with result
                }
            }];
```

**Swift**:

```
client.invokeAPI("sendEmail",
            body: [ "contents": "Hello World" ],
            HTTPMethod: "POST",
            parameters: [ "to": "bill@contoso.com", "subject" : "Hi!" ],
            headers: nil)
            {
                (result, response, error) -> Void in
                if let err = error {
                    print("ERROR ", err)
                } else if let res = result {
                          // Do something with result
                }
        }
```

##<a name="templates"></a>Jak: szablony wypychanych rejestru do wysyłania powiadomień i platform

Aby zarejestrować szablonów, należy przekazać szablonów z metodą **client.push registerDeviceToken** w aplikacji klienta.

**Cel C**:

```
[client.push registerDeviceToken:deviceToken template:iOSTemplate completion:^(NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    }
}];
```

**Swift**:

```
    client.push?.registerDeviceToken(NSData(), template: iOSTemplate, completion: { (error) in
        if let err = error {
            print("ERROR ", err)
        }
    })
```

Szablony są typu NSDictionary i mogą zawierać wiele szablonów w następującym formacie:

**Cel C**:

```
NSDictionary *iOSTemplate = @{ @"templateName": @{ @"body": @{ @"aps": @{ @"alert": @"$(message)" } } } };
```

**Swift**:

```
let iOSTemplate = ["templateName": ["body": ["aps": ["alert": "$(message)"]]]]
```

Wszystkie znaczniki są usuwane z żądania zabezpieczeń.  Aby dodać znaczniki do instalacji lub szablonów w ramach instalacji, zobacz [Praca z serwera wewnętrznej bazy danych programu .NET SDK dla aplikacji Mobile Azure][4].  Aby wysłać powiadomienia przy użyciu tych szablonów zarejestrowanych, Praca z [Interfejsów API koncentratory powiadomienie][3].

##<a name="errors"></a>Jak: obsługi błędów

Podczas połączenia usługa Azure aplikacji mobilnych wewnętrznej bazy danych blok zakończenia zawiera `NSError` parametru. Gdy wystąpi błąd, ten parametr jest wartością zero. W kodzie należy sprawdzić ten parametr i obsługi błędów, stosownie do potrzeb, jak pokazano w powyższej wstawki kodu.

Plik [`<WindowsAzureMobileServices/MSError.h>`] [6] definiuje stałe `MSErrorResponseKey`, `MSErrorRequestKey`, i `MSErrorServerItemKey`. Aby wyświetlić więcej danych związane z błędem:

**Cel C**:

```
NSDictionary *serverItem = [error.userInfo objectForKey:MSErrorServerItemKey];
```

**Swift**:

```
let serverItem = error.userInfo[MSErrorServerItemKey]
```

Ponadto plik definiuje stałe dla każdego kodu błędu:

**Cel C**:

```
if (error.code == MSErrorPreconditionFailed) {
```

**Swift**:

```
if (error.code == MSErrorPreconditionFailed) {
```

## <a name="adal"></a>Jak: uwierzytelniania użytkowników z Active Directory Authentication Library

Biblioteki uwierzytelniania Active Directory (ADAL) umożliwia użytkownikom zalogować się do aplikacji przy użyciu usługi Azure Active Directory. Uwierzytelnianie przepływu klientów za pośrednictwem dostawcy tożsamości SDK jest używane zamiast `loginWithProvider:completion:` metody.  Uwierzytelnianie klienta na podstawie przepływu zawiera więcej natywnych działanie interfejsu użytkownika i umożliwia dodatkowe dostosowanie.

1. Konfigurowanie sieci wewnętrznej bazy danych aplikacji dla urządzeń przenośnych dla logowania AAD, wykonując [sposobu konfigurowania aplikacji usługi logowania usługi Active Directory] [ 7] samouczka. Upewnij się ukończyć krok opcjonalny rejestrowania natywnej aplikacji. Dla systemu iOS, zalecamy przekierowania URI ma postać `<app-scheme>://<bundle-id>`. Aby uzyskać więcej informacji, zobacz [Szybki Start ADAL iOS][8].

2. Instalowanie przy użyciu Cocoapods ADAL. Edytowanie swojego Podfile, aby uwzględnić następujące definicje, zamieniając **Projektu jego** nazwy projektu Xcode:

        source 'https://github.com/CocoaPods/Specs.git'
        link_with ['YOUR-PROJECT']
        xcodeproj 'YOUR-PROJECT'

   i Pod:

        pod 'ADALiOS'

3. Korzystanie z funkcji Terminal, uruchom `pod install` z katalogu zawierającej projektu, a następnie otwórz wygenerowane obszaru roboczego Xcode (nie projekt).

4. Dodaj następujący kod do aplikacji, zgodnie z językiem, którego używasz. W każdej wprowadź te poprawnej:

    * Zamień **Wstawianie urząd tutaj** nazwę dzierżawy, w którym możesz obsługi administracyjnej aplikacji. Format powinien być https://login.windows.net/contoso.onmicrosoft.com. Ta wartość można skopiować na karcie domeny w swojej usługi Azure Active Directory w [Azure portal klasyczny].
    * Zamień **Identyfikator tutaj, wstawianie ZASOBU w-** identyfikator klienta do wewnętrznej bazy danych aplikacji dla urządzeń przenośnych. Identyfikator klienta można uzyskać na karcie **Zaawansowane** w obszarze **Ustawienia usługi Azure Active Directory** w portalu.
    * Zamień **Identyfikator tutaj, wstawianie klienta w-** identyfikator klienta, skopiowane z klientami aplikacji.
    * Zamień **Wstawianie PRZEKIERUJ-URI-tutaj** witryny _/.auth/login/done_ punktu końcowego, przy użyciu schematu HTTPS. Ta wartość powinna być podobna do _https://contoso.azurewebsites.net/.auth/login/done_.

**Cel C**:

    #import <ADALiOS/ADAuthenticationContext.h>
    #import <ADALiOS/ADAuthenticationSettings.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*))completionBlock;
    {
        NSString *authority = @"INSERT-AUTHORITY-HERE";
        NSString *resourceId = @"INSERT-RESOURCE-ID-HERE";
        NSString *clientId = @"INSERT-CLIENT-ID-HERE";
        NSURL *redirectUri = [[NSURL alloc]initWithString:@"INSERT-REDIRECT-URI-HERE"];
        ADAuthenticationError *error;
        ADAuthenticationContext *authContext = [ADAuthenticationContext authenticationContextWithAuthority:authority error:&error];
        authContext.parentController = parent;
        [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
        [authContext acquireTokenWithResource:resourceId
                                     clientId:clientId
                                  redirectUri:redirectUri
                              completionBlock:^(ADAuthenticationResult *result) {
                                  if (result.status != AD_SUCCEEDED)
                                  {
                                      completionBlock(nil, result.error);;
                                  }
                                  else
                                  {
                                      NSDictionary *payload = @{
                                                                @"access_token" : result.tokenCacheStoreItem.accessToken
                                                                };
                                      [client loginWithProvider:@"aad" token:payload completion:completionBlock];
                                  }
                              }];
    }


**Swift**:

    // add the following imports to your bridging header:
    //      #import <ADALiOS/ADAuthenticationContext.h>
    //      #import <ADALiOS/ADAuthenticationSettings.h>

    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let authority = "INSERT-AUTHORITY-HERE"
        let resourceId = "INSERT-RESOURCE-ID-HERE"
        let clientId = "INSERT-CLIENT-ID-HERE"
        let redirectUri = NSURL(string: "INSERT-REDIRECT-URI-HERE")
        var error: AutoreleasingUnsafeMutablePointer<ADAuthenticationError?> = nil
        let authContext = ADAuthenticationContext(authority: authority, error: error)
        authContext.parentController = parent
        ADAuthenticationSettings.sharedInstance().enableFullScreen = true
        authContext.acquireTokenWithResource(resourceId, clientId: clientId, redirectUri: redirectUri) { (result) in
                if result.status != AD_SUCCEEDED {
                    completion(nil, result.error)
                }
                else {
                    let payload: [String: String] = ["access_token": result.tokenCacheStoreItem.accessToken]
                    client.loginWithProvider("aad", token: payload, completion: completion)
                }
            }
    }

## <a name="facebook-sdk"></a>Jak: uwierzytelniania użytkowników z SDK Facebook dla systemu iOS

SDK Facebook dla systemu iOS umożliwia użytkownikom zalogować się do aplikacji przy użyciu Facebook.  Przy użyciu funkcji uwierzytelniania przepływu klienta są używane zamiast `loginWithProvider:completion:` metody.  Uwierzytelnianie klienta przepływu zawiera więcej natywnych działanie interfejsu użytkownika i umożliwia dodatkowe dostosowanie.

1. Konfigurowanie sieci wewnętrznej bazy danych aplikacji dla urządzeń przenośnych dla logowania w serwisie Facebook, wykonując [sposobu konfigurowania aplikacji usługi Facebook logowania] [ 9] samouczka.

2. Zainstaluj zestaw SDK Facebook dla systemu iOS, wykonując [SDK Facebook dla systemu iOS — wprowadzenie] [ 10] dokumentacji. Zamiast tworzyć aplikacji, możesz dodać do istniejącego rejestracji platformy systemu iOS. 

3. Dokumentacja w serwisie Facebook zawiera kodu celem-C w pełnomocnika aplikacji. Jeśli używasz **Swift**są dostępne następujące tłumaczenie dla AppDelegate.swift:
  
        // Add the following import to your bridging header:
        //      #import <FBSDKCoreKit/FBSDKCoreKit.h>
        
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            FBSDKApplicationDelegate.sharedInstance().application(application, didFinishLaunchingWithOptions: launchOptions)
            // Add any custom logic here.
            return true
        }

        func application(application: UIApplication, openURL url: NSURL, sourceApplication: String?, annotation: AnyObject?) -> Bool {
            let handled = FBSDKApplicationDelegate.sharedInstance().application(application, openURL: url, sourceApplication: sourceApplication, annotation: annotation)
            // Add any custom logic here.
            return handled
        }

4. Oprócz dodawania `FBSDKCoreKit.framework` do projektu, a także dodać odwołanie do `FBSDKLoginKit.framework` w taki sam sposób. 

4. Dodaj następujący kod do aplikacji, zgodnie z językiem, którego używasz. 

**Cel C**:

    #import <FBSDKLoginKit/FBSDKLoginKit.h>
    #import <FBSDKCoreKit/FBSDKAccessToken.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*)) completionBlock;
    {       
        FBSDKLoginManager *loginManager = [[FBSDKLoginManager alloc] init];
        [loginManager
         logInWithReadPermissions: @[@"public_profile"]
         fromViewController:parent
         handler:^(FBSDKLoginManagerLoginResult *result, NSError *error) {
             if (error) {
                 completionBlock(nil, error);
             } else if (result.isCancelled) {
                 completionBlock(nil, error);
             } else {
                 NSDictionary *payload = @{
                                           @"access_token":result.token.tokenString
                                           };
                 [client loginWithProvider:@"facebook" token:payload completion:completionBlock];
             }
         }];
    }

**Swift**:

    // Add the following imports to your bridging header:
    //      #import <FBSDKLoginKit/FBSDKLoginKit.h>
    //      #import <FBSDKCoreKit/FBSDKAccessToken.h>
    
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let loginManager = FBSDKLoginManager()
        loginManager.logInWithReadPermissions(["public_profile"], fromViewController: parent) { (result, error) in
            if (error != nil) {
                completion(nil, error)
            }
            else if result.isCancelled {
                completion(nil, error)
            }
            else {
                let payload: [String: String] = ["access_token": result.token.tokenString]
                client.loginWithProvider("facebook", token: payload, completion: completion)
            }
        }
    }

## <a name="twitter-fabric"></a>Jak: uwierzytelniania użytkowników z serwisu Twitter tkaninie dla systemu iOS

Tkaninie dla systemu iOS umożliwia użytkownikom zalogować się do aplikacji przy użyciu Twitter. Uwierzytelnianie klienta na podstawie przepływu jest używane zamiast `loginWithProvider:completion:` metody, ponieważ zawiera więcej natywnych działanie interfejsu użytkownika i umożliwia dodatkowe dostosowania.

1. Konfigurowanie sieci wewnętrznej bazy danych aplikacji dla urządzeń przenośnych dla logowania Twitter, wykonując samouczek [sposobu konfigurowania aplikacji usługi logowania Twitter](app-service-mobile-how-to-configure-twitter-authentication.md) .

2. Dodawanie tkaninie do projektu, zgodnie z dokumentacją [tkaninie dla systemu iOS — wprowadzenie] i konfigurując TwitterKit.

    > [AZURE.NOTE] Domyślnie tkaninie utworzy aplikacji Twitter. Można uniknąć tworzenia aplikacji przez proces rejestracji klucz i tajny dla klientów indywidualnych, utworzonej wcześniej za pomocą następujących wstawki kodu.  Możesz także zastąpić wartości klucz i tajny dla klientów indywidualnych, które umożliwiają aplikacji usługi z wartościami, które są widoczne w [Tkaninie pulpitu nawigacyjnego]. Jeśli wybierzesz tę opcję, pamiętaj ustawić adres URL zwrotnego na wartość symbol zastępczy, takich jak `https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`.

    Jeśli zdecydujesz się na użycie hasła, utworzony wcześniej, Dodaj następujący kod do pełnomocnika aplikacji:
    
    **Cel C**:

        #import <Fabric/Fabric.h>
        #import <TwitterKit/TwitterKit.h>
        // ...
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [[Twitter sharedInstance] startWithConsumerKey:@"your_key" consumerSecret:@"your_secret"];
            [Fabric with:@[[Twitter class]]];
            // Add any custom logic here.
            return YES;
        }
        
    **Swift**:
    
        import Fabric
        import TwitterKit
        // ...
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            Twitter.sharedInstance().startWithConsumerKey("your_key", consumerSecret: "your_secret")
            Fabric.with([Twitter.self])
            // Add any custom logic here.
            return true
        }
    
3. Dodaj następujący kod do aplikacji, zgodnie z językiem, którego używasz. 

**Cel C**:

    #import <TwitterKit/TwitterKit.h>
    // ...
    - (void)authenticate:(UIViewController*)parent completion:(void (^) (MSUser*, NSError*))completionBlock
    {
        [[Twitter sharedInstance] logInWithCompletion:^(TWTRSession *session, NSError *error) {
            if (session) {
                NSDictionary *payload = @{
                                            @"access_token":session.authToken,
                                            @"access_token_secret":session.authTokenSecret
                                        };
                [client loginWithProvider:@"twitter" token:payload completion:completionBlock];
            } else {
                completionBlock(nil, error);
            }
        }];
    }

**Swift**:

    import TwitterKit
    // ...
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let client = self.table!.client
        Twitter.sharedInstance().logInWithCompletion { session, error in
            if (session != nil) {
                let payload: [String: String] = ["access_token": session!.authToken, "access_token_secret": session!.authTokenSecret]
                client.loginWithProvider("twitter", token: payload, completion: completion)
            } else {
                completion(nil, error)
            }
        }
    }

## <a name="google-sdk"></a>Jak: uwierzytelniania użytkowników z usługi Google logowania SDK dla systemu iOS

Google logowania SDK dla systemu iOS umożliwia użytkownikom zalogować się do aplikacji przy użyciu konta Google.  Google ostatnio ogłoszone zmiany ich zasad zabezpieczeń OAuth.  Zmiany zasad wymaga stosowania Google SDK w przyszłości.

1. Konfigurowanie sieci wewnętrznej bazy danych aplikacji dla urządzeń przenośnych potrzeby Google logowania, wykonując samouczek [sposobu konfigurowania aplikacji usługi Google logowania](app-service-mobile-how-to-configure-google-authentication.md) .

2. Zainstaluj zestaw SDK Google dla systemu iOS, wykonując dokumentacji [Google logowania dla systemu iOS - rozpoczęcie integracji](https://developers.google.com/identity/sign-in/ios/start-integrating) . Możesz pominąć sekcji "Uwierzytelniania z serwera wewnętrznej bazy danych".

3. Dodaj następujący tekst na pełnomocnika `signIn:didSignInForUser:withError:` metody, zgodnie z językiem używasz.

**Cel C**:

        NSDictionary *payload = @{
                                  @"id_token":user.authentication.idToken,
                                  @"authorization_code":user.serverAuthCode
                                  };
        
        [client loginWithProvider:@"google" token:payload completion:^(MSUser *user, NSError *error) {
            // ...
        }];

**Swift**:

        let payload: [String: String] = ["id_token": user.authentication.idToken, "authorization_code": user.serverAuthCode]
        client.loginWithProvider("google", token: payload) { (user, error) in
            // ...
        }

4. Upewnij się, można też dodać poniższe czynności, aby `application:didFinishLaunchingWithOptions:` w na pełnomocnika aplikacji, zamieniając "SERVER_CLIENT_ID" z tym samym Identyfikatorem, który umożliwia konfigurowanie aplikacji usługi w kroku 1.

**Cel C**:

        [GIDSignIn sharedInstance].serverClientID = @"SERVER_CLIENT_ID";
 
 **Swift**:
 
        GIDSignIn.sharedInstance().serverClientID = "SERVER_CLIENT_ID"

 
 5. Dodaj następujący kod do aplikacji w UIViewController, który wykonuje `GIDSignInUIDelegate` protokołu, zgodnie z językiem używasz.  Wylogowano się przed rejestrowaniu ponownie, a mimo że nie musisz ponownie wprowadzić poświadczenia, zobacz okno dialogowe zgody.  Tylko wywołać tej metody, gdy wygaśnie tokenu sesji.
 
 **Cel C**:

        #import <Google/SignIn.h>
        // ...
        - (void)authenticate
        {
                [GIDSignIn sharedInstance].uiDelegate = self;
                [[GIDSignIn sharedInstance] signOut];
                [[GIDSignIn sharedInstance] signIn];
        }
 
 **Swift**:
    
        // ...
        func authenticate() {
            GIDSignIn.sharedInstance().uiDelegate = self
            GIDSignIn.sharedInstance().signOut()
            GIDSignIn.sharedInstance().signIn()
        }
        
<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[Setup and Prerequisites]: #Setup
[How to: Create the Mobile Services client]: #create-client
[How to: Create a table reference]: #table-reference
[How to: Query data from a mobile service]: #querying
[Filter returned data]: #filtering
[Sort returned data]: #sorting
[Return data in pages]: #paging
[Select specific columns]: #selecting
[How to: Bind data to the user interface]: #binding
[How to: Insert data into a mobile service]: #inserting
[How to: Modify data in a mobile service]: #modifying
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching-tokens
[How to: Upload images and large files]: #blobs
[How to: Handle errors]: #errors
[How to: Design unit tests]: #unit-testing
[How to: Customize the client]: #customizing
[Customize request headers]: #custom-headers
[Customize data type serialization]: #custom-serialization
[Next Steps]: #next-steps
[How to: Use MSQuery]: #query-object

<!-- Images. -->

<!-- URLs. -->
[Azure urządzeń przenośnych aplikacji Szybki Start]: app-service-mobile-ios-get-started.md

[Add Mobile Services to Existing App]: /develop/mobile/tutorials/get-started-data
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Validate and modify data in Mobile Services by using server scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-ios
[Mobile Services SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[iOS SDK]: https://developer.apple.com/xcode

[Handling Expired Tokens]: http://go.microsoft.com/fwlink/p/?LinkId=301955
[Live Connect SDK]: http://go.microsoft.com/fwlink/p/?LinkId=301960
[Permissions]: http://msdn.microsoft.com/library/windowsazure/jj193161.aspx
[Service-side Authorization]: mobile-services-javascript-backend-service-side-authorization.md
[Use scripts to authorize users]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[Dynamiczne schematu]: http://go.microsoft.com/fwlink/p/?LinkId=296271
[How to: access custom parameters]: /develop/mobile/how-to-guides/work-with-server-scripts#access-headers
[Create a table]: http://msdn.microsoft.com/library/windowsazure/jj193162.aspx
[NSDictionary object]: http://go.microsoft.com/fwlink/p/?LinkId=301965
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[CLI to manage Mobile Services tables]: ../virtual-machines-command-line-tools.md#Mobile_Tables
[Conflict-Handler]: mobile-services-ios-handling-conflicts-offline-data.md#add-conflict-handling

[Pulpit nawigacyjny tkaninie]: https://www.fabric.io/home
[Tkaninie dla systemu iOS — wprowadzenie]: https://docs.fabric.io/ios/fabric/getting-started.html
[1]: https://github.com/Azure/azure-mobile-apps-ios-client/blob/master/README.md#ios-client-sdk
[2]: http://azure.github.io/azure-mobile-apps-ios-client/
[3]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[4]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags
[5]: http://azure.github.io/azure-mobile-services/iOS/v3/Classes/MSClient.html#//api/name/invokeAPI:data:HTTPMethod:parameters:headers:completion:
[6]: https://github.com/Azure/azure-mobile-services/blob/master/sdk/iOS/src/MSError.h
[7]: app-service-mobile-how-to-configure-active-directory-authentication.md
[8]: ../active-directory/active-directory-devquickstarts-ios.md#em1-determine-what-your-redirect-uri-will-be-for-iosem
[9]: app-service-mobile-how-to-configure-facebook-authentication.md
[10]: https://developers.facebook.com/docs/ios/getting-started
