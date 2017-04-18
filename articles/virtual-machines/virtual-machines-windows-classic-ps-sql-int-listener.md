<properties
    pageTitle="Konfigurowanie detektor ILB dla zawsze na grupy dostępności | Microsoft Azure"
    description="Ten samouczek używa zasobów utworzone za pomocą modelu Klasyczny wdrożenia i tworzy zawsze na dostępność grupy detektor platformy Azure za pomocą wewnętrznych równoważenia obciążenia (ILB)."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="MikeRayMSFT" />

# <a name="configure-an-ilb-listener-for-always-on-availability-groups-in-azure"></a>Konfigurowanie detektor ILB zawsze na grup dostępność platformy Azure

> [AZURE.SELECTOR]
- [Odbiornik wewnętrznych](virtual-machines-windows-classic-ps-sql-int-listener.md)
- [Odbiornik zewnętrznych](virtual-machines-windows-classic-ps-sql-ext-listener.md)

## <a name="overview"></a>Omówienie

W tym temacie przedstawiono sposób konfigurowania detektor zawsze na grupy dostępności za pomocą **Usługi równoważenia obciążenia wewnętrzny (ILB)**.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Aby skonfigurować detektor ILB dla grupy zawsze na dostępność w modelu Menedżera zasobów, zobacz [Konfigurowanie usługi równoważenia obciążenia wewnętrznych zawsze na dostępność grupy w Azure](virtual-machines-windows-portal-sql-alwayson-int-listener.md).


Grupy dostępności mogą zawierać repliki lokalnego, tylko Azure lub obejmują zarówno w lokalnej, jak i Azure w przypadku konfiguracji hybrydowych. Azure repliki mogą znajdować się w tym samym regionie lub w wielu regionów przy użyciu wielu sieci wirtualnych (VNets). Poniższe kroki przyjęto założenie, masz już [skonfigurowane grupy dostępności](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md) , ale nie skonfigurowano detektor.

## <a name="guidelines-and-limitations-for-internal-listeners"></a>Wskazówki i ograniczenia dotyczące detektory wewnętrznych
Zwróć uwagę na odbiornika grupy dostępność platformy Azure za pomocą ILB tych wskazówek:

- Dostępność odbiornika grupy jest obsługiwana w systemie Windows Server 2008 R2, Windows Server 2012 i Windows Server 2012 R2.

- Tylko jeden detektor grupy dostępność wewnętrzny jest obsługiwana na usługi w chmurze, ponieważ odbiornik jest skonfigurowany do ILB, a istnieje tylko jedna ILB na usługi w chmurze. Istnieje możliwość utworzenia wielu detektory zewnętrznych. Aby uzyskać więcej informacji zobacz [Konfigurowanie detektor zewnętrznych zawsze na dostępność grup w Azure](virtual-machines-windows-classic-ps-sql-ext-listener.md).

- Tworzenie detektor wewnętrznych w tym samym usługi w chmurze, gdy masz również detektor zewnętrznych za pomocą publicznej VIP usługi w chmurze nie jest obsługiwane.

## <a name="determine-the-accessibility-of-the-listener"></a>Określanie dostępności odbiornika

