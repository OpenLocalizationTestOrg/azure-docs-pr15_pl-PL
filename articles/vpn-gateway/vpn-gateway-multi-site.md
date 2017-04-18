<properties 
   pageTitle="Nawiązywanie połączenia wirtualnej sieci z wielu witryn przy użyciu programu PowerShell i Brama VPN | Microsoft Azure"
   description="Ten artykuł prowadzi użytkownika przez wielu witrynach lokalne lokalnego nawiązywanie połączenia wirtualnej sieci przy użyciu bramy sieci VPN dla modelu Klasyczny wdrożenia."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags="azure-service-management"/>

<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/11/2016"
   ms.author="yushwang" />

# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection"></a>Dodawanie witryny do witryny połączenia z serwisem VNet z istniejącego połączenia VPN bramy

> [AZURE.SELECTOR]
- [Menedżer zasobów — portalu](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
- [Klasyczny - programu PowerShell](vpn-gateway-multi-site.md)

W tym artykule opisano dodawanie połączeń między witryny (S2S) do bramy sieci VPN, która ma istniejącego połączenia za pomocą programu PowerShell. Tego typu połączenia są często nazywane Konfiguracja "wielu witryny". 

Ten artykuł dotyczy wirtualnych sieci utworzony przy użyciu modelu Klasyczny wdrożenia (usługa Zarządzanie). Te kroki nie dotyczą konfiguracji połączenia współistnienia ExpressRoute---witryny. Zobacz [połączenia współistnienia ExpressRoute-S2S](../expressroute/expressroute-howto-coexist-classic.md) uzyskać informacji na temat współistnienia połączenia.

### <a name="deployment-models-and-methods"></a>Modeli wdrażania i metody

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Firma Microsoft aktualizować jako nowe artykuły w tej tabeli i dodatkowe narzędzia staną się dostępne dla tej konfiguracji. Po udostępnieniu artykułu możemy łącze do niego bezpośrednio z tej tabeli.

[AZURE.INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)] 



## <a name="about-connecting"></a>O nawiązywaniu połączeń

Wiele witryn lokalnego można nawiązać jednej wirtualnej sieci. To jest szczególnie atrakcyjny dla tworzenie rozwiązań chmury hybrydowych. Tworzenie połączenia wielu witryn Centrum Azure wirtualnej sieci jest bardzo podobne do tworzenia innych połączeń witryny do witryny. W rzeczywistości, można użyć istniejącej bramy Azure VPN, jak bramy jest dynamiczne (w oparciu o rozsyłania).

Jeśli masz już statyczne bramy połączeni z siecią wirtualną, można zmienić typu bramy na dynamiczne bez konieczności odbudowanie wirtualną sieć, aby zezwalały na wiele witryn. Przed zmianą typu routingu, upewnij się, że bramy sieci VPN lokalnego obsługuje konfiguracji sieci VPN opartych na trasę. 

![diagram witryny wielu elementów] (./media/vpn-gateway-multi-site/multisite.png "wiele witryn")

## <a name="points-to-consider"></a>Kwestie do rozważenia

**Nie można korzystać z portalu klasyczny Azure, aby wprowadzić zmiany w tym wirtualnej sieci.** W tej wersji musisz wprowadzić zmiany w pliku konfiguracji sieci, zamiast za pomocą portalu klasyczny Azure. Jeśli wprowadzisz zmiany w portalu klasyczny Azure zastępują ustawienia odwołanie do wielu elementów witryny dla tej wirtualnej sieci. 

Uznasz zasadzie doświadczenia za pomocą pliku konfiguracji sieci przez godzinę wykonaniu procedury wiele witryn. Jednak jeśli masz wiele osób pracujących w konfiguracji sieci, musisz upewnij się, wszyscy zna to ograniczenie. To nie oznacza, że Portal klasyczny Azure nie można używać w ogóle. Możesz używać go do wszystkie inne części, z wyjątkiem wprowadzania zmian w konfiguracji z tą siecią wirtualną określonego.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Przed rozpoczęciem konfiguracji, należy sprawdzić, czy są następujące czynności:

- Subskrypcję usługi Azure. Jeśli nie masz jeszcze subskrypcji usługi Azure, można aktywować swoje [korzyści subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) lub Utwórz konto [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial/).

- Zgodny sprzęt VPN dla każdej lokalnej lokalizacji. Sprawdź [O urządzeniach VPN łączności sieciowej wirtualnych](vpn-gateway-about-vpn-devices.md) do weryfikacji, jeśli urządzenie, którego chcesz użyć jest coś, określanym w celu zachowania zgodności.

