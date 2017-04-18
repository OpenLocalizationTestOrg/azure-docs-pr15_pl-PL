<properties
    pageTitle="Konfigurowanie detektor zewnętrznych zawsze na grup dostępność | Microsoft Azure"
    description="Ten samouczek przeprowadzi Cię przez kroki tworzenia zawsze na dostępność grupy detektor platformy Azure zewnętrznie dostępnej za pomocą publiczny adres IP wirtualnego usług w chmurze skojarzone."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/12/2016"
    ms.author="MikeRayMSFT" />

# <a name="configure-an-external-listener-for-always-on-availability-groups-in-azure"></a>Konfigurowanie detektor zewnętrznych zawsze na grup dostępność platformy Azure

> [AZURE.SELECTOR]
- [Odbiornik wewnętrznych](virtual-machines-windows-classic-ps-sql-int-listener.md)
- [Odbiornik zewnętrznych](virtual-machines-windows-classic-ps-sql-ext-listener.md)

W tym temacie pokazano, jak skonfigurować detektor zawsze na dostępność grupy, który jest dostępny zewnętrznie w Internecie. Jest to możliwe, kojarząc go adres **Publicznej wirtualnych IP (VIP)** usługi w chmurze z odbiornika.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Grupy dostępności mogą zawierać repliki lokalnego, tylko Azure lub obejmują zarówno w lokalnej, jak i Azure w przypadku konfiguracji hybrydowych. Azure repliki mogą znajdować się w tym samym regionie lub w wielu regionów przy użyciu wielu sieci wirtualnych (VNets). Poniższe kroki przyjęto założenie, masz już [skonfigurowane grupy dostępności](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md) , ale nie skonfigurowano detektor.

## <a name="guidelines-and-limitations-for-external-listeners"></a>Wskazówki i ograniczenia dotyczące detektory zewnętrznych

Zwróć uwagę następujące wskazówki dotyczące odbiornika grupy dostępność platformy Azure w przypadku wdrażania za pomocą chmury usługi wyraz pomiot VIP adresu:

- Dostępność odbiornika grupy jest obsługiwana w systemie Windows Server 2008 R2, Windows Server 2012 i Windows Server 2012 R2.

- Aplikacja klienta musi znajdować się na usługi w chmurze inny niż ten, który zawiera grupy dostępność maszyny wirtualne. Azure nie obsługuje bezpośredniego serwera zwrotu z klienta i serwera w tym samym usługi w chmurze.

- Domyślnie w tym artykule pokazano, jak skonfigurować jednego odbiornika do korzystania z adresu chmury usługi wirtualnych IP (VIP). Istnieje możliwość zarezerwować i utworzyć kilka adresów VIP usługi w chmurze. Umożliwia tworzenie wielu nasłuchujących każdego skojarzone z różnych VIP, wykonaj czynności podane w tym artykule. Aby uzyskać informacje na temat tworzenia wielu adresów IP Zobacz [Wielu służącą na usługi w chmurze](../load-balancer/load-balancer-multivip.md).

- Jeśli tworzysz detektor dla środowiska hybrydowego sieci lokalnej musi istnieć łączność z Internetem poza VPN witryny do witryny z Azure wirtualną sieć. W razie Azure podsieci, odbiornik grupy dostępność jest dostępny tylko przez publiczny adres IP usługi w chmurze odpowiednich.

- Tworzenie detektor zewnętrznych w tym samym usługi w chmurze, gdy masz również detektor wewnętrznych przy użyciu wewnętrznych równoważenia obciążenia (ILB) nie jest obsługiwane.

## <a name="determine-the-accessibility-of-the-listener"></a>Określanie dostępności odbiornika

