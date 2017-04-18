<properties
   pageTitle="Zbieranie dzienników przy użyciu zestawu narzędzi Diagnostyka Azure | Microsoft Azure"
   description="W tym artykule opisano, jak skonfigurować diagnostyki Azure do zbierania dzienników od klastrze tkaninie usługa działa w Azure."
   services="service-fabric"
   documentationCenter=".net"
   authors="ms-toddabel"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/28/2016"
   ms.author="toddabel"/>


# <a name="collect-logs-by-using-azure-diagnostics"></a>Zbieranie dzienników przy użyciu zestawu narzędzi Diagnostyka Azure

> [AZURE.SELECTOR]
- [Systemu Windows](service-fabric-diagnostics-how-to-setup-wad.md)
- [Linux](service-fabric-diagnostics-how-to-setup-lad.md)

Gdy korzystasz z klastrem tkaninie usługi Azure jest dobrym pomysłem jest zebrać dzienniki ze wszystkich węzłów w centralnej lokalizacji. O dziennikach w centralnej lokalizacji ułatwia analizowanie i rozwiązywanie problemów w klastrze lub problemów występujących w aplikacji i usług uruchamiania w klastrze.

Jednym ze sposobów przekazywanie i zbieranie dzienników ma rozszerzenie diagnostyki Azure przekazywanie dzienników do magazynu Azure za pomocą. Dzienniki nie są to przydatne bezpośrednio w magazynie. Ale proces zewnętrzny umożliwia odczyt zdarzenia miejsca do magazynowania i umieść je w produkcie, takich jak [Analizy dziennika](../log-analytics/log-analytics-service-fabric.md), [Elastyczne wyszukiwania](service-fabric-diagnostic-how-to-use-elasticsearch.md)lub inne rozwiązanie analizowania dziennika.

## <a name="prerequisites"></a>Wymagania wstępne
Niektóre z tych operacji wykonywanie w tym dokumencie za pomocą tych narzędzi:

