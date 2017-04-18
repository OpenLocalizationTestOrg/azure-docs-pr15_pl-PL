<properties 
    pageTitle="Integracja SDK Uniwersalny dostęp do aplikacji systemu Windows" 
    description="Jak zintegrować Reach Azure zaangażowania urządzeń przenośnych z systemem Windows uniwersalny aplikacjami"
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

# <a name="windows-universal-apps-reach-sdk-integration"></a>Integracja SDK Uniwersalny dostęp do aplikacji systemu Windows

Należy wykonać opisaną Integracja z programem [Windows uniwersalny zaangażowania SDK integracji](mobile-engagement-windows-store-integrate-engagement.md) przed wykonaniem tego przewodnika.

## <a name="embed-the-engagement-reach-sdk-into-your-windows-universal-project"></a>Osadzanie SDK osiągnięcia zaangażowania w projekcie uniwersalny systemu Windows

Nie masz nic, aby dodać. `EngagementReach`odwołania i zasoby są już w projekcie.

> [AZURE.TIP] Możesz dostosować obrazów znajduje się w `Resources` folderu projektu, w szczególności ikona marki (to ustawienie domyślne ikony zaangażowania). Na uniwersalny aplikacji można również przesuwać `Resources` folder dla udostępnionego projektu, aby udostępnić jej zawartość między aplikacjami, ale należy zachować `Resources\EngagementConfiguration.xml` pliku w lokalizacji domyślnej, ponieważ jest zależne platformy.

## <a name="enable-the-windows-notification-service"></a>Włączanie usługi powiadomień systemu Windows

### <a name="windows-8x-and-windows-phone-81-only"></a>Windows 8.x i Windows Phone 8.1 tylko

Aby można było korzystać z **Usługi powiadomień systemu Windows** (nazywane WNS) w swojej `Package.appxmanifest` plików na `Application UI` polecenie `All Image Assets` w polu po lewej stronie robot. Po prawej stronie pola w `Notifications`, zmienianie `toast capable` z `(not set)` do `(Yes)`.

### <a name="all-platforms"></a>Wszystkie platformy

