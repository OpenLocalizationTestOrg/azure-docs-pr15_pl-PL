<properties 
    pageTitle="Windows Phone Silverlight zaangażowania SDK integracji" 
    description="Jak zintegrować Azure zaangażowania urządzeń przenośnych z aplikacjami programu Silverlight Windows Phone"                  
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-engagement-sdk-integration"></a>Windows Phone Silverlight zaangażowania SDK integracji

> [AZURE.SELECTOR] 
- [Uniwersalny systemu Windows](mobile-engagement-windows-store-integrate-engagement.md) 
- [Program Silverlight Windows Phone](mobile-engagement-windows-phone-integrate-engagement.md) 
- [iOS](mobile-engagement-ios-integrate-engagement.md) 
- [Android](mobile-engagement-android-integrate-engagement.md) 

W tej procedurze opisano Najprostszym sposobem aktywowania zaangażowania Mobile Azure analizy i funkcji w aplikacji Silverlight Windows Phone monitorowania.

Poniższe kroki są można aktywować raport dzienniki, aby obliczyć wszystkie statystyki dotyczące użytkowników, sesje, działania, awarii i Technicals. Raport dzienniki potrzebne do obliczania innych statystyk, takich jak zadań, problemów i zdarzeń musi odbywać się ręcznie za pomocą interfejsu API zaangażowania (Dowiedz się, [jak za pomocą zaawansowanych zaangażowania Mobile znakowania interfejsu API w aplikacji Silverlight Windows Phone](mobile-engagement-windows-phone-use-engagement-api.md) poniżej), ponieważ statystyki te są zależne od aplikacji.

##<a name="supported-versions"></a>Obsługiwane wersje

Kierowanie aplikacje mogą być tylko zintegrowane SDK zaangażowania Mobile dla systemu Windows Silverlight:

-   Windows Phone 8.0
-   Windows Phone 8.1 Silverlight

> [AZURE.NOTE] Jeśli są kierowanie Windows Phone 8.1 (innych niż program Silverlight), należy skorzystać z [procedury integracji uniwersalny systemu Windows](mobile-engagement-windows-store-integrate-engagement.md).

##<a name="install-the-mobile-engagement-silverlight-sdk"></a>Zainstaluj zestaw SDK programu Silverlight zaangażowania urządzeń przenośnych

Zestaw SDK zaangażowania Mobile dla systemu Windows Silverlight jest dostępna w pakiecie Nuget o nazwie *MicrosoftAzure.MobileEngagement*. Możesz zainstalować go za pomocą programu Visual Studio Nuget pakiet menedżera. 

##<a name="add-the-capabilities"></a>Dodawanie funkcji

SDK zaangażowania musi pewne możliwości Silverlight SDK Windows Phone, aby działał poprawnie.

Otwieranie swojej `WMAppManifest.xml` plik i upewnij się, że następujące funkcje są deklarowane w `Capabilities` panelu:

-   `ID_CAP_NETWORKING`
-   `ID_CAP_IDENTITY_DEVICE`

##<a name="initialize-the-engagement-sdk"></a>Inicjowanie zaangażowania SDK

### <a name="engagement-configuration"></a>Konfiguracja zaangażowania

Konfiguracja zaangażowania jest scentralizowane w `Resources\EngagementConfiguration.xml` pliku projektu.

Edytowanie tego pliku, aby określić:

-   Ciąg połączenia aplikacji między znacznikami `<connectionString>` i `<\connectionString>`.

Jeśli chcesz określić je w czasie rzeczywistym, możesz zadzwonić poniższej metody przed inicjowanie agenta zaangażowania:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

Parametry połączenia dla aplikacji zostanie wyświetlona w portalu klasyczny Azure.

### <a name="engagement-initialization"></a>Inicjowanie zaangażowania

Podczas tworzenia nowego projektu, `App.xaml.cs` wygenerowanie pliku. Ta klasa dziedziczy `Application` i zawiera wiele metod ważne. Go będzie również zainicjować SDK zaangażowania.

Modyfikowanie `App.xaml.cs`:

-   Dodawanie do swojego `using` instrukcji:

        using Microsoft.Azure.Engagement;

-   Wstawianie `EngagementAgent.Instance.Init` w `Application_Launching` metody:

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
          EngagementAgent.Instance.Init();
        }

-   Wstawianie `EngagementAgent.Instance.OnActivated` w `Application_Activated` metody:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
        }

> [AZURE.WARNING] Firma Microsoft zdecydowanie zapobieżenia go można dodać inicjowanie zaangażowania w innym miejscu aplikacji. Jednak należy pamiętać, że `EngagementAgent.Instance.Init` metoda jest uruchamiany w dedykowanej wątku, a nie na wątku interfejsu użytkownika.

##<a name="basic-reporting"></a>Podstawowe raportowania

### <a name="recommended-method--overload-your-phoneapplicationpage-classes"></a>Zalecana metoda: przeciążeń usługi `PhoneApplicationPage` klasy

Aby aktywować raportu wszystkie dzienniki wymagane przez zaangażowania do obliczenia użytkowników, sesje, działania, awarii i technicznych danych statystycznych, po prostu pozwalające wszystkie usługi `PhoneApplicationPage` klasy podrzędne dziedziczą `EngagementPage` klasy.

Oto przykład jak to zrobić na stronie aplikacji. Możesz zrobić to samo dla wszystkich stron w aplikacji.

#### <a name="c-source-file"></a>Plik źródłowy C#

Modyfikowanie strony `.xaml.cs` pliku:

-   Dodawanie do swojego `using` instrukcje:

        using Microsoft.Azure.Engagement;

-   Zamienianie `PhoneApplicationPage` z `EngagementPage` :

**Bez zaangażowania:**

        namespace Example
        {
          public partial class ExamplePage : PhoneApplicationPage
          {
            [...]
          }
        }

**Z zaangażowania:**

        using Microsoft.Azure.Engagement;
        
        namespace Example
        {
          public partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [AZURE.WARNING] Jeśli strony dziedziczy `OnNavigatedTo` metody, uważaj umożliwić `base.OnNavigatedTo(e)` połączeń. W przeciwnym razie nie zostaną zgłoszone działania. W rzeczywistości `EngagementPage` dzwoni `StartActivity` wewnątrz `OnNavigatedTo` metody.

#### <a name="xaml-file"></a>Plik danych XAML

Modyfikowanie strony `.xaml` pliku:

-   Dodać do swojej deklaracji nazw:

        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"

-   Zamienianie `phone:PhoneApplicationPage` z `engagement:EngagementPage` :

**Bez zaangażowania:**

        <phone:PhoneApplicationPage>
            <!-- layout -->
        </phone:PhoneApplicationPage>

**Z zaangażowania:**

        <engagement:EngagementPage 
            xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP">
        
            <!-- layout -->
        </engagement:EngagementPage >

#### <a name="override-the-default-behavior"></a>Zastępuje domyślne zachowanie

Domyślnie nazwa klasy strony jest zgłoszone jako nazwę aktywności, bez dodatkowych. Jeśli klasy używa sufiks "Strona", zaangażowania spowoduje również usunięcie go.

Jeśli chcesz zastąpić zachowanie domyślne nazwy, po prostu dodaj to kodu:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
           /* your code */
           return "new name";
        }

Jeśli chcesz zgłosić pewne dodatkowe informacje o swojej aktywności, możesz dodać to kodu:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
           /* your code */
           return extra;
        }

Metody te są nazywane z poziomu `OnNavigatedTo` metody strony.

### <a name="alternate-method-call-startactivity-manually"></a>Alternatywne metody: połączenie `StartActivity()` ręcznie

Jeśli nie można lub nie chcesz, aby przeciążeń usługi `PhoneApplicationPage` klasy, możesz zamiast tego Rozpocznij Twoich działań, łącząc `EngagementAgent` bezpośrednio metod.

Zalecamy telefonicznej `StartActivity` wewnątrz usługi `OnNavigatedTo` metody usługi PhoneApplicationPage.

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
           base.OnNavigatedTo(e);
           EngagementAgent.Instance.StartActivity("MyPage");
        }

