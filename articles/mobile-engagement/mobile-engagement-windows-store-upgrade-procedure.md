<properties 
    pageTitle="Procedury uaktualniania uniwersalny aplikacje zestawu SDK systemu Windows" 
    description="Windows uniwersalny SDK aplikacje uaktualnienia procedury Azure zaangażowania urządzeń przenośnych"                     
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

#<a name="windows-universal-apps-sdk-upgrade-procedures"></a>Procedury uaktualniania uniwersalny aplikacje zestawu SDK systemu Windows

Jeśli już masz zintegrowany ze starszej wersji programu zaangażowania do aplikacji, należy wziąć pod uwagę następujące punkty podczas uaktualniania zestawu SDK.

Być może trzeba wykonać kilka procedury nie odebrano kilka wersji pakietu. Na przykład możesz przeprowadzić migrację z 0.10.1 do 0.11.0, musisz najpierw należy wykonać procedurę "od 0.9.0 do 0.10.1" następnie procedury "od 0.10.1 do 0.11.0".

##<a name="from-330-to-340"></a>Z 3.3.0 do 3.4.0

### <a name="test-logs"></a>Testowanie dzienników

Dzienniki konsoli tworzone przez zestawu SDK można teraz włączone/wyłączone i filtrowane. Aby dostosować tak, zaktualizuj właściwość `EngagementAgent.Instance.TestLogEnabled` do jednej z wartości dostępne na `EngagementTestLogLevel` wyliczenie, na przykład:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="resources"></a>Zasoby

Ulepszono nakładki Reach. Jest częścią zasoby pakietu SDK NuGet.

Podczas uaktualniania do nowej wersji zestawu SDK można wybrać, czy chcesz zachować istniejące pliki z folderu nakładki zasobów, czy nie:

