<properties
   pageTitle="Tworzenie równoważenia obciążenia wewnętrznych, przy użyciu programu PowerShell w Menedżerze zasobów | Microsoft Azure"
   description="Dowiedz się, jak utworzyć równoważenia obciążenia wewnętrznych, przy użyciu programu PowerShell w Menedżerze zasobów"
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

# <a name="create-an-internal-load-balancer-using-powershell"></a>Tworzenie równoważenia obciążenia wewnętrznych, przy użyciu programu PowerShell

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][model klasyczny wdrożenia](load-balancer-get-started-ilb-classic-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]


Poniższe kroki wyjaśniono, jak utworzyć równoważenia obciążenia wewnętrznych, Menedżer zasobów Azure za pomocą programu PowerShell. Przy użyciu Menedżera zasobów Azure elementy, aby utworzyć równoważenia obciążenia wewnętrzne są skonfigurowane indywidualnie i następnie łączyć w celu tworzenia równoważenia obciążenia.

Musisz utworzyć i skonfigurować następujące obiekty do wdrożenia usługi równoważenia obciążenia:

- Wierzch koniec konfiguracji IP - skonfiguruje prywatny adres IP dla ruchu sieci przychodzącego
- Pula adresów wewnętrznej bazy danych — skonfiguruje interfejsów sieciowych, które otrzymają ruchu równoważenia obciążenia pochodzące z puli IP fronton
- Załaduj równoważenia reguł — źródła i konfigurację portu lokalnego dla usługi równoważenia obciążenia.
- Sondy — konfiguruje sondy stanu kondycji wystąpień maszyn wirtualnych.
- Translatora adresów Sieciowych reguły przychodzące — konfiguruje reguły portu, aby uzyskać bezpośredni dostęp do jednego wystąpienia maszyn wirtualnych.

Składniki równoważenia można uzyskać więcej informacji na temat ładowania z menedżerem Azure w [Azure Menedżera zasobów pomocy technicznej dotyczącej usługi równoważenia obciążenia](load-balancer-arm.md).

Poniższe czynności wyjaśniono, jak skonfigurować usługę równoważenia obciążenia między dwoma maszyn wirtualnych.


## <a name="setup-powershell-to-use-resource-manager"></a>Konfigurowanie programu PowerShell za pomocą Menedżera zasobów

Upewnij się, jest zainstalowana najnowsza wersja produkcji Azure modułu dla programu PowerShell i skonfigurowaniu programu PowerShell poprawnie Aby uzyskać dostęp do subskrypcji usługi Azure.

### <a name="step-1"></a>Krok 1

        Login-AzureRmAccount

### <a name="step-2"></a>Krok 2

Sprawdzanie subskrypcji dla konta

        Get-AzureRmSubscription

Możesz zostanie wyświetlony monit o uwierzytelniania przy użyciu poświadczeń.<BR>

### <a name="step-3"></a>Krok 3

Wybranie Azure subskrypcji korzystać. <BR>


        Select-AzureRmSubscription -Subscriptionid "GUID of subscription"

### <a name="create-resource-group-for-load-balancer"></a>Tworzenie grupy zasobów dla usługi równoważenia obciążenia

### <a name="step-4"></a>Krok 4

Tworzenie nowej grupy zasobów (Pomiń ten krok, jeśli z istniejącej grupy zasobów)

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

Azure Menedżera zasobów wymaga, aby wszystkie grupy zasobów określ lokalizację. To jest używany jako domyślnej lokalizacji dla zasobów w danej grupy zasobów. Upewnij się, że wszystkie polecenia do tworzenia równoważenia obciążenia będzie korzystać z tej samej grupy zasobów.

W powyższym przykładzie możemy utworzyć grupę zasobów o nazwie "NRP RG" i lokalizacja "Zachód z NAMI".

## <a name="create-virtual-network-and-a-private-ip-address-for-front-end-ip-pool"></a>Tworzenie wirtualnych sieci i prywatny adres IP fronton puli adresów IP


### <a name="step-1"></a>Krok 1

Tworzy podsieć dla wirtualnej sieci i przypisuje zmiennej $backendSubnet

    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24

Tworzenie wirtualnych sieci:

    $vnet= New-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

