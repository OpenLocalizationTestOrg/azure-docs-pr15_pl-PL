<properties 
    pageTitle="IOS mostka widok sieci Web przy użyciu natywnego iOS zaangażowania Mobile SDK" 
    description="W tym artykule opisano sposób tworzenia mostka między systemem Javascript i natywnych iOS zaangażowania Mobile SDK widok sieci Web"      
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-ios" 
    ms.devlang="objective-c" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="bridge-ios-webview-with-native-mobile-engagement-ios-sdk"></a>IOS mostka widok sieci Web przy użyciu natywnego iOS zaangażowania Mobile SDK

> [AZURE.SELECTOR]
- [Mostek android](mobile-engagement-bridge-webview-native-android.md)
- [iOS mostka](mobile-engagement-bridge-webview-native-ios.md)

Niektóre aplikacje mobilne są przeznaczone jako aplikację hybrydowe, których samej aplikacji jest utworzony przy użyciu rozwoju natywnych iOS celem-C, ale niektóre lub nawet wszystkich ekranów są renderowane w systemie iOS widok sieci Web. Możesz nadal korzystać z zaangażowania Mobile iOS SDK takich aplikacji i tego samouczka opisano, jak Przejdź o w ten sposób. 

Istnieją dwie metody, aby osiągnąć ten cel, ale są nieudokumentowanych:

- Najpierw jedną opisano dla tego [łącza](http://stackoverflow.com/questions/9826792/how-to-invoke-objective-c-method-from-javascript-and-send-back-data-to-javascrip) obejmuje rejestrowanie `UIWebViewDelegate` w widoku strony sieci web i efektywnej i natychmiast Anuluj zmiany lokalizacji w Javascript. 
- Drugi jedną jest oparty na tej [Sesja WWDC 2013](https://developer.apple.com/videos/play/wwdc2013/615)metody, który jest bardziej czytelny niż pierwszy wiersz i firma Microsoft będzie wykonaj dla tego przewodnika. Należy zauważyć, że ta metoda działa tylko na systemu iOS7 i powyżej. 

Dla systemu iOS Mostek próbki, wykonaj następujące czynności:

1. Przede wszystkim należy się upewnić, że przejściu nasz [Samouczek Wprowadzenie do](mobile-engagement-ios-get-started.md) integrowania iOS zaangażowania Mobile SDK w aplikacji hybrydowego. Opcjonalnie można włączyć, test rejestrowania w następujący sposób, aby mogli przeglądać metody SDK, jak firma Microsoft wyzwalanie widok sieci Web. 
    
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
           ....
            [EngagementAgent setTestLogEnabled:YES];
           ....
        }

2. Teraz upewnij się, że aplikacji hybrydowych zawiera ekran z widoku sieci Web na nim. Możesz dodać ją do `Main.storyboard` aplikacji. 

3. Skojarzyć z **ViewController** ten widok sieci Web, klikając i przeciągając widok sieci Web z sceny Kontroler Widok do `ViewController.h` edytowanie ekranu, umieszczając je tuż poniżej `@interface` linii. 

4. Gdy to zrobisz, okno dialogowe zostanie wyskakujące monitu o podanie nazwy. Wprowadź nazwę jako **Widok sieci Web**. Usługi `ViewController.h` pliku powinien wyglądać następująco:

        #import <UIKit/UIKit.h>
        #import "EngagementViewController.h"
        
        @interface ViewController : EngagementViewController
        @property (strong, nonatomic) IBOutlet UIWebView *webView;
        
        @end

5. Firma Microsoft zaktualizuje `ViewController.m` pliku później, ale najpierw zostanie utworzony plik mostka, który tworzy opakowanie na niektóre często używane iOS zaangażowania Mobile SDK metod. Tworzenie nowego pliku nagłówka o nazwie **EngagementJsExports.h** , która używa `JSExport` mechanizmu opisanego wyżej [sesji](https://developer.apple.com/videos/play/wwdc2013/615) udostępniania metody natywnych iOS. 

        #import <Foundation/Foundation.h>
        #import <JavaScriptCore/JavascriptCore.h>
        
        @protocol EngagementJsExports <JSExport>
        
        + (void) sendEngagementEvent:(NSString*) name :(NSString*)extras;
        + (void) startEngagementJob:(NSString*) name :(NSString*)extras;
        + (void) endEngagementJob:(NSString*) name;
        + (void) sendEngagementError:(NSString*) name :(NSString*)extras;
        + (void) sendEngagementAppInfo:(NSString*) appInfo;
        
        @end

        @interface EngagementJs : NSObject <EngagementJsExports>

        @end

6. Następnie zostanie utworzony drugiej części pliku mostka. Tworzenie pliku o nazwie **EngagementJsExports.m** , który będzie zawierał implementacji tworzenia otoki rzeczywisty przez wywołanie metody SDK systemu iOS zaangażowania Mobile. Również Zauważ, że firma Microsoft podczas analizowania `extras` były przekazywane z kodu javascript widok sieci Web, a które do umieszczania `NSMutableDictionary` obiektu w celu przekazania połączeń metody SDK zaangażowania.  

        #import <UIKit/UIKit.h>
        #import "EngagementAgent.h"
        #import "EngagementJsExports.h"
        
        @implementation EngagementJs
        
        +(void) sendEngagementEvent:(NSString*)name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] sendEvent:name extras:extrasInput];
        }
        
        + (void) startEngagementJob:(NSString*) name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] startJob:name extras:extrasInput];
        }
        
        + (void) endEngagementJob:(NSString*) name {
           [[EngagementAgent shared] endJob:name];
        }
        
        + (void) sendEngagementError:(NSString*) name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] sendError:name extras:extrasInput];
        }
        
        + (void) sendEngagementAppInfo:(NSString*) appInfo {
           NSMutableDictionary* appInfoInput = [self ParseExtras:appInfo];
           [[EngagementAgent shared] sendAppInfo:appInfoInput];
        }
        
        + (NSMutableDictionary*) ParseExtras:(NSString*) input {
           NSData *data = [input dataUsingEncoding:NSUTF8StringEncoding];
           NSError* error = nil;
           NSMutableDictionary* extras = [NSJSONSerialization JSONObjectWithData:data options:0 error:&error];
           
           return extras;
        }
        
        @end