Należy zsynchronizować aplikacji do swojego konta Microsoft i platformie zaangażowania. W tym musisz utworzyć konto lub zaloguj się [Centrum deweloperów systemu windows](https://dev.windows.com). Po wykonaniu tej Tworzenie nowej aplikacji i Znajdź identyfikator zabezpieczeń i klucza tajnego. W przypadku frontend zaangażowania przejść na ustawienia aplikacji w `native push` i wklejanie poświadczenia. Po wykonaniu tej, kliknij prawym przyciskiem myszy nad projektem, wybierz pozycję `store` i `Associate App with the Store...`. Wystarczy wybierz aplikację utworzono przed, aby zsynchronizować go.

## <a name="initialize-the-engagement-reach-sdk"></a>Inicjowanie Reach zaangażowania SDK

Modyfikowanie `App.xaml.cs`:

-   Wstawianie `EngagementReach.Instance.Init` tylko po `EngagementAgent.Instance.Init` w swojej `InitEngagement` metody:

        private void InitEngagement(IActivatedEventArgs e)
        {
          EngagementAgent.Instance.Init(e);
          EngagementReach.Instance.Init(e);
        }

    `EngagementReach.Instance.Init` Działa w dedykowanej wątku. Nie masz zrobić samodzielnie.

> [AZURE.NOTE] Jeśli używasz powiadomienia wypychane innym miejscu w aplikacji następnie należy [udostępnić kanału wypychanych](#push-channel-sharing) osiągnięcia zaangażowania.

## <a name="integration"></a>Integracja z programem

Zaangażowania udostępnia dwa sposoby dodawania Reach w aplikacji transparenty i międzysegmentowe widoków ogłoszenia i ankiety w aplikacji: integracja nakładki oraz integracja ręcznego widoki sieci web. Nie należy wpisywać obie metody na tej samej stronie.

Wybór między dwoma integracji może zostać podsumowane w ten sposób:

-   Możesz wybrać integracji nakładki Jeśli stron już dziedziczy agenta `EngagementPage`, jest wystarczy zamieniania `EngagementPage` przez `EngagementPageOverlay` i `xmlns:engagement="using:Microsoft.Azure.Engagement"` przez `xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"` na stronach.
-   Możesz wybrać widoków web integracja ręczna Jeśli chcesz umieścić dokładnie osiągnięcia interfejsu użytkownika w stron lub jeśli nie chcesz, aby dodać kolejnego poziomu dziedziczenie do stron sieci. 

### <a name="overlay-integration"></a>Integracja nakładki

Nakładki zaangażowania dynamicznie dodaje elementy interfejsu użytkownika umożliwia wyświetlanie kampanii Reach na stronie. Jeśli nie lubisz się nakładki układu należy rozważyć widoki sieci web integracja ręczna zamiast tego.

W przypadku zmiany pliku .xaml `EngagementPage` odwołuje się do`EngagementPageOverlay`

-   Dodać do swojej deklaracji nazw:

        xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"

-   Zamienianie `engagement:EngagementPage` z `engagement:EngagementPageOverlay`:

**Z EngagementPage:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
        
            <!-- Your layout -->
        </engagement:EngagementPage>

**Z EngagementPageOverlay:**

        <engagement:EngagementPageOverlay 
            xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay">
        
            <!-- Your layout -->
        </engagement:EngagementPageOverlay>

Następnie w pliku CS znakowanie strony w `EngagementPageOverlay` zamiast `EngagementPage` i zaimportować `Microsoft.Azure.Engagement.Overlay`.

            using Microsoft.Azure.Engagement.Overlay;

-   Zamienianie `EngagementPage` z `EngagementPageOverlay`:

**Z EngagementPage:**

            using Microsoft.Azure.Engagement;
            
            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPage
              {
                [...]
              }
            }

**Z EngagementPageOverlay:**

            using Microsoft.Azure.Engagement.Overlay;
            
            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPageOverlay 
              {
                [...]
              }
            }


Dodaje nakładki zaangażowania `Grid` element na stronie składający się z układu i dwa `WebView` elementów z nich transparentem i drugi międzysegmentowe widoku.

Można dostosowywać elementy nakładki bezpośrednio w `EngagementPageOverlay.cs` pliku.

### <a name="web-views-manual-integration"></a>Integracja ręczna widoki sieci Web

Reach będą wyszukiwać stron dla dwóch `WebView` elementy odpowiedzialny za wyświetlanie transparentu i międzysegmentowe widoku. Jedynym elementem, musisz wykonać jest dodanie tych dwóch `WebView` elementy gdzieś na stronach, Oto przykład:

    <Grid x:Name="engagementGrid">

      <!-- Your layout -->

      <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Stretch" VerticalAlignment="Top"/>
      <WebView x:Name="engagement_announcement_content" Visibility="Collapsed"  HorizontalAlignment="Stretch"  VerticalAlignment="Stretch"/>
    </Grid>


W tym przykładzie `WebView` elementy są rozciągnięta w celu dopasowania ich kontenera, który automatycznie ponownie rozmiary ich w przypadku zmiany rozmiaru ekranu obrotu lub okna.

> [AZURE.WARNING] Ważne jest, aby zachować samej nazw `engagement_notification_content` i `engagement_announcement_content` dla `WebView` elementy. Reach jest identyfikowanie ich nazwy. 

## <a name="handle-datapush-optional"></a>Uchwyt datapush (opcjonalnie)

Aplikację, aby mieć możliwość odbierania Reach umieszcza danych, należy zaimplementować dwa zdarzenia klasy EngagementReach:

W App.xaml.cs w Konstruktorze App() Dodaj:

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

## <a name="customize-ui-optional"></a>Dostosowywanie interfejsu użytkownika (opcjonalnie)

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