Tworzy wirtualną sieć i dodaje podsieci można podsieci kg wirtualnej sieci NRPVNet i przypisuje zmiennej $vnet



## <a name="create-front-end-ip-pool-and-backend-address-pool"></a>Tworzenie puli IP zakończenia wierzch i puli adresów wewnętrznej bazy danych

Konfigurowanie puli adresów IP fronton dla poczty przychodzącej ładowania równoważenia ruchu i wewnętrznej bazy danych puli adresów sieciowych do odbierania Załaduj strategie ruch.

### <a name="step-1"></a>Krok 1

Tworzenie puli adresów IP fronton przy użyciu prywatny adres IP 10.0.2.5 dla 10.0.2.0/24 podsieci który będzie przychodzących końcowy ruchu sieciowego.

    $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PrivateIpAddress 10.0.2.5 -SubnetId $vnet.subnets[0].Id

### <a name="step-2"></a>Krok 2

Konfigurowanie puli adresów wewnętrznej do odbierania ruch przychodzący z puli IP fronton:

    $beaddresspool= New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"


## <a name="create-lb-rules-nat-rules-probe-and-load-balancer"></a>Tworzenie reguły kg, reguły translatora adresów Sieciowych, sonda i Usługa równoważenia obciążenia

Po utworzeniu puli IP fronton i puli adresów wewnętrznej bazy danych, należy utworzyć reguły należące do zasobu usługi równoważenia obciążenia:

### <a name="step-1"></a>Krok 1

    $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP1" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

    $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP2" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -RequestPath "HealthProbe.aspx" -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

     $lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HTTP" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80


W powyższym przykładzie są tworzone następujące elementy:

- Reguły translatora adresów Sieciowych, która cały ruch przychodzący do portu 3441 zostaną przekierowane do portu 3389.
- drugą regułę translatora adresów Sieciowych, która cały ruch przychodzący do portu 3442 zostaną przekierowane do portu 3389.
- reguły równoważenia obciążenia, która będzie ładowana saldo cały ruch przychodzący ruch na porcie publicznej 80 do portu lokalnego 80 w puli adresów wewnętrznej.
- reguły sondy, która będzie sprawdzanie stanu kondycji ścieżki "HealthProbe.aspx"



### <a name="step-2"></a>Krok 2

Tworzenie równoważenia obciążenia zsumowanie wszystkich obiektów (reguły translatora adresów Sieciowych, zasady równoważenia obciążenia, konfiguracji sondy):

    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName "NRP-RG" -Name "NRP-LB" -Location "West US" -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe


## <a name="create-network-interfaces"></a>Tworzenie interfejsów sieciowych

Po utworzeniu równoważenia obciążenia wewnętrznych, potrzebne do definiowania interfejsów sieciowych, które będzie otrzymywał przychodzących ruch sieciowy równoważenia obciążenia, reguły translatora adresów Sieciowych i sondy. Interfejs sieć w tym przypadku skonfigurowano pojedynczo i można przypisywać maszyn wirtualnych później.


### <a name="step-1"></a>Krok 1


Uzyskiwanie zasobów wirtualnej sieci i podsieci, aby utworzyć interfejsy:

    $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG

    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet


Spowoduje to utworzenie karty sieciowej, w której będzie należeć do puli wewnętrznej równoważenia obciążenia i kojarzenie pierwszą regułę translatora adresów Sieciowych dla RDP dla tego sieciowej:

    $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic1-be -Location "West US" -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]

### <a name="step-2"></a>Krok 2

Utwórz drugą interfejs sieć o nazwie można Nic2 kg:

Spowoduje to utworzenie karty sieciowej drugi, przypisywanie do tej samej puli wewnętrznej równoważenia obciążenia i kojarzenie drugą regułę translatora adresów Sieciowych utworzone RDP:

     $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic2-be -Location "West US" -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]

Wynik końcowy będzie zawierać następujące informacje:

    $backendnic1