- Zewnętrznie przeciwległych publiczny adres IP protokołu IPv4 dla każdego urządzenia VPN. Adres IP nie może znajdować się za translatora adresów sieciowych. Jest wymagane.

- Musisz zainstalować najnowszą wersję programu PowerShell Azure poleceń cmdlet. Aby uzyskać więcej informacji o instalowaniu poleceń cmdlet programu PowerShell, zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) .

- Osoby, która jest wykorzystać Konfigurowanie sprzętu sieci VPN. Nie można skonfigurować urządzenia VPN za pomocą skryptów sieci VPN generowane automatycznie z portalu klasyczny Azure. Oznacza to, że musisz mieć znaczący zrozumienie sposobu Skonfiguruj urządzenie VPN oraz Praca z osobą, która ma.

- Zakresy adresów IP, których chcesz użyć na potrzeby wirtualnej sieci (jeśli jeszcze nie utworzono jeden). 

- Zakresy adresów IP dla każdej z tych witryn sieci lokalnej, które będzie łączyć się z. Konieczne będzie upewnij się, że zakresy adresów IP dla każdej z tych witryn sieci lokalnej, które chcesz nawiązać połączenie nie pokrywają się. W przeciwnym razie Portal klasyczny Azure lub interfejsu API usługi REST odrzuci konfiguracji przekazywanego. 

    Na przykład jeśli masz dwie witryny sieci lokalnej, że zawierają 10.2.3.0/24 zakres adres IP i masz pakiet przy użyciu docelowego adresu 10.2.3.3 Azure nie znasz witryn, które chcesz wysłać pakiet do, ponieważ wszystkie nakładające się zakresy adresów. Aby uniknąć problemów dotyczących routingu, Azure nie zezwala na przekazywanie pliku konfiguracji, który ma nakładających się zakresów.



## <a name="1-create-a-site-to-site-vpn"></a>1. Tworzenie VPN witryny do witryny

