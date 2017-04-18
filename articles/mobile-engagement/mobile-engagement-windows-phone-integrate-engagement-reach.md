<properties 
    pageTitle="Windows Phone Silverlight Reach SDK integracji" 
    description="Jak zintegrować Reach Azure zaangażowania urządzeń przenośnych z aplikacjami programu Silverlight Windows Phone"                    
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article"
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-reach-sdk-integration"></a>Windows Phone Silverlight Reach SDK integracji

Należy wykonać opisaną Integracja z programem [Windows Phone Silverlight zaangażowania SDK integracji](mobile-engagement-windows-phone-integrate-engagement.md) przed wykonaniem tego przewodnika.

##<a name="embed-the-engagement-reach-sdk-into-your-windows-phone-silverlight-project"></a>Osadzanie SDK osiągnięcia zaangażowania w projekcie Silverlight Windows Phone

Nie masz nic, aby dodać. `EngagementReach`odwołania i zasoby są już w projekcie.

> [AZURE.TIP]  Możesz dostosować obrazów znajduje się w `Resources` folderu projektu, w szczególności ikona marki (to ustawienie domyślne ikony zaangażowania).

##<a name="add-the-capabilities"></a>Dodawanie funkcji

SDK osiągnięcia zaangażowania musi pewne dodatkowe funkcje.

Otwieranie swojej `WMAppManifest.xml` plik i upewnij się, że zgłaszane są następujące możliwości:

-   `ID_CAP_PUSH_NOTIFICATION`
-   `ID_CAP_WEBBROWSERCOMPONENT`

Pierwszy z nich jest używane przez usługę MPNS umożliwia wyświetlanie wyskakującego powiadomienia. Drugi służy do osadzania zadania przeglądarki do zestawu SDK.

Edytowanie `WMAppManifest.xml` plik i dodać wewnątrz `<Capabilities />` znacznika:

    <Capability Name="ID_CAP_PUSH_NOTIFICATION" />
    <Capability Name="ID_CAP_WEBBROWSERCOMPONENT" />

##<a name="enable-the-microsoft-push-notification-service"></a>Włączanie Usługa powiadomień Push firmy Microsoft

Aby można było korzystać z **Usługi wypychanych powiadomień firmy Microsoft** (nazywane MPNS) z `WMAppManifest.xml` plik musi mieć `<App />` znakowanie z `Publisher` ustawić atrybutu nazwy projektu.

##<a name="initialize-the-engagement-reach-sdk"></a>Inicjowanie Reach zaangażowania SDK

### <a name="engagement-configuration"></a>Konfiguracja zaangażowania

Konfiguracja zaangażowania jest scentralizowane w `Resources\EngagementConfiguration.xml` pliku projektu.

Edytowanie tego pliku, aby określić reach konfiguracji:

-   *Opcjonalnie*wskazują, czy włączono natywnych wypychanych (MPNS), czy między `<enableNativePush>` i `</enableNativePush>` znaczniki, (`true` domyślnie).
-   *Opcjonalnie*, wskazać nazwę kanału wypychanych między `<channelName>` i `</channelName>` znaczniki, podaj to aplikacja może obecnie za pomocą lub pozostaw to pole puste.

Jeśli chcesz określić je w czasie rzeczywistym, możesz zadzwonić poniższej metody przed inicjowanie agenta zaangażowania:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
    
    engagementConfiguration.Reach.EnableNativePush = true;                  
    /* [Optional] whether the native push (MPNS) is activated or not. */
    
    engagementConfiguration.Reach.ChannelName = "YOUR_PUSH_CHANNEL_NAME";   
    /* [Optional] Provide the same channel name that your application may currently use. */
    
    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

> [AZURE.TIP] Możesz określić nazwę kanału wypychanych MPNS aplikacji. Domyślnie zaangażowania tworzy nazwę oparte na identyfikator. Masz bez konieczności należy wpisać nazwę użytkownika, z wyjątkiem Jeśli zamierzasz użyć kanału wypychanych poza zaangażowania.

### <a name="engagement-initialization"></a>Inicjowanie zaangażowania

Modyfikowanie `App.xaml.cs`:

-   Dodawanie do swojego `using` instrukcje:

        using Microsoft.Azure.Engagement;

-   Wstawianie `EngagementReach.Instance.Init` tylko po `EngagementAgent.Instance.Init` w `Application_Launching` :

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }

-   Wstawianie `EngagementReach.Instance.OnActivated` w `Application_Activated` metody:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }

> [AZURE.IMPORTANT] `EngagementReach.Instance.Init` Działa w dedykowanej wątku. Nie masz zrobić samodzielnie.

##<a name="app-store-submission-considerations"></a>Zagadnienia dotyczące przesyłania ze sklepu App

Microsoft nakłada pewne reguły, używając powiadomienia wypychane:

W dokumentacji firmy Microsoft [Zasady aplikacji] sekcji 2.9:

1) Musisz poprosić użytkownikowi Zaakceptuj, aby otrzymywać powiadomienia push. Następnie należy w ustawieniach, należy dodać możliwość wyłączenia powiadomień wypychanych.

