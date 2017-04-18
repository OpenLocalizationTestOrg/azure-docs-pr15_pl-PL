<properties 
   pageTitle="Sterowanie routingu i używanie wirtualnych urządzenia przy użyciu programu PowerShell w modelu Klasyczny wdrożenia | Microsoft Azure"
   description="Dowiedz się, jak możesz sterować routing VNets przy użyciu programu PowerShell w modelu Klasyczny wdrażania"
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
   ms.date="02/02/2016"
   ms.author="jdial" />

#<a name="control-routing-and-use-virtual-appliances-classic-using-powershell"></a>Sterowanie routingu i używanie urządzeń wirtualnych (klasyczny) przy użyciu programu PowerShell

[AZURE.INCLUDE [virtual-network-create-udr-classic-selectors-include.md](../../includes/virtual-network-create-udr-classic-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]W tym artykule opisano, jak model klasyczny wdrożenia.

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Próbki Azure programu PowerShell poleceń poniżej oczekiwać środowisku prosty utworzone oparty na scenariusz powyżej. Jeśli chcesz uruchomić polecenia wyświetlaną w tym dokumencie, Utwórz środowiska pokazano [Tworzenie VNet (klasyczny) przy użyciu programu PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md).

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Tworzenie UDR podsieci fronton
Aby utworzyć tabelę routingu i rozsyłania wymagane przez podsieci fronton według tego scenariusza powyżej, wykonaj poniższe czynności.

3. Uruchamianie **`New-AzureRouteTable`** polecenia cmdlet, aby utworzyć tabelę rozsyłania podsieci frontonu.

        New-AzureRouteTable -Name UDR-FrontEnd `
            -Location uswest `
            -Label "Route table for front end subnet"

    Wynik:

        Name         Location   Label                          
        ----         --------   -----                          
        UDR-FrontEnd West US    Route table for front end subnet

4. Uruchamianie **`Set-AzureRoute`** polecenia cmdlet, aby utworzyć trasę w tabeli rozsyłania utworzony w poprzednim przykładzie do wysyłania całego ruchu przeznaczone do podsieci wewnętrznej (192.168.2.0/24), aby **FW1** maszyn wirtualnych (192.168.0.4).
    
        Get-AzureRouteTable UDR-FrontEnd `
            |Set-AzureRoute -RouteName RouteToBackEnd -AddressPrefix 192.168.2.0/24 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress 192.168.0.4

    Wynik:

        Name     : UDR-FrontEnd
        Location : West US
        Label    : Route table for frontend subnet
        Routes   : 
                   Name                 Address Prefix    Next hop type        Next hop IP address
                   ----                 --------------    -------------        -------------------
                   RouteToBackEnd       192.168.2.0/24    VirtualAppliance     192.168.0.4  

5. Uruchamianie **`Set-AzureSubnetRouteTable`** polecenia cmdlet, aby skojarzyć tabelę routingu utworzony w poprzednim przykładzie podsieci **serwera sieci Web** .

        Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
            -SubnetName FrontEnd `
            -RouteTableName UDR-FrontEnd
 
## <a name="create-the-udr-for-the-back-end-subnet"></a>Tworzenie UDR podsieci wewnętrznej
Aby utworzyć tabelę routingu i rozsyłania wymagane przez podsieci wewnętrznej według tego scenariusza powyżej, wykonaj poniższe czynności.

3. Uruchamianie **`New-AzureRouteTable`** polecenia cmdlet, aby utworzyć tabelę rozsyłania podsieci wewnętrznej.

        New-AzureRouteTable -Name UDR-BackEnd `
            -Location uswest `
            -Label "Route table for back end subnet"

4. Uruchamianie **`Set-AzureRoute`** polecenia cmdlet, aby utworzyć trasę w tabeli rozsyłania utworzony w poprzednim przykładzie do wysyłania całego ruchu przeznaczone do podsieci fronton (192.168.1.0/24), aby **FW1** maszyn wirtualnych (192.168.0.4).

        Get-AzureRouteTable UDR-BackEnd `
            |Set-AzureRoute -RouteName RouteToFrontEnd -AddressPrefix 192.168.1.0/24 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress 192.168.0.4

5. Uruchamianie **`Set-AzureSubnetRouteTable`** polecenia cmdlet, aby skojarzyć tabelę routingu utworzony w poprzednim przykładzie podsieci **wewnętrznej bazy danych** .

        Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
            -SubnetName BackEnd `
            -RouteTableName UDR-BackEnd

## <a name="enable-ip-forwarding-on-the-fw1-vm"></a>Włącz przesyłanie dalej IP na maszyn wirtualnych FW1
Aby włączyć przekazywanie w FW1 maszyn wirtualnych adresów IP, wykonaj poniższe czynności.

1. Uruchamianie **`Get-AzureIPForwarding`** polecenia cmdlet, aby Sprawdź stan przekazywania adresów IP

        Get-AzureVM -Name FW1 -ServiceName TestRGFW `
            | Get-AzureIPForwarding

    Wynik:

        Disabled

2. Uruchamianie **`Set-AzureIPForwarding`** polecenia umożliwia przekazywanie adresów IP dla *FW1* maszyn wirtualnych.

        Get-AzureVM -Name FW1 -ServiceName TestRGFW `
            | Set-AzureIPForwarding -Enable
