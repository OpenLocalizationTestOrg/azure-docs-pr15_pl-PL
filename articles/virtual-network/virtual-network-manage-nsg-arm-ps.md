<properties 
   pageTitle="Zarządzanie NSGs przy użyciu programu PowerShell w Menedżerze zasobów | Microsoft Azure"
   description="Dowiedz się, jak zarządzać istniejącego NSGs przy użyciu programu PowerShell w Menedżerze zasobów"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/14/2016"
   ms.author="jdial" />

# <a name="manage-nsgs-using-powershell"></a>Zarządzanie NSGs przy użyciu programu PowerShell

[AZURE.INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]model klasyczny wdrożenia.

[AZURE.INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="retrieve-information"></a>Pobieranie informacji

Można wyświetlić z istniejących NSGs, pobieranie reguł dla istniejących NSG i Dowiedz się, jakie zasoby NSG jest skojarzony.

### <a name="view-existing-nsgs"></a>Wyświetlanie istniejących NSGs
Aby wyświetlić wszystkie NSGs istniejącej subskrypcji, uruchom `Get-AzureRmNetworkSecurityGroup` polecenia cmdlet, tak jak pokazano poniżej.

    Get-AzureRmNetworkSecurityGroup

Oczekiwany wynik:

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 :                         
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
    
    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
                            
    Name                 : WEB1
    ResourceGroupName    : RG101
    Location             : eastus2
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG101/providers/M
                           icrosoft.Network/networkSecurityGroups/WEB1
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]


Aby wyświetlić listę NSGs w danej grupy zasobów, uruchom `Get-AzureRmNetworkSecurityGroup` polecenia cmdlet, tak jak pokazano poniżej. 

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG

Oczekiwany wynik:

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 :                         
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
    
    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
         
### <a name="list-all-rules-for-an-nsg"></a>Lista wszystkich reguł dla NSG

Aby wyświetlić zasady NSG o nazwie **NSG FrontEnd**, uruchom `Get-AzureRmNetworkSecurityGroup` polecenia cmdlet, tak jak pokazano poniżej. 

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd | Select SecurityRules -ExpandProperty SecurityRules

Oczekiwany wynik:
    
    Name                     : rdp-rule
    Id                       : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/                        Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp-rule
    Etag                     : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ProvisioningState        : Succeeded
    Description              : Allow RDP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 100
    Direction                : Inbound
    
    Name                     : web-rule
    Id                       : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/                        Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule
    Etag                     : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ProvisioningState        : Succeeded
    Description              : Allow HTTP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 80
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 101
    Direction                : Inbound

>[AZURE.NOTE] Można również użyć `Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name "NSG-FrontEnd" | Select DefaultSecurityRules -ExpandProperty DefaultSecurityRules` do wyświetlania listy domyślne reguły z **Serwera sieci Web NSG** NSG.

### <a name="view-nsgs-associations"></a>Wyświetlanie powiązań NSGs

Aby wyświetlić zasoby NSG **NSG FrontEnd** jest skojarzony z, uruchom `Get-AzureRmNetworkSecurityGroup` polecenia cmdlet, tak jak pokazano poniżej.

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd

Należy poszukać właściwości **NetworkInterfaces** i **podsieci** , tak jak pokazano poniżej:

    NetworkInterfaces    : []
    Subnets              : [
                             {
                               "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                               "IpConfigurations": []
                             }
                           ]

W powyższym przykładzie NSG nie jest przypisana do żadnych interfejsów sieci (NIC), a jest przypisana do podsieci o nazwie **serwera sieci Web**.

## <a name="manage-rules"></a>Zarządzanie regułami

Można dodać reguły do istniejącego NSG, edytowanie istniejących reguł i usunąć reguły.

### <a name="add-a-rule"></a>Dodawanie reguły

Aby dodać regułę zezwolić ruchu **przychodzącego** na porcie **443** z dowolnego komputera do **Serwera sieci Web NSG** NSG, wykonaj poniższe czynności.

