<properties
   pageTitle="Tworzenie równoważenia obciążenia internetową dostępną w Menedżerze zasobów przy użyciu programu PowerShell | Microsoft Azure"
   description="Dowiedz się, jak utworzyć równoważenia obciążenia internetową dostępną w Menedżerze zasobów przy użyciu programu PowerShell"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
  ms.service="load-balancer"
  ms.devlang="na"
  ms.topic="get-started-article"
  ms.tgt_pltfrm="na"
  ms.workload="infrastructure-services"
   ms.date="10/24/2016"
  ms.author="sewhee" />

# <a name="get-started"></a>Tworzenie równoważenia obciążenia internetową dostępną w Menedżerze zasobów przy użyciu programu PowerShell

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]W tym artykule opisano, jak model wdrożenia Menedżera zasobów. Można także [dowiedzieć się, jak utworzyć równoważenia obciążenia z Internetu przy użyciu modelu Klasyczny wdrożenia](load-balancer-get-started-internet-classic-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-by-using-azure-powershell"></a>Wdrożenie rozwiązania przy użyciu programu PowerShell Azure

Poniższych procedurach opisano sposób tworzenia równoważenia obciążenia z Internetu za pomocą Menedżera zasobów Azure za pomocą programu PowerShell. Przy użyciu Menedżera zasobów Azure każdego zasobu jest utworzony i skonfigurowany pojedynczo, a następnie umieść pogrupowane w celu utworzenia równoważenia obciążenia.

Należy utworzyć i skonfigurować następujące obiekty do wdrożenia usługi równoważenia obciążenia:

- Konfiguracja IP frontonu: zawiera publicznych adresów IP (PIP) dla przychodzących ruchu sieciowego.
- Pula adresów wewnętrznej: zawiera interfejsów sieciowych (NIC) dla maszyn wirtualnych otrzymywać ruch sieciowy z usługi równoważenia obciążenia.
- Zasady równoważenia obciążenia: zawiera reguły, które mapowanie portem publicznej usługi równoważenia obciążenia do portu w puli adresów wewnętrznej.
- Translatora adresów Sieciowych reguły przychodzące: zawiera reguły, które mapowanie portem publicznej usługi równoważenia obciążenia na port dla określonej maszyny wirtualnej w puli adresów wewnętrznej.
- Sondy: zawiera sondy kondycji umożliwia sprawdzanie dostępności wystąpień maszyn wirtualnych w puli adresów wewnętrznej.

Aby uzyskać więcej informacji zobacz [Azure Menedżera zasobów pomocy technicznej dotyczącej usługi równoważenia obciążenia](load-balancer-arm.md).

## <a name="set-up-powershell-to-use-resource-manager"></a>Konfigurowanie programu PowerShell za pomocą Menedżera zasobów

Upewnij się, że masz najnowszą wersję produkcji modułu Azure Menedżera zasobów dla programu PowerShell:

1. Zaloguj się do Azure.

        Login-AzureRmAccount

    Wprowadź poświadczenia po wyświetleniu monitu.

2. Sprawdzanie subskrypcji dla tego konta.

        Get-AzureRmSubscription

3. Wybranie Azure subskrypcji korzystać.

        Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'

4. Tworzenie grupy zasobów. (Pomiń ten krok, jeśli korzystasz z istniejącej grupy zasobów).

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Tworzenie wirtualnych sieci i publiczny adres IP zewnętrzną puli adresów IP

1. Tworzenie podsieci i wirtualnej sieci.

        $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
        New-AzureRmvirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

2. Tworzenie Azure publicznej zasób adres IP, o nazwie **PublicIP**może być używany przez zewnętrzną pulę adresów IP przy użyciu nazw DNS **loadbalancernrp.westus.cloudapp.azure.com**. Następujące polecenie używa typu statyczne alokacji.

        $publicIP = New-AzureRmPublicIpAddress -Name PublicIp -ResourceGroupName NRP-RG -Location 'West US' –AllocationMethod Static -DomainNameLabel loadbalancernrp

    >[AZURE.IMPORTANT]Usługi równoważenia obciążenia używa etykieta domeny publiczny adres IP jako prefiksu nazwy FQDN. To różni się od modelu Klasyczny wdrożenia, który używa usługi w chmurze jako równoważenia obciążenia FQDN.
    >W tym przykładzie nazwy FQDN jest **loadbalancernrp.westus.cloudapp.azure.com**.

## <a name="create-a-front-end-ip-pool-and-a-back-end-address-pool"></a>Tworzenie puli zewnętrzną IP i puli adresów wewnętrznej

1. Tworzenie puli adresów IP zewnętrzną o nazwie **Frontend kg** , który korzysta z zasobu **PublicIp** .

        $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PublicIpAddress $publicIP

2. Tworzenie puli adres wewnętrznej o nazwie **wewnętrznej bazy danych kg**.

        $beaddresspool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name LB-backend

## <a name="create-nat-rules-a-load-balancer-rule-a-probe-and-a-load-balancer"></a>Tworzenie reguł translatora adresów Sieciowych, reguły równoważenia obciążenia, sonda i równoważenia obciążenia

W tym przykładzie tworzy następujące elementy:

- Aby przetłumaczyć cały ruch przychodzący na porcie 3441 do portu 3389 reguły translatora adresów Sieciowych
- Aby przetłumaczyć cały ruch przychodzący na porcie 3442 do portu 3389 reguły translatora adresów Sieciowych
- Reguła sondy, aby sprawdzić stan kondycji na stronie o nazwie **HealthProbe.aspx**
- Reguły równoważenia obciążenia do równoważenia cały ruch przychodzący na porcie 80 do portu 80 na adresy w puli wewnętrznej
- Usługi równoważenia obciążenia, która korzysta z tych obiektów

Wykonaj następujące kroki:

1. Tworzenie reguł translatora adresów Sieciowych.

        $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP1 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

        $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP2 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

2. Tworzenie sondy kondycji. Istnieją dwa sposoby konfigurowania sondy:

    Sonda HTTP

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    Sonda TCP

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2

3. Tworzenie reguły równoważenia obciążenia.

        $lbrule = New-AzureRmLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $frontendIP -BackendAddressPool  $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80

4. Tworzenie równoważenia obciążenia przy użyciu poprzednio utworzone obiekty.

        $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name NRP-LB -Location 'West US' -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe

## <a name="create-nics"></a>Tworzenie nic

Tworzenie interfejsów (lub modyfikować istniejące) i kojarzenie ich reguły, zasady równoważenia obciążenia i sondy translatora adresów Sieciowych:

1. Uzyskaj wirtualnej sieci i podsieci wirtualnej sieci, gdzie sieciowe muszą zostać utworzone.

        $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
        $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet

2. Tworzenie NIC o nazwie **można nic1 kg**i skojarzyć ją z pierwszą regułę translatora adresów Sieciowych i puli adresów wewnętrznej pierwszy (i nie tylko).

        $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic1-be -Location 'West US' -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]

