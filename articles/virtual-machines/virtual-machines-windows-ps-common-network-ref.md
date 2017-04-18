<properties
    pageTitle="Polecenia siecią programu PowerShell dla maszyny wirtualne | Microsoft Azure"
    description="Typowe polecenia programu PowerShell ułatwiające pracę, tworzenie wirtualnej sieci i jego skojarzonych z nimi zasobów dla maszyny wirtualne."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="common-network-azure-powershell-commands-for-vms"></a>Typowe polecenia programu PowerShell Azure sieci do maszyny wirtualne

Jeśli chcesz utworzyć maszyny wirtualnej, musisz utworzyć [wirtualną sieć](../virtual-network/virtual-networks-overview.md) lub o istniejących wirtualnej sieci, w którym można dodać maszyn wirtualnych. Zazwyczaj podczas tworzenia maszyny należy rozważyć utworzenie zasoby opisane w tym artykule.

Zobacz, [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) dla informacji na temat instalowania najnowszej wersji programu PowerShell Azure, wybierając subskrypcji i logowanie się do swojego konta.

## <a name="create-network-resources"></a>Tworzenie zasobów

Zadanie | Polecenie 
-------------- | -------------------------
Tworzenie podsieci konfiguracji | $podsieć1 = [New AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt619412.aspx) -nazwa "subnet_name" - AddressPrefix XX. X.X.X/XX<BR>$podsieć2 = New AzureRmVirtualNetworkSubnetConfig-nazwa "subnet_name" - AddressPrefix XX. X.X.X/XX<BR><BR>Typowej sieci może być podsieć dla [internet przeciwległych równoważenia obciążenia](../load-balancer/load-balancer-internet-overview.md) i osobną podsieć dla [równoważenia obciążenia wewnętrzny](../load-balancer/load-balancer-internal-overview.md). |
Tworzenie wirtualnych sieci | $vnet = [New AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx) -- ResourceGroupName "virtual_network_name" Nazwa "resource_group_name"-lokalizacji "location_name" - AddressPrefix XX. X.X.X/XX-podsieci $podsieć1, $podsieć2
Test dla nazwy domeny unikatowe | [Test AzureRmDnsAvailability](https://msdn.microsoft.com/library/mt619419.aspx) - DomainQualifiedName "nazwa_domeny"-lokalizacji "location_name"<BR><BR>Można określić nazwę DNS domeny dla [publicznej zasobu IP](../virtual-network/virtual-network-ip-addresses-overview-arm.md), który tworzy mapowanie domainname.location.cloudapp.azure.com publiczny adres IP na serwerach DNS zarządzane Azure. Nazwa może zawierać tylko litery, cyfry i łączniki. Pierwszy i ostatni znak musi być literą lub numer i nazwa domeny musi być unikatowa w lokalizacji Azure. Jeśli zostanie zwrócona wartość **PRAWDA** , nazwę proponowana jest globalnie unikatowa.
Tworzenie publiczny adres IP | $pip = [AzureRmPublicIpAddress nowy](https://msdn.microsoft.com/library/mt603620.aspx) -nazwa_domeny"- ResourceGroupName"resource_group_name"- DomainNameLabel" Nazwa "ip_address_name"-lokalizacji "location_name" - AllocationMethod dynamiczne<BR><BR>Publiczny adres IP używa nazwy domeny, które wcześniej przetestowane i jest używany przez konfiguracji serwera sieci Web usługi równoważenia obciążenia.
Tworzenie konfiguracji IP serwera sieci Web | $frontendIP = [New AzureRmLoadBalancerFrontendIpConfig](https://msdn.microsoft.com/library/mt603510.aspx) -nazwa "frontend_ip_name" - PublicIpAddress $pip<BR><BR>Konfiguracja serwera sieci Web obejmuje publiczny adres IP, utworzonego wcześniej dla przychodzącego ruchu sieciowego.
Tworzenie puli adres wewnętrznej bazy danych | $beAddressPool = [New AzureRmLoadBalancerBackendAddressPoolConfig](https://msdn.microsoft.com/library/mt603791.aspx) -nazwa "backend_pool_name"<BR><BR>Zapewnia adresów wewnętrznej bazy danych z usługi równoważenia obciążenia, które są dostępne za pośrednictwem karty sieciowej.
Tworzenie sondy | $healthProbe = [AzureRmLoadBalancerProbeConfig nowy](https://msdn.microsoft.com/library/mt603847.aspx) -nazwa "probe_name" - RequestPath "HealthProbe.aspx"-protokołu http-Port 80 - IntervalInSeconds 15 - ProbeCount 2<BR><BR>Zawiera sondy kondycji umożliwia sprawdzanie dostępności wystąpień maszyn wirtualnych w puli adresów wewnętrznej bazy danych.
Tworzenie reguły równoważenia obciążenia | $lbRule = [AzureRmLoadBalancerRuleConfig nowy](https://msdn.microsoft.com/library/mt619391.aspx) -HTTP nazwa - FrontendIpConfiguration $frontendIP - BackendAddressPool $beAddressPool-sondy $healthProbe-protokołu Tcp - FrontendPort 80 - BackendPort 80<BR><BR>Zawiera reguły, które przypisać publicznej portu do równoważenia obciążenia do portu w puli adresów wewnętrznej bazy danych.
Utwórz regułę ruchu przychodzącego translatora adresów Sieciowych | $inboundNATRule = [AzureRmLoadBalancerInboundNatRuleConfig nowy](https://msdn.microsoft.com/library/mt603606.aspx) -nazwa "rule_name" - FrontendIpConfiguration $frontendIP-3441 protokołu TCP - FrontendPort - BackendPort 3389<BR><BR>Zawiera reguły mapowanie portem publicznej usługi równoważenia obciążenia na port dla określonej maszyny wirtualnej w puli adresów wewnętrznej bazy danych.
Tworzenie równoważenia obciążenia | $loadBalancer = [New AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt619450.aspx) - ResourceGroupName "resource_group_name"-Nazwa "load_balancer_name"-lokalizacji "location_name" - FrontendIpConfiguration $frontendIP - InboundNatRule $inboundNATRule - LoadBalancingRule $lbRule - BackendAddressPool $beAddressPool-sondy $healthProbe
Tworzenie karty sieciowej | $nic1 = [New AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx) - ResourceGroupName "resource_group_name"-Nazwa "network_interface_name"-lokalizacji "location_name" - PrivateIpAddress XX. X.X.X-$loadBalancer.BackendAddressPools[0 - LoadBalancerBackendAddressPool podsieć2 podsieci] - LoadBalancerInboundNatRule $loadBalancer.InboundNatRules[0]<BR><BR>Tworzenie karty sieciowej przy użyciu publiczny adres IP i podsieci wirtualnej sieci, uprzednio utworzony.
    
## <a name="get-information-about-network-resources"></a>Uzyskiwanie informacji na temat zasobów

Zadanie | Polecenie 
-------------- | -------------------------
Lista wirtualnych sieci | [Get-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603515.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Wyświetla listę wszystkich wirtualnych sieci w grupie zasobów.
Uzyskiwanie informacji na temat wirtualnej sieci | Get-AzureRmVirtualNetwork-- ResourceGroupName "virtual_network_name" Nazwa "resource_group_name"
Lista podsieci w wirtualnej sieci | Get AzureRmVirtualNetwork-nazwa "virtual_network_name" - ResourceGroupName "resource_group_name" & #124; Wybierz pozycję podsieci
Uzyskiwanie informacji na temat podsieci | [Get AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt603817.aspx) -nazwa "subnet_name" - VirtualNetwork $vnet<BR><BR>Pobiera informacje o podsieci w określonej wirtualnej sieci. Wartość $vnet reprezentuje obiekt zwrócony przez Get-AzureRmVirtualNetwork.
Adresy IP | [Get-AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt619342.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Wyświetla listę publicznych adresów IP w grupie zasobów.
Urządzenia do równoważenia obciążenia listy | [Get-AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt603668.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Wyświetla listę wszystkich równoważenia obciążenia w grupie zasobów.
Lista interfejsów | [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Wyświetla listę wszystkich interfejsów sieciowych w grupie zasobów.
Uzyskiwanie informacji na temat karty sieciowej | Get-AzureRmNetworkInterface-- ResourceGroupName "network_interface_name" Nazwa "resource_group_name"<BR><BR>Pobiera informacje o określonym sieciowej.
Uzyskiwanie konfiguracji IP karty sieciowej | [Get AzureRmNetworkInterfaceIPConfig](https://msdn.microsoft.com/library/mt732618.aspx) -nazwa "ipconfiguration_name" - NetworkInterface $nic<BR><BR>Pobiera informacje o konfiguracji IP interfejsu określonej sieci. Wartość $nic reprezentuje obiekt zwrócony przez Get-AzureRmNetworkInterface.

## <a name="manage-network-resources"></a>Zarządzanie zasobami sieci

Zadanie | Polecenie 
-------------- | -------------------------
Dodawanie podsieci do wirtualnej sieci | [Dodawanie AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt603722.aspx) - AddressPrefix XX. X.X.X/XX-nazwa "subnet_name" - VirtualNetwork $vnet<BR><BR>Dodaje podsieć do istniejącej sieci wirtualnej. Wartość $vnet reprezentuje obiekt zwrócony przez Get-AzureRmVirtualNetwork.
Usuwanie wirtualnej sieci | [Usuń AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt619338.aspx) -- ResourceGroupName "virtual_network_name" Nazwa "resource_group_name"<BR><BR>Usuwa określoną wirtualnej sieci z grupy zasobów.
Usuwanie karty sieciowej | [Usuń AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt603836.aspx) -- ResourceGroupName "network_interface_name" Nazwa "resource_group_name"<BR><BR>Usuwa określoną sieciowej z grupy zasobów.
Usuwanie usługi równoważenia obciążenia | [Usuń AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt603862.aspx) -- ResourceGroupName "load_balancer_name" Nazwa "resource_group_name"<BR><BR>Usuwa równoważenia obciążenia określonego z grupy zasobów.
Usuwanie publiczny adres IP | [Usuń AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt619352.aspx)-- ResourceGroupName "ip_address_name" Nazwa "resource_group_name"<BR><BR>Usuwa określony publiczny adres IP z grupy zasobów.

## <a name="next-steps"></a>Następne kroki

- Za pomocą interfejsu sieci właśnie utworzony podczas [tworzenia maszyn wirtualnych](virtual-machines-windows-ps-create.md).
- Dowiedz się, jak możesz [utworzyć maszyn wirtualnych z wielu interfejsów](../virtual-network/virtual-networks-multiple-nics.md).
