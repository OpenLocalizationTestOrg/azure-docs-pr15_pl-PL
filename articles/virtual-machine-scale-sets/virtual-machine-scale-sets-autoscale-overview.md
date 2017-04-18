<properties
    pageTitle="Automatyczne skalowanie i maszyn wirtualnych skalowanie zestawy | Microsoft Azure"
    description="Informacje o używaniu narzędzia diagnostyczne i zasoby autoscale automatyczne skalowanie maszyn wirtualnych w zestawie Skala."
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="davidmu"/>

# <a name="automatic-scaling-and-virtual-machine-scale-sets"></a>Automatyczne skalowanie i maszyn wirtualnych skalowanie zestawy

Automatyczne skalowanie maszyn wirtualnych w zestawie skala jest utworzenia lub usunięcia maszyn w zestawie stosownie do potrzeb, zgodnie z wymaganiami wydajności. W miarę zakresu pracy aplikacja może wymagać dodatkowych zasobów, aby umożliwić efektywnie wykonywać zadania.

Automatyczne skalowanie jest zautomatyzowany proces, że pomaga ułatwić zarządzanie. Zmniejszając obciążenie, nie musisz stale monitorować wydajność systemu lub podjęcie decyzji o sposobie zarządzania zasobami. Skalowanie jest procesem elastyczne. Więcej zasobów można dodać wraz ze wzrostem obciążenia, ale jako żądanie można usunąć obniżki zasobów, aby zminimalizować koszty i Obsługa poziomów wydajności.

Skonfiguruj automatyczne skalowanie skali ustawić przy użyciu szablonu Menedżera zasobów Azure, Azure programu PowerShell, polecenie Azure lub Azure portal.

## <a name="set-up-scaling-by-using-resource-manager-templates"></a>Konfigurowanie skalowania przy użyciu szablonów Menedżera zasobów

Zamiast wdrażanie i zarządzanie każdemu zasobowi aplikacji oddzielnie, należy użyć szablonu, który wdraża wszystkie zasoby w jednym, skoordynowanego operacji. W szablonie zasoby aplikacji są definiowane i wdrażania parametry są określane w różnych środowiskach. Szablon zawiera JSON i wyrażeń, których można użyć do utworzenia wartości dla wdrożenia. Aby dowiedzieć się więcej, spójrz na [szablonów do tworzenia Azure Menedżera zasobów](../resource-group-authoring-templates.md).

W szablonie Określ element możliwości:

    "sku": {
      "name": "Standard_A0",
      "tier": "Standard",
      "capacity": 3
    },

Wydajność określa liczbę maszyn wirtualnych w zestawie. Wydajność można zmienić ręcznie wdrażając szablon z inną wartość. W przypadku rozmieszczania szablonu, aby zmienić tylko wydajność może zawierać tylko element SKU z możliwości zaktualizowane.

Automatyczne zmienianie wydajność usługi skali ustawić przy użyciu kombinacji zasobów autoscaleSettings i rozszerzenia diagnostyki.

### <a name="configure-the-azure-diagnostics-extension"></a>Konfigurowanie rozszerzenia diagnostyki Azure

Automatyczne skalowanie można wykonać tylko, jeśli zbioru metryki zakończyło się pomyślnie na każdym komputerze wirtualnych w zestawie Skala. Rozszerzenie diagnostyki Azure zapewnia możliwości monitorowania i diagnostyki, które spełnia wymagania zbioru metryki zasobu autoscale. Możesz zainstalować rozszerzenie jako część szablonu Menedżera zasobów.

W tym przykładzie przedstawiono zmiennych, które są używane w szablonie, aby skonfigurować rozszerzenia diagnostyki:

    "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'saa')]",
    "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/', 'Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
    "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
    "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Thread Count\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
    "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
    "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
    "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"

Parametry znajdują się po wdrożeniu szablonu. W tym przykładzie nazwę konta miejsca do magazynowania, w której są przechowywane dane i nazwę zestawu skali, w którym dane są zbierane są dostarczane. Również w tym przykładzie Windows Server tylko wątku wydajności licznik są zbierane. Wszystkie liczniki wydajności dostępne w systemie Windows i Linux oraz służy do zbierania informacji diagnostycznych i zostaną uwzględnione w konfiguracji rozszerzenia.

W tym przykładzie przedstawiono definicję rozszerzenia w szablonie:

    "extensionProfile": {
      "extensions": [
        {
          "name": "Microsoft.Insights.VMDiagnosticsSettings",
          "properties": {
            "publisher": "Microsoft.Azure.Diagnostics",
            "type": "IaaSDiagnostics",
            "typeHandlerVersion": "1.5",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "xmlCfg": "[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
              "storageAccount": "[variables('diagnosticsStorageAccountName')]"
            },
            "protectedSettings": {
              "storageAccountName": "[variables('diagnosticsStorageAccountName')]",
              "storageAccountKey": "[listkeys(variables('accountid'), variables('apiVersion')).key1]",
              "storageAccountEndPoint": "https://core.windows.net"
            }
          }
        }
      ]
    }