Następnie, ustawić zawartość `EngagementReach.Instance.Handler` pola z niestandardowego obiektu w swojej `App.xaml.cs` klasy w `App()` metody.

**Przykładowy kod:**

            protected override void OnLaunched(LaunchActivatedEventArgs args)
            {
              // your app initialization 
              EngagementReach.Instance.Handler = new ExampleReachHandler();
              // Engagement Agent and Reach initialization
            }

> [AZURE.NOTE]Domyślnie zaangażowania korzysta z własną implementację `EngagementReachHandler`.
> Nie trzeba tworzyć własne, a jeśli zrobisz, nie trzeba zastępować każdej metody. Domyślne zachowanie należy wybrać zaangażowania obiektu podstawowego.

### <a name="web-view"></a>Widok sieci Web

Domyślnie Reach użyje zasoby osadzone biblioteki do wyświetlania powiadomienia i stron.

Aby zapewnić pełne dostosowywanie możliwości korzystamy są tylko widoku strony sieci web. Jeśli chcesz dostosowywanie układów bezpośrednio zastępowania plików zasobów `EngagementAnnouncement.html` i `EngagementNotification.html`. Zaangażowania musi cały kod w `<body></body>` do prawidłowego działania. Ale możesz dodać znacznik zewnętrzne `engagement_webview_area`.

Można korzystać z własnych zasobów.

Można zastąpić `EngagementReachHandler` metod w swojej klasy zaangażowania układach, ale należy ustalić osadzony mechanizm zaangażowania:

**Przykładowy kod:**
            
            // In your subclass of EngagementReachHandler
            
            public override string GetAnnouncementHTML()
            {
              return base.GetAnnouncementHTML();
            }
            public override string GetAnnouncementName()
            {
              return base.GetAnnouncementName();
            }
            public override string GetNotfificationHTML()
            {
              return base.GetNotfificationHTML();
            }
            public override string GetNotfificationName()
            {
              return base.GetNotfificationName();
            }


Domyślnie jest AnnouncementHTML `ms-appx-web:///Resources/EngagementAnnouncement.html`. Przedstawia plik html, który zaprojektować zawartość wiadomości wypychanych (zawiadomienie o tekstu, anoucement sieci Web i zawiadomienie o ankiety). AnnouncementName jest `engagement_announcement_content`. Jest to nazwa projektu Widok sieci Web na stronie xaml.

NotfificationHTML jest `ms-appx-web:///Resources/EngagementNotification.html`. Przedstawia plik html, który projektowanie powiadomień wypychanych wiadomości. NotfificationName jest `engagement_notification_content`. Jest to nazwa projektu Widok sieci Web na stronie xaml.

### <a name="customization"></a>Dostosowywanie

Możesz dostosować powiadomienie i powiadomienia, że widoku strony sieci web możesz Jeśli zachowanie zaangażowania obiektu. Zwrócić uwagę, że widok sieci Web obiekt opisano trzy razy — po raz pierwszy w swojej xaml po raz drugi w pliku CS w metodzie "setwebview()", a trzecia czasu w pliku html.

-   W swojej xaml możesz opisać bieżącego składnika układu graficznego w widoku sieci Web.
-   W pliku CS można zdefiniować "setwebview()", w którym możesz ustawić wymiar dwóch widok sieci Web (powiadomienie, zawiadomienie o). Jest bardzo skutecznych, gdy aplikacja jest zmiany rozmiaru.
-   W pliku html zaangażowania opisano zawartości widoku sieci Web, projektowanie i położenia elementów między sobą.

### <a name="launch-message"></a>Uruchamianie wiadomości

Gdy użytkownik kliknie powiadomienie systemu (wyskakującego), zaangażowania uruchamia aplikację, załaduj treść wiadomości wypychanych i wyświetlić stronę dla odpowiednich kampanii.

Istnieje opóźnienie między uruchamianie aplikacji i wyświetlanie strony (w zależności od szybkości sieci).

