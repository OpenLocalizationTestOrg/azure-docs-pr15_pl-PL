<properties
    pageTitle="Wysyłanie powiadomień wypychanych w systemie iOS z koncentratorów powiadomienie Azure | Microsoft Azure"
    description="W tym samouczku dowiesz się, jak za pomocą koncentratorów powiadomienie Azure wysyłania powiadomień wypychanych aplikacji systemu iOS."
    services="notification-hubs"
    documentationCenter="ios"
    keywords="powiadomienia push, powiadomienia push ios powiadomienia wypychane"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="hero-article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-to-ios-with-azure-notification-hubs"></a>Wysyłanie powiadomienia wypychane w systemie iOS z koncentratorów powiadomienie Azure

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Omówienie

> [AZURE.NOTE] Aby użyć tego samouczka, musi mieć konto Azure aktywne. Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-ios-get-started).

Ten samouczek pokazano, jak używać koncentratory powiadomienie Azure do wysyłania powiadomień wypychanych aplikacji systemu iOS. Utworzysz aplikację pusty iOS, która, otrzymuje powiadomienia wypychane za pomocą [Usługi powiadomień Push firmy Apple (APN)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html). 

Po zakończeniu, będzie mógł korzystać z Twoim Centrum powiadomienie do emisji powiadomienia wypychane do wszystkich urządzeń uruchamianie aplikacji.

## <a name="before-you-begin"></a>Przed rozpoczęciem

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Ukończony kodu dla tego samouczka można znaleźć [w GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/iOS/GetStartedNH/GetStarted). 

##<a name="prerequisites"></a>Wymagania wstępne

Ten samouczek ma następujące wymagania:

