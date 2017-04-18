<properties
    pageTitle="Zaawansowane zgłoszenie zaangażowania MobileApps uniwersalnego systemu Windows"
    description="Jak zintegrować z aplikacjami uniwersalny systemu Windows Azure zaangażowania urządzeń przenośnych"                  
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-reporting-with-the-windows-universal-apps-engagement-sdk"></a>Zaawansowane zgłoszenie zaangażowania uniwersalny aplikacje systemu Windows SDK

> [AZURE.SELECTOR]
- [Uniwersalny systemu Windows](mobile-engagement-windows-store-advanced-reporting.md)
- [Program Silverlight Windows Phone](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-advanced-reporting.md)

W tym temacie opisano dodatkowe scenariusze raportowania w aplikacji systemu Windows uniwersalny. Scenariusze te obejmują opcje, które mogą być dotyczą aplikacji utworzonych w samouczku [Wprowadzenie](mobile-engagement-windows-store-dotnet-get-started.md) .

## <a name="prerequisites"></a>Wymagania wstępne

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

Przed rozpoczęciem tego samouczka, należy wykonać samouczek [Wprowadzenie](mobile-engagement-windows-store-dotnet-get-started.md) , celowo bezpośredni i proste. Ten samouczek obejmuje dodatkowe opcje, które można wybierać spośród.

## <a name="specifying-engagement-configuration-at-runtime"></a>Określanie konfiguracji zaangażowania w czasie rzeczywistym

Konfiguracja zaangażowania jest scentralizowane w `Resources\EngagementConfiguration.xml` pliku projektu, czyli miejsce, w którym określono w temacie [Wprowadzenie](mobile-engagement-windows-store-dotnet-get-started.md) .

Ale można ją też określić w czasie wykonywania: może wywołać następującej metody przed inicjowanie agenta zaangażowania:

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set the Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);



## <a name="recommended-method-overload-your-page-classes"></a>Zalecana metoda: przeciążeń usługi `Page` klasy

Aby aktywować raportowania wszystkie dzienniki wymagane przez zaangażowania do obliczenia użytkowników, sesje, działania, awarii i technicznych danych statystycznych, wyświetlanie wszystkich usługi `Page` klasy podrzędne dziedziczą `EngagementPage` klasy.

Poniżej przedstawiono przykład strony aplikacji. Możesz zrobić to samo dla wszystkich stron w aplikacji.

### <a name="c-source-file"></a>Plik źródłowy C#

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

> [AZURE.IMPORTANT] Jeśli zastępuje stronę `OnNavigatedTo` metodę, pamiętaj nawiązać połączenie `base.OnNavigatedTo(e)`. W przeciwnym razie nie jest należy podać działania ( `EngagementPage` połączeń `StartActivity` wewnątrz jego `OnNavigatedTo` metody).

### <a name="xaml-file"></a>Plik danych XAML

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

### <a name="override-the-default-behaviour"></a>Zastępowanie zachowań domyślnych

Domyślnie nazwa klasy strony jest zgłoszone jako nazwę aktywności, bez dodatkowych. Jeśli klasy używa sufiks "Strona", zaangażowania usuwane.

Aby zastąpić domyślne zachowanie nazwy, Dodaj kod:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

Aby zgłosić dodatkowe informacje o swojej aktywności, Dodaj kod:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

Metody te są nazywane z poziomu `OnNavigatedTo` metody strony.

### <a name="alternate-method-call-startactivity-manually"></a>Alternatywne metody: połączenie `StartActivity()` ręcznie

Jeśli nie można lub nie chcesz, aby przeciążeń usługi `Page` klasy, możesz zamiast tego Rozpocznij Twoich działań, łącząc `EngagementAgent` bezpośrednio metod.

Zalecamy dzwonisz `StartActivity` wewnątrz usługi `OnNavigatedTo` metody strony.

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [AZURE.IMPORTANT]  Upewnij się, że poprawnie zakończyć sesję.
>
> Uniwersalny zestawu SDK systemu Windows automatycznie połączeń `EndActivity` metody, gdy aplikacja zostanie zamknięta. Dlatego **zdecydowanie** zalecane połączenie jest `StartActivity` zawsze, gdy jest działanie użytkownika, zmienianie i **nigdy** połączenie `EndActivity` metody. Ta metoda powiadamia serwer zaangażowania, że bieżącego użytkownika opuścił aplikacji, która będzie mieć wpływ na wszystkie dzienniki aplikacji.

## <a name="advanced-reporting"></a>Zaawansowane raportowania

Opcjonalnie możesz zechcieć raportowanie zdarzeń specyficzne dla aplikacji, błędy i zadania, aby to zrobić za pomocą metody znaleźć w innym `EngagementAgent` zajęć. Interfejs API zaangażowania umożliwia korzystanie z zaawansowanych funkcji wszystkich zaangażowania.

Aby uzyskać więcej informacji zobacz [jak za pomocą zaawansowanych zaangażowania Mobile znakowania interfejsu API w aplikacji systemu Windows uniwersalny](mobile-engagement-windows-store-use-engagement-api.md).
