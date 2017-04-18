Musisz utworzyć VNet i podsieci bramy najpierw przed rozpoczęciem pracy następujące zadania. Zobacz artykuł [Konfigurowanie wirtualną sieć za pomocą portalu klasyczny](../articles/expressroute/expressroute-howto-vnet-portal-classic.md) Aby uzyskać więcej informacji.   

## <a name="add-a-gateway"></a>Dodawanie bram

Aby utworzyć bramę, należy użyć polecenia poniżej. Pamiętaj zastąpić wartości do własnego.

    New-AzureVirtualNetworkGateway -VNetName "MyAzureVNET" -GatewayName "ERGateway" -GatewayType Dedicated -GatewaySKU  Standard

## <a name="verify-the-gateway-was-created"></a>Upewnij się, że brama została utworzona.

Aby zweryfikować, że brama została utworzona za pomocą polecenia poniżej. To polecenie pobiera również ID bramy, który jest wymagany dla innych operacji.

    Get-AzureVirtualNetworkGateway

## <a name="resize-a-gateway"></a>Zmienianie rozmiaru bramy

Istnieje szereg [SKU bramy](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Następujące polecenie umożliwia w dowolnej chwili zmienić SKU bramy.

>[AZURE.IMPORTANT] To polecenie nie działa UltraPerformance bramy. Aby zmienić uzyskanie bramy UltraPerformance, najpierw usuń istniejący bramy ExpressRoute, a następnie utwórz Nowa brama UltraPerformance. Na starszą wersję Centrum z bramy UltraPerformance, najpierw usuń bramy UltraPerformance, a następnie utwórz Nowa brama. 

    Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance

## <a name="remove-a-gateway"></a>Usuwanie bramy

Użyj polecenia poniżej, aby usunąć bramę

    Remove-AzureVirtualNetworkGateway -GatewayId <Gateway ID>