<properties 
   pageTitle="Konfigurowanie wymuszonego tunneling połączeń witryny do witryny przy użyciu modelu Klasyczny wdrożenia | Microsoft Azure"
   description="Jak przekierowywać lub "siły" cały ruch powiązanych z Internetem powrót do lokalizacji lokalnego."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/10/2016"
   ms.author="cherylmc" />

# <a name="configure-forced-tunneling-using-the-classic-deployment-model"></a>Konfigurowanie wymuszonego tunelowania przy użyciu modelu Klasyczny wdrażania

> [AZURE.SELECTOR]
- [PowerShell — klasyczny](vpn-gateway-about-forced-tunneling.md)
- [PowerShell — Menedżera zasobów](vpn-gateway-forced-tunneling-rm.md)

Wymuszone tunneling pozwala Przekieruj lub "życie" cały ruch Internet związanych z powrotem do lokalizacji lokalnego za pośrednictwem tunelem VPN witryny do witryny, kontroli i inspekcji. Jest to wymagane krytycznych dla większości enterprise IT zasady. 

Bez wymuszonego tunneling ruch Internet związanych z pośrednictwem usługi SMS platformy Azure będzie zawsze przechodzenia z infrastruktury sieciowej Azure bezpośrednio się z Internetem, bez opcji pozwala sprawdzać lub kontrolować dane. Dostęp do Internetu nieautoryzowanym potencjalnie może prowadzić do ujawnienie informacji lub innych typów naruszenia bezpieczeństwa.

W tym artykule przeprowadzi Cię przez konfigurowanie wymuszone tunneling wirtualnych sieci utworzony przy użyciu modelu Klasyczny wdrożenia. 

**Informacje dotyczące modeli Azure wdrażania**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

**Modeli wdrażania i narzędzia umożliwiające wymuszonego tunneling**

Połączenie tunelowania wymuszonego można skonfigurować dla modelu Klasyczny wdrożenia i model wdrożenia Menedżera zasobów. Zobacz poniższej tabeli, aby uzyskać więcej informacji. Firma Microsoft aktualizować w tej tabeli jako nowe artykuły, nowe modele wdrożenia i dodatkowe narzędzia staną się dostępne dla tej konfiguracji. Po udostępnieniu artykułu możemy łącze do niego bezpośrednio z tabeli.

[AZURE.INCLUDE [vpn-gateway-forcedtunnel](../../includes/vpn-gateway-table-forcedtunnel-include.md)] 


## <a name="requirements-and-considerations"></a>Wymagania i zagadnienia

Wymuszone tunneling platformy Azure skonfigurowano za pośrednictwem wirtualnych sieci przekierowuje zdefiniowane przez użytkownika (UDR). Przekierowywanie ruchu do lokalnej witryny jest wyrażony jako domyślnej trasy do bramy Azure VPN. Sekcji poniżej wymieniono bieżące ograniczenie routingu tabeli i przekierowuje dla wirtualnej sieci Azure:


-  Każda podsieć wirtualną sieć ma wbudowanych tabeli routingu systemu. Tabela routingu systemowa występują następujące trzy grupy przekierowuje:

    - **Kieruje lokalne VNet:** Bezpośrednio do miejsca docelowego maszyny wirtualne w tej samej sieci wirtualnej
    
    - **Na trasy lokalnej:** Aby brama Azure VPN
    
    - **Trasy domyślnej:** Bezpośrednio do Internetu. Pakiety przeznaczone do prywatnych adresów IP nie są objęte poprzedniego na dwa sposoby zostaną usunięte.


-  W wersji przekierowuje zdefiniowane przez użytkownika można utworzyć tabelę routingu, aby dodać trasę domyślną i następnie skojarzyć tabeli routingu do adresy podsieci usługi VNet mogą być używane w taki sposób, umożliwiający wymuszonego tunneling tych podsieci.

- Należy ustawić "domyślnej witryny" między lokalne witryny lokalnej krzyżowe połączony z siecią wirtualną.

- Wymuszone tunneling muszą być skojarzone z VNet, zawierającą dynamiczne routingu VPN bramy (nie statyczne).
 
- ExpressRoute wymuszone tunneling nie jest skonfigurowany za pomocą tego mechanizmu, ale zamiast tego jest włączona przez reklamę trasę domyślną za pośrednictwem sesji peering ExpressRoute BGP. Można znaleźć w [Dokumentacji ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) , aby uzyskać więcej informacji.



## <a name="configuration-overview"></a>Omówienie konfiguracji

W poniższym przykładzie Frontend podsieci nie jest wymuszone tunelowany. Obciążenie pracą w podsieci Frontend nadal akceptować i odpowiadać na wezwania klienta i z Internetu. Wymuszenie dużych i wewnętrznej bazy danych podsieci tunelowanej. Wymuszone lub przekierowany z powrotem do lokalnej witryny przy użyciu jednej z tuneli S2S VPN wszystkie połączenia wychodzące z tych dwóch podsieci w Internecie.