Obiekt EngagementReach oferuje dwie metody do zarządzania przystąpienia do programu w i rezygnacji, `EnableNativePush()` i `DisableNativePush()`. Można na przykład utworzyć odpowiednią opcję w obszarze Ustawienia z przełącznika, aby wyłączyć lub włączyć MPNS.

Można również określić zdezaktywować MPNS przy użyciu konfiguracji zaangażowania\<systemu windows phone — sdk-reach konfiguracji\>.

> 2.9.1) aplikacji należy najpierw opis powiadomienia dostarczanych i **uzyskania wyraźnej zgody użytkownika (Wyraź zgodę na)**i **musi obsługiwać mechanizm, za pomocą której użytkownik zrezygnować z otrzymywania powiadomień push**. Wszystkie powiadomienia realizowane przy użyciu usługi powiadomień Push programu Microsoft muszą być zgodne z opisem udostępnionej użytkownikowi i muszą spełniać wszystkie odpowiednie [Zasady aplikacji]  [ Content Policies] i [Dodatkowe wymagania dla określonych typów aplikacji].

2) Nie należy używać zbyt wiele powiadomienia wypychane. Zaangażowania będzie obsługiwany powiadomienia dla Ciebie.

> 2.9.2) aplikacji i jego stosowania Usługa powiadomień Push programu Microsoft musi nie nadmiernie za pomocą pojemności sieci lub przepustowości usługi Microsoft wypychanych powiadomień w przeciwnym razie nadmiernie obciążać Windows Phone lub inne urządzenie firmy Microsoft lub usługi powiadomień wypychanych nadmiarowe, określoną przez firmę Microsoft w jego rozsądne uznania i musi nie szkody lub zakłócać sieci Microsoft lub serwerów lub dowolnej innej firmy serwerów lub sieciach podłączonych do Usługa powiadomień Push programu Microsoft.

3) Nie są oparte na MPNS do wysyłania informacji criticals. Zaangażowania używa MPNS, aby ta reguła dotyczy również kampanii utworzone wewnątrz zaangażowania zewnętrzną.

> 2.9.3) Usługa powiadomień Push programu Microsoft może być używany do wysyłania powiadomień, które wymagają krytyczne lub w inny sposób może mieć wpływ na sprawach życia lub śmierci, łącznie z bez powiadomienia krytyczne ograniczenia związane z medyczne lub warunku. FIRMA MICROSOFT NIE WYRAŹNIE ŻADNYCH GWARANCJI, ŻE WYKORZYSTANIE WYPYCHANYCH MICROSOFT USŁUGA POWIADOMIEŃ LUB DOSTARCZENIA POWIADOMIENIA USŁUGA POWIADOMIEŃ PUSH PROGRAMU MICROSOFT BĘDZIE NIEPRZERWANIE, BŁĄD BEZPŁATNE LUB W INNY SPOSÓB GWARANTOWANA MA BYĆ WYKONYWANA NA PODSTAWIE W CZASIE RZECZYWISTYM.

**Firma Microsoft nie daje gwarancji, aplikacja przejdzie procesu sprawdzania poprawności Jeśli nie przestrzega tych zaleceń.**

##<a name="handle-data-push-optional"></a>Uchwyt danych push (opcjonalnie)

Aplikację, aby mieć możliwość odbierania Reach umieszcza danych, należy zaimplementować dwa zdarzenia klasy EngagementReach:

    EngagementReach.Instance.DataPushStringReceived += (body) =>
    {
       Debug.WriteLine("String data push message received: " + body);
       return true;
    };
    
    EngagementReach.Instance.DataPushBase64Received += (decodedBody, encodedBody) =>
    {
       Debug.WriteLine("Base64 data push message received: " + encodedBody);
       // Do something useful with decodedBody like updating an image view
       return true;
    };

Widać, że zwrotnego metod zwraca wartość logiczną. Zaangażowania wysyła opinii do końca wstecz po wysłaniu wypychanych danych. Jeśli wywołanie zwrotne zwraca wartość false, `exit` opinii będzie Wyślij. W przeciwnym razie zostanie on `action`. Jeśli nie zwrotnego jest ustawiona dla zdarzenia, `drop` opinii zostanie zwrócony zaangażowania.

> [AZURE.WARNING] Zaangażowania nie będzie mógł otrzymywać opinie wielokrotności zamówienie danych. Jeśli zamierzasz skonfigurować kilka obsługi na zdarzenie, należy pamiętać, że opinii będą odpowiadać do ostatniej jedną wysyłane. W tym przypadku zaleca się zawsze zwraca tę samą wartość, aby uniknąć skomplikowana opinii na zewnętrzną.

##<a name="customize-ui-optional"></a>Dostosowywanie interfejsu użytkownika (opcjonalnie)

### <a name="first-step"></a>Pierwszy krok

Firma Microsoft umożliwia dostosowywanie reach interfejsu użytkownika.

Aby to zrobić, musisz utworzyć rodzaj `EngagementReachHandler` zajęć.

