<properties 
    pageTitle="Mostek Android widok sieci Web przy użyciu natywnego SDK Android zaangażowania Mobile" 
    description="W tym artykule opisano sposób tworzenia mostka między systemem Javascript i natywnych SDK Android zaangażowania Mobile widok sieci Web"      
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-android" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="bridge-android-webview-with-native-mobile-engagement-android-sdk"></a>Mostek Android widok sieci Web przy użyciu natywnego SDK Android zaangażowania Mobile

> [AZURE.SELECTOR]
- [Mostek android](mobile-engagement-bridge-webview-native-android.md)
- [iOS mostka](mobile-engagement-bridge-webview-native-ios.md)

Niektóre aplikacje mobilne są przeznaczone jako aplikację hybrydowe, których samej aplikacji jest utworzony przy użyciu natywnego rozwoju Android, ale niektóre lub nawet wszystkich ekranów są renderowane w Android widok sieci Web. Można nadal korzystać SDK Android zaangażowania Mobile takich aplikacji i tego samouczka opisano, jak Przejdź o w ten sposób. Poniższy przykładowy kod jest oparty na Android dokumentacji [tutaj](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript). Go w tym artykule opisano sposób tej metody udokumentowane mogą służyć do wykonania taki sam dla najczęściej używanych metod Mobile zaangażowania Android zestawu SDK tak, aby widok sieci Web za pomocą aplikacji hybrydowych może również inicjować żądania do śledzenia zdarzeń, zadań, problemów, informacje o aplikacji podczas połączeń rurowych ich przez naszych SDK systemu Android. 

1. Przede wszystkim należy się upewnić, że przejściu nasz [Samouczek Wprowadzenie do](mobile-engagement-android-get-started.md) integrowania SDK Android zaangażowania Mobile w aplikacji hybrydowych. Po wykonaniu tej usługi `OnCreate` metody będzie wyglądać następująco.  
    
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("<Mobile Engagement Conn String>");
            EngagementAgent.getInstance(this).init(engagementConfiguration);
        }

2. Teraz upewnij się, że aplikacji hybrydowych zawiera ekran z widoku sieci Web na nim. Kod go będzie podobny do następującego miejsce, w którym są ładowane lokalne HTML pliku **Sample.html** w widoku sieci Web w `onCreate` metody ekranu. 

        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
        }

        protected void onCreate(Bundle savedInstanceState) {
            ...
            ...
            SetWebView();
        }

