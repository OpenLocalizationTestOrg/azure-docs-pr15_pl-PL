<properties
   pageTitle="Otwórz porty do maszyny przy użyciu programu PowerShell | Microsoft Azure"
   description="Dowiedz się, jak otworzyć port / Tworzenie punktu końcowego do swojego maszyn wirtualnych systemu Windows za pomocą trybie wdrożenia Menedżera zasobów Azure i Azure programu PowerShell"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="opening-ports-and-endpoints-to-a-vm-in-azure-using-powershell"></a>Otwieranie portów i punktów końcowych do maszyn wirtualnych w Azure przy użyciu programu PowerShell
[AZURE.INCLUDE [virtual-machines-common-nsg-quickstart](../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Szybkie polecenia
Aby utworzyć grupę zabezpieczeń sieci i list ACL reguły potrzebujesz [najnowszą wersję programu PowerShell Azure zainstalowany](../powershell-install-configure.md). Możesz również [wykonać poniższe czynności za pomocą portalu Azure](virtual-machines-windows-nsg-quickstart-portal.md).

Zaloguj się do konta Azure:

```powershell
Login-AzureRmAccount
```

W poniższych przykładach należy zastąpić przykładowe nazwy parametrów własne wartości. Przykład nazwy parametrów dostępnych `myResourceGroup`, `myNetworkSecurityGroup`, i `myVnet`.

Tworzenie reguły. Poniższy przykład tworzy reguły o nazwie `myNetworkSecurityGroupRule` umożliwia ruch TCP na porcie 80:

```powershell
$httprule = New-AzureRmNetworkSecurityRuleConfig -Name "myNetworkSecurityGroupRule" `
    -Description "Allow HTTP" -Access "Allow" -Protocol "Tcp" -Direction "Inbound" `
    -Priority "100" -SourceAddressPrefix "Internet" -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 80
```

Następnie utwórz grupę zabezpieczeń sieci i przypisać zasady HTTP właśnie utworzonego w następujący sposób. Poniższy przykład tworzy grupę zabezpieczeń sieci o nazwie `myNetworkSecurityGroup`:

```powershell
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNetworkSecurityGroup" -SecurityRules $httprule
```

Teraz przejdźmy do podsieci Przypisywanie grupy zabezpieczeń sieci. Poniższy przykład przypisuje istniejących wirtualną sieć o nazwie `myVnet` do zmiennej `$vnet`:

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
    -Name "myVnet"
```

Grupy zabezpieczeń sieci skojarzyć z danej podsieci. Poniższy przykład kojarzy podsieci o nazwie `mySubnet` z grupą zabezpieczeń sieci:

```powershell
$subnetPrefix = $vnet.Subnets|?{$_.Name -eq 'mySubnet'}

Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "mySubnet" `
    -AddressPrefix $subnetPrefix.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

Na koniec aktualizacji sieci wirtualnych, aby zmiany zostały wprowadzone:

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```


## <a name="more-information-on-network-security-groups"></a>Więcej informacji na temat grup zabezpieczeń sieci
Szybkie polecenia tutaj umożliwiają rozpocząć pracę z ruchem ułożony do swojego maszyn wirtualnych. Grupy zabezpieczeń sieci stanowią wielu doskonałe funkcje i dokładności do kontrolowania dostępu do zasobów. Więcej informacji o [tworzeniu reguł ACL tutaj i grupy zabezpieczeń sieci](../virtual-network/virtual-networks-create-nsg-arm-ps.md).

Można zdefiniować grupy zabezpieczeń sieci i reguły ACL jako część szablony Azure Menedżera zasobów. Dowiedz się więcej o [tworzeniu grup zabezpieczeń sieci przy użyciu szablonów](../virtual-network/virtual-networks-create-nsg-arm-template.md).

Jeśli musisz użyć przekierowania portów mapowania unikatowe portu zewnętrznego do wewnętrznego portu usługi maszyn wirtualnych, za pomocą usługi równoważenia obciążenia i reguły tłumaczenia adresów sieciowych (NAT). Na przykład można udostępnić port TCP 8080 zewnętrznie i mieć ruch kierowany do port TCP 80 na maszyny. Aby Dowiedz się więcej o [tworzeniu równoważenia obciążenia w Internecie](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="next-steps"></a>Następne kroki
W tym przykładzie utworzono proste reguły w celu zezwalanie na ruch HTTP. Możesz znaleźć informacji na temat tworzenia środowiska bardziej szczegółowe w następujących artykułach:

- [Omówienie Menedżera zasobów Azure](../azure-resource-manager/resource-group-overview.md)
- [Co to jest grupa zabezpieczeń sieci (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Omówienie Menedżera zasobów Azure urządzenia do równoważenia obciążenia](../load-balancer/load-balancer-arm.md)