<properties
    pageTitle="Tworzenie wirtualnych systemu Windows komputera przy użyciu monitorowania i diagnostyki przy użyciu szablonu Menedżera zasobów Azure | Microsoft Azure"
    description="Szablon Menedżera zasobów Azure umożliwia tworzenie nowego komputera wirtualnych systemu Windows z rozszerzeniem Azure diagnostyki."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="sbtron"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="saurabh"/>

# <a name="create-a-windows-virtual-machine-with-monitoring-and-diagnostics-using-azure-resource-manager-template"></a>Tworzenie wirtualnych systemu Windows komputera przy użyciu monitorowania i diagnostyki przy użyciu szablonu Menedżera zasobów Azure

Rozszerzenie diagnostyki Azure zawiera, monitorowania i podstawie możliwości diagnostyki w systemie Windows Azure maszyn wirtualnych. Te funkcje na komputerze wirtualnych można włączyć, łącznie z rozszerzeniem jako część szablonu Menedżera zasobów azure. Zobacz [Tworzenie szablonów Menedżera zasobów Azure z rozszerzeniami maszyn wirtualnych](virtual-machines-windows-extensions-authoring-templates.md) uzyskać więcej informacji o tym dowolnymi jako część szablonu maszyn wirtualnych. W tym artykule opisano, jak dodać rozszerzenia diagnostyki Azure do szablonu maszyn wirtualnych systemu windows.  
  

## <a name="add-the-azure-diagnostics-extension-to-the-vm-resource-definition"></a>Dodawanie rozszerzenia diagnostyki Azure do definicji zasobu maszyn wirtualnych 

Aby włączyć rozszerzenie diagnostyki na komputerze wirtualnych systemu Windows, musisz dodać rozszerzenie jako zasób maszyn wirtualnych w szablonie Menedżera zasobów.

Dla prostych Menedżera zasobów maszyn wirtualnych systemu dodać konfiguracji rozszerzenia do tablicy *zasobów* dla maszyny wirtualnej: 

    "resources": [
                {
                    "name": "Microsoft.Insights.VMDiagnosticsSettings",
                    "type": "extensions",
                    "location": "[resourceGroup().location]",
                    "apiVersion": "2015-06-15",
                    "dependsOn": [
                        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
                    ],
                    "tags": {
                        "displayName": "AzureDiagnostics"
                    },
                    "properties": {
                        "publisher": "Microsoft.Azure.Diagnostics",
                        "type": "IaaSDiagnostics",
                        "typeHandlerVersion": "1.5",
                        "autoUpgradeMinorVersion": true,
                        "settings": {
                            "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), variables('vmName'), variables('wadcfgxend')))]",
                            "storageAccount": "[parameters('existingdiagnosticsStorageAccountName')]"
                        },
                        "protectedSettings": {
                            "storageAccountName": "[parameters('existingdiagnosticsStorageAccountName')]",
                            "storageAccountKey": "[listkeys(variables('accountid'), '2015-05-01-preview').key1]",
                            "storageAccountEndPoint": "https://core.windows.net"
                        }
                    }
                }
            ]


Inny Konwencja typowych jest dodawanie konfiguracji rozszerzenia w węzeł główny zasobów szablonu zamiast definiowania go węźle zasobów maszyny wirtualnej. Z tej metody należy jawnie określić hierarchicznych relacji między rozszerzeniem a maszyny wirtualnej z wartościami *Nazwa* i *Typ* . Na przykład: 
  
    "name": "[concat(variables('vmName'),'Microsoft.Insights.VMDiagnosticsSettings')]",
    "type": "Microsoft.Compute/virtualMachines/extensions",

Rozszerzenie są zawsze skojarzone z maszyny wirtualnej, można bezpośrednio bezpośrednio Definiowanie węźle zasób maszyny wirtualnej lub zdefiniować go na poziomie base i skojarzyć maszyny wirtualnej za pomocą konwencji nazewnictwa hierarchicznych.

Dla zestawów skali maszyn wirtualnych konfiguracji rozszerzenia określono we właściwości *extensionProfile* *VirtualMachineProfile*.
   
