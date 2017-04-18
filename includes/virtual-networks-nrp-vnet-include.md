## <a name="virtual-network"></a>Wirtualna sieć
Zasoby wirtualne sieci (VNET) i podsieci pomoc w określeniu granicę zabezpieczeń dla obciążenia działa Azure. VNet jest określony przez zbiór spacje adres, uznany za CIDR bloków. 

>[AZURE.NOTE] Administratorzy sieci znają notacji CIDR. Jeśli nie znasz CIDR, [Dowiedz się więcej o](http://whatismyipaddress.com/cidr).

![VNet z wielu podsieci](./media/resource-groups-networking/Figure4.png)

VNets zawiera następujące właściwości.

|Właściwość|Opis|Przykładowe wartości|
|---|---|---|
|**addressSpace**|Kolekcja prefiksów adresów, które składają się VNet w notacji CIDR|192.168.0.0/16|
|**podsieci**|Kolekcja podsieci, które składają się VNet|zobacz [podsieci](#Subnets) poniżej.|
|**adres IP**|Adres IP przypisany do obiektu. Jest to właściwość tylko do odczytu.|104.42.233.77|

### <a name="subnets"></a>Podsieci
Podsieć jest zasób podrzędne VNet i ułatwia określ segmenty przestrzeni adresów w bloku CIDR, przy użyciu prefiksy adresów IP. NIC mogą być dodawane do podsieci lub połączony z pośrednictwem SMS, dostarczając łączności dla różnych obciążenia.

Podsieci zawiera następujące właściwości. 

|Właściwość|Opis|Przykładowe wartości|
|---|---|---|
|**addressPrefix**|Prefiks jeden adres, które składają się podsieci w notacji CIDR|192.168.1.0/24|
|**networkSecurityGroup**|NSG stosowane do danej podsieci|zobacz [NSGs](#Network-Security-Group)|
|**routeTable**|Tabela trasy stosowane do danej podsieci|zobacz [UDR](#Route-table)|
|**ipConfigurations**|Kolekcja obiektów configruation IP używane przez nic podłączony do podsieci|zobacz [UDR](#Route-table)|


Przykładowe VNet w formacie JSON:

    {
        "name": "TestVNet",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/virtualNetworks",
        "location": "westus",
        "tags": {
            "displayName": "VNet"
        },
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "addressSpace": {
                "addressPrefixes": [
                    "192.168.0.0/16"
                ]
            },
            "subnets": [
                {
                    "name": "FrontEnd",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "192.168.1.0/24",
                        "networkSecurityGroup": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd"
                        },
                        "routeTable": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-FrontEnd"
                        },
                        "ipConfigurations": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConfigurations/ipconfig1"
                            },
                            ...]
                    }
                },
                ...]
        }
    }

### <a name="additional-resources"></a>Dodatkowe zasoby

- Uzyskaj więcej informacji na temat [VNet](../articles/virtual-network/virtual-networks-overview.md).
- Zapoznaj się z [interfejsu API usługi REST dokumentacji](https://msdn.microsoft.com/library/azure/mt163650.aspx) dla VNets.
- Zapoznaj się z [interfejsu API usługi REST dokumentacji](https://msdn.microsoft.com/library/azure/mt163618.aspx) dla podsieci.