* Jeśli poprzedniego nakładki pracuje dla Ciebie lub są integracji `WebView` elementy ręcznie następnie możesz zdecydować, aby zachować pliki istniejącej, to nadal będzie działać. 
* Aktualizacja do nowego nakładki, po prostu zastąpić całą `overlay` folder od zasobów na nową z pakietu SDK (aplikacje UWP: po uaktualnieniu, zostanie wyświetlony nowy folder w trybie nakładania z % USERPROFILE %\\.nuget\packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [AZURE.WARNING] Przy użyciu nowego nakładki zostaną zastąpione wszelkie zmiany wprowadzone w poprzedniej wersji.

##<a name="from-320-to-330"></a>Z 3.2.0 do 3.3.0

### <a name="resources"></a>Zasoby
Ten krok dotyczy tylko niestandardowych zasobów. Jeśli został on dostosowany zasobów udostępnianych przez SDK (html, obrazów, nakładki) następnie musisz utworzyć kopię zapasową ich przed uaktualnieniem i ponowne stosowanie dostosowań uaktualnione zasobów.

##<a name="from-310-to-320"></a>Z 3.1.0 do 3.2.0

### <a name="resources"></a>Zasoby
Ten krok dotyczy tylko niestandardowych zasobów. Jeśli został on dostosowany zasobów udostępnianych przez SDK (html, obrazów, nakładki) następnie musisz utworzyć kopię zapasową ich przed uaktualnieniem i ponowne stosowanie dostosowań uaktualnione zasobów.

### <a name="webview-integration"></a>Integracja widok sieci Web
Usprawnień zgodnie z innego urządzenia formularz czynniki zostały wprowadzone w tej wersji. Upewnij się, że usługi integracji widok sieci Web zgodne z następującymi:

W swojej XAML strony ():

            <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Right" VerticalAlignment="Top"/>
            <WebView x:Name="engagement_announcement_content" Visibility="Collapsed" HorizontalAlignment="Right" VerticalAlignment="Top"/> 

W pliku CS skojarzone:

    using Microsoft.Azure.Engagement;
    using System;
    using Windows.ApplicationModel.Core;
    using Windows.UI.ViewManagement;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Navigation;

    namespace My.Namespace.Example
    {
            /// <summary>
            /// An empty page that can be used on its own or navigated to within a Frame.
            /// </summary>
            public sealed partial class ExampleEngagementReachPage : EngagementPage
            {
              public ExampleEngagementReachPage()
              {
                this.InitializeComponent();
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }
            
              #region to implement
              /* Attach events when page is navigated. */
              protected override void OnNavigatedTo(NavigationEventArgs e)
              {
                /* Update the webview when the app window is resized. */
                Window.Current.SizeChanged += DisplayProperties_OrientationChanged;

                /* Update the webview when the app/status bar is resized. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged += DisplayProperties_VisibleBoundsChanged; 
    #endif
                base.OnNavigatedTo(e);
              }

              /* When page is left ensure to detach SizeChanged handler. */
              protected override void OnNavigatedFrom(NavigationEventArgs e)
              {
                Window.Current.SizeChanged -= DisplayProperties_OrientationChanged;
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged -= DisplayProperties_VisibleBoundsChanged;
    #endif
                base.OnNavigatedFrom(e);
              }
              
              /* "width" and "height" are the current size of your application display. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
              double width = ApplicationView.GetForCurrentView().VisibleBounds.Width;
              double height = ApplicationView.GetForCurrentView().VisibleBounds.Height;
    #else
              double width =  Window.Current.Bounds.Width;
              double height =  Window.Current.Bounds.Height;
    #endif
            
              /// <summary>
              /// Set your webview elements to the correct size.
              /// </summary>
              /// <param name="width">The width of your current display.</param>
              /// <param name="height">The height of your current display.</param>
              private void SetWebView(double width, double height)
              {
                #pragma warning disable 4014
                CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                        () =>
                        {
                          this.engagement_notification_content.Width = width;
                          this.engagement_announcement_content.Width = width;
                          this.engagement_announcement_content.Height = height;
                        });
              }
            
              /// <summary>
              /// Handler that takes the Windows.Current.SizeChanged and indicates that webviews have to be resized.
              /// </summary>
              /// <param name="sender">Original event trigger.</param>
              /// <param name="e">Window Size Changed Event arguments.</param>
              private void DisplayProperties_OrientationChanged(object sender, Windows.UI.Core.WindowSizeChangedEventArgs e)
              {
                double width = e.Size.Width;
                double height = e.Size.Height;
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }

    #if WINDOWS_PHONE_APP || WINDOWS_UWP              
              /// <summary>
              /// Handler that takes the ApplicationView.VisibleBoundsChanged and indicates that webviews have to be resized
              /// </summary>
              /// <param name="sender">The related application view.</param>
              /// <param name="e">Related event arguments.</param>
              private void DisplayProperties_VisibleBoundsChanged(ApplicationView sender, Object e)
              {
                double width = sender.VisibleBounds.Width;
                double height = sender.VisibleBounds.Height;
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }
    #endif
              #endregion
            }
    }

##<a name="from-200-to-300"></a>Z 2.0.0 do 3.0.0

### <a name="resources"></a>Zasoby
Ten krok dotyczy tylko niestandardowych zasobów. Jeśli został on dostosowany zasobów udostępnianych przez SDK (html, obrazów, nakładki) następnie musisz utworzyć kopię zapasową ich przed uaktualnieniem i ponowne stosowanie dostosowań uaktualnione zasobów.

##<a name="from-111-to-200"></a>Z 1.1.1 do 2.0.0

Poniżej opisano, jak przeprowadzić migrację SDK integracji z usługi Capptain oferowanych przez skojarzenia zabezpieczeń Capptain do aplikacji korzystająca z zaangażowania Mobile Azure. 

> [Azure.IMPORTANT] Capptain i zaangażowania Mobile nie są takie same usługi i procedury przedstawionej poniżej wyróżnienie tylko jak przeprowadzić migrację aplikacji klienta. Migrowanie SDK w aplikacji będą migrowane dane z serwerów Capptain się z serwerami zaangażowania Mobile

Jeśli są migrowane z wcześniejszej wersji, zajrzyj do witryny sieci web Capptain, aby przeprowadzić migrację do 1.1.1, a następnie Zastosuj poniższą procedurę

### <a name="nuget-package"></a>Pakiet Nuget

Zamień **Capptain.WindowsPhone** przez pakiet Nuget **MicrosoftAzure.MobileEngagement** .

### <a name="applying-mobile-engagement"></a>Stosowanie zaangażowania urządzeń przenośnych

Termin korzysta z zestawu SDK `Engagement`. Musisz zaktualizować projektu zgodnie z tej zmiany.

Musisz odinstalować pakietu nuget Capptain bieżącego. Weź pod uwagę, że zostaną usunięte wszystkie zmiany w folderze Capptain zasobów. Jeśli chcesz zachować te pliki, a następnie utwórz kopię.

Po wykonaniu tej instalowanie nowego pakietu Microsoft Azure zaangażowania w nuget nad projektem. Możesz go znaleźć bezpośrednio witrynie [nuget]. lub tutaj indeksu. Ta akcja powoduje zastąpienie wszystkich plików zasobów używanych przez zaangażowania i dodaje nowe DLL zaangażowania do odwołania projektu.

Należy wyczyścić odwołania projektu, usuwając Capptain DLL odwołania. Jeśli nie wprowadzisz to, spowoduje konflikt wersji Capptain i nastąpi błędy.

Jeśli dostosowanych Capptain zasobów, skopiuj zawartość starych plików i wklej je do nowych plików zaangażowania. Należy pamiętać, że zarówno xaml cs pliki i trzeba będzie aktualizowany.

Po wykonaniu tych kroków wystarczy zamienić stary Capptain odwołuje się nowych odwołań zaangażowania.

1. Wszystkie przestrzenie nazw Capptain muszą być zaktualizowane.

    Przed migracją:
    
        using Capptain.Agent;
        using Capptain.Reach;
    
    Po zakończeniu migracji:
    
        using Microsoft.Azure.Engagement;

2. Wszystkie klasy Capptain, które zawierają "Capptain" powinien zawierać "Zaangażowania".

    Przed migracją:
    
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
    
    Po zakończeniu migracji:
    
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }

3. Pliki xaml Capptain nazw i atrybuty również zmienić.

    Przed migracją:
    
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
    
    Po zakończeniu migracji:
    
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>

4. Nakładanie zmiany strony
    > [AZURE.IMPORTANT] Powoduje także nakładki. Jego nowy obszar nazw jest `Microsoft.Azure.Engagement.Overlay`. Musi być używane w plikach zarówno xaml i cs. Ponadto `CapptainGrid` ma być o nazwie `EngagementGrid`, `capptain_notification_content` i `capptain_announcement_content` są nazywane `engagement_notification_content` i `engagement_announcement_content`.
    
    Do nakładki:
    
        <capptain:CapptainPageOverlay
          xmlns:capptain="using:Capptain.Overlay"
          ...
        </capptain:CapptainPageOverlay>
    
    Staje się:
    
        <EngagementPageOverlay
          engagement="using:Microsoft.Azure.Engagement.Overlay"
          ...
        </engagement:EngagementPageOverlay>

5. Dla innych zasoby takie jak Capptain obrazów i plików HTML należy pamiętać, że one także zmieniono umożliwia "Zaangażowania".

### <a name="project-declaration"></a>Deklaracja projektu

Na Package.appxmanifest `File Type Associations` została zaktualizowana z:

 -   capptain\_osiągnięcia\_zawartości do zaangażowania\_osiągnięcia\_zawartości
 -   capptain\_dziennika\_pliku do zaangażowania\_dziennika\_pliku

### <a name="application-id--sdk-key"></a>Identyfikator aplikacji / klucz SDK

Zaangażowania używa parametrów połączenia. Nie musisz określić identyfikator aplikacji i klawisza SDK zaangażowania Mobile, wystarczy, że Określ parametry połączenia. Możesz skonfigurować go na plik EngagementConfiguration.

Konfiguracja zaangażowania można ustawić w swojej `Resources\EngagementConfiguration.xml` pliku projektu.

Edytowanie tego pliku, aby określić:

-   Ciąg połączenia aplikacji między znacznikami `<connectionString>` i `<\connectionString>`.

Jeśli chcesz określić je w czasie rzeczywistym, możesz zadzwonić poniższej metody przed inicjowanie agenta zaangażowania:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
    
    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(args, engagementConfiguration);

Parametry połączenia dla aplikacji zostanie wyświetlona w portalu klasyczny Azure.

### <a name="items-name-change"></a>Zmiana nazwy elementów

Wszystkie elementy o nazwie *capptain* zostały nazwy *zaangażowania*. Podobnie do *Capptain* *zaangażowania*.

Przykłady często używanych elementów Capptain:

-   CapptainConfiguration teraz nazwę EngagementConfiguration
-   CapptainAgent teraz nazwę EngagementAgent
-   CapptainReach teraz nazwę EngagementReach
-   CapptainHttpConfig teraz nazwę EngagementHttpConfig
-   GetCapptainPageName teraz nazwę GetEngagementPageName

Należy zauważyć, że również zmienić wpływa na zastąpić metod.

 
