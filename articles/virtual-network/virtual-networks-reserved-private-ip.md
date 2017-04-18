<properties 
   pageTitle="Jak ustawić statyczny adres IP prywatne wewnętrznych"
   description="Opis wewnętrznych statyczne adresy IP (spadku) i jak nimi zarządzać"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/22/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-internal-private-ip-address-using-powershell-classic"></a>Jak ustawić statyczne wewnętrznych prywatny adres IP przy użyciu programu PowerShell (klasyczny)
W większości przypadków nie trzeba określać statyczny adres IP wewnętrznego komputera wirtualnych. Maszyny wirtualne w wirtualnej sieci automatycznie otrzymają wewnętrzny adres IP z zakresu, który można określić. Ale w niektórych przypadkach, określenie statycznego adresu IP dla określonego maszyn wirtualnych ma sens. Jeśli na przykład do maszyn wirtualnych jest, przechodząc do uruchomienia DNS lub będzie kontrolera domeny. Statyczny adres IP wewnętrznego pozostanie przy maszyn wirtualnych nawet za pośrednictwem stan Zatrzymaj/deprovision. 

> [AZURE.IMPORTANT] Azure występują dwa modele rozmieszczania służące do tworzenia i pracy z zasobami: [Menedżer zasobów i klasyczny](../resource-manager-deployment-model.md). W tym artykule opisano, jak przy użyciu modelu Klasyczny wdrożenia. Firma Microsoft zaleca się, że większość nowych wdrożeń za pomocą [Menedżera zasobów wdrożenia modelu](virtual-networks-static-private-ip-arm-ps.md).

## <a name="how-to-verify-if-a-specific-ip-address-is-available"></a>Jak sprawdzić, czy określony adres IP jest dostępna
Aby sprawdzić, jeśli adres IP *10.0.0.7* jest dostępna w vnet o nazwie *TestVnet*, uruchom następujące polecenie programu PowerShell i sprawdź wartość *IsAvailable*:

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 10.0.0.7 

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

>[AZURE.NOTE] Jeśli chcesz sprawdzić polecenie powyżej w bezpiecznego środowiska postępuj zgodnie z wytycznymi zawartymi w [Tworzenie wirtualnej sieci](virtual-networks-create-vnet-classic-portal.md) , aby utworzyć vnet o nazwie *TestVnet* i upewnij się, że używa przestrzeni adresów *10.0.0.0/8* .

## <a name="how-to-specify-a-static-internal-ip-when-creating-a-vm"></a>Jak określić statyczny adres IP wewnętrznego podczas tworzenia maszyn wirtualnych
Skrypt programu PowerShell poniżej umożliwia utworzenie nowej usługi w chmurze o nazwie *TestService*, następnie pobiera obraz z platformy Azure, a następnie tworzy maszyny o nazwie *TestVM* w nowej usługi w chmurze przy użyciu obrazu pobrane ustawia maszyn wirtualnych w podsieci o nazwie *1 podsieci*i zestawy *10.0.0.7* jako statyczny adres IP wewnętrznego dla maszyn wirtualnych:

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| Set-AzureSubnet –SubnetNames Subnet-1 `
  	| Set-AzureStaticVNetIP -IPAddress 10.0.0.7 `
  	| New-AzureVM -ServiceName "TestService" –VNetName TestVnet

## <a name="how-to-retrieve-static-internal-ip-information-for-a-vm"></a>Jak pobrać statyczne informacje wewnętrzne adresów IP dla maszyn wirtualnych
Aby wyświetlić statyczną informacje wewnętrzne adresów IP dla maszyn wirtualnych, utworzone za pomocą skryptu powyżej, uruchom następujące polecenie programu PowerShell i obserwować wartości *adres IP*:

    Get-AzureVM -Name TestVM -ServiceName TestService

    DeploymentName              : TestService
    Name                        : TestVM
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : Provisioning
    IpAddress                   : 10.0.0.7
    InstanceStateDetails        : Windows is preparing your computer for first use...
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : TestVM
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

## <a name="how-to-remove-a-static-internal-ip-from-a-vm"></a>Jak usunąć statyczny adres IP wewnętrznego z maszyny
Aby usunąć statyczny adres IP wewnętrznych, dodane do maszyn wirtualnych w skrypt powyżej, uruchom następujące polecenia programu PowerShell:
    
    Get-AzureVM -ServiceName TestService -Name TestVM `
  	| Remove-AzureStaticVNetIP `
  	| Update-AzureVM

## <a name="how-to-add-a-static-internal-ip-to-an-existing-vm"></a>Jak dodać statyczny adres IP wewnętrznego do istniejącego maszyn wirtualnych
Aby dodać statycznego wewnętrznych IP do maszyn wirtualnych utworzony przy użyciu skryptu powyżej runt on następującego polecenia:

    Get-AzureVM -ServiceName TestService000 -Name TestVM `
  	| Set-AzureStaticVNetIP -IPAddress 10.10.0.7 `
  	| Update-AzureVM

## <a name="next-steps"></a>Następne kroki

[Zastrzeżony adres IP](virtual-networks-reserved-public-ip.md)

[Publiczny adres IP poziom wystąpienia (ILPIP)](virtual-networks-instance-level-public-ip.md)

[Interfejsy API pozostałych zastrzeżony adres IP](https://msdn.microsoft.com/library/azure/dn722420.aspx)
 
