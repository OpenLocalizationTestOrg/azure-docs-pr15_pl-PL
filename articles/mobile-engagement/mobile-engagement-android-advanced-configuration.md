<properties
    pageTitle="Konfiguracja zaawansowana dla systemu Android SDK zaangażowania Mobile Azure"
    description="W tym artykule opisano opcje konfiguracji zaawansowanej, takie jak Android pojawiają z zestawu SDK systemu Android zaangażowania Mobile Azure"
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
    ms.date="10/04/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-configuration-for-azure-mobile-engagement-android-sdk"></a>Konfiguracja zaawansowana dla systemu Android SDK zaangażowania Mobile Azure

> [AZURE.SELECTOR]
- [Uniwersalny systemu Windows](mobile-engagement-windows-store-advanced-configuration.md)
- [Program Silverlight Windows Phone](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-advanced-configuration.md)

Tej procedurze opisano sposób konfigurowania różne opcje konfiguracji systemu Azure Mobile zaangażowania aplikacje.

## <a name="prerequisites"></a>Wymagania wstępne

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="permission-requirements"></a>Wymagania dotyczące uprawnień
Niektóre opcje wymagają określonych uprawnień, które są wymienione tutaj dla odwołania, a w tekście w określonych funkcji. Dodawanie tych uprawnień do AndroidManifest.xml projektu, bezpośrednio przed lub po `<application>` znacznik.

Kod uprawnień musi wyglądać następująco, gdzie Wypełnij odpowiednie uprawnienia w tabeli poniżej.

    <uses-permission android:name="android.permission.[specific permission]"/>


| Uprawnień | Użycie |
| ---------- | --------- |
| INTERNET | Wymagane. Aby uzyskać podstawowe raportowania |
| ACCESS_NETWORK_STATE | Wymagane. Aby uzyskać podstawowe raportowania |
| RECEIVE_BOOT_COMPLETED | Wymagane. Pokazanie Centrum powiadomienia po ponownym uruchomieniu urządzenia |
| WAKE_LOCK | Zalecane. Umożliwia zbieranie danych przy użyciu sieci Wi-Fi lub gdy ekran jest wyłączony |
| WIBRACJE | Opcjonalnie. Umożliwia wibracji po odebraniu powiadomienia |
| DOWNLOAD_WITHOUT_NOTIFICATION | Opcjonalnie. Włącza powiadamianie Android przeglądu |
| WRITE_EXTERNAL_STORAGE | Opcjonalnie. Włącza powiadamianie Android przeglądu |
| ACCESS_COARSE_LOCATION | Opcjonalnie. Włącza raportowanie lokalizacji w czasie rzeczywistym |
| ACCESS_FINE_LOCATION | Opcjonalnie. Włącza raportowanie lokalizacji opartej na GPS |

Uruchamianie z systemem Android M, [niektóre uprawnienia zarządza się w czasie wykonywania](mobile-engagement-android-location-reporting.md#Android-M-Permissions).

Jeśli już używasz ``ACCESS_FINE_LOCATION``, a następnie nie trzeba również używać ``ACCESS_COARSE_LOCATION``.

## <a name="android-manifest-configuration-options"></a>Opcje konfiguracji manifestu android

### <a name="crash-report"></a>Raport z awarii

Aby wyłączyć raporty z awarii, Dodaj kod między `<application>` i `</application>` znaczników:

    <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a>Próg serii

Domyślnie raporty usług zaangażowania dzienniki w czasie rzeczywistym. Jeśli dzienniki raport aplikacji mogą się różnić często lepiej jest buforu dzienniki i zgłosić je wszystkie naraz na zwykłą podstawy czasu (nazywane "Tryb szybki"). W tym celu należy dodać ten kod między `<application>` i `</application>` znaczników:

    <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

Tryb serii nieco zwiększa baterii, ale ma wpływ na monitorze zaangażowania: wszystkie czas trwania sesji i zadania są zaokrąglane do progu serii (w związku z tym sesje i zadań krótsze od progu serii mogą nie być widoczne). Próg z serii powinny być dłuższa niż 30000 (30s).

### <a name="session-timeout"></a>Limit czasu sesji

 Można zakończyć działanie, naciskając klawisz **Home** lub do **tyłu** , przez ustawienie bezczynne telefonu lub przechodzenie do innej aplikacji. Domyślnie jest zakończenie sesji dziesięć sekund od końca jego ostatniego działania. Pozwala to uniknąć podziału sesji każdym zamknięciu i zwraca do aplikacji szybko, która mogą zdarzyć się to gdy użytkownik wybiera obrazu, sprawdza powiadomienie itp. Być może chcesz zmodyfikować ten parametr. W tym celu należy dodać ten kod między `<application>` i `</application>` znaczników:

    <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

## <a name="disable-log-reporting"></a>Wyłączanie raportowania dziennika

### <a name="using-a-method-call"></a>Przy użyciu metody połączenia

Jeśli chcesz zaangażowania, aby zatrzymać wysyłanie dzienników można zadzwonić do:

    EngagementAgent.getInstance(context).setEnabled(false);

To wywołanie jest trwała: używa pliku udostępnionego preferencji.

Jeśli zaangażowania jest aktywne, gdy połączenie tej funkcji, może potrwać minutę przez usługę zakończyć. Jednak go nie uruchamianie usługi w ogóle przy następnym uruchomieniu aplikacji.

Możesz włączyć ponownie raportowania, dzwoniąc do tej samej funkcji z dziennika `true`.

### <a name="integration-in-your-own-preferenceactivity"></a>Integracja z programem własną`PreferenceActivity`

Zamiast połączenia audio tej funkcji można zintegrować to ustawienie bezpośrednio w istniejącą `PreferenceActivity`.

Zaangażowania umożliwia plik preferencji (z odpowiednim tryb) można konfigurować w `AndroidManifest.xml` pliku z `application meta-data`:

-   `engagement:agent:settings:name` Klucza jest używana do definiowania nazwę pliku udostępnionego Preferencje.
-   `engagement:agent:settings:mode` Klucz służy do definiowanie trybu pliku udostępnionego preferencji. Korzystanie z trybu same jak w sieci `PreferenceActivity`. Tryb muszą być przekazywane jako liczbę: Jeśli używasz kombinację flag stałej w kodzie, sprawdź Całkowita wartość.

Używa zaangażowania zawsze `engagement:key` logiczna klucz w pliku preferencji zarządzania to ustawienie.

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
