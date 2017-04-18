<properties 
    pageTitle="Integracja SDK zaangażowania uniwersalny aplikacje systemu Windows" 
    description="Jak zintegrować z aplikacjami uniwersalny systemu Windows Azure zaangażowania urządzeń przenośnych"                  
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

# <a name="windows-universal-apps-engagement-sdk-integration"></a>Integracja SDK zaangażowania uniwersalny aplikacje systemu Windows

> [AZURE.SELECTOR] 
- [Uniwersalny systemu Windows](mobile-engagement-windows-store-integrate-engagement.md) 
- [Program Silverlight Windows Phone](mobile-engagement-windows-phone-integrate-engagement.md) 
- [iOS](mobile-engagement-ios-integrate-engagement.md) 
- [Android](mobile-engagement-android-integrate-engagement.md) 

W tej procedurze opisano Najprostszym sposobem aktywowania analizy i funkcji w aplikacji systemu Windows uniwersalny monitorowania i zaangażowania.

Poniższe kroki są można aktywować raport dzienniki, aby obliczyć wszystkie statystyki dotyczące użytkowników, sesje, działania, awarii i Technicals. Raport dzienniki potrzebne do obliczania innych statystyk, takich jak zadań, problemów i zdarzeń musi odbywać się ręcznie za pomocą interfejsu API zaangażowania (zobacz [jak za pomocą zaawansowanych zaangażowania Mobile znakowania interfejsu API w aplikacji systemu Windows uniwersalny](mobile-engagement-windows-store-use-engagement-api.md) , ponieważ statystyki te są zależne aplikacji.

## <a name="supported-versions"></a>Obsługiwane wersje

Tylko Mobile zaangażowania SDK dla systemu Windows uniwersalny aplikacje mogą być zintegrowane środowisko uruchomieniowe systemu Windows i aplikacji platformy Windows uniwersalny kierowanie:

-   Windows 8
-   System Windows 8.1
-   Windows Phone 8.1
-   Windows 10 (komputerowych i przenośnych grupy)

> [AZURE.NOTE] Jeśli są kierowanie Silverlight Windows Phone, należy skorzystać z [procedury Integracja z programem Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md).

## <a name="install-the-mobile-engagement-universal-apps-sdk"></a>Zainstaluj zestaw SDK przenośnych zaangażowania uniwersalny aplikacji

### <a name="all-platforms"></a>Wszystkie platformy

Zaangażowania SDK dla systemu Windows uniwersalny aplikacji Mobile jest dostępny w pakiecie Nuget o nazwie *MicrosoftAzure.MobileEngagement*. Możesz zainstalować go za pomocą programu Visual Studio Nuget pakiet menedżera.

### <a name="windows-8x-and-windows-phone-81"></a>Windows 8.x i Windows Phone 8.1

NuGet automatycznie wdraża zasobów SDK w `Resources` folder w katalogu głównym projektu aplikacji.

### <a name="windows-10-universal-windows-platform-applications"></a>Aplikacje systemu Windows 10 uniwersalny platformy systemu Windows

NuGet nie automatycznie wdrożyć zasobów SDK w aplikacji UWP jeszcze. Musisz to zrobić ręcznie do momentu powraca wdrażanie zasobów w NuGet:

1.  Otwórz okno usługi Eksploratora plików.
2.  Przejdź do następującej lokalizacji (**x.x.x** jest wersja zaangażowania instalujesz): *% USERPROFILE %\\.nuget\packages\MicrosoftAzure.MobileEngagement\\**x.x.x**\\content\win81*
3.  Przeciągnij i upuść folderu **zasoby** w Eksploratorze plików w katalogu głównym projektu w programie Visual Studio.
4.  W programie Visual Studio wybierz projektu i aktywować ikonę **Pokaż wszystkie pliki** na bieżąco w **Eksploratorze rozwiązań**.
5.  Niektóre pliki nie są uwzględniane w projekcie. Do zaimportowania ich jednocześnie kliknij prawym przyciskiem myszy folder **zasoby** **wykluczyć z programu project** , a następnie innego kliknij prawym przyciskiem myszy folder **zasoby** , **Uwzględnij w programie project** , aby ponownie dołączyć cały folder. Wszystkie pliki z folderu **zasoby** znajdują się teraz w projekcie.

## <a name="add-the-capabilities"></a>Dodawanie funkcji

SDK zaangażowania musi pewne możliwości SDK systemu Windows, aby działał poprawnie.

Otwieranie swojej `Package.appxmanifest` plik i upewnij się, że zgłaszane są następujące możliwości:

-   `Internet (Client)`

## <a name="initialize-the-engagement-sdk"></a>Inicjowanie zaangażowania SDK

### <a name="engagement-configuration"></a>Konfiguracja zaangażowania

Konfiguracja zaangażowania jest scentralizowane w `Resources\EngagementConfiguration.xml` pliku projektu.

Edytowanie tego pliku, aby określić:

-   Ciąg połączenia aplikacji między znacznikami `<connectionString>` i `<\connectionString>`.

Jeśli chcesz określić je w czasie rzeczywistym, możesz zadzwonić poniższej metody przed inicjowanie agenta zaangażowania:
          
          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set the Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);