1. Uruchamianie `Get-AzureRmNetworkSecurityGroup` polecenia cmdlet, aby pobrać istniejących NSG i przechowywać w zmiennej, tak jak pokazano poniżej.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Uruchamianie `Add-AzureRmNetworkSecurityRuleConfig` polecenia cmdlet, tak jak pokazano poniżej.

        Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule `
            -Description "Allow HTTPS" `
            -Access Allow `
            -Protocol Tcp `
            -Direction Inbound `
            -Priority 102 `
            -SourceAddressPrefix * `
            -SourcePortRange * `
            -DestinationAddressPrefix * `
            -DestinationPortRange 443

3. Aby zapisać zmiany wprowadzone NSG, uruchom `Set-AzureRmNetworkSecurityGroup` polecenia cmdlet, tak jak pokazano poniżej.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Oczekiwany wynik przedstawiający tylko reguły zabezpieczeń:

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "*",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="change-a-rule"></a>Zmienianie reguły

Aby zmienić regułę utworzony w poprzednim przykładzie umożliwia ruch przychodzący z **Internetu** tylko, wykonaj poniższe czynności.

1. Uruchamianie `Get-AzureRmNetworkSecurityGroup` polecenia cmdlet, aby pobrać istniejący NSG i przechowywać w zmiennej, tak jak pokazano poniżej.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Uruchamianie `Set-AzureRmNetworkSecurityRuleConfig` polecenia cmdlet, tak jak pokazano poniżej.

        Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule `
            -Description "Allow HTTPS" `
            -Access Allow `
            -Protocol Tcp `
            -Direction Inbound `
            -Priority 102 `
            -SourceAddressPrefix * `
            -SourcePortRange Internet `
            -DestinationAddressPrefix * `
            -DestinationPortRange 443

3. Aby zapisać zmiany wprowadzone NSG, uruchom `Set-AzureRmNetworkSecurityGroup` polecenia cmdlet, tak jak pokazano poniżej.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Oczekiwany wynik przedstawiający tylko reguły zabezpieczeń:

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "Internet",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="delete-a-rule"></a>Usuwanie reguły

1. Uruchamianie `Get-AzureRmNetworkSecurityGroup` polecenia cmdlet, aby pobrać istniejący NSG i przechowywać w zmiennej, tak jak pokazano poniżej.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Uruchamianie `Remove-AzureRmNetworkSecurityRuleConfig` polecenia cmdlet, tak jak pokazano poniżej.

        Remove-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule

3. Aby zapisać zmiany wprowadzone NSG, uruchom `Set-AzureRmNetworkSecurityGroup` polecenia cmdlet, tak jak pokazano poniżej.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Oczekiwany wynik przedstawiający powiadomienie, które **Reguła https** nie jest już wyświetlana tylko reguły zabezpieczeń:

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 }
                               ]

## <a name="manage-associations"></a>Zarządzanie skojarzenia

Możesz skojarzyć NSG do podsieci i nic. Można również dissociate NSG z dowolnego zasobu, który jest skojarzona.

### <a name="associate-an-nsg-to-a-nic"></a>Kojarzenie NSG, aby NIC

Aby skojarzyć **NSG FrontEnd** NSG do **TestNICWeb1** NIC, wykonaj poniższe czynności.

1. Uruchamianie `Get-AzureRmNetworkSecurityGroup` polecenia cmdlet, aby pobrać istniejący NSG i przechowywać w zmiennej, tak jak pokazano poniżej.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Uruchamianie `Get-AzureRmNetworkInterface` polecenia cmdlet, aby pobrać istniejących NIC i przechowywać w zmiennej, tak jak pokazano poniżej.

        $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG `
            -Name TestNICWeb1

3. Wartość zmiennej **NSG** , należy ustawić właściwość **NetworkSecurityGroup** zmiennej **NIC** , tak jak pokazano poniżej.

        $nic.NetworkSecurityGroup = $nsg

4. Aby zapisać zmiany wprowadzone do karty Sieciowej, uruchom `Set-AzureRmNetworkInterface` polecenia cmdlet, tak jak pokazano poniżej.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Przewidywaną tylko właściwości **NetworkSecurityGroup** :

        NetworkSecurityGroup : {
                                 "SecurityRules": [],
                                 "DefaultSecurityRules": [],
                                 "NetworkInterfaces": [],
                                 "Subnets": [],
                                 "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                               }

### <a name="dissociate-an-nsg-from-a-nic"></a>DISSOCIATE NSG z NIC

Aby usunąć **NSG FrontEnd** NSG z **TestNICWeb1** NIC, wykonaj poniższe czynności.

1. Uruchamianie `Get-AzureRmNetworkSecurityGroup` polecenia cmdlet, aby pobrać istniejący NSG i przechowywać w zmiennej, tak jak pokazano poniżej.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Uruchamianie `Get-AzureRmNetworkInterface` polecenia cmdlet, aby pobrać istniejący NIC i przechowywać w zmiennej, tak jak pokazano poniżej.

        $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG `
            -Name TestNICWeb1

3. Ustaw właściwość **NetworkSecurityGroup** zmiennej **NIC** **$null**, tak jak pokazano poniżej.

        $nic.NetworkSecurityGroup = $null

4. Aby zapisać zmiany wprowadzone do karty Sieciowej, uruchom `Set-AzureRmNetworkInterface` polecenia cmdlet, tak jak pokazano poniżej.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Oczekiwany wynik tylko właściwości **NetworkSecurityGroup** :

        NetworkSecurityGroup : null

