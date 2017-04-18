<properties
    pageTitle="Tworzenie internetowej przeciwległych równoważenia obciążenia, z protokołu IPv6 przy użyciu programu PowerShell dla Menedżera zasobów | Microsoft Azure"
    description="Dowiedz się, jak utworzyć internetową przeciwległych równoważenia obciążenia, z protokołu IPv6 przy użyciu programu PowerShell dla Menedżera zasobów"
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager"
    keywords="Protokół IPv6, równoważenia obciążenia azure, podwójne stos, publiczny adres ip, natywny protokół ipv6, mobile, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="get-started-creating-an-internet-facing-load-balancer-with-ipv6-using-powershell-for-resource-manager"></a>Wprowadzenie do tworzenia internetowej przeciwległych równoważenia obciążenia, z protokołu IPv6 przy użyciu programu PowerShell dla Menedżera zasobów

> [AZURE.SELECTOR]
- [Programu PowerShell](./load-balancer-ipv6-internet-ps.md)
- [Polecenie Azure](./load-balancer-ipv6-internet-cli.md)
- [Szablon](./load-balancer-ipv6-internet-template.md)

Usługi równoważenia obciążenia Azure jest równoważenia obciążenia warstwy-4 (port TCP, UDP). Usługi równoważenia obciążenia zapewnia wysoką dostępność przekazując ruch przychodzący między wystąpień prawidłowy usługi w chmurze usług lub maszyn wirtualnych w zestawie równoważenia obciążenia. Azure równoważenia obciążenia można także przedstawić tych usług na wiele portów i/lub wiele adresów IP.

## <a name="example-deployment-scenario"></a>Przykładowy scenariusz wdrażania

Na poniższym diagramie przedstawiono rozwiązanie wdrażania w tym artykule równoważenia obciążenia.

![Scenariusz równoważenia obciążenia](./media/load-balancer-ipv6-internet-ps/lb-ipv6-scenario.png)

W tym scenariuszu utworzysz Azure następujące zasoby:

- usługi równoważenia obciążenia z Internetu przy użyciu protokołu IPv4 i IPv6 publiczny adres IP
- dwa ładowanie równoważenia reguły do zamapować publicznej służącą do prywatnych punktów końcowych
- Dostępność grupy zgodnej zawiera dwa maszyny wirtualne
- dwa wirtualnych maszyn
- wirtualna sieciowej dla każdego maszyn wirtualnych z adresami IPv4 i IPv6 przypisane

## <a name="deploying-the-solution-using-the-azure-powershell"></a>Wdrożenie rozwiązania przy użyciu programu PowerShell Azure

Poniższe kroki pokazują sposób tworzenia internetowej przeciwległych równoważenia obciążenia Menedżera zasobów Azure za pomocą programu PowerShell. Przy użyciu Menedżera zasobów Azure każdego zasobu zostanie utworzona i skonfigurowana pojedynczo, następnie umieść ze sobą, aby utworzyć zasób.

Aby wdrożyć usługi równoważenia obciążenia, możesz utworzyć i skonfigurować następujących obiektów:

- Zewnętrzną Konfiguracja IP — zawiera publicznych adresów IP dla ruchu sieci przychodzącego.
- Pula adresów wewnętrznej — zawiera interfejsów sieciowych (NIC) maszyn wirtualnych otrzymywać ruch sieciowy od usługi równoważenia obciążenia.
- Zasady równoważenia obciążenia - zawiera reguły mapowania portem publicznej usługi równoważenia obciążenia do portu w puli adresów wewnętrznej.
- Translatora adresów Sieciowych reguły przychodzące — zawiera reguły mapowanie portem publicznej usługi równoważenia obciążenia na port dla określonej maszyny wirtualnej w puli adresów wewnętrznej.
- Sondy — zawiera sondy kondycji umożliwia sprawdzanie dostępności wystąpień maszyn wirtualnych w puli adresów wewnętrznej.

Aby uzyskać więcej informacji zobacz [Azure Menedżera zasobów pomocy technicznej dotyczącej usługi równoważenia obciążenia](load-balancer-arm.md).

## <a name="set-up-powershell-to-use-resource-manager"></a>Konfigurowanie programu PowerShell za pomocą Menedżera zasobów

