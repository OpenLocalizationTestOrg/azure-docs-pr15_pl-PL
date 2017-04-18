<properties
   pageTitle="Program Access i zabezpieczenia w szablonach Menedżera zasobów Azure | Microsoft Azure" 
   description="Samouczek DotNet Core Azure maszyn wirtualnych"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/21/2016"
   ms.author="nepeters"/>

# <a name="access-and-security-in-azure-resource-manager-templates"></a>Program Access i zabezpieczenia w szablonach Azure Menedżera zasobów

Aplikacji znajdującej się w Azure prawdopodobnych muszą być dostęp z Internetu lub sieci VPN i połączenia rozsyłania Express z Azure. Z próbki aplikacji sklep muzyczny w witrynie sieci web jest udostępniana w Internecie za pomocą publicznego adresu IP. Ustanowioną uzyskiwania dostępu za pomocą połączenia z aplikacją i dostęp do samych zasobach maszyn wirtualnych powinny być zabezpieczone. Zabezpieczenia programu access jest dostarczany z grupy zabezpieczeń sieci. 

Ten dokument, szczegóły, jak aplikacji sklep muzyczny jest zabezpieczone w szablonie Menedżera zasobów Azure próbki. Wszystkie zależności i unikatowe konfiguracje są wyróżnione. Aby uzyskać najlepsze wyniki, wstępnie wdrożyć wystąpienie rozwiązanie do subskrypcji Azure i pracy wraz z szablonu Azure Menedżera zasobów. Zakończenie szablonu można znaleźć tutaj — [Wdrożenia sklepu muzyki na Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).


## <a name="public-ip-address"></a>Publiczny adres IP

Aby zapewnić publicznego dostępu do zasobu Azure, można używać publicznej zasobu adresu IP. Publiczny adres IP można skonfigurować przy użyciu adresu IP statyczną lub dynamiczną. Jeśli jest używany dynamicznego adresu, a maszyny wirtualnej zatrzymaniu i alokację, adresy zostanie usunięty. Po ponownym uruchomieniu komputera może być przypisana różnych publiczny adres IP. Aby uniemożliwić zmianę adresu IP, można zastrzeżonego adresu IP. 

Publiczny adres IP można dodawać do szablonu Menedżera zasobów Azure za pomocą programu Visual Studio Kreatora dodawania nowych zasobów lub wstawiając prawidłowych JSON do szablonu. 

Wykonaj to łącze, aby wyświetlić przykładowe JSON w szablonie Menedżera zasobów — [Publiczny adres IP](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L121).


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[variables('publicipaddressName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "public-ip-front"
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

Wykonaj to łącze, aby wyświetlić przykładowe JSON w szablonie Menedżera zasobów — [skojarzenia publiczny adres IP przy użyciu usługi równoważenia obciążenia](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L208).

```none
"frontendIPConfigurations": [
  {
    "properties": {
      "publicIPAddress": {
        "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicipaddressName'))]"
      }
    },
    "name": "LoadBalancerFrontend"
  }
]
```

Publiczny adres IP oglądana z portalu Azure. Zwróć uwagę, że publiczny adres IP jest skojarzony do równoważenia obciążenia i nie maszyny wirtualnej. Urządzenia do równoważenia obciążenia sieci wyszczególniono w dokumencie następnego tej serii.

![Publiczny adres IP](./media/virtual-machines-linux-dotnet-core/pubip.png)

Aby uzyskać więcej informacji na publicznych adresów IP Azure zobacz [adresy IP platformy Azure](../virtual-network/virtual-network-ip-addresses-overview-arm.md).

## <a name="network-security-group"></a>Grupa zabezpieczeń sieci

Po ustanowienia dostęp do zasobów Azure, dostęp powinien być ograniczony. Dla Azure maszyn wirtualnych bezpiecznego dostępu można to osiągnąć przy użyciu grupy zabezpieczeń sieci. Z próbki aplikacji sklep muzyczny dostęp do maszyny wirtualnej jest ograniczony z wyjątkiem przez port 80 dostępu http i port 22 dostępu SSH. Grupy zabezpieczeń sieci można dodawać do szablonu Menedżera zasobów Azure za pomocą programu Visual Studio Kreatora dodawania nowych zasobów lub wstawiając prawidłowych JSON do szablonu.

Wykonaj to łącze, aby wyświetlić przykładowe JSON w szablonie Menedżera zasobów — [Grupy zabezpieczeń sieci](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L68).

```none
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Network/networkSecurityGroups",
  "name": "[variables('nsgfront')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "nsg-front"
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
}
```

W tym przykładzie grupy zabezpieczeń sieci jest skojarzyć z obiektu podsieci zadeklarowane w zasobie wirtualnej sieci. 

Wykonaj to łącze, aby wyświetlić przykładowe JSON w szablonie Menedżera zasobów — [skojarzenia grupy zabezpieczeń sieci z wirtualnej sieci](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L158).


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
```

Poniżej przedstawiono, jak wygląda grupy zabezpieczeń sieci z portalu Azure. Należy zauważyć, że NSG można skojarzyć z interfejsem podsieci i / lub sieci. W tym przypadku NSG jest skojarzony podsieci. W tej konfiguracji reguły przychodzące dotyczą wszystkich maszyn wirtualnych podłączony do podsieci.

![Grupa zabezpieczeń sieci](./media/virtual-machines-linux-dotnet-core/nsg.png)

Aby uzyskać szczegółowe informacje dotyczące grup zabezpieczeń sieci zobacz [Co to jest grupa zabezpieczeń sieci]( https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/).

## <a name="next-step"></a>Następny krok

<hr>

[Krok 3 — dostępności i skali w Azure szablony Menedżera zasobów](./virtual-machines-linux-dotnet-core-4-availability-scale.md)