3. Teraz utworzyć plik mostka o nazwie **WebAppInterface** , który tworzy opakowanie w niektórych najczęściej używanych metod SDK Android zaangażowania Mobile za pomocą `@JavascriptInterface` podejście opisane w [dokumentacji Android](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript):

        import android.content.Context;
        import android.os.Bundle;
        import android.util.Log;
        import android.webkit.JavascriptInterface;
        
        import com.microsoft.azure.engagement.EngagementAgent;
        
        import org.json.JSONArray;
        import org.json.JSONObject;
        
        public class WebAppInterface {
            Context mContext;
        
            /** Instantiate the interface and set the context */
            WebAppInterface(Context c) {
                mContext = c;
            }
        
            @JavascriptInterface
            public void sendEngagementEvent(String name, String extras ){
                EngagementAgent.getInstance(mContext).sendEvent(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void startEngagementJob(String name, String extras ){
                EngagementAgent.getInstance(mContext).startJob(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void endEngagementJob(String name){
                EngagementAgent.getInstance(mContext).endJob(name);
            }
        
            @JavascriptInterface
            public void sendEngagementError(String name, String extras ){
                EngagementAgent.getInstance(mContext).sendError(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void sendEngagementAppInfo(String appInfo){
                EngagementAgent.getInstance(mContext).sendAppInfo(ParseExtras(appInfo));
            }
        
            public Bundle ParseExtras(String input) {
                Bundle extras = new Bundle();
        
                try {
                    JSONObject jObject = new JSONObject(input);
                    extras.putString(jObject.names().getString(0),
                            jObject.get(jObject.names().getString(0)).toString());
                } catch (Exception e) {
                    e.printStackTrace();
                }
                return extras;
            }
        }  

4. Gdy został utworzony plik mostka powyżej, trzeba upewnij się, że jest skojarzony z naszych widok sieci Web. W tym celu należy edytować usługi `SetWebview` metody, dzięki czemu wygląda następująco:

        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
            WebSettings webSettings = myWebView.getSettings();
            webSettings.setJavaScriptEnabled(true);
            myWebView.addJavascriptInterface(new WebAppInterface(this), "EngagementJs");
        }

5. W powyższym wstawek, możemy o nazwie `addJavascriptInterface` chcesz skojarzyć klasy Nasze mostka naszych widok sieci Web, a także utworzony uchwyt o nazwie **EngagementJs** nawiązać połączenie z pliku mostka metod. 

6. Teraz można utworzyć następujący plik o nazwie **Sample.html** w projekcie w folderze o nazwie **aktywa** której zostanie załadowana do widoku sieci Web i miejsce, w którym będzie nazywamy metody z pliku mostka.

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
                      log('window.onerror: ' + err);
                    }
        
                    send = function(inputId)
                    {
                        var input = document.getElementById(inputId);
                        if(input)
                        {
                            var value = input.value;
                            // Example of how extras info can be passed with the Engagement logs
                            var extras = '{"CustomerId":"MS290011"}';
        
                            if(value && value.length > 0)
                            {
                                switch(inputId)
                                {
                                    case "event":
                                    EngagementJs.sendEngagementEvent(value, extras);
                                    break;
        
                                    case "job":
                                    EngagementJs.startEngagementJob(value, extras);
                                    window.setTimeout( function()
                                    {
                                      EngagementJs.endEngagementJob(value);
                                    }, 10000 );
                                    break;
        
                                    case "error":
                                    EngagementJs.sendEngagementError(value, extras);
                                    break;
        
                                    case "appInfo":
                                    EngagementJs.sendEngagementAppInfo({"customer_name":value});
                                    break;
                                }
                            }
                        }
                    }
                </script>
            </head>
            <body>
                <h1>Bridge Tester</h1>
                <div id='engagement'>
                    <h2>Event</h2>
                    <input type="text" id="event" size="35">
                    <button onclick="send('event')">Send</button>
        
                    <h2>Job</h2>
                    <input type="text" id="job" size="35">
                    <button onclick="send('job')">Send</button>
        
                    <h2>Error</h2>
                    <input type="text" id="error" size="35">
                    <button onclick="send('error')">Send</button>
        
                    <h2>AppInfo</h2>
                    <input type="text" id="appInfo" size="35">
                    <button onclick="send('appInfo')">Send</button>
                </div>
            </body>
        </html>

8. Zwróć uwagę następujące kwestie dotyczące powyżej pliku HTML:

    -   Zawiera zestaw pól wprowadzania danych, której można wprowadzić danych może być używany jako nazwy dla wydarzenia, zadania, błąd, AppInfo. Po kliknięciu przycisku obok Javascript, która wywołuje na końcu metody z mostka pliku do przekazania tego wywołania SDK Android zaangażowania Mobile jest nawiązać połączenia. 
    -   Firma Microsoft są znakowania na niektórych statyczne dodatkowe informacje, aby zdarzenia, zadań i nawet błędów w celu zademonstrowania, jak można to zrobić. Te dodatkowe informacje o jest wysyłana jako ciąg JSON, które w `WebAppInterface` plików, jest przeanalizować i umieścić w systemie Android `Bundle` i przekazywana wraz z wysyłanie zdarzenia, zadań, problemów. 
    -   Zadanie zaangażowania Mobile jest kopać nazwą określić w polu wprowadzania danych, uruchom 10 sekund i zamknij. 
    -   Appinfo zaangażowania Mobile lub znacznik są przekazywane z "customer_name" statyczny klucz i wartość, wprowadzone w danych wejściowych jako wartość znacznika. 
 
9. Uruchom aplikację i pojawi się poniżej. Teraz udostępnia kilka nazwę zdarzenia test, jak na następującym przykładzie i kliknij przycisk **Wyślij** znajdujących się pod nim. 

    ![][1]

10. Jeśli przejdziesz na karcie **Monitor** aplikacji i wygląd w obszarze **zdarzenia -> Szczegóły**, zostanie wyświetlone to zdarzenie są wyświetlane razem z statyczne aplikacji — informacje o którą firma Microsoft są wysyłane. 

    ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-android/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-android/event-output.png
