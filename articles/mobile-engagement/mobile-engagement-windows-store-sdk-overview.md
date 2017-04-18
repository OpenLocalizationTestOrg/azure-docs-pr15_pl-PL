<properties
    pageTitle="Integracja uniwersalny SDK systemu Windows"
    description="Integracja uniwersalny systemu Windows dla SDK dla Azure zaangażowania urządzeń przenośnych"                                     
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
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

#<a name="windows-universal-sdk-integration-for-azure-mobile-engagement"></a>Integracji uniwersalny SDK systemu Windows Azure zaangażowania urządzeń przenośnych

W tym dokumencie opisano wszystkie integracji i konfiguracji opcje dostępne dla Azure Mobile zaangażowania uniwersalny zestaw SDK systemu Windows.

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego samouczka, należy wykonać nasz [Samouczek 15-minutowe](mobile-engagement-windows-store-dotnet-get-started.md).

## <a name="advanced-features"></a>Zaawansowane funkcje

### <a name="reporting-features"></a>Funkcje raportowania
Można dodać następujące funkcje:

1. [Zaawansowane opcje raportowania](mobile-engagement-windows-store-advanced-reporting.md)

2. [Zaawansowane opcje konfiguracji](mobile-engagement-windows-store-advanced-configuration.md)

### <a name="notifications"></a>Powiadomienia

[Jak zintegrować w aplikacji systemu Windows Uniwersalny dostęp do (powiadomienia)](mobile-engagement-windows-store-integrate-engagement-reach.md)

### <a name="tag-plan-implementation"></a>Znacznik plan wykonania:

[Jak za pomocą zaawansowanych zaangażowania Mobile znakowanie interfejsu API w aplikacji systemu Windows uniwersalny](mobile-engagement-windows-store-use-engagement-api.md)

##<a name="release-notes"></a>Informacje o wersji

###<a name="340-04192016"></a>3.4.0 (2016-04-19)

-   Osiągnięcia ulepszenia nakładki.
-   Dzienniki konsoli dodane "TestLogLevel" interfejsu API do Włączanie i wyłączanie/filtru wysyłane przez zestaw SDK.
-   Uruchom stały powiadomienia w aktywności kierowanie z pierwszym działaniem nie są wyświetlane w aplikacji.

We wcześniejszych wersjach zobacz [pełne informacje o wersji](mobile-engagement-windows-store-release-notes.md)

##<a name="upgrade-procedures"></a>Procedury uaktualniania

Jeśli już masz zintegrowany ze starszej wersji programu zaangażowania do aplikacji, należy wziąć pod uwagę następujące punkty podczas uaktualniania zestawu SDK.

Jeśli nie odebrano kilka wersji zestawu SDK, może być konieczne procedurami. Zobacz pełny opis [Procedur uaktualniania](mobile-engagement-windows-store-upgrade-procedure.md). Na przykład możesz przeprowadzić migrację z 0.10.1 do 0.11.0, musisz najpierw należy wykonać procedurę "od 0.9.0 do 0.10.1" następnie procedury "od 0.10.1 do 0.11.0".

###<a name="from-330-to-340"></a>Z 3.3.0 do 3.4.0

####<a name="test-logs"></a>Testowanie dzienników

Dzienniki konsoli tworzone przez zestawu SDK można teraz włączone/wyłączone i filtrowane. Aby dostosować, zaktualizuj właściwość `EngagementAgent.Instance.TestLogEnabled` do jednej z wartości dostępne na `EngagementTestLogLevel` wyliczenie, na przykład:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

####<a name="resources"></a>Zasoby

Ulepszono nakładki Reach. Jest częścią zasoby pakietu SDK NuGet.

Podczas uaktualniania do nowej wersji zestawu SDK, można wybrać, czy chcesz zachować istniejące pliki z folderu nakładki zasobów, czy nie:

* Nakładki poprzedniego pracuje dla Ciebie, czy są integracji `WebView` elementy ręcznie, następnie możesz zdecydować zachować usługi zamykania plików, to nadal będzie działać.
* Aby zaktualizować nowy nakładki, Zamień całą `overlay` folder od zasobów na nową z pakietu SDK (aplikacje UWP: po uaktualnieniu, zostanie wyświetlony nowy folder w trybie nakładania z % USERPROFILE %\\.nuget\packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [AZURE.WARNING] Przy użyciu nowego nakładki zostaną zastąpione wszelkie zmiany wprowadzone w poprzedniej wersji.

### <a name="upgrade-from-older-versions"></a>Uaktualnienie starszych wersji

Zobacz [Uaktualnianie procedury](mobile-engagement-windows-store-upgrade-procedure.md)