### <a name="dissociate-an-nsg-from-a-subnet"></a>DISSOCIATE NSG z podsieci

Aby usunąć **NSG FrontEnd** NSG z podsieci **serwera sieci Web** , wykonaj poniższe czynności.

1. Uruchamianie `Get-AzureRmVirtualNetwork` polecenia cmdlet, aby pobrać istniejący VNet i przechowywać w zmiennej, tak jak pokazano poniżej.

        $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG `
            -Name TestVNet

2. Uruchamianie `Get-AzureRmVirtualNetworkSubnetConfig` polecenia cmdlet, aby pobrać podsieci **serwera sieci Web** i przechowywać w zmiennej, tak jak pokazano poniżej.

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
            -Name FrontEnd 

3. Ustaw właściwość **NetworkSecurityGroup** zmiennej **podsieci** **$null**, tak jak pokazano poniżej.

        $subnet.NetworkSecurityGroup = $null

4. Aby zapisać zmiany wprowadzone do podsieci, uruchom `Set-AzureRmVirtualNetwork` polecenia cmdlet, tak jak pokazano poniżej.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

    Oczekiwany wynik pokazujący tylko właściwości podsieci **serwera sieci Web** . Należy zauważyć, że nie istnieje właściwość **NetworkSecurityGroup**:

            ...
            Subnets           : [
                                  {
                                    "Name": "FrontEnd",
                                    "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                    "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24",
                                    "IpConfigurations": [
                                      {
                                        "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1"
                                      },
                                      {
                                        "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1"
                                      }
                                    ],
                                    "ProvisioningState": "Succeeded"
                                  },
                                    ...
                                ]

### <a name="associate-an-nsg-to-a-subnet"></a>Kojarzenie NSG do podsieci

Aby ponownie skojarzyć **NSG FrontEnd** NSG do podsieci **FronEnd** , wykonaj poniższe czynności.

1. Uruchamianie `Get-AzureRmVirtualNetwork` polecenia cmdlet, aby pobrać istniejący VNet i przechowywać w zmiennej, tak jak pokazano poniżej.

        $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG `
            -Name TestVNet

2. Uruchamianie `Get-AzureRmVirtualNetworkSubnetConfig` polecenia cmdlet, aby pobrać podsieci **serwera sieci Web** i przechowywać w zmiennej, tak jak pokazano poniżej.

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
            -Name FrontEnd 

1. Uruchamianie `Get-AzureRmNetworkSecurityGroup` polecenia cmdlet, aby pobrać istniejący NSG i przechowywać w zmiennej, tak jak pokazano poniżej.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

3. Ustaw właściwość **NetworkSecurityGroup** zmiennej **podsieci** **$null**, tak jak pokazano poniżej.

        $subnet.NetworkSecurityGroup = $nsg

4. Aby zapisać zmiany wprowadzone do podsieci, uruchom `Set-AzureRmVirtualNetwork` polecenia cmdlet, tak jak pokazano poniżej.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

    Oczekiwany wynik tylko właściwości **NetworkSecurityGroup** podsieci **serwera sieci Web** :

        ...
        "NetworkSecurityGroup": {
                                  "SecurityRules": [],
                                  "DefaultSecurityRules": [],
                                  "NetworkInterfaces": [],
                                  "Subnets": [],
                                  "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                                }
        ...

## <a name="delete-an-nsg"></a>Usuwanie NSG

NSG można usuwać tylko, jeśli nie jest skojarzone z dowolnym zasobem. Aby usunąć NSG, wykonaj poniższe czynności.

1. Aby sprawdzić zasoby skojarzone NSG, uruchom `azure network nsg show` jak pokazano w [widoku NSGs skojarzenia](#View-NSGs-associations).
2. Jeśli nie jest skojarzone z dowolnym nic NSG, uruchom `azure network nic set` jak pokazano w [Dissociate NSG z NIC](#Dissociate-an-NSG-from-a-NIC) dla każdej karty sieciowej. 
3. Jeśli NSG jest przypisana do dowolnej podsieci, uruchom `azure network vnet subnet set` przedstawione [Dissociate NSG z podsieci](#Dissociate-an-NSG-from-a-subnet) dla każdej podsieci.
4. Aby usunąć NSG, uruchom `Remove-AzureRmNetworkSecurityGroup` polecenia cmdlet, tak jak pokazano poniżej.

        Remove-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd -Force

    >[AZURE.NOTE] **-Wymuś** parametru zapewnia nie potrzebujesz potwierdzić usunięcie.

## <a name="next-steps"></a>Następne kroki

- [Włącz rejestrowanie](virtual-network-nsg-manage-log.md) dla NSGs.