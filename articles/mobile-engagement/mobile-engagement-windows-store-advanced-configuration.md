<properties
    pageTitle="Zaawansowana konfiguracja dla systemu Windows aplikacje uniwersalny zaangażowania SDK"
    description="Zaawansowane opcje konfiguracji Azure zatrudnienie uniwersalny aplikacje systemu Windows Mobile"                    
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
    ms.date="10/04/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-configuration-for-windows-universal-apps-engagement-sdk"></a>Zaawansowana konfiguracja dla systemu Windows aplikacje uniwersalny zaangażowania SDK

> [AZURE.SELECTOR]
- [Uniwersalny systemu Windows](mobile-engagement-windows-store-advanced-configuration.md)
- [Program Silverlight Windows Phone](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-advanced-configuration.md)

Tej procedurze opisano sposób konfigurowania różne opcje konfiguracji systemu Azure Mobile zaangażowania aplikacje.

## <a name="prerequisites"></a>Wymagania wstępne

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

##<a name="advanced-configuration"></a>Konfiguracja zaawansowana

### <a name="disable-automatic-crash-reporting"></a>Wyłącz automatyczne raportowanie

Możesz wyłączyć automatyczne awarii, funkcja zaangażowania raportowania. Następnie gdy wystąpi powodu nieobsługiwanego wyjątku, zaangażowania nie działają.

> [AZURE.WARNING] Jeśli możesz wyłączyć tę funkcję, następnie wystąpieniu nieobsługiwanego awarii w aplikacji, zaangażowania nie send awarii **i** nie zostanie zamknięty sesji i zadania.

Aby wyłączyć automatyczne raportowania, Dostosuj konfigurację w zależności od sposobu jego zadeklarowane:

#### <a name="from-engagementconfigurationxml-file"></a>Z `EngagementConfiguration.xml` pliku

Ustaw raport awarii `false` między `<reportCrash>` i `</reportCrash>` znaczniki.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Z `EngagementConfiguration` obiektu w czasie wykonywania

Ustaw raport awarii ma wartość FAŁSZ, przy użyciu obiektu EngagementConfiguration.

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="disable-real-time-reporting"></a>Wyłączanie raportowania w czasie rzeczywistym

Domyślnie raporty usług zaangażowania dzienniki w czasie rzeczywistym. Jeśli aplikacja często raporty dzienników, lepiej jest buforu dzienniki i przedstawianie zbiorczego na zasadzie zwykłej. Jest to "Tryb szybki".

W tym celu należy wywołać metodę:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Argument ma wartość (w **milisekundach)**. Gdy chcesz ponownie aktywować rejestrowania w czasie rzeczywistym, wywołaj metodę bez parametrów lub wartość 0.

Tryb serii nieco zwiększa baterii, ale ma wpływ na monitorze zaangażowania: wszystkie czas trwania sesji i zadania są zaokrąglane do progu serii (w związku z tym sesje i zadań krótsze od progu serii mogą nie być widoczne). Firma Microsoft zaleca używanie próg w serii się niż 30000 (30s). Zapisane dzienniki są ograniczone do 300 elementów. W przypadku wysyłania jest zbyt długi, może spowodować utratę niektórych dzienników.

> [AZURE.WARNING] Próg serii nie można skonfigurować do okresu mniej niż jednej sekundy. Jeśli zrobisz, zestawu SDK zawiera śledzenia z powodu błędu i automatycznie resetuje na wartość domyślną zerowe sekund. Uaktywnia to SDK raportowania dzienniki w czasie rzeczywistym.

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