Upewnij się, że masz najnowszą wersję produkcji modułu Azure Menedżera zasobów dla programu PowerShell.

1. Zaloguj się do Azure

        Login-AzureRmAccount

    Wprowadź poświadczenia po wyświetleniu monitu.

2. Sprawdzanie subskrypcji dla konta

        Get-AzureRmSubscription

3. Wybranie Azure subskrypcji korzystać.

        Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'

4. Tworzenie grupy zasobów (Pomiń ten krok, jeśli z istniejącej grupy zasobów)

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Tworzenie wirtualnych sieci i publiczny adres IP zewnętrzną puli adresów IP

1. Tworzenie wirtualnych sieci z podsieci.

        $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
        $vnet = New-AzureRmvirtualNetwork -Name VNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

2. Tworzenie Azure publiczny adres IP (PIP) zasobów zewnętrzną puli adresów IP.

        $publicIPv4 = New-AzureRmPublicIpAddress -Name 'pub-ipv4' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -IpAddressVersion IPv4 -DomainNameLabel lbnrpipv4
        $publicIPv6 = New-AzureRmPublicIpAddress -Name 'pub-ipv6' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Dynamic -IpAddressVersion IPv6 -DomainNameLabel lbnrpipv6

    >[AZURE.IMPORTANT] Usługi równoważenia obciążenia używa etykieta domeny publiczny adres IP jako prefiks nazwy FQDN. W tym przykładzie nazwy FQDN są *lbnrpipv4.westus.cloudapp.azure.com* i *lbnrpipv6.westus.cloudapp.azure.com*.

## <a name="create-a-front-end-ip-configurations-and-a-back-end-address-pool"></a>Tworzenie konfiguracji IP frontonu i puli adresów wewnętrznej

1. Tworzenie konfiguracji frontonu adresu, który korzysta z adresów publiczny adres IP, utworzony.

        $FEIPConfigv4 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv4" -PublicIpAddress $publicIPv4
        $FEIPConfigv6 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv6" -PublicIpAddress $publicIPv6

2. Tworzenie puli adresów wewnętrznej.

        $backendpoolipv4 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv4"
        $backendpoolipv6 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv6"


## <a name="create-lb-rules-nat-rules-a-probe-and-a-load-balancer"></a>Tworzenie kg reguły, translatora adresów Sieciowych reguły, sonda i równoważenia obciążenia

W tym przykładzie tworzy następujące elementy:

- Aby przetłumaczyć cały ruch przychodzący na porcie 443 do portu 4443 reguły translatora adresów Sieciowych
- Reguła równoważenia obciążenia do saldo cały ruch przychodzący na porcie 80 do portu 80 na adresy w puli wewnętrznej.
- Reguła równoważenia obciążenia umożliwia połączenie RDP maszyny wirtualne na porcie 3389.
- Reguła sondy, aby sprawdzić stan kondycji na stronie o nazwie *HealthProbe.aspx* lub usługi na porcie 8080
- usługi równoważenia obciążenia, która korzysta z tych obiektów

1. Tworzenie reguł translatora adresów Sieciowych.

        $inboundNATRule1v4 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev4" -FrontendIpConfiguration $FEIPConfigv4 -Protocol TCP -FrontendPort 443 -BackendPort 4443
        $inboundNATRule1v6 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev6" -FrontendIpConfiguration $FEIPConfigv6 -Protocol TCP -FrontendPort 443 -BackendPort 4443

2. Tworzenie sondy kondycji. Istnieją dwa sposoby konfigurowania sondy:

    Sonda HTTP

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    lub sondy TCP

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -Protocol Tcp -Port 8080 -IntervalInSeconds 15 -ProbeCount 2
        $RDPprobe = New-AzureRmLoadBalancerProbeConfig -Name 'RDPprobe' -Protocol Tcp -Port 3389 -IntervalInSeconds 15 -ProbeCount 2


    W tym przykładzie firma Microsoft będzie używany sondy TCP.

3. Tworzenie reguły równoważenia obciążenia.

        $lbrule1v4 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv4" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
        $lbrule1v6 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv6" -FrontendIpConfiguration $FEIPConfigv6 -BackendAddressPool $backendpoolipv6 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
        $RDPrule = New-AzureRmLoadBalancerRuleConfig -Name "RDPrule" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $RDPprobe -Protocol Tcp -FrontendPort 3389 -BackendPort 3389