5. Teraz możemy wróć do **ViewController.m** i zaktualizować następujący kod: 
        
        #import <JavaScriptCore/JavaScriptCore.h>
        #import "ViewController.h"
        #import "EngagementJsExports.h"
        
        @interface ViewController ()
        
        @end
        
        @implementation ViewController
        
        - (void)viewDidLoad
        {
           self.webView.delegate = self;
           [super viewDidLoad];
           [self loadWebView];
        }
        
        - (void)loadWebView {
           NSBundle* mainBundle = [NSBundle mainBundle];
           NSURL* htmlPage = [mainBundle URLForResource:@"LocalPage" withExtension:@"html"];
           
           NSURLRequest* urlReq = [NSURLRequest requestWithURL:htmlPage];
           [self.webView loadRequest:urlReq];
        }
        
        - (void)webViewDidFinishLoad:(UIWebView*)wv
        {
           JSContext* context = [wv valueForKeyPath:@"documentView.webView.mainFrame.javaScriptContext"];
           
           context[@"EngagementJs"] = [EngagementJs class];
        }
        
        - (void)webView:(UIWebView*)wv didFailLoadWithError:(NSError*)error
        {
           NSLog(@"Error for WEBVIEW: %@", [error description]);
        }
        
        - (void)didReceiveMemoryWarning {
           [super didReceiveMemoryWarning];
           // Dispose of any resources that can be recreated.
        }
        
        @end

6. Zwróć uwagę następujące kwestie dotyczące pliku **ViewController.m** :

    - W `loadWebView` metody, możemy ładowania lokalnego pliku o nazwie **LocalPage.html** którego kod możemy przejrzeć dalej. 
    - W `webViewDidFinishLoad` metody, możemy są chwytając `JsContext` i kojarzenia z nim klasy Nasze opakowanie. Dzięki temu będzie wywoływania naszych opakowanie metod SDK za pomocą uchwytu **EngagementJs** widok sieci Web. 

