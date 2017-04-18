<properties
    pageTitle="Omówienie diagnostyki Azure | Microsoft Azure"
    description="Diagnostyka Azure za pomocą debugowania, pomiaru wydajności, monitorowania, analizy ruchu w usług w chmurze, maszyn wirtualnych i tkaninie usługi"
    services="multiple"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/02/2016"
    ms.author="robb"/>


# <a name="what-is-microsoft-azure-diagnostics"></a>Co to jest Diagnostyka pakietu Microsoft Azure


Diagnostyka Azure to funkcja w Azure umożliwiający zbieranie danych diagnostycznych na wdrożonej aplikacji. Za pomocą rozszerzenia diagnostyki z wielu różnych źródeł. Obecnie obsługiwane są Azure chmury usługi w sieci Web i ról pracownik, maszyn wirtualnych Azure systemem Microsoft Windows i tkaninie usługi. Inne usługi Azure mieć własne osobnych diagnostyki.

## <a name="data-you-can-collect"></a>Dane, które może zbierać

Diagnostyka Azure może zbierać następujące typy danych:

Źródła danych|Opis
---|---
Liczniki wydajności | System operacyjny i liczniki wydajności niestandardowe
Dzienniki aplikacji     | Śledzenie wiadomości zapisanych przez aplikacji
Dzienniki zdarzeń systemu Windows   | Informacje są wysyłane do systemu rejestrowania zdarzeń systemu Windows
Źródło zdarzenia .NET    | Kod pisania zdarzeń za pomocą klasy.NET [źródła zdarzeń](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx)
Dzienniki programu IIS             | Informacje dotyczące witryn sieci web usług IIS
Pojawiają ETW zależności   | Zdarzenie śledzenia dla systemu Windows zdarzenia wygenerowane przez wszelkich procesów
Zrzuty awaryjne          | Informacje o stanie procesu wypadku awarii aplikacji
Dzienniki błędów niestandardowych    | Dzienniki tworzone przez aplikacji lub usługi
Dzienniki Azure infrastruktury diagnostyczne|Informacje o diagnostyki się

Rozszerzenie Azure diagnostyki można przenieść te dane do konta usługi Azure miejsca do magazynowania lub wyślij go do usługi, takie jak [Wniosków aplikacji](./application-insights/app-insights-cloudservices.md). Za pomocą danych dla debugowania i rozwiązywania problemów, pomiaru wydajności, monitorowanie użycia zasobów, analiza ruchu i planowanie pojemności i inspekcja.


## <a name="versioning"></a>Przechowywanie wersji
Zobacz [Narzędzia diagnostyczne Azure Historia wersji](azure-diagnostics-versioning-history.md).

## <a name="next-steps"></a>Następne kroki
Wybierz usługi, które chcesz zebrać diagnostyki na i wykonaj następujące artykuły, aby rozpocząć pracę. Skorzystaj z łączy ogólne diagnostyki Azure dla odwołania dla określonych zadań.

## <a name="web-apps"></a>Aplikacje sieci Web
Uwaga aplikacje sieci Web nie należy używać diagnostyki Azure. Znajdowanie odpowiednich informacji w [Aplikacjach sieci Web](./app-service-web/web-sites-enable-diagnostic-log.md)

## <a name="cloud-services-using-azure-diagnostics"></a>Usług w chmurze za pomocą narzędzia diagnostyczne Azure
- Jeśli przy użyciu programu Visual Studio, zobacz [Używanie programu Visual Studio do śledzenia aplikacji usług w chmurze](./vs-azure-tools-debug-cloud-services-virtual-machines.md) , aby rozpocząć pracę. W przeciwnym wypadku zobacz
- [Jak można monitorować za pomocą narzędzia diagnostyczne Azure usług w chmurze](./cloud-services/cloud-services-how-to-monitor.md)
- [Konfigurowanie diagnostyki Azure w chmurze aplikacji usługi](./cloud-services/cloud-services-dotnet-diagnostics.md)

Aby uzyskać bardziej zaawansowane zagadnienia zobacz

- [Za pomocą narzędzia diagnostyczne Azure wniosków aplikacji dla usług w chmurze](./application-insights/app-insights-cloudservices.md)
- [Śledzenie przepływu aplikacji usług w chmurze Diagnostyka Azure](./cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)
- [Konfigurowanie Diagnostyka dotycząca usług w chmurze przy użyciu programu PowerShell](./virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md)


## <a name="virtual-machines-using-azure-diagnostics"></a>Maszyn wirtualnych przy użyciu narzędzia diagnostyczne Azure
- Jeśli przy użyciu programu Visual Studio, zobacz [Używanie programu Visual Studio do śledzenia maszyn wirtualnych Azure](./vs-azure-tools-debug-cloud-services-virtual-machines.md) rozpocząć pracę. W przeciwnym wypadku zobacz
- [Konfigurowanie diagnostyki Azure na maszyn wirtualnych Azure](./virtual-machines-dotnet-diagnostics.md)

Aby uzyskać bardziej zaawansowane zagadnienia zobacz

- [Konfigurowanie diagnostyki na maszyn wirtualnych Azure za pomocą programu PowerShell](./virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md)
- [Tworzenie wirtualnych systemu Windows komputera przy użyciu monitorowania i diagnostyki przy użyciu szablonu Menedżera zasobów Azure](./virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)

## <a name="service-fabric-using-azure-diagnostics"></a>Za pomocą narzędzia diagnostyczne Azure tkaninie usługi
Rozpoczynanie pracy w [monitorze aplikacji usługi tkaninie](./service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). Wiele innych artykułów diagnostyki tkaninie usługi są dostępne w drzewie nawigacji po lewej stronie, po przejściu do tego artykułu.

## <a name="general-azure-diagnostics-articles"></a>Artykuły diagnostyki Azure ogólne
- [Konfiguracja schematu diagnostyki azure](https://msdn.microsoft.com/library/azure/mt634524.aspx) — informacje o zmienianiu pliku schematu do gromadzenia i kierowanie diagnostyki danych. Notatki można także użyć programu Visual Studio do zmiany pliku schematu.
- [Jak diagnostyki Azure dane są przechowywane w magazynie Azure](./cloud-services/cloud-services-dotnet-diagnostics-storage.md) - znać nazwy tabel i obiektów blob miejsce, w którym są zapisywane dane diagnostyczne.
- Dowiedz się, jak [korzystać z liczników wydajności w Azure informacje diagnostyczne](./cloud-services/cloud-services-dotnet-diagnostics-performance-counters.md).
- Dowiedz się, jak [informacje diagnostyczne rozsyłania Azure analizy aplikacji](./azure-diagnostics-configure-applicationinsights.md)
- Jeśli masz problemy z diagnostyki uruchamiania lub znajdowanie danych w tabelach magazyn Azure, zobacz [Rozwiązywanie problemów z diagnostyki Azure](./azure-diagnostics-troubleshooting.md)
