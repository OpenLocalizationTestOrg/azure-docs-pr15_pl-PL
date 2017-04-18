## <a name="traffic-manager-profile"></a>Menedżer ruchu profilu

Menedżer ruchu i jego podrzędne punktu końcowego zasobów umożliwiają routingu DNS do punktów końcowych w Azure i spoza niej Azure. Taki podział ruch podlega metody routingu zasad. Menedżer ruchu umożliwia również kondycji punktu końcowego monitorowania i ruch kierowane odpowiednio według kondycji punktu końcowego. 

| Właściwość | Opis |
|---|---|
|**trafficRoutingMethod**| Możliwe wartości są *Wydajność*, *ważone*i *priorytet* | 
| **dnsConfig** | Nazwy FQDN w profilu | 
| **Protocol (protokół)** | Protokół monitorowania, możliwe wartości są *HTTP* i *HTTPS*|
| **Port** | monitorowania |  
| **Ścieżka** | Ścieżka monitorowania |
| **Punkty końcowe** |  kontener dla zasobów punktu końcowego | 

### <a name="endpoint"></a>Punkt końcowy 

Punkt końcowy jest zasobem podrzędne profil Menedżera ruch. Reprezentuje usługę lub punkt końcowy sieci web do użytkownika, który jest rozpowszechniane na ruch na podstawie skonfigurowanych zasad w profilu ruch Menedżera zasobów. 

| Właściwość | Opis | 
|---|---| 
| **Typ** |  Typ punktu końcowego, możliwe wartości są *punktu końcowego Azure*, *Zewnętrzny punkt końcowy*i *Zagnieżdżonych punktu końcowego* | 
| **targetResourceId** |  publiczny adres IP punktu końcowego usługi lub sieci web. Może to być punktu końcowego Azure lub zewnętrzne. | 
| **Weight (waga)** | Waga punktu końcowego używane w zarządzanie ruchem. | 
| **Priority (priorytet)** | priorytet punktu końcowego, używane do definiowania akcji pracy awaryjnej |

Przykład: Menedżer ruchu w formacie Json: 


        {
            "apiVersion": "[variables('tmApiVersion')]",
            "type": "Microsoft.Network/trafficManagerProfiles",
            "name": "VMEndpointExample",
            "location": "global",
            "dependsOn": [
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '0')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '1')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '2')]",
            ],
            "properties": {
                "profileStatus": "Enabled",
                "trafficRoutingMethod": "Weighted",
                "dnsConfig": {
                    "relativeName": "[parameters('dnsname')]",
                    "ttl": 30
                },
                "monitorConfig": {
                    "protocol": "http",
                    "port": 80,
                    "path": "/"
                },
                "endpoints": [
                    {
                        "name": "endpoint0",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 0))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint1",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 1))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint2",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 2))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    }
                ]
            }
        }

 
## <a name="additional-resources"></a>Dodatkowe zasoby

Aby uzyskać więcej informacji, należy przeczytać [dokumentację interfejsu API usługi REST dla Menedżera ruch](https://msdn.microsoft.com/library/azure/mt163664.aspx) .
