<properties 
   pageTitle="Jak ustawić statyczny adres IP prywatne w trybie ARM za pomocą interfejsu wiersza polecenia | Microsoft Azure"
   description="Opis statyczne adresy IP (spadku) i jak nimi zarządzać w trybie ARM za pomocą interfejsu wiersza polecenia"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-in-azure-cli"></a>Jak skonfigurować statyczny adres IP prywatne w polecenie Azure

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]W tym artykule opisano, jak model wdrożenia Menedżera zasobów. Można również [zarządzać statycznego adresu IP prywatne w modelu Klasyczny wdrożenia](virtual-networks-static-private-ip-classic-cli.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Poniższe przykładowe polecenie Azure polecenia przewiduje środowisku prosty już utworzone. Jeśli chcesz uruchomić polecenia wyświetlaną w tym dokumencie, należy najpierw utworzyć środowisku testowym opisanego w [Tworzenie vnet](virtual-networks-create-vnet-arm-cli.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Jak określić statyczny adres IP prywatne podczas tworzenia maszyn wirtualnych
Aby utworzyć maszyny o nazwie *DNS01* w podsieci *FrontEnd* VNet o nazwie *TestVNet* przy użyciu statycznego prywatne IP *192.168.1.101*, wykonaj następujące czynności:

1. Jeśli po raz pierwszy używasz polecenie Azure, zobacz [Instalowanie i konfigurowanie polecenie Azure](../xplat-cli-install.md) i postępuj zgodnie z instrukcjami do miejsca, w którym wybierz swoje konto Azure i subskrypcji.

2. Uruchom polecenie **Konfiguracja azure tryb** , aby przełączyć się do trybu Menedżera zasobów, tak jak pokazano poniżej.

        azure config mode arm

    Oczekiwany wynik:

        info:    New mode is arm

3. Uruchom **Tworzenie ip publicznej sieci azure** tworzenia publiczny adres IP dla maszyn wirtualnych. Lista wyświetlana po wynik wyjaśniono parametry używane.

        azure network public-ip create -g TestRG -n TestPIP -l centralus
    
    Oczekiwany wynik:

        info:    Executing command network public-ip create
        + Looking up the public ip "TestPIP"
        + Creating public ip address "TestPIP"
        + Looking up the public ip "TestPIP"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP
        data:    Name                            : TestPIP
        data:    Type                            : Microsoft.Network/publicIPAddresses
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Allocation method               : Dynamic
        data:    Idle timeout                    : 4
        info:    network public-ip create command OK

    - **-g (lub — grupa zasobów)**. Nazwa grupy zasobów publiczny adres IP zostanie utworzony w.
    - **-n (lub — nazwa)**. Nazwa publiczny adres IP.
    - **-l (lub — lokalizacja)**. Azure region, w której zostanie utworzony publiczny adres IP. W naszym scenariuszu *centralus*.

3. Uruchom polecenie **Utwórz sieć azure nic** , aby utworzyć NIC z statyczny adres IP prywatne. Lista wyświetlana po wynik wyjaśniono parametry używane.

        azure network nic create -g TestRG -n TestNIC -l centralus -a 192.168.1.101 -m TestVNet -k FrontEnd

    Oczekiwany wynik:

        + Looking up the network interface "TestNIC"
        + Looking up the subnet "FrontEnd"
        + Creating network interface "TestNIC"
        + Looking up the network interface "TestNIC"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
        data:    Name                            : TestNIC
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.1.101
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:
        info:    network nic create command OK

    - **(lub — prywatny adres ip)**. Prywatne adresem IP dla karty sieciowej.
    - **-m (lub — podsieci vnet nazw)**. Nazwa VNet, w której zostanie utworzony karty Sieciowej.
    - **-k (lub — nazwę podsieci)**. Nazwa podsieci, w której zostanie utworzony karty Sieciowej.

4. Uruchom polecenie **Utwórz azure maszyn wirtualnych** , aby utworzyć maszyn wirtualnych, przy użyciu publiczny adres IP i NIC utworzony w poprzednim przykładzie. Lista wyświetlana po wynik wyjaśniono parametry używane.

        azure vm create -g TestRG -n DNS01 -l centralus -y Windows -f TestNIC -i TestPIP -F TestVNet -j FrontEnd -o vnetstorage -q bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 -u adminuser -p AdminP@ssw0rd

    Oczekiwany wynik:

        info:    Executing command vm create
        + Looking up the VM "DNS01"
        info:    Using the VM Size "Standard_A1"
        warn:    The image "bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2" will be used for VM
        info:    The [OS, Data] Disk or image configuration requires storage account
        + Looking up the storage account vnetstorage
        + Looking up the NIC "TestNIC"
        info:    Found an existing NIC "TestNIC"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd" in the NIC "TestNIC"
        info:    Found public ip parameters, trying to setup PublicIP profile
        + Looking up the public ip "TestPIP"
        info:    Found an existing PublicIP "TestPIP"
        info:    Configuring identified NIC IP configuration with PublicIP "TestPIP"
        + Updating NIC "TestNIC"
        + Looking up the NIC "TestNIC"
        + Creating VM "DNS01"
        info:    vm create command OK

    - **-y (lub typ — systemu operacyjnego)**. Typ systemu operacyjnego dla Głosowa, *systemu Windows* i *Linux oraz*.
    - **-f (lub nazwa — nic)**. Nazwa sieciowa użyje maszyn wirtualnych.
    - **-i (lub — publicznej ip nazw)**. Nazwa publiczny adres IP użyje maszyn wirtualnych.
    - **-F (lub nazwa — vnet)**. Nazwa VNet, w której zostanie utworzony maszyn wirtualnych.
    - **-j (lub nazwa — vnet-podsieci)**. Nazwa podsieci, w której zostanie utworzony maszyn wirtualnych.

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Jak pobrać statyczne prywatne informacje o adresie IP dla maszyn wirtualnych

Aby wyświetlić statyczną prywatne informacje o adresie IP dla maszyn wirtualnych, utworzone za pomocą skryptu powyżej, uruchom następujące polecenie, polecenie Azure i obserwować wartości dla *metody alokacji prywatne IP* i *prywatny adres IP*:

    azure vm show -g TestRG -n DNS01

Oczekiwany wynik:

    info:    Executing command vm show
    + Looking up the VM "DNS01"
    + Looking up the NIC "TestNIC"
    + Looking up the public ip "TestPIP
    data:    Id                              :/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01
    data:    ProvisioningState               :Succeeded
    data:    Name                            :DNS01
    data:    Location                        :centralus
    data:    Type                            :Microsoft.Compute/virtualMachines
    data:
    data:    Hardware Profile:
    data:      Size                          :Standard_A1
    data:
    data:    Storage Profile:
    data:      Source image:
    data:        Id                          :/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/services/images/bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
    data:
    data:      OS Disk:
    data:        OSType                      :Windows
    data:        Name                        :cli08d7bd987a0112a8-os-1441774961355
    data:        Caching                     :ReadWrite
    data:        CreateOption                :FromImage
    data:        Vhd:
    data:          Uri                       :https://vnetstorage2.blob.core.windows.net/vhds/cli08d7bd987a0112a8-os-1441774961355vhd
    data:
    data:    OS Profile:
    data:      Computer Name                 :DNS01
    data:      User Name                     :adminuser
    data:      Windows Configuration:
    data:        Provision VM Agent          :true
    data:        Enable automatic updates    :true
    data:
    data:    Network Profile:
    data:      Network Interfaces:
    data:        Network Interface #1:
    data:          Id                        :/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
    data:          Primary                   :true
    data:          MAC Address               :00-0D-3A-90-1A-A8
    data:          Provisioning State        :Succeeded
    data:          Name                      :TestNIC
    data:          Location                  :centralus
    data:            Private IP alloc-method :Static
    data:            Private IP address      :192.168.1.101
    data:            Public IP address       :40.122.213.159
    info:    vm show command OK

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Jak usunąć statycznego adresu IP prywatne z maszyny
Nie można usunąć statycznego adresu IP prywatne z NIC w Azure interfejsu wiersza polecenia dla Menedżera zasobów. Należy Tworzenie nowej karty Sieciowej korzystającego z dynamicznego IP, usuwanie poprzedniego NIC z maszyn wirtualnych, a następnie dodaj nowe NIC do maszyn wirtualnych. Aby zmienić Sieciowej w celu int maszyn wirtualnych używane eh poleceń powyżej, wykonaj poniższe czynności.
    
1. Uruchom polecenie **Utwórz sieć azure nic** , aby utworzyć nowy NIC, przy użyciu przydzielania adresów IP. Zwróć uwagę, jak nie musisz określić adres IP, tym razem.

        azure network nic create -g TestRG -n TestNIC2 -l centralus -m TestVNet -k FrontEnd

    Oczekiwany wynik:

        info:    Executing command network nic create
        + Looking up the network interface "TestNIC2"
        + Looking up the subnet "FrontEnd"
        + Creating network interface "TestNIC2"
        + Looking up the network interface "TestNIC2"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2
        data:    Name                            : TestNIC2
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.1.6
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:
        info:    network nic create command OK

2. Uruchom polecenie **Ustaw azure maszyn wirtualnych** , aby zmienić NIC używane przez maszyn wirtualnych.

        azure vm set -g TestRG -n DNS01 -N TestNIC2

    Oczekiwany wynik:

        info:    Executing command vm set
        + Looking up the VM "DNS01"
        + Looking up the NIC "TestNIC2"
        + Updating VM "DNS01"
        info:    vm set command OK

3. Jeśli chce, uruchom polecenie **Usuń azure sieci nic** , aby usunąć stare karty sieciowej.

        azure network nic delete -g TestRG -n TestNIC --quiet

    Oczekiwany wynik:

        info:    Executing command network nic delete
        + Looking up the network interface "TestNIC"
        + Deleting network interface "TestNIC"
        info:    network nic delete command OK

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Jak dodać statycznego adresu IP prywatne do istniejącego maszyn wirtualnych
Aby dodać statycznego adresu IP prywatne NIC używane przez maszyn wirtualnych, utworzony przy użyciu skrypt powyżej, uruchom następujące polecenie:

    azure network nic set -g TestRG -n TestNIC2 -a 192.168.1.101

Oczekiwany wynik:

    info:    Executing command network nic set
    + Looking up the network interface "TestNIC2"
    + Updating network interface "TestNIC2"
    + Looking up the network interface "TestNIC2"
    data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2
    data:    Name                            : TestNIC2
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : centralus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-90-29-25
    data:    Enable IP forwarding            : false
    data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01
    data:    IP configurations:
    data:      Name                          : NIC-config
    data:      Provisioning state            : Succeeded
    data:      Private IP address            : 192.168.1.101
    data:      Private IP Allocation Method  : Static
    data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

## <a name="next-steps"></a>Następne kroki

- Informacje na temat zastrzeżone publiczne adresy [IP](virtual-networks-reserved-public-ip.md) .
- Informacje na temat adresów [wystąpienie poziomu publicznych adresów IP (ILPIP)](virtual-networks-instance-level-public-ip.md) .
- Zwróć się [interfejsy API pozostałych zastrzeżony adres IP](https://msdn.microsoft.com/library/azure/dn722420.aspx).