Oczekiwany wynik:

    Name                 : lb-nic1-be
    ResourceGroupName    : NRP-RG
    Location             : westus
    Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
    Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
    ProvisioningState    : Succeeded
    Tags                 :
    VirtualMachine       : null
    IpConfigurations     : [
                         {
                           "PrivateIpAddress": "10.0.2.6",
                           "PrivateIpAllocationMethod": "Static",
                           "Subnet": {
                             "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                           },
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
                           "ProvisioningState": "Succeeded",
                           "Name": "ipconfig1",
                           "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                           "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1"
                         }
                       ]
    DnsSettings          : {
                         "DnsServers": [],
                         "AppliedDnsServers": []
                       }
    AppliedDnsSettings   :
    NetworkSecurityGroup : null
    Primary              : False



### <a name="step-3"></a>Krok 3

Aby przypisać NIC maszyn wirtualnych za pomocą polecenia Dodaj AzureRmVMNetworkInterface.

Znajdują się instrukcje krok po kroku, aby utworzyć maszyny wirtualnej i przypisać NIC, zgodnie z dokumentacją: [Tworzenie maszyn wirtualnych Azure przy użyciu programu PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md).

Jeśli masz już utworzony maszyny wirtualnej, możesz dodać sieciowej poniższe czynności:

#### <a name="step-1"></a>Krok 1

Ładowanie zasobów równoważenia obciążenia do zmiennej (jeśli można jeszcze). Zmienna nosi nazwę $lb i tych samych nazwach z zasobu usługi równoważenia obciążenia utworzony w poprzednim przykładzie.

    $lb= Get-AzureRmLoadBalancer –name NRP-LB -resourcegroupname NRP-RG

#### <a name="step-2"></a>Krok 2

Ładowanie konfiguracji wewnętrznej bazy danych do zmiennej.

    $backend= Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb

#### <a name="step-3"></a>Krok 3

Ładowanie utworzone sieciowej do zmiennej. Nazwa zmiennej używany jest $ karty sieciowej. Nazwa interfejsu sieci używana jest taki sam z powyższego przykładu.

    $nic=Get-AzureRmNetworkInterface –name lb-nic1-be -resourcegroupname NRP-RG

#### <a name="step-4"></a>Krok 4

Zmień konfigurację wewnętrznej bazy danych na karcie sieciowej.

    $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend

#### <a name="step-5"></a>Krok 5

Zapisz obiekt interfejsu sieciowego.

    Set-AzureRmNetworkInterface -NetworkInterface $nic

Po dodaniu karty sieciowej puli wewnętrznej bazy danych usługi równoważenia obciążenia rozpoczyna się odbierania ruchu sieciowego zgodnie z regułami dla tego zasobu usługi równoważenia obciążenia równoważenia obciążenia.

## <a name="update-an-existing-load-balancer"></a>Aktualizowanie istniejącego równoważenia obciążenia


### <a name="step-1"></a>Krok 1

Przy użyciu usługi równoważenia obciążenia z powyższego przykładu, przypisywanie obiektu usługi równoważenia obciążenia zmiennej $slb przy użyciu Get-AzureRmLoadBalancer

    $slb=get-azureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

### <a name="step-2"></a>Krok 2

W poniższym przykładzie spowoduje dodanie nowej reguły przychodzące translatora adresów Sieciowych przy użyciu port 81 w zewnętrznej i 8181 puli wewnętrznej do istniejącego równoważenia obciążenia

    $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol Tcp


### <a name="step-3"></a>Krok 3

Zapisywanie nowej konfiguracji przy użyciu zestawu AzureLoadBalancer

    $slb | Set-AzureRmLoadBalancer

## <a name="remove-a-load-balancer"></a>Usuwanie usługi równoważenia obciążenia

Usuwanie o nazwie "NRP — kg" w grupie zasobów o nazwie "NRP RG" usługę równoważenia obciążenia poprzednio utworzone za pomocą polecenia Usuń AzureRmLoadBalancer

    Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

>[AZURE.NOTE] Możesz użyć opcjonalne przełączanie — wymuszanie, aby uniknąć wiersz do usunięcia.



## <a name="next-steps"></a>Następne kroki

[Konfigurowanie trybu rozkładu równoważenia obciążenia](load-balancer-distribution-mode.md)

[Konfigurowanie ustawienia przekroczenia limitu czasu bezczynności protokołu TCP dla usługi równoważenia obciążenia](load-balancer-tcp-idle-timeout.md)