+ [IOS usług Mobile zestawu SDK w wersji 1.2.4]
+ Najnowsza wersja pakietu [Xcode]
+ IOS 8 (lub nowszy) — do urządzenia
+ Członkostwo w [Programie Deweloper firmy Apple](https://developer.apple.com/programs/) .

   > [AZURE.NOTE] Ze względu na wymagania dotyczące konfiguracji powiadomienia wypychane należy wdrożyć i przetestować powiadomienia wypychane na urządzeniu fizycznie iOS (iPhone lub iPad) zamiast iOS Simulator.

Ten samouczek jest wymagane w przypadku innych samouczki koncentratory powiadomienie dotyczące aplikacji systemu iOS.

[AZURE.INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

##<a name="configure-your-notification-hub-for-ios-push-notifications"></a>Konfigurowanie usługi powiadomień Centrum dla systemu iOS powiadomienia wypychane

W tej sekcji opisano podczas tworzenia nowego koncentratora powiadomienie i konfigurowanie uwierzytelniania za pomocą APN przy użyciu certyfikatu wypychanych **.p12** utworzone przez Ciebie. Jeśli chcesz używać koncentratora powiadomienie, które zostały już utworzone, można przejdź do kroku 5.

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li>
<p>Kliknij przycisk <b>Usługi powiadomień</b> w karta <b>Ustawienia</b> , a następnie wybierz pozycję <b>Apple (APN)</b>. Kliknij na <b>Przekazywanie certyfikatu</b> i wybierz wcześniej wyeksportowany plik <b>.p12</b> . Upewnij się, że można także określić poprawne hasło.</p>
<p>Upewnij się wybrać tryb <b>piaskownicy</b> , ponieważ jest to rozwoju. <b>Produkcji</b> należy używać tylko, jeśli chcesz wysłać powiadomienia wypychane dla użytkowników, którzy zakupili aplikacji ze sklepu.</p>
</li>
</ol>
&emsp;&emsp;![Konfigurowanie APN w Azure Portal](./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png)

&emsp;&emsp;![Konfigurowanie certyfikacji APN w Azure Portal](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png)



Twoim Centrum powiadomienie jest skonfigurowany do pracy z APN, i masz ciągów połączeń do rejestrowania aplikacji i wysyłanie powiadomienia wypychane.

##<a name="connect-your-ios-app-to-notification-hubs"></a>Łączenie aplikacji w systemie iOS do koncentratorów powiadomień

1. W Xcode Utwórz nowy projekt iOS i wybierz szablon **Jednej aplikacji widoku** .

    ![Xcode - jednego widoku aplikacji][8]

2. Po ustawieniu żądanych opcji nowego projektu, upewnij się użyć tej samej **Nazwie produktu** i **Identyfikator organizacji** , który został użyty podczas ustawione wcześniej identyfikator pakietu w portalu firmy Apple Deweloper.

    ![Xcode — opcje programu project][11]

3. W obszarze **elementów docelowych**kliknij nazwę projektu, kliknij kartę **Ustawienia tworzenia** i rozwiń **Tożsamości podpisywanie kodu**, a następnie w obszarze **Debugowanie**, ustaw swoją tożsamość podpisywanie kodu. Przełączanie **poziomy** od **najprostszych** do **wszystkich**i ustaw **Inicjowania obsługi administracyjnej profilu** do obsługi administracyjnej profil, który został utworzony wcześniej.

    Jeśli nie widzisz nowego profilu obsługi administracyjnej utworzony w Xcode, spróbuj odświeżyć profile dla Twojej tożsamości podpisywania. Na pasku menu kliknij pozycję **Xcode** , kliknij polecenie **Preferencje**, kliknij kartę **konto** , kliknij przycisk **Wyświetl szczegóły** , kliknij przycisk podpisywania tożsamości, a następnie kliknij przycisk Odśwież w prawym dolnym rogu.

    ![Xcode - obsługi administracyjnej profilu][9]

4. Pobierz [iOS usług Mobile zestawu SDK w wersji 1.2.4] i rozpakuj plik. W Xcode kliknij prawym przyciskiem myszy projektu, a następnie kliknij opcję **Dodaj pliki, aby** dodać **WindowsAzureMessaging.framework** folder do projektu Xcode. Zaznacz **skopiować elementy w razie potrzeby**, a następnie kliknij przycisk **Dodaj**.

    >[AZURE.NOTE] Koncentratory powiadomienie SDK nie obsługuje obecnie bitcode na Xcode 7.  **Włączanie Bitcode** musi być ustawiona na **nie** w obszarze **Opcje tworzenia** projektu.

    ![Rozpakuj plik Azure SDK][10]

5. Dodawanie nowego pliku nagłówka do projektu o nazwie `HubInfo.h`. Ten plik zostanie przytrzymaj stałych do koncentratora powiadomienie.  Dodaj następujące definicje i zamienianie symboli zastępczych literał ciągu na swoją *nazwę Centrum* i *DefaultListenSharedAccessSignature* wspomniano wcześniej.

        #ifndef HubInfo_h
        #define HubInfo_h
        
            #define HUBNAME @"<Enter the name of your hub>"
            #define HUBLISTENACCESS @"<Enter your DefaultListenSharedAccess connection string"
        
        #endif /* HubInfo_h */

6. Otwieranie swojej `AppDelegate.h` pliku Dodaj następujące dyrektywy importu:

         #import <WindowsAzureMessaging/WindowsAzureMessaging.h> 
         #import "HubInfo.h"
        
7. W swojej `AppDelegate.m file`, Dodaj poniższy kod w `didFinishLaunchingWithOptions` metoda oparta na Twojej wersji systemu iOS. Kod rejestruje uchwyt urządzenia APN:

    Dla systemu iOS 8:

        UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                UIUserNotificationTypeAlert | UIUserNotificationTypeBadge categories:nil];

        [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];

    IOS wersje przed 8:

         [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];


8. W tym samym pliku Dodaj następujące metody. Kod łączy do koncentratora powiadomień przy użyciu określonej w HubInfo.h informacje o połączeniu. Następnie oferuje token urządzenia do koncentratora powiadomienie tak, aby Centrum powiadomienie można wysłać powiadomień:

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *) deviceToken {
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:HUBLISTENACCESS
                                        notificationHubPath:HUBNAME];

            [hub registerNativeWithDeviceToken:deviceToken tags:nil completion:^(NSError* error) {
                if (error != nil) {
                    NSLog(@"Error registering for notifications: %@", error);
                }
                else {
                    [self MessageBox:@"Registration Status" message:@"Registered"];
                }
            }];
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }


