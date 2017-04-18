<properties
   pageTitle="Jak skonfigurować routing obwód ExpressRoute dla modelu Klasyczny wdrażania przy użyciu programu PowerShell | Microsoft Azure"
   description="W tym artykule opisano kroki tworzenia i inicjowania obsługi administracyjnej prywatnych, publicznych i Microsoft zaglądanie obwodu ExpressRoute. Ten artykuł zawiera także jak sprawdzić stan, aktualizowanie lub usuwanie peerings dla swojego obwodu."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr"/>

# <a name="create-and-modify-routing-for-an-expressroute-circuit"></a>Tworzenie i modyfikowanie routing obwód ExpressRoute

> [AZURE.SELECTOR]
[Portal Azure - Menedżer zasobów](expressroute-howto-routing-portal-resource-manager.md)
[programu PowerShell — Menedżer zasobów](expressroute-howto-routing-arm.md)
[PowerShell — klasyczny](expressroute-howto-routing-classic.md)



W tym artykule opisano kroki tworzenia i zarządzania konfiguracji routingu dla obwodu ExpressRoute przy użyciu programu PowerShell i model klasyczny wdrożenia. Poniższe kroki będzie również pokazano, jak sprawdzić stan, aktualizowanie lub usuwanie i deprovision peerings dla obwodu ExpressRoute.


**Informacje dotyczące modeli Azure wdrażania**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-prerequisites"></a>Wymagania wstępne dotyczące konfiguracji

