<properties
    pageTitle="Integracja Android SDK Azure zaangażowania urządzeń przenośnych"
    description="Najnowszych aktualizacji i procedury dla systemu Android SDK dla zaangażowania Mobile Azure"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-engagement-on-android"></a>Jak zintegrować zaangażowania w systemie Android

> [AZURE.SELECTOR]
- [Uniwersalny systemu Windows](mobile-engagement-windows-store-integrate-engagement.md)
- [Program Silverlight Windows Phone](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-integrate-engagement.md)

W tej procedurze opisano Najprostszym sposobem aktywowania analizy i funkcji w aplikacji Android monitorowania i zaangażowania.

> [AZURE.IMPORTANT] Minimalny poziom interfejsu API zestawu SDK systemu Android musi być 10 lub nowszego (Android 2.3.3 lub nowszy).

Poniższe kroki są tyle uaktywnia raport dzienniki, aby obliczyć wszystkie statystyki dotyczące użytkowników, sesje, działania, awarii i Technicals. Raport dzienniki potrzebne do obliczania innych statystyk, takich jak zadań, problemów i zdarzeń musi odbywać się ręcznie za pomocą interfejsu API zaangażowania (zobacz [jak za pomocą zaawansowanych zaangażowania Mobile znakowania interfejsu API w systemie Android](mobile-engagement-android-use-engagement-api.md) , ponieważ statystyki te są zależne aplikacji.

##<a name="embed-the-engagement-sdk-and-service-into-your-android-project"></a>Osadzanie SDK zaangażowania i usług w projekcie systemu Android

Pobrać Android SDK [tutaj](https://aka.ms/vq9mfn) Get `mobile-engagement-VERSION.jar` i umieszczanie ich w `libs` folder Android projektu (Utwórz folder bibliotekami, jeśli jeszcze nie istnieje).

> [AZURE.IMPORTANT]
> Tworzenie pakietu aplikacji z ProGuard, należy zachować niektóre klasy. Można używać następujących wstawek konfiguracji:
>
>
            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
            <methods>;
            }

Określ ciąg połączenia zaangażowania, dzwoniąc z poniższej metody w działaniu uruchamiania:

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

Parametry połączenia dla aplikacji jest wyświetlany na Azure Portal.

-   Jeśli brakuje, Dodaj następujące uprawnienia Android (przed `<application>` znacznika):

            <uses-permission android:name="android.permission.INTERNET"/>
            <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>

-   Dodaj następującą sekcję (między `<application>` i `</application>` znaczniki):

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

-   Zmienianie `<Your application name>` według nazwy aplikacji.

> [AZURE.TIP] `android:label` Atrybut pozwala wybrać nazwę usługi zaangażowania, jak będzie wyglądał użytkownikom na ekranie telefonu "Z usługami". Zaleca się atrybut na `"<Your application name>Service"` (np. `"AcmeFunGameService"`).

Określanie `android:process` atrybut gwarantuje, że usługa zaangażowania zostanie uruchomiona we własnym procesie (działa zaangażowania w tym samym procesem, jak aplikacja spowoduje, że do wątku głównego i interfejsu użytkownika będzie potencjalnie mniej odpowiada).

> [AZURE.NOTE] Cały kod umieścić w `Application.onCreate()` i innych aplikacji zwrotne, spowoduje uruchomienie procesy wszystkich aplikacji, łącznie z usługą zaangażowania. Może mieć niepożądane skutki strony (na przykład przydziału pamięci niepotrzebne i wątki procesu zaangażowania, zduplikowane odbiorców emisji lub usług).

Jeśli zostanie zmieniona `Application.onCreate()`, zalecamy Dodaj następujący fragment kodu na początku swojej `Application.onCreate()` funkcji:

             public void onCreate()
             {
               if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
                 return;

               ... Your code...
             }

Można wykonywać te same kroki dla `Application.onTerminate()`, `Application.onLowMemory()` i `Application.onConfigurationChanged(...)`.

Można również rozszerzyć `EngagementApplication` zamiast rozszerzenia `Application`: wywołanie zwrotne `Application.onCreate()` czy Sprawdź proces i połączeń `Application.onApplicationProcessCreate()` tylko jeśli bieżący proces nie jest uruchamiana usługa zaangażowania, same reguły są stosowane do innych zwrotne.

##<a name="basic-reporting"></a>Podstawowe raportowania

### <a name="recommended-method-overload-your-activity-classes"></a>Zalecana metoda: przeciążeń usługi `Activity` klasy

Aby aktywować raportu wszystkie dzienniki wymagane przez zaangażowania do obliczenia użytkowników, sesje, działania, awarii i technicznych danych statystycznych, wystarczy wyświetlanie wszystkich usługi `*Activity` klasy podrzędne dziedziczą odpowiednie `Engagement*Activity` klas (np. Jeśli starsze aktywności `ListActivity`, tworzącej rozszerza `EngagementListActivity`).

**Bez zaangażowania:**

            package com.company.myapp;

            import android.app.Activity;
            import android.os.Bundle;

            public class MyApp extends Activity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

**Z zaangażowania:**

            package com.company.myapp;

            import com.microsoft.azure.engagement.activity.EngagementActivity;
            import android.os.Bundle;

            public class MyApp extends EngagementActivity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

> [AZURE.IMPORTANT] Podczas korzystania z `EngagementListActivity` lub `EngagementExpandableListActivity`, upewnij się, że dowolny wywołanie `requestWindowFeature(...);` nastąpi przed połączenie `super.onCreate(...);`, w przeciwnym razie wystąpienia awarii.

Możesz znaleźć te klasy w `src` folder i można skopiować je do projektu. Klasy są również **JavaDoc**.

### <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Alternatywne metody: połączenie `startActivity()` i `endActivity()` ręcznie

Jeśli nie można lub nie chcesz, aby przeciążeń usługi `Activity` klasy, możesz zamiast tego rozpoczęcia i zakończenia działań, dzwoniąc `EngagementAgent`w metod bezpośrednio.

> [AZURE.IMPORTANT] Android SDK nigdy nie wywołuje `endActivity()` metody, nawet wtedy, gdy aplikacja zostanie zamknięta (w systemie Android, aplikacje są faktyczną nigdy nie zamknięte). Dlatego *zdecydowanie* zalecane połączenie jest `startActivity()` metoda `onResume` zwrotnego *wszystko* Twoich działań oraz `endActivity()` metody w `onPause()` zwrotnego *wszystko* działań. To jest jedynym sposobem upewnij się, że sesje nie będzie przecieku. Jeśli sesji jest przecieku, usługę zaangażowania będą nigdy nie odłączyć od zaangażowania wewnętrznej bazy danych (ponieważ usługa połączono jak oczekuje sesji).

Oto przykład:

            public class MyActivity extends Some3rdPartyActivity
            {
              @Override
              protected void onResume()
              {
                super.onResume();
                String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at the end.
                EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
              }

              @Override
              protected void onPause()
              {
                super.onPause();
                EngagementAgent.getInstance(this).endActivity();
              }
            }

W tym przykładzie bardzo podobne do `EngagementActivity` zajęć i jego odmiany, których kod źródłowy znajduje się w `src` folder.

##<a name="test"></a>Test

Teraz Sprawdź, czy usługi integracji uruchamiając usługi aplikacji dla urządzeń przenośnych w emulatora lub urządzenia i sprawdzanie rejestruje sesji na karcie Monitor.

Kolejne sekcje są opcjonalne.

##<a name="location-reporting"></a>Raportowanie lokalizacji

Jeśli chcesz lokalizacje, które mają zostać zgłoszone, musisz dodać kilka wierszy konfiguracji (między `<application>` i `</application>` znaczniki).

### <a name="lazy-area-location-reporting"></a>Raportowanie lokalizacji obszaru opóźnieniem

Raportowanie lokalizacji obszaru opóźnieniem umożliwia raportowania kraju, regiony i miejscowości skojarzony z urządzenia. Ten typ raportowania lokalizacji wykorzystuje tylko lokalizacje sieciowe (na podstawie identyfikatorów komórki lub sieci Wi-Fi). Obszar urządzenie jest zgłaszany co najwyżej raz na sesję. GPS nigdy nie jest używany, a więc ten typ raportu w lokalizacji ma bardzo mało (Aby na przykład nie) wpływ na baterii.

Obszary zgłoszoną są używane do obliczenia geograficzne statystyk dotyczących użytkowników, sesje, zdarzeń i błędów. Można też nimi jako kryterium w kampanii Reach.

Włączanie funkcji raportowania lokalizacji obszaru opóźnieniem, możesz wykonać je przy użyciu konfiguracji powyżej w tej procedurze:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Trzeba również Jeśli brakujące, Dodaj następujące uprawnienia:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Lub możesz korzystać ``ACCESS_FINE_LOCATION`` Jeśli już używasz w aplikacji.

### <a name="real-time-location-reporting"></a>Raportowanie lokalizacji w czasie rzeczywistym

Raportowanie lokalizacji w czasie rzeczywistym umożliwia raportowania szerokości i długości geograficznej skojarzony z urządzenia. Domyślnie ten typ raportowania lokalizacji korzysta tylko lokalizacje sieciowe (na podstawie identyfikatorów komórki lub sieci Wi-Fi), a raportowania jest aktywne po uruchomieniu aplikacji na pierwszym planie (to znaczy podczas sesji).

Lokalizacje w czasie rzeczywistym są *nie* używana do obliczania statystyk. Ich jedynym celem jest zezwala na użycie czasie rzeczywistym geo ogrodzenia \<Reach-odbiorców — geofencing\> kryterium w Reach kampanii.

Aby włączyć raportowanie miejsca w czasie rzeczywistym, możesz wykonać je przy użyciu konfiguracji powyżej w tej procedurze:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Trzeba również Jeśli brakujące, Dodaj następujące uprawnienia:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Lub możesz korzystać ``ACCESS_FINE_LOCATION`` Jeśli już używasz w aplikacji.

#### <a name="gps-based-reporting"></a>GPS podstawie raportowania

Domyślnie raportowania lokalizacji w czasie rzeczywistym używa tylko lokalizacje sieciowe. Aby włączyć funkcję lokalizacje GPS podstawie, (które są bardziej precyzyjne), należy użyć obiektu konfiguracji:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Trzeba również Jeśli brakujące, Dodaj następujące uprawnienia:

            <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a>Raportowanie tła

Domyślnie raportowania lokalizacji w czasie rzeczywistym jest aktywne po uruchomieniu aplikacji na pierwszym planie (to znaczy podczas sesji). Włączanie funkcji raportowania również, w tle, należy użyć obiektu konfiguracji:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [AZURE.NOTE] Gdy aplikacja działa w tle, tylko sieć podstawie lokalizacje są zgłaszane, nawet jeśli włączona GPS.

Raport lokalizacji tła zostanie zatrzymany, jeśli użytkownik ponownym uruchomieniu urządzenia, możesz dodać tę opcję, aby był automatycznie uruchom ponownie w czasie uruchamiania:

            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>

Trzeba również Jeśli brakujące, Dodaj następujące uprawnienia:

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

### <a name="android-m-permissions"></a>Android uprawnienia M

Począwszy od Android M, niektóre uprawnienia są zarządzane w czasie rzeczywistym i wymaga zatwierdzenia użytkownika.

Uprawnienia środowisko uruchomieniowe zostanie wyłączona domyślnie dla nowych instalacji aplikacji, jeśli docelowy poziom Android interfejsu API 23. W przeciwnym razie go będzie jest domyślnie włączona.

Użytkownik mogą włączać/wyłączać te uprawnienia w menu Ustawienia urządzenia. Wyłączenie uprawnień z menu systemowego kasuje procesy w tle aplikacji, to jest zachowanie systemu i nie ma wpływu na możliwość odbierania wypychanych w tle.

W kontekście zaangażowania Mobile uprawnienia, które wymagają zatwierdzenia w czasie wykonywania są:

- `ACCESS_COARSE_LOCATION`
- `ACCESS_FINE_LOCATION`
- `WRITE_EXTERNAL_STORAGE`(tylko wtedy, gdy kierowanie Android interfejsu API poziom 23 tej)

Zewnętrzne przestrzeni dyskowej są używane tylko do funkcji przeglądu Reach. Jeśli znajdziesz pytaniem użytkowników to uprawnienie być kłopotliwe środki, można je usunąć Jeśli używany tylko w przypadku zaangażowania Mobile, ale kosztem wyłączanie funkcji przeglądu.

W przypadku funkcji lokalizacji należy zażądaj uprawnień użytkownika za pomocą okna dialogowego standardowego systemu. Jeśli użytkownik zatwierdzających, należy sprawdzić ``EngagementAgent`` robienia tej zmiany do konta w czasie rzeczywistym (w przeciwnym razie zmiany zostaną przetworzone przy następnym użytkownik uruchamia aplikację).

Poniżej przedstawiono przykładowy kod za pomocą w działaniu aplikacji zażądaj uprawnień i przesyła wynik, jeśli jest dodatni, aby ``EngagementAgent``:

    @Override
    public void onCreate(Bundle savedInstanceState)
    {
      /* Other code... */

      /* Request permissions */
      requestPermissions();
    }

    @TargetApi(Build.VERSION_CODES.M)
    private void requestPermissions()
    {
      /* Avoid crashing if not on Android M */
      if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M)
      {
        /*
         * Request location permission, but this won't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

        /* Only if you want to keep features using external storage */
        if (checkSelfPermission(android.Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.WRITE_EXTERNAL_STORAGE }, 1);
      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }

##<a name="advanced-reporting"></a>Zaawansowane raportowania

Opcjonalnie, jeśli chcesz zgłosić zdarzenia określonej aplikacji, błędów i zadania, należy użyć interfejsu API zaangażowania za pomocą metody `EngagementAgent` zajęć. Obiekt tej klasy może być pobranych, dzwoniąc `EngagementAgent.getInstance()` metody statycznej.

Interfejs API zaangażowania pozwala korzystać ze wszystkich zaawansowanych funkcji zaangażowania firmy i szczegółowo w jak korzystać z interfejsu API zaangażowania w systemie Android (oraz w dokumentacji technicznej `EngagementAgent` klasy).

##<a name="advanced-configuration-in-androidmanifestxml"></a>Konfiguracja zaawansowana (w AndroidManifest.xml)

### <a name="wake-locks"></a>Wznawianie blokady

Jeśli chcesz upewnij się, że statystyki są wysyłane w czasie rzeczywistym przy użyciu sieci Wi-Fi lub ekranu jest wyłączona, Dodaj następujące opcjonalne uprawnień:

            <uses-permission android:name="android.permission.WAKE_LOCK"/>

### <a name="crash-report"></a>Raport z awarii

Jeśli chcesz wyłączyć raporty z awarii, Dodaj ten (między `<application>` i `</application>` znaczniki):

            <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a>Próg serii

Domyślnie raporty usług zaangażowania dzienniki w czasie rzeczywistym. Jeśli aplikacja bardzo często raporty dzienników, lepiej jest buforu dzienniki i przedstawianie zbiorczego na zwykłą podstawy czasu (jest to "tryb serii"). W tym celu należy dodać (między `<application>` i `</application>` znaczniki):

            <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

Tryb serii nieco zwiększa czas, ale ma wpływ na monitorze zaangażowania: wszystkie czas trwania sesji i zadania zostanie zaokrąglona do progu serii (w związku z tym sesje i zadań krótsze od progu serii mogą nie być widoczne). Zaleca się używanie próg w serii się niż 30000 (30s).

### <a name="session-timeout"></a>Limit czasu sesji

Domyślnie sesja jest 10s zakończone po zakończeniu jego ostatniego działania (który zwykle występuje, naciskając klawisz Home lub Wstecz, przez ustawienie bezczynne przez telefon lub przechodzenie do innej aplikacji). To, aby uniknąć podziału sesji każdej godziny zakończenia użytkownika i powrócić do aplikacji bardzo szybko (który mogą zdarzyć się to przed on wybierz obraz, sprawdź powiadomienie itp.). Być może chcesz zmodyfikować ten parametr. W tym celu należy dodać (między `<application>` i `</application>` znaczniki):

            <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

##<a name="disable-log-reporting"></a>Wyłączanie raportowania dziennika

### <a name="using-a-method-call"></a>Przy użyciu metody połączenia

Jeśli chcesz zaangażowania, aby zatrzymać wysyłanie dzienników można zadzwonić do:

            EngagementAgent.getInstance(context).setEnabled(false);

To wywołanie jest trwała: używa pliku udostępnionego preferencji.

Jeśli zaangażowania jest aktywne, gdy połączenie tej funkcji, może potrwać minutę przez usługę zakończyć. Jednak go nie uruchamianie usługi w ogóle przy następnym uruchomieniu aplikacji.

Możesz włączyć ponownie raportowania, dzwoniąc do tej samej funkcji z dziennika `true`.

### <a name="integration-in-your-own-preferenceactivity"></a>Integracja z własną`PreferenceActivity`

Zamiast połączenia audio tej funkcji można zintegrować to ustawienie bezpośrednio w istniejącą `PreferenceActivity`.

Zaangażowania umożliwia plik preferencji (z odpowiednim tryb) można konfigurować w `AndroidManifest.xml` pliku z `application meta-data`:

-   `engagement:agent:settings:name` Klucza jest używana do definiowania nazwę pliku udostępnionego Preferencje.
-   `engagement:agent:settings:mode` Klucz służy do definiowanie trybu pliku udostępnionego preferencji, należy używać tego samego trybu jak w sieci `PreferenceActivity`. Tryb muszą być przekazywane jako liczbę: Jeśli używasz kombinację flag stałej w kodzie, sprawdź Całkowita wartość.

Używanie zaangażowania zawsze `engagement:key` logiczna klucz w pliku preferencji zarządzania to ustawienie.

Poniższy przykład `AndroidManifest.xml` zawiera wartości domyślnej:

            <application>
                [...]
                <meta-data
                  android:name="engagement:agent:settings:name"
                  android:value="engagement.agent" />
                <meta-data
                  android:name="engagement:agent:settings:mode"
                  android:value="0" />

Następnie możesz dodać `CheckBoxPreference` w układzie preferencji podobny do następującego:

            <CheckBoxPreference
              android:key="engagement:enabled"
              android:defaultValue="true"
              android:title="Use Engagement"
              android:summaryOn="Engagement is enabled."
              android:summaryOff="Engagement is disabled." />

<!-- URLs. -->
[Device API]: http://go.microsoft.com/?linkid=9876094