* [Diagnostyka Azure](../cloud-services/cloud-services-dotnet-diagnostics.md) (związane z usługami w chmurze Azure, ale mam dobre informacje i przykłady)
* [Azure Menedżera zasobów](../azure-resource-manager/resource-group-overview.md)
* [Azure programu PowerShell](../powershell-install-configure.md)
* [Azure klienta Menedżera zasobów](https://github.com/projectkudu/ARMClient)
* [Azure szablonu Menedżera zasobów](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)


## <a name="log-sources-that-you-might-want-to-collect"></a>Dziennik źródeł, które mają być zbierane
- **Dzienniki usługi tkaninie**: emisji z platformy do standardowego kanałów zdarzenia śledzenia systemu Windows (ETW) i źródła zdarzeń. Może to być jednego z kilku typów dzienników:
  - Zdarzenia operacyjne: dzienników operacji wykonywanych platformy tkaninie usługi. Jako przykład można wymienić tworzenie aplikacji i usług, zmieni się stan węzła i informacje o uaktualnieniu.
  - [Niezawodne aktorów programowanie zdarzeń modelu](service-fabric-reliable-actors-diagnostics.md)
  - [Niezawodne programowania zdarzeń modelu usług](service-fabric-reliable-services-diagnostics.md)
- **Zdarzenia aplikacji**: zdarzenia emisji z tej usługi kodu i zapisywane przy użyciu źródła zdarzeń klasie opisane w szablonach programu Visual Studio. Aby uzyskać więcej informacji na temat zapisywanie dzienników w aplikacji, zobacz [Monitor i diagnozowanie usług w ustawieniach rozwoju komputer lokalny](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).


## <a name="deploy-the-diagnostics-extension"></a>Wdrażanie rozszerzenia diagnostyki
Zbieranie dzienników pierwszym krokiem jest wdrażanie rozszerzenia diagnostyki na każdej maszyny wirtualne w klastrze tkaninie usługi. Rozszerzenie diagnostyki zbieranie dzienników na poszczególnych maszyn wirtualnych i przekazuje je do konta miejsca do magazynowania, określonym przez użytkownika. Kroki nieco zależeć od tego, czy korzystasz z Azure portal lub Menedżer zasobów Azure. Kroki różnią się także na podstawie tego, czy wdrożenie jest częścią tworzenia klaster lub dotyczy klaster, który już istnieje. Przyjrzyjmy się czynności dla każdego scenariusza.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-through-the-portal"></a>Wdrażanie rozszerzenia diagnostyki jako część tworzenia klaster za pośrednictwem portalu
Aby wdrożyć rozszerzenia diagnostyki maszyny wirtualne w klastrze jako część tworzenia klaster, należy użyć panelu Ustawienia diagnostyki pokazano na poniższej ilustracji. Aby włączyć aktorów zaufanego lub zaufanego usług zbieranie zdarzeń, upewnij się, że diagnostyki jest ustawiona **na** (ustawienie domyślne). Po utworzeniu klaster, nie można zmienić te ustawienia za pomocą portalu.

![Azure ustawień diagnostyki w portalu do utworzenia klaster](./media/service-fabric-diagnostics-how-to-setup-wad/portal-cluster-creation-diagnostics-setting.png)

Obsługa *wymaga* zespołu pomocy technicznej Azure dzienniki w celu rozwiązania żądań obsługi tworzonych przez Ciebie. Dzienniki te są zbierane w czasie rzeczywistym i są przechowywane w jednym z kont miejsca do magazynowania utworzonych w grupie zasobów. Ustawienia diagnostyki konfigurowanie zdarzeń poziomie aplikacji. Zdarzenia te obejmują zdarzeń [Niezawodne uczestników](service-fabric-reliable-actors-diagnostics.md) , zdarzenia [Niezawodne usług](service-fabric-reliable-services-diagnostics.md) i niektóre zdarzenia usługi tkaninie systemowe mają być przechowywane w magazynie Azure.

Produktów, takich jak [Elastyczne wyszukiwania](service-fabric-diagnostic-how-to-use-elasticsearch.md) lub własny proces uzyskać zdarzenia z konta miejsca do magazynowania. Jest obecnie nie ma sposobu na filtrowanie lub oczyszczenie zdarzenia, które są wysyłane do tabeli. Jeśli nie zaimplementować proces usuwania zdarzeń z tabeli, tabeli będą powiększanie.

Podczas tworzenia klastrze za pomocą portalu, zaleca się pobraniu szablonu *przed kliknięciem przycisku * *OK*** utworzyć klaster. Aby uzyskać szczegółowe informacje zapoznaj się z [Konfigurowanie klastrze tkaninie usługi za pomocą szablonu Azure Menedżera zasobów](service-fabric-cluster-creation-via-arm.md). Konieczne będzie szablon, aby zmienić później, ponieważ nie można wprowadzić zmiany za pomocą portalu.

Szablony z portalu można wyeksportować, wykonując poniższe czynności. Jednak tych szablonów może być trudne do używania, ponieważ może zawierać wartości null, których brakuje wymagane informacje.

1. Otwieranie swojej grupy zasobów.
2. Wybierz pozycję **Ustawienia** , aby wyświetlić panel Ustawienia.
3. Wybierz pozycję **wdrożenia** , aby wyświetlić panel Historia wdrożenia.
4. Wybierz pozycję wdrażania, aby wyświetlić szczegóły wdrożenia.
5. Wybierz pozycję **Eksportuj szablon** , aby wyświetlić panel szablonu.
6. Wybierz opcję **zapisać do pliku** , aby wyeksportować plik zip, który zawiera szablon, parametr oraz plików programu PowerShell.

Po wyeksportowaniu pliki chcesz wprowadzić zmiany. Edytowanie pliku parameters.json i Usuń **adminPassword** element. Spowoduje to monit o podanie hasła po uruchomieniu skrypt wdrożenia. Gdy używasz skrypt wdrożenia, może być konieczne rozwiązywanie zerowe wartości parametrów.

Aby użyć szablonu pobranego zaktualizować konfiguracji:

1. Wyodrębnić jego zawartość do folderu na komputerze lokalnym.
2. Modyfikowanie zawartość w celu odzwierciedlenia nowej konfiguracji.
3. Uruchom program PowerShell i przejdź do folderu, w którym wyodrębnieniu zawartości.
4. Uruchamianie **deploy.ps1** i wprowadź identyfikator subskrypcji, nazwa grupy zasobów (używaj tej samej nazwy, aby zaktualizować konfigurację) i nazwę rozmieszczenia.


### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a>Wdrażanie rozszerzenia diagnostyki jako część tworzenia klaster przy użyciu Menedżera zasobów Azure
Aby utworzyć klaster przy użyciu Menedżera zasobów, musisz dodać konfiguracji diagnostyki JSON do szablonu Menedżera zasobów pełny klaster przed utworzeniem klaster. Firma Microsoft udostępnia przykładowy szablon Menedżera zasobów klaster maszyn wirtualnych pięciu konfiguracji diagnostyki dodanych w ramach naszej próbek szablonu Menedżera zasobów. Będzie ona widoczna w tej lokalizacji w galerii przykładów Azure: [klaster pięć z przykładowymi szablonu diagnostyki Menedżera zasobów](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype-wad).

Aby wyświetlić ustawienia diagnostyki w szablonie Menedżera zasobów, otwórz plik azuredeploy.json i wyszukiwanie **IaaSDiagnostics**. Aby utworzyć klaster za pomocą tego szablonu, kliknij przycisk **Deploy Azure** dostępnego pod adresem poprzedniego łącza.

Możesz też możesz pobrać przykładowy Menedżera zasobów, wprowadzać w nim zmiany i utworzyć klaster o zmodyfikowany szablon przy użyciu `New-AzureRmResourceGroupDeployment` polecenia w oknie Azure programu PowerShell. Zobacz poniższy kod dla parametrów, które należy przekazać do polecenia. Aby uzyskać szczegółowe informacje na temat instalacji grupa zasobów przy użyciu programu PowerShell zobacz artykuł [Rozmieszczanie grupa zasobów za pomocą szablonu Azure Menedżera zasobów](../resource-group-template-deploy.md).

```powershell

New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -Name $deploymentName -TemplateFile $pathToARMConfigJsonFile -TemplateParameterFile $pathToParameterFile –Verbose
```

### <a name="deploy-the-diagnostics-extension-to-an-existing-cluster"></a>Wdrażanie rozszerzenia diagnostyki z istniejącym klastrem
Jeśli masz istniejącym klastrem, która nie zawiera diagnostyki wdrożony lub jeśli chcesz zmodyfikować istniejący konfigurację, można dodawać lub je zaktualizować. Modyfikowanie szablonu Menedżera zasobów, który służy do tworzenia istniejący klaster lub pobrać szablon z portalu w sposób opisany. Modyfikowanie pliku template.json, wykonując następujące zadania.

Dodawanie nowego zasobu miejsca do magazynowania w szablonie, dodając do sekcji Zasoby.

```json
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[parameters('applicationDiagnosticsStorageAccountName')]",
  "location": "[parameters('computeLocation')]",
  "properties": {
    "accountType": "[parameters('applicationDiagnosticsStorageAccountType')]"
  },
  "tags": {
    "resourceType": "Service Fabric",
    "clusterName": "[parameters('clusterName')]"
  }
},
```

 Następnie dodaj do sekcji Parametry zaraz po definicje konta miejsca do magazynowania, między `supportLogStorageAccountName` i `vmNodeType0Name`. Zamień symbol zastępczy tekstu, *nazwę konta magazynu tu* nazwę konta magazynu.

```json
    "applicationDiagnosticsStorageAccountType": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "Replication option for the application diagnostics storage account"
      }
    },
    "applicationDiagnosticsStorageAccountName": {
      "type": "string",
      "defaultValue": "storage account name goes here",
      "metadata": {
        "description": "Name for the storage account that contains application diagnostics data from the cluster"
      }
    },
```
Następnie należy zaktualizować `VirtualMachineProfile` sekcji, dodając następujący kod znajdujące się wewnątrz tablicy rozszerzenia pliku template.json. Pamiętaj dodać średnik na początku lub na końcu, w zależności od tego, w którym został wstawiony.

```json
{
    "name": "[concat(parameters('vmNodeType0Name'),'_Microsoft.Insights.VMDiagnosticsSettings')]",
    "properties": {
        "type": "IaaSDiagnostics",
        "autoUpgradeMinorVersion": true,
        "protectedSettings": {
        "storageAccountName": "[parameters('applicationDiagnosticsStorageAccountName')]",
        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
        "storageAccountEndPoint": "https://core.windows.net/"
        },
        "publisher": "Microsoft.Azure.Diagnostics",
        "settings": {
        "WadCfg": {
            "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": "50000",
            "EtwProviders": {
                "EtwEventSourceProviderConfiguration": [
                {
                    "provider": "Microsoft-ServiceFabric-Actors",
                    "scheduledTransferKeywordFilter": "1",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableActorEventTable"
                    }
                },
                {
                    "provider": "Microsoft-ServiceFabric-Services",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableServiceEventTable"
                    }
                }
                ],
                "EtwManifestProviderConfiguration": [
                {
                    "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                    "scheduledTransferLogLevelFilter": "Information",
                    "scheduledTransferKeywordFilter": "4611686018427387904",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricSystemEventTable"
                    }
                }
                ]
            }
            }
        },
        "StorageAccount": "[parameters('applicationDiagnosticsStorageAccountName')]"
        },
        "typeHandlerVersion": "1.5"
    }
}
```

Po zmodyfikowaniu pliku template.json, zgodnie z opisem, ponowne opublikowanie szablonu Menedżera zasobów. Jeśli szablon został wyeksportowany, uruchamianie pliku deploy.ps1 je opublikuje szablonu. Po wdrożeniu, upewnij się, że **ProvisioningState** jest **zakończyła się pomyślnie**.


## <a name="update-diagnostics-to-collect-and-upload-logs-from-new-eventsource-channels"></a>Aktualizowanie narzędzia diagnostyczne do gromadzenia i przekaż dzienniki z nowych kanałów źródła zdarzeń
Aby zaktualizować diagnostyki zbieranie dzienników z nowych kanałów źródła zdarzeń reprezentujących nowej aplikacji, której zamierzasz wdrożyć, wykonaj te same kroki, jak pokazano w [poprzedniej sekcji](#deploywadarm) dla ustawienia diagnostyki dla istniejącym klastrem.

Aktualizowanie `EtwEventSourceProviderConfiguration` sekcji w pliku template.json, aby dodać wpisy nowe kanały źródła zdarzeń przed zastosowaniem konfiguracji aktualizacji przy użyciu `New-AzureRmResourceGroupDeployment` polecenia programu PowerShell. Nazwa źródła zdarzenia jest zdefiniowany jako części kodu w pliku wygenerowane Visual Studio ServiceEventSource.cs.

Na przykład jeśli źródło zdarzenia o nazwie Moje źródła zdarzeń, Dodaj następujący kod, aby umieścić wydarzenia z Moje źródła zdarzeń w tabeli o nazwie MyDestinationTableName.

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

Zbieranie liczników wydajności i dzienniki zdarzeń, zmodyfikuj szablon Menedżera zasobów za pomocą przykłady podane w sekcji [Tworzenie maszyny wirtualnej systemu Windows z monitorowania i diagnostyki za pomocą szablonu Azure Menedżera zasobów](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md). Następnie ponowne opublikowanie szablonu Menedżera zasobów.

## <a name="next-steps"></a>Następne kroki
Aby dowiedzieć się bardziej szczegółowo zdarzenia, jakie należy szukać podczas rozwiązywania problemów, zobacz zdarzeń diagnostycznych emisji dla [Uczestników zaufanego](service-fabric-reliable-actors-diagnostics.md) i [Niezawodnego usług](service-fabric-reliable-services-diagnostics.md).


## <a name="related-articles"></a>Artykuły pokrewne
* [Dowiedz się, jak zbieranie dzienników lub liczniki wydajności przy użyciu rozszerzenia diagnostyki](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)
* [Rozwiązanie tkaninie usługi w analizy dziennika](../log-analytics/log-analytics-service-fabric.md)
