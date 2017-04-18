<properties
   pageTitle="Konfigurowanie zawsze na grupy dostępności detektorów — Microsoft Azure"
   description="Skonfiguruj detektory grupy dostępność na modelu Menedżera zasobów Azure, równoważenia obciążenia wewnętrznych za pomocą jednego lub więcej adresów IP."
   services="virtual-machines"
   documentationCenter="na"
   authors="MikeRayMSFT"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows-sql-server"
   ms.workload="infrastructure-services"
   ms.date="10/20/2016"
   ms.author="MikeRayMSFT"/>

# <a name="configure-one-or-more-always-on-availability-group-listeners---resource-manager"></a>Konfigurowanie jeden lub więcej zawsze na dostępność grupy detektory - Menedżera zasobów 

W tym temacie przedstawiono sposób wykonać dwie czynności:

- Tworzenie równoważenia obciążenia wewnętrznych dla grup dostępność programu SQL Server przy użyciu poleceń cmdlet programu PowerShell.

- Dodawanie dodatkowych adresów IP do równoważenia obciążenia do obsługi więcej niż jedną grupę dostępność programu SQL Server. 

W programie SQL Server detektor grupy dostępność jest nazwą wirtualnej sieci, której klienci łączą się w celu uzyskania dostępu do bazy danych w replice podstawowym lub pomocniczym. W przypadku Azure maszyn wirtualnych równoważenia obciążenia zawiera adres IP odbiornika. Usługi równoważenia obciążenia kieruje ruch do wystąpienia programu SQL Server, które nasłuchują na porcie sondy. W większości przypadków grupy dostępności używa równoważenia obciążenia wewnętrzny. Równoważenia obciążenia wewnętrznych Azure można udostępnić jeden lub wiele adresów IP. Każdego adresu IP korzysta z portu określonych sondy. Ten dokument przedstawiono sposób tworzenia nowej usługi równoważenia obciążenia i Dodaj adresy IP do istniejącej usługi równoważenia obciążenia dla grup dostępność programu SQL Server za pomocą programu PowerShell. 

