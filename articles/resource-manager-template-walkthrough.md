<properties
   pageTitle="Menedżer zasobów szablonu Instruktaż | Microsoft Azure"
   description="Instrukcje krok po kroku inicjowania obsługi administracyjnej podstawowej architektury Azure IaaS szablonu Menedżera zasobów."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="navalev"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/04/2016"
   ms.author="navale;tomfitz"/>
   
# <a name="resource-manager-template-walkthrough"></a>Przewodnik po szablonu Menedżera zasobów

Jedną z pierwszego pytania podczas tworzenia szablonu jest "jak zacząć?". Jeden Rozpoczynanie od pustego szablonu, wykonując podstawowa struktura opisane w [artykule Tworzenie szablonu](resource-group-authoring-templates.md#template-format)i dodawanie zasobów i odpowiednie parametry i zmiennych. Warto alternatywny może być Rozpocznij, przechodząc do [galerii Szybki Start](https://github.com/Azure/azure-quickstart-templates) i odszukaj podobne scenariuszy, które ten, który chcesz utworzyć. Można scalić kilka szablonów lub poddaj edycji istniejącą do potrzeb określonego rozwiązania. 

Spójrzmy na wspólnej infrastruktury:

* Dwa maszyn wirtualnych, które są w ten sam zestaw dostępność i w tej samej podsieci wirtualnej sieci, używać tego samego konta miejsca do magazynowania.
* Pojedynczy NIC i maszyn wirtualnych IP adres dla każdej maszyny wirtualnej.
* Równoważenia obciążenia, z reguły na porcie 80 równoważenia obciążenia

![Architektura](./media/resource-group-overview/arm_arch.png)

W tym temacie opisano kroki tworzenia szablonu Menedżera zasobów dla tej infrastruktury. Ostateczne tworzonego szablonu jest oparty na szablonie Szybki Start o nazwie [2 maszyny wirtualne usługi równoważenia obciążenia i reguły równoważenia obciążenia](https://azure.microsoft.com/documentation/templates/201-2-vms-loadbalancer-lbrules/).

Ale jest wiele można utworzyć w całym dokumencie, więc warto najpierw utworzyć konto miejsca do magazynowania i wdrożyć go. Po mieć format zarządzany Tworzenie konta miejsca do magazynowania, będzie dodać inne zasoby i ponownie wdrożyć szablonu do wykonania infrastruktury.

>[AZURE.NOTE] Dowolny typ edytora można używać podczas tworzenia szablonu. Programu Visual Studio zawiera narzędzia, których uprościć rozwoju szablon, ale nie ma potrzeby Visual Studio do użycia tego samouczka. Samouczek dotyczący tworzenie wdrażania aplikacji sieci Web i baza danych SQL za pomocą programu Visual Studio zobacz [Tworzenie i wdrażanie grupy zasobów Azure za pomocą programu Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md). 

## <a name="create-the-resource-manager-template"></a>Tworzenie szablonu Menedżera zasobów

Szablon jest plikiem JSON, który definiuje wszystkie zasoby, które zostanie wdrożony. W tym obszarze pozwala także definiowanie parametrów zdefiniowanych podczas wdrażania, zmienne skonstruowane z innych wartości i wyrażeń i wyjść z wdrożenia. 

Zacznijmy od najprostszych szablonu:

```json
    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {  },
      "variables": {  },
      "resources": [  ],
      "outputs": {  }
    }
 ```

Zapisz plik jako **azuredeploy.json** (należy zauważyć, że szablonu może mieć dowolną nazwę, po prostu ten musi być plikiem json).

## <a name="create-a-storage-account"></a>Tworzenie konta miejsca do magazynowania
W sekcji **zasoby** dodać obiektu, który definiuje konta miejsca do magazynowania, tak jak pokazano poniżej. 

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[parameters('storageAccountName')]",
    "apiVersion": "2015-06-15",
    "location": "[resourceGroup().location]",
    "properties": {
      "accountType": "Standard_LRS"
    }
  }
]
```

Być może zastanawiasz się te właściwości i wartości skąd. Właściwości **Typ**, **Nazwa**, **apiVersion**i **lokalizacji** są standardowe elementy, które są dostępne dla wszystkich typów zasobów. Aby Dowiedz się więcej o typowych elementów na [zasoby](resource-group-authoring-templates.md#resources). **Nazwa** jest ustawiona na wartość parametru, który należy przekazać w podczas wdrażania i **lokalizacji** jako lokalizacji używane według grup zasobów. Przyjrzymy jak określić **Typ** i **apiVersion** w poniższych sekcjach.

W sekcji **Właściwości** zawiera wszystkie właściwości, które są unikatowe dla określonego typu zasobów. Wartości określone w tej sekcji dokładnie pasuje do operacji położenie w interfejsie API usługi REST tworzenia tego typu zasobów. Podczas tworzenia konta miejsca do magazynowania, musisz podać **dla konta**. Zwróć uwagę w [Interfejsie API usługi REST dotyczące tworzenia konta miejsca do magazynowania](https://msdn.microsoft.com/library/azure/mt163564.aspx) , że sekcja właściwości operacji pozostałych zawiera również właściwości **dla konta** i opisano dozwolone wartości. W tym przykładzie typ konta jest ustawiona na **Standard_LRS**, ale można określić wartość lub zezwolić użytkownikom w celu przekazania w polu Typ konta jako parametru.

Teraz dodamy przejdź do sekcji **Parametry** i zobacz, jak zdefiniować nazwę konta magazynu. Więcej informacji o używaniu parametrów w [parametrów](resource-group-authoring-templates.md#parameters)można znaleźć. 

```json
"parameters" : {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Storage Account Name"
      }
    }
}
```
W tym miejscu możesz zdefiniować parametr typu ciąg, który będzie zawierał nazwę konta magazynu. Wartość tego parametru przewiduje się podczas wdrażania szablonu.

## <a name="deploying-the-template"></a>Wdrażanie szablonu
Mamy pełny szablonu w celu tworzenia nowego konta miejsca do magazynowania. Jak odwołać, szablon został zapisany w pliku **azuredeploy.json** :

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters" : {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Storage Account Name"
      }
    }
  },  
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[parameters('storageAccountName')]",
      "apiVersion": "2015-06-15",
      "location": "[resourceGroup().location]",
      "properties": {
        "accountType": "Standard_LRS"
      }
    }
  ]
}
```