9. W tym samym pliku dodać poniższej metody, aby wyświetlić **UIAlert** , jeśli otrzymania powiadomienia, gdy aplikacja jest aktywna:


        - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

10. Tworzenie i uruchamianie aplikacji na urządzeniu, aby zweryfikować, że nie błędy.

## <a name="send-test-push-notifications"></a>Wysyłanie powiadomienia wypychane testowych


Istnieje możliwość przetestowania otrzymywania powiadomień w aplikacji przez wysłanie powiadomień wypychanych w [Azure Portal] za pomocą sekcję **Rozwiązywanie problemów** w Centrum karta (Użyj opcji *Testowanie Wyślij* ).

![Portal Azure — Wyślij Test][30]

[AZURE.INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]


## <a name="optional-send-push-notifications-from-the-app"></a>(Opcjonalnie) Wysyłanie powiadomienia wypychane z aplikacji

>[AZURE.IMPORTANT] W tym przykładzie wysyłania powiadomień z aplikacji klienckiej zapewnia nauki wyłącznie. Ponieważ wymaga to `DefaultFullSharedAccessSignature` do uczestniczenia w aplikacji klienta, prezentuje ona Twoim Centrum powiadomień na ryzyko, że użytkownik mogą uzyskać dostęp do wysyłania powiadomień nieautoryzowanego dla klientów.

Jeśli chcesz wysłać powiadomienia wypychane z poziomu aplikacji, ta sekcja zawiera przykład jak to zrobić przy użyciu interfejsu użytkownika pozostałych.

1. Otwórz w Xcode, `Main.storyboard` i dodaj następujące składniki interfejsu użytkownika z biblioteki obiektów, aby umożliwić użytkownikowi do wysyłania powiadomień wypychanych w aplikacji:

    - Etykieta bez tekstu etykiety. Umożliwią raportowanie błędów podczas wysyłania powiadomień. Właściwość **wierszy** powinna być równa **0** , aby ją zostanie automatycznie rozmiar ograniczony po prawej stronie i marginesy po lewej stronie u góry widoku.
    - Pola tekstowego z tekstem **zastępczym** ustaw **Wprowadź wiadomości z powiadomieniem**. Ograniczyć pole tuż poniżej etykiety, tak jak pokazano poniżej. Ustaw widok kontroler jako pełnomocnik wylotu.
    - Przycisk tytuł **Wyślij powiadomienie** ograniczone tuż poniżej pola tekstowego i środka w poziomie.

    Widok powinien wyglądać następująco:

    ![Projektant Xcode][32]


2. [Dodaj punkty](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html) w polu Etykieta i tekst połączony widok i zaktualizować usługi `interface` definicji do obsługi `UITextFieldDelegate` i `NSXMLParserDelegate`. Dodawanie deklaracji trzy właściwości pokazanego poniżej do wsparcia nawiązywania połączeń z interfejsu API usługi REST i analizowanie odpowiedź.

    Plik ViewController.h powinna wyglądać następująco:

        #import <UIKit/UIKit.h>

        @interface ViewController : UIViewController <UITextFieldDelegate, NSXMLParserDelegate>
        {
            NSXMLParser *xmlParser;
        }

        // Make sure these outlets are connected to your UI by ctrl+dragging
        @property (weak, nonatomic) IBOutlet UITextField *notificationMessage;
        @property (weak, nonatomic) IBOutlet UILabel *sendResults;

        @property (copy, nonatomic) NSString *statusResult;
        @property (copy, nonatomic) NSString *currentElement;

        @end

3. Otwórz `HubInfo.h` i dodaj następujące stałe, które będą używane do wysyłania powiadomień na Twoim Centrum. Zamień literał ciągu symbol zastępczy rzeczywisty ciąg połączenia *DefaultFullSharedAccessSignature* .

        #define API_VERSION @"?api-version=2015-01"
        #define HUBFULLACCESS @"<Enter Your DefaultFullSharedAccess Connection string>"

