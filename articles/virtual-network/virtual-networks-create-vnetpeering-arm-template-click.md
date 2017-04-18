<properties
   pageTitle="Tworzenie VNet zaglądanie korzystania z szablonów Menedżera zasobów | Microsoft Azure"
   description="Dowiedz się, jak tworzyć, wirtualną sieć zaglądanie, korzystając z szablonów w Menedżerze zasobów."
   services="virtual-network"
   documentationCenter=""
   authors="narayanannamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="narayanannamalai;annahar"/>

# <a name="create-vnet-peering-using-resource-manager-templates"></a>Tworzenie VNet zaglądanie korzystania z szablonów Menedżera zasobów

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Aby utworzyć VNet zaglądanie przy użyciu szablonów Menedżera zasobów, wykonaj poniższe czynności:

1. Jeśli po raz pierwszy używasz Azure programu PowerShell, zobacz, [jak instalowanie i konfigurowanie programu PowerShell Azure](../powershell-install-configure.md) i postępuj zgodnie z instrukcjami maksymalnie end, aby zalogować się do Azure i wybierz subskrypcję.

    > [AZURE.NOTE] Polecenia cmdlet programu PowerShell do zarządzania zaglądanie VNet jest dołączona do programu [Azure programu PowerShell 1.6.](http://www.powershellgallery.com/packages/Azure/1.6.0)

2. Tekst poniżej przedstawiono definicję łącza peering VNet VNet1 do VNet2 według tego scenariusza powyżej. Kopiowanie zawartości poniżej, a następnie zapisz go w pliku o nazwie VNetPeeringVNet1.json.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToVNet2",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet2')]"       
        }
            }
            }
        ]
        }

3. Sekcję poniżej przedstawiono definicję łącza peering VNet VNet2 do VNet1 według tego scenariusza powyżej.  Kopiowanie zawartości poniżej, a następnie zapisz go w pliku o nazwie VNetPeeringVNet2.json.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet2/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
        ]
        }

    Jak było widać w szablonie powyżej, istnieje kilka można konfigurować właściwości zaglądanie VNet:

  	|Opcja|Opis|Domyślne|
  	|:-----|:----------|:------|
  	|AllowVirtualNetworkAccess|Czy przestrzeni adresów partnera VNet wchodzi w skład znacznika virtual_network.|Tak|
  	|AllowForwardedTraffic|Czy ruch nie pochodzących z peered VNet jest zaakceptowane lub odrzucone.|Brak|
  	|AllowGatewayTransit|Umożliwia równorzędnych VNet używać Centrum VNet.|Brak|
  	|UseRemoteGateways|Za pomocą usługi równorzędnych VNet bramy. Funkcja VNet musi zawierać bramy skonfigurowane i AllowGatewayTransit zaznaczone. Nie można używać tej opcji, jeśli masz skonfigurowane bramy.|Brak|

    Każde z łączy w zaglądanie VNet zawiera zestaw właściwości powyżej. Można na przykład AllowVirtualNetworkAccess Aby ustaw wartość True dla łącza zaglądanie VNet VNet1 VNet2 i nadaj mu wartość False łącza peering VNet w kierunku przeciwnym.


4. Aby wdrożyć plik szablonu, możesz uruchomić polecenia cmdlet AzureRmResourceGroupDeployment nowy, aby utworzyć lub zaktualizować wdrożenia. Więcej informacji na temat korzystania z szablonów Menedżera zasobów można znaleźć w tym [artykule](../resource-group-template-deploy.md).

        New-AzureRmResourceGroupDeployment -ResourceGroupName <resource group name> -TemplateFile <template file path> -DeploymentDebugLogLevel all

    > [AZURE.NOTE] Zamień grupy szablon i nazwę pliku zasobu zależnie od potrzeb.

    Poniżej przedstawiono przykład jest oparty na scenariusz powyżej:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet1.json -DeploymentDebugLogLevel all

    Przedstawia wynik:

        DeploymentName      : VNetPeeringVNet1
        ResourceGroupName   : VNet101
        ProvisioningState       : Succeeded
        Timestamp           : 7/26/2016 9:05:03 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters          :
        Outputs         :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet2.json -DeploymentDebugLogLevel all

    Przedstawia wynik:

        DeploymentName      : VNetPeeringVNet2
        ResourceGroupName   : VNet101
        ProvisioningState       : Succeeded
        Timestamp           : 7/26/2016 9:07:22 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters          :
        Outputs         :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