Istnieje jest kilka sposobów rozmieszczanie szablonu, jak widać w [artykule wdrożenia zasobów](resource-group-template-deploy.md). Aby wdrożyć szablon przy użyciu programu PowerShell Azure, należy użyć:

```powershell
# create a new resource group
New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West Europe"

# deploy the template to the resource group
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile azuredeploy.json
```

Lub, aby wdrożyć szablon przy użyciu interfejsu wiersza polecenia Azure, użyj:

```
azure group create -n ExampleResourceGroup -l "West Europe"

azure group deployment create -f azuredeploy.json -g ExampleResourceGroup -n ExampleDeployment
```

Teraz masz dumny właściciel konta miejsca do magazynowania!

Następne kroki należy dodać wszystkie zasoby, które są wymagane do wdrożenia architektura opisanych w polu Rozpocznij tego samouczka. W tym samym szablonie, którym pracujesz w na dodasz te zasoby.

## <a name="availability-set"></a>Konfigurowanie dostępności
Po definicji konta miejsca do magazynowania dodać availably ustawione dla maszyn wirtualnych. W tym przypadku brak jest dodatkowe właściwości wymagane, więc definicję jest dość proste. W przypadku, gdy chcesz zdefiniować aktualizacji domeny błędów i liczba Liczba wartości domeny, zobacz [Interfejsu API usługi REST związane z tworzeniem, Ustaw dostępność](https://msdn.microsoft.com/library/azure/mt163607.aspx) dla sekcji pełny właściwości.

```json
{
   "type": "Microsoft.Compute/availabilitySets",
   "name": "[variables('availabilitySetName')]",
   "apiVersion": "2015-06-15",
   "location": "[resourceGroup().location]",
   "properties": {}
}
```

Należy zauważyć, że **Nazwa** jest ustawiona na wartość zmiennej. Dla tego szablonu nazwę zestawu dostępność jest potrzebny w kilku różnych miejscach. Możesz łatwo zachować szablonu przez definiowanie tej wartości raz i korzystania z niego w wielu miejscach.

Wartość określona dla **typu** zawiera zarówno dostawcy zasobów i typ zasobu. Dla zestawów dostępność dostawcy zasobów jest **Microsoft.Compute** i typ zasobu jest **availabilitySets**. Możesz uzyskać listę dostawców dostępne zasobów przy użyciu następującego polecenia programu PowerShell:

```powershell
    Get-AzureRmResourceProvider -ListAvailable
```

Lub, jeśli korzystasz z platformy Azure interfejsu wiersza polecenia, można uruchomić następujące polecenie:
```
    azure provider list
```
Zakładając, że w tym temacie tworzysz z kontami miejsca do magazynowania, maszyn wirtualnych i wirtualnej sieci, pracujesz z:

- Microsoft.Storage
- Microsoft.Compute
- Microsoft.Network

Aby wyświetlić typów zasobów dla określonego dostawcy, uruchom następujące polecenia programu PowerShell:

```powershell
    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute).ResourceTypes
```

Lub na polecenie Azure następujące polecenie zwróci dostępnych typów w formacie JSON i zapisanie go w pliku.

```
    azure provider show Microsoft.Compute --json > c:\temp.json
```

**AvailabilitySets** powinna być widoczna w jednym z typów w **Microsoft.Compute**. Imię i nazwisko typu jest **Microsoft.Compute/availabilitySets**. Można określić nazwę typu zasobu dla zasobów w szablonie możesz.

## <a name="public-ip"></a>Publiczny adres IP
Definiowanie publiczny adres IP. Ponownie Spójrz na [Interfejsu API usługi REST dla publicznych adresów IP](https://msdn.microsoft.com/library/azure/mt163590.aspx) dla właściwości, aby ustawić.

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[parameters('publicIPAddressName')]",
  "location": "[resourceGroup().location]",
  "properties": {
    "publicIPAllocationMethod": "Dynamic",
    "dnsSettings": {
      "domainNameLabel": "[parameters('dnsNameforLBIP')]"
    }
  }
}
```

Metoda alokacji jest ustawiona na **dynamiczne** , ale można ustawić go na wartość potrzebujesz lub ustawić go, aby zaakceptować wartości parametru. Włączono użytkownicy szablonu w celu przekazania w wyniku wartość etykiety nazwę domeny.

Teraz Przyjrzyjmy się jak ustalić **apiVersion**. Po prostu podaną wartość pasuje do wersji interfejsu API usługi REST, który ma być używany podczas tworzenia zasobu. Tak można przeglądać dokumentację interfejsu API usługi REST dla tego typu zasobów. Lub można uruchomić następujące polecenia programu PowerShell dla określonego typu.

```powershell
    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Network).ResourceTypes | Where-Object ResourceTypeName -eq publicIPAddresses).ApiVersions
