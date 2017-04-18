## <a name="load-balancer"></a>Usługa równoważenia obciążenia
Równoważenia obciążenia jest używana, gdy chcesz skalować aplikacji. Typowe wdrożeń obejmować aplikacje na wielu wystąpień maszyn wirtualnych. Wystąpienia maszyn wirtualnych są fronted przez równoważenia obciążenia, który ułatwia rozpowszechnianie ruchu sieciowego do różnych wystąpień. 

![NIC na jednym maszyn wirtualnych](./media/resource-groups-networking/figure8.png)

| Właściwość | Opis |
|---|---|
| *frontendIPConfigurations* | usługi równoważenia obciążenia może zawierać jedną lub więcej adresów IP fronton, nazywanego wirtualne adresy IP (służącą). Te adresy IP służyć jako ingress ruchu i może być publiczny adres IP lub prywatne IP |
|*backendAddressPools* | są to adresy IP skojarzone z nic maszyn wirtualnych, do którego będą podzielone ładowania |
|*loadBalancingRules* | właściwości reguła mapy danego fronton IP i kombinację port do zestawu adresów IP wewnętrznej i kombinację port. Z jednej definicji zasobu usługi równoważenia obciążenia można zdefiniować wielu równoważenia reguły obciążenia, każda reguła odzwierciedlające kombinacja wierzch zakończyć adresu IP i portu i z powrotem w celu IP oraz portu skojarzonego z maszyn wirtualnych. Reguła jest jeden port w puli fronton do wiele maszyn wirtualnych w puli wewnętrznej |  
| *Sondy* | sondy umożliwiają umożliwia śledzenie kondycji wystąpień maszyn wirtualnych. Jeśli sonda kondycji nie powiedzie się, w wystąpieniu maszyn wirtualnych zostaną automatycznie przeniesione do poza obrotu |
| *inboundNatRules* | Reguły translatora adresów Sieciowych definiowania ruchu przychodzącego ułożony do przodu zakończenie IP i rozdzielana IP wewnętrznej wystąpieniu określonych maszyn wirtualnych. Reguła NAT jest jeden port w puli fronton maszyn wirtualnych w puli wewnętrznej | 

Przykład szablonu równoważenia obciążenia w formacie Json:

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "dnsNameforLBIP": {
          "type": "string",
          "metadata": {
            "description": "Unique DNS name"
          }
        },
        "location": {
          "type": "string",
          "allowedValues": [
            "East US",
            "West US",
            "West Europe",
            "East Asia",
            "Southeast Asia"
          ],
          "metadata": {
            "description": "Location to deploy"
          }
        },
        "addressPrefix": {
          "type": "string",
          "defaultValue": "10.0.0.0/16",
          "metadata": {
            "description": "Address Prefix"
          }
        },
        "subnetPrefix": {
          "type": "string",
          "defaultValue": "10.0.0.0/24",
          "metadata": {
            "description": "Subnet Prefix"
          }
        },
        "publicIPAddressType": {
          "type": "string",
          "defaultValue": "Dynamic",
          "allowedValues": [
            "Dynamic",
            "Static"
          ],
          "metadata": {
            "description": "Public IP type"
          }
        }
      },
      "variables": {
        "virtualNetworkName": "virtualNetwork1",
        "publicIPAddressName": "publicIp1",
        "subnetName": "subnet1",
        "loadBalancerName": "loadBalancer1",
        "nicName": "networkInterface1",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
        "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]",
        "publicIPAddressID": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]",
        "lbID": "[resourceId('Microsoft.Network/loadBalancers',variables('loadBalancerName'))]",
        "nicId": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]",
        "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/loadBalancerFrontEnd')]",
        "backEndIPConfigID": "[concat(variables('nicId'),'/ipConfigurations/ipconfig1')]"
      },
      "resources": [
    {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIPAddressName')]",
      "location": "[parameters('location')]",
      "properties": {
        "publicIPAllocationMethod": "[parameters('publicIPAddressType')]",
        "dnsSettings": {
          "domainNameLabel": "[parameters('dnsNameforLBIP')]"
        }
      }
    },
    {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[parameters('location')]",
      "properties": {
        "addressSpace": {
          "addressPrefixes": [
            "[parameters('addressPrefix')]"
          ]
        },
        "subnets": [
          {
            "name": "[variables('subnetName')]",
            "properties": {
              "addressPrefix": "[parameters('subnetPrefix')]"
            }
          }
        ]
      }
    },
    {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[parameters('location')]",
      "dependsOn": [
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
        "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "subnet": {
                "id": "[variables('subnetRef')]"
              },
              "loadBalancerBackendAddressPools": [
                {
                  "id": "[concat(variables('lbID'), '/backendAddressPools/LoadBalancerBackend')]"
                }
              ],
              "loadBalancerInboundNatRules": [
                {
                  "id": "[concat(variables('lbID'),'/inboundNatRules/RDP')]"
                }
              ]
            }
          }
        ]
      }
    },
    {
      "apiVersion": "2015-05-01-preview",
      "name": "[variables('loadBalancerName')]",
      "type": "Microsoft.Network/loadBalancers",
      "location": "[parameters('location')]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]"
      ],
      "properties": {
        "frontendIPConfigurations": [
          {
            "name": "loadBalancerFrontEnd",
            "properties": {
              "publicIPAddress": {
                "id": "[variables('publicIPAddressID')]"
              }
            }
          }
        ],
        "backendAddressPools": [
          {
            "name": "loadBalancerBackEnd"
          }
        ],
        "inboundNatRules": [
          {
            "name": "RDP",
            "properties": {
              "frontendIPConfiguration": {
                "id": "[variables('frontEndIPConfigID')]"
              },
              "protocol": "tcp",
              "frontendPort": 3389,
              "backendPort": 3389,
              "enableFloatingIP": false
            }
          }
        ]
      }
    }
      ]
    }

### <a name="additional-resources"></a>Dodatkowe zasoby

W tym artykule [interfejsu API usługi REST Usługa równoważenia obciążenia](https://msdn.microsoft.com/library/azure/mt163651.aspx) , aby uzyskać więcej informacji.
