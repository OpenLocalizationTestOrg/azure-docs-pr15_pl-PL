<properties 
   pageTitle="Sterowanie routingu i za pomocą urządzenia wirtualnego w Menedżerze zasobów za pomocą interfejsu wiersza polecenia Azure | Microsoft Azure"
   description="Dowiedz się, jak kontrolować routing i używanie wirtualnych urządzeń za pomocą interfejsu wiersza polecenia Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
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

#<a name="create-user-defined-routes-udr-in-the-azure-cli"></a>Tworzenie trasy (UDR) zdefiniowane przez użytkownika w polecenie Azure

[AZURE.INCLUDE [virtual-network-create-udr-arm-selectors-include.md](../../includes/virtual-network-create-udr-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]W tym artykule opisano, jak model wdrożenia Menedżera zasobów. Możesz również [utworzyć UDRs w modelu Klasyczny wdrożenia](virtual-network-create-udr-classic-cli.md).

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Poniższe przykładowe polecenie Azure polecenia przewiduje środowisku prosty już utworzone w zależności od tego scenariusza powyżej. Jeśli chcesz uruchomić polecenia wyświetlaną w tym dokumencie, najpierw utworzyć środowisku testowym Wdrażając [tego szablonu](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), kliknij pozycję **Deploy Azure**, Zamień wartości domyślne parametrów w razie potrzeby i postępuj zgodnie z instrukcjami w portalu.

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Tworzenie UDR podsieci fronton
Aby utworzyć tabelę routingu i rozsyłania wymagane przez podsieci fronton według tego scenariusza powyżej, wykonaj poniższe czynności.

3. Uruchamianie **`azure network route-table create`** polecenie, aby utworzyć tabelę rozsyłania podsieci frontonu.

        azure network route-table create -g TestRG -n UDR-FrontEnd -l uswest

    Wynik:

        info:    Executing command network route-table create
        info:    Looking up route table "UDR-FrontEnd"
        info:    Creating route table "UDR-FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    Name                            : UDR-FrontEnd
        data:    Type                            : Microsoft.Network/routeTables
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        info:    network route-table create command OK

    Parametry:
    - **-g (lub — grupa zasobów)**. Nazwa grupy zasobów, w której zostanie utworzony UDR. W naszym scenariuszu *TestRG*.
    - **-l (lub — lokalizacja)**. Azure region, w którym zostanie utworzone nowe UDR. W naszym scenariuszu *westus*.
    - **-n (lub — nazwa)**. Nazwa nowego UDR. W naszym scenariuszu *UDR FrontEnd*.

4. Uruchamianie **`azure network route-table route create`** polecenie, aby utworzyć trasę w tabeli rozsyłania utworzony w poprzednim przykładzie do wysyłania całego ruchu przeznaczone do podsieci wewnętrznej (192.168.2.0/24), aby **FW1** maszyn wirtualnych (192.168.0.4).

        azure network route-table route create -g TestRG -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -y VirtualAppliance -p 192.168.0.4

    Wynik:

        info:    Executing command network route-table route create
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        info:    Creating route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd/routes/RouteToBackEnd
        data:    Name                            : RouteToBackEnd
        data:    Provisioning state              : Succeeded
        data:    Next hop type                   : VirtualAppliance
        data:    Next hop IP address             : 192.168.0.4
        data:    Address prefix                  : 192.168.2.0/24
        info:    network route-table route create command OK

    Parametry:
    - **-r (lub — rozsyłania tabeli nazw)**. Nazwa tabeli rozsyłania miejsce, w którym zostaną dodane trasę. W naszym scenariuszu *UDR FrontEnd*.
    - **(lub — prefiksu adresu)**. Prefiks adresu podsieci, gdzie pakiety są przeznaczone do. W naszym scenariuszu *192.168.2.0/24*.
    - **-y (lub — dalej przeskoku typu)**. Typ obiektu ruchu będą wysyłane do. Możliwe wartości to *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*lub *Brak*.
    - **-p (lub--dalej przeskoku adres ip**). Adres IP następnego przeskoku. W naszym scenariuszu *192.168.0.4*.

5. Uruchamianie **`azure network vnet subnet set`** polecenie, aby skojarzyć tabelę routingu utworzony w poprzednim przykładzie podsieci **serwera sieci Web** .

        azure network vnet subnet set -g TestRG -e TestVNet -n FrontEnd -r UDR-FrontEnd

    Wynik:

        info:    Executing command network vnet subnet set
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        info:    Setting subnet "FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    Route Table                     : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    IP configurations:
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConf
        igurations/ipconfig1
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB2/ipConf
        igurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK

    Parametry:
    - **-e (lub nazwa — vnet)**. Nazwa VNet, w którym znajduje się podsieci. W naszym scenariuszu *TestVNet*.
 
## <a name="create-the-udr-for-the-back-end-subnet"></a>Tworzenie UDR podsieci wewnętrznej
Aby utworzyć tabelę routingu i rozsyłania wymagane przez podsieci wewnętrznej według tego scenariusza powyżej, wykonaj poniższe czynności.

1. Uruchamianie **`azure network route-table create`** polecenie, aby utworzyć tabelę rozsyłania podsieci wewnętrznej.

        azure network route-table create -g TestRG -n UDR-BackEnd -l westus

4. Uruchamianie **`azure network route-table route create`** polecenie, aby utworzyć trasę w tabeli rozsyłania utworzony w poprzednim przykładzie do wysyłania całego ruchu przeznaczone do podsieci fronton (192.168.1.0/24), aby **FW1** maszyn wirtualnych (192.168.0.4).

        azure network route-table route create -g TestRG -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -y VirtualAppliance -p 192.168.0.4

5. Uruchamianie **`azure network vnet subnet set`** polecenie, aby skojarzyć tabelę routingu utworzony w poprzednim przykładzie podsieci **wewnętrznej bazy danych** .

        azure network vnet subnet set -g TestRG -e TestVNet -n BackEnd -r UDR-BackEnd

## <a name="enable-ip-forwarding-on-fw1"></a>Włącz przesyłanie dalej IP na FW1
Aby włączyć przekazywanie w NIC używane przez **FW1**IP, wykonaj poniższe czynności.

1. Uruchamianie **`azure network nic show`** polecenia i zwróć wartość do **przesyłania Włącz adresów IP**. Należy ustawić na *wartość false*.

        azure network nic show -g TestRG -n NICFW1

    Wynik:
        
        info:    Executing command network nic show
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : false
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic show command OK

2. Uruchamianie **`azure network nic set`** polecenia umożliwia przekazywanie adresów IP.

        azure network nic set -g TestRG -n NICFW1 -f true

    Wynik:

        info:    Executing command network nic set
        info:    Looking up the network interface "NICFW1"
        info:    Updating network interface "NICFW1"
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : true
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic set command OK

    Parametry:

    - **-f (lub — Włącz przesyłanie dalej ip)**. *wartość PRAWDA* lub *FAŁSZ*.
