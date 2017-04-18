Można sprawdzić, czy połączenie powiodło się za pomocą `Get-AzureRmVirtualNetworkGatewayConnection` polecenia cmdlet, z lub bez `-Debug`. 

1. W poniższym przykładzie polecenia cmdlet, konfigurowanie wartości zgodnie z własną za pomocą. Jeśli zostanie wyświetlony monit, wybierz pozycję "A" do uruchamiania "Wszystkie". W tym przykładzie `-Name` odwołuje się do nazwy połączenia utworzone przez użytkownika, który ma zostać sprawdzone.

        Get-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnection -ResourceGroupName MyRG

2. Po zakończeniu polecenia cmdlet, należy wyświetlić wartości. W poniższym przykładzie stan połączenia jest wyświetlany jako "Połączono", a zobaczysz ingress i wyjściowego bajtów.

        Body:
        {
          "name": "MyGWConnection",
          "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/connections/MyGWConnection",
          "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "1c484f82-23ec-47e2-8cd8-231107450446b",
            "virtualNetworkGateway1": {
              "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/virtualNetworkGa
        teways/vnetgw1"
            },
            "localNetworkGateway2": {
              "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/localNetworkGate
        ways/LocalSite"
            },
            "connectionType": "IPsec",
            "routingWeight": 10,
            "sharedKey": "abc123",
            "connectionStatus": "Connected",
            "ingressBytesTransferred": 33509044,
            "egressBytesTransferred": 4142431
          }