Aby wskazać użytkownikowi coś ładowania, należy podać wizualne informacje, takie jak pasek postępu lub wskaźnik postępu. Zaangażowania nie obsługuje, ale oferuje kilka obsługi dla Ciebie.

Aby zastosować zwrotnego, w App.xaml.cs w "{} App() publicznej" Dodaj:

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

Można ustawić zwrotnego w metodę "App() publicznej {}" usługi `App.xaml.cs` pliku najlepiej przed `EngagementReach.Instance.Init()` połączeń.

> [AZURE.TIP] Każdy program obsługi jest wywoływana przez wątku interfejsu użytkownika. Nie musisz martwić się podczas korzystania z MessageBox lub coś związane z interfejsu użytkownika.

##<a id="push-channel-sharing"></a>Udostępnianie kanału powiadomienia push

Jeśli korzystasz z powiadomień wypychanych dla innego celu w aplikacji następnie musisz użyć kanału wypychanych funkcja SDK zaangażowania udostępniania. To, aby uniknąć nieodebranych wypychanych.

- Można udostępnić własne kanału wypychanych do zainicjowania osiągnięcia zaangażowania. Zestaw SDK użyje go zamiast żąda nową.

Aktualizowanie inicjowanie osiągnięcia zaangażowania z kanału wypychanych w `InitEngagement` metody `App.xaml.cs` pliku:
    
    /* Your own push channel logic... */
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
    
    /*...Engagement initialization */
    EngagementAgent.Instance.Init(e);
    EngagementReach.Instance.Init(e,pushChannel);

- Możesz też jeśli chcesz po prostu używanie kanału wypychanych po możesz inicjowanie Reach można ustawić zwrotnego zaangażowania osiągnięcia uzyskanie kanału wypychanych po jego utworzeniu przez zestaw SDK.

Ustawianie swojego zwrotnego w dowolnym miejscu **po** inicjowanie Reach:

    /* Set action on the SDK push channel. */
    EngagementReach.Instance.SetActionOnPushChannel((PushNotificationChannel channel) => 
    {
      /* The forwarded channel can be null if its creation fails for any reason. */
      if (channel != null)
      {
        /* Your own push channel logic... */
      });
    }

## <a name="custom-scheme-tip"></a>Porada niestandardowy schemat

Firma Microsoft udostępnia Użyj niestandardowy schemat. Możesz wysłać innego typu identyfikatora URI z zaangażowania serwera sieci Web może być używany w aplikacji zaangażowania. Domyślny schemat, takich jak `http, ftp, ...` są zarządzanie monit przez system Windows, będzie okna w przypadku domyślnej aplikacji zainstalowanego na urządzeniu. Można także tworzyć własne niestandardowy schemat aplikacji.

Prosty sposób, aby ustawić niestandardowy schemat w aplikacji jest otwarcie Twojej `Package.appxmanifest` Przejdź w `Declarations` panelu. Wybierz pozycję `Protocol` deklaracje dostępne przewiń pole i dodaj go. Edytowanie `Name` pole z Twojej nowy protokół żądaną nazwę.

Aby użyć tego protokołu, edytować usługi `App.xaml.cs` z `OnActivated` metody i nie zapomnij też zainicjować zaangażowania tutaj:

            /// <summary>
            /// Enter point when app his called by another way than user click
            /// </summary>
            /// <param name="args">Activation args</param>
            protected override void OnActivated(IActivatedEventArgs args)
            {
              /* Init engagement like it was launch by a custom uri scheme */
              EngagementAgent.Instance.Init(args);
              EngagementReach.Instance.Init(args);
            
              //TODO design action to do when app is launch
            
              #region Custom scheme use
              if (args.Kind == ActivationKind.Protocol)
              {
                ProtocolActivatedEventArgs myProtocol = (ProtocolActivatedEventArgs)args;
            
                if (myProtocol.Uri.Scheme.Equals("protocolName"))
                {
                  string path = myProtocol.Uri.AbsolutePath;
                }
              }
              #endregion
 
