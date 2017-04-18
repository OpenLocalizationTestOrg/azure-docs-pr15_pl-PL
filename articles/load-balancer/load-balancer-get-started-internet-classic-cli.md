<properties
   pageTitle="Wprowadzenie do tworzenia internetowej przeciwległych równoważenia obciążenia w modelu Klasyczny wdrażania za pomocą interfejsu wiersza polecenia Azure | Microsoft Azure"
   description="Dowiedz się, jak utworzyć internetową przeciwległych równoważenia obciążenia w modelu Klasyczny wdrażania za pomocą interfejsu wiersza polecenia Azure"
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

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-the-azure-cli"></a>Wprowadzenie do tworzenia internetowej przeciwległych równoważenia obciążenia (klasyczny) w polecenie Azure

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]W tym artykule opisano, jak model klasyczny wdrożenia. Można także [dowiedzieć się, jak utworzyć internetową przeciwległych równoważenia obciążenia za pomocą Menedżera zasobów Azure](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]


## <a name="step-by-step-creating-an-internet-facing-load-balancer-using-cli"></a>Krok po kroku tworzenia internetowej przeciwległych równoważenia obciążenia przy użyciu interfejsu wiersza polecenia

Ten przewodnik pokazano, jak utworzyć równoważenia obciążenia Internet według tego scenariusza powyżej.

1. Jeśli po raz pierwszy używasz polecenie Azure, zobacz [Instalowanie i konfigurowanie polecenie Azure](../../articles/xplat-cli-install.md) i postępuj zgodnie z instrukcjami do miejsca, w którym wybierz swoje konto Azure i subskrypcji.

2. Uruchom polecenie **Konfiguracja azure tryb** , aby przełączyć się do trybu klasycznego, tak jak pokazano poniżej.

        azure config mode asm

    Oczekiwany wynik:

        info:    New mode is asm


## <a name="create-endpoint-and-load-balancer-set"></a>Tworzenie punktu końcowego i konfigurowanie usługi równoważenia obciążenia

Scenariuszu założono maszyn wirtualnych "sieci Web 1" i nie utworzono "sieci Web 2".
Ten przewodnik spowoduje utworzenie zestawu równoważenia obciążenia przy użyciu porty 80 jako publicznej port i portu 80 jako port lokalny. Port sondy jest również skonfigurowane na porcie 80 i nosi nazwę zestawu równoważenia obciążenia "lbset".


### <a name="step-1"></a>Krok 1

Tworzenie pierwszego punktu końcowego i zestaw przy użyciu Usługa równoważenia obciążenia `azure network vm endpoint create` dla maszyn wirtualnych "sieci Web 1".

    azure vm endpoint create web1 80 -k 80 -o tcp -t 80 -b lbset

Parametry używane:

**-k** - port lokalny maszyn wirtualnych<br>
**-o** - Protocol (protokół)<BR>
**-t** - port sondy<BR>
**-b** - nazwa usługi równoważenia obciążenia<BR>

## <a name="step-2"></a>Krok 2

Dodaj drugi maszyny wirtualnej "sieci Web 2" do zestawu równoważenia obciążenia.

    azure vm endpoint create web2 80 -k 80 -o tcp -t 80 -b lbset

## <a name="step-3"></a>Krok 3

Upewnij się, z konfiguracji równoważenia obciążenia `azure vm show` .

    azure vm show web1

Dane wyjściowe będą:

    data:    DNSName "contoso.cloudapp.net"
    data:    Location "East US"
    data:    VMName "web1"
    data:    IPAddress "10.0.0.5"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-2015
    6-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "joaoma-1-web1-0-201509251804250879"
    data:    OSDisk mediaLink "https://XXXXXXXXXXXXXXX.blob.core.windows.
    /vhds/joaomatest-web1-2015-09-25.vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Se
    r-2012-R2-20150916-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "XXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 name "XXXXXXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    Network Endpoints 0 loadBalancedEndpointSetName "lbset"
    data:    Network Endpoints 0 localPort 80
    data:    Network Endpoints 0 name "tcp-80-80"
    data:    Network Endpoints 0 port 80
    data:    Network Endpoints 0 loadBalancerProbe port 80
    data:    Network Endpoints 0 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 0 loadBalancerProbe intervalInSeconds 15
    data:    Network Endpoints 0 loadBalancerProbe timeoutInSeconds 31
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 5986
    data:    Network Endpoints 1 name "PowerShell"
    data:    Network Endpoints 1 port 5986
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 3389
    data:    Network Endpoints 2 name "Remote Desktop"
    data:    Network Endpoints 2 port 58081
    info:    vm show command OK

## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a>Utworzyć punkt końcowy pulpitu zdalnego dla maszyny wirtualnej

Możesz utworzyć punkt końcowy pulpitu zdalnego przekazywania ruchu sieciowego z publicznej portu do portu lokalnego dla określonego maszyn wirtualnych za pomocą `azure vm endpoint create`.

    azure vm endpoint create web1 54580 -k 3389


## <a name="remove-virtual-machine-from-load-balancer"></a>Usuwanie maszyn wirtualnych usługi równoważenia obciążenia

Musisz usunąć punkt końcowy skojarzone z modułem równoważenia obciążenia z maszyny wirtualnej. Po usunięciu punkt końcowy maszyny wirtualnej nie należy do równoważenia obciążenia, ustaw już.

 W powyższym przykładzie można usunąć punkt końcowy utworzoną dla maszyn wirtualnych "sieci Web 1" z równoważenia obciążenia "lbset" za pomocą polecenia `azure vm endpoint delete`.

    azure vm endpoint delete web1 tcp-80-80


>[AZURE.NOTE] Można wyświetlić więcej opcji, aby zarządzać punkty końcowe za pomocą polecenia`azure vm endpoint --help`


## <a name="next-steps"></a>Następne kroki

[Przed rozpoczęciem konfigurowania równoważenia obciążenia wewnętrznych](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurowanie trybu rozkładu równoważenia obciążenia](load-balancer-distribution-mode.md)

[Konfigurowanie ustawienia przekroczenia limitu czasu bezczynności protokołu TCP dla usługi równoważenia obciążenia](load-balancer-tcp-idle-timeout.md)

