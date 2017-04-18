<properties
    pageTitle="Przy użyciu magazyn obiektów blob dla usług IIS i tabeli miejsca do magazynowania dla zdarzenia | Microsoft Azure"
    description="Analizy dziennika można przeczytać dzienniki usługi Azure modyfikujące diagnostyki z magazynem tabel lub dzienniki programu IIS zapisywane blob miejsca do magazynowania."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="banders"/>


# <a name="using-blob-storage-for-iis-and-table-storage-for-events"></a>Przy użyciu magazyn obiektów blob dla usług IIS i Magazyn tabel dla zdarzenia

Analizy dziennika można przeczytać dzienniki dla następujących usług, które pisanie diagnostyki z magazynem tabel lub dzienniki programu IIS zapisywane blob miejsca do magazynowania:

+ Usługa tkaninie klastrów (wersja Preview)
+ Maszyn wirtualnych
+ Role w sieci Web i pracownika

Przed analizy dziennika może zbierać dane dla tych zasobów, musi być włączony diagnostyki Azure.

Po włączeniu narzędzia diagnostyczne można użyć Azure portal lub programu PowerShell Konfigurowanie analizy dziennika do zbierania dzienników.

Diagnostyka Azure to rozszerzenie Azure umożliwiający zbieranie danych diagnostycznych z roli Pracownik, ról w sieci web lub maszyn wirtualnych działa Azure. Dane są przechowywane na koncie Azure miejsca do magazynowania i następnie mogą być zbierane przez analizy dziennika.

Dziennik analiz zebranie dzienników diagnostycznych Azure dzienniki muszą być w następujących lokalizacjach:

| Typ dziennika | Typ zasobu | Lokalizacja |
| -------- | ------------- | -------- |
| Dzienniki programu IIS | Maszyn wirtualnych <br> Role w sieci Web <br> Role pracownika | wad usług iis logfiles (magazyn obiektów Blob) |
| SYSLOG | Maszyn wirtualnych | LinuxsyslogVer2v0 (Tabela miejsca do magazynowania) |
| Zdarzenia operacyjne tkaninie usługi | Węzły tkaninie usługi | WADServiceFabricSystemEventTable |
| Zdarzenia zaufanego Aktor tkaninie usługi | Węzły tkaninie usługi | WADServiceFabricReliableActorEventTable |
| Zdarzenia usługi zaufanego tkaninie usługi | Węzły tkaninie usługi | WADServiceFabricReliableServiceEventTable |
| Dzienniki zdarzeń systemu Windows | Węzły tkaninie usługi <br> Maszyn wirtualnych <br> Role w sieci Web <br> Role pracownika | WADWindowsEventLogsTable (magazyn tabel) |
| Dzienniki zdarzeń systemu Windows w systemie Windows | Węzły tkaninie usługi <br> Maszyn wirtualnych <br> Role w sieci Web <br> Role pracownika | WADETWEventTable (magazyn tabel) |

>[AZURE.NOTE] Dzienniki programu IIS pochodzących z takich witryn Azure nie są obecnie obsługiwane.

Dla maszyn wirtualnych masz opcję instalowania [agenta analizy dziennika](log-analytics-azure-vm-extension.md) do komputera wirtualnych, aby włączyć dodatkowe wnioski. Oprócz możliwość analizowania dzienniki programu IIS i dzienniki zdarzeń, można wykonać dodatkową analizę tym śledzenia zmian konfiguracji, ocena SQL i ocena aktualizacji.

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection"></a>Włączanie Azure diagnostyki w maszyny wirtualnej w dzienniku zdarzeń i usług IIS dziennika kolekcji

Poniższa procedura umożliwia włączanie Azure diagnostyki w maszyny wirtualnej do dziennika zdarzeń i IIS zbieranie dzienników za pomocą portalu Microsoft Azure.

### <a name="to-enable-azure-diagnostics-in-a-virtual-machine-with-the-azure-portal"></a>Aby włączyć Azure diagnostyki w maszyny wirtualnej Portal Azure

1. Po utworzeniu maszyny wirtualnej, należy zainstalować agenta maszyn wirtualnych. Jeśli maszyny wirtualnej już istnieje, upewnij się, że Agent maszyn wirtualnych jest już zainstalowany.
    - W portalu usługi Azure przejdź do maszyny wirtualnej, wybierz **Opcjonalnym**, a następnie **Narzędzia diagnostyczne** i ustaw **Stan** **na**.

    Po zakończeniu maszyn wirtualnych ma rozszerzenie diagnostyki Azure, zainstalować i uruchomić. To rozszerzenie jest odpowiedzialny za zbieranie danych diagnostyki.

