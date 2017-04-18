<properties
   pageTitle="Zastrzeżone IP | Microsoft Azure"
   description="Opis zastrzeżone adresy IP i jak nimi zarządzać"
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

# <a name="reserved-ip-overview"></a>Omówienie zastrzeżony adres IP
Adresy IP platformy Azure podzielić na dwie kategorie: dynamiczne i zastrzeżone. Zarządzane przez Azure publiczne adresy IP są dynamiczne domyślnie. Oznacza to, że adres IP używane do usługi w chmurze danej (VIP) lub uzyskać dostęp do maszyn wirtualnych lub rolę wystąpieniem bezpośrednio (ILPIP) można zmienić od czasu do czasu, gdy zasoby są zamknięcia oraz alokację.

Aby zapobiec zmienianiu adresy IP, można zarezerwować adres IP. Adresy IP zastrzeżone służy tylko jako VIP, zapewniające, że adres IP dla usługi w chmurze będą takie same nawet jako zasoby są zamknięcia lub alokację. Ponadto można przekonwertować istniejący dynamiczne adresy IP używane jako VIP zastrzeżone adres IP.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Dowiedz się, jak zarezerwować statyczne publiczny adres IP przy użyciu [modelu wdrożenia Menedżera zasobów](virtual-network-ip-addresses-overview-arm.md).

Upewnij się, że wiesz, jak działają [adresów IP](virtual-network-ip-addresses-overview-classic.md) w Azure.

## <a name="when-do-i-need-a-reserved-ip"></a>Jeśli potrzebujesz zastrzeżony adres IP?
- **Chcesz mieć pewność, że IP jest zarezerwowana w ramach subskrypcji**. Jeśli chcesz zarezerwować adres IP, która nie zostanie wydana z subskrypcji w żadnym wypadku, należy używać zastrzeżone publiczny adres IP.  
- **Ma swój adres IP, aby otrzymywać z Twojej usługi w chmurze nawet w stanie zatrzymana lub alokację (pośrednictwem SMS)**. Jeśli chcesz korzystać z tej usługi można uzyskać dostęp przy użyciu adresu IP, który nie ulegnie zmianie nawet wtedy, gdy maszyny wirtualne w usłudze w chmurze są tabulatora lub alokację.
- **Chcesz mieć pewność, że ruchu wychodzącego z platformy Azure używa przewidywalne adresu IP**. Może być skonfigurowany do zezwalania tylko na ruch z określonych adresów IP Zapora w lokalnej. Rezerwowanie IP, możesz wiedzieli, źródłowy adres IP i nie trzeba zaktualizować reguł zapory z powodu zmianie adresów IP.

## <a name="faq"></a>FAQ
1. Czy można używać zastrzeżony adres IP dla wszystkich usług Azure?  
  - Zastrzeżone adresy IP można używać tylko dla maszyny wirtualne i ról wystąpienie usługi cloud dostępne za pośrednictwem VIP.
1. Ile zastrzeżone adresy IP może mieć?  
  - W tej chwili wszystkie subskrypcje Azure mogą korzystać 20 zastrzeżone adresy IP. Możesz jednak zażądać dodatkowych zastrzeżone adresy IP. Odwiedź stronę [subskrypcji i ograniczenia usługi](../azure-subscription-service-limits.md) , aby uzyskać więcej informacji.
