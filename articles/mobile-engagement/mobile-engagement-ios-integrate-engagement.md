<properties
    pageTitle="Azure iOS zaangażowania Mobile integracji SDK | Microsoft Azure"
    description="Najnowszych aktualizacji i procedury dla systemu iOS SDK dla zaangażowania Mobile Azure"
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
    ms.date="09/14/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-engagement-on-ios"></a>Jak zintegrować zaangażowania systemie iOS

> [AZURE.SELECTOR]
- [Uniwersalny systemu Windows](mobile-engagement-windows-store-integrate-engagement.md)
- [Program Silverlight Windows Phone](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-integrate-engagement.md)

W tej procedurze opisano Najprostszym sposobem aktywowania analizy i funkcji w aplikacji systemu iOS monitorowania i zaangażowania.

SDK zaangażowania wymaga iOS6 + i Xcode 8: docelowej wdrażania aplikacji musi mieć co najmniej iOS 6.

> [AZURE.NOTE]
> Jeśli naprawdę są zależne od XCode 7 można użyć [iOS v3.2.4 SDK zaangażowania](https://aka.ms/r6oouh). Jest znane błąd w module Reach tej poprzedniej wersji, podczas pracy na urządzeniach z systemem iOS 10 zobacz [integracji modułów reach](mobile-engagement-ios-integrate-engagement-reach.md) uzyskać więcej szczegółowych informacji. Jeśli chcesz użyć SDK v3.2.4, a następnie po prostu pominąć `UserNotifications.framework` importowanie w następnym kroku.

Poniższe kroki są można aktywować raport dzienniki, aby obliczyć wszystkie statystyki dotyczące użytkowników, sesje, działania, awarii i Technicals. Raport dzienniki potrzebne do obliczania innych statystyk, takich jak zadań, problemów i zdarzeń musi odbywać się ręcznie za pomocą interfejsu API zaangażowania (zobacz [jak za pomocą zaawansowanych zaangażowania Mobile znakowania interfejsu API w aplikacji systemu iOS](mobile-engagement-ios-use-engagement-api.md) , ponieważ statystyki te są zależne aplikacji.

##<a name="embed-the-engagement-sdk-into-your-ios-project"></a>Osadzanie SDK zaangażowania w projekcie systemu iOS

- Pobierz iOS SDK [tutaj](http://aka.ms/qk2rnj).

- Dodawanie SDK zaangażowania do projektu systemu iOS: Xcode, kliknij prawym przyciskiem myszy nad projektem i zaznacz **"Dodaj pliki, aby..."** i wybierz polecenie `EngagementSDK` folder.

- Zaangażowania wymaga dodatkowych RAM pracy: w programie project explorer Otwórz okienko programu project i wybierz poprawny element docelowy. Następnie otwórz kartę **"Utworzenie fazy"** i w menu **"binarny z bibliotekami"** Dodaj tych RAM:

    -   `UserNotifications.framework`-łącze jako`Optional`
    -   `AdSupport.framework`-łącze jako`Optional`
    -   `SystemConfiguration.framework`
    -   `CoreTelephony.framework`
    -   `CFNetwork.framework`
    -   `CoreLocation.framework`
    -   `libxml2.dylib`

> [AZURE.NOTE] Struktura AdSupport może być usuwany. Zaangażowania musi tej struktury na potrzeby zbierania IDFA. Jednak może zostać wyłączona zbioru IDFA \<ios — sdk zaangażowania idfa\> do wykonania nowych zasad firmy Apple dotyczące tego identyfikatora.

##<a name="initialize-the-engagement-sdk"></a>Inicjowanie zaangażowania SDK

Należy zmodyfikować pełnomocnik aplikacji:

-   W górnej części pliku implementacji importowanie agenta zaangażowania:

        [...]
        #import "EngagementAgent.h"

-   Inicjowanie zaangażowania wewnątrz metody "**applicationDidFinishLaunching:**"lub"**aplikacji: didFinishLaunchingWithOptions:**":

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
          [...]
          [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
          [...]
        }

##<a name="basic-reporting"></a>Podstawowe raportowania

### <a name="recommended-method-overload-your-uiviewcontroller-classes"></a>Zalecana metoda: przeciążeń usługi `UIViewController` klasy

Aby aktywować raportu wszystkie dzienniki wymagane przez zaangażowania do obliczenia użytkowników, sesje, działania, awarii i technicznych danych statystycznych, po prostu pozwalające wszystkie usługi `UIViewController` klasy podrzędne dziedziczą `EngagementViewController` klasy (takie same reguły dla `UITableViewController`  - \> `EngagementTableViewController`).

**Bez zaangażowania:**

    #import <UIKit/UIKit.h>

    @interface Tab1ViewController : UIViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

**Z zaangażowania:**

    #import <UIKit/UIKit.h>
    #import "EngagementViewController.h"

    @interface Tab1ViewController : EngagementViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

### <a name="alternate-method-call-startactivity-manually"></a>Alternatywne metody: połączenie `startActivity()` ręcznie

Jeśli nie można lub nie chcesz, aby przeciążeń usługi `UIViewController` klasy, możesz zamiast tego Rozpocznij Twoich działań, łącząc `EngagementAgent`osoby metod bezpośrednio.

> [AZURE.IMPORTANT] IOS SDK automatycznie połączeń `endActivity()` metody, gdy aplikacja zostanie zamknięta. Dlatego *zdecydowanie* zalecane połączenie jest `startActivity` zawsze, gdy jest działanie użytkownika, zmienianie i *nigdy* połączenie `endActivity` metody, ponieważ wywołanie tej metody wymusza bieżącej sesji do zakończenia.

##<a name="location-reporting"></a>Raportowanie lokalizacji

Apple warunki użytkowania usługi aplikacji do użytku śledzenie tylko w celu statystyki lokalizacji nie jest możliwe. W związku z tym zaleca się włączenie raportów lokalizacji tylko wtedy, gdy aplikacja także używać śledzenia z innego powodu lokalizacji.

Począwszy od iOS 8, należy podać opis aplikacji używaniu usługi lokalizacji, ustawiając ciągu dla klucza [NSLocationWhenInUseUsageDescription] lub [NSLocationAlwaysUsageDescription] w swojej aplikacji Info.plist pliku. Jeśli chcesz lokalizacji raportu w tle z zaangażowania Dodaj klucz NSLocationAlwaysUsageDescription. We wszystkich innych przypadkach Dodaj klucz NSLocationWhenInUseUsageDescription.

### <a name="lazy-area-location-reporting"></a>Raportowanie lokalizacji obszaru opóźnieniem

Raportowanie lokalizacji obszaru opóźnieniem umożliwia raportowania kraju, regiony i miejscowości skojarzony z urządzenia. Ten typ raportowania lokalizacji wykorzystuje tylko lokalizacje sieciowe (na podstawie identyfikatorów komórki lub sieci Wi-Fi). Obszar urządzenie jest zgłaszany co najwyżej raz do sesji. GPS nigdy nie jest używany, a więc ten typ raportu w lokalizacji ma bardzo mało (Aby na przykład nie) wpływ na baterii.

Obszary zgłoszoną są używane do obliczenia geograficzne statystyk dotyczących użytkowników, sesje, zdarzeń i błędów. Można też nimi jako kryterium w kampanii Reach. Znane obszaru ostatniej zgłoszone dla urządzenia mogą być pobierane dzięki [Interfejsu API urządzenia].

Włączanie funkcji raportowania lokalizacji obszaru opóźnieniem, Dodaj następujący wiersz po inicjowania agenta zaangażowania:

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
      [...]
      [[EngagementAgent shared] setLazyAreaLocationReport:YES];
      [...]
    }

### <a name="real-time-location-reporting"></a>Raportowanie lokalizacji w czasie rzeczywistym

Raportowanie lokalizacji w czasie rzeczywistym umożliwia raportowania szerokości i długości geograficznej skojarzony z urządzenia. Domyślnie ten typ raportowania lokalizacji korzysta tylko lokalizacje sieciowe (na podstawie identyfikatorów komórki lub sieci Wi-Fi), a raportowania jest aktywne po uruchomieniu aplikacji na pierwszym planie (to znaczy podczas sesji).

Lokalizacje w czasie rzeczywistym są *nie* używana do obliczania statystyk. Ich jedynym celem jest zezwala na użycie czasie rzeczywistym geo ogrodzenia \<Reach-odbiorców — geofencing\> kryterium w Reach kampanii.

Włączanie funkcji raportowania lokalizacji w czasie rzeczywistym, należy dodać następujący wiersz po inicjowania agenta zaangażowania:

    [[EngagementAgent shared] setRealtimeLocationReport:YES];

#### <a name="gps-based-reporting"></a>GPS podstawie raportowania

Domyślnie raportowania lokalizacji w czasie rzeczywistym używa tylko lokalizacje sieciowe. Aby włączyć funkcję lokalizacje GPS podstawie, (które są bardziej precyzyjne), należy dodać:

    [[EngagementAgent shared] setFineRealtimeLocationReport:YES];

#### <a name="background-reporting"></a>Raportowanie tła

Domyślnie raportowania lokalizacji w czasie rzeczywistym jest aktywne po uruchomieniu aplikacji na pierwszym planie (to znaczy podczas sesji). Włączanie funkcji raportowania również, w tle, należy dodać:

    [[EngagementAgent shared] setBackgroundRealtimeLocationReport:YES withLaunchOptions:launchOptions];

> [AZURE.NOTE] Gdy aplikacja działa w tle, tylko sieć podstawie lokalizacje są zgłaszane, nawet jeśli włączona GPS.

Wykonanie tej funkcji nawiąże połączenie [startMonitoringSignificantLocationChanges] , gdy aplikacja przechodzi do tła. Należy pamiętać, że go będzie automatycznie uruchom ponownie aplikację na tle Jeśli nowe zdarzenie lokalizacji.

##<a name="advanced-reporting"></a>Zaawansowane raportowania

Opcjonalnie, jeśli chcesz zgłosić zdarzenia określonej aplikacji, błędów i zadania, należy użyć interfejsu API zaangażowania za pomocą metody `EngagementAgent` zajęć. Obiekt tej klasy mogą być pobierane, dzwoniąc `[EngagementAgent shared]` metody statycznej.

Interfejs API zaangażowania pozwala korzystać ze wszystkich zaawansowanych funkcji zaangażowania firmy i szczegółowo w jak korzystać z interfejsu API zaangażowania na iOS (oraz w dokumentacji technicznej `EngagementAgent` klasy).

##<a name="disable-idfa-collection"></a>Wyłączanie zbioru IDFA

Domyślnie zaangażowania przy użyciu [IDFA] jednoznacznie identyfikować użytkownika. Jednak jeśli nie korzystasz z reklam w innym miejscu w aplikacji, mogą zostać odrzucone przez proces recenzji sklep. Zbiór IDFA może zostać wyłączona przez dodawanie preprocesora makra `ENGAGEMENT_DISABLE_IDFA` w pliku pch (lub w `Build Settings` aplikacji). Temu będziesz mieć pewność, że jest nie odwołania do `ASIdentifierManager`, `advertisingIdentifier` lub `isAdvertisingTrackingEnabled` w oknie Tworzenie aplikacji.

Integracja z pliku **prefix.pch** :

    #define ENGAGEMENT_DISABLE_IDFA
    ...

Można sprawdzić, czy zbieranie IDFA poprawnie jest wyłączone w aplikacji, zaznaczając pole wyboru dzienniki test zaangażowania. Zobacz testowego integracji\<ios — sdk zaangażowania test-idfa\> dokumentacji, aby uzyskać więcej informacji.

##<a name="disable-log-reporting"></a>Wyłączanie raportowania dziennika

### <a name="using-a-method-call"></a>Przy użyciu metody połączenia

Jeśli chcesz zaangażowania, aby zatrzymać wysyłanie dzienników można zadzwonić do:

    [[EngagementAgent shared] setEnabled:NO];

To wywołanie jest trwała: używa `NSUserDefaults` do przechowywania informacji.

Możesz włączyć ponownie raportowania, dzwoniąc do tej samej funkcji z dziennika `YES`.

### <a name="integration-in-your-settings-bundle"></a>Integracja z programem w pakiecie do ustawień

Zamiast połączenia audio tej funkcji można zintegrować to ustawienie bezpośrednio w istniejącą `Settings.bundle` pliku. Ciąg `engagement_agent_enabled` musi być używany jako identyfikator preferencji i muszą być skojarzone z przełącznikiem przełącznik (`PSToggleSwitchSpecifier`).

Poniższy przykład `Settings.bundle` pokazano, jak wdrażać go:

    <dict>
        <key>PreferenceSpecifiers</key>
        <array>
            <dict>
                <key>DefaultValue</key>
                <true/>
                <key>Key</key>
                <string>engagement_agent_enabled</string>
                <key>Title</key>
                <string>Log reporting enabled</string>
                <key>Type</key>
                <string>PSToggleSwitchSpecifier</string>
            </dict>
        </array>
        <key>StringsTable</key>
        <string>Root</string>
    </dict>

<!-- URLs. -->
[Urządzenie interfejsu API]: http://go.microsoft.com/?linkid=9876094
[NSLocationWhenInUseUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26
[NSLocationAlwaysUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18
[startMonitoringSignificantLocationChanges]:http://developer.apple.com/library/IOs/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html#//apple_ref/occ/instm/CLLocationManager/startMonitoringSignificantLocationChanges
[IDFA]:https://developer.apple.com/library/ios/documentation/AdSupport/Reference/ASIdentifierManager_Ref/ASIdentifierManager.html#//apple_ref/occ/instp/ASIdentifierManager/advertisingIdentifier