- Konieczne będzie najnowszą wersję pakietu moduły Azure programu PowerShell. Możesz pobrać najnowszą modułu programu PowerShell z sekcji programu PowerShell [strony pobierania Azure](https://azure.microsoft.com/downloads/). Postępuj zgodnie z instrukcjami na stronie [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) , aby uzyskać instrukcje krok po kroku dotyczące konfigurowania komputera do moduły Azure programu PowerShell. 
- Upewnij się, że przejrzeniu stronie [wymagania wstępne](expressroute-prerequisites.md) , strona [routingu wymagania](expressroute-routing.md) i strona [przepływy pracy](expressroute-workflows.md) przed rozpoczęciem konfiguracji.
- Musi być aktywna obwód ExpressRoute. Postępuj zgodnie z instrukcjami, aby [utworzyć obwód ExpressRoute](expressroute-howto-circuit-classic.md) i że masz elektrycznego włączone przez dostawcę usługi łączności, przed kontynuowaniem. Obwód ExpressRoute musi być w stanie ustanawianie i włączony będzie możliwe uruchomić polecenia cmdlet, opisane poniżej.

>[AZURE.IMPORTANT] Te instrukcje dotyczą tylko obwody utworzone za pomocą dostawców usług oferujących usługi łączności warstwy 2. Jeśli używasz usługodawcy oferujących usługi zarządzanych Layer 3 (zazwyczaj IPVPN, takich jak MPLS) dostawcy łączności Konfigurowanie i zarządzanie routingu dla Ciebie.

Można skonfigurować jeden, dwa lub wszystkie trzy peerings (Azure publicznych prywatne, Azure i Microsoft) dla obwodu ExpressRoute. Peerings można konfigurować w wybranym w jakiejkolwiek kolejności. Jednak należy się upewnić, aby zakończyć konfigurację poszczególnych peering w danej chwili. 

## <a name="azure-private-peering"></a>Azure zaglądanie prywatnych

Ta sekcja zawiera instrukcje na temat Tworzenie, pobieranie, aktualizowanie i usuwanie Azure prywatne konfiguracji peering obwód ExpressRoute. 

### <a name="to-create-azure-private-peering"></a>Aby utworzyć Azure zaglądanie prywatnych

1. **Importowanie modułu programu PowerShell dla ExpressRoute.**
    
    Moduły Azure i ExpressRoute należy zaimportować do sesji programu PowerShell, aby rozpocząć korzystanie z poleceń cmdlet ExpressRoute. Uruchom następujące polecenia zaimportować moduły Azure i ExpressRoute do sesji programu PowerShell.  

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **Tworzenie obwodu ExpressRoute.**
    
    Postępuj zgodnie z instrukcjami, aby utworzyć [obwód ExpressRoute](expressroute-howto-circuit-classic.md) i masz przygotowana przez dostawcę łączności. Jeśli dostawca łączności oferuje usług zarządzanych Layer 3, możesz przejąć dostawcy łączności umożliwiające Azure zaglądanie prywatnych dla Ciebie. W takim przypadku nie będzie wymagało postępuj zgodnie z instrukcjami w następnej sekcji. Jednak jeśli dostawcy łączności nie zarządza routingu, po utworzeniu z obwodem, wykonaj poniższe instrukcje. 

3. **Sprawdź obwodu ExpressRoute, aby upewnić się, że zainicjowaniu obsługi administracyjnej.**

    Należy najpierw zaznaczyć, aby wyświetlić obwód ExpressRoute jest obsługi administracyjnej, a także włączone. Zobacz przykład poniżej.

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    Upewnij się, że obwód to Provisioned i włączone. W przeciwnym razie Praca z dostawcy łączności, aby przejść do obwód do wymaganego stanu i stanu.

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled


4. **Konfigurowanie Azure prywatne zaglądanie do układu.**

    Upewnij się, dysponować następującymi elementami, przed kontynuowaniem następne kroki:

    - /30 podsieć łącze podstawowe. Nie może być częścią dowolne miejsce adres zastrzeżone wirtualnych sieci.
    - /30 podsieci pomocniczej łącza. Nie może być częścią dowolne miejsce adres zastrzeżone wirtualnych sieci.
    - Prawidłowego Identyfikatora VLAN, aby ustalić to zaglądanie na. Upewnij się, że nie inne zaglądanie w układzie używa tego samego identyfikatora VLAN.
    - JAKO numer zaglądanie. Można użyć zarówno 2-bajtowa i 4-bajtowe jako liczby. Możesz użyć prywatnej jako numer ten zaglądanie. Upewnij się, że nie używasz 65515.
    - Mieszanie MD5 wybranie korzystać. **Jest to opcjonalne**.
    
    Możesz uruchomić następujące polecenie cmdlet, aby skonfigurować Azure zaglądanie prywatnych dla swojej obwodu.

        New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100

    Jeśli zdecydujesz się na użycie mieszanie MD5, możesz użyć polecenia cmdlet poniżej.

        New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100 -SharedKey "A1B2C3D4"

    >[AZURE.IMPORTANT] Upewnij się, określ swój numer jako zaglądanie WPW klienta ASN.

### <a name="to-view-azure-private-peering-details"></a>Aby wyświetlić szczegóły Azure zaglądanie prywatnych

Możesz uzyskać szczegółowe informacje o konfiguracji przy użyciu następującego polecenia cmdlet

    Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 100


### <a name="to-update-azure-private-peering-configuration"></a>Aby zaktualizować konfigurację Azure zaglądanie prywatnych

Możesz zaktualizować dowolną część konfiguracji, używając następującego polecenia cmdlet. W poniższym przykładzie Identyfikatora sieci obwodu jest aktualizowany od 100 do 500.

    Set-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 500 -SharedKey "A1B2C3D4"

### <a name="to-delete-azure-private-peering"></a>Aby usunąć Azure zaglądanie prywatnych

Możesz usunąć peering konfigurację, uruchamiając następujące polecenie cmdlet.

>[AZURE.WARNING] Należy się upewnić, że wszystkie wirtualnych sieci nie są połączone z obwodem ExpressRoute przed uruchomieniem tego polecenia cmdlet. 

    Remove-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"


## <a name="azure-public-peering"></a>Azure zaglądanie publicznej

Ta sekcja zawiera instrukcje dotyczące tworzenia, pobieranie, aktualizowanie i usuwanie Azure publicznej konfiguracji peering obwód ExpressRoute.

### <a name="to-create-azure-public-peering"></a>Aby utworzyć Azure zaglądanie publicznej

1. **Importowanie modułu programu PowerShell dla ExpressRoute.**
    
    Aby rozpocząć korzystanie z poleceń cmdlet ExpressRoute, należy zaimportować modułów Azure i ExpressRoute do sesji programu PowerShell. Uruchom następujące polecenia zaimportować moduły Azure i ExpressRoute do sesji programu PowerShell. 

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **Tworzenie obwodu ExpressRoute**
    
    Postępuj zgodnie z instrukcjami, aby utworzyć [obwód ExpressRoute](expressroute-howto-circuit-classic.md) i masz przygotowana przez dostawcę łączności. Jeśli dostawca łączności oferuje usług zarządzanych Layer 3, możesz przejąć dostawcy łączności umożliwiające Azure publicznej zaglądanie dla Ciebie. W takim przypadku nie będzie wymagało postępuj zgodnie z instrukcjami w następnej sekcji. Jednak jeśli dostawcy łączności nie zarządza routingu, po utworzeniu z obwodem, wykonaj poniższe instrukcje.

3. **Sprawdzanie obwodu ExpressRoute, aby upewnić się, że jest obsługi administracyjnej**

    Należy najpierw zaznaczyć, aby wyświetlić obwód ExpressRoute jest obsługi administracyjnej, a także włączone. Zobacz przykład poniżej.

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    Upewnij się, że obwód to Provisioned i włączone. W przeciwnym razie Praca z dostawcy łączności, aby przejść do obwód do wymaganego stanu i stanu.

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled

    

4. **Konfigurowanie Azure publicznej zaglądanie do układu**

    Upewnij się, że masz następujące informacje, przed kontynuowaniem dalej.

    - /30 podsieć łącze podstawowe. Musi to być prawidłowy prefiks IPv4 publicznej.
    - /30 podsieci pomocniczej łącza. Musi to być prawidłowy prefiks IPv4 publicznej.
    - Prawidłowego Identyfikatora VLAN, aby ustalić to zaglądanie na. Upewnij się, że nie inne zaglądanie w układzie używa tego samego identyfikatora VLAN.
    - JAKO numer zaglądanie. Można użyć zarówno 2-bajtowa i 4-bajtowe jako liczby.
    - Mieszanie MD5 wybranie korzystać. **Jest to opcjonalne**.
    
    Można uruchomić następujące polecenie cmdlet, aby skonfigurować Azure publicznej zaglądanie dla swojego obwodu

        New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200

    Możesz użyć polecenia cmdlet poniżej, jeśli zdecydujesz się na użycie mieszanie MD5

        New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200 -SharedKey "A1B2C3D4"

    >[AZURE.IMPORTANT] Upewnij się, określ swój numer jako zaglądanie WPW i klienta ASN.

### <a name="to-view-azure-public-peering-details"></a>Aby wyświetlić szczegóły zaglądanie publicznej Azure

Możesz uzyskać szczegółowe informacje o konfiguracji przy użyciu następującego polecenia cmdlet

    Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 131.107.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 131.107.0.4/30
    State                          : Enabled
    VlanId                         : 200


### <a name="to-update-azure-public-peering-configuration"></a>Aby zaktualizować konfigurację zaglądanie publicznej Azure

Można aktualizować dowolną część konfiguracji, używając następującego polecenia cmdlet

    Set-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 600 -SharedKey "A1B2C3D4"

Identyfikatora sieci obwodu jest aktualizowany od 200 do 600 w powyższym przykładzie.

### <a name="to-delete-azure-public-peering"></a>Aby usunąć Azure zaglądanie publicznej

Możesz usunąć peering konfigurację, uruchamiając następujące polecenie cmdlet

    Remove-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

## <a name="microsoft-peering"></a>Zaglądanie firmy Microsoft

Ta sekcja zawiera instrukcje dotyczące tworzenia, pobieranie, aktualizowanie i usuwanie dla obwodu ExpressRoute peering konfiguracji systemu Microsoft. 

### <a name="to-create-microsoft-peering"></a>Aby utworzyć zaglądanie firmy Microsoft

1. **Importowanie modułu programu PowerShell dla ExpressRoute.**
    
    Moduły Azure i ExpressRoute należy zaimportować do sesji programu PowerShell, aby rozpocząć korzystanie z poleceń cmdlet ExpressRoute. Uruchom następujące polecenia zaimportować moduły Azure i ExpressRoute do sesji programu PowerShell.  

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **Tworzenie obwodu ExpressRoute**
    
    Postępuj zgodnie z instrukcjami, aby utworzyć [obwód ExpressRoute](expressroute-howto-circuit-classic.md) i masz przygotowana przez dostawcę łączności. Jeśli dostawca łączności oferuje usług zarządzanych Layer 3, możesz przejąć dostawcy łączności umożliwiające Azure zaglądanie prywatnych dla Ciebie. W takim przypadku nie będzie wymagało postępuj zgodnie z instrukcjami w następnej sekcji. Jednak jeśli dostawcy łączności nie zarządza routingu, po utworzeniu z obwodem, wykonaj poniższe instrukcje.

3. **Sprawdzanie obwodu ExpressRoute, aby upewnić się, że jest obsługi administracyjnej**

    Należy najpierw zaznaczyć, aby sprawdzić, czy obwód ExpressRoute jest w stanie Provisioned i włączone.

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    Upewnij się, że obwód to Provisioned i włączone. W przeciwnym razie Praca z dostawcy łączności, aby przejść do obwód do wymaganego stanu i stanu.

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled


4. **Konfigurowanie Microsoft zaglądanie do układu**

    Upewnij się, że masz następujące informacje, przed kontynuowaniem.

    - /30 podsieć łącze podstawowe. Musi to być prawidłowy publicznej prefiks IPv4 należącą do Ciebie i zarejestrowane w RIR-IRR.
    - /30 podsieci pomocniczej łącza. Musi to być prawidłowy publicznej prefiks IPv4 należącą do Ciebie i zarejestrowane w RIR-IRR.
    - Prawidłowego Identyfikatora VLAN, aby ustalić to zaglądanie na. Upewnij się, że nie inne zaglądanie w układzie używa tego samego identyfikatora VLAN.
    - JAKO numer zaglądanie. Można użyć zarówno 2-bajtowa i 4-bajtowe jako liczby.
    - Ogłaszane prefiksy: należy podać listę wszystkich prefiksów zamierzasz ogłaszanie sesji BGP. Tylko publicznej prefiksy adresów IP są akceptowane. Możesz wysłać listę rozdzielanych przecinkami, jeśli zamierzasz Wyślij zestaw prefiksów. Tych prefiksów musi być zarejestrowany dla Ciebie w RIR-IRR.
    - Odbiorca ASN: Jeśli prefiksy reklam, które nie są rejestrowane do zaglądanie jako liczby, można określić numer AS, do której są rejestrowane. **Jest to opcjonalne**.
    - Kierowanie rejestru Nazwa: Można określić RIR-IRR, dla której numer i prefiksy są rejestrowane.
    - Mieszanie MD5, jeśli chcesz użyć jednego. **Jest to opcjonalne.**
    
    Można uruchomić następujące polecenie cmdlet, aby skonfigurować pering firmy Microsoft dla swojego obwodu

        New-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"


### <a name="to-view-microsoft-peering-details"></a>Aby wyświetlić szczegóły zaglądanie firmy Microsoft

Możesz uzyskać szczegółowe informacje o konfiguracji przy użyciu następującego polecenia cmdlet.

    Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 123.0.0.0/30
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 2245
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : ARIN
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 300


### <a name="to-update-microsoft-peering-configuration"></a>Aby zaktualizować konfigurację zaglądanie firmy Microsoft

Możesz zaktualizować dowolną część konfiguracji, używając następującego polecenia cmdlet.

        Set-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"

### <a name="to-delete-microsoft-peering"></a>Aby usunąć zaglądanie firmy Microsoft

Możesz usunąć peering konfigurację, uruchamiając następujące polecenie cmdlet.

    Remove-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

## <a name="next-steps"></a>Następne kroki

Następne, [łącze VNet obwodem ExpressRoute](expressroute-howto-linkvnet-classic.md).


-  Aby uzyskać więcej informacji dotyczących przepływów pracy zobacz [ExpressRoute przepływy pracy](expressroute-workflows.md).
-  Aby uzyskać więcej informacji na temat obwodu zaglądanie zobacz [ExpressRoute obwody elektryczne i układy domen routingu](expressroute-circuit-peerings.md).