2. Włączanie monitorowania i konfigurowanie rejestrowania na istniejące maszyn wirtualnych zdarzeń. Możesz włączyć diagnostyki na poziomie maszyn wirtualnych. Aby włączyć narzędzia diagnostyczne i skonfiguruj rejestrowanie zdarzeń, wykonaj następujące czynności:
    1. Wybierz pozycję maszyn wirtualnych.
    2. Kliknij pozycję **monitorowania**.
    3. Kliknij pozycję **Diagnostyka**.
    4. Ustaw **Stan** **włączone**.
    5. Zaznacz każdy dziennik diagnostyki, który chcesz zebrać.
    7. Kliknij **przycisk OK**.

Przy użyciu programu PowerShell Azure można dokładniej określić zdarzenia, które są zapisywane na magazyn Azure. Zapoznaj się z [Zbieranie danych przy użyciu diagnostyki Azure zapisywane w magazyn tabel lub dzienniki programu IIS zapisywane blob](log-analytics-azure-storage-json.md).


## <a name="enable-azure-diagnostics-in-a-web-role-for-iis-log-and-event-collection"></a>Włączanie Azure diagnostyki w roli sieci Web usług IIS dziennika i zdarzeń zbioru

Ogólne instrukcje dotyczące włączania Azure diagnostyki odwołują się do [Jak Aby włączyć diagnostyki w usłudze w chmurze](../cloud-services/cloud-services-dotnet-diagnostics.md) . Poniższe instrukcje za pomocą tych informacji i dostosować go do użytku z analizy dziennika.

Diagnostyka Azure włączone:

- Dzienniki programu IIS są przechowywane domyślnie transferowane interwałem scheduledTransferPeriod transfer danych dziennika.
- Domyślnie nie są transferowane dzienniki zdarzeń systemu Windows.

### <a name="to-enable-diagnostics"></a>Aby włączyć diagnostyki

Aby włączyć dzienniki zdarzeń systemu Windows lub zmienić scheduledTransferPeriod, konfigurowanie diagnostyki Azure za pomocą pliku konfiguracji XML (diagnostics.wadcfg), jak pokazano w [Krok 4: Tworzenie pliku konfiguracji narzędzia diagnostyczne i zainstaluj rozszerzenie](../cloud-services/cloud-services-dotnet-diagnostics.md)

Następujący przykład pliku konfiguracji zbiera dzienniki programu IIS i wszystkie zdarzenia z Dzienniki aplikacji i systemu:

```
    <?xml version="1.0" encoding="utf-8" ?>
    <DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"
          configurationChangePollInterval="PT1M"
          overallQuotaInMB="4096">

      <Directories bufferQuotaInMB="0"
         scheduledTransferPeriod="PT10M">  
        <!-- IISLogs are only relevant to Web roles -->
        <IISLogs container="wad-iis" directoryQuotaInMB="0" />
      </Directories>

      <WindowsEventLog bufferQuotaInMB="0"
         scheduledTransferLogLevelFilter="Verbose"
         scheduledTransferPeriod="PT10M">
        <DataSource name="Application!*" />
        <DataSource name="System!*" />
      </WindowsEventLog>

    </DiagnosticMonitorConfiguration>
```

Upewnij się, że usługi appSettings Określa konto miejsca do magazynowania, tak jak w poniższym przykładzie:

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"/>
    </ConfigurationSettings>
