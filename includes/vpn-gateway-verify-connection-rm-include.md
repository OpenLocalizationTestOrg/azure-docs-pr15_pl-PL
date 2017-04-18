### <a name="to-verify-your-connection-by-using-powershell"></a>Aby sprawdzić połączenie przy użyciu programu PowerShell

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

### <a name="to-verify-your-connection-by-using-the-azure-portal"></a>Aby sprawdzić połączenie przy użyciu Azure portal

Portal Azure możesz wyświetlić stan połączenia, przechodząc do połączenia. Istnieje kilka sposobów wykonaj następujące czynności. Poniższe kroki pokazują jeden sposób, aby przejść do połączenia i sprawdź, czy.

1. W [portalu Azure](http://portal.azure.com)kliknij opcję **wszystkie zasoby** i przejdź do Centrum wirtualnej sieci.
2. Na karta bramy usługi wirtualnej sieci kliknij przycisk **połączenia**. Można wyświetlić stan wszystkich połączeń.
3. Kliknij nazwę połączenia, który chcesz sprawdzić, aby otworzyć **Essentials**. Podstawowe informacje dotyczące możesz wyświetlić więcej informacji o połączeniu. **Stan** jest "Powiodło się" i "Pozostawanie w kontakcie" po wprowadzeniu połączenia.

    ![Sprawdź połączenie](./media/vpn-gateway-verify-connection-rm-include/connectionsucceeded.png)