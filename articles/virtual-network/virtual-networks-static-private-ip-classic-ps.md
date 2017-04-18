<properties 
   pageTitle="Jak skonfigurować statyczny adres IP prywatne w trybie klasycznym przy użyciu programu PowerShell | Microsoft Azure"
   description="Opis prywatnych statyczne adresy IP (spadku) i jak nimi zarządzać w trybie klasycznym i programu PowerShell"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-classic-in-powershell"></a>Jak ustawić prywatne statycznego adresu IP (klasyczny) w programie PowerShell

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]W tym artykule opisano, jak model klasyczny wdrożenia. Można również [zarządzać statycznego adresu IP prywatne w modelu wdrożenia Menedżera zasobów](virtual-networks-static-private-ip-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Polecenia programu PowerShell przykładowe poniżej przewiduje środowisku prosty już utworzone. Jeśli chcesz uruchomić polecenia wyświetlaną w tym dokumencie, należy najpierw utworzyć środowisku testowym opisane w sekcji [Tworzenie VNet](virtual-networks-create-vnet-classic-netcfg-ps.md).

## <a name="how-to-verify-if-a-specific-ip-address-is-available"></a>Jak sprawdzić, czy określony adres IP jest dostępna
Aby sprawdzić, jeśli adres IP *192.168.1.101* jest dostępna w VNet o nazwie *TestVNet*, uruchom następujące polecenie programu PowerShell i sprawdź wartość *IsAvailable*:

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 192.168.1.101 

Oczekiwany wynik:

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Jak określić statyczny adres IP prywatne podczas tworzenia maszyn wirtualnych
Skrypt programu PowerShell poniżej umożliwia utworzenie nowej usługi w chmurze o nazwie *TestService*, a następnie pobiera obraz z Azure, tworzy maszyny o nazwie *DNS01* w nowej usługi w chmurze przy użyciu obrazu pobrane ustawia maszyn wirtualnych w podsieci o nazwie *serwera sieci Web*i zestawy *192.168.1.7* jako prywatne statycznego adresu IP dla maszyn wirtualnych:

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage | where {$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name DNS01 -InstanceSize Small -ImageName $image.ImageName |
      Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! |
      Set-AzureSubnet –SubnetNames FrontEnd |
      Set-AzureStaticVNetIP -IPAddress 192.168.1.7 |
      New-AzureVM -ServiceName TestService –VNetName TestVNet

Oczekiwany wynik:

    WARNING: No deployment found in service: 'TestService'.
    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    New-AzureService     fcf705f1-d902-011c-95c7-b690735e7412 Succeeded      
    New-AzureVM          3b99a86d-84f8-04e5-888e-b6fc3c73c4b9 Succeeded  

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Jak pobrać statyczne prywatne informacje o adresie IP dla maszyn wirtualnych
Aby wyświetlić statyczną prywatne informacje o adresie IP dla maszyn wirtualnych, utworzone za pomocą skryptu powyżej, uruchom następujące polecenie programu PowerShell i obserwować wartości *adres IP*:

    Get-AzureVM -Name DNS01 -ServiceName TestService

Oczekiwany wynik:

    DeploymentName              : TestService
    Name                        : DNS01
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : Provisioning
    IpAddress                   : 192.168.1.7
    InstanceStateDetails        : Windows is preparing your computer for first use...
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : DNS01
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : rsR2-797
    AvailabilitySetName         : 
    DNSName                     : http://testservice000.cloudapp.net/
    Status                      : Provisioning
    GuestAgentStatus            : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 
    PublicIPName                : 
    NetworkInterfaces           : {}
    ServiceName                 : TestService
    OperationDescription        : Get-AzureVM
    OperationId                 : 34c1560a62f0901ab75cde4fed8e8bd1
    OperationStatus             : OK

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Jak usunąć statycznego adresu IP prywatne z maszyny
Aby usunąć statyczny adres IP prywatne dodane do maszyn wirtualnych w skrypt powyżej, uruchom następujące polecenie programu PowerShell:
    
    Get-AzureVM -ServiceName TestService -Name DNS01 |
      Remove-AzureStaticVNetIP |
      Update-AzureVM

Oczekiwany wynik:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Update-AzureVM       052fa6f6-1483-0ede-a7bf-14f91f805483 Succeeded

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Jak dodać statycznego adresu IP prywatne do istniejącego maszyn wirtualnych
Aby dodać statycznego prywatny adres IP maszyn wirtualnych, utworzone za pomocą skryptu powyżej runt on następującego polecenia:

    Get-AzureVM -ServiceName TestService -Name DNS01 |
      Set-AzureStaticVNetIP -IPAddress 192.168.1.7 |
      Update-AzureVM

Oczekiwany wynik:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Update-AzureVM       77d8cae2-87e6-0ead-9738-7c7dae9810cb Succeeded 

## <a name="next-steps"></a>Następne kroki

- Informacje na temat zastrzeżone publiczne adresy [IP](virtual-networks-reserved-public-ip.md) .
- Informacje na temat adresów [wystąpienie poziomu publicznych adresów IP (ILPIP)](virtual-networks-instance-level-public-ip.md) .
- Zwróć się [interfejsy API pozostałych zastrzeżony adres IP](https://msdn.microsoft.com/library/azure/dn722420.aspx).