[AZURE.INCLUDE [ag-listener-accessibility](../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

W tym artykule omówiono tworzenie odbiornik używa **Wewnętrznych równoważenia obciążenia (ILB)**. W razie potrzeby detektor publiczna/zewnętrznego, zapoznaj się z wersją niniejszego artykułu opisano procedury dotyczące konfigurowania [odbiornika zewnętrznych](virtual-machines-windows-classic-ps-sql-ext-listener.md)

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Tworzenie zwrotu równoważenia obciążenia punkty końcowe maszyn wirtualnych z serwerem bezpośrednie

Dla ILB należy najpierw utworzyć równoważenia obciążenia wewnętrzny. Można to zrobić w obszarze skrypt poniżej.

[AZURE.INCLUDE [load-balanced-endpoints](../../includes/virtual-machines-ag-listener-load-balanced-endpoints.md)]

1. W przypadku **ILB**należy przypisać statyczny adres IP. Najpierw sprawdź bieżącej konfiguracji VNet, uruchamiając następujące polecenie:

        (Get-AzureVNetConfig).XMLConfiguration

1. Zanotuj nazwę **podsieci** dla podsieci, która zawiera maszyny wirtualne, które obsługują repliki. To zostanie użyty w parametrze **$SubnetName** skryptu.

1. Zanotuj nazwę **VirtualNetworkSite** i początkowe **AddressPrefix** dla podsieci, która zawiera maszyny wirtualne, które obsługują repliki. Wyszukaj adres IP przekazywania obie wartości do polecenia **Test AzureStaticVNetIP** i badania **AvailableAddresses**. Na przykład jeśli VNet był o nazwie *MyVNet* zawiera adres podsieci zakresu, który rozpoczęto o *172.16.0.128*, następujące polecenie pojawi się lista dostępnych adresów:

        (Test-AzureStaticVNetIP -VNetName "MyVNet"-IPAddress 172.16.0.128).AvailableAddresses

1. Wybierz jedną z dostępnych adresów i używać go w parametrze **$ILBStaticIP** następujący skrypt.

3. Skopiuj skrypt programu PowerShell poniżej do edytora tekstów i ustaw wartości zmiennych do potrzeb środowiska (należy zauważyć, że zostały dołączone wartości domyślnych dla niektórych parametrów). Należy zauważyć, że istniejące wdrożonych koligacji grup nie można dodać ILB. Aby uzyskać więcej informacji na wymagania ILB zobacz [Wewnętrzny usługi równoważenia obciążenia](../load-balancer/load-balancer-internal-overview.md). Ponadto jeśli grupy dostępność obejmuje Azure regionów, należy uruchomić skrypt raz w każdej centrum danych usługi w chmurze i węzłów, które znajdują się w tym centrum danych.

        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas
        $SubnetName = "<MySubnetName>" # subnet name that the replicas use in the VNet
        $ILBStaticIP = "<MyILBStaticIPAddress>" # static IP address for the ILB in the subnet
        $ILBName = "AGListenerLB" # customize the ILB name or use this default value

        # Create the ILB
        Add-AzureInternalLoadBalancer -InternalLoadBalancerName $ILBName -SubnetName $SubnetName -ServiceName $ServiceName -StaticVNetIPAddress $ILBStaticIP

        # Configure a load balanced endpoint for each node in $AGNodes using ILB
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -LBSetName "ListenerEndpointLB" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ILBName -DirectServerReturn $true | Update-AzureVM
        }

1. Po ustawieniu zmiennych, skopiuj skrypt z edytora tekstu do sesji programu Azure PowerShell, aby go uruchomić. Jeśli nadal wyświetlany jest monit >>, wpisz ENTER ponownie, aby upewnić się, uruchamiania skrypt. Uwaga

>[AZURE.NOTE] Klasyczny portalu Azure nie obsługuje wewnętrznych równoważenia obciążenia w tej chwili, więc nie zostaną wyświetlone ILB lub punkty końcowe w portalu klasyczny Azure. Jednak **Get-AzureEndpoint** Zwraca wewnętrzny adres IP, jeśli równoważenia obciążenia działa on. W przeciwnym razie zwraca wartość null.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Sprawdź, czy KB2854082 jest zainstalowany, w razie potrzeby

[AZURE.INCLUDE [kb2854082](../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>Otwieranie portów zapory w węzły grup dostępności

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>Tworzenie odbiornika grupy dostępności

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-create-listener.md)]

1. Dla ILB należy użyć adresu IP z wewnętrznych ładowania równoważenia (ILB) utworzony wcześniej. Poniższy skrypt umożliwia uzyskanie tego adresu IP w programie PowerShell.

        # Define variables
        $ServiceName="<MyServiceName>" # the name of the cloud service that contains the AG nodes
        (Get-AzureInternalLoadBalancer -ServiceName $ServiceName).IPAddress

1. Na jednym z pośrednictwem SMS Skopiuj skrypt programu PowerShell dla używanego systemu operacyjnego do edytora tekstów i ustaw wartości, które wspomniano wcześniej zmiennych.

    Dla systemu Windows Server 2012 lub nowszym należy użyć następującego skryptu:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB)

        Import-Module FailoverClusters

        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
        
    W przypadku systemu Windows Server 2008 R2 Użyj tego skryptu:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB)

        Import-Module FailoverClusters

        cluster res $IPResourceName /priv enabledhcp=0 address=$ILBIP probeport=59999  subnetmask=255.255.255.255
    

1. Po ustawiono zmiennych, otwórz podwyższonym poziomem okno programu Windows PowerShell, a następnie skopiować skrypt w edytorze tekstów i Wklej do sesji programu Azure PowerShell, aby go uruchomić. Jeśli nadal wyświetlany jest monit >>, wpisz ENTER ponownie, aby upewnić się, uruchamiania skrypt.

2. Powtórz tę czynność na każdym maszyn wirtualnych. Ten skrypt konfiguruje zasobu adres IP przy użyciu adresu IP usługi w chmurze i ustawia inne parametry podobnie jak port sondy. Gdy tryb online zasobu adres IP, go następnie odpowiedzieć ankieta na porcie sondy od punktu końcowego równoważenia obciążenia utworzone wcześniej w tym samouczku.

## <a name="bring-the-listener-online"></a>Przesuń odbiornika w trybie online

[AZURE.INCLUDE [Bring-Listener-Online](../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Elementy monitorowania

[AZURE.INCLUDE [Follow-up](../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-vnet"></a>Testowanie odbiornika grupy dostępności (w tym samym VNet)

[AZURE.INCLUDE [Test-Listener-Within-VNET](../../includes/virtual-machines-ag-listener-test.md)]

## <a name="next-steps"></a>Następne kroki

[AZURE.INCLUDE [Listener-Next-Steps](../../includes/virtual-machines-ag-listener-next-steps.md)]