3. Tworzenie NIC o nazwie **można nic2 kg**i skojarzyć ją z drugą regułę translatora adresów Sieciowych i puli adresów wewnętrznej pierwszy (i nie tylko).

        $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic2-be -Location 'West US' -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]

4. Sprawdź sieciowe.

        $backendnic1

    Oczekiwany wynik:

        Name                 : lb-nic1-be
        ResourceGroupName    : NRP-RG
        Location             : westus
        Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
        ResourceGuid         : 896cac4f-152a-40b9-b079-3e2201a5906e
        ProvisioningState    : Succeeded
        Tags                 :
        VirtualMachine       : null
        IpConfigurations     : [
                            {
                            "Name": "ipconfig1",
                            "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                            "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1",
                            "PrivateIpAddress": "10.0.2.6",
                            "PrivateIpAllocationMethod": "Static",
                            "Subnet": {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                            },
                            "ProvisioningState": "Succeeded",
                            "PrivateIpAddressVersion": "IPv4",
                            "PublicIpAddress": {
                                "Id": null
                            },
                            "LoadBalancerBackendAddressPools": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/backendAddressPools/LB-backend"
                                }
                            ],
                            "LoadBalancerInboundNatRules": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/inboundNatRules/RDP1"
                                }
                            ],
                            "Primary": true,
                            "ApplicationGatewayBackendAddressPools": []
                            }
                        ]
        DnsSettings          : {
                            "DnsServers": [],
                            "AppliedDnsServers": [],
                            "InternalDomainNameSuffix": "prcwibzcuvie5hnxav0yjks2cd.dx.internal.cloudapp.net"
                        }
        EnableIPForwarding   : False
        NetworkSecurityGroup : null
        Primary              :