1. Czy istnieje opłatę dla zastrzeżone adresy IP?
  - Aby uzyskać informacje o cenach, zobacz [Szczegóły dotyczące ceny adresu IP zastrzeżone](http://go.microsoft.com/fwlink/?LinkID=398482) .
1. Jak zarezerwować adres IP?
  - Za pomocą programu PowerShell lub [Interfejsu API usługi REST zarządzania Azure](https://msdn.microsoft.com/library/azure/dn722420.aspx) zarezerwować adresu IP w wybranym obszarze. Ten zastrzeżonego adresu IP jest skojarzona z subskrypcją. Nie można zarezerwować adres IP za pomocą portalu zarządzania.
1. Do czego służy ten z grupą koligacji podstawie VNets?
  - Zastrzeżone adresy IP są obsługiwane tylko w VNets regionalne. Nie jest obsługiwane dla VNets skojarzonych z grupami koligacji. Aby uzyskać więcej informacji na temat kojarzenia VNet regionie lub grupę koligacji zobacz [temat regionalne VNets i grupy koligacji](virtual-networks-migrate-to-regional-vnet.md).

## <a name="how-to-manage-reserved-vips"></a>Jak zarządzać służącą zastrzeżone

Zanim będzie można używać zastrzeżone adresy IP, należy dodać do swojej subskrypcji. Aby utworzyć zastrzeżony adres IP z puli dostępne publicznych adresów IP w lokalizacji *Centralnej USA* , uruchom następujące polecenia programu PowerShell:

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"

Zwróć uwagę, że nie można określić, co to jest IP zastrzeżone. Aby zobaczyć, jakie adresy IP są zarezerwowane w ramach subskrypcji, uruchom następujące polecenie programu PowerShell i zwróć uwagę wartości *ReservedIPName* i *adres*:

    Get-AzureReservedIP

    ReservedIPName       : MyReservedIP
    Address              : 23.101.114.211
    Id                   : d73be9dd-db12-4b5e-98c8-bc62e7c42041
    Label                :
    Location             : Central US
    State                : Created
    InUse                : False
    ServiceName          :
    DeploymentName       :
    OperationDescription : Get-AzureReservedIP
    OperationId          : 55e4f245-82e4-9c66-9bd8-273e815ce30a
    OperationStatus      : Succeeded

Gdy jest zastrzeżona IP, pozostaje skojarzona z subskrypcją dopóki nie zostaną usunięte. Aby usunąć zastrzeżony adres IP, jak pokazano powyżej, uruchom następujące polecenia programu PowerShell:

    Remove-AzureReservedIP -ReservedIPName "MyReservedIP"

## <a name="how-to-reserve-the-ip-address-of-an-existing-cloud-service"></a>Jak zarezerwować adres IP istniejącego usługi w chmurze

Można zarezerwować adres IP istniejącego usługi w chmurze, dodając parametr *- NazwaUsługi* . Aby zarezerwować adres IP usługi w chmurze *TestService* w lokalizacji *Centralnej Stanów Zjednoczonych* , uruchom następujące polecenia programu PowerShell:

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US" -ServiceName TestService


## <a name="how-to-associate-a-reserved-ip-to-a-new-cloud-service"></a>Jak skojarzyć zastrzeżony adres IP do nowej usługi w chmurze
Poniżej skrypt tworzy nowe zastrzeżony adres IP, a następnie kojarzy go do nowej usługi w chmurze o nazwie *TestService*.

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| New-AzureVM -ServiceName TestService -ReservedIPName MyReservedIP -Location "Central US"

>[AZURE.NOTE] Po utworzeniu zastrzeżony adres IP do użytku z usługi w chmurze, nadal konieczne będzie dotyczyć maszyn wirtualnych przy użyciu *VIP:&lt;numer portu >* dla komunikacji przychodzącej. Rezerwowanie adresów IP nie oznacza, że możesz połączyć się ze maszyn wirtualnych bezpośrednio. Zastrzeżony adres IP jest przypisany do usługi w chmurze, który został wdrożony maszyn wirtualnych do. Jeśli chcesz nawiązać połączenie maszyn wirtualnych według adresów IP bezpośrednio, należy skonfigurować publiczny adres IP poziom wystąpienie. Publiczny adres IP poziom wystąpienie jest typem publiczny adres IP (nazywane ILPIP) przypisany bezpośrednio do swojej maszyn wirtualnych. Nie można zarezerwować. Aby uzyskać więcej informacji, zobacz [Wystąpienia poziomu publicznych adresów IP (ILPIP)](virtual-networks-instance-level-public-ip.md) .

## <a name="how-to-remove-a-reserved-ip-from-a-running-deployment"></a>Jak usunąć zastrzeżony adres IP z uruchomionego wdrażania
Aby usunąć zastrzeżonego adresu IP dodane do nowej usługi utworzone w skrypt powyżej, uruchom następujące polecenia programu PowerShell:

    Remove-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService

>[AZURE.NOTE] Odebranie zastrzeżony adres IP uruchomionego wdrożenia nie powoduje usunięcia zastrzeżenia ze swojej subskrypcji. Powoduje po prostu zwolnienie IP może być używany przez inny zasób w ramach subskrypcji.

## <a name="how-to-associate-a-reserved-ip-to-a-running-deployment"></a>Jak skojarzyć zastrzeżony adres IP uruchomionego wdrażania
Skrypt poniżej umożliwia utworzenie nowej usługi w chmurze o nazwie *TestService2* z nowej maszyny o nazwie *TestVM2*, a następnie kojarzy istniejących zastrzeżonego adresu IP o nazwie *MyReservedIP* do usługi w chmurze.

    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM2 -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| New-AzureVM -ServiceName TestService2 -Location "Central US"
    Set-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService2

## <a name="how-to-associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file"></a>Jak skojarzyć zastrzeżony adres IP do usługi w chmurze za pomocą pliku konfiguracji usługi
Można także skojarzyć zastrzeżony adres IP do usługi w chmurze za pomocą usługi pliku konfiguracji (CSCFG). Xml przykładowe poniżej przedstawiono sposób konfigurowania usługi w chmurze umożliwia zastrzeżone VIP o nazwie *MyReservedIP*:

    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <AddressAssignments>
          <ReservedIPs>
           <ReservedIP name="MyReservedIP"/>
          </ReservedIPs>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## <a name="next-steps"></a>Następne kroki

- Zrozumieć, jak [adresy IP](virtual-network-ip-addresses-overview-classic.md) działa w modelu Klasyczny wdrożenia.

- Informacje na temat [zastrzeżone prywatnych adresów IP](virtual-networks-reserved-private-ip.md).

- Informacje na temat [adresów IP publicznej poziom wystąpienia (ILPIP)](virtual-networks-instance-level-public-ip.md).