```
Która zwraca następujące wartości:

    2015-06-15
    2015-05-01-preview
    2014-12-01-preview

Aby wyświetlić wersje interfejsu API o polecenie Azure, uruchom tego samego polecenia **Pokaż azure dostawcy** pokazano wcześniej.

Podczas tworzenia nowego szablonu, wybierz najbardziej aktualną wersję interfejsu API.

## <a name="virtual-network-and-subnet"></a>Wirtualnej sieci i podsieci
Tworzenie wirtualnych sieci z jednej podsieci. Przyjrzyj się [Interfejsu API usługi REST wirtualnych sieci](https://msdn.microsoft.com/library/azure/mt163661.aspx) dla właściwości, aby ustawić.

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Network/virtualNetworks",
   "name": "[parameters('vnetName')]",
   "location": "[resourceGroup().location]",
   "properties": {
     "addressSpace": {
       "addressPrefixes": [
         "10.0.0.0/16"
       ]
     },
     "subnets": [
       {
         "name": "[variables('subnetName')]",
         "properties": {
           "addressPrefix": "10.0.0.0/24"
         }
       }
     ]
   }
}
```

## <a name="load-balancer"></a>Usługa równoważenia obciążenia
Teraz możesz utworzyć zewnętrznych przeciwległych równoważenia obciążenia. Ponieważ tej usługi równoważenia obciążenia używa publiczny adres IP, należy zadeklarować zależność na publiczny adres IP w sekcji **dependsOn** . Oznacza to, że równoważenia obciążenia będzie nie są wdrażane zakończeniem publiczny adres IP wdrażanie. Bez definiowania tę współzależność, zostanie wyświetlony komunikat o błędzie, ponieważ Menedżer zasobów spróbuje wdrażanie zasobów równolegle i spróbuje Ustawianie równoważenia obciążenia publiczny adres IP, która jeszcze nie istnieje. 

