<properties
    pageTitle="Monitorowanie wydajności dla aplikacji sieci web urządzeń przenośnych z Deweloper analizy | Microsoft Azure"
    description="Wydajność aplikacji i monitorowanie użycia dla deweloperów aplikacji dla urządzeń przenośnych. , pulpit, usługi sieci web i wewnętrznej bazy danych aplikacji z HockeyApp i wniosków aplikacji."
    authors="alancameronwills"
    services="application-insights"
    documentationCenter=""
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="awills"/>

# <a name="mobile-analytics-with-hockeyapp-and-application-insights"></a>Analizy urządzeń przenośnych z HockeyApp i wniosków aplikacji

Monitorowanie wydajności i użycia beta test i wdrożonym przenośnych i stacjonarnych aplikacji z [HockeyApp](https://hockeyapp.net/). Monitorowanie obsługi sieci web usługi i wewnętrznej bazy danych aplikacji przy użyciu [Programu Visual Studio aplikacji wnioski](app-insights-overview.md). Analizowanie danych w aplikacjach klienta i serwera w analizy, a wyświetlanie wykresów obok siebie w Azure pulpitu nawigacyjnego.

Microsoft Developer analizy oferuje dwie monitorowania wydajności aplikacji:

* **HockeyApp dla urządzeń przenośnych** i aplikacji klasycznych.
 * Rozpowszechniać test dla wybranych użytkowników.
 * Ulec awarii analizy.
 * Niestandardowe telemetrycznego na potrzeby analizy użycia.
* Aplikacje **Wniosków aplikacji dla witryny sieci web** i usług i wewnętrznej bazy danych.
 * Wskaźniki wydajności i użycia i alerty.
 * Zgłoszenie wyjątku i śledzenie diagnostyczne.
 * Diagnostyczne wyszukiwania z łączami telemetrycznego filtrowania i pokrewnych.

Oferują obu rozwiązań:

 * Zaawansowane **[języka kwerend analityczny](app-insights-analytics.md)** narzędzia diagnostyczne i analizy.
 * **[Eksportowanie danych](app-insights-export-telemetry.md)** do własnych miejsca do magazynowania.
 * Wyświetlanie **zintegrowane pulpitu nawigacyjnego** analitycznym wykresy i tabele.

## <a name="monitor-your-app-components"></a>Monitorowanie składniki aplikacji

Postępuj zgodnie z instrukcjami te strony do instalowania zestawu SDK w kodzie i rozpocząć śledzenie aplikacji.

### <a name="web-apps---application-insights"></a>Aplikacje Web apps — wnioski aplikacji

* [Aplikacja sieci web programu ASP.NET](app-insights-asp-net.md) 
* [Aplikacja sieci web języka Java](app-insights-java-get-started.md)
* [Node.js aplikacji sieci web](https://github.com/Microsoft/ApplicationInsights-node.js)
* [Usług w chmurze Azure](app-insights-cloudservices.md)

### <a name="mobile-apps---hockeyapp"></a>Aplikacje dla urządzeń przenośnych — HockeyApp

* [aplikacji w systemie iOS](https://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-ios)
* [Mac OS X aplikacji](https://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-mac-os-x)
* [Aplikacja systemu android](https://support.hockeyapp.net/kb/client-integration-android/hockeyapp-for-android-sdk)
* [Aplikacja Universal systemu Windows](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/how-to-create-an-app-for-uwp)
* [Windows Phone 8 lub 8.1 aplikacji](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/hockeyapp-for-windows-phone-silverlight-apps-80-and-81)
* [Prezentacja Windows Foundation aplikacji](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/hockeyapp-for-windows-wpf-apps)

W przypadku aplikacji klasycznej systemu Windows zaleca się HockeyApp. Ale możesz też [wysyłać telemetrycznego za pomocą aplikacji systemu Windows do wniosków aplikacji](app-insights-windows-desktop.md). Można to zrobić aby poeksperymentować z wniosków aplikacji.


## <a name="analytics-and-export-for-hockeyapp-telemetry"></a>Analizy i Eksportuj do telemetrycznego HockeyApp

Możesz badanie HockeyApp niestandardowe i zaloguj się przy użyciu analizy i eksportowanie ciągły funkcji aplikacji wniosków przez [Skonfigurowanie mostka](app-insights-hockeyapp-bridge-app.md)telemetrycznego.