Parametry połączenia dla aplikacji zostanie wyświetlona w portalu klasyczny Azure.

### <a name="engagement-initialization"></a>Inicjowanie zaangażowania

Podczas tworzenia nowego projektu, `App.xaml.cs` wygenerowanie pliku. Ta klasa dziedziczy `Application` i zawiera wiele metod ważne. Go będzie również zainicjować SDK zaangażowania.

Modyfikowanie `App.xaml.cs`:

-   Dodawanie do swojego `using` instrukcje:

        using Microsoft.Azure.Engagement;

-   Definiowanie sposobu udostępniania inicjowanie zaangażowania raz dla wszystkich połączeń:

        private void InitEngagement(IActivatedEventArgs e)
        {
          EngagementAgent.Instance.Init(e);
        
          // or
        
          EngagementAgent.Instance.Init(e, engagementConfiguration);
        }
        
-   Połączenie `InitEngagement` w `OnLaunched` metody:

        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
          InitEngagement(e);
        }

-   Po uruchomieniu aplikacji, przy użyciu niestandardowy schemat, innej aplikacji lub wiersza polecenia, a następnie `OnActivated` nosi nazwę metody. Ponadto trzeba zainicjować SDK zaangażowania po aktywowaniu aplikacji. W tym celu należy zastąpić `OnActivated` metody:

        protected override void OnActivated(IActivatedEventArgs args)
        {
          InitEngagement(args);
        }

> [AZURE.IMPORTANT] Firma Microsoft zdecydowanie zapobieżenia go można dodać inicjowanie zaangażowania w innym miejscu aplikacji.

## <a name="basic-reporting"></a>Podstawowe raportowania

### <a name="recommended-method-overload-your-page-classes"></a>Zalecana metoda: przeciążeń usługi `Page` klasy

Aby aktywować raportu wszystkie dzienniki wymagane przez zaangażowania do obliczenia użytkowników, sesje, działania, awarii i technicznych danych statystycznych, po prostu pozwalające wszystkie usługi `Page` klasy podrzędne dziedziczą `EngagementPage` klasy.

Oto przykład jak to zrobić na stronie aplikacji. Możesz zrobić to samo dla wszystkich stron w aplikacji.

#### <a name="c-source-file"></a>Plik źródłowy C#

Modyfikowanie strony `.xaml.cs` pliku:

-   Dodawanie do swojego `using` instrukcje:

        using Microsoft.Azure.Engagement;

-   Zamienianie `Page` z `EngagementPage`:

**Bez zaangażowania:**
    
        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

**Z zaangażowania:**

        using Microsoft.Azure.Engagement;
        
        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [AZURE.IMPORTANT] Jeśli zastępuje stronę `OnNavigatedTo` metodę, pamiętaj nawiązać połączenie `base.OnNavigatedTo(e)`. W przeciwnym razie działanie nie jest informowany ( `EngagementPage` połączeń `StartActivity` wewnątrz jego `OnNavigatedTo` metody).

#### <a name="xaml-file"></a>Plik danych XAML

Modyfikowanie strony `.xaml` pliku:

-   Dodać do swojej deklaracji nazw:

        xmlns:engagement="using:Microsoft.Azure.Engagement"

-   Zamienianie `Page` z `engagement:EngagementPage`:

**Bez zaangażowania:**

        <Page>
            <!-- layout -->
            ...
        </Page>

**Z zaangażowania:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

#### <a name="override-the-default-behaviour"></a>Zastępowanie zachowań domyślnych

Domyślnie nazwa klasy strony jest zgłoszone jako nazwę aktywności, bez dodatkowych. Jeśli klasy używa sufiks "Strona", zaangażowania spowoduje również usunięcie go.

