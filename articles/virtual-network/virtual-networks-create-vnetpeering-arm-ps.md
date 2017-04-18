<properties
   pageTitle="Tworzenie VNet zaglądanie przy użyciu poleceń cmdlet środowiska Powershell | Microsoft Azure"
   description="Dowiedz się, jak tworzyć wirtualną sieć za pomocą portalu Azure w Menedżerze zasobów."
   services="virtual-network"
   documentationCenter=""
   authors="NarayanAnnamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="narayanannamalai; annahar"/>

# <a name="create-vnet-peering-using-powershell-cmdlets"></a>Tworzenie VNet zaglądanie przy użyciu poleceń cmdlet programu Powershell

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Aby utworzyć VNet zaglądanie przy użyciu programu PowerShell, wykonaj poniższe czynności:

1. Jeśli po raz pierwszy używasz Azure programu PowerShell, zobacz, [jak instalowanie i konfigurowanie programu PowerShell Azure](../powershell-install-configure.md) i postępuj zgodnie z instrukcjami maksymalnie end, aby zalogować się do Azure i wybierz subskrypcję.

> [AZURE.NOTE] Polecenia cmdlet programu PowerShell do zarządzania zaglądanie VNet jest dołączona do programu [Azure programu PowerShell 1.6.](http://www.powershellgallery.com/packages/Azure/1.6.0)

2. Przeczytaj wirtualną sieć obiektów:

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1
        $vnet2 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet2

3. W celu ustalenia VNet zaglądanie, należy utworzyć dwa łącza, po jednej dla każdego kierunku. Następny krok utworzy łącze peering VNet dla VNet1 do VNet2 najpierw:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet2.Id

    Przedstawia wynik:

        Name            : LinkToVNet2
        Id: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Initiated
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic   : False
        AllowGatewayTransit : False
        UseRemoteGateways   : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

4. W tym kroku utworzy łącza peering VNet dla VNet2 do VNet1:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet1 -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet1.Id

    Przedstawia wynik:

        Name            : LinkToVNet1
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2/virtualNetworkPeerings/LinkToVNet1
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet2
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic   : False
        AllowGatewayTransit : False
        UseRemoteGateways   : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

5. Po utworzeniu łącza peering VNet można wyświetlić stanu łącza poniżej:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name linktovnet2

    Przedstawia wynik:

        Name            : LinkToVNet2
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                             "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

    Istnieje kilka można konfigurować właściwości zaglądanie VNet:

  	|Opcja|Opis|Domyślne|
  	|:-----|:----------|:------|
  	|AllowVirtualNetworkAccess|Czy adres miejsca VNet równorzędnych mają zostać uwzględnione znacznika Virtual_network|Tak|
  	|AllowForwardedTraffic|Czy zaakceptowane lub odrzucone ruch nie pochodzących z peered VNet|Brak|
  	|AllowGatewayTransit|Umożliwia równorzędnych VNet używać Centrum VNet|Brak|
  	|UseRemoteGateways|Za pomocą usługi równorzędnych VNet bramy. Funkcja VNet musi zawierać bramy skonfigurowane i AllowGatewayTransit zaznaczone. Nie można użyć tej opcji, jeśli masz skonfigurowane bramy|Brak|

    Każde z łączy w zaglądanie VNet zawiera zestaw właściwości powyżej. Można na przykład AllowVirtualNetworkAccess Aby ustaw wartość True dla łącza zaglądanie VNet VNet1 VNet2 i nadaj mu wartość False łącza peering VNet w kierunku przeciwnym.

        $LinktoVNet2 = Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name LinkToVNet2
        $LinktoVNet2.AllowForwardedTraffic = $true
        Set-AzureRmVirtualNetworkPeering -VirtualNetworkPeering $LinktoVNet2

    Można uruchamiać Get-AzureRmVirtualNetworkPeering do wyboru w podwójny wartość właściwości po zmianie. Z zestawu wyników możesz sprawdzić, czy AllowForwardedTraffic zmian ustaw wartość true po uruchomieniu powyżej poleceń cmdlet.

        Name            : LinkToVNet2
        Id          : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic       : True
        AllowGatewayTransit     : False
        UseRemoteGateways       : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

    Po zaglądanie w tym scenariuszu należy inicjowania połączeń z maszyn wirtualnych do maszyn wirtualnych obu VNets. Domyślnie AllowVirtualNetworkAccess ma wartość PRAWDA, a zaglądanie VNet będzie obsługi administracyjnej pisane z wielkiej litery ACL umożliwienie komunikacji między VNets. Można stosować zasad grupy (NSG) zabezpieczeń sieci do blokowania łączność między określonej podsieci lub maszyn wirtualnych przejęcie kontroli cienkie ziarno dostępu między dwoma wirtualnych sieci.  Więcej informacji na temat tworzenia reguł NSG można znaleźć w tym [artykule](virtual-networks-create-nsg-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

Aby utworzyć VNet zaglądanie w subskrypcjach przy użyciu programu PowerShell, wykonaj poniższe czynności:

1. Zaloguj się do Azure o uprawnieniach użytkownika A jest konto w usłudze A subskrypcji i uruchom następujące polecenie cmdlet:

        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet5

    Nie jest to wymagane zaglądanie można ustalić nawet w przypadku użytkowników pojedynczo podnieść zaglądanie żądania dla ich odpowiednich VNets, ile żądania zgodne. Dodawanie użytkownikiem innego VNet jako użytkownika w lokalnym VNet ułatwia wykonaj konfiguracji.

2. Logowanie się do Azure za pomocą konta uprzywilejowanych użytkownika-B dla subskrypcji-B i uruchom następujące polecenie cmdlet:

        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet3

3. W odpowiedzi użytkownika jest sesji logowania, uruchom polecenie cmdlet poniżej:

        $vnet3 = Get-AzureRmVirtualNetwork -ResourceGroupName hr-vnets -Name vnet3

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet5 -VirtualNetwork $vnet3 -RemoteVirtualNetworkId "/subscriptions/<Subscription-B-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet5" -BlockVirtualNetworkAccess

4. W sesji logowania użytkownika B Uruchom polecenie cmdlet poniżej:

        $vnet5 = Get-AzureRmVirtualNetwork -ResourceGroupName vendor-vnets -Name vnet5

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet3 -VirtualNetwork $vnet5 -RemoteVirtualNetworkId "/subscriptions/<Subscriptoin-A-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet3" -BlockVirtualNetworkAccess

5. Po ustaleniu zaglądanie maszyn wirtualnych w VNet3 powinno być możliwe komunikowanie się za pomocą maszyn wirtualnych w VNet5.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. W tym scenariuszu można uruchamiać poniżej, aby ustalić zaglądanie VNet poleceń cmdlet programu PowerShell.  Należy ustawić właściwość AllowForwardedTraffic na PRAWDA i połączyć VNET1 HubVNet, dzięki czemu ruch przychodzący z poza peering przestrzeni adresów VNet.

        $hubVNet = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name HubVNet
        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1

        Add-AzureRmVirtualNetworkPeering -Name LinkToHub -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $HubVNet.Id -AllowForwardedTraffic

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet1 -VirtualNetwork $HubVNet -RemoteVirtualNetworkId $vnet1.Id

2. Po ustaleniu zaglądanie można można znaleźć w tym [artykule](virtual-network-create-udr-arm-ps.md) i zdefiniować rozsyłania (UDR), aby przekierowywać ruch VNet1 za pośrednictwem wirtualnych urządzenia, aby używać funkcji zdefiniowanych przez użytkownika. Po określeniu adres następnego przeskoku w trasę można ustawić adres IP wirtualnego urządzenia w konwersacjach VNet HubVNet. Poniżej przedstawiono przykładowy:

        $route = New-AzureRmRouteConfig -Name TestNVA -AddressPrefix 10.3.0.0/16 -NextHopType VirtualAppliance -NextHopIpAddress 192.0.1.5

        $routeTable = New-AzureRmRouteTable -ResourceGroupName VNet101 -Location brazilsouth -Name TestRT -Route $route

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName VNet101 -Name VNet1

        Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet1 -Name subnet-1 -AddressPrefix 10.1.1.0/24 -RouteTable $routeTable

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet1

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

Aby utworzyć VNet zaglądanie między klasyczny wirtualnej sieci i Menedżera zasobów Azure wirtualnych sieci w programie PowerShell, wykonaj następujące czynności:

1. Przeczytaj obiektu wirtualną sieć dla **VNET1**, wirtualną sieć Menedżera zasobów Azure w następujący sposób:

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1

2. W celu ustalenia VNet zaglądanie w tym scenariuszu, potrzebna jest tylko jedno łącze, w szczególności łącze z **VNET1** do **VNET2**. Ten krok wymaga znajomości VNet klasyczny identyfikator zasobu. Format identyfikator grupy zasobów wygląda:

        /subscriptions/{SubscriptionID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.ClassicNetwork/virtualNetworks/{VirtualNetworkName}

    Pamiętaj zastąpić odpowiednie nazwy SubscriptionID, ResourceGroupName i VirtualNetworkName.

    Można to osiągnąć przez następujące czynności:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2

3. Po VNet zaglądanie łącze zostanie utworzona, można wyświetlić stanu łącza przedstawione poniżej wynik:

        Name                             : LinkToVNet2
        Id                               : /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeerings/LinkToVNet2
        Etag                             : W/"acecbd0f-766c-46be-aa7e-d03e41c46b16"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                         "Id": "/subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                       }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

## <a name="remove-vnet-peering"></a>Usuwanie zaglądanie VNet

1.  Aby usunąć VNet zaglądanie, należy uruchomić następujące polecenie cmdlet:

        Remove-AzureRmVirtualNetworkPeering  

        Remove both links, using the following commands:

        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2
        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2

2. Po usunięciu jedno łącze w zaglądanie VNET stanu łącza równorzędnych przejdzie do Rozłączono. W tym stanie nie można ponownie utworzyć łącze stanu łącza równorzędnych przybiera zainicjowane. Zalecamy usunięcie łącza przed ponownie utworzyć zaglądanie VNet.