Po uruchomieniu rozszerzenia diagnostyki dane są zbierane w tabeli, która znajduje się na koncie miejsca do magazynowania, określonym przez użytkownika. W tabeli WADPerformanceCounters znajdują się zebrane dane:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountBefore2.png)

### <a name="configure-the-autoscalesettings-resource"></a>Konfigurowanie zasobów autoScaleSettings

Zasób autoscaleSettings korzysta z informacji z rozszerzenia diagnostyki zdecydować, czy chcesz zwiększyć lub zmniejszyć liczbę maszyn wirtualnych w zestawie Skala.

W tym przykładzie pokazano konfigurację zasobu w szablonie:

    {
      "type": "Microsoft.Insights/autoscaleSettings",
      "apiVersion": "2015-04-01",
      "name": "[concat(parameters('resourcePrefix'),'as1')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
      ],
      "properties": {
        "enabled": true,
        "name": "[concat(parameters('resourcePrefix'),'as1')]",
        "profiles": [
          {
            "name": "Profile1",
            "capacity": {
              "minimum": "1",
              "maximum": "10",
              "default": "1"
            },
            "rules": [
              {
                "metricTrigger": {
                  "metricName": "\\Process(_Total)\\Thread Count",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 650
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              },
              {
                "metricTrigger": {
                  "metricName": "\\Process(_Total)\\Thread Count",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "LessThan",
                  "threshold": 550
                },
                "scaleAction": {
                  "direction": "Decrease",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              }
            ]
          }
        ],
        "targetResourceUri": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]"
      }
    }

W powyższym przykładzie dwie reguły są tworzone określających skalowania akcji automatycznych. Pierwszą regułę definiuje akcję poza skalowanie i drugą regułę definiuje Akcja Skala. Te wartości są dostarczane w regułach:

- **metricName** — ta wartość jest taka sama, jak zdefiniowanej w zmiennej wadperfcounter rozszerzenia diagnostyki licznika wydajności. W powyższym przykładzie jest używany licznik wątku.  
- **metricResourceUri** — ta wartość jest identyfikator zasobu zestawu skali maszyn wirtualnych. Ten identyfikator zawiera nazwę grupy zasobów, nazwa dostawcy zasobów i nazwę Skala ustaw skalowanie.
- **timeGrain** — ta wartość jest szczegółowości miar, które były zbierane. W powyższym przykładzie dane są zbierane z interwałem minutę. Ta wartość jest używana z timeWindow.
- **statystyczny** — ta wartość określa, jak metryki są łączone, aby zezwalały na automatyczne skalowanie akcji. Możliwe wartości: średnia, minimum, maksimum.
- **timeWindow** — ta wartość jest przedział czasu, w którym są zbierane dane wystąpienia. Musi to być od 5 minut do 12 godzin.
- **timeAggregation** — ta wartość określa, jak powinny być połączone dane, które są zbierane w czasie. Wartość domyślna to średnia. Możliwe wartości: średnia, Minimum, maksimum, ostatni, Suma, liczba.
- **operator** — ta wartość jest operator, który jest używany do porównania danych metryczne i progu. Możliwe wartości: jest równe, NotEquals, większe, równe, mniejsze, równe.
- **Próg** — ta wartość jest wartością Uaktywnia zadanie skali. Należy podać wystarczające różnią progu akcji poza skalowanie i próg Akcja Skala. Jeśli ustawisz wartości, które mają być taka sama, system przewiduje stałej zmiany, które uniemożliwiają wykonania akcji skalowania. Na przykład ustawiając oba 600 wątków w poprzednim przykładzie nie działa.
- **kierunek** — ta wartość Określa akcję, która zostanie wykonana po uzyskuje się wartość progowa. Możliwe wartości to zwiększyć lub zmniejszyć.
- **Typ** — ta wartość jest typem akcję, która powinna zostać wykonana i musi być ustawiona na ChangeCount.
- **wartość** — ta wartość jest liczby maszyn wirtualnych, które zostały dodane do lub usunięte z zestawu Skala. Wartość ta musi być 1 lub większy.
- **cooldown** — ta wartość jest czas oczekiwania od ostatniej akcji skalowania przed wystąpieniem następnej akcji. Ta wartość musi być między minutę i tydzień.

