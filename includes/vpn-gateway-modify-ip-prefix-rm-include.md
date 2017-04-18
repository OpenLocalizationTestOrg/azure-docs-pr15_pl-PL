### <a name="noconnection"></a>Jak dodać lub usunąć prefiksy — Brak połączenia bramy

- **Aby dodać** prefiksów dodatkowych adresów lokalnie sieci bramy, która została utworzona, ale który nie jeszcze połączenie bramy, użyj poniższego przykładu. Pamiętaj zmienić wartości do własnego.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

- **Aby usunąć** prefiks adresu z bramy sieci lokalnej, która nie zawiera połączenie VPN, należy użyć w poniższym przykładzie. Opuszczenie prefiksów, które nie są już potrzebne. W tym przykładzie firma Microsoft nie jest już potrzebna prefiks 20.0.0.0/24 (w poprzednim przykładzie), więc zostanie odpowiednio zaktualizowana lokalnym sieci bramy i Wyklucz tego prefiksu.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','30.0.0.0/24')

### <a name="withconnection"></a>Jak dodać lub usunąć prefiksy - istniejące połączenie bramy

Jeśli utworzono połączenie bramy i chcesz dodać lub usunąć prefiksy adresów IP zawarte w Centrum sieci lokalnej, musisz wykonać następujące czynności w kolejności. Spowoduje to niektóre przestoje połączenie VPN. Podczas aktualizacji usługi prefiksy, będzie najpierw usunąć połączenie, modyfikowanie prefiksów, a następnie utwórz nowe połączenie. W poniższych przykładach należy pamiętać o zmianie wartości do własnego.

>[AZURE.IMPORTANT] Nie usuwaj Brama VPN. Jeśli zrobisz, musisz wrócić do czynności, aby utworzyć je ponownie, a także skonfigurowanie routera lokalnego z nowymi ustawieniami.
 
1. Usuń połączenie.

        Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName

2. Modyfikowanie prefiksów adresów dla Centrum sieci lokalnej.

    Ustaw zmienną dla LocalNetworkGateway.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName

    Modyfikowanie prefiksów.

        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

4. Utwórz połączenie. W tym przykładzie możemy konfigurowania IPsec typu połączenia. Gdy odtworzyć połączenie, użyć typu połączenia, który jest określony dla danej konfiguracji. Dla typów dodatkowych połączeń zobacz stronę [polecenia cmdlet programu PowerShell](https://msdn.microsoft.com/library/mt603611.aspx) .

    Ustaw zmienną dla VirtualNetworkGateway.

        $gateway1 = Get-AzureRmVirtualNetworkGateway -Name RMGateway  -ResourceGroupName MyRGName

    Utwórz połączenie. Należy zauważyć, że w tym przykładzie użyto zmiennej $local ustawiany w poprzednim kroku.


        New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
        -ResourceGroupName MyRGName -Location 'West US' `
        -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
        -ConnectionType IPsec `
        -RoutingWeight 10 -SharedKey 'abc123'