4. Dodaj następujący `#import` instrukcji, aby usługi `ViewController.h` pliku.

        #import <CommonCrypto/CommonHMAC.h>
        #import "HubInfo.h"

5. W `ViewController.m` Dodaj następujący kod do implementacji interfejsu. Kod analizuje *DefaultFullSharedAccessSignature* ciąg połączenia. Jak wskazano w [interfejsie API usługi REST odwołania](http://msdn.microsoft.com/library/azure/dn495627.aspx), zanalizowana informacje będzie używana do wygenerowania tokenu skojarzeń zabezpieczeń, w nagłówku żądania **autoryzacji** .

        NSString *HubEndpoint;
        NSString *HubSasKeyName;
        NSString *HubSasKeyValue;

        -(void)ParseConnectionString
        {
            NSArray *parts = [HUBFULLACCESS componentsSeparatedByString:@";"];
            NSString *part;

            if ([parts count] != 3)
            {
                NSException* parseException = [NSException exceptionWithName:@"ConnectionStringParseException"
                    reason:@"Invalid full shared access connection string" userInfo:nil];

                @throw parseException;
            }

            for (part in parts)
            {
                if ([part hasPrefix:@"Endpoint"])
                {
                    HubEndpoint = [NSString stringWithFormat:@"https%@",[part substringFromIndex:11]];
                }
                else if ([part hasPrefix:@"SharedAccessKeyName"])
                {
                    HubSasKeyName = [part substringFromIndex:20];
                }
                else if ([part hasPrefix:@"SharedAccessKey"])
                {
                    HubSasKeyValue = [part substringFromIndex:16];
                }
            }
        }

6. W `ViewController.m`, zaktualizuj `viewDidLoad` metodą na analizowanie parametry połączenia, podczas ładowania widoku. Również dodać metod narzędzie, jak pokazano poniżej, aby implementacji interfejsu.  


        - (void)viewDidLoad
        {
            [super viewDidLoad];
            [self ParseConnectionString];
            [_notificationMessage setDelegate:self];
        }

        -(NSString *)CF_URLEncodedString:(NSString *)inputString
        {
           return (__bridge NSString *)CFURLCreateStringByAddingPercentEscapes(NULL, (CFStringRef)inputString,
                NULL, (CFStringRef)@"!*'();:@&=+$,/?%#[]", kCFStringEncodingUTF8);
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }





7. W `ViewController.m`, Dodaj następujący kod do implementacji interfejsu do wygenerowania tokenu autoryzacji skojarzeń zabezpieczeń otrzymany w nagłówku **autoryzacji** , wymienione w [Pozostałych API Reference](http://msdn.microsoft.com/library/azure/dn495627.aspx).

        -(NSString*) generateSasToken:(NSString*)uri
        {
            NSString *targetUri;
            NSString* utf8LowercasedUri = NULL;
            NSString *signature = NULL;
            NSString *token = NULL;

            @try
            {
                // Add expiration
                uri = [uri lowercaseString];
                utf8LowercasedUri = [self CF_URLEncodedString:uri];
                targetUri = [utf8LowercasedUri lowercaseString];
                NSTimeInterval expiresOnDate = [[NSDate date] timeIntervalSince1970];
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60;
                UInt64 expires = trunc(expiresOnDate);
                NSString* toSign = [NSString stringWithFormat:@"%@\n%qu", targetUri, expires];

                // Get an hmac_sha1 Mac instance and initialize with the signing key
                const char *cKey  = [HubSasKeyValue cStringUsingEncoding:NSUTF8StringEncoding];
                const char *cData = [toSign cStringUsingEncoding:NSUTF8StringEncoding];
                unsigned char cHMAC[CC_SHA256_DIGEST_LENGTH];
                CCHmac(kCCHmacAlgSHA256, cKey, strlen(cKey), cData, strlen(cData), cHMAC);
                NSData *rawHmac = [[NSData alloc] initWithBytes:cHMAC length:sizeof(cHMAC)];
                signature = [self CF_URLEncodedString:[rawHmac base64EncodedStringWithOptions:0]];

                // Construct authorization token string
                token = [NSString stringWithFormat:@"SharedAccessSignature sig=%@&se=%qu&skn=%@&sr=%@",
                    signature, expires, HubSasKeyName, targetUri];
            }
            @catch (NSException *exception)
            {
                [self MessageBox:@"Exception Generating SaS Token" message:[exception reason]];
            }
            @finally
            {
                if (utf8LowercasedUri != NULL)
                    CFRelease((CFStringRef)utf8LowercasedUri);
                if (signature != NULL)
                CFRelease((CFStringRef)signature);
            }

            return token;
        }


8. CTRL + przeciągnij z **Wyślij powiadomienie** przycisk, aby `ViewController.m` w celu dodania operacji o nazwie **SendNotificationMessage** dla zdarzenia **Dotyku w dół** . Zaktualizuj metody poniższy kod do wysyłania powiadomień przy użyciu interfejsu API usługi REST.

        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";
            [self SendNotificationRESTAPI];
        }

        - (void)SendNotificationRESTAPI
        {
            NSURLSession* session = [NSURLSession
                             sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                             delegate:nil delegateQueue:nil];

            // Apple Notification format of the notification message
            NSString *json = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"%@\"}}",
                                self.notificationMessage.text];

            // Construct the message's REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                                HUBNAME, API_VERSION]];

            // Generate the token to be used in the authorization header
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create the request to add the APNs notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

            // Signify Apple notification format
            [request setValue:@"apple" forHTTPHeaderField:@"ServiceBusNotification-Format"];

            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

            // Send the REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (error || (httpResponse.statusCode != 200 && httpResponse.statusCode != 201))
                {
                    NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                }
                if (data != NULL)
                {
                    xmlParser = [[NSXMLParser alloc] initWithData:data];
                    [xmlParser setDelegate:self];
                    [xmlParser parse];
                }
            }];
            [dataTask resume];
        }


