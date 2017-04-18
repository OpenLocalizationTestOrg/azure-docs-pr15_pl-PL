Instrukcje dotyczące tego zadania za pomocą VNet na podstawie wartości poniżej. Nazwy i dodatkowe ustawienia podane są też na tej liście. Firma Microsoft nie skorzystać z tej listy bezpośrednio w instrukcje, mimo że dodajemy zmiennych na podstawie wartości na tej liście. Możesz skopiować z listy, aby użyć jako odwołanie, zamieniając wartości na własny.

Lista referencyjna konfiguracji:
    
- Nazwa wirtualną sieć = "TestVNet"
- Wirtualna sieć przestrzeni adresów = 192.168.0.0/16
- Grupa zasobów = "TestRG"
- Nazwa podsieć1 = "FrontEnd" 
- Przestrzeń adresów podsieć1 = "192.168.0.0/16"
- Nazwa podsieci bramy: "GatewaySubnet" zawsze należy nadać nazwę podsieci bramy *GatewaySubnet*.
- Przestrzeń adresów podsieci bramy = "192.168.200.0/26"
- Region = "Wschodniego US"
- Nazwa bramy = "GW"
- Nazwa bramy IP = "GWIP"
- Konfiguracja IP bramy nazwa = "gwipconf"
-  Typ = "ExpressRoute" ten typ jest wymagane na potrzeby konfiguracji ExpressRoute.
- Nazwa IP publicznej bramy = "gwpip"


## <a name="add-a-gateway"></a>Dodawanie bram

1. Podłącz do subskrypcji usługi Azure. 

        Login-AzureRmAccount
        Get-AzureRmSubscription 
        Select-AzureRmSubscription -SubscriptionName "Name of subscription"

2. Deklarowanie zmiennych w tym celu. W tym przykładzie użyje Użyj zmiennych w przykładzie poniżej. Pamiętaj edytować tę opcję, aby uwzględnić ustawienia, których chcesz użyć. 
        
        $RG = "TestRG"
        $Location = "East US"
        $GWName = "GW"
        $GWIPName = "GWIP"
        $GWIPconfName = "gwipconf"
        $VNetName = "TestVNet"

3. Należy zapisać obiekt wirtualną sieć jako zmienna.

        $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG

4. Dodawanie podsieci bramy do wirtualnej sieci. Podsieć bramy musi mieć nazwę "GatewaySubnet". Warto utworzyć bramę, która jest /27 lub większa (/ 26, / 25, itp.).
            
        Add-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet -AddressPrefix 192.168.200.0/26

5. Ustawienie konfiguracji.

            Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

6. Przechowywanie podsieci bramy jako zmienna.

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet

7. Żądanie publiczny adres IP. Adres IP jest wymagany przed utworzeniem bramy. Nie można określić adres IP, który ma być używany; dynamiczne nadawany. Ten adres IP, które będą używane w następnej sekcji konfiguracji. AllocationMethod musi być dynamiczne.

        $pip = New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic

8. Tworzenie konfiguracji bramy sieci. Konfiguracji bramy definiuje podsieci i publiczny adres IP do użycia. W tym kroku określasz konfiguracji, który będzie używany podczas tworzenia bramy. Ten krok nie powoduje utworzenia obiektu bramy. Poniższe przykładowe umożliwia utworzenie konfiguracji bramy. 

        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip

9. Tworzenie bramy. W tym kroku **-GatewayType** jest szczególnie ważne. Należy użyć wartości **ExpressRoute**. Należy zauważyć, że po uruchomieniu tych poleceń cmdlet, bramy może potrwać 20 minut lub więcej, aby utworzyć.

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG -Location $Location -IpConfigurations $ipconf -GatewayType Expressroute -GatewaySku Standard

## <a name="verify-the-gateway-was-created"></a>Upewnij się, że brama została utworzona.

Aby zweryfikować, że brama została utworzona za pomocą polecenia poniżej.

    Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG

## <a name="resize-a-gateway"></a>Zmienianie rozmiaru bramy

Istnieje szereg [SKU bramy](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Następujące polecenie umożliwia w dowolnej chwili zmienić SKU bramy.

>[AZURE.IMPORTANT] To polecenie nie działa UltraPerformance bramy. Aby zmienić uzyskanie bramy UltraPerformance, najpierw usuń istniejący bramy ExpressRoute, a następnie utwórz Nowa brama UltraPerformance. Na starszą wersję Centrum z bramy UltraPerformance, najpierw usuń bramy UltraPerformance, a następnie utwórz Nowa brama.

    $gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance

## <a name="remove-a-gateway"></a>Usuwanie bramy

Użyj polecenia poniżej, aby usunąć bramę

    Remove-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG  