Jeśli masz już połączenie VPN witryny do witryny z bramą routingu dynamicznego ponosić! Można przystąpić do [wyeksportowania ustawienia konfiguracji wirtualnej sieci](#export). Jeśli nie, wykonaj następujące czynności:

### <a name="if-you-already-have-a-site-to-site-virtual-network-but-it-has-a-static-policy-based-routing-gateway"></a>Jeśli masz już wirtualnej sieci witryny do witryny, ale nie statyczne bramy routingu (na podstawie zasad):

1. Zmienić typ bramy do kierowania dynamiczne. Sieć VPN wiele witryn wymaga dynamiczne bramy routingu (nazywane także trasę w oparciu o). Aby zmienić typ bramy, musisz najpierw usunąć istniejące bramy, a następnie utwórz nowy. Aby uzyskać instrukcje zobacz [jak zmienić typ routingu VPN z bramy](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md#how-to-change-the-vpn-routing-type-for-your-gateway).  

2. Konfigurowanie usługi Nowa brama i Utwórz tunelem sieci VPN. Aby uzyskać instrukcje zobacz [Konfigurowanie bramy sieci VPN w portalu klasyczny Azure](vpn-gateway-configure-vpn-gateway-mp.md). Najpierw zmienić typ bramy, aby routing dynamiczny. 

### <a name="if-you-dont-have-a-site-to-site-virtual-network"></a>Jeśli nie masz wirtualnej sieci witryny do witryny:

1. Tworzenie wirtualnych sieci witryny do witryny przy użyciu te instrukcje: [Tworzenie wirtualnej sieci się przez połączenie VPN witryny do witryny w portalu klasyczny Azure](vpn-gateway-site-to-site-create.md).  

2. Konfigurowanie dynamicznego routingu bramy przy użyciu te instrukcje: [Konfigurowanie bramy sieci VPN](vpn-gateway-configure-vpn-gateway-mp.md). Należy wybrać **routing dynamiczny** typu bramy.

## <a name="export"></a>2. Eksportowanie pliku konfiguracji sieci 

Eksportowanie do pliku konfiguracji sieci. Wyeksportowany plik ma służyć do skonfigurować nowe ustawienia w wielu witrynach. Aby uzyskać instrukcje dotyczące wyeksportować plik, zobacz sekcję zawiera artykuł: [jak utworzyć VNet za pomocą pliku konfiguracji sieci w Azure Portal](../virtual-network/virtual-networks-create-vnet-classic-portal.md#how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal). 

## <a name="3-open-the-network-configuration-file"></a>3. Otwórz plik konfiguracji sieci

Otwórz pobranego pliku konfiguracji sieci w ostatnim kroku. Użyj dowolnego edytora xml, który chcesz. Plik powinna wyglądać podobnie do następującej:

        <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <LocalNetworkSites>
              <LocalNetworkSite name="Site1">
                <AddressSpace>
                  <AddressPrefix>10.0.0.0/16</AddressPrefix>
                  <AddressPrefix>10.1.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.2.3.4</VPNGatewayAddress>
              </LocalNetworkSite>
              <LocalNetworkSite name="Site2">
                <AddressSpace>
                  <AddressPrefix>10.2.0.0/16</AddressPrefix>
                  <AddressPrefix>10.3.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.4.5.6</VPNGatewayAddress>
              </LocalNetworkSite>
            </LocalNetworkSites>
            <VirtualNetworkSites>
              <VirtualNetworkSite name="VNet1" AffinityGroup="USWest">
                <AddressSpace>
                  <AddressPrefix>10.20.0.0/16</AddressPrefix>
                  <AddressPrefix>10.21.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="FE">
                    <AddressPrefix>10.20.0.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="BE">
                    <AddressPrefix>10.20.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="GatewaySubnet">
                    <AddressPrefix>10.20.2.0/29</AddressPrefix>
                  </Subnet>
                </Subnets>
                <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="Site1">
                      <Connection type="IPsec" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

## <a name="4-add-multiple-site-references"></a>4. dodać wiele odwołań witryny

Podczas dodawania lub usuwanie witryny informacje będą wprowadzisz zmiany konfiguracji ConnectionsToLocalNetwork-LocalNetworkSiteRef. Dodawanie nowego wyzwalaczy odwołanie lokalnej witryny Azure, aby utworzyć nowy tunelem. W poniższym przykładzie konfiguracja sieci jest połączenia pojedynczej witryny. Zapisz plik po zakończeniu wprowadzania zmian.

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

    To add additional site references (create a multi-site configuration), simply add additional "LocalNetworkSiteRef" lines, as shown in the example below: 

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Site2"><Connection type="IPsec" /></LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

## <a name="5-import-the-network-configuration-file"></a>5. Importowanie pliku konfiguracji sieci

Importowanie pliku konfiguracji sieci. Po zaimportowaniu tego pliku przy użyciu zmian zostanie dodany nowych tuneli. Tunele użyje bramy dynamiczne, który został utworzony wcześniej. Aby uzyskać instrukcje dotyczące importowania pliku, zobacz sekcję zawiera artykuł: [jak utworzyć VNet za pomocą pliku konfiguracji sieci w Azure Portal](../virtual-network/virtual-networks-create-vnet-classic-portal.md#how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal). 

## <a name="6-download-keys"></a>6. klucze Pobierz

Po dodaniu do nowych tuneli, należy użyć polecenia cmdlet programu PowerShell `Get-AzureVNetGatewayKey` uzyskanie kluczy wstępnych IPsec/IKE dla każdego tunelem.

Na przykład:

    Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site1"

    Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site2"

Jeśli wolisz, można uzyskać kluczy wstępnych w interfejsu API usługi REST *Uzyskiwanie wirtualna sieć udostępniane klucza bramy* .

## <a name="7-verify-your-connections"></a>7. Sprawdź połączenia

Sprawdź stan tunelem wiele witryn. Po pobraniu klawiszy dla każdego tunelem, należy sprawdzić połączenia. Używanie `Get-AzureVnetConnection` w celu uzyskania listy tuneli wirtualnej sieci, jak pokazano w poniższym przykładzie. VNet1 jest nazwą VNet.

    Get-AzureVnetConnection -VNetName VNET1
        
    ConnectivityState         : Connected
    EgressBytesTransferred    : 661530
    IngressBytesTransferred   : 519207
    LastConnectionEstablished : 5/2/2014 2:51:40 PM
    LastEventID               : 23401
    LastEventMessage          : The connectivity state for the local network site 'Site1' changed from Not Connected to Connected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site1
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7f68a8e6-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded
        
    ConnectivityState         : Connected
    EgressBytesTransferred    : 789398
    IngressBytesTransferred   : 143908
    LastConnectionEstablished : 5/2/2014 3:20:40 PM
    LastEventID               : 23401
    LastEventMessage          : The connectivity state for the local network site 'Site2' changed from Not Connected to Connected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site2
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7893b329-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat bram sieci VPN, zobacz [Temat bram sieci VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md).

