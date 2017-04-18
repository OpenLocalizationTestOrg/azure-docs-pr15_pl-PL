<properties 
   pageTitle="Sterowanie routingu i używanie wirtualnych urządzenia za pomocą interfejsu wiersza polecenia Azure w modelu Klasyczny wdrożenia | Microsoft Azure"
   description="Dowiedz się, jak możesz sterować routing VNets za pomocą interfejsu wiersza polecenia Azure w modelu Klasyczny wdrażania"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

#<a name="control-routing-and-use-virtual-appliances-classic-using-the-azure-cli"></a>Sterowanie kierowaniu i używanie urządzeń wirtualnych (klasyczny) za pomocą interfejsu wiersza polecenia Azure

[AZURE.INCLUDE [virtual-network-create-udr-classic-selectors-include.md](../../includes/virtual-network-create-udr-classic-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]W tym artykule opisano, jak model klasyczny wdrożenia. Można również [kontroli routingu i za pomocą urządzenia wirtualnego w modelu wdrożenia Menedżera zasobów](virtual-network-create-udr-arm-cli.md).

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Poniższe przykładowe polecenie Azure polecenia przewiduje środowisku prosty już utworzone w zależności od tego scenariusza powyżej. Jeśli chcesz uruchomić polecenia wyświetlaną w tym dokumencie, Utwórz środowiska pokazano [Tworzenie VNet (klasyczny) za pomocą interfejsu wiersza polecenia Azure](virtual-networks-create-vnet-classic-cli.md).

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Tworzenie UDR podsieci fronton
Aby utworzyć tabelę routingu i rozsyłania wymagane przez podsieć fronton według tego scenariusza powyżej, wykonaj poniższe czynności.

1. Uruchamianie **`azure config mode`** Aby przełączyć się do trybu klasycznego.

        azure config mode asm

    Wynik:

        info:    New mode is asm

3. Uruchamianie **`azure network route-table create`** polecenie, aby utworzyć tabelę rozsyłania podsieci frontonu.

        azure network route-table create -n UDR-FrontEnd -l uswest

    Wynik:

        info:    Executing command network route-table create
        info:    Creating route table "UDR-FrontEnd"
        info:    Getting route table "UDR-FrontEnd"
        data:    Name                            : UDR-FrontEnd
        data:    Location                        : West US
        info:    network route-table create command OK

    Parametry:
    - **-l (lub — lokalizacja)**. Azure region, w którym zostanie utworzone nowe NSG. W naszym scenariuszu *westus*.
    - **-n (lub — nazwa)**. Nazwa nowego NSG. W naszym scenariuszu *NSG FrontEnd*.

4. Uruchamianie **`azure network route-table route set`** polecenie, aby utworzyć trasę w tabeli rozsyłania utworzony w poprzednim przykładzie do wysyłania całego ruchu przeznaczone do podsieci wewnętrznej (192.168.2.0/24), aby **FW1** maszyn wirtualnych (192.168.0.4).

        azure network route-table route set -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -t VirtualAppliance -p 192.168.0.4

    Wynik:

        info:    Executing command network route-table route set
        info:    Getting route table "UDR-FrontEnd"
        info:    Setting route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    network route-table route set command OK

    Parametry:
    - **-r (lub — rozsyłania tabeli nazw)**. Nazwa tabeli rozsyłania miejsce, w którym zostaną dodane trasę. W naszym scenariuszu *UDR FrontEnd*.
    - **(lub — prefiksu adresu)**. Prefiks adresu podsieci, gdzie pakiety są przeznaczone do. W naszym scenariuszu *192.168.2.0/24*.
    - **-t (lub — dalej przeskoku typu)**. Typ obiektu ruchu będą wysyłane do. Możliwe wartości to *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*lub *Brak*.
    - **-p (lub--dalej przeskoku adres ip**). Adres IP następnego przeskoku. W naszym scenariuszu *192.168.0.4*.

5. Uruchamianie **`azure network vnet subnet route-table add`** polecenie, aby skojarzyć tabelę routingu utworzony w poprzednim przykładzie podsieci **serwera sieci Web** .

        azure network vnet subnet route-table add -t TestVNet -n FrontEnd -r UDR-FrontEnd

    Wynik:

        info:    Executing command network vnet subnet route-table add
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        info:    Associating route table "UDR-FrontEnd" and subnet "FrontEnd"
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        data:    Route table name                : UDR-FrontEnd
        data:      Location                      : West US
        data:      Routes:
        info:    network vnet subnet route-table add command OK 

    Parametry:
    - **-t (lub nazwa — vnet)**. Nazwa VNet, w którym znajduje się podsieci. W naszym scenariuszu *TestVNet*.
    - **- n (lub--nazwa podsieci**. Nazwa tabeli routingu podsieci zostanie dodana do. W naszym scenariuszu dla *serwera sieci Web*.
 
## <a name="create-the-udr-for-the-back-end-subnet"></a>Tworzenie UDR podsieci wewnętrznej
Aby utworzyć tabelę routingu i rozsyłania wymagane przez podsieci wewnętrznej według tego scenariusza powyżej, wykonaj poniższe czynności.

3. Uruchamianie **`azure network route-table create`** polecenie, aby utworzyć tabelę rozsyłania podsieci wewnętrznej.

        azure network route-table create -n UDR-BackEnd -l uswest

4. Uruchamianie **`azure network route-table route set`** polecenie, aby utworzyć trasę w tabeli rozsyłania utworzony w poprzednim przykładzie do wysyłania całego ruchu przeznaczone do podsieci fronton (192.168.1.0/24), aby **FW1** maszyn wirtualnych (192.168.0.4).

        azure network route-table route set -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -t VirtualAppliance -p 192.168.0.4

5. Uruchamianie **`azure network vnet subnet route-table add`** polecenie, aby skojarzyć tabelę routingu utworzony w poprzednim przykładzie podsieci **wewnętrznej bazy danych** .

        azure network vnet subnet route-table add -t TestVNet -n BackEnd -r UDR-BackEnd

