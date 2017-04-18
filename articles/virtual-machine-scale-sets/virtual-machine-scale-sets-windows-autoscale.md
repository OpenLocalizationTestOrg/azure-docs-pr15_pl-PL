<properties
    pageTitle="Ustawia skali maszyn wirtualnych systemu Windows Autoscale | Microsoft Azure"
    description="Konfigurowanie autoscaling dla systemu Windows maszyn wirtualnych skali zestawu przy użyciu programu PowerShell Azure"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="automatically-scale-machines-in-a-virtual-machine-scale-set"></a>Automatyczne skalowanie maszyny w zestawie skali maszyn wirtualnych

Zestawy skali maszyn wirtualnych ułatwiają wdrażanie i zarządzanie identyczne maszyn wirtualnych jako zestaw. Zestawy skali zapewniają warstwy dużą skalowalność i możliwości dostosowywania obliczeń dla aplikacji informatycznych i obsługują obrazów platformy systemu Windows, Linux platformy obrazy, niestandardowych obrazów i rozszerzenia. Aby uzyskać więcej informacji na temat zestawów skali zobacz [Ustawia skali maszyn wirtualnych](virtual-machine-scale-sets-overview.md).

Ten samouczek pokazano, jak utworzyć zestaw skali maszyn wirtualnych systemu Windows i automatyczne skalowanie maszyny w zestawie. Możesz utworzyć Skala ustaw i ustaw skalowanie przez tworzenie szablonu wiadomości Menedżera zasobów Azure i wdrożenie go przy użyciu programu PowerShell Azure. Aby uzyskać więcej informacji na temat szablonów zobacz [Szablony do tworzenia Azure Menedżera zasobów](../resource-group-authoring-templates.md). Aby dowiedzieć się więcej na temat Automatyczne skalowanie zestawy skali, zobacz [Automatyczne skalowanie i zestawy skali maszyn wirtualnych](virtual-machine-scale-sets-autoscale-overview.md).

W tym artykule wdrożyć następujące zasoby i rozszerzenia:

- Microsoft.Storage/storageAccounts
- Microsoft.Network/virtualNetworks
- Microsoft.Network/publicIPAddresses
- Microsoft.Network/loadBalancers
- Microsoft.Network/networkInterfaces
- Microsoft.Compute/virtualMachines
- Microsoft.Compute/virtualMachineScaleSets
- Microsoft.Insights.VMDiagnosticsSettings
- Microsoft.Insights/autoscaleSettings

Aby uzyskać więcej informacji o zasobach Menedżera zasobów zobacz [obliczyć Azure, sieci i dostawców miejsca do magazynowania w obszarze Menedżer zasobów Azure](../virtual-machines/virtual-machines-windows-compare-deployment-models.md).

## <a name="step-1-install-azure-powershell"></a>Krok 1: Instalowanie programu PowerShell Azure

Zobacz, [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) dla informacji na temat instalowania najnowszej wersji programu PowerShell Azure, wybierając subskrypcji i logowanie się do Azure.

## <a name="step-2-create-a-resource-group-and-a-storage-account"></a>Krok 2: Tworzenie grupy zasobów i kontem miejsca do magazynowania

1. **Tworzenie grupy zasobów** — wszystkie zasoby należy wdrożyć do grupy zasobów. [Nowy AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx) należy utworzyć grupę zasobów o nazwie **vmsstestrg1**.

2. **Tworzenie konta miejsca do magazynowania** — to konto miejsca do magazynowania jest miejsce, w którym jest przechowywany szablon. Użyj [AzureRmStorageAccount nowy](https://msdn.microsoft.com/library/mt607148.aspx) , aby utworzyć konto miejsca do magazynowania, o nazwie **vmsstestsa**.

## <a name="step-3-create-the-template"></a>Krok 3: Tworzenie szablonu
Szablon programu Menedżer zasobów Azure umożliwia wdrażać i zarządzania zasobami Azure razem przy użyciu opis JSON zasobów i parametrów skojarzone wdrożenia.

1. W edytorze Ulubione Utwórz plik C:\VMSSTemplate.json i Dodaj początkowej struktury JSON do obsługi szablonu.

        {
          "$schema":"http://schema.management.azure.com/schemas/2014-04-01-preview/VM.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
          },
          "variables": {
          },
          "resources": [
          ]
        }