> [AZURE.IMPORTANT] Upewnij się, że poprawnie zakończyć sesję.
>
> Zestaw SDK automatycznie połączeń `EndActivity` metody, gdy aplikacja zostanie zamknięta. Dlatego **zdecydowanie** zalecane połączenie jest `StartActivity` zawsze, gdy jest działanie użytkownika, zmienianie i **nigdy** połączenie `EndActivity` metody. Ta metoda wysyła wiadomości na serwerze zaangażowania bieżącego użytkownika opuścił aplikacji, a to wpływa na wszystkie dzienniki aplikacji.

##<a name="advanced-reporting"></a>Zaawansowane raportowania

Opcjonalnie, warto zdarzenia określonej aplikacji raportu, błędów i zadania, w tym celu przy użyciu metody znaleźć w innym `EngagementAgent` zajęć. Interfejs API zaangażowania pozwala korzystać ze wszystkich zaawansowanych funkcji zaangażowania firmy.

Aby uzyskać więcej informacji zobacz [jak za pomocą zaawansowanych zaangażowania Mobile znakowania interfejsu API w aplikacji Silverlight Windows Phone](mobile-engagement-windows-phone-use-engagement-api.md).

##<a name="advanced-configuration"></a>Konfiguracja zaawansowana

### <a name="disable-automatic-crash-reporting"></a>Wyłącz automatyczne raportowanie

Możesz wyłączyć automatyczne awarii, funkcja zaangażowania raportowania. Następnie, gdy wystąpi powodu nieobsługiwanego wyjątku, zaangażowania nie żadne dodatkowe czynności.

> [AZURE.WARNING] Jeśli planujesz wyłączyć tę funkcję, należy pamiętać, że po awarii nieobsługiwanego wystąpi w aplikacji, zaangażowania nie będzie wysyłać awarii **i** nie zostanie zamknięte sesji i zadania.

Aby wyłączyć automatyczne raportowania, po prostu dostosować konfiguracji w zależności od sposobu jego deklarowane:

#### <a name="from-engagementconfigurationxml-file"></a>Z `EngagementConfiguration.xml` pliku

Ustaw raport awarii `false` między `<reportCrash>` i `</reportCrash>` znaczniki.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Z `EngagementConfiguration` obiektu w czasie wykonywania

Ustaw raport awarii ma wartość FAŁSZ, przy użyciu obiektu EngagementConfiguration.

        /* Engagement configuration. */

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration(); engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        /\* Disable Engagement crash reporting. \*/ engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a>Tryb serii

Domyślnie raporty usług zaangażowania dzienniki w czasie rzeczywistym. Jeśli aplikacja bardzo często raporty dzienników, lepiej jest buforu dzienniki i przedstawianie zbiorczego na zwykłą podstawy czasu (jest to "tryb serii").

W tym celu należy wywołać metodę:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Argument ma wartość (w **milisekundach)**. W dowolnym momencie Jeśli chcesz ponownie aktywować rejestrowania w czasie rzeczywistym, po prostu wywołać metodę bez parametrów lub wartość 0.

Tryb serii nieco zwiększa czas, ale ma wpływ na monitorze zaangażowania: wszystkie czas trwania sesji i zadania zostanie zaokrąglona do progu serii (w związku z tym sesje i zadań krótsze od progu serii mogą nie być widoczne). Zaleca się używanie próg w serii się niż 30000 (30s). Masz, o których warto pamiętać, że zapisać dzienniki są ograniczone do 300 elementów. W przypadku wysyłania jest zbyt długa może spowodować utratę niektórych dzienników.

> [AZURE.WARNING] Próg serii nie można skonfigurować do okresu mniejsze niż jednej sekundy. Przy próbie w tym celu zestawu SDK zostanie wyświetlona śledzenia z błędu i zostanie automatycznie ustawiany na wartość domyślna, oznacza to, że zero sekund. Spowoduje to SDK raportowania dzienniki w czasie rzeczywistym.
 
