<properties
    pageTitle="Jak zaplanować wirtualnej sieci zbioru Azure RemoteApp | Microsoft Azure"
    description="Dowiedz się, jak zaplanować wirtualnej sieci zbioru Azure RemoteApp."
    services="remoteapp"
    documentationCenter="" 
    authors="mghosh1616"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="how-to-plan-your-virtual-network-for-azure-remoteapp"></a>Jak planować wirtualnej sieci Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Ten dokument opisano, jak skonfigurować Azure sieci wirtualnych (VNET), jak i podsieć Azure RemoteApp. Jeśli użytkownik nie zna Azure wirtualnych sieci, to funkcja, która ułatwia Aby dokonać wirtualizacji infrastruktury sieciowej w chmurze i tworzenia rozwiązania hybrydowe z Azure i lokalnych zasobów. Więcej informacji o jej [w tym miejscu](../virtual-network/virtual-networks-overview.md).

Jeśli chcesz zdefiniować zasady zabezpieczeń dla ruchu (przychodzące i wychodzące) w miejsce, w którym są wdrażania Azure RemoteApp wirtualnej sieci, zalecamy utworzyć osobną podsieć Azure RemoteApp od pozostałej części wdrażania usługi Azure wirtualnej sieci. Aby uzyskać więcej informacji dotyczących sposobu definiowania zasad zabezpieczeń w danej podsieci Azure wirtualnej sieci, przeczytaj [Co to jest grupa zabezpieczeń sieci (NSG)?](../virtual-network/virtual-networks-nsg.md).

## <a name="types-of-azure-remoteapp-collections-with-azure-virtual-networks"></a>Typy Azure RemoteApp kolekcji z Azure wirtualnych sieci

Poniższej ilustracji Pokaż dwie opcje innej kolekcji, gdy chcesz użyć wirtualnej sieci.

### <a name="azure-remoteapp-cloud-collection-with-vnet"></a>Azure zbiór chmury RemoteApp z VNET

 ![Azure RemoteApp - zbiór chmury z VNET](./media/remoteapp-planvpn/ra-cloudvpn.png)

Jest to zbiór Azure RemoteApp miejsce, w którym wszystkie zasoby, które sesji RemoteApp hosts muszą mieć dostęp do wdrożonych w Azure. Można je w tym samym VNET jako RemoteApp VNET lub innego VNET platformy Azure.

### <a name="azure-remoteapp-hybrid-collection-with-vnet"></a>Azure zbiór hybrydowych RemoteApp z VNET

![Azure RemoteApp - zbiór hybrydowego z VNET](./media/remoteapp-planvpn/ra-hybridvpn.png)

Jest to zbiór Azure RemoteApp niektóre zasoby, które hostów sesji RemoteApp muszą mieć dostęp do umiejscowienia wdrożonym lokalnego. RemoteApp VNET jest połączony z siecią lokalnego przy użyciu technologii hybrydowych Azure, takiego jak VPN lub rozsyłania Express witryny do witryny.


## <a name="how-the-system-works"></a>Jak działa system

W obszarze okładki Azure RemoteApp wdraża Azure maszyn wirtualnych (z przekazanego obrazu) do wybranego podczas inicjowania obsługi administracyjnej podsieci wirtualnej sieci. Jeśli przyjął zbioru hybrydowych próbie rozpoznawania nazwy FQDN kontrolera domeny, wprowadzone w obsługi administracyjnej przepływu pracy z serwera DNS w sieci wirtualnej.  
Jeśli łączysz się z istniejącą siecią wirtualną, upewnij się udostępnić potrzebne porty w Twojej sieci grupy zabezpieczeń w danej podsieci Azure RemoteApp. 

Zaleca się, że używasz [wystarczającą podsieć Azure RemoteApp](remoteapp-vnetsizing.md). Największa obsługiwanych przez Azure wirtualnej sieci jest /8 (za pomocą definicji podsieci CIDR). Danej podsieci powinny być wystarczająco duża, aby uwzględnić wszystkie maszyny RemoteApp Azure wirtualne w górę skali po więcej użytkowników uzyskują dostęp do aplikacji. 

Poniżej przedstawiono czynności, które należy włączyć w danej podsieci wirtualną sieć: 

2.  Ruch wychodzący z podsieci powinny mieć możliwość w zakresie portów 10101 10175 komunikowanie się za pomocą jednej z wewnętrznych usług Azure RemoteApp.
3.  Ruch wychodzący powinny mieć możliwość z danej podsieci nawiązywanie połączenia z magazynem Azure na porcie 443
4.  Jeśli masz usługi Active Directory obsługiwany w Azure, upewnij się, że wszelkie maszyn wirtualnych w wirtualnej sieci podsieć Azure RemoteApp się połączyć dla tego kontrolera domeny. Systemu DNS w sieci wirtualnej powinno być możliwe do rozpoznawania nazwy FQDN tego kontrolera domeny.


## <a name="virtual-network-with-forced-tunneling"></a>Wirtualna sieć z wymuszonego tunneling

[Wymuszone tunelowania](../vpn-gateway/vpn-gateway-about-forced-tunneling.md) jest teraz obsługiwane dla wszystkich nowych zbiorów Azure RemoteApp. Obecnie nie jest obsługiwana migracji istniejącego zbioru do obsługi tunneling wymuszonego.  Musisz usunąć wszystkie istniejące zbiory przy użyciu VNET, do którego tworzysz łącze, aby Azure RemoteApp i utworzyć nową, aby uzyskać wymuszone tunneling włączone kolekcji. 