2. Parametry nie są zawsze wymagane, ale umożliwiają do wprowadzania wartości po wdrożeniu szablonu. Dodawanie tych parametrów w obszarze parametrów elementu nadrzędnego, który zostanie dodany do szablonu.

        "vmName": { "type": "string" },
        "vmSSName": { "type": "string" },
        "instanceCount": { "type": "string" },
        "adminUsername": { "type": "string" },
        "adminPassword": { "type": "securestring" },
        "resourcePrefix": { "type": "string" }

    - Ustaw nazwę osobnych maszyny wirtualnej, która służy do uzyskiwania dostępu do maszyn w skali.
    - Nazwę konta miejsca do magazynowania, w której jest przechowywany szablon.
    - Ustawianie liczby maszyn wirtualnych do wstępnie utworzone w skali.
    - Nazwę i hasło do konta administratora w środowisku maszyn wirtualnych systemu.
    - Ustaw prefiks nazwy dla zasobów, które są tworzone do pomocy technicznej odpowiednią skalę.

3. Zmienne może służyć w szablonie, aby określić wartości, które mogą się zmienić często lub wartości, które muszą zostać utworzone z kombinacji wartości parametrów. Dodać zmienne w obszarze Zmienne elementu nadrzędnego, który zostanie dodany do szablonu.

        "dnsName1": "[concat(parameters('resourcePrefix'),'dn1')]",
        "dnsName2": "[concat(parameters('resourcePrefix'),'dn2')]",
        "publicIP1": "[concat(parameters('resourcePrefix'),'ip1')]",
        "publicIP2": "[concat(parameters('resourcePrefix'),'ip2')]",
        "loadBalancerName": "[concat(parameters('resourcePrefix'),'lb1')]",
        "virtualNetworkName": "[concat(parameters('resourcePrefix'),'vn1')]",
        "nicName": "[concat(parameters('resourcePrefix'),'nc1')]",
        "lbID": "[resourceId('Microsoft.Network/loadBalancers',variables('loadBalancerName'))]",
        "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/loadBalancerFrontEnd')]",
        "storageAccountSuffix": [ "a", "g", "m", "s", "y" ],
        "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'a')]",
        "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/','Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
          "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
        "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
        "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
        "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
        "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"

  - Nazw DNS, które są używane przez sieć.
    - Nazwy adresów IP i prefiksów dla wirtualnej sieci i podsieci.
    - Nazwy i identyfikatory wirtualną sieć Załaduj równoważenia i interfejsów.
    - Ustawianie nazwy kont miejsca do magazynowania dla kont skojarzonych z urządzenia w skali.
    - Ustawienia dla rozszerzenia diagnostyki zainstalowanej w środowisku maszyn wirtualnych systemu. Aby uzyskać więcej informacji na temat rozszerzenia diagnostyki zobacz [Tworzenie wirtualnych systemu Windows maszyny liczącej z monitorowania i diagnostyki przy użyciu szablonu Menedżera zasobów Azure](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md).

4. Dodawanie miejsca do magazynowania zasobu konta w obszarze elementu nadrzędnego zasobów, który zostanie dodany do szablonu. Ten szablon używa pętli utworzyć zalecane pięć kont miejsca do magazynowania przechowywania dysków systemu operacyjnego i danych diagnostycznych. Ten zestaw kont może obsługiwać do 100 maszyn wirtualnych w zestawie skala jest aktualne maksimum. Przy użyciu określenia listu, zdefiniowanego w zmiennych połączone z prefiksem dostarczone w parametrach szablonu nosi nazwę każdego konta miejsca do magazynowania.

        {
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[concat(parameters('resourcePrefix'), variables('storageAccountSuffix')[copyIndex()])]",
          "apiVersion": "2015-06-15",
          "copy": {
            "name": "storageLoop",
            "count": 5
          },
          "location": "[resourceGroup().location]",
          "properties": { "accountType": "Standard_LRS" }
        },

5. Dodawanie zasobu wirtualnej sieci. Aby uzyskać więcej informacji zobacz [Dostawcy zasobów sieci](../virtual-network/resource-groups-networking.md).

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "[variables('virtualNetworkName')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
            "subnets": [
              {
                "name": "subnet1",
                "properties": { "addressPrefix": "10.0.0.0/24" }
              }
            ]
          }
        },

6. Dodawanie publicznej zasoby adresów IP, które są używane przez usługi równoważenia obciążenia i sieciowej.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "[variables('publicIP1')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "[variables('dnsName1')]"
            }
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "[variables('publicIP2')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "[variables('dnsName2')]"
            }
          }
        },

