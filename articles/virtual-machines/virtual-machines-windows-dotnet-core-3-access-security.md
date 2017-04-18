<properties
   pageTitle="Program Access i zabezpieczenia w szablonach Menedżera zasobów Azure | Microsoft Azure" 
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

# <a name="access-and-security-in-azure-resource-manager-templates"></a>Program Access i zabezpieczenia w szablonach Azure Menedżera zasobów

Aplikacji znajdującej się w Azure prawdopodobnych muszą być dostęp z Internetu lub sieci VPN i połączenia rozsyłania Express z Azure. Z próbki aplikacji sklep muzyczny w witrynie sieci web jest udostępniana w Internecie za pomocą publicznego adresu IP. Ustanowioną uzyskiwania dostępu za pomocą połączenia z aplikacją i dostęp do samych zasobach maszyn wirtualnych powinny być zabezpieczone. Zabezpieczenia programu access jest dostarczany z grupy zabezpieczeń sieci. 

Ten dokument, szczegóły, jak aplikacji sklep muzyczny jest zabezpieczone w szablonie Menedżera zasobów Azure próbki. Wszystkie zależności i unikatowe konfiguracje są wyróżnione. Aby uzyskać najlepsze wyniki, wstępnie wdrażać wystąpienie rozwiązanie do subskrypcji Azure i pracy wraz z szablonu Azure Menedżera zasobów. Zakończenie szablonu można znaleźć tutaj — [Wdrożenia sklepu muzyki w systemie Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).


## <a name="public-ip-address"></a>Publiczny adres IP

Aby zapewnić publicznego dostępu do zasobu Azure, można używać publicznej zasobu adresu IP. Publiczny adres IP można skonfigurować przy użyciu adresu IP statyczną lub dynamiczną. Jeśli jest używany dynamicznego adresu, a maszyny wirtualnej zatrzymaniu i alokację, adresy zostanie usunięty. Po ponownym uruchomieniu komputera może być przypisana różnych publiczny adres IP. Aby uniemożliwić zmianę adresu IP, można zastrzeżonego adresu IP. 

Publiczny adres IP można dodawać do szablonu Menedżera zasobów Azure za pomocą programu Visual Studio Kreatora dodawania nowych zasobów lub wstawiając prawidłowych JSON do szablonu. 

Wykonaj to łącze, aby wyświetlić przykładowe JSON w szablonie Menedżera zasobów — [Publiczny adres IP](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L110).


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[variables('publicIpAddressName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [],
  "tags": {
    "displayName": "public-ip"
  },
  "properties": {
    "publicIPAllocationMethod": "Dynamic",
    "dnsSettings": {
      "domainNameLabel": "[parameters('publicipaddressDnsName')]"
    }
  }
}
```

Publiczny adres IP może być skojarzone z wirtualną sieciową lub równoważenia obciążenia. W tym przykładzie ponieważ w witrynie sieci Web sklep muzyczny jest równoważone w kilku maszyn wirtualnych, publiczny adres IP jest dołączony do usługi równoważenia obciążenia.

Wykonaj to łącze, aby wyświetlić przykładowe JSON w szablonie Menedżera zasobów — [skojarzenia publiczny adres IP przy użyciu usługi równoważenia obciążenia](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L211).

```none
"frontendIPConfigurations": [
  {
    "properties": {
      "publicIPAddress": {
        "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIpAddressName'))]"
      }
    },
    "name": "LoadBalancerFrontend"
  }
]
```

Publiczny adres IP oglądana z portalu Azure. Zwróć uwagę, że publiczny adres IP jest skojarzony do równoważenia obciążenia i nie maszyny wirtualnej. Urządzenia do równoważenia obciążenia sieci wyszczególniono w dokumencie następnego tej serii.

![Publiczny adres IP](./media/virtual-machines-windows-dotnet-core/pubip-win.png)

Aby uzyskać więcej informacji na publicznych adresów IP Azure zobacz [adresy IP platformy Azure](../virtual-network/virtual-network-ip-addresses-overview-arm.md).

## <a name="network-security-group"></a>Grupa zabezpieczeń sieci

Po ustanowienia dostęp do zasobów Azure, dostęp powinien być ograniczony. Dla Azure maszyn wirtualnych bezpiecznego dostępu można to osiągnąć przy użyciu grupy zabezpieczeń sieci. Z próbki aplikacji sklep muzyczny dostęp do maszyny wirtualnej jest ograniczony z wyjątkiem na porcie 80 dostępu http, a portu 3389 dostępu RDP. Grupy zabezpieczeń sieci można dodawać do szablonu Menedżera zasobów Azure za pomocą programu Visual Studio Kreatora dodawania nowych zasobów lub wstawiając prawidłowych JSON do szablonu.

Wykonaj to łącze, aby wyświetlić przykładowe JSON w szablonie Menedżera zasobów — [Grupy zabezpieczeń sieci](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L57).

```none
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Network/networkSecurityGroups",
  "name": "[variables('networkSecurityGroup')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "network-security-group"
  },
  "properties": {
    "securityRules": [
      {
        "name": "http",
        "properties": {
          "description": "http endpoint",
          "protocol": "Tcp",
          "sourcePortRange": "*",
          "destinationPortRange": "80",
          "sourceAddressPrefix": "*",
          "destinationAddressPrefix": "*",
          "access": "Allow",
          "priority": 100,
          "direction": "Inbound"
        }
      },
      ........<truncated> 
    ]
  }
},
```

W tym przykładzie grupy zabezpieczeń sieci jest skojarzyć z obiektu podsieci zadeklarowane w zasobie wirtualnej sieci. 

Wykonaj to łącze, aby wyświetlić przykładowe JSON w szablonie Menedżera zasobów — [skojarzenia grupy zabezpieczeń sieci z wirtualnej sieci](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L143).


```none
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
```

Poniżej przedstawiono, jak wygląda grupy zabezpieczeń sieci z portalu Azure. Należy zauważyć, że NSG można skojarzyć z interfejsem podsieci i / lub sieci. W tym przypadku NSG jest skojarzony podsieci. W tej konfiguracji reguły przychodzące dotyczą wszystkich maszyn wirtualnych podłączony do podsieci.

![Grupa zabezpieczeń sieci](./media/virtual-machines-windows-dotnet-core/nsg-win.png)

Aby uzyskać szczegółowe informacje dotyczące grup zabezpieczeń sieci zobacz [Co to jest grupa zabezpieczeń sieci]( https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/).

## <a name="next-step"></a>Następny krok

<hr>

[Krok 3 — dostępności i skali w Azure szablony Menedżera zasobów](./virtual-machines-windows-dotnet-core-4-availability-scale.md)