5. Po zakończeniu rozmieszczania można uruchomić polecenia cmdlet poniżej, aby wyświetlić stan peering:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNet1 -ResourceGroupName VNet101 -Name linktoVNet2

    Przedstawia wynik:

        Name            : LinkToVNet2
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : VNet101
        VirtualNetworkName  : VNet1
        ProvisioningState       : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

    Po zaglądanie w tym scenariuszu należy inicjować połączenia z maszyn wirtualnych z maszyn wirtualnych w obu VNets. Domyślnie AllowVirtualNetworkAccess ma wartość PRAWDA, a zaglądanie VNet będzie obsługi administracyjnej pisane z wielkiej litery ACL umożliwienie komunikacji między VNets. Można stosować zasad grupy (NSG) zabezpieczeń sieci do blokowania łączność między określonej podsieci lub maszyn wirtualnych przejęcie kontroli ziarno małe dostępu między dwoma wirtualnych sieci.  Więcej informacji: tworzenie reguł NSG można znaleźć w tym [artykule](virtual-networks-create-nsg-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

Aby utworzyć VNet zaglądanie w subskrypcjach, wykonaj poniższe czynności:

1. Zaloguj się do Azure o uprawnieniach użytkownika A użytkownika konta w A subskrypcji i uruchom następujące polecenie cmdlet:

        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet5

    Nie jest to wymagane zaglądanie można ustalić nawet w przypadku użytkowników pojedynczo podnieść zaglądanie żądania dla ich odpowiednich Vnets, ile żądania zgodne. Dodawanie użytkownikiem innego VNet jako użytkowników w lokalnej VNet ułatwia wykonaj konfiguracji.

2. Logowanie się do Azure za pomocą konta uprzywilejowanych użytkownika-B dla subskrypcji-B i uruchom następujące polecenie cmdlet:

        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet3

3. W odpowiedzi użytkownika jest sesji logowania, uruchom to polecenie cmdlet:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet3.json -DeploymentDebugLogLevel all

    Poniżej przedstawiono, jak zdefiniowano plik JSON.  

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet3/LinkToVNet5",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/<Subscription-B-ID>/resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet5"
                }
            }
            }
        ]
        }

4. W sesji logowania użytkownika B uruchom następujące polecenie cmdlet:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet5.json -DeploymentDebugLogLevel all

    Poniżej opisano, jak zdefiniowano plik JSON:

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet5/LinkToVNet3",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/Subscription-A-ID /resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet3"
                }
            }
            }
        ]
        }

    Po zaglądanie w tym scenariuszu należy zainicjować połączeń z maszyn wirtualnych maszyn wirtualnych obu VNets w różnych subskrypcjach.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. W tym scenariuszu należy wdrożyć Poniższy przykładowy szablon ustanawiania zaglądanie VNet.  Należy ustawić właściwość AllowForwardedTraffic na wartość True, dzięki czemu urządzenia wirtualna sieć w peered VNet, aby wysyłać i odbierać dane.

    Oto szablonu do tworzenia VNet zaglądanie z HubVNet do VNet1. Zauważ, że AllowForwardedTraffic jest ustawiona na wartość FAŁSZ.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "HubVNet/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
            }
        ]
        }

2. Oto szablonu do tworzenia VNet zaglądanie z VNet1 do HubVnet. Należy zauważyć, że AllowForwardedTraffic jest ustawiona na PRAWDA.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToHubVNet",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": true,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'HubVnet')]"       
                }
            }
            }
        ]
        }


3. Po ustaleniu zaglądanie można używać odwołań do tego [artykułu](virtual-network-create-udr-arm-ps.md) , aby zdefiniować routes(UDR) przekierowywać ruch VNet1 za pośrednictwem wirtualnych urządzenia, aby używać funkcji zdefiniowanych przez użytkownika. Po określeniu adres następnego przeskoku kierowanie można ustawić adres IP wirtualnego urządzenia w konwersacjach VNet HubVNet.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

Aby utworzyć zaglądanie między wirtualnych sieci z modeli rozmieszczania, wykonaj następujące czynności:
1. Tekst poniżej przedstawiono definicję łącza peering VNet VNET1 do VNET2 w tym scenariuszu. Tylko jedno łącze jest wymagane równorzędne klasyczny wirtualną sieć wirtualną siecią Menedżera zasobów Azure.

    Pamiętaj umieścić w identyfikator subskrypcji, dla której znajduje się klasyczny wirtualnej sieci lub VNET2 i zmień MyResouceGroup na odpowiedni zasób nazwę grupy.

    {"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#", "contentVersion": "1.0.0.0", "Parametry": {}, "zmienne": {}, "zasobów": [{"apiVersion": "2016-06-01", "typ": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings", "Nazwa": "VNET1/LinkToVNET2", "położenie": "[Grupa zasobów () .location]", "właściwości": {"allowVirtualNetworkAccess": wartość true, "allowForwardedTraffic": ma wartość FAŁSZ, "allowGatewayTransit": ma wartość FAŁSZ, "useRemoteGateways": ma wartość FAŁSZ, "remoteVirtualNetwork": {"identyfikator": "[resourceId (" Microsoft.ClassicNetwork/virtualNetworks, "VNET2")] "}}}]}

2. Aby wdrożyć plik szablonu, uruchom następujące polecenie cmdlet, aby utworzyć lub zaktualizować wdrażanie.

        New-AzureRmResourceGroupDeployment -ResourceGroupName MyResourceGroup -TemplateFile .\VnetPeering.json -DeploymentDebugLogLevel all

        Output shows:

        DeploymentName          : VnetPeering
        ResourceGroupName       : MyResourceGroup
        ProvisioningState       : Succeeded
        Timestamp               : XX/XX/YYYY 5:42:33 PM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
        Outputs                 :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

3. Po pomyślnym wdrażanie można uruchomić następujące polecenie cmdlet, aby wyświetlić stan peering:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNET1 -ResourceGroupName MyResourceGroup -Name LinkToVNET2

        Output shows:

        Name                             : LinkToVNET2
        Id                               : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResource
                                   Group/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeering
                                   s/LinkToVNET2
        Etag                             : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                     "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/M
                                   yResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                   }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

Po zaglądanie między klasyczny VNet i Menedżer zasobów VNet należy inicjować połączenia z maszyn wirtualnych w VNET1 z maszyn wirtualnych w VNET2 i na odwrót.