4. Tworzenie równoważenia obciążenia przy użyciu poprzednio utworzone obiekty.

        $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name 'myNrpIPv6LB' -Location 'West US' -FrontendIpConfiguration $FEIPConfigv4,$FEIPConfigv6 -InboundNatRule $inboundNATRule1v6,$inboundNATRule1v4 -BackendAddressPool $backendpoolipv4,$backendpoolipv6 -Probe $healthProbe,$RDPprobe -LoadBalancingRule $lbrule1v4,$lbrule1v6,$RDPrule

## <a name="create-nics-for-the-back-end-vms"></a>Tworzenie nic dla maszyny wirtualne wewnętrznej

1. Uzyskaj wirtualnej sieci i wirtualnych podsieci, w której sieciowe muszą zostać utworzone.

        $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
        $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet

2. Tworzenie konfiguracji adresów IP i nic dla maszyny wirtualne.

        $nic1IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4 -LoadBalancerInboundNatRule $inboundNATRule1v4
        $nic1IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6 -LoadBalancerInboundNatRule $inboundNATRule1v6
        $nic1 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic0' -IpConfiguration $nic1IPv4,$nic1IPv6 -ResourceGroupName NRP-RG -Location 'West US'

        $nic2IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4
        $nic2IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6
        $nic2 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic1' -IpConfiguration $nic2IPv4,$nic2IPv6 -ResourceGroupName NRP-RG -Location 'West US'

## <a name="create-virtual-machines-and-assign-the-newly-created-nics"></a>Tworzenie maszyn wirtualnych i przypisywanie nowo utworzonego nic

Aby uzyskać więcej informacji na temat tworzenia maszyn wirtualnych, zobacz [Tworzenie wstępnie skonfigurować maszyny wirtualnej systemu Windows Azure programu PowerShell i Menedżera zasobów](..\virtual-machines\virtual-machines-windows-ps-create.md)

1. Tworzenie konta Ustawianie dostępności i miejsca do magazynowania

        New-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG -location 'West US'
        $availabilitySet = Get-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG
        New-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct' -Location 'West US' -SkuName $LRS
        $CreatedStorageAccount = Get-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct'

2. Tworzenie poszczególnych maszyn wirtualnych i przypisywanie poprzedniego utworzone nic

        $mySecureCredentials= Get-Credential -Message “Type the username and password of the local administrator account.”

        $vm1 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM0' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
        $vm1 = Set-AzureRmVMOperatingSystem -VM $vm1 -Windows -ComputerName 'myNrpIPv6VM0' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
        $vm1 = Set-AzureRmVMSourceImage -VM $vm1 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
        $vm1 = Add-AzureRmVMNetworkInterface -VM $vm1 -Id $nic1.Id -Primary
        $osDisk1Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM0osdisk.vhd"
        $vm1 = Set-AzureRmVMOSDisk -VM $vm1 -Name 'myNrpIPv6VM0osdisk' -VhdUri $osDisk1Uri -CreateOption FromImage
        New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm1

        $vm2 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM1' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
        $vm2 = Set-AzureRmVMOperatingSystem -VM $vm2 -Windows -ComputerName 'myNrpIPv6VM1' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
        $vm2 = Set-AzureRmVMSourceImage -VM $vm2 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
        $vm2 = Add-AzureRmVMNetworkInterface -VM $vm2 -Id $nic2.Id -Primary
        $osDisk2Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM1osdisk.vhd"
        $vm2 = Set-AzureRmVMOSDisk -VM $vm2 -Name 'myNrpIPv6VM1osdisk' -VhdUri $osDisk2Uri -CreateOption FromImage
        New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm2

## <a name="next-steps"></a>Następne kroki

[Przed rozpoczęciem konfigurowania równoważenia obciążenia wewnętrznych](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurowanie trybu rozkładu równoważenia obciążenia](load-balancer-distribution-mode.md)

[Konfigurowanie ustawienia przekroczenia limitu czasu bezczynności protokołu TCP dla usługi równoważenia obciążenia](load-balancer-tcp-idle-timeout.md)
