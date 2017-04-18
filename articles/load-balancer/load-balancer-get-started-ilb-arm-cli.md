<properties
   pageTitle="Tworzenie równoważenia obciążenia wewnętrznych za pomocą interfejsu wiersza polecenia Azure w Menedżerze zasobów | Microsoft Azure"
   description="Dowiedz się, jak utworzyć równoważenia obciążenia wewnętrznych przy użyciu interfejsu wiersza polecenia Azure w Menedżerze zasobów"
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

# <a name="create-an-internal-load-balancer-by-using-the-azure-cli"></a>Tworzenie równoważenia obciążenia wewnętrznych za pomocą interfejsu wiersza polecenia Azure

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][model klasyczny wdrożenia](load-balancer-get-started-ilb-classic-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-solution-by-using-the-azure-cli"></a>Wdrażanie rozwiązania przy użyciu interfejsu wiersza polecenia Azure

Poniższe kroki pokazują sposób tworzenia równoważenia obciążenia z Internetu przy użyciu Menedżera zasobów Azure interfejsu wiersza polecenia. Przy użyciu Menedżera zasobów Azure każdego zasobu jest utworzony i skonfigurowany pojedynczo, a następnie umieść ze sobą, aby utworzyć zasób.

Musisz utworzyć i skonfigurować następujące obiekty do wdrożenia usługi równoważenia obciążenia:

- **Konfiguracja IP frontonu**: zawiera publicznych adresów IP dla ruchu sieci przychodzącego
- **Pula adresów wewnętrznej**: zawiera interfejsy (NIC), które umożliwiają maszyn wirtualnych otrzymywać ruch sieciowy z usługi równoważenia obciążenia
- **Zasady równoważenia obciążenia**: zawiera reguły, które mapowanie portem publicznej usługi równoważenia obciążenia na port w puli adresów wewnętrznej
- **Reguły przychodzące translatora adresów Sieciowych**: zawiera reguły, które mapowanie portem publicznej usługi równoważenia obciążenia na port dla określonej maszyny wirtualnej w puli adresów wewnętrznej
- **Sondy**: zawiera sondy kondycji, które są używane do sprawdzania dostępności wystąpienia maszyn wirtualnych w puli adresów wewnętrznej

Aby uzyskać więcej informacji zobacz [Azure Menedżera zasobów pomocy technicznej dotyczącej usługi równoważenia obciążenia](load-balancer-arm.md).

## <a name="set-up-cli-to-use-resource-manager"></a>Konfigurowanie interfejsu wiersza polecenia za pomocą Menedżera zasobów

1. Jeśli po raz pierwszy używasz polecenie Azure, zobacz [zainstalować i skonfigurować polecenie Azure](../../articles/xplat-cli-install.md). Postępuj zgodnie z instrukcjami do miejsca, w którym wybierz swoje konto Azure i subskrypcji.

2. Uruchom polecenie **Konfiguracja azure tryb** , aby przełączyć się do trybu Menedżera zasobów, w następujący sposób:

        azure config mode arm

    Oczekiwany wynik:

        info:    New mode is arm

## <a name="create-an-internal-load-balancer-step-by-step"></a>Tworzenie równoważenia obciążenia wewnętrznych krok po kroku

1. Zaloguj się do Azure.

        azure login

    Po wyświetleniu monitu wprowadź poświadczenia, Azure.

2. Zmienianie polecenia narzędzia do trybu Azure Menedżera zasobów.

        azure config mode arm

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Wszystkie zasoby w Menedżerze zasobów Azure są skojarzone z grupą zasobów. Jeśli nie zostało to zrobione jeszcze, należy utworzyć grupy zasobów.

    azure group create <resource group name> <location>

## <a name="create-an-internal-load-balancer-set"></a>Tworzenie zestawu równoważenia obciążenia wewnętrznych

1. Tworzenie równoważenia obciążenia wewnętrznych

    W następującym scenariuszu grupa zasobów o nazwie nrprg zostanie utworzona w regionu wschodniego USA.

        azure network lb create --name nrprg --location eastus

    >[AZURE.NOTE] Wszystkie zasoby dla wewnętrznego urządzenia do równoważenia obciążenia, takich jak wirtualnych sieci i podsieci wirtualnej sieci, muszą być w tej samej grupy zasobów i w tym samym regionie.

2. Tworzenie zewnętrzną adres IP dla usługi równoważenia obciążenia wewnętrzny.

    Adres IP, którego używasz, musi być z zakresu podsieci wirtualnej sieci.

        azure network lb frontend-ip create --resource-group nrprg --lb-name ilbset --name feilb --private-ip-address 10.0.0.7 --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet

3. Tworzenie puli adresów wewnętrznej.

        azure network lb address-pool create --resource-group nrprg --lb-name ilbset --name beilb

    Po zdefiniowaniu zewnętrzną adres IP i puli adresów wewnętrznej, można utworzyć reguły równoważenia obciążenia, reguł ruchu przychodzącego translatora adresów Sieciowych i sondy dostosowane kondycji.

4. Utwórz regułę równoważenia obciążenia usługi równoważenia obciążenia wewnętrzny.

    Po wykonaniu powyższych czynności, polecenie tworzy regułę równoważenia obciążenia słuchanie port 1433 w puli frontonu i wysyłania równoważenia obciążenia ruchu sieciowego do puli adresów wewnętrznej, również za pomocą portu 1433.

        azure network lb rule create --resource-group nrprg --lb-name ilbset --name ilbrule --protocol tcp --frontend-port 1433 --backend-port 1433 --frontend-ip-name feilb --backend-address-pool-name beilb

5. Tworzenie reguły przychodzące translatora adresów Sieciowych.

    Reguły przychodzące translatora adresów Sieciowych są używane do tworzenia punkty końcowe w równoważenia obciążenia, kierujące do wystąpienia określonego maszyn wirtualnych. Wykonanie powyższych czynności spowoduje utworzyć dwie reguły translatora adresów Sieciowych dla pulpitu zdalnego.

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule1 --protocol TCP --frontend-port 5432 --backend-port 3389

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule2 --protocol TCP --frontend-port 5433 --backend-port 3389

6. Tworzenie sondy kondycji dla usługi równoważenia obciążenia.

    Sonda kondycji sprawdza, czy wszystkie wystąpienia maszyn wirtualnych, aby upewnić się, że możesz wysłać ruch sieciowy. Wystąpienia maszyn wirtualnych o niepowodzeniu sondy testy zostanie usunięta z usługi równoważenia obciążenia, aż trafia do trybu online i wyboru sondy Określa, że jest uszkodzony.

        azure network lb probe create --resource-group nrprg --lb-name ilbset --name ilbprobe --protocol tcp --interval 300 --count 4

    >[AZURE.NOTE]Platformy Microsoft Azure używa statyczne, publicznie routingu adres IP protokołu IPv4 dla różnych scenariuszach administracyjnych. Adres IP jest 168.63.129.16. Ten adres IP nie powinno zostać zablokowane przez dowolnego zapory, ponieważ może to powodować nieoczekiwane działanie.
    >W odniesieniu do równoważenia obciążenia wewnętrznych Azure ten adres IP jest używany przez monitorowanie sondy z usługi równoważenia obciążenia do określenia stanu kondycji dla maszyn wirtualnych w zestawie równoważenia obciążenia. Jeśli grupa zabezpieczeń sieci służy do ograniczania ruchu do Azure maszyn wirtualnych w zestawie wewnętrznie równoważenia obciążenia lub zostanie zastosowany do wirtualnego podsieci, upewnij się, czy regułę zabezpieczeń sieci została dodana do zezwalanie na ruch z 168.63.129.16.

## <a name="create-nics"></a>Tworzenie nic

Musisz utworzyć nic (lub zmodyfikować istniejące) i kojarzenie ich reguły, zasady równoważenia obciążenia i sondy translatora adresów Sieciowych.

1. Tworzenie NIC o nazwie *można nic1 kg*, a następnie skojarzyć z puli adresów wewnętrznej *beilb* i reguły translatora adresów Sieciowych *rdp1* .

        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" --location eastus

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

2. Tworzenie NIC o nazwie *można nic2 kg*, a następnie skojarzyć z puli adresów wewnętrznej *beilb* i reguły translatora adresów Sieciowych *rdp2* .

        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" --location eastus

3. Tworzenie maszyny wirtualnej o nazwie *Degresywna 1*, a następnie skojarzyć z NIC o nazwie *można nic1 kg*. Konto miejsca do magazynowania o nazwie *web1nrp* jest tworzona przed uruchomieniem następujące polecenie:

        azure vm create --resource--resource-grouproup nrprg --name DB1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

    >[AZURE.IMPORTANT] Maszyny wirtualne w równoważenia obciążenia muszą być w tym samym zestawie dostępności. Używanie `azure availset create` Aby utworzyć dostępność zestaw.

4. Tworzenie maszyny wirtualnej (maszyn wirtualnych) o nazwie *DB2*, a następnie skojarzyć z NIC o nazwie *można nic2 kg*. Konto miejsca do magazynowania o nazwie *web1nrp* został utworzony przed uruchomieniem następujące polecenie.

        azure vm create --resource--resource-grouproup nrprg --name DB2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

## <a name="delete-a-load-balancer"></a>Usuwanie usługi równoważenia obciążenia

Aby usunąć równoważenia obciążenia, użyj następującego polecenia:

    azure network lb delete --resource-group nrprg --name ilbset

## <a name="next-steps"></a>Następne kroki

[Konfigurowanie trybu rozkładu równoważenia obciążenia przy użyciu źródła IP koligacji](load-balancer-distribution-mode.md)

[Konfigurowanie ustawienia przekroczenia limitu czasu bezczynności protokołu TCP dla usługi równoważenia obciążenia](load-balancer-tcp-idle-timeout.md)
