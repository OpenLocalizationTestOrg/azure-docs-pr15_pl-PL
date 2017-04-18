<properties
    pageTitle="Przechowywanie i wyświetlanie danych diagnostycznych w magazynie Azure | Microsoft Azure"
    description="Pobieranie danych Azure diagnostyki do magazynowania Azure i wyświetlić"
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor="tysonn" />
<tags
    ms.service="cloud-services"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/01/2016"
    ms.author="robb" />

# <a name="store-and-view-diagnostic-data-in-azure-storage"></a>Przechowywanie i wyświetlanie danych diagnostycznych w magazynie Azure

Diagnostyczne dane nie są trwale przechowywane, chyba że transfer emulatora miejsca do magazynowania Microsoft Azure lub Azure miejsca do magazynowania. Raz w magazynie, można je wyświetlić od jednego z kilku dostępnych narzędzi.

## <a name="specify-a-storage-account"></a>Określanie konta miejsca do magazynowania

Określanie konta miejsca do magazynowania, który ma być używany w pliku ServiceConfiguration.cscfg. Informacje o koncie jest definiowana jako parametry połączenia w ustawieniach konfiguracji. W poniższym przykładzie pokazano domyślny ciąg połączenia utworzone dla nowego projektu usługi w chmurze w programie Visual Studio:


```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
    </ConfigurationSettings>
```

Możesz zmienić ten ciąg połączenia o podanie informacji o koncie usługi konto Azure miejsca do magazynowania.

W zależności od typu danych diagnostycznych, są zebrane Diagnostyka Azure używa albo obiektów Blob usługi lub tabeli. W poniższej tabeli przedstawiono źródła danych, które są zachowywane i ich formatów.

|Źródła danych|Format przechowywania|
|---|---|
|Dzienniki Azure|Tabela|
|Dzienniki usług IIS 7.0|Obiektów blob|
|Azure dzienników infrastruktury Diagnostyka|Tabela|
|Nie można zażądać dzienników|Obiektów blob|
|Dzienniki zdarzeń systemu Windows|Tabela|
|Liczniki wydajności|Tabela|
|Zrzuty awaryjne|Obiektów blob|
|Dzienniki błędów niestandardowych|Obiektów blob|

## <a name="transfer-diagnostic-data"></a>Transferować dane diagnostyczne

Dla zestawu SDK 2,5 i nowszych żądanie transferu danych diagnostycznych mogą występować w pliku konfiguracji. Możesz przenieść dane diagnostyczne w harmonogramie odstępach czasowych określonych w konfiguracji.

Dla 2,4 SDK i poprzedni można zażądać transfer danych diagnostycznych za pośrednictwem pliku konfiguracji także jako programowy. Programistyczne metoda pozwala również przeniesienia na żądanie.


>[AZURE.IMPORTANT] Podczas przenoszenia danych diagnostycznych z klientem Azure miejsca do magazynowania, spowodować kosztów zasobów miejsca do magazynowania, używane dane diagnostyczne.

## <a name="store-diagnostic-data"></a>Przechowywanie danych diagnostycznych

Dane dziennika są przechowywane w magazynie obiektów Blob lub tabelę z następującymi nazwami:

**Tabele**

- **WadLogsTable** - Dzienniki zapisane w kodzie przy użyciu odbiornika śledzenia.

- Zmienia **WADDiagnosticInfrastructureLogsTable** - monitor diagnostyczne i konfiguracji.

- **WADDirectoriesTable** — katalogów, które monitoruje monitorowania diagnostycznego.  Ta opcja uwzględnia dzienniki programu IIS, usług IIS nie powiodło się, dzienników żądanie i niestandardowe katalogów.  W polu kontenera określono lokalizację pliku dziennika obiektów blob, a nazwa to znajduje się w polu RelativePath.  Pole ŚcieżkaBezwględna wskazuje lokalizację i nazwę pliku, znajdowały się na komputerze Azure wirtualną.

- **WADPerformanceCountersTable** — liczników wydajności.

- **WADWindowsEventLogsTable** — dzienniki zdarzeń systemu Windows.

**BLOB**

- **wad sterowania kontenera** — (tylko w przypadku 2,4 SDK i poprzednich) zawiera pliki konfiguracji XML, które sterują Azure diagnostyki.

- dzienniki **wad-usług iis — failedreqlogfiles** — zawiera informacje z usług IIS nie powiodło się żądanie.

- dzienniki **wad-usług iis-logfiles** — zawiera informacje dotyczące usług IIS.

- **"niestandardowy"** — kontenera niestandardowe oparte na Konfigurowanie katalogów są monitorowane przez monitor diagnostyczne.  Nazwa tego kontenera obiektów blob będzie określona w WADDirectoriesTable.

## <a name="tools-to-view-diagnostic-data"></a>Narzędzia, aby wyświetlić dane diagnostyczne
Kilka narzędzi dostępnych do wyświetlania tych danych, gdy zostanie przeniesiona do magazynu. Na przykład:

- Eksplorator serwera w programie Visual Studio — po zainstalowaniu narzędzia Azure dla programu Microsoft Visual Studio umożliwia węzeł magazyn Azure w Eksploratorze serwera wyświetlanie obiektów blob tylko do odczytu i dane w tabeli z kont Azure miejsca do magazynowania. Można wyświetlać dane z magazynu lokalnego konta emulatora, a także z kont miejsca do magazynowania dla utworzono Azure. Aby uzyskać więcej informacji zobacz [Przeglądanie i zarządzanie zasobami magazynowania w Eksploratorze serwera](../vs-azure-tools-storage-resources-server-explorer-browse-manage.md).

- [Eksplorator magazynu usługi Microsoft Azure](../vs-azure-tools-storage-manage-with-storage-explorer.md) jest aplikację autonomicznej, która umożliwia łatwe pracę z danymi magazyn Azure w systemie Windows, OSX i Linux.

- [Azure Management Studio](http://www.cerebrata.com/products/azure-management-studio/introduction) zawiera Menedżera diagnostyki Azure, dzięki czemu można wyświetlać, Pobierz i zarządzanie diagnostyki danych zebranych przez aplikacje na Azure.


## <a name="next-steps"></a>Następne kroki

[Śledzenie przepływu w aplikacji usług w chmurze Diagnostyka Azure](cloud-services-dotnet-diagnostics-trace-flow.md)
