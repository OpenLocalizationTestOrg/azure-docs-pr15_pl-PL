<properties
   pageTitle="Wdrażanie obliczyć zasoby z Azure szablony Menedżera zasobów | Microsoft Azure"
   description="Samouczek DotNet Core Azure maszyn wirtualnych"
   services="virtual-machines-windows"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="nepeters"/>

# <a name="application-architecture-with-azure-resource-manager-templates"></a>Architektura aplikacji z szablonami Azure Menedżera zasobów

Podczas tworzenia wdrożenia usługi Azure Menedżera zasobów, wymagania obliczeń muszą być mapowane do zasobów Azure i usług. Jeśli aplikacja składa się z kilku http punktów końcowych, bazy danych i danych pamięci podręcznej usługi, Azure zasobów obsługujących każdego z tych składników musi być usprawnione. Na przykład przykładowej aplikacji sklep muzyczny zawiera aplikacji sieci web, który znajduje się na komputerze wirtualnych i baza danych SQL, który znajduje się w bazie danych Azure SQL. 

Ten dokument, szczegóły, jak zasoby obliczeń sklep muzyczny są skonfigurowane w szablonie Menedżera zasobów Azure próbki. Wszystkie zależności i unikatowe konfiguracje są wyróżnione. Aby uzyskać najlepsze wyniki, wstępnie wdrażać wystąpienie rozwiązanie do subskrypcji Azure i pracy wraz z szablonu Azure Menedżera zasobów. Zakończenie szablonu można znaleźć tutaj — [Wdrożenia sklepu muzyki w systemie Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="virtual-machine"></a>Maszyn wirtualnych

Aplikacja sklep muzyczny zawiera aplikacji sieci web, w którym klientów można wyszukiwać i kupować muzyki. Gdy istnieje kilka usług Azure, które mogą obsługiwać aplikacje sieci web, w tym przykładzie jest używany maszyny wirtualnej. Za pomocą szablonu sklep muzyczny przykładowe maszyny wirtualnej jest używany, zainstalować na serwerze sieci web i witryny sieci Web sklep muzyczny zainstalowaniu i skonfigurowaniu. Ze względu na ten artykuł jest szczegółowe tylko wdrażania maszyn wirtualnych. Konfiguracji serwera sieci web i aplikacji jest szczegółowe w artykule później.

Maszyny wirtualnej można dodawać do szablonu, za pomocą kreatora Visual Studio Dodawanie nowego zasobu lub, wstawiając prawidłowych JSON w szablonie rozmieszczania. Wdrażając maszyny wirtualnej, potrzebne są również kilka powiązanych zasobów. Jeśli przy użyciu programu Visual Studio, aby utworzyć szablon, te zasoby są tworzone automatycznie. Jeśli ręcznego tworzenia szablonu, te zasoby muszą być wstawiony i skonfigurowany.

Wykonaj to łącze, aby wyświetlić przykładowe JSON w szablonie Menedżera zasobów — [JSON maszyn wirtualnych](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L285).

```none
{
  {
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/virtualMachines",
  "name": "[concat(variables('vmName'),copyindex())]",
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "virtualMachineLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "tags": {
    "displayName": "virtual-machine"
  },
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('vhdStorageName'))]",
    "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]",
    "nicLoop"
  ],
  "properties": {
    "availabilitySet": {
      "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
    },
  ........<truncated>  
}
```

Po wdrożeniu właściwości maszyn wirtualnych można znaleźć w portalu Azure.

![Maszyn wirtualnych](./media/virtual-machines-windows-dotnet-core/vm-win.png)

## <a name="storage-account"></a>Konto miejsca do magazynowania

W przypadku kont przestrzeni dyskowej są wiele opcji przechowywania i funkcje. W kontekście maszyn wirtualnych Azure konta magazynu zawiera wirtualnych twardych maszyny wirtualnej i wszelkie dodatkowe dane dysków. Przykładowe sklep muzyczny zawiera jedno konto miejsca do magazynowania do przechowywania wirtualnego dysku twardego każdej maszyny wirtualnej w ramach wdrożenia. 

Wykonaj to łącze, aby wyświetlić przykładowe JSON w szablonie Menedżera zasobów — [Konta miejsca do magazynowania](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L98).


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[variables('vhdStorageName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "storage-account"
  },
  "properties": {
    "accountType": "[variables('vhdStorageType')]"
  }
}
```

Konto miejsca do magazynowania jest skojarzyć z maszyny wirtualnej wewnątrz deklaracji szablonu Menedżera zasobów maszyny wirtualnej. 

Wykonaj to łącze, aby wyświetlić przykładowe JSON w szablonie Menedżera zasobów — [skojarzenia maszyn wirtualnych i konto miejsca do magazynowania](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L321).

```none
"osDisk": {
  "name": "osdisk",
  "vhd": {
    "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/',variables('vhdStorageName')), '2015-06-15').primaryEndpoints.blob,'vhds/osdisk', copyindex(), '.vhd')]"
  },
  "caching": "ReadWrite",
  "createOption": "FromImage"
}
```

Po wdrożeniu można wyświetlić konta miejsca do magazynowania w portalu Azure.

![Konto miejsca do magazynowania](./media/virtual-machines-windows-dotnet-core/storacct-win.png)

Kliknięcie w kontenerze obiektów blob konta miejsca do magazynowania, są widoczne pliku wirtualnego dysku twardego dla każdej maszyny wirtualnej wdrożone na podstawie tego szablonu.

![Wirtualna dyskach twardych](./media/virtual-machines-windows-dotnet-core/vhd-win.png)

Więcej informacji o magazyn Azure [Magazyn Azure](https://azure.microsoft.com/documentation/services/storage/)dokumentacji.

## <a name="virtual-network"></a>Wirtualna sieć

Jeśli maszyna wirtualna wymaga wewnętrznych sieci, takich jak możliwość komunikowania się z innymi maszyn wirtualnych i zasoby Azure, wirtualną sieć Azure jest wymagane.  Wirtualna sieć nie udostępnić maszyny wirtualnej w Internecie. Łączności publicznej w zakresie wymaga publiczny adres IP, który jest szczegółowe w dalszej części tej serii.

Wykonaj to łącze, aby wyświetlić przykładowe JSON w szablonie Menedżera zasobów — [wirtualnej sieci i podsieci](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L126).

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/virtualNetworks",
  "name": "[variables('virtualNetworkName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Network/networkSecurityGroups/', variables('networkSecurityGroup'))]"
  ],
  "tags": {
    "displayName": "virtual-network"
  },
  "properties": {
    "addressSpace": {
      "addressPrefixes": [
        "10.0.0.0/24"
      ]
    },
    "subnets": [
      {
        "name": "[variables('subnetName')]",
        "properties": {
          "addressPrefix": "10.0.0.0/24",
          "networkSecurityGroup": {
            "id": "[resourceId('Microsoft.Network/networkSecurityGroups', variables('networkSecurityGroup'))]"
          }
        }
      }
    ]
  }
}
```

Z portalu Azure wirtualną sieć wygląda na poniższej ilustracji. Zwróć uwagę, że wszystkie maszyn wirtualnych wdrożone na podstawie tego szablonu zostaną dołączone do wirtualnej sieci.

![Wirtualna sieć](./media/virtual-machines-windows-dotnet-core/vnet-win.png)

## <a name="network-interface"></a>Interfejs sieciowy

 Karty sieciowej łączy maszyny wirtualnej sieci wirtualnej, w szczególności do podsieci zdefiniowanej w wirtualnej sieci. 
 
 Wykonaj to łącze, aby wyświetlić przykładowe JSON w szablonie Menedżera zasobów — [Sieciowej](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L156).
 
```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/networkInterfaces",
  "name": "[concat(variables('networkInterfaceName'), copyindex())]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "network-interface"
  },
  "copy": {
    "name": "nicLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "dependsOn": [
    "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
    "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIpAddressName'))]",
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'), '/inboundNatRules/', 'RDP-VM', copyIndex())]"
  ],
  "properties": {
    "ipConfigurations": [
      {
        "name": "ipconfig",
        "properties": {
          "privateIPAllocationMethod": "Dynamic",
          "subnet": {
            "id": "[variables('subnetRef')]"
          },
          "loadBalancerBackendAddressPools": [
            {
              "id": "[variables('lbPoolID')]"
            }
          ],
          "loadBalancerInboundNatRules": [
            {
              "id": "[concat(variables('lbID'),'/inboundNatRules/RDP-VM', copyIndex())]"
            }
          ]
        }
      }
    ]
  }
}
```

Każdemu zasobowi maszyn wirtualnych zawiera profil sieci. Interfejs sieciowy jest skojarzony z maszyny wirtualnej w tym profilu.  

Wykonaj to łącze, aby wyświetlić przykładowe JSON w szablonie Menedżera zasobów — [Maszyn wirtualnych sieci profilu](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L330).


```none
"networkProfile": {
  "networkInterfaces": [
    {
      "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('networkInterfaceName'), copyindex()))]"
    }
  ]
}
```

Z portalu Azure interfejsu sieciowego wygląda tak: poniższy obraz. Wewnętrzny adres IP i skojarzenia maszyn wirtualnych mogą być widoczne na zasobu interfejsu sieciowego.

![Interfejs sieciowy](./media/virtual-machines-windows-dotnet-core/nic-win.png)

Więcej informacji o wirtualnych sieci Azure [Wirtualną sieć Azure](https://azure.microsoft.com/documentation/services/virtual-network/)dokumentacji.

## <a name="azure-sql-database"></a>Baza danych SQL Azure

Oprócz maszyny wirtualnej hostingu witryny sieci Web sklep muzyczny bazy danych SQL Azure zostanie wdrożony do obsługi sklep muzyczny bazy danych. Korzyścią ze stosowania bazy danych SQL Azure w tym miejscu jest drugi zestaw maszyn wirtualnych nie jest wymagane, — skalę i dostępność jest wbudowany w usłudze.

Bazy danych programu Azure SQL można dodawać przy użyciu programu Visual Studio Dodawanie nowego zasobu kreatora lub wstawiając prawidłowych JSON do szablonu. Zasób programu SQL Server zawiera nazwę użytkownika i hasło, którego udzielono uprawnień administratora na wystąpienie programu SQL. Ponadto zostanie dodany zasób zapory SQL. Domyślnie aplikacji znajdującej się w Azure mogą połączyć się z wystąpienie programu SQL. Aby umożliwić aplikacji zewnętrznej takie SQL Server Management studio nawiązywania połączenia z wystąpieniem programu SQL, zapory trzeba skonfigurować. Ze względu na pokaz sklep muzyczny konfiguracji domyślnej jest prawidłowy. 

Wykonaj to łącze, aby wyświetlić przykładowe JSON w szablonie Menedżera zasobów — [Bazy danych SQL Azure](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L379).


```none
{
  "apiVersion": "2014-04-01-preview",
  "type": "Microsoft.Sql/servers",
  "name": "[variables('musicstoresqlName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [],
  "tags": {
    "displayName": "sql-music-store"
  },
  "properties": {
    "administratorLogin": "[parameters('adminUsername')]",
    "administratorLoginPassword": "[parameters('adminPassword')]"
  },
  "resources": [
    {
      "apiVersion": "2014-04-01-preview",
      "type": "firewallrules",
      "name": "firewall-allow-azure",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Sql/servers/', variables('musicstoresqlName'))]"
      ],
      "properties": {
        "startIpAddress": "0.0.0.0",
        "endIpAddress": "0.0.0.0"
      }
    }
  ]
}
```

Widok programu SQL server i bazy danych MusicStore w Azure portal.

![Program SQL Server](./media/virtual-machines-windows-dotnet-core/sql-win.png)

Aby uzyskać więcej informacji na temat wdrażania bazy danych SQL Azure zobacz [dokumentację bazy danych SQL Azure](https://azure.microsoft.com/documentation/services/sql-database/).

## <a name="next-step"></a>Następny krok

<hr>

[Krok 2 — program Access i zabezpieczenia w szablonach Azure Menedżera zasobów](./virtual-machines-windows-dotnet-core-3-access-security.md)