[AZURE.INCLUDE [ag-listener-accessibility](../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

W tym artykule omówiono tworzenie odbiornika, która korzysta z **zewnętrznego równoważenia obciążenia**. Jeśli chcesz, aby odbiornik jest oznaczony jako prywatny wirtualnej sieci, zobacz wersji tym artykule opisano procedury dotyczące konfigurowania [odbiornik ILB](virtual-machines-windows-classic-ps-sql-int-listener.md)

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Tworzenie zwrotu równoważenia obciążenia punkty końcowe maszyn wirtualnych z serwerem bezpośrednie

Równoważenia obciążenia zewnętrznych używa wirtualnego publiczny adres IP wirtualnych usługi w chmurze, obsługującego pośrednictwem usługi SMS. Dlatego nie trzeba tworzyć lub konfigurować równoważenia obciążenia w tym przypadku.

[AZURE.INCLUDE [load-balanced-endpoints](../../includes/virtual-machines-ag-listener-load-balanced-endpoints.md)]

1. Skopiuj skrypt programu PowerShell poniżej do edytora tekstów i ustaw wartości zmiennych do potrzeb środowiska (domyślne udzielono Ci dla niektórych parametrów). Należy zauważyć, że grupy dostępność obejmuje Azure regionów, należy uruchomić skrypt raz w każdej centrum danych usługi w chmurze i węzłów, które znajdują się w tym centrum danych.

        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas

        # Configure a load balanced endpoint for each node in $AGNodes, with direct server return enabled
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -Protocol "TCP" -PublicPort 1433 -LocalPort 1433 -LBSetName "ListenerEndpointLB" -ProbePort 59999 -ProbeProtocol "TCP" -DirectServerReturn $true | Update-AzureVM
        }

1. Po ustawieniu zmiennych, skopiuj skrypt z edytora tekstu do sesji programu Azure PowerShell, aby go uruchomić. Jeśli nadal wyświetlany jest monit >>, wpisz ENTER ponownie, aby upewnić się, uruchamiania skrypt.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Sprawdź, czy KB2854082 jest zainstalowany, w razie potrzeby

[AZURE.INCLUDE [kb2854082](../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>Otwieranie portów zapory w węzły grup dostępności

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>Tworzenie odbiornika grupy dostępności

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-create-listener.md)]

1. W przypadku równoważenia obciążenia zewnętrznych spowoduje uzyskanie wirtualnych publicznego adresu IP usługi w chmurze, zawierający usługi repliki. Zaloguj się do portalu klasyczny Azure. Przejdź do usługi w chmurze, zawierający grupy dostępność maszyn wirtualnych. Otwieranie widoku **pulpitu nawigacyjnego** .

3. Zwróć uwagę na adres podany w obszarze **adres publicznej wirtualnych IP (VIP)**. Jeśli rozwiązanie obejmuje VNets, powtórz ten krok dla każdej usługi w chmurze, zawierający maszyn wirtualnych, w którym znajduje się replice.

4. Na jednym z pośrednictwem SMS Skopiuj skrypt programu PowerShell poniżej do edytora tekstów i ustawienie zmiennych do wartości, które wspomniano wcześniej.

        # Define variables
        $ClusterNetworkName = "<ClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $CloudServiceIP = "<X.X.X.X>" # Public Virtual IP (VIP) address of your cloud service

        Import-Module FailoverClusters

        # If you are using Windows Server 2012 or higher, use the Get-Cluster Resource command. If you are using Windows Server 2008 R2, use the cluster res command. Both commands are commented out. Choose the one applicable to your environment and remove the # at the beginning of the line to convert the comment to an executable line of code.

        # Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$CloudServiceIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"OverrideAddressMatch"=1;"EnableDhcp"=0}
        # cluster res $IPResourceName /priv enabledhcp=0 overrideaddressmatch=1 address=$CloudServiceIP probeport=59999  subnetmask=255.255.255.255


1. Po ustawiono zmiennych, otwórz podwyższonym poziomem okno programu Windows PowerShell, a następnie skopiować skrypt w edytorze tekstów i Wklej do sesji programu Azure PowerShell, aby go uruchomić. Jeśli nadal wyświetlany jest monit >>, wpisz ENTER ponownie, aby upewnić się, uruchamiania skrypt.

1. Powtórz tę czynność na każdym maszyn wirtualnych. Ten skrypt konfiguruje zasobu adres IP przy użyciu adresu IP usługi w chmurze i ustawia inne parametry podobnie jak port sondy. Gdy tryb online zasobu adres IP, go następnie odpowiedzieć ankieta na porcie sondy od punktu końcowego równoważenia obciążenia utworzone wcześniej w tym samouczku.

## <a name="bring-the-listener-online"></a>Przesuń odbiornika w trybie online

[AZURE.INCLUDE [Bring-Listener-Online](../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Elementy monitorowania

[AZURE.INCLUDE [Follow-up](../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-vnet"></a>Testowanie odbiornika grupy dostępności (w tym samym VNet)

[AZURE.INCLUDE [Test-Listener-Within-VNET](../../includes/virtual-machines-ag-listener-test.md)]

## <a name="test-the-availability-group-listener-over-the-internet"></a>Testowanie odbiornika grupy dostępności (w Internecie)

Aby uzyskać dostęp do odbiornika z zewnątrz wirtualnej sieci, należy używać równoważenia obciążenia zewnętrzne publicznej (opisane w tym temacie) zamiast ILB, która jest dostępne tylko w tym samym VNet. W parametrach połączenia należy określić nazwę usługi cloud. Na przykład gdyby usługi w chmurze przy użyciu nazwy *mycloudservice*instrukcji sqlcmd będzie się następująco:

    sqlcmd -S "mycloudservice.cloudapp.net,<EndpointPort>" -d "<DatabaseName>" -U "<LoginId>" -P "<Password>"  -Q "select @@servername, db_name()" -l 15

W przeciwieństwie do poprzedniego przykładu należy użyć uwierzytelnianie programu SQL, ponieważ wywołujący nie można używać uwierzytelniania systemu windows w Internecie. Aby uzyskać więcej informacji, zobacz [zawsze na dostępność grupy w maszyn wirtualnych Azure: scenariusze łączności klienta](http://blogs.msdn.com/b/sqlcat/archive/2014/02/03/alwayson-availability-group-in-windows-azure-vm-client-connectivity-scenarios.aspx). Przy użyciu funkcji uwierzytelniania SQL, upewnij się, że możesz utworzyć samej logowania na replikami. Aby uzyskać więcej informacji na temat rozwiązywania problemów logowania przy użyciu grup dostępność zobaczyć, [jak mapowanie logowania lub użyć zawiera użytkownik bazy danych SQL nawiązać połączenie z innymi replikami i zamapować na dostępność bazy danych](http://blogs.msdn.com/b/alwaysonpro/archive/2014/02/19/how-to-map-logins-or-use-contained-sql-database-user-to-connect-to-other-replicas-and-map-to-availability-databases.aspx).

Jeśli repliki zawsze na znajdują się w różnych podsieci, musisz określić klientów **MultisubnetFailover = True** w parametrach połączenia. Powoduje połączenie równoległe prób z replikami w różnych podsieci. Zauważ, że w tym scenariuszu zawiera wdrożenia zawsze na grupy dostępności krzyżowe region.

## <a name="next-steps"></a>Następne kroki

[AZURE.INCLUDE [Listener-Next-Steps](../../includes/virtual-machines-ag-listener-next-steps.md)]
