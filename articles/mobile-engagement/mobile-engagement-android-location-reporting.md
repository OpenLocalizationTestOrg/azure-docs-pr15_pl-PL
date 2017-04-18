<properties
    pageTitle="Lokalizacja raportowania dla systemu Android SDK Azure zaangażowania urządzeń przenośnych"
    description="W tym artykule opisano sposób konfigurowania lokalizacji raportowania dla systemu Android SDK zaangażowania Mobile Azure"
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
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

# <a name="location-reporting-for-azure-mobile-engagement-android-sdk"></a>Lokalizacja raportowania dla systemu Android SDK Azure zaangażowania urządzeń przenośnych

> [AZURE.SELECTOR]
- [Android](mobile-engagement-android-integrate-engagement.md)

W tym temacie opisano, jak wykonać lokalizacji raportowania dla systemu Android aplikacji.

## <a name="prerequisites"></a>Wymagania wstępne

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="location-reporting"></a>Raportowanie lokalizacji

Jeśli chcesz lokalizacje, które mają zostać zgłoszone, musisz dodać kilka wierszy konfiguracji (między `<application>` i `</application>` znaczniki).

### <a name="lazy-area-location-reporting"></a>Raportowanie lokalizacji obszaru opóźnieniem

Lokalizacja obszaru opóźnieniem raportowania umożliwia raportowanie kraj, region i miejscowość skojarzone z urządzenia. Ten typ raportowania lokalizacji wykorzystuje tylko lokalizacje sieciowe (na podstawie identyfikatorów komórki lub sieci Wi-Fi). Obszar urządzenie jest zgłaszany co najwyżej raz do sesji. GPS nigdy nie jest używany, a więc raporcie lokalizację tego typu ma wpływ niskiej na baterii.

Obszary zgłoszoną są używane do obliczenia geograficzne statystyk dotyczących użytkowników, sesje, zdarzeń i błędów. Można też nimi jako kryterium w kampanii Reach.

Włącz Lokalizacja obszaru opóźnieniem raportów przy użyciu konfiguracji powyżej w tej procedurze:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Należy określić uprawnienie lokalizacji. Ten kod zawiera ``COARSE`` uprawnień:

    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Jeśli aplikacji wymaga, możesz użyć ``ACCESS_FINE_LOCATION`` zamiast tego.

### <a name="real-time-location-reporting"></a>Raportowanie w czasie rzeczywistym lokalizacji

Raportowanie w czasie rzeczywistym lokalizacji umożliwia raportowanie szerokości i długości geograficznej skojarzone z urządzenia. Domyślnie ten typ raportowania lokalizacji wykorzystuje tylko lokalizacji sieciowych, na podstawie identyfikatorów komórki lub sieci Wi-Fi. Zgłaszanie jest aktywna tylko po uruchomieniu aplikacji na pierwszym planie, (na przykład podczas sesji).

W czasie rzeczywistym lokalizacje są *nie* używana do obliczania statystyk. Ich jedynym celem jest zezwala na użycie w czasie rzeczywistym ogrodzenia geo \<Reach-odbiorców — geofencing\> kryterium w Reach kampanii.

Aby włączyć lokalizacji w czasie rzeczywistym raportowania, dodawanie wiersza kodu do miejsce, w którym Ustawianie parametrów połączenia zaangażowania w działaniu uruchamiania. Wynik wygląda następująco:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

        You also need to specify a location permission. This code uses ``COARSE`` permission:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

        If your app requires it, you can use ``ACCESS_FINE_LOCATION`` instead.

#### <a name="gps-based-reporting"></a>GPS podstawie raportowania

Domyślnie raportowania w czasie rzeczywistym lokalizacji tylko korzysta z lokalizacji sieciowej. Aby włączyć oparte na GPS miejscach, które są bardziej precyzyjne, należy użyć obiektu konfiguracji:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Trzeba również Jeśli brakujące, Dodaj następujące uprawnienia:

    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a>Raportowanie tła

Domyślnie raportowania w czasie rzeczywistym lokalizacji jest aktywne po uruchomieniu aplikacji na pierwszym planie (na przykład podczas sesji). Włączanie funkcji raportowania również, w tle, należy użyć tego obiektu konfiguracji:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [AZURE.NOTE] Po uruchomieniu aplikacji w tle, tylko lokalizacje sieciowe są zgłaszane, nawet jeśli włączona GPS.

Jeśli użytkownik ponownym uruchomieniu urządzenia, raport lokalizacji tło jest zatrzymany. Aby umożliwić automatyczne ponowne uruchomienie w czasie uruchamiania, Dodaj kod.

    <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
           android:exported="false">
        <intent-filter>
            <action android:name="android.intent.action.BOOT_COMPLETED" />
        </intent-filter>
    </receiver>

Trzeba również Jeśli brakujące, Dodaj następujące uprawnienia:

    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

## <a name="android-m-permissions"></a>Android uprawnienia M

Począwszy od Android M, niektóre uprawnienia zarządza się w czasie wykonywania a wymagają zatwierdzenia użytkownika.

Jeśli potencjalni poziom Android interfejsu API 23 uprawnienia środowisko uruchomieniowe zostaną wyłączone domyślnie dla nowych instalacji aplikacji. W przeciwnym razie są włączone domyślnie.

Możesz mogą włączać/wyłączać te uprawnienia w menu Ustawienia urządzenia. Wyłączenie uprawnień z menu systemowego kasuje procesy w tle aplikacji, która jest zachowanie system, a nie ma wpływu na możliwość odbierania wypychanych w tle.

W kontekście lokalizacji zaangażowania Mobile raportowania są uprawnienia, które wymagają zatwierdzenia w czasie wykonywania:

- `ACCESS_COARSE_LOCATION`
- `ACCESS_FINE_LOCATION`

Zażądaj uprawnień użytkownika za pomocą okna dialogowego standardowego systemu. Jeśli użytkownik zaakceptuje, określić ``EngagementAgent`` podjęcie tej zmiany do konta w czasie rzeczywistym. W przeciwnym razie zmiany są przetwarzane przy następnym użytkownik uruchamia aplikację.

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
         * Request location permission, but this doesn't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }
