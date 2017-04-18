<properties 
   pageTitle="Wystąpienia poziomu publicznych adresów IP (ILPIP) | Microsoft Azure"
   description="Opis ILPIP (PIP) i jak nimi zarządzać"
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
   ms.date="02/10/2016"
   ms.author="jdial" />

# <a name="instance-level-public-ip-overview"></a>Omówienie IP publicznej poziomu wystąpienia
Wystąpienie poziomu publicznych adresów IP (ILPIP) jest publiczny adres IP, które można przypisać bezpośrednio do wystąpienia maszyn wirtualnych lub rolę, a nie do usługi w chmurze, który wystąpienia maszyn wirtualnych lub rolę znajduje się w. To nie miejsce VIP (wirtualnych adresów IP), przypisane do usługi w chmurze. Zamiast jest dodatkowego adresu IP używanego do połączenia bezpośrednio z wystąpienia maszyn wirtualnych lub rolę.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Dowiedz się, jak [wykonać te kroki przy użyciu modelu Menedżera zasobów](virtual-network-ip-addresses-overview-arm.md). 

Upewnij się, że wiesz, jak działają [adresów IP](virtual-network-ip-addresses-overview-classic.md) w Azure.

>[AZURE.NOTE] W przeszłości ILPIP zostało określone jako PIP, który oznacza publiczny adres IP. 

![Różnica między ILPIP i VIP](./media/virtual-networks-instance-level-public-ip/Figure1.png)

Jak pokazano na rysunku 1, usługa w chmurze jest dostępne za pomocą VIP, gdy poszczególnych maszyny wirtualne są zwykle dostępne za pomocą VIP:&lt;numer portu&gt;. Przypisując ILPIP do określonych Głosowa, że można uzyskać dostępu do maszyn wirtualnych bezpośrednio przy użyciu tego adresu IP.

Po utworzeniu usługi w chmurze w Azure odpowiednich rekordów DNS A są tworzone automatycznie umożliwienie dostępu do usług za pośrednictwem w pełni kwalifikowaną nazwę domeny (FQDN) zamiast rzeczywistych VIP. Ten sam proces ma miejsce dla ILPIP zezwolenia na dostęp do wystąpienia maszyn wirtualnych lub roli za nazwy FQDN zamiast ILPIP. Na przykład jeśli utworzysz o nazwie *contosoadservice*usługi w chmurze i konfigurowania roli sieci web o nazwie *contosoweb* z dwoma wystąpieniami, Azure będzie rejestrować następujące rekordy A wystąpień:

- contosoweb\_IN_0.contosoadservice.cloudapp.net
- contosoweb\_IN_1.contosoadservice.cloudapp.net 

>[AZURE.NOTE] Można przypisać tylko jeden ILPIP dla każdego wystąpienia maszyn wirtualnych lub rolę. Możesz użyć maksymalnie 5 ILPIP osoby na subskrypcję. W tej chwili ILPIP nie jest obsługiwane dla wielu NIC maszyny wirtualne.

## <a name="why-should-i-request-an-ilpip"></a>Dlaczego zażądać ILPIP?
Jeśli chcesz można było połączyć się z wystąpieniem rolę lub maszyn wirtualnych przy użyciu adresu IP bezpośrednio do niego przypisana, a nie przy użyciu chmury usługi VIP:&lt;numer portu&gt;, żądanie ILPIP dla swojego maszyn wirtualnych lub wystąpienia roli.
- **Pasywne FTP** - przez ILPIP na swojej Głosowa, możesz odbierać ruchu z niemal każdego portu nie musisz otworzyć punktu końcowego, aby odbierać danych. Dzięki temu scenariuszy, takich jak FTP pasywne miejsce, w którym porty zostały wybrane dynamiczne.
- **Wychodzącego** - ruchu wychodzącego pochodzących z maszyn wirtualnych przechodzi z ILPIP jako źródła i jednoznacznie identyfikuje maszyn wirtualnych do zewnętrznych obiektów.

## <a name="how-to-request-an-ilpip-during-vm-creation"></a>Jak zażądać ILPIP podczas tworzenia maszyn wirtualnych
Skrypt programu PowerShell poniżej umożliwia utworzenie nowej usługi w chmurze o nazwie *FTPService*, a następnie pobiera obraz z Azure i tworzy maszyny o nazwie *FTPInstance* przy użyciu pobrane obrazu, ustawia maszyn wirtualnych, aby użyć ILPIP i dodaje maszyn wirtualnych do nowej usługi:

    New-AzureService -ServiceName FTPService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name FTPInstance -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| Set-AzurePublicIP -PublicIPName ftpip | New-AzureVM -ServiceName FTPService -Location "Central US"

## <a name="how-to-retrieve-ilpip-information-for-a-vm"></a>Jak pobrać informacji ILPIP dla maszyn wirtualnych
Aby wyświetlić informacje o ILPIP dla maszyn wirtualnych, utworzone za pomocą skryptu powyżej, uruchom następujące polecenie programu PowerShell i przestrzegania wartości *PublicIPAddress* i *PublicIPName*:

    Get-AzureVM -Name FTPInstance -ServiceName FTPService

    DeploymentName              : FTPService
    Name                        : FTPInstance
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : ReadyRole
    IpAddress                   : 100.74.118.91
    InstanceStateDetails        : 
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : FTPInstance
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : FTPInstance
    AvailabilitySetName         : 
    DNSName                     : http://ftpservice888.cloudapp.net/
    Status                      : ReadyRole
    GuestAgentStatus            : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 104.43.142.188
    PublicIPName                : ftpip
    NetworkInterfaces           : {}
    ServiceName                 : FTPService
    OperationDescription        : Get-AzureVM
    OperationId                 : 568d88d2be7c98f4bbb875e4d823718e
    OperationStatus             : OK

## <a name="how-to-remove-an-ilpip-from-a-vm"></a>Jak usunąć ILPIP z maszyny
Aby usunąć ILPIP dodane do maszyn wirtualnych w skrypt powyżej, uruchom następujące polecenia programu PowerShell:
    
    Get-AzureVM -ServiceName FTPService -Name FTPInstance `
  	| Remove-AzurePublicIP `
  	| Update-AzureVM

## <a name="how-to-add-an-ilpip-to-an-existing-vm"></a>Jak dodać ILPIP do istniejącego maszyn wirtualnych
Aby dodać ILPIP maszyn wirtualnych, utworzony przy użyciu skrypt powyżej, uruchom następujące polecenie:

    Get-AzureVM -ServiceName FTPService -Name FTPInstance `
  	| Set-AzurePublicIP -PublicIPName ftpip2 `
  	| Update-AzureVM

## <a name="how-to-associate-an-ilpip-to-a-vm-by-using-a-service-configuration-file"></a>Jak skojarzyć ILPIP do maszyny za pomocą pliku konfiguracji usługi
Można także skojarzyć ILPIP do maszyny za pomocą usługi pliku konfiguracji (CSCFG). Xml przykładowe poniżej przedstawiono sposób konfigurowania usługi w chmurze umożliwia ILPIP o nazwie *MyPublicIP* w przypadku wystąpienia roli: 
    
    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <VirtualNetworkSite name="VNet"/>
        <AddressAssignments>
          <InstanceAddress roleName="VMRolePersisted">
            <Subnets>
              <Subnet name="Subnet1"/>
              <Subnet name="Subnet2"/>
            </Subnets>
            <PublicIPs>
              <PublicIP name="MyPublicIP" domainNameLabel="MyPublicIP" />
            </PublicIPs>
          </InstanceAddress>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## <a name="next-steps"></a>Następne kroki

- Zrozumieć, jak [adresy IP](virtual-network-ip-addresses-overview-classic.md) działa w modelu Klasyczny wdrożenia.

- Informacje na temat [zastrzeżone adresy IP](virtual-networks-reserved-public-ip.md).
 