Pozwala na ograniczanie i sprawdzanie dostęp do Internetu w środowisku maszyn wirtualnych systemu lub chmury usługi Azure, pozostawiając umożliwiające architektury usługi wielu wymagane. Możesz również zastosować wymuszonego tunelowania do całej sieci wirtualnych w przypadku nie obciążenia z Internetu w sieci wirtualnej.


![Wymuszone Tunneling](./media/vpn-gateway-about-forced-tunneling/forced-tunnel.png)



## <a name="before-you-begin"></a>Przed rozpoczęciem

Sprawdź, czy masz następujące elementy przed rozpoczęciem konfiguracji.

- Subskrypcję usługi Azure. Jeśli nie masz jeszcze subskrypcji usługi Azure, można aktywować swoje [korzyści subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) lub Utwórz konto [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial/).

- Skonfigurowaną wirtualnej sieci. 

- Najnowsza wersja polecenia cmdlet programu PowerShell Azure. Aby uzyskać więcej informacji o instalowaniu poleceń cmdlet programu PowerShell, zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) .


## <a name="configure-forced-tunneling"></a>Konfigurowanie wymuszonego tunneling

Poniższa procedura umożliwia określenie, wymuszonego tunelowanie dla wirtualnej sieci. Kroki konfiguracji odpowiadają pliku konfiguracji sieci VNet.



    <VirtualNetworkSite name="MultiTier-VNet" Location="North Europe">
     <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="DefaultSiteHQ">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch1">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch2">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch3">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
        </Gateway>
      </VirtualNetworkSite>
    </VirtualNetworkSite>

W tym przykładzie, wirtualną sieć "MultiTier VNet" występują trzy podsieci: podsieci *serwera sieci Web*, *Midtier*i *wewnętrznej bazy danych* , z połączeniami między czterema w środowisku lokalnym: *DefaultSiteHQ*i trzy *oddziałów*. 

Kroki będą *DefaultSiteHQ* jako domyślne połączenie witryny dla wymuszone tunneling i skonfigurować Midtier i wymuszone podsieci wewnętrznej bazy danych, aby użyć tunelowania.


1. Utwórz tabelę routingu. Następujące polecenie cmdlet umożliwia utworzenie tabeli routingu.

        New-AzureRouteTable –Name "MyRouteTable" –Label "Routing Table for Forced Tunneling" –Location "North Europe"

2. Dodaj domyślną trasę do tabeli routingu. 

    Poniższy przykład dodaje domyślną trasę do tabeli routingu utworzony w kroku 1. Należy zauważyć, że tylko trasę obsługiwane jest prefiks "0.0.0.0/0" do "VPNGateway" następny skok miejsca docelowego.
 
        Get-AzureRouteTable -Name "MyRouteTable" | Set-AzureRoute –RouteTable "MyRouteTable" –RouteName "DefaultRoute" –AddressPrefix "0.0.0.0/0" –NextHopType VPNGateway

3. Kojarzenie tabeli routingu do podsieci. 

    Po utworzeniu tabeli routingu i dodawana trasa, użyj w poniższym przykładzie, aby dodać lub skojarz tę tabelę trasy do podsieci VNet. W przykładzie dodawane tabelę routingu "MyRouteTable" VNet MultiTier-VNet podsieci Midtier i wewnętrznej bazy danych.

        Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Midtier" -RouteTableName "MyRouteTable"

        Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Backend" -RouteTableName "MyRouteTable"

4. Przypisywanie domyślnej witryny dla wymuszone tunelowania. 

    W poprzednim kroku przykładowe skrypty polecenia cmdlet utworzona tabela routingu i skojarzone tabeli trasy do dwóch podsieci VNet. Pozostały krok należy wybrać spośród wielu witrynach połączenia wirtualnej sieci lokalnej witryny jako domyślnej witryny lub tunelem.

        $DefaultSite = @("DefaultSiteHQ")
        Set-AzureVNetGatewayDefaultSite –VNetName "MultiTier-VNet" –DefaultSite "DefaultSiteHQ"

## <a name="additional-powershell-cmdlets"></a>Dodatkowe polecenia cmdlet programu PowerShell

### <a name="to-delete-a-route-table"></a>Aby usunąć tabelę routingu

    Remove-AzureRouteTable -Name <routeTableName>

### <a name="to-list-a-route-table"></a>Aby wyświetlić tabelę routingu

    Get-AzureRouteTable [-Name <routeTableName> [-DetailLevel <detailLevel>]]

### <a name="to-delete-a-route-from-a-route-table"></a>Aby usunąć trasę z tabeli routingu

    Remove-AzureRouteTable –Name <routeTableName>

### <a name="to-remove-a-route-from-a-subnet"></a>Aby usunąć trasę z podsieci

    Remove-AzureSubnetRouteTable –VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>

### <a name="to-list-the-route-table-associated-with-a-subnet"></a>Aby wyświetlić tabelę routingu skojarzone z podsieci
    
    Get-AzureSubnetRouteTable -VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>

### <a name="to-remove-a-default-site-from-a-vnet-vpn-gateway"></a>Aby usunąć domyślnej witryny z bramą VNet VPN

    Remove-AzureVnetGatewayDefaultSite -VNetName <virtualNetworkName>