```

**Nazwa konta** i **AccountKey** wartości znajdują się w portalu Azure na pulpicie nawigacyjnym do konta z miejsca do magazynowania, w obszarze Zarządzanie klawiszy dostępu. Protokół ciągu połączenia musi być **https**.

Po zaktualizowaną konfigurację diagnostyki jest stosowana do usługi w chmurze i zapisuje diagnostyki do magazynu Azure, następnie możesz przystąpić do konfigurowania analizy dziennika.


## <a name="use-the-azure-portal-to-collect-logs-from-azure-storage"></a>Zbieranie dzienników z magazynu Azure za pomocą portalu Azure

Azure portal służy do konfigurowania analizy dziennika do zbierania dzienników dla następujących usług Azure:

+ Usługa tkaninie klastrów
+ Maszyn wirtualnych
+ Role w sieci Web i pracownika

W portalu usługi Azure przejdź do obszaru roboczego analizy dziennika i wykonać następujące czynności:

1. Kliknij pozycję *Dzienniki kont miejsca do magazynowania*
2. Kliknij pozycję *Dodaj* zadanie
3. Wybierz konto miejsca do magazynowania, które zawiera dzienniki diagnostyczne
  - To konto może być konto klasyczny miejsca do magazynowania lub konta usługi Azure Menedżera zasobów miejsca do magazynowania
4. Wybierz typ danych chcesz zbieranie dzienników
  - Dostępne są następujące dzienniki programu IIS; Zdarzenia; SYSLOG (Linux); Dzienniki zdarzeń systemu Windows; Zdarzenia tkaninie usługi
5. Wartość w polu Źródło jest automatycznie wypełnione na podstawie typu danych i nie można zmienić
6. Kliknij przycisk OK, aby zapisać konfigurację

Powtórz kroki od 2 do 6 dla kont dodatkowego miejsca do magazynowania i typy danych, które mają analizy dziennika do zbierania.

W około 30 minut jest możliwe wyświetlanie danych z konta miejsca do magazynowania w dzienniku analizy. Będziesz widzieć tylko dane, które są zapisywane do magazynu po zastosowaniu konfiguracji. Analizy dziennika nie odczytuje istniejące dane z konta miejsca do magazynowania.

>[AZURE.NOTE] Portalu nie sprawdza, czy źródło istnieje na koncie miejsca do magazynowania lub jeśli nowe dane są zapisywane.

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection-using-powershell"></a>Włączanie Azure diagnostyki w maszyny wirtualnej w dzienniku zdarzeń i usług IIS dziennika zbioru przy użyciu programu PowerShell

Wykonaj czynności podane w [Konfigurowaniu dziennika analizy indeksowania diagnostyki Azure](log-analytics-powershell-workspace-configuration.md#configuring-log-analytics-to-index-azure-diagnostics) za pomocą programu PowerShell odczyt Azure diagnostyki, które są zapisywane magazyn tabel.

Przy użyciu programu PowerShell Azure można dokładniej określić zdarzenia, które są zapisywane na magazyn Azure.
Aby uzyskać więcej informacji zobacz [Włączanie diagnostyki w maszyn wirtualnych Azure](../virtual-machines-dotnet-diagnostics.md).

Można włączyć i aktualizowanie diagnostyki Azure za pomocą skryptu programu PowerShell.
Za pomocą tego skryptu z konfiguracją Rejestrowanie niestandardowe.
Zmodyfikuj skrypt, aby ustawić konto miejsca do magazynowania, nazwa usługi i nazwa maszyn wirtualnych.
Skrypt używa polecenia cmdlet klasyczny maszyn wirtualnych.

Przejrzyj poniższy przykładowy skrypt, skopiuj go, zmodyfikuj je stosownie do potrzeb, Zapisz próbki jako plik skryptu programu PowerShell, a następnie uruchom skrypt.

```
    #Connect to Azure
    Add-AzureAccount

    # settings to change:
    $wad_storage_account_name = "myStorageAccount"
    $service_name = "myService"
    $vm_name = "myVM"

    #Construct Azure Diagnostics public config and convert to config format

    # Collect just system error events:
    $wad_xml_config = "<WadCfg><DiagnosticMonitorConfiguration><WindowsEventLog scheduledTransferPeriod=""PT1M""><DataSource name=""System!* "" /></WindowsEventLog></DiagnosticMonitorConfiguration></WadCfg>"

    $wad_b64_config = [System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($wad_xml_config))
    $wad_public_config = [string]::Format("{{""xmlCfg"":""{0}""}}",$wad_b64_config)

    #Construct Azure diagnostics private config

    $wad_storage_account_key = (Get-AzureStorageKey $wad_storage_account_name).Primary
    $wad_private_config = [string]::Format("{{""storageAccountName"":""{0}"",""storageAccountKey"":""{1}""}}",$wad_storage_account_name,$wad_storage_account_key)

    #Enable Diagnostics Extension for Virtual Machine

    $wad_extension_name = "IaaSDiagnostics"
    $wad_publisher = "Microsoft.Azure.Diagnostics"
    $wad_version = (Get-AzureVMAvailableExtension -Publisher $wad_publisher -ExtensionName $wad_extension_name).Version # Gets latest version of the extension

    (Get-AzureVM -ServiceName $service_name -Name $vm_name) | Set-AzureVMExtension -ExtensionName $wad_extension_name -Publisher $wad_publisher -PublicConfiguration $wad_public_config -PrivateConfiguration $wad_private_config -Version $wad_version | Update-AzureVM
```


## <a name="next-steps"></a>Następne kroki

- [Użyj JSON plików w magazynie obiektów blob](log-analytics-azure-storage-json.md) odczytywanie dzienniki Azure usług tej diagnostyki zapisu magazynem obiektów blob w formacie JSON.
- [Włączanie rozwiązań](log-analytics-add-solutions.md) o podanie wgląd w danych.
- [Używanie kwerend wyszukiwania](log-analytics-log-searches.md) do analizy danych.