Będzie również utworzyć puli adresów wewnętrznej bazy danych, kilka reguł ruchu przychodzącego translatora adresów Sieciowych do RDP do maszyny wirtualne i reguły równoważenia obciążenia o sondy tcp na porcie 80 w definicji tego zasobu. Wyewidencjonowywanie [Interfejsu API usługi REST dla równoważenia obciążenia](https://msdn.microsoft.com/library/azure/mt163574.aspx) dla wszystkich właściwości.

```json
{
   "apiVersion": "2015-06-15",
   "name": "[parameters('lbName')]",
   "type": "Microsoft.Network/loadBalancers",
   "location": "[resourceGroup().location]",
   "dependsOn": [
     "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]"
   ],
   "properties": {
     "frontendIPConfigurations": [
       {
         "name": "LoadBalancerFrontEnd",
         "properties": {
           "publicIPAddress": {
             "id": "[variables('publicIPAddressID')]"
           }
         }
       }
     ],
     "backendAddressPools": [
       {
         "name": "BackendPool1"
       }
     ],
     "inboundNatRules": [
       {
         "name": "RDP-VM0",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "protocol": "tcp",
           "frontendPort": 50001,
           "backendPort": 3389,
           "enableFloatingIP": false
         }
       },
       {
         "name": "RDP-VM1",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "protocol": "tcp",
           "frontendPort": 50002,
           "backendPort": 3389,
           "enableFloatingIP": false
         }
       }
     ],
     "loadBalancingRules": [
       {
         "name": "LBRule",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "backendAddressPool": {
             "id": "[variables('lbPoolID')]"
           },
           "protocol": "tcp",
           "frontendPort": 80,
           "backendPort": 80,
           "enableFloatingIP": false,
           "idleTimeoutInMinutes": 5,
           "probe": {
             "id": "[variables('lbProbeID')]"
           }
         }
       }
     ],
     "probes": [
       {
         "name": "tcpProbe",
         "properties": {
           "protocol": "tcp",
           "port": 80,
           "intervalInSeconds": 5,
           "numberOfProbes": 2
         }
       }
     ]
   }
}
```

## <a name="network-interface"></a>Interfejs sieciowy
Utworzysz 2 interfejsy, jedną dla każdego maszyn wirtualnych. Zamiast zawierać zduplikowane pozycje interfejsów sieciowych umożliwia [Funkcja copyIndex()](resource-group-create-multiple.md) iteracyjne pętli Kopiuj (nazywane nicLoop) i tworzenie liczba interfejsów zgodnie z definicją w `numberOfInstances` zmiennych. Interfejs sieciowy zależy od Tworzenie wirtualnych sieci i równoważenia obciążenia. Używa podsieci zdefiniowanej w tworzenie wirtualnych sieci i identyfikator równoważenia obciążenia, aby skonfigurować puli adresów równoważenia obciążenia i reguły przychodzące translatora adresów Sieciowych.
Przyjrzyj się [Interfejsu API usługi REST interfejsów sieciowych](https://msdn.microsoft.com/library/azure/mt163668.aspx) dla wszystkich właściwości.

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Network/networkInterfaces",
   "name": "[concat(parameters('nicNamePrefix'), copyindex())]",
   "location": "[resourceGroup().location]",
   "copy": {
     "name": "nicLoop",
     "count": "[variables('numberOfInstances')]"
   },
   "dependsOn": [
     "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]",
     "[concat('Microsoft.Network/loadBalancers/', parameters('lbName'))]"
   ],
   "properties": {
     "ipConfigurations": [
       {
         "name": "ipconfig1",
         "properties": {
           "privateIPAllocationMethod": "Dynamic",
           "subnet": {
             "id": "[variables('subnetRef')]"
           },
           "loadBalancerBackendAddressPools": [
             {
               "id": "[concat(variables('lbID'), '/backendAddressPools/BackendPool1')]"
             }
           ],
           "loadBalancerInboundNatRules": [
             {
               "id": "[concat(variables('lbID'),'/inboundNatRules/RDP-VM', copyindex())]"
             }
           ]
         }
       }
     ]
   }
}
```

## <a name="virtual-machine"></a>Maszyn wirtualnych
2 maszyn wirtualnych, zostanie utworzony przy użyciu funkcji copyIndex(), tak jak podczas tworzenia [interfejsów](#network-interface).
Tworzenie maszyn wirtualnych zależy od tego konta miejsca do magazynowania sieci interfejs i dostępność zestawu. Ten maszyn wirtualnych zostanie utworzony z obrazu marketplace, zgodnie z definicją w `storageProfile` właściwość — `imageReference` jest używana do definiowania obrazu w programie publisher, oferty i sku wersji. Na koniec diagnostyczne profil jest skonfigurowany do włączania diagnostyki dla maszyn wirtualnych. 

Aby znaleźć odpowiednie właściwości obrazu marketplace, wykonaj artykuły [Wybierz obrazy maszyn wirtualnych Linux](./virtual-machines/virtual-machines-linux-cli-ps-findimage.md) lub [Wybierz obrazy maszyn wirtualnych systemu Windows](./virtual-machines/virtual-machines-windows-cli-ps-findimage.md) .

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Compute/virtualMachines",
   "name": "[concat(parameters('vmNamePrefix'), copyindex())]",
   "copy": {
     "name": "virtualMachineLoop",
     "count": "[variables('numberOfInstances')]"
   },
   "location": "[resourceGroup().location]",
   "dependsOn": [
     "[concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName'))]",
     "[concat('Microsoft.Network/networkInterfaces/', parameters('nicNamePrefix'), copyindex())]",
     "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]"
   ],
   "properties": {
     "availabilitySet": {
       "id": "[resourceId('Microsoft.Compute/availabilitySets',variables('availabilitySetName'))]"
     },
     "hardwareProfile": {
       "vmSize": "[parameters('vmSize')]"
     },
     "osProfile": {
       "computerName": "[concat(parameters('vmNamePrefix'), copyIndex())]",
       "adminUsername": "[parameters('adminUsername')]",
       "adminPassword": "[parameters('adminPassword')]"
     },
     "storageProfile": {
       "imageReference": {
         "publisher": "[parameters('imagePublisher')]",
         "offer": "[parameters('imageOffer')]",
         "sku": "[parameters('imageSKU')]",
         "version": "latest"
       },
       "osDisk": {
         "name": "osdisk",
         "vhd": {
           "uri": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net/vhds/','osdisk', copyindex(), '.vhd')]"
         },
         "caching": "ReadWrite",
         "createOption": "FromImage"
       }
     },
     "networkProfile": {
       "networkInterfaces": [
         {
           "id": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'),copyindex()))]"
         }
       ]
     },
     "diagnosticsProfile": {
       "bootDiagnostics": {
          "enabled": "true",
          "storageUri": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net')]"
       }
     }
}
```

>[AZURE.NOTE] W przypadku obrazów publikowane przez **dostawców 3**będzie konieczne Określ inną właściwość o nazwie `plan`. Przykład można znaleźć w [tym szablonie](https://github.com/Azure/azure-quickstart-templates/tree/master/checkpoint-single-nic) z galerii Szybki Start. 

Określeniu zasoby dla szablonu.

## <a name="parameters"></a>Parametry

W sekcji Parametry określ wartości, które można określić podczas wdrażania szablonu. Tylko Definiowanie parametrów do wartości, które prawdopodobnie powinien być zróżnicowany podczas wdrażania. Można podać wartość domyślną dla parametru, który jest używany, jeśli nie został dostarczony podczas wdrażania. Można także zdefiniować dozwolona wartość, jak pokazano w parametrze **imageSKU** .

```json
"parameters": {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of storage account"
      }
    },
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Admin username"
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Admin password"
      }
    },
    "dnsNameforLBIP": {
      "type": "string",
      "metadata": {
        "description": "DNS for Load Balancer IP"
      }
    },
    "vmNamePrefix": {
      "type": "string",
      "defaultValue": "myVM",
      "metadata": {
        "description": "Prefix to use for VM names"
      }
    },
    "imagePublisher": {
      "type": "string",
      "defaultValue": "MicrosoftWindowsServer",
      "metadata": {
        "description": "Image Publisher"
      }
    },
    "imageOffer": {
      "type": "string",
      "defaultValue": "WindowsServer",
      "metadata": {
        "description": "Image Offer"
      }
    },
    "imageSKU": {
      "type": "string",
      "defaultValue": "2012-R2-Datacenter",
      "allowedValues": [
        "2008-R2-SP1",
        "2012-Datacenter",
        "2012-R2-Datacenter"
      ],
      "metadata": {
        "description": "Image SKU"
      }
    },
    "lbName": {
      "type": "string",
      "defaultValue": "myLB",
      "metadata": {
        "description": "Load Balancer name"
      }
    },
    "nicNamePrefix": {
      "type": "string",
      "defaultValue": "nic",
      "metadata": {
        "description": "Network Interface name prefix"
      }
    },
    "publicIPAddressName": {
      "type": "string",
      "defaultValue": "myPublicIP",
      "metadata": {
        "description": "Public IP Name"
      }
    },
    "vnetName": {
      "type": "string",
      "defaultValue": "myVNET",
      "metadata": {
        "description": "VNET name"
      }
    },
    "vmSize": {
      "type": "string",
      "defaultValue": "Standard_D1",
      "metadata": {
        "description": "Size of the VM"
      }
    }
  }
```

## <a name="variables"></a>Zmienne

W sekcji Zmienne można zdefiniować wartości, które są używane w więcej niż jednym miejscu w szablonie lub wartości, które są wykonane z innych wyrażenia lub zmiennych. Zmienne są często używane w celu uproszczenia składni szablonu.

```json
"variables": {
    "availabilitySetName": "myAvSet",
    "subnetName": "Subnet-1",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('vnetName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables ('subnetName'))]",
    "publicIPAddressID": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]",
    "numberOfInstances": 2,
    "lbID": "[resourceId('Microsoft.Network/loadBalancers',parameters('lbName'))]",
    "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/LoadBalancerFrontEnd')]",
    "lbPoolID": "[concat(variables('lbID'),'/backendAddressPools/BackendPool1')]",
    "lbProbeID": "[concat(variables('lbID'),'/probes/tcpProbe')]"
  }
```

Szablon została ukończona! Możesz porównać szablonu dla tego pełny szablonu w [galerii Szybki Start](https://github.com/Azure/azure-quickstart-templates) w obszarze [2 maszyny wirtualne z Usługa równoważenia obciążenia i ładowanie szablonu reguł równoważenia](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules). Szablon mogą się nieco różnić oparte na temat korzystania z innej wersji liczb. 

Szablon można ponownie wdrożyć za pomocą poleceń samej użytą podczas wdrażania konta miejsca do magazynowania. Nie musisz usunąć konto miejsca do magazynowania przed wdrożeniem ponownie, ponieważ Menedżer zasobów pominie ponownego tworzenia zasobów, które już istnieje, a nie zmieniły się.

## <a name="next-steps"></a>Następne kroki

- [Azure Menedżera zasobów szablon Wizualizator (ARMViz)](http://armviz.io/#/) jest doskonałym narzędziem do wizualizacji szablonów ARM, ponieważ może stać się zbyt duży, aby zrozumieć tylko z odczytywania pliku json.
- Aby dowiedzieć się więcej o strukturze szablonu, zobacz [Szablony do tworzenia Azure Menedżera zasobów](resource-group-authoring-templates.md).
- Aby dowiedzieć się o wdrażaniu szablonu, zobacz temat [Deploy grupa zasobów za pomocą szablonu Azure Menedżera zasobów](resource-group-template-deploy.md)