Możliwość przypisywania wiele adresów IP do równoważenia obciążenia wewnętrznych nowego Azure i jest dostępna tylko w modelu Menedżera zasobów. Aby wykonać to zadanie, należy następująco skonfigurować grupę dostępność programu SQL Server wdrożony w przypadku Azure maszyn wirtualnych w modelu Menedżera zasobów. Oba maszyn wirtualnych programu SQL Server musi należeć do tego samego zestawu dostępności. [Szablon programu Microsoft](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) umożliwia automatyczne tworzenie grupy dostępności w Menedżerze zasobów Azure. Ten szablon automatycznie tworzy grupy dostępności, w tym równoważenia obciążenia wewnętrznych dla Ciebie. Jeśli wolisz, możesz [ręcznie skonfigurować grupy dostępności (AlwaysOn)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

W tym temacie wymaga grup dostępności są już skonfigurowane.  

Tematy pokrewne obejmują:

- [Konfigurowanie grupy dostępności (AlwaysOn) w Azure maszyn wirtualnych (Graficznym)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   

- [Konfigurowanie połączenia VNet do VNet za pomocą Menedżera zasobów Azure i programu PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-vm-powershell.md)]

## <a name="configure-the-windows-firewall"></a>Konfigurowanie Zapory systemu Windows

Konfigurowanie Zapory systemu Windows, aby umożliwić dostęp programu SQL Server. Należy skonfigurować zaporę do zezwalania na połączenia TCP do używania porty przez wystąpienie programu SQL Server, a także port używany przez sondy odbiornika. Aby uzyskać szczegółowe instrukcje, zobacz [Konfigurowanie Zapory systemu Windows dla dostępu aparatu bazy danych](http://msdn.microsoft.com/library/ms175043.aspx#Anchor_1). Utwórz regułę ruchu przychodzącego portu programu SQL Server i portu sondy.

## <a name="example-script-create-an-internal-load-balancer-with-powershell"></a>Przykładowy skrypt: Tworzenie równoważenia obciążenia wewnętrznych przy użyciu programu PowerShell

> [AZURE.NOTE] Jeśli utworzono grupy dostępności za pomocą [szablonu Microsoft](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) Załaduj nie musisz wykonać ten krok. 

Poniższy skrypt programu PowerShell tworzy równoważenia obciążenia wewnętrznych, konfiguruje reguły równoważenia obciążenia i zestawy adresów IP dla usługi równoważenia obciążenia. Aby uruchomić skrypt, otwórz Windows PowerShell ISE i wklej skrypt w okienku Skrypt. Używanie `Login-AzureRMAccount` aby zalogować się do programu PowerShell. Jeśli masz wiele subskrypcji Azure za pomocą `Select-AzureRmSubscription ` ustawić subskrypcji. 

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<Resource Group Name>" # Resource group name
$VNetName = "<Virtual Network Name>"         # Virtual network name
$SubnetName = "<Subnet Name>"                # Subnet name
$ILBName = "<Load Balancer Name>"            # ILB name
$Location = "<Azure Region>"                 # Azure location
$VMNames = "<VM1>","<VM2>"                   # Virtual machine names

$ILBIP = "<n.n.n.n>"                         # IP address
[int]$ListenerPort = "<nnnn>"                # AG listener port
[int]$ProbePort = "<nnnn>"                   # Probe port

$LBProbeName ="ILBPROBE_$ListenerPort"       # The Load balancer Probe Object Name              
$LBConfigRuleName = "ILBCR_$ListenerPort"    # The Load Balancer Rule Object Name

$FrontEndConfigurationName = "FE_SQLAGILB_1" # Object name for the Front End configuration 
$BackEndConfigurationName ="BE_SQLAGILB_1"   # Object name for the Back End configuration

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 

$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName 

$FEConfig = New-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.id

$BEConfig = New-AzureRMLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName 

$SQLHealthProbe = New-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -Protocol tcp -Port $ProbePort -IntervalInSeconds 15 -ProbeCount 2

$ILBRule = New-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP 

$ILB= New-AzureRmLoadBalancer -Location $Location -Name $ILBName -ResourceGroupName $ResourceGroupName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -LoadBalancingRule $ILBRule -Probe $SQLHealthProbe 

$bepool = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB 

foreach($VMName in $VMNames)
    {
        $VM = Get-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VMName 
        $NICName = ($VM.NetworkInterfaceIDs[0].Split('/') | select -last 1)
        $NIC = Get-AzureRmNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools = $BEPool
        Set-AzureRmNetworkInterface -NetworkInterface $NIC
        start-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VM.Name 
    }
```

## <a name="example-script-add-an-ip-address-to-an-existing-load-balancer-with-powershell"></a>Przykładowy skrypt: Dodaj do istniejących usługi równoważenia obciążenia przy użyciu programu PowerShell adres IP

Aby użyć więcej niż jedną grupę dostępności, umożliwia dodawanie dodatkowego adresu IP do istniejącego równoważenia obciążenia programu PowerShell. Każdego adresu IP wymaga własnej równoważenia reguły, port sondy i port przedniej obciążenia.

Port fronton jest które aplikacje używane do łączenia się wystąpienie programu SQL Server. Adresy IP dostępność różnych grup można używać tego samego portu frontonu.

>[AZURE.NOTE] Grupy dostępności programu SQL Server każdego adresu IP wymaga portu określonych sondy. Na przykład jeśli jeden adres IP na równoważenia obciążenia korzysta z portu sondy 59999, bez adresów IP na tym równoważenia obciążenia można korzysta z portu sondy 59999. 

- Informacji na temat usługi równoważenia obciążenia limity dla **prywatnych wierzch zakończyć adresów IP dla usługi równoważenia obciążenia** w obszarze [Sieć limity - Azure Menedżera zasobów](../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).

- Aby uzyskać informacje o dostępności limity grupy zobacz [ograniczeń (Dostępność grupy)](http://msdn.microsoft.com/library/ff878487.aspx#RestrictionsAG).

Poniższy skrypt dodaje nowy adres IP do istniejącego równoważenia obciążenia. Aktualizowanie zmiennych w środowisku usługi. ILB korzysta z portu odbiornika portu fronton równoważenia obciążenia. Ten port może być port, do którego nasłuchują na program SQL Server. W przypadku wystąpienia domyślnego programu SQL Server jest port 1433. Reguły grupy dostępność usługi równoważenia obciążenia wymaga przestawny IP (zwrotu bezpośredni server), port wewnętrznej jest taka sama jak port frontonu.

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<ResourceGroup>"          # Resource group name
$VNetName = "<VirtualNetwork>"                  # Virtual network name
$SubnetName = "<Subnet>"                        # Subnet name
$ILBName = "<ILBName>"                          # ILB name                      

$ILBIP = "<n.n.n.n>"                            # IP address
[int]$ListenerPort = "<nnnn>"                   # AG listener port
[int]$ProbePort = "<nnnnn>"                     # Probe port 

$ILB = Get-AzureRmLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName 

$count = $ILB.FrontendIpConfigurations.Count+1
$FrontEndConfigurationName ="FE_SQLAGILB_$count"  

$LBProbeName = "ILBPROBE_$count"
$LBConfigrulename = "ILBCR_$count"

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 
$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

$ILB | Add-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id 

$ILB | Add-AzureRmLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 15  | Set-AzureRmLoadBalancer 

$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

$FEConfig = get-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB

$SQLHealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

$BEConfig = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $ILB.BackendAddressPools[0].Name -LoadBalancer $ILB 

$ILB | Add-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort  $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP | Set-AzureRmLoadBalancer   
```



## <a name="configure-the-cluster-to-use-the-load-balancer-ip-address"></a>Skonfiguruj klaster na używanie adresu IP usługi równoważenia obciążenia 

Następnym krokiem jest Konfigurowanie odbiornika w klastrze, a następnie przełącz odbiornika w trybie online. Aby to zrobić, wykonaj następujące czynności: 

1. Tworzenie odbiornika grupy dostępność w klastrze pracy awaryjnej  

2. Przesuń odbiornika w trybie online

## <a name="1-create-the-availability-group-listener-on-the-failover-cluster"></a>1. Tworzenie odbiornika grupy dostępność w klastrze pracy awaryjnej

W tym kroku Dodaj punkt dostępu klienta z klastrem pracy awaryjnej z Menedżer klastrów pracy awaryjnej, a następnie użyj programu PowerShell, aby skonfigurować zasób klaster, aby odsłuchać na porcie sondy. 

1. Nawiązywanie połączenia z Azure maszyny wirtualnej, która obsługuje podstawowego replice za pomocą RDP. 

2. Otwórz Menedżera klaster pracy awaryjnej.

3. Wybierz węzeł **sieci** i zanotuj nazwę sieci klaster. Użyj tej nazwy w `$ClusterNetworkName` zmiennych w skrypt programu PowerShell.

4. Rozwinięcie nazwy klaster, a następnie kliknij przycisk **role**.

5. W okienku **role** kliknij prawym przyciskiem myszy nazwę grupy dostępności, a następnie wybierz pozycję **Dodaj zasobów** > **Punkt dostępu klienta**.

6. W polu **Nazwa** Tworzenie nazwy dla tej nowej odbiornika, a następnie kliknij przycisk **Dalej** dwa razy, a następnie kliknij **Zakończ**. Nie powodują odbiornika lub zasób do trybu online na tym etapie.
 
    Nazwa dla nowego odbiornika jest nazwę sieciową, której aplikacji będzie korzystać z bazami danych w grupie dostępność programu SQL Server.

7. Kliknij kartę **zasoby** , a następnie rozwiń węzeł właśnie utworzony punkt dostępu klienta. Kliknij prawym przyciskiem myszy zasób IP, a następnie kliknij polecenie Właściwości. Zanotuj nazwę z adresem IP. Będzie używać tej nazwy w `$IPResourceName` zmiennych w skrypt programu PowerShell.

8. W polu **Adres IP** kliknij **Statyczny adres IP** i ustaw statycznego adresu IP do tego samego adresu, który został użyty podczas skonfigurować adres IP usługi równoważenia obciążenia na Azure portal. 

9. Wyłącz NetBIOS dla tego adresu i kliknij **przycisk OK**. Powtórz ten krok dla każdego zasobu adresów IP, jeśli rozwiązanie obejmuje wiele VNets Azure. 

10. Wprowadź zależne od adresu IP zasobu grupy dostępności serwera SQL. Kliknij prawym przyciskiem myszy zasób w Menedżerze klaster, to na karcie **zasoby** w obszarze **Inne zasoby**. 

11. Na węzła obsługującego podstawowego replice Otwórz podwyższonym poziomem środowiska PowerShell ISE i wklej następujące polecenia do nowego skryptu. Na karcie **zależności** kliknij nazwę odbiornika.
 
    ```PowerShell
    $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
    $IPResourceName = "<IPResourceName>" # the IP Address resource name
    $ILBIP = “<n.n.n.n>” # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
    [int]$ProbePort = <nnnnn>

    Import-Module FailoverClusters
    
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
    ```
 
6. Zaktualizuj zmienne i uruchomić skrypt programu PowerShell, aby skonfigurować adres IP i port dla nowych odbiornika.

 >[AZURE.NOTE] Jeśli serwery SQL znajdują się w oddzielnym regionów, należy uruchomić skrypt programu PowerShell dwa razy. Po raz pierwszy za pomocą nazwy sieci klaster, nazwa zasobu Klaster IP i ładowanie adres IP równoważenia z pierwszej grupy zasobów. Po raz drugi Użyj nazwy sieci klaster, nazwa zasobu Klaster IP i załadować adresu IP równoważenia z drugim grupa zasobów.

Klaster zawiera teraz zasób odbiornika Dostępność grupy.

## <a name="2-bring-the-listener-online"></a>2. Przesuń odbiornika w trybie online

Z dostępność zasobu odbiornika grupy skonfigurowane możesz zabrać ze sobą odbiornika online, aby aplikacje można nawiązać połączenie bazy danych w grupie dostępności z odbiornika.

1. Przejdź wstecz do Menedżer klastrów pracy awaryjnej. Rozwiń węzeł **role** , a następnie wyróżnij grupy dostępności. Na karcie **zasoby** kliknij prawym przyciskiem myszy nazwę odbiornika, a następnie kliknij polecenie **Właściwości**.

1. Kliknij kartę **zależności** . W przypadku wielu zasobów na liście, sprawdź, czy adresy IP są lub nie oraz, zależności. Kliknij **przycisk OK**.

1. Kliknij prawym przyciskiem myszy nazwę odbiornika, a następnie kliknij przycisk **Przejdź do trybu Online**.

1. Gdy odbiornika online, na karcie **zasoby** , kliknij prawym przyciskiem myszy grupy dostępności, a następnie kliknij polecenie **Właściwości**.

1. Tworzenie współzależności dla zasobu Nazwa odbiornika (nie IP adres zasoby Nazwa). Kliknij **przycisk OK**.

1. Uruchamianie programu SQL Server Management Studio i łączenie replice podstawowego.

1. Przejdź do **dostępności (AlwaysOn) wysoki** | **grupy dostępności** | **detektory grupy dostępności**. 

1. Powinien zostać wyświetlony nazwę odbiornika utworzoną w Menedżerze klaster pracy awaryjnej. Kliknij prawym przyciskiem myszy nazwę odbiornika, a następnie kliknij polecenie **Właściwości**.

1. W polu **Port** Określ numer portu odbiornika grupy dostępności za pomocą $EndpointPort użyto wcześniej (1433 był domyślnym), kliknij **przycisk OK**.

Teraz masz grupy dostępności programu SQL Server Azure maszyn wirtualnych działa w trybie Menedżera zasobów. 

## <a name="test-the-connection-to-the-listener"></a>Testowanie połączenia odbiornika

Aby przetestować połączenie:

1. RDP z programem SQL Server, która znajduje się w tej samej sieci wirtualnej, ale nie ma replice. Może to być inne programu SQL Server w klastrze.

2. Aby przetestować połączenie za pomocą narzędzia **sqlcmd** . Na przykład poniższy skrypt nawiąże połączenie **sqlcmd** podstawowego replice za pośrednictwem detektora uwierzytelniania systemu Windows:

    ```
    sqlmd -S <listenerName> -E
    ```

    Jeśli odbiornik korzysta z portu inny niż domyślny port (1433), określ port w parametrach połączenia. Na przykład następujące polecenie sqlcmd łączy detektor na porcie 1435: 
    
    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

Połączenie SQLCMD automatycznie łączy podstawowego replice znajduje się niezależnie od tego wystąpienia programu SQL Server. 

>[AZURE.NOTE] Upewnij się, że port zadanej jest otwarty na zaporze zarówno serwera SQL. Oba serwery wymagają regułę ruchu przychodzącego port TCP, którego używasz. Aby uzyskać więcej informacji, zobacz [Dodawanie lub edytowanie reguły zapory](http://technet.microsoft.com/library/cc753558.aspx) . 

## <a name="guidelines-and-limitations"></a>Wskazówki i ograniczenia

Należy zauważyć, że usługa równoważenia obciążenia tych wskazówek na dostępność grupy odbiornika platformy Azure za pomocą wewnętrznych:

- Przy użyciu usługi równoważenia obciążenia wewnętrznych tylko dostęp odbiornika z w tej samej sieci wirtualnej.

## <a name="for-more-information"></a>Aby uzyskać więcej informacji

Aby uzyskać więcej informacji, zobacz [Konfigurowanie zawsze na dostępność grupy ręcznie maszyn wirtualnych Azure](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

### <a name="powershell-cmdlets"></a>Polecenia cmdlet programu PowerShell

Umożliwia utworzenie równoważenia obciążenia wewnętrznych dla Azure maszyn wirtualnych następujące polecenia cmdlet programu PowerShell.

- `New-AzureRmLoadBalancer`Tworzy równoważenia obciążenia. Aby uzyskać więcej informacji, zobacz [AzureRmLoadBalancer nowy](http://msdn.microsoft.com/library/mt619450.aspx) . 

- `New-AzureRMLoadBalancerFrontendIpConfig`Tworzy zewnętrzną konfiguracji adresów IP dla usługi równoważenia obciążenia. Aby uzyskać więcej informacji, zobacz [AzureRMLoadBalancerFrontendIpConfig nowy](http://msdn.microsoft.com/library/mt603510.aspx) .

- `New-AzureRmLoadBalancerRuleConfig`Tworzy konfiguracji reguły dla usługi równoważenia obciążenia. Aby uzyskać więcej informacji, zobacz [AzureRmLoadBalancerRuleConfig nowy](http://msdn.microsoft.com/library/mt619391.aspx) . 

- `New-AzureRMLoadBalancerBackendAddressPoolConfig`Tworzy Konfiguracja puli adresów wewnętrznej bazy danych dla usługi równoważenia obciążenia. Aby uzyskać więcej informacji, zobacz [AzureRmLoadBalancerBackendAddressPoolConfig nowy](http://msdn.microsoft.com/library/mt603791.aspx) . 

- `New-AzureRmLoadBalancerProbeConfig`tworzy konfigurację sondy do równoważenia obciążenia. Aby uzyskać więcej informacji, zobacz [AzureRmLoadBalancerProbeConfig nowy](http://msdn.microsoft.com/library/mt603847.aspx) .

Jeśli chcesz usunąć równoważenia obciążenia grupy zasobów Azure za pomocą `Remove-AzureRmLoadBalancer`. Aby uzyskać więcej informacji zobacz [Usuwanie AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt603862.aspx).