7. Dodawanie zasobu równoważenia obciążenia jest używana przez zestaw Skala. Aby uzyskać więcej informacji zobacz [Menedżer zasobów Azure obsługę usługi równoważenia obciążenia](../load-balancer/load-balancer-arm.md).

        {
          "apiVersion": "2015-06-15",
          "name": "[variables('loadBalancerName')]",
          "type": "Microsoft.Network/loadBalancers",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
          ],
          "properties": {
            "frontendIPConfigurations": [
              {
                "name": "loadBalancerFrontEnd",
                "properties": {
                  "publicIPAddress": {
                    "id": "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
                  }
                }
              }
            ],
            "backendAddressPools": [ { "name": "bepool1" } ],
            "inboundNatPools": [
              {
                "name": "natpool1",
                "properties": {
                  "frontendIPConfiguration": {
                    "id": "[variables('frontEndIPConfigID')]"
                  },
                  "protocol": "tcp",
                  "frontendPortRangeStart": 50000,
                  "frontendPortRangeEnd": 50500,
                  "backendPort": 3389
                }
              }
            ]
          }
        },

8. Dodawanie zasobu interfejsu sieciowego jest używana przez osobne maszyny wirtualnej. Ponieważ komputery w zestawie skali nie są dostępne za pośrednictwem publiczny adres IP, oddzielnych maszyny wirtualnej zostanie utworzona w tej samej sieci wirtualnej zdalnego dostępu maszyny.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[variables('nicName')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP2'))]",
            "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
          ],
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "publicIPAddress": {
                    "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIP2'))]"
                  },
                  "subnet": {
                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                  }
                }
              }
            ]
          }
        },

9. Dodawanie osobnych maszyny wirtualnej w tej samej sieci określonych Skala.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "[parameters('vmName')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "storageLoop",
            "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
          ],
          "properties": {
            "hardwareProfile": { "vmSize": "Standard_A1" },
            "osProfile": {
              "computername": "[parameters('vmName')]",
              "adminUsername": "[parameters('adminUsername')]",
              "adminPassword": "[parameters('adminPassword')]"
            },
            "storageProfile": {
              "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "2012-R2-Datacenter",
                "version": "latest"
              },
              "osDisk": {
                "name": "[concat(parameters('resourcePrefix'), 'os1')]",
                "vhd": {
                  "uri":  "[concat('https://',parameters('resourcePrefix'),'a.blob.core.windows.net/vhds/',parameters('resourcePrefix'),'os1.vhd')]"
                },
                "caching": "ReadWrite",
                "createOption": "FromImage"        
              }
            },
            "networkProfile": {
              "networkInterfaces": [
                {
                  "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
                }
              ]
            }
          }
        },

10. Dodawanie zestawu skali maszyn wirtualnych zasobów i określ rozszerzenia diagnostyki, które jest zainstalowana na wszystkich maszyn wirtualnych w zestawie Skala. Wiele ustawień dla tego zasobu są podobne z zasobem maszyn wirtualnych. Główne różnice są element wydajność, który określa liczbę maszyn wirtualnych w skali zestawu i upgradePolicy, która określa, jak aktualizacje są przystosowane do maszyn wirtualnych. Ustawianie skali nie zostanie utworzony, aż do utworzenia wszystkich kont miejsca do magazynowania określone z elementem dependsOn.

            {
              "type": "Microsoft.Compute/virtualMachineScaleSets",
              "apiVersion": "2016-03-30",
              "name": "[parameters('vmSSName')]",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "storageLoop",
                "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
                "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
              ],
              "sku": {
                "name": "Standard_A1",
                "tier": "Standard",
                "capacity": "[parameters('instanceCount')]"
              },
              "properties": {
                "upgradePolicy": {
                  "mode": "Manual"
                },
                "virtualMachineProfile": {
                  "storageProfile": {
                    "osDisk": {
                      "vhdContainers": [
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[0], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[1], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[2], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[3], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[4], '.blob.core.windows.net/vhds')]"
                      ],
                      "name": "vmssosdisk",
                      "caching": "ReadOnly",
                      "createOption": "FromImage"
                    },
                    "imageReference": {
                      "publisher": "MicrosoftWindowsServer",
                      "offer": "WindowsServer",
                      "sku": "2012-R2-Datacenter",
                      "version": "latest"
                    }
                  },
                  "osProfile": {
                    "computerNamePrefix": "[parameters('vmSSName')]",
                    "adminUsername": "[parameters('adminUsername')]",
                    "adminPassword": "[parameters('adminPassword')]"
                  },
                  "networkProfile": {
                    "networkInterfaceConfigurations": [
                      {
                        "name": "networkconfig1",
                        "properties": {
                          "primary": "true",
                          "ipConfigurations": [
                            {
                              "name": "ip1",
                              "properties": {
                                "subnet": {
                                  "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                                },
                                "loadBalancerBackendAddressPools": [
                                  {
                                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/backendAddressPools/bepool1')]"
                                  }
                                ],
                                "loadBalancerInboundNatPools": [
                                  {
                                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/inboundNatPools/natpool1')]"
                                  }
                                ]
                              }
                            }
                          ]
                        }
                      }
                    ]
                  },
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
                            "storageAccountKey": "[listkeys(variables('accountid'), '2015-06-15').key1]",
                            "storageAccountEndPoint": "https://core.windows.net"
                          }
                        }
                      }
                    ]
                  }
                }
              }
            },