5. Używanie `Add-AzureRmVMNetworkInterface` polecenia cmdlet, aby przypisać sieciowe do różnych maszyny wirtualne.

## <a name="create-a-virtual-machine"></a>Tworzenie maszyny wirtualnej

Aby uzyskać wskazówki na temat tworzenia maszyny wirtualnej i przypisywanie NIC zobacz [Tworzenie maszyn wirtualnych Azure przy użyciu programu PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md).

## <a name="add-the-network-interface-to-the-load-balancer"></a>Dodawanie interfejsu sieciowego do równoważenia obciążenia

1. Pobieranie równoważenia obciążenia z platformy Azure.

    Ładowanie zasobów równoważenia obciążenia do zmiennej (jeśli można jeszcze). Zmienna nosi nazwę **$lb**. Użyj tych samych nazwach z zasobu usługi równoważenia obciążenia utworzony wcześniej.

        $lb= get-azurermloadbalancer –name NRP-LB -resourcegroupname NRP-RG

2. Ładowanie konfiguracji wewnętrznej do zmiennej.

        $backend=Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb

3. Ładowanie utworzone sieciowej do zmiennej. Nazwa zmiennej jest **$nic**. Nazwa interfejsu sieci jest tym samym z poprzedniego przykładu.

        $nic =get-azurermnetworkinterface –name lb-nic1-be -resourcegroupname NRP-RG

4. Zmień konfigurację wewnętrznej na karcie sieciowej.

        $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend

5. Zapisz obiekt interfejsu sieciowego.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Po dodaniu karty sieciowej puli wewnętrznej równoważenia obciążenia rozpoczyna się odbierania ruchu sieciowego zgodnie z regułami równoważenia obciążenia dla tego zasobu usługi równoważenia obciążenia.

## <a name="update-an-existing-load-balancer"></a>Aktualizowanie istniejącego równoważenia obciążenia

1. Za pomocą usługi równoważenia obciążenia z poprzedniego przykładu, przypisz obiektu usługi równoważenia obciążenia do zmiennej **$slb** , używając `Get-AzureLoadBalancer`.

        $slb = get-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

2. W poniższym przykładzie można dodać regułę ruchu przychodzącego translatora adresów Sieciowych — przy użyciu port 81 w puli frontonu i 8181 puli wewnętrznej — istniejące usługi równoważenia obciążenia.

        $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol TCP

3. Zapisywanie nowej konfiguracji przy użyciu `Set-AzureLoadBalancer`.

        $slb | Set-AzureRmLoadBalancer

## <a name="remove-a-load-balancer"></a>Usuwanie usługi równoważenia obciążenia

Za pomocą polecenia `Remove-AzureLoadBalancer` do usunięcia równoważenia obciążenia poprzednio utworzone o nazwie **NRP kg** w grupie zasobów o nazwie **NRP RG**.

    Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

>[AZURE.NOTE] Można użyć opcjonalny przełącznik **-życie** unikać wiersz do usunięcia.

## <a name="next-steps"></a>Następne kroki

[Przed rozpoczęciem konfigurowania równoważenia obciążenia wewnętrznych](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurowanie trybu rozkładu równoważenia obciążenia](load-balancer-distribution-mode.md)

[Konfigurowanie ustawienia przekroczenia limitu czasu bezczynności protokołu TCP dla usługi równoważenia obciążenia](load-balancer-tcp-idle-timeout.md)
