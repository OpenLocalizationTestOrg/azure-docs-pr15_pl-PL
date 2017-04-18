## <a name="how-to-create-a-vnet-using-the-azure-cli"></a>Jak utworzyć VNet za pomocą interfejsu wiersza polecenia Azure

Polecenie Azure umożliwia zarządzanie zasobami Azure z wiersza polecenia z dowolnym komputerze z systemem Windows, Linux lub OSX. Aby utworzyć VNet za pomocą interfejsu wiersza polecenia Azure, wykonaj poniższe czynności.

1. Jeśli po raz pierwszy używasz polecenie Azure, zobacz [Instalowanie i konfigurowanie polecenie Azure](../articles/xplat-cli-install.md) i postępuj zgodnie z instrukcjami do miejsca, w którym wybierz swoje konto Azure i subskrypcji.
2. Uruchom polecenie **Konfiguracja azure tryb** , aby przełączyć się do trybu Menedżera zasobów, tak jak pokazano poniżej.

        azure config mode arm

    Oto przewidywaną powyżej polecenia:

        info:    New mode is arm

3. W razie potrzeby uruchom **Tworzenie grupy azure** Tworzenie nowej grupy zasobów, jak pokazano poniżej. Zwróć uwagę, dane wyjściowe polecenia. Lista wyświetlana po wynik wyjaśniono parametry używane. Aby uzyskać więcej informacji dotyczących grup zasobów odwiedź stronę [Azure Omówienie Menedżera zasobów](../articles/virtual-network/resource-group-overview.md#resource-groups).

        azure group create -n TestRG -l centralus

    Oto przewidywaną powyżej polecenia:

        info:    Executing command group create
        + Getting resource group TestRG
        + Creating resource group TestRG
        info:    Created resource group TestRG
        data:    Id:                  /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            centralus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

    - **-n (lub — nazwa)**. Nazwa nowej grupy zasobów. W naszym scenariuszu *TestRG*.
    - **-l (lub — lokalizacja)**. Region Azure miejsce, w którym można utworzyć nową grupę zasobów. W naszym scenariuszu *centralus*.

4. Uruchom polecenie **Utwórz vnet azure sieci** , aby utworzyć VNet i podsieci, tak jak pokazano poniżej. 

        azure network vnet create -g TestRG -n TestVNet -a 192.168.0.0/16 -l centralus

    Oto przewidywaną powyżej polecenia:

        info:    Executing command network vnet create
        + Looking up virtual network "TestVNet"
        + Creating virtual network "TestVNet"
        + Loading virtual network state
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet2
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        info:    network vnet create command OK

    - **-g (lub — grupa zasobów)**. Nazwa grupy zasobów, w której zostanie utworzony VNet. W naszym scenariuszu *TestRG*.
    - **-n (lub — nazwa)**. Nazwa VNet ma zostać utworzony. W naszym scenariuszu *TestVNet*
    - **(lub — prefiksów adresów)**. Lista bloków CIDR używane do obszaru adresów VNet. W naszym scenariuszu *192.168.0.0/16*
    - **-l (lub — lokalizacja)**. Azure region, w której zostanie utworzony VNet. W naszym scenariuszu *centralus*.

5. Uruchom polecenie **Utwórz azure vnet podsieci** , aby utworzyć podsieć, tak jak pokazano poniżej. Zwróć uwagę, dane wyjściowe polecenia. Lista wyświetlana po wynik wyjaśniono parametry używane.

        azure network vnet subnet create -g TestRG -e TestVNet -n FrontEnd -a 192.168.1.0/24

    Oto przewidywaną powyżej polecenia:

        info:    Executing command network vnet subnet create
        + Looking up the subnet "FrontEnd"
        + Creating subnet "FrontEnd"
        + Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:
        info:    network vnet subnet create command OK

    - **-e (lub nazwa — vnet**. Nazwa VNet, w której zostanie utworzony podsieci. W naszym scenariuszu *TestVNet*.
    - **-n (lub — nazwa)**. Nazwa nowej podsieci. W naszym scenariuszu dla *serwera sieci Web*.
    - **(lub — prefiksu adresu)**. Blok CIDR podsieci. Cztery naszych scenariusza, *192.168.1.0/24*.

6. Powtórz krok 5 powyżej, aby utworzyć innych podsieci, jeśli to konieczne. W naszym scenariuszu uruchom poniższe polecenie, aby utworzyć podsieci *wewnętrznej bazy danych* .

        azure network vnet subnet create -g TestRG -e TestVNet -n BackEnd -a 192.168.2.0/24

4. Uruchom polecenie **Pokaż vnet azure sieci** , aby wyświetlić właściwości nowego vnet, tak jak pokazano poniżej.

        azure network vnet show -g TestRG -n TestVNet

    Oto przewidywaną powyżej polecenia:

        info:    Executing command network vnet show
        + Looking up virtual network "TestVNet"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        data:    Subnets:
        data:      Name                          : FrontEnd
        data:      Address prefix                : 192.168.1.0/24
        data:
        data:      Name                          : BackEnd
        data:      Address prefix                : 192.168.2.0/24
        data:
        info:    network vnet show command OK