W zależności od licznika wydajności, którego używasz niektóre elementy znajdujące się w konfiguracji szablonu są używane inaczej. W poprzednim przykładzie licznik wydajności jest liczba wątku, wartość progu wynosi 650 akcji skala w nowym oknie, a wartość progowa jest 550 akcji w skali. Użycie licznik, takie jak % czas procesora wartość progowa jest równa wartości procentowe użycie Procesora, określające skalowania akcji.

Po utworzeniu obciążenie w przypadku maszyn wirtualnych, które uaktywnia skalowania zadanie możliwości zestawu zostaje zwiększona na podstawie wartości w szablonie. Na przykład w skali zestawu których wydajność ma wartość 3 i wartość akcji skali jest równa 1:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerBefore.png)

Załaduj utworzenia powodujący statystyka średnia wątku, aby przejść powyżej progu 650:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountAfter.png)

Akcja poza skalowanie zostanie wywołana powodujący stanie zwiększony o jeden zestaw:

    "sku": {
      "name": "Standard_A0",
      "tier": "Standard",
      "capacity": 4
    },

I maszyny wirtualnej jest dodawana do zestawu skali:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerAfter.png)

Po upływie cooldown pięć minut Jeśli średnia liczba wątków komputerów pozostaje ponad 600 inny komputer jest dodawany do zestawu. Jeśli liczba wątków średnia pozostaje poniżej 550, możliwości zestawu skali zmniejsza się o jedną i komputerze zostanie usunięty z zestawu.

## <a name="set-up-scaling-using-azure-powershell"></a>Konfigurowanie skalowania przy użyciu programu PowerShell Azure

Aby wyświetlić przykłady przy użyciu programu PowerShell, aby skonfigurować autoscaling, spójrz na [Azure Monitor programu PowerShell szybki start próbki](../monitoring-and-diagnostics/insights-powershell-samples.md).

## <a name="set-up-scaling-using-azure-cli"></a>Konfigurowanie skalowania przy użyciu interfejsu wiersza polecenia Azure

Aby wyświetlić przykłady używania polecenie Azure, aby skonfigurować autoscaling, spójrz na [polecenie między platformami Azure Monitor szybki start próbki](../monitoring-and-diagnostics/insights-cli-samples.md).

## <a name="set-up-scaling-using-the-azure-portal"></a>Konfigurowanie skalowania za pomocą portalu Azure

Aby zobaczyć przykład konfigurowania autoscaling za pomocą portalu Azure, spójrz na [Utwórz zestaw skali maszyn wirtualnych za pomocą portalu Azure](virtual-machine-scale-sets-portal-create.md).

## <a name="investigate-scaling-actions"></a>Badanie akcje skalowania

- [Azure portal]() - obecnie Ci ograniczoną ilość informacji za pomocą portalu.
- [Eksplorator zasobów azure]() — to narzędzie jest najlepszy dla uzyskiwanie informacji o bieżącym stanie zestawu Skala. Wykonaj następującą ścieżkę i powinien zostać wyświetlony widok wystąpienia zestawu skali utworzone przez Ciebie: subskrypcje > {subskrypcji} > resourceGroups > {grupy zasobów} > dostawcy > Microsoft.Compute > virtualMachineScaleSets > {zestawu skali} > virtualMachines
- Azure PowerShell — Użyj tego polecenia, aby uzyskać pewne informacje:

        Get-AzureRmResource -name vmsstest1 -ResourceGroupName vmsstestrg1 -ResourceType Microsoft.Compute/virtualMachineScaleSets -ApiVersion 2015-06-15
        Get-Autoscalesetting -ResourceGroup rainvmss -DetailedOutput

- Połącz maszyn wirtualnych jumpbox tak, jak chcesz innego komputera, a następnie zdalny dostęp maszyn wirtualnych w skali Ustawianie monitorowanie indywidualnych procesów.

## <a name="next-steps"></a>Następne kroki

- Przyjrzyj się [Automatyczne skalowanie maszyny w zestawie maszyn wirtualnych skali](virtual-machine-scale-sets-windows-autoscale.md) aby zobaczyć przykład sposobu tworzenia skali włączoną funkcję Automatyczne skalowanie skonfigurowane.
- Znajdź przykłady monitorowania funkcje w [Azure Monitor programu PowerShell szybki start próbek](../monitoring-and-diagnostics/insights-powershell-samples.md) monitora Azure
- Więcej informacji na temat funkcji powiadomień w [za pomocą akcji autoscale do wysyłania wiadomości e-mail i webhook alertów w monitorze Azure](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md).
- Więcej informacji na temat sposobu [użycia inspekcji logowania do wysyłania wiadomości e-mail i webhook alertów w monitorze Azure](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)
- Informacje na temat [autoscale zaawansowanych scenariuszy](./virtual-machine-scale-sets-advanced-autoscale.md).
