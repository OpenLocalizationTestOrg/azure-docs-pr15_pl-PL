<properties
   pageTitle="Tworzenie internetowej przeciwległych równoważenia obciążenia w Menedżerze zasobów za pomocą interfejsu wiersza polecenia Azure | Microsoft Azure"
   description="Dowiedz się, jak utworzyć internetową przeciwległych równoważenia obciążenia w Menedżerze zasobów za pomocą interfejsu wiersza polecenia Azure"
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

# <a name="creating-an-internal-load-balancer-using-the-azure-cli"></a>Tworzenie równoważenia obciążenia wewnętrznych, za pomocą interfejsu wiersza polecenia Azure

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]W tym artykule opisano, jak model wdrożenia Menedżera zasobów. Możesz także [dowiedzieć się, jak utworzyć internetową przeciwległych równoważenia obciążenia za pomocą klasyczny](load-balancer-get-started-internet-classic-portal.md)


[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-using-the-azure-cli"></a>Wdrożenie rozwiązania przy użyciu interfejsu wiersza polecenia Azure

Poniższe kroki pokazują sposób tworzenia internetowej przeciwległych równoważenia obciążenia Menedżera zasobów Azure za pomocą interfejsu wiersza polecenia. Przy użyciu Menedżera zasobów Azure każdego zasobu zostanie utworzona i skonfigurowana pojedynczo, następnie umieść ze sobą, aby utworzyć zasób.

Należy utworzyć i skonfigurować następujące obiekty do wdrożenia usługi równoważenia obciążenia:

- Zewnętrzną Konfiguracja IP — zawiera publicznych adresów IP dla ruchu sieci przychodzącego.
- Pula adresów wewnętrznej — zawiera interfejsów sieciowych (NIC) maszyn wirtualnych otrzymywać ruch sieciowy od usługi równoważenia obciążenia.
- Zasady równoważenia obciążenia - zawiera reguły mapowania portem publicznej usługi równoważenia obciążenia do portu w puli adresów wewnętrznej.
- Translatora adresów Sieciowych reguły przychodzące — zawiera reguły mapowanie portem publicznej usługi równoważenia obciążenia na port dla określonej maszyny wirtualnej w puli adresów wewnętrznej.
- Sondy — zawiera sondy kondycji umożliwia sprawdzanie dostępności wystąpień maszyn wirtualnych w puli adresów wewnętrznej.

Aby uzyskać więcej informacji, zobacz [Azure Menedżera zasobów pomocy technicznej dotyczącej usługi równoważenia obciążenia](load-balancer-arm.md).

## <a name="set-up-cli-to-use-resource-manager"></a>Konfigurowanie interfejsu wiersza polecenia za pomocą Menedżera zasobów

1. Jeśli po raz pierwszy używasz polecenie Azure, zobacz [Instalowanie i konfigurowanie polecenie Azure](../../articles/xplat-cli-install.md) i postępuj zgodnie z instrukcjami do miejsca, w którym wybierz swoje konto Azure i subskrypcji.

2. Uruchom polecenie **Konfiguracja azure tryb** , aby przełączyć się do trybu Menedżera zasobów, tak jak pokazano poniżej.

        azure config mode arm

    Oczekiwany wynik:

        info:    New mode is arm

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Tworzenie wirtualnych sieci i publiczny adres IP zewnętrzną puli adresów IP

1. Tworzenie wirtualnych sieci (VNet) o nazwie *NRPVnet* w lokalizacji wschodniego USA przy użyciu grupy zasobów o nazwie *NRPRG*.

        azure network vnet create NRPRG NRPVnet eastUS -a 10.0.0.0/16

    Tworzenie podsieci o nazwie *NRPVnetSubnet* z bloku CIDR 10.0.0.0/24 w *NRPVnet*.

        azure network vnet subnet create NRPRG NRPVnet NRPVnetSubnet -a 10.0.0.0/24

2. Tworzenie publiczny adres IP o nazwie *NRPPublicIP* może być używany przez zewnętrzną pulę adresów IP przy użyciu nazw DNS *loadbalancernrp.eastus.cloudapp.azure.com*. Poniższe polecenie używa typu alokacji statyczne i limit czasu bezczynności w minutach, po upływie 4.

        azure network public-ip create -g NRPRG -n NRPPublicIP -l eastus -d loadbalancernrp -a static -i 4

    >[AZURE.IMPORTANT]Jako nazwy FQDN równoważenia obciążenia zostanie użyty etykieta domeny publiczny adres IP. Ta zmiana klasyczny wdrożenia, używanym w chmurze usługi jako równoważenia obciążenia w pełni kwalifikowaną domeny nazwy (FQDN).
    >W tym przykładzie nazwy FQDN jest *loadbalancernrp.eastus.cloudapp.azure.com*.

## <a name="create-a-load-balancer"></a>Tworzenie równoważenia obciążenia

Następujące polecenie tworzy usługi równoważenia obciążenia o nazwie *NRPlb* w grupie zasobów *NRPRG* w *USA wschodniego* Azure lokalizacji.

    azure network lb create NRPRG NRPlb eastus

## <a name="create-a-front-end-ip-pool-and-a-backend-address-pool"></a>Tworzenie puli zewnętrzną IP i puli adresów wewnętrznej bazy danych

W tym przykładzie przedstawiono sposób tworzenia puli IP zewnętrzną otrzymuje przychodzący ruch sieciowy na równoważenia obciążenia i miejsce, w którym puli zewnętrzną wysyła ruch sieciowy równoważenia obciążenia puli IP wewnętrznej bazy danych.

1. Tworzenie puli adresów IP zewnętrzną kojarzenie publiczny adres IP utworzony w poprzednim kroku i równoważenia obciążenia.

        azure network lb frontend-ip create nrpRG NRPlb NRPfrontendpool -i nrppublicip

2. Konfigurowanie puli adresów wewnętrznej do odbierania ruch przychodzący z zewnętrzną puli adresów IP.

        azure network lb address-pool create NRPRG NRPlb NRPbackendpool

## <a name="create-lb-rules-nat-rules-and-probe"></a>Tworzenie reguł kg, reguł translatora adresów Sieciowych i sondy

W tym przykładzie tworzy następujące elementy.

- Aby przetłumaczyć cały ruch przychodzący na porcie 21 port 22<sup>1</sup> reguły translatora adresów Sieciowych
- Aby przetłumaczyć cały ruch przychodzący na porcie 23 do portu 22 reguły translatora adresów Sieciowych
- Reguła równoważenia obciążenia do saldo cały ruch przychodzący na porcie 80 do portu 80 na adresy w puli wewnętrznej.
- Reguła sondy, aby sprawdzić stan kondycji na stronie o nazwie *HealthProbe.aspx*.

<sup>1</sup> translatora adresów Sieciowych reguły są skojarzone z wystąpieniem określonego maszyn wirtualnych za usługi równoważenia obciążenia. Ruch sieciowy przychodzące do portu 21 są wysyłane do określonych maszyn wirtualnych w port 22 skojarzony z tą regułą translatora adresów Sieciowych. Dla reguły translatora adresów Sieciowych, musisz określić protocol (UDP lub TCP). Nie można przypisać obu protokołów do tego samego portu.

1. Tworzenie reguł translatora adresów Sieciowych.

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh1 --protocol tcp --frontend-port 21 --backend-port 22
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh2 --protocol tcp --frontend-port 23 --backend-port 22

2. Tworzenie reguły równoważenia obciążenia.

        azure network lb rule create nrprg nrplb lbrule -p tcp -f 80 -b 80 -t NRPfrontendpool -o NRPbackendpool

3. Tworzenie sondy kondycji.

        azure network lb probe create --resource-group nrprg --lb-name nrplb --name healthprobe --protocol "http" --port 80 --path healthprobe.aspx --interval 15 --count 4

4. Sprawdź ustawienia.

        azure network lb show nrprg nrplb

    Oczekiwany wynik:

        info:    Executing command network lb show
        + Looking up the load balancer "nrplb"
        + Looking up the public ip "NRPPublicIP"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb
        data:    Name                            : nrplb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : eastus
        data:    Provisioning State              : Succeeded
        data:    Frontend IP configurations:
        data:      Name                          : NRPfrontendpool
        data:      Provisioning state            : Succeeded
        data:      Public IP address id          : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/publicIPAddresses/NRPPublicIP
        data:      Public IP allocation method   : Static
        data:      Public IP address             : 40.114.13.145
        data:
        data:    Backend address pools:
        data:      Name                          : NRPbackendpool
        data:      Provisioning state            : Succeeded
        data:
        data:    Load balancing rules:
        data:      Name                          : HTTP
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 80
        data:      Backend port                  : 80
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:      Backend address pool          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:
        data:    Inbound NAT rules:
        data:      Name                          : ssh1
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 21
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:      Name                          : ssh2
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 23
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:    Probes:
        data:      Name                          : healthprobe
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Http
        data:      Port                          : 80
        data:      Interval in seconds           : 15
        data:      Number of probes              : 4
        data:
        info:    network lb show command OK

## <a name="create-nics"></a>Tworzenie nic

Musisz utworzyć nic (lub zmodyfikować istniejące) i kojarzenie ich reguły, zasady równoważenia obciążenia i sondy translatora adresów Sieciowych.

1. Tworzenie NIC o nazwie *można nic1 kg*i skojarzyć ją z reguły translatora adresów Sieciowych *rdp1* i puli adresów wewnętrznej *NRPbackendpool* .

        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" eastus

    Oczekiwany wynik:

        info:    Executing command network nic create
        + Looking up the network interface "lb-nic1-be"
        + Looking up the subnet "nrpvnetsubnet"
        + Creating network interface "lb-nic1-be"
        + Looking up the network interface "lb-nic1-be"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        data:    Name                            : lb-nic1-be
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : eastus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 10.0.0.4
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet
        data:      Load balancer backend address pools
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:      Load balancer inbound NAT rules:
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1
        data:
        info:    network nic create command OK

2. Tworzenie NIC o nazwie *można nic2 kg*i kojarzenie go z reguły translatora adresów Sieciowych *rdp2* i puli adresów wewnętrznej *NRPbackendpool* .

        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" eastus

3. Tworzenie maszyny wirtualnej (maszyn wirtualnych) o nazwie *sieci Web 1*i skojarzyć NIC o nazwie *można nic1 kg*. Konto miejsca do magazynowania o nazwie *web1nrp* został utworzony przed uruchomieniem polecenia poniżej.

        azure vm create --resource-group nrprg --name web1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

    >[AZURE.IMPORTANT] Maszyny wirtualne w równoważenia obciążenia muszą być w tym samym zestawie dostępności. Używanie `azure availset create` Aby utworzyć dostępność zestaw.

    Wynik powinien być podobny do następującego:

        info:    Executing command vm create
        + Looking up the VM "web1"
        Enter username: azureuser
        Enter password for azureuser: *********
        Confirm password: *********
        info:    Using the VM Size "Standard_A1"
        info:    The [OS, Data] Disk or image configuration requires storage account
        + Looking up the storage account web1nrp
        + Looking up the availability set "nrp-avset"
        info:    Found an Availability set "nrp-avset"
        + Looking up the NIC "lb-nic1-be"
        info:    Found an existing NIC "lb-nic1-be"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet" in the NIC "lb-nic1-be"
        info:    This is a NIC without publicIP configured
        + Creating VM "web1"
        info:    vm create command OK

    >[AZURE.NOTE] Okno komunikatu **to NIC bez publicIP skonfigurowane** jest planowane od NIC utworzone dla usługi równoważenia obciążenia nawiązywanie połączenia z Internetem przy użyciu obciążenia równoważenia publiczny adres IP.

    Ponieważ *można nic1 kg* NIC jest skojarzony z regułą *rdp1* translatora adresów Sieciowych, można nawiązać połączenie przy użyciu RDP poprzez port 3441 równoważenia obciążenia *sieci Web 1* .

4. Tworzenie maszyny wirtualnej (maszyn wirtualnych) o nazwie *sieci Web 2*i skojarzyć NIC o nazwie *można nic2 kg*. Konto miejsca do magazynowania o nazwie *web1nrp* został utworzony przed uruchomieniem polecenia poniżej.

        azure vm create --resource-group nrprg --name web2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

## <a name="update-an-existing-load-balancer"></a>Aktualizowanie istniejącego równoważenia obciążenia

Możesz dodać reguły odwoływanie się do istniejącej usługi równoważenia obciążenia. W następnym przykładzie nową regułę równoważenia obciążenia jest dodawany do istniejącej równoważenia obciążenia **NRPlb**

    azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule2 --protocol tcp --frontend-port 8080 --backend-port 8051 --frontend-ip-name frontendnrppool --backend-address-pool-name NRPbackendpool

## <a name="delete-a-load-balancer"></a>Usuwanie usługi równoważenia obciążenia

Aby usunąć równoważenia obciążenia, użyj następującego polecenia:

    azure network lb delete --resource-group nrprg --name nrplb

## <a name="next-steps"></a>Następne kroki

[Przed rozpoczęciem konfigurowania równoważenia obciążenia wewnętrznych](load-balancer-get-started-ilb-arm-cli.md)

[Konfigurowanie trybu rozkładu równoważenia obciążenia](load-balancer-distribution-mode.md)

[Konfigurowanie ustawienia przekroczenia limitu czasu bezczynności protokołu TCP dla usługi równoważenia obciążenia](load-balancer-tcp-idle-timeout.md)