Właściwości *programu publisher* z wartością **Microsoft.Azure.Diagnostics** i *Typ* z wartością **IaaSDiagnostics** jednoznacznie identyfikować rozszerzenia diagnostyki Azure.

Wartość właściwości *nazwy* może służyć do rozszerzenia w grupie zasobów. Ustawienie z specjalnie **Microsoft.Insights.VMDiagnosticsSettings** umożliwi łatwą identyfikację Azure klasyczny portalu portalu zapewniające monitorowania wykresy wyświetlane poprawnie w portalu klasyczny Azure.

*TypeHandlerVersion* Określa wersję rozszerzenia, którego chcesz użyć. Ustawienie *autoUpgradeMinorVersion* wersją pomocniczą **wartość PRAWDA** zapewnia, otrzymasz najnowsza wersja pomocnicza rozszerzenia, który jest dostępny. Zdecydowanie zaleca się zawsze wartość *autoUpgradeMinorVersion* , aby **zawsze spełnienie tak, aby zawsze uzyskać dostęp do najnowszych rozszerzenia diagnostyki dostępne za pomocą wszystkie nowe funkcje i poprawki** . 

Element *Ustawienia* zawiera właściwości konfiguracji dla rozszerzenia, które można ustawić i ponownie odczyt rozszerzenia (nazywane publicznej konfiguracji). Właściwość *xmlcfg* zawiera konfigurację oparte na języku xml dla dzienniki diagnostyczne liczniki wydajności itp, która zostanie pobrana przez agenta diagnostyki. Zobacz [Narzędzia diagnostyczne konfiguracji schemat](https://msdn.microsoft.com/library/azure/dn782207.aspx) uzyskać więcej informacji o sobie schematu xml. Wspólnych ćwiczenia jest przechowywania konfiguracji rzeczywisty xml jako zmienna w szablonie Azure Menedżera zasobów, a następnie concatenate i base64 kodowanie ich tak, aby ustawić wartość *xmlcfg*. Zobacz sekcję na [Zmienne konfiguracji diagnostyki](#diagnostics-configuration-variables) , aby dowiedzieć się więcej na temat sposobu przechowywania xml w zmiennych. Właściwość *storageAccount* Określa nazwę konta miejsca do magazynowania, do którego będą przekazywane diagnostyki danych. 
 
Można ustawić właściwości w *protectedSettings* (nazywane konfiguracji prywatnych), ale nie można odczytać po ustawiany. Tylko do zapisu rodzaj *protectedSettings* ułatwia przydatne do przechowywania hasła, takie jak klucz konta magazynowania miejsce, w którym będą zapisywane dane diagnostyki.    

## <a name="specifying-diagnostics-storage-account-as-parameters"></a>Określanie konta miejsca do magazynowania diagnostyki jako parametrów 

Diagnostyka rozszerzenia json wstawkę kodu powyżej założono dwa parametry *existingdiagnosticsStorageAccountName* i *existingdiagnosticsStorageResourceGroup* do określenia konta miejsca do magazynowania diagnostyki przechowywania danych diagnostyki. Określanie konta miejsca do magazynowania diagnostyki jako parametru ułatwia łatwo zmienić konto diagnostyki przestrzeni dyskowej w różnych środowiskach np. można za pomocą konta miejsca do magazynowania różnych diagnostyki do testowania i inny rozmieszczania produkcji.  

        "existingdiagnosticsStorageAccountName": {
            "type": "string",
            "metadata": {
        "description": "The name of an existing storage account to which diagnostics data will be transfered."
            }        
        },
        "existingdiagnosticsStorageResourceGroup": {
            "type": "string",
            "metadata": {
        "description": "The resource group for the storage account specified in existingdiagnosticsStorageAccountName"
            }
        }

Najlepszym rozwiązaniem jest wykonanie określić konto diagnostyki miejsca do magazynowania w różnych grupach zasobów niż grupa zasobów dla maszyny wirtualnej. Grupa zasobów można uznać za jednostkę wdrażania przy użyciu własnej istnienia, maszyna wirtualna może być używany i ponownego rozmieszczania serwera nowych aktualizacji konfiguracji zostały wprowadzone go do niego, ale chcesz kontynuować przechowywanie danych diagnostyki z tego samego konta miejsca do magazynowania w tych wdrożeń maszyn wirtualnych. Bez konta przestrzeni dyskowej w różnych zasobów umożliwia konto przestrzeni dyskowej, aby akceptował dane z różnych wdrożeń maszyn wirtualnych, aby ułatwić rozwiązywanie problemów z wielu różnych wersji.

>[AZURE.NOTE] Po utworzeniu szablonu maszyn wirtualnych systemu windows z programu Visual Studio mogą mieć ustawionej domyślne konto miejsca do magazynowania używać tego samego konta miejsca do magazynowania, w której przekazane maszyny wirtualnej wirtualnego dysku twardego. To uprościć początkowej konfiguracji maszyn wirtualnych. Należy ponownie współczynnika szablonu do korzystania z konta innego miejsca do magazynowania, który może być jako parametr przekazano w. 

## <a name="diagnostics-configuration-variables"></a>Zmienne konfiguracji diagnostyki
 
Diagnostyka rozszerzenia json wstawkę kodu powyżej definiuje zmiennej *parametrem accountid* w celu uproszczenia wprowadzenie klucz konta miejsca do magazynowania do przechowywania diagnostyki:   
    
    "accountid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/',parameters('existingdiagnosticsStorageResourceGroup'), '/providers/','Microsoft.Storage/storageAccounts/', parameters('existingdiagnosticsStorageAccountName'))]"


Właściwość *xmlcfg* dla rozszerzenia diagnostyki jest zdefiniowana przy użyciu wielu zmiennych, które są łączone. Wartości tych zmiennych są w formacie xml, więc ich należy wstawić poprawnie podczas ustawiania zmiennych json.

Poniżej przedstawiono kod xml konfiguracji diagnostyki, który zbiera liczniki poziom wydajności systemu standardowej wraz z niektórych dzienniki infrastruktury narzędzia diagnostyczne i dzienniki zdarzeń systemu windows. Został wyjściowym i poprawnie sformatowany, tak aby konfiguracji można wkleić bezpośrednio do sekcji Zmienne szablonu. Zobacz [Narzędzia diagnostyczne konfiguracji schematu](https://msdn.microsoft.com/library/azure/dn782207.aspx) więcej ludzi przykład czytelne XML konfiguracji.
    
        "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
        "wadperfcounters1": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Privileged Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU privileged time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% User Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU user time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor Information(_Total)\\Processor Frequency\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"CPU frequency\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\System\\Processes\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Processes\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Threads\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Handle Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Handles\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\% Committed Bytes In Use\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Memory usage\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Available Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory available\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Committed Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory committed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Commit Limit\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory commit limit\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active time\" locale=\"en-us\"/></PerformanceCounterConfiguration>",
        "wadperfcounters2": "<PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Read Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active read time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Write Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active write time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Transfers/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Reads/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk read operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Writes/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk write operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Read Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk read speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Write Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk write speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\LogicalDisk(_Total)\\% Free Space\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk free space (percentage)\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
        "wadcfgxstart": "[concat(variables('wadlogs'), variables('wadperfcounters1'), variables('wadperfcounters2'), '<Metrics resourceId=\"')]",
        "wadmetricsresourceid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name , '/providers/', 'Microsoft.Compute/virtualMachines/')]",
        "wadcfgxend": "\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>"

Węzeł xml definicji metryki w konfiguracji powyżej zostanie element ważne konfiguracji, jak definiuje sposobu agregacją i przechowywania zdefiniowanej wcześniej w xml w węźle *PerformanceCounter* liczników wydajności. 

> [AZURE.IMPORTANT] Te metryki sterują monitorowania wykresy i alerty w portalu Azure.  Węzeł **metryki** z *resourceID* i **MetricAggregation** ma być uwzględniana w konfiguracji Diagnostyka sieci maszyn wirtualnych, jeśli chcesz wyświetlić dane monitorowania maszyn wirtualnych w portalu Azure. 

Oto przykład XML dla definicje wskaźników: 

        <Metrics resourceId="/subscriptions/subscription().subscriptionId/resourceGroups/resourceGroup().name/providers/Microsoft.Compute/virtualMachines/vmName">
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>

Atrybut *resourceID* jednoznacznie identyfikuje maszyny wirtualnej w ramach subskrypcji. Pamiętaj użyć funkcji subscription() i resourceGroup() tak, aby szablon automatycznie aktualizuje te wartości na podstawie subskrypcji i grupa zasobów, który instalujesz do.

Jeśli tworzysz wiele maszyn wirtualnych w pętli będzie mieć do zapełnienia wartość *resourceID* z funkcją copyIndex() poprawnie rozróżnienie poszczególnych poszczególnych maszyn wirtualnych. Wartość *xmlCfg* można aktualizować do obsługi to w następujący sposób:  

    "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), concat(parameters('vmNamePrefix'), copyindex()), variables('wadcfgxend')))]", 

Wartość MetricAggregation *PT1H* i *PT1M* oznaczają agregacji na minutę i agregacji w ciągu godziny.

## <a name="wadmetrics-tables-in-storage"></a>Tabele WADMetrics w magazynie

Konfiguracja metryki powyżej wygeneruje tabel na koncie diagnostyki miejsca do magazynowania z następujące konwencje nazewnictwa:

- **WADMetrics** : standardowego prefiksu dla wszystkich tabel WADMetrics
- **PT1H** lub **PT1M** : oznacza, że tabela zawiera agregowanie danych przekracza 1 godzina lub 1 minuta
- **P10D** : oznacza tabeli będzie zawierać dane przez 10 dni od uruchomienia tabeli na zbieranie danych
- **V2S** : stała ciągu
- **RRRRMMDD** : datę, dla której tabeli rozpocząć zbieranie danych

Przykład: *WADMetricsPT1HP10DV2S20151108* będzie zawierać dane metryki agregacją przez godzinę 10 dni, począwszy od dnia 11-lis-2015 r.    

Każda tabela WADMetrics będzie zawierać następujące kolumny:

- **PartitionKey**: partitionkey jest tworzona na podstawie wartości *resourceID* jednoznacznie zasobów maszyn wirtualnych. na przykład: 002Fsubscriptions:<subscriptionID>: 002FresourceGroups:002F<ResourceGroupName>: 002Fproviders:002FMicrosoft:002ECompute:002FvirtualMachines:002F<vmName>  
- **RowKey** : w formacie <Descending time tick>:<Performance Counter Name>. Dane w kolejności malejącej obliczanie osi czasu jest maksymalny czas znaczniki minus czas rozpoczęcia okres rozliczeniowy. Np. Jeśli okres przykładowy 2015-10-lis będzie 00:00Hrs UTC, a następnie obliczeń: DateTime.MaxValue.Ticks - (nowy DateTime(2015,11,10,0,0,0,DateTimeKind.Utc). Znaczniki osi). Dla pamięci wydajności dostępnych bajtów licznik klucz wiersza będzie wyglądać na przykład: 2519551871999999999__:005CMemory:005CAvailable:0020 bajtów
- **CounterName** : jest to nazwa licznika wydajności. Odpowiada *counterSpecifier* zdefiniowane w konfiguracji xml.
- **Maksymalna liczba** : maksymalną wartość licznika w okresie agregacji.
- **Minimalna** : minimalną wartość licznika w okresie agregacji.
- **Suma** : Suma wszystkich wartości licznika wydajności zgłoszone w okresie agregacji.
- **Liczba** : Całkowita liczba wartości zgłoszone dla licznika wydajności.
- **Średnia** : wartość średnią (suma i Statystyka) licznika w okresie agregacji.


## <a name="next-steps"></a>Następne kroki

- Dla szablonu całą próbkę maszyny wirtualnej systemu Windows z rozszerzeniem diagnostyki zobaczyć [201-maszyn wirtualnych monitorowania — Diagnostyka rozszerzenie](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-monitoring-diagnostics-extension)   
- Wdrażanie szablonu Menedżera zasobów za pomocą [Programu PowerShell Azure](virtual-machines-windows-ps-manage.md) lub [wiersza polecenia Azure](virtual-machines-linux-cli-deploy-templates.md)
- Dowiedz się więcej na temat [tworzenia szablonów Azure Menedżera zasobów](../resource-group-authoring-templates.md)