7. Utwórz plik o nazwie **LocalPage.html** następujący kod:

        <!doctype html>
        <html>
           <head>
               <style type='text/css'>
                   html { font-family:Helvetica; color:#222; }
                   h1 { color:steelblue; font-size:22px; margin-top:16px; }
                   h2 { color:grey; font-size:14px; margin-top:18px; margin-bottom:0px; }
               </style>
               
               <script type="text/javascript">
               
               window.onerror = function(err)
               {
                   alert('window.onerror: ' + err);
               }
               
               function send(inputId)
               {
                   var input = document.getElementById(inputId);
                   if(input)
                   {
                       var value = input.value;
                       // Example of how extras info can be passed with the Engagement logs
                       var extras = '{"CustomerId":"MS290011"}';
                   }
                   
                   if(value && value.length > 0)
                   {
                       switch(inputId)
                       {
                           case "event":
                           EngagementJs.sendEngagementEvent(value, extras);
                           break;
                           
                           case "job":
                           EngagementJs.startEngagementJob(value, extras);
                           window.setTimeout(
                           function(){
                               EngagementJs.endEngagementJob(value);
                           }, 10000 );
                           break;
        
                           case "error":
                           EngagementJs.sendEngagementError(value, extras);
                           break;
                           
                           case "appInfo":
                           var appInfo = '{"customer_name":"' + value + '"}';
                           EngagementJs.sendEngagementAppInfo(appInfo);
                           break;
                       }
                   }
               }
               </script>
               
           </head>
           <body>
               <h1>Bridge Tester</h1>
               
               <div id='engagement'>
                   
                   <br/>
                   <h2>Event</h2>
                   <input type="text" id="event" size="35">
                   <button onclick="send('event')">Send</button>
                   
                   <br/>
                   <h2>Job</h2>
                   <input type="text" id="job" size="35">
                   <button onclick="send('job')">Send</button>
                   
                   <br/>
                   <h2>Error</h2>
                   <input type="text" id="error" size="35">
                   <button onclick="send('error')">Send</button
                   
                   <br/>
                   <h2>AppInfo</h2>
                   <input type="text" id="appInfo" size="35">
                   <button onclick="send('appInfo')">Send</button>
               
               </div>
           </body>
        </html>

8. Zwróć uwagę następujące kwestie dotyczące powyżej pliku HTML:

    -   Zawiera zestaw pól wprowadzania danych, której można wprowadzić danych może być używany jako nazwy dla wydarzenia, zadania, błąd, AppInfo. Po kliknięciu przycisku obok niej połączenia podejmowana Javascript, która wywołuje na końcu metody z pliku mostka, aby przekazać ten połączenia w systemie iOS zaangażowania Mobile SDK. 
    -   Firma Microsoft są znakowania na niektórych statyczne dodatkowe informacje, aby zdarzenia, zadań i nawet błędów w celu zademonstrowania, jak można to zrobić. Te dodatkowe informacje o jest wysyłana jako ciąg JSON, które w `EngagementJsExports.m` plików, jest przeanalizować i przekazywane wraz z wysyłanie zdarzenia, zadań, problemów. 
    -   Zadanie zaangażowania Mobile jest kopać nazwą określić w polu wprowadzania danych, uruchom 10 sekund i zamknij. 
    -   Appinfo zaangażowania Mobile lub znacznik są przekazywane z "customer_name" statyczny klucz i wartość, wprowadzone w danych wejściowych jako wartość znacznika. 
 
9. Uruchom aplikację i pojawi się poniżej. Teraz udostępnia kilka nazwę zdarzenia test, jak na następującym przykładzie i kliknij przycisk **Wyślij** obok niej. 

    ![][1]

10. Jeśli przejdziesz na karcie **Monitor** aplikacji i wygląd w obszarze **zdarzenia -> Szczegóły**, zostanie wyświetlone to zdarzenie są wyświetlane razem z statyczne aplikacji — informacje o którą firma Microsoft są wysyłane. 

    ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-ios/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-ios/event-output.png