9. W `ViewController.m`, Dodaj następujące metody pełnomocnika do obsługi zamykanie klawiatury dla pola tekstowego. CTRL + przeciągnij z pola tekstowego na ikonę widok kontroler w Projektancie interfejs, aby ustawić widok kontroler jako pełnomocnik wylotu.

        //===[ Implement UITextFieldDelegate methods ]===

        -(BOOL)textFieldShouldReturn:(UITextField *)textField
        {
            [textField resignFirstResponder];
            return YES;
        }


10. W `ViewController.m`, Dodaj następujące metody pełnomocnika do obsługi analizy odpowiedzi przy użyciu `NSXMLParser`.

        //===[ Implement NSXMLParserDelegate methods ]===

        -(void)parserDidStartDocument:(NSXMLParser *)parser
        {
            self.statusResult = @"";
        }

        -(void)parser:(NSXMLParser *)parser didStartElement:(NSString *)elementName
            namespaceURI:(NSString *)namespaceURI qualifiedName:(NSString *)qName
            attributes:(NSDictionary *)attributeDict
        {
            NSString * element = [elementName lowercaseString];
            NSLog(@"*** New element parsed : %@ ***",element);

            if ([element isEqualToString:@"code"] | [element isEqualToString:@"detail"])
            {
                self.currentElement = element;
            }
        }

        -(void) parser:(NSXMLParser *)parser foundCharacters:(NSString *)parsedString
        {
            self.statusResult = [self.statusResult stringByAppendingString:
                [NSString stringWithFormat:@"%@ : %@\n", self.currentElement, parsedString]];
        }

        -(void)parserDidEndDocument:(NSXMLParser *)parser
        {
            // Set the status label text on the UI thread
            dispatch_async(dispatch_get_main_queue(),
            ^{
                [self.sendResults setText:self.statusResult];
            });
        }



11. Tworzenie projektu i sprawdź, czy są żadne błędy.


> [AZURE.NOTE] Jeśli wystąpi błąd kompilacji w Xcode7 o obsługiwanych bitcode, należy zmienić **Ustawienia tworzenia** > **Włączyć Bitcode (ENABLE_BITCODE)** na wartość **Brak** w Xcode. SDK koncentratory powiadomienie nie obsługuje obecnie bitcode. 