11. Dodawanie zasobu autoscaleSettings, definiujące, jak zestaw skali skoryguje oparte na użycie procesora na komputerach w zestawie Skala.

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
                          "metricName": "\\Processor(_Total)\\% Processor Time",
                          "metricNamespace": "",
                          "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]",
                          "timeGrain": "PT1M",
                          "statistic": "Average",
                          "timeWindow": "PT5M",
                          "timeAggregation": "Average",
                          "operator": "GreaterThan",
                          "threshold": 50.0
                        },
                        "scaleAction": {
                          "direction": "Increase",
                          "type": "ChangeCount",
                          "value": "1",
                          "cooldown": "PT5M"
                        }
                      }
                    ]
                  }
                ],
                "targetResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
              }
            }

    Ten samouczek wartości te są ważne:

    - **metricName** — ta wartość jest taka sama, jak zdefiniowanej w zmiennej wadperfcounter licznika wydajności. Za pomocą tej zmiennej, zbiera rozszerzenia diagnostyki **Procesor(_Total)\% czas procesora** licznik.
    - **metricResourceUri** — ta wartość jest identyfikator zasobu zestawu skali maszyn wirtualnych.
    - **timeGrain** — ta wartość jest szczegółowości miar, które były zbierane. W tym szablonie jest ustawiony na minutę.
    - **statystyczny** — ta wartość określa, jak metryki są łączone, aby zezwalały na automatyczne skalowanie akcji. Możliwe wartości: średnia, minimum, maksimum. W tym szablonie są zbierane średnia całkowite użycie Procesora maszyn wirtualnych.
    - **timeWindow** — ta wartość jest przedział czasu, w którym są zbierane dane wystąpienia. Musi to być od 5 minut do 12 godzin.
    - **timeAggregation** — ta wartość określa, jak powinny być połączone dane, które są zbierane w czasie. Wartość domyślna to średnia. Możliwe wartości: średnia, Minimum, maksimum, ostatni, Suma, liczba.
    - **operator** — ta wartość jest operator, który jest używany do porównania danych metryczne i progu. Możliwe wartości: jest równe, NotEquals, większe, równe, mniejsze, równe.
    - **Próg** — ta wartość jest wartością Uaktywnia zadanie skali. W tym szablonie komputery są dodawane do skali Ustawianie w przypadku średnia użycie Procesora między maszyny w zestawie ponad 50%.
    - **kierunek** — ta wartość Określa akcję, która zostanie wykonana po uzyskuje się wartość progowa. Możliwe wartości to zwiększyć lub zmniejszyć. W tym szablonie liczby maszyn wirtualnych w zestawie skala jest zwiększane, gdy próg wynosi ponad 50% w oknie zdefiniowanego czasu.
    - **Typ** — ta wartość jest typem akcję, która powinna zostać wykonana i musi być ustawiona na ChangeCount.
    - **wartość** — ta wartość jest liczba maszyn wirtualnych, które są dodawane lub usuwane z zestawu Skala. Wartość ta musi być 1 lub większy. Wartością domyślną jest 1. W tym szablonie liczba komputerów w skali zestawu podwyżki przez 1, po spełnieniu progu.
    - **cooldown** — ta wartość jest czas oczekiwania od ostatniej akcji skalowania przed wystąpieniem następnej akcji. Ta wartość musi być między minutę i tydzień.

12. Zapisz plik szablonu.    

## <a name="step-4-upload-the-template-to-storage"></a>Krok 4: Przekazywanie szablonu do miejsca do magazynowania

Szablon można przekazywać pod warunkiem znasz nazwę i klucza podstawowego konta miejsca do magazynowania, który został utworzony w kroku 1.

1.  W oknie Microsoft Azure programu PowerShell Ustaw zmienną, która określa nazwę konta miejsca do magazynowania, który został utworzony w kroku 1.

            $storageAccountName = "vmstestsa"

2.  Ustaw zmienną określająca klucza podstawowego konta miejsca do magazynowania.

            $storageAccountKey = "<primary-account-key>"

    Ten klucz można uzyskać, klikając ikonę klucza podczas przeglądania zasobów konta miejsca do magazynowania w portalu Azure.

