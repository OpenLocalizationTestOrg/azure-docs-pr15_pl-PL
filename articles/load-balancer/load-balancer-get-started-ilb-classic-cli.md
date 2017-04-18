<properties
   pageTitle="Tworzenie równoważenia obciążenia wewnętrznych, za pomocą interfejsu wiersza polecenia Azure w modelu Klasyczny wdrożenia | Microsoft Azure"
   description="Dowiedz się, jak utworzyć równoważenia obciążenia wewnętrznych, za pomocą interfejsu wiersza polecenia Azure w modelu Klasyczny wdrażania"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internal-load-balancer-classic-using-the-azure-cli"></a>Wprowadzenie do tworzenia wewnętrzny usługi równoważenia obciążenia (klasyczny) za pomocą interfejsu wiersza polecenia Azure

[AZURE.INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Dowiedz się, jak [wykonać te kroki przy użyciu modelu Menedżera zasobów](load-balancer-get-started-ilb-arm-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]


## <a name="to-create-an-internal-load-balancer-set-for-virtual-machines"></a>Aby utworzyć równoważenia obciążenia wewnętrznych ustaw dla maszyn wirtualnych

Aby utworzyć zestaw równoważenia obciążenia wewnętrznych i serwerów, które będą wysyłać ruch do niej, wykonaj następujące czynności:

1. Utwórz wystąpienie wewnętrznych równoważenia obciążenia który będzie punkt końcowy ruch przychodzący być równoważenia na serwerach: Konfigurowanie usługi równoważenia obciążenia obciążenia.

1. Dodaj punkty końcowe odpowiadające maszyn wirtualnych, które będzie otrzymywał ruch przychodzący.

1. Konfigurowanie serwerów, które będą wysyłane na ruch można zastosować równoważenia chcesz wysyłać ruch ich wirtualny adres IP (VIP) wystąpienia równoważenia obciążenia wewnętrznych obciążenia.

## <a name="step-by-step-creating-an-internal-load-balancer-using-cli"></a>Krok po kroku tworzenia równoważenia obciążenia wewnętrznych, za pomocą interfejsu wiersza polecenia

Ten przewodnik pokazano, jak utworzyć równoważenia obciążenia wewnętrznych, oparte na scenariusz powyżej.

1. Jeśli po raz pierwszy używasz polecenie Azure, zobacz [Instalowanie i konfigurowanie polecenie Azure](../../articles/xplat-cli-install.md) i postępuj zgodnie z instrukcjami do miejsca, w którym wybierz swoje konto Azure i subskrypcji.

2. Uruchom polecenie **Konfiguracja azure tryb** , aby przełączyć się do trybu klasycznego, tak jak pokazano poniżej.

        azure config mode asm

    Oczekiwany wynik:

        info:    New mode is asm


## <a name="create-endpoint-and-load-balancer-set"></a>Tworzenie punktu końcowego i konfigurowanie usługi równoważenia obciążenia

Scenariuszu założono maszyn wirtualnych "Degresywna 1" i "DB2" w usłudze w chmurze o nazwie "mytestcloud". Oba maszyn wirtualnych używają wirtualną sieć o nazwie Moje "testvnet" z podsieci "podsieci-1".

Ten przewodnik utworzy zestaw równoważenia obciążenia wewnętrznych przy użyciu portu 1433 jako prywatne port i 1433 jako port lokalny.

Jest to typowy scenariusz, w którym masz maszyn wirtualnych SQL na końcu Wstecz, aby mieć pewność, że serwer bazy danych nie będą dostępne, bezpośrednio przy użyciu publicznego adresu IP przy użyciu usługi równoważenia obciążenia wewnętrznych.


### <a name="step-1"></a>Krok 1

Tworzenie równoważenia obciążenia wewnętrznych zestaw przy użyciu `azure network service internal-load-balancer add`.

     azure service internal-load-balancer add -r mytestcloud -n ilbset -t subnet-1 -a 192.168.2.7

Parametry używane:

**-r** - nazwa usługi w chmurze<BR>
**-n** - nazwa usługi równoważenia obciążenia wewnętrznych<BR>
**-t** - Nazwa podsieci (samej podsieci w środowisku maszyn wirtualnych systemu, dodawanych do równoważenia obciążenia wewnętrznego)<BR>
**** - (opcjonalnie) dodawanie statycznego adresu IP prywatnych<BR>

Zapoznaj się z `azure service internal-load-balancer --help` uzyskać więcej informacji.

Możesz sprawdzić właściwości równoważenia obciążenia wewnętrznych, za pomocą polecenia `azure service internal-load-balancer list` *Nazwa usługi w chmurze*.

Poniżej przedstawiono przykład wyniku są następujące:

    azure service internal-load-balancer list my-testcloud
    info:    Executing command service internal-load-balancer list
    + Getting cloud service deployment
    data:    Name    Type     SubnetName  StaticVirtualNetworkIPAddress
    data:    ------  -------  ----------  -----------------------------
    data:    ilbset  Private  subnet-1    192.168.2.7
    info:    service internal-load-balancer list command OK


## <a name="step-2"></a>Krok 2

Ustawianie równoważenia obciążenia wewnętrznych można skonfigurować po dodaniu pierwszego punktu końcowego. Zostanie skojarzony punktu końcowego, maszyn wirtualnych oraz portu sondy do zestawu równoważenia obciążenia wewnętrznych w tym kroku.

    azure vm endpoint create db1 1433 -k 1433 tcp -t 1433 -r tcp -e 300 -f 600 -i ilbset

Parametry używane:

**-k** - port lokalny maszyn wirtualnych<BR>
**-t** - port sondy<BR>
**-r** - sondy Protocol (protokół)<BR>
**-e** - interwał sondy w sekundach<BR>
**-f** - limit czasu w sekundach <BR>
**-i** - nazwa równoważenia obciążenia wewnętrznych <BR>


## <a name="step-3"></a>Krok 3

Upewnij się, z konfiguracji równoważenia obciążenia `azure vm show` *nazwę maszyn wirtualnych*

    azure vm show DB1

Dane wyjściowe będą:

        azure vm show DB1
    info:    Executing command vm show
    + Getting virtual machines
    data:    DNSName "mytestcloud.cloudapp.net"
    data:    Location "East US 2"
    data:    VMName "DB1"
    data:    IPAddress "192.168.2.4"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-20151022-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "db1-DB1-0-201511120457370846"
    data:    OSDisk mediaLink "https://XXXX.blob.core.windows.net/vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-20151022-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "137.116.64.107"
    data:    VirtualIPAddresses 0 name "db1ContractContract"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    VirtualIPAddresses 1 address "192.168.2.7"
    data:    VirtualIPAddresses 1 name "ilbset"
    data:    Network Endpoints 0 localPort 5986
    data:    Network Endpoints 0 name "PowerShell"
    data:    Network Endpoints 0 port 5986
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "137.116.64.107"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 3389
    data:    Network Endpoints 1 name "Remote Desktop"
    data:    Network Endpoints 1 port 60173
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "137.116.64.107"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 1433
    data:    Network Endpoints 2 name "tcp-1433-1433"
    data:    Network Endpoints 2 port 1433
    data:    Network Endpoints 2 loadBalancerProbe port 1433
    data:    Network Endpoints 2 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 2 loadBalancerProbe intervalInSeconds 300
    data:    Network Endpoints 2 loadBalancerProbe timeoutInSeconds 600
    data:    Network Endpoints 2 protocol "tcp"
    data:    Network Endpoints 2 virtualIPAddress "192.168.2.7"
    data:    Network Endpoints 2 enableDirectServerReturn false
    data:    Network Endpoints 2 loadBalancerName "ilbset"
    info:    vm show command OK


## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a>Utworzyć punkt końcowy pulpitu zdalnego dla maszyny wirtualnej

Możesz utworzyć punkt końcowy pulpitu zdalnego przekazywania ruchu sieciowego z publicznej portu do portu lokalnego dla określonego maszyn wirtualnych za pomocą `azure vm endpoint create`.

    azure vm endpoint create web1 54580 -k 3389


## <a name="remove-virtual-machine-from-load-balancer"></a>Usuwanie maszyn wirtualnych usługi równoważenia obciążenia

Maszyny wirtualnej można usunąć z równoważenia obciążenia wewnętrznych, ustaw, usuwając skojarzone punktu końcowego. Po usunięciu punkt końcowy maszyny wirtualnej nie należą do równoważenia obciążenia, ustaw już.

 W powyższym przykładzie można usunąć punkt końcowy utworzoną dla maszyn wirtualnych "Degresywna 1" z równoważenia obciążenia wewnętrznych "ilbset" przy użyciu polecenia `azure vm endpoint delete`.

    azure vm endpoint delete DB1 tcp-1433-1433


Zapoznaj się z `azure vm endpoint --help` uzyskać więcej informacji.


## <a name="next-steps"></a>Następne kroki

[Konfigurowanie trybu rozkładu równoważenia obciążenia przy użyciu źródła IP koligacji](load-balancer-distribution-mode.md)

[Konfigurowanie ustawienia przekroczenia limitu czasu bezczynności protokołu TCP dla usługi równoważenia obciążenia](load-balancer-tcp-idle-timeout.md)