Ładunki możliwe powiadomień można znaleźć w Apple [Push Podręcznik programowania powiadomienie i lokalny].


##<a name="checking-if-your-app-can-receive-push-notifications"></a>Sprawdzanie, jeśli aplikacji mogą otrzymywać powiadomienia wypychane

Aby przetestować powiadomień wypychanych w systemie iOS, należy wdrożyć aplikację do urządzenia fizycznie iOS. Nie możesz wysyłać powiadomienia push firmy Apple za pomocą iOS Simulator.

1. Uruchamianie aplikacji i sprawdź, czy rejestracji zakończyło się powodzeniem, a następnie naciśnij **przycisk OK**.

    ![iOS badań rejestracji powiadomienia Push aplikacji][33]

2. Możesz wysłać powiadomienia wypychane test z [Azure Portal]zgodnie z powyższym opisem. Po dodaniu kodu do wysyłania powiadomień wypychanych w aplikacji dotknij wewnątrz pola tekstowego, aby wprowadzić wiadomości z powiadomieniem. Naciśnij przycisk **Wyślij** na klawiaturze lub przycisku **Wyślij powiadomienie** w widoku, aby wysłać wiadomość z powiadomieniem.

    ![iOS aplikacji wypychanych powiadomień Wyślij Test][34]

3. Powiadomienia wypychane są wysyłane do wszystkich urządzeń, które są rejestrowane, aby otrzymywać powiadomienia o jego z określonego Centrum powiadomienie.

    ![iOS Test do odbierania powiadomień Push aplikacji][35]


##<a name="next-steps"></a>Następne kroki

W tym przykładzie prosty powiadomień wypychanych jest emitowana do wszystkich urządzeń zarejestrowanych iOS. Sugerujemy jako następnego kroku, w które przejdź do samouczka [Azure powiadomienie koncentratory Powiadom użytkowników dla systemu iOS z wewnętrznej bazy danych programu .NET] przeprowadzi Cię przez proces tworzenia wewnętrznej bazie danych do wysyłania powiadomień wypychanych przy użyciu tagów stopień opanowania materiału. 

Jeśli chcesz segmentu użytkowników według grup odsetek Ponadto na można przenosić do samouczka [Użyj koncentratory powiadomień do wysłania najnowszych wiadomości] . 

Aby uzyskać ogólne informacje na temat koncentratory powiadomień zobacz [Wskazówki na temat koncentratory powiadomienie].



<!-- Images. -->

[6]: ./media/notification-hubs-ios-get-started/notification-hubs-configure-ios.png
[8]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app.png
[9]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app2.png
[10]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app3.png
[11]: ./media/notification-hubs-ios-get-started/notification-hubs-xcode-product-name.png

[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png

[31]: ./media/notification-hubs-ios-get-started/notification-hubs-ios-ui.png
[32]: ./media/notification-hubs-ios-get-started/notification-hubs-storyboard-view.png
[33]: ./media/notification-hubs-ios-get-started/notification-hubs-test1.png
[34]: ./media/notification-hubs-ios-get-started/notification-hubs-test2.png
[35]: ./media/notification-hubs-ios-get-started/notification-hubs-test3.png



<!-- URLs. -->
[IOS usług Mobile zestawu SDK w wersji 1.2.4]: http://aka.ms/kymw2g
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Azure Classic Portal]: https://manage.windowsazure.com/
[Powiadomienie o koncentratory wskazówki]: http://msdn.microsoft.com/library/jj927170.aspx
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-ios-get-started-push.md
[Azure powiadomienie koncentratory Powiadom użytkowników dla systemu iOS z wewnętrznej bazy danych programu .NET]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Wysyłanie najnowszych wiadomości za pomocą koncentratorów powiadomień]: notification-hubs-ios-xplat-segmented-apns-push-notification.md

[Lokalne i przewodnik programowania powiadomienia Push]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Azure Portal]: https://portal.azure.com