Jeśli chcesz zastąpić zachowanie domyślne dla nazwy, po prostu dodaj to kodu:

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

Jeśli nie można lub nie chcesz, aby przeciążeń usługi `Page` klasy, możesz zamiast tego Rozpocznij Twoich działań, łącząc `EngagementAgent` bezpośrednio metod.

Zalecamy, aby nawiązać połączenie `StartActivity` wewnątrz usługi `OnNavigatedTo` metody strony.

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [AZURE.IMPORTANT]  Upewnij się, że poprawnie zakończyć sesję.
> 
> Uniwersalny zestawu SDK systemu Windows automatycznie połączeń `EndActivity` metody, gdy aplikacja zostanie zamknięta. Dlatego **zdecydowanie** zalecane połączenie jest `StartActivity` zawsze, gdy jest działanie użytkownika, zmienianie i **nigdy** połączenie `EndActivity` metoda ta metoda wysyła do serwera zaangażowania, że bieżący użytkownik ma Pozostaw aplikację, to wpływa na wszystkie dzienniki aplikacji.

## <a name="advanced-reporting"></a>Zaawansowane raportowania

Opcjonalnie, warto zdarzenia określonej aplikacji raportu, błędów i zadania, w tym celu przy użyciu metody znaleźć w innym `EngagementAgent` zajęć. Interfejs API zaangażowania pozwala korzystać ze wszystkich zaawansowanych funkcji zaangażowania firmy.

Aby uzyskać więcej informacji zobacz [jak za pomocą zaawansowanych zaangażowania Mobile znakowania interfejsu API w aplikacji systemu Windows uniwersalny](mobile-engagement-windows-store-use-engagement-api.md).

##<a name="advanced-configuration"></a>Konfiguracja zaawansowana

### <a name="disable-automatic-crash-reporting"></a>Wyłącz automatyczne raportowanie

Możesz wyłączyć automatyczne awarii, funkcja zaangażowania raportowania. Następnie, gdy wystąpi powodu nieobsługiwanego wyjątku, zaangażowania nie żadne dodatkowe czynności.

> [AZURE.WARNING] Jeśli planujesz wyłączyć tę funkcję, należy pamiętać, że po awarii nieobsługiwanego wystąpi w aplikacji, zaangażowania nie wyśle awarii **i** nie zostanie zamknięte sesji i zadania.

Aby wyłączyć automatyczne raportowania, po prostu dostosować konfiguracji w zależności od sposobu jego zadeklarowane:

#### <a name="from-engagementconfigurationxml-file"></a>Z `EngagementConfiguration.xml` pliku

Ustaw raport awarii `false` między `<reportCrash>` i `</reportCrash>` znaczniki.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Z `EngagementConfiguration` obiektu w czasie wykonywania

Ustaw raport awarii ma wartość FAŁSZ, przy użyciu obiektu EngagementConfiguration.

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        
        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a>Tryb serii

Domyślnie raporty usług zaangażowania dzienniki w czasie rzeczywistym. Jeśli aplikacja bardzo często raporty dzienników, lepiej jest buforu dzienniki i przedstawianie zbiorczego na zwykłą podstawy czasu (jest to "tryb serii").

W tym celu należy wywołać metodę:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Argument ma wartość (w **milisekundach)**. W dowolnym momencie Jeśli chcesz ponownie aktywować rejestrowania w czasie rzeczywistym, po prostu wywołać metodę bez parametrów lub wartość 0.

Tryb serii nieco zwiększa czas, ale ma wpływ na monitorze zaangażowania: wszystkie czas trwania sesji i zadania zostanie zaokrąglona do progu serii (w związku z tym sesje i zadań krótsze od progu serii mogą nie być widoczne). Zaleca się używanie próg w serii się niż 30000 (30s). Masz, o których warto pamiętać, że zapisać dzienniki są ograniczone do 300 elementów. W przypadku wysyłania jest zbyt długa może spowodować utratę niektórych dzienników.

> [AZURE.WARNING] Nie można skonfigurować progu serii okresowi mniejsze niż wartości 1. Jeśli spróbujesz to zrobić, zestawu SDK zostaną wyświetlone śledzenia z powodu błędu, a zostanie automatycznie ustawiany na wartość domyślna to znaczy 0s. Spowoduje to SDK raportowania dzienniki w czasie rzeczywistym.

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
 