3.  Tworzenie obiektu kontekst konta miejsca do magazynowania, który jest używany do sprawdzania poprawności operacji za pomocą konta miejsca do magazynowania.

            $ctx = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

4.  Tworzenie kontenera do przechowywania szablonu.

            $containerName = "templates"
            New-AzureStorageContainer -Name $containerName -Context $ctx  -Permission Blob

5.  Przekaż plik szablonu do nowego kontenera.

            $blobName = "VMSSTemplate.json"
            $fileName = "C:\" + $BlobName
            Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob  $blobName -Context $ctx

## <a name="step-5-deploy-the-template"></a>Krok 5: Wdrażanie szablonu

Teraz, gdy masz utworzony szablon, można rozpocząć wdrażanie zasobów. Użyj tego polecenia, aby rozpocząć proces:

    New-AzureRmResourceGroupDeployment -Name "vmsstestdp1" -ResourceGroupName "vmsstestrg1" -TemplateUri "https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json"

Po naciśnięciu klawisza wpisz, zostanie wyświetlony monit o podanie wartości dla zmiennych, które zostały przydzielone. Wprowadź następujące wartości:

    vmName: vmsstestvm1
      vmSSName: vmsstest1
      instanceCount: 5
      adminUserName: vmadmin1
      adminPassword: VMpass1
      resourcePrefix: vmsstest

Należy trwa około 15 minut dla wszystkich zasobów pomyślnie wdrożone.

>[AZURE.NOTE] Za pomocą portalu możliwość wdrożenia zasobów. Użyj tego łącza: "https://portal.azure.com/#create/Microsoft.Template/uri/<link to VM Scale Set JSON template>"

## <a name="step-6-monitor-resources"></a>Krok 6: Monitor zasobów

Można uzyskać pewne informacje na temat zestawów skali maszyn wirtualnych za pomocą następujących metod:

 - Portal Azure — obecnie można uzyskać z ograniczoną ilość informacji za pomocą portalu.
 - [Eksplorator zasobów Azure](https://resources.azure.com/) - to narzędzie jest najlepszy dla uzyskiwanie informacji o bieżącym stanie zestawu Skala. Wykonaj następującą ścieżkę i powinien zostać wyświetlony widok wystąpienia zestawu skali utworzone przez Ciebie:

        subscriptions > {your subscription} > resourceGroups > vmsstestrg1 > providers > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtualMachines

 - Azure PowerShell — Użyj tego polecenia, aby uzyskać pewne informacje:

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

        Or

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceView

 - Łączenie oddzielnych maszyn wirtualnych tak, jak chcesz innego komputera, a następnie zdalny dostęp maszyn wirtualnych w skali Ustawianie monitorowanie indywidualnych procesów.

>[AZURE.NOTE] Ukończono interfejsu API usługi REST w celu uzyskania informacji na temat zestawów skali znajdują się w [Ustawia skali maszyn wirtualnych](https://msdn.microsoft.com/library/mt589023.aspx)

## <a name="step-7-remove-the-resources"></a>Krok 7: Usuwanie zasobów

Ponieważ jest naliczany dla zasobów używanych w Azure, zawsze jest dobrym rozwiązaniem jest usunięcie zasoby, które nie są już potrzebne. Nie musisz osobno usunąć każdego zasobu z grupy zasobów. Możesz usunąć grup zasobów i wszystkie jego zasoby są automatycznie usuwane.

    Remove-AzureRmResourceGroup -Name vmsstestrg1

Jeśli chcesz zachować grupy zasobów, możesz usunąć skali tylko.

    Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name"

## <a name="next-steps"></a>Następne kroki

- Zarządzanie zestawu skali właśnie utworzony przy użyciu informacji w [Zarządzanie maszyn wirtualnych w zestawie skali maszyn wirtualnych](virtual-machine-scale-sets-windows-manage.md).
- Dowiedz się więcej o skalowanie pionowe przeglądając [Pionowa autoscale z zestawami skali maszyn wirtualnych](virtual-machine-scale-sets-vertical-scale-reprovision.md)
- Znajdź przykłady monitorowania funkcje w [Azure Monitor programu PowerShell szybki start próbek](../monitoring-and-diagnostics/insights-powershell-samples.md) monitora Azure
- Więcej informacji na temat funkcji powiadomień w [za pomocą akcji autoscale do wysyłania wiadomości e-mail i webhook alertów w monitorze Azure](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)
- Dowiedz się, jak [Dzienniki inspekcji Użyj do wysyłania wiadomości e-mail i webhook alertów w monitorze Azure](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)
