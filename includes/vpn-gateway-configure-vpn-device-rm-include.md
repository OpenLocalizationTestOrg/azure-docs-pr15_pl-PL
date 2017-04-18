
Aby skonfigurować urządzenia VPN, musisz publiczny adres IP bramy wirtualnej sieci dotyczące konfigurowania urządzenia VPN lokalnego. Praca z producenta urządzenia, aby uzyskać informacje o konfiguracji określonego i skonfiguruj urządzenie. Odwołują się do [Urządzenia VPN](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md) uzyskać więcej informacji o urządzeniach sieci VPN, które podczas pracy z Azure.

Aby znaleźć publiczny adres IP Centrum wirtualnej sieci przy użyciu programu PowerShell, użyj Poniższy przykładowy:

    Get-AzureRmPublicIpAddress -Name GW1PublicIP -ResourceGroupName TestRG

Możesz także wyświetlić publiczny adres IP dla usługi bramy wirtualnej sieci przy użyciu Azure portal. Przejdź do **wirtualnej sieci bram**, a następnie kliknij nazwę swojego bramy.