**Przykładowy kod:**

    using Microsoft.Azure.Engagement;
    
    namespace Example
    {
       internal class ExampleReachHandler : EngagementReachHandler
       {
          // Override EngagementReachHandler methods depending on your needs
       }
    }

Następnie, ustawić zawartość `EngagementReach.Instance.Handler` pola z niestandardowego obiektu w swojej `App.xaml.cs` klasy w `Application_Launching` metody.

**Przykładowy kod:**

    private void Application_Launching(object sender, LaunchingEventArgs e)
    {
       // your app initialization 
       EngagementReach.Instance.Handler = new ExampleReachHandler();
       // Engagement Agent and Reach initialization
    }

> [AZURE.NOTE] Domyślnie zaangażowania korzysta z własną implementację `EngagementReachHandler`. Nie trzeba tworzyć własne, a jeśli zrobisz, nie trzeba zastępować każdej metody. Domyślne zachowanie należy wybrać zaangażowania obiektu podstawowego.

### <a name="layouts"></a>Układy

Domyślnie Reach użyje zasoby osadzone biblioteki do wyświetlania powiadomienia i stron.

Można korzystać z własnych zasobów w celu odzwierciedlenia marki w tych składników.

Można zastąpić `EngagementReachHandler` metod w swojej klasy przekazać zaangażowania używać swojej układów:

**Przykładowy kod:**

    // In your subclass of EngagementReachHandler
    
    public override string GetTextViewAnnouncementUri()
    {
       // return the path of your own xaml
    }
    
    public override string GetWebViewAnnouncementUri()
    {
       // return the path of your own xaml
    }
    
    public override string GetPollUri()
    {
       // return the path of your own xaml
    }
    
    public override EngagementNotificationView CreateNotification(EngagementNotificationViewModel viewModel)
    {
       // return a new instance of your own notification
    }

> [AZURE.TIP] `CreateNotification` Metoda może zwracać wartość null. Nie zostaną wyświetlone powiadomienie i kampanii reach zostaną usunięte.

Aby uprościć implementacji układu, oferujemy własnego xaml, który może służyć jako podstawa kodu. Znajdują się one w archiwum SDK zaangażowania (-src-reach-).

> [AZURE.WARNING] Źródła, które udostępniamy są dokładnie takie same jak, które firma Microsoft korzysta z. Tak, jeśli chcesz zmodyfikować je bezpośrednio, nie zapomnij zmienianie nazw i nazwę.

### <a name="notification-position"></a>Położenie powiadomień

Domyślnie na dole po lewej stronie aplikacji zostanie wyświetlone powiadomienie w aplikacji. Można zmienić przez zastąpienie `GetNotificationPosition` metody `EngagementReachHandler` obiektu.

    // In your subclass of EngagementReachHandler
    
    public override EngagementReachHandler.NotificationPosition GetNotificationPosition(EngagementNotificationViewModel viewModel)
    {
       // return a value of the EngagementReachHandler.NotificationPosition enum (TOP or BOTTOM)
    }

Obecnie możesz wybrać jeden z `BOTTOM` (ustawienie domyślne) i `TOP` pozycji.

### <a name="launch-message"></a>Uruchamianie wiadomości

Gdy użytkownik kliknie powiadomienie systemu (wyskakującego), zaangażowania uruchamianie aplikacji, załaduj treść wiadomości wypychanych i wyświetlić stronę dla odpowiednich kampanii.

Istnieje opóźnienie między uruchamianie aplikacji i wyświetlanie strony (w zależności od szybkości sieci).

Aby wskazać użytkownikowi coś ładowania, należy podać wizualne informacje, takie jak pasek postępu lub wskaźnik postępu. Zaangażowania nie obsługuje, ale oferuje kilka obsługi dla Ciebie.

Aby zastosować wywołanie zwrotne, należy wykonać:

    /* The application has launched and the content is loading.
     * You should display an indicator here.
     */
    EngagementReach.Instance.RetrieveLaunchMessageStarted += () => { [...] };
    
    /* The application has finished loading the content and the page
     * is about to be displayed.
     * You should hide the indicator here.
     */
    EngagementReach.Instance.RetrieveLaunchMessageCompleted += () => { [...] };
    
    /* The content has been loaded, but an error has occurred.
     * You can provide an information to the user.
     * You should hide the indicator here.
     */
    EngagementReach.Instance.RetrieveLaunchMessageFailed += () => { [...] };

Wywołanie zwrotne można ustawić w swojej `Application_Launching` metody usługi `App.xaml.cs` pliku najlepiej przed `EngagementReach.Instance.Init()` połączeń.

> [AZURE.TIP] Każdy program obsługi jest wywoływana przez wątku interfejsu użytkownika. Nie musisz martwić się podczas korzystania z MessageBox lub coś związane z interfejsu użytkownika.

[Zasady aplikacji]:http://msdn.microsoft.com/library/windows/apps/hh184841(v=vs.105).aspx
[Content Policies]:http://msdn.microsoft.com/library/windows/apps/hh184842(v=vs.105).aspx
[Dodatkowe wymagania dotyczące typów określonej aplikacji]:http://msdn.microsoft.com/library/windows/apps/hh184838(v=vs.105).aspx
 
