Aby zmienić adres IP bramy, użyj `New-AzureRmVirtualNetworkGatewayConnection` polecenia cmdlet. Jak zachować nazwy bramy sieci lokalnej, dokładnie taka sama, jak nazwa istniejącego, ustawienia zostaną zastąpione. W tej chwili polecenia cmdlet "Ustawianie" nie obsługuje modyfikowania adresu IP bramy.

### <a name="gwipnoconnection"></a>Jak zmienić adres IP bramy — Brak połączenia bramy

Aby zaktualizować adres IP bramy dla swojej sieci lokalnej bramy, która jeszcze nie ma połączenia, użyj poniższego przykładu. Możesz także zaktualizować prefiksów adresów w tym samym czasie. Wybrane ustawienia zastępują istniejące ustawienia. Należy użyć istniejącej nazwy Centrum sieci lokalnej. Jeśli Cię nie trzeba utworzyć Nowa brama sieci lokalnej nie zastępowania istniejącego wzornika.

Za pomocą następującym przykładzie zamienianiu wartości do własnego.

    New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
    -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
    -GatewayIpAddress "5.4.3.2" -ResourceGroupName MyRGName


### <a name="gwipwithconnection"></a>Jak zmienić adres IP bramy - istniejące połączenie bramy

Jeśli połączenie bramy już istnieje, musisz najpierw usunąć połączenie. Następnie możesz zmienić adres IP bramy i ponownie utworzyć nowe połączenie. Spowoduje to niektóre przestoje połączenie VPN.


>[AZURE.IMPORTANT] Nie usuwaj Brama VPN. Jeśli zrobisz, musisz wrócić do czynności, aby utworzyć je ponownie, a także skonfigurowanie routera lokalnego o adresie IP, który zostanie przypisany do nowo utworzonej bramy.
 

1. Usuń połączenie. Możesz znaleźć nazwę połączenia za pomocą `Get-AzureRmVirtualNetworkGatewayConnection` polecenia cmdlet.

        Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
        -ResourceGroupName MyRGName

2. Zmień wartość GatewayIpAddress. W razie potrzeby, możesz zmodyfikować prefiksy do adresów w tej chwili. Należy zauważyć, że zastąpi istniejące ustawienia bramy sieci lokalnej. Użyj istniejącej nazwy bramy swojej sieci lokalnej, modyfikując tak, aby ustawienia zostaną zastąpione. Jeśli Cię nie trzeba utworzyć Nowa brama sieci lokalnej nie modyfikowanie istniejącego wzornika.

        New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
        -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
        -GatewayIpAddress "104.40.81.124" -ResourceGroupName MyRGName

3. Utwórz połączenie. W tym przykładzie możemy konfigurowania IPsec typu połączenia. Gdy odtworzyć połączenie, użyć typu połączenia, który jest określony dla danej konfiguracji. Dla typów dodatkowych połączeń zobacz stronę [polecenia cmdlet programu PowerShell](https://msdn.microsoft.com/library/mt603611.aspx) .  Aby uzyskać nazwę VirtualNetworkGateway, można uruchomić `Get-AzureRmVirtualNetworkGateway` polecenia cmdlet.

    Ustawienie zmiennych:

        $local = Get-AzureRMLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        $vnetgw = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName MyRGName

    Tworzenie połączenia:
    
        New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName `
        -Location "West US" `
        -VirtualNetworkGateway1 $vnetgw `
        -LocalNetworkGateway2 $local `
        -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

