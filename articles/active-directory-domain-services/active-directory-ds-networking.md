<properties
    pageTitle="Azure usługach domenowych AD: Wskazówki dotyczące sieci | Microsoft Azure"
    description="Sieć usługi Azure Active Directory Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="maheshu"/>

# <a name="networking-considerations-for-azure-ad-domain-services"></a>Sieć zagadnienia związane z usług domenowych AD Azure

## <a name="how-to-select-an-azure-virtual-network"></a>Jak zaznaczyć Azure wirtualnej sieci
Poniższe wskazówki ułatwiają wybierz wirtualnej sieci korzystać z usług domenowych AD Azure.

### <a name="type-of-azure-virtual-network"></a>Typ Azure wirtualnej sieci

- Możesz włączyć usług domenowych AD Azure w klasycznym Azure wirtualnej sieci.

- Azure AD domeny usługi **nie można go włączyć wirtualnych sieci utworzony przy użyciu Menedżera zasobów Azure**.

- Menedżer zasobów wirtualnych sieci można nawiązać klasyczny wirtualną sieć włączeniu Azure usług domenowych AD. Następnie można usług domenowych AD Azure w wirtualnej sieci oparte na Menedżera zasobów. Aby uzyskać więcej informacji zobacz sekcję [łączności sieciowej](active-directory-ds-networking.md#network-connectivity) .

- **Regionalne wirtualnych sieci**: Jeśli zamierzasz użyć istniejącej sieci wirtualnej zapewnienia regionalne wirtualnej sieci.

    - Wirtualnych sieci korzystające z mechanizmu starsze koligacji grup nie można używać z usługami Azure AD domeny.

    - Aby używać usług domenowych AD Azure, [Migrowanie starszych sieci wirtualne regionalnych wirtualnych sieci](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).


### <a name="azure-region-for-the-virtual-network"></a>Azure region wirtualnej sieci

- Wdrożenie zarządzanych domeny w tym samym regionie Azure jako wirtualną sieć usług Azure AD domeny można włączyć usługę w.

- Zaznacz wirtualnej sieci w regionie Azure obsługiwane w usługach domenowych AD Azure.

- Odwiedź stronę [usług Azure według regionów](https://azure.microsoft.com/regions/#services/) wiedzieć Azure regiony, w których Azure AD Domain Services jest dostępny.


### <a name="requirements-for-the-virtual-network"></a>Wymagania dotyczące wirtualnej sieci

- **Odległość do usługi Azure obciążenia**: Wybierz wirtualna sieć, którą obecnie obsługuje/będzie przechowywana maszyn wirtualnych, które muszą uzyskiwać dostęp do usług domenowych AD Azure.

- **Serwery DNS niestandardowy i wyświetlić swój własne**: Upewnij się, że są nie niestandardowych serwery DNS skonfigurowane dla wirtualnej sieci.

- **Istniejące domeny z tą samą nazwą domeny**: Upewnij się, że nie masz istniejącej domeny z tą samą nazwą domeny, dostępne w tym wirtualnej sieci. Na przykład przyjęto założenie, że masz domenie o nazwie "contoso.com" już dostępne w zaznaczonej wirtualnej sieci. Później podczas próby Włączanie usług domenowych AD Azure zarządzanych domeny z tą samą nazwą domeny (który jest "contoso.com") w tym wirtualnej sieci. Wystąpienia awarii podczas próby włączyć usługi Azure AD domeny. Ten błąd jest z powodu konfliktów nazw dla nazwy domeny, w tym wirtualnej sieci. W tej sytuacji należy użyć pod inną nazwą konfigurowania domeny zarządzanych usług domenowych AD Azure. Alternatywnie możesz Anuluj obsługi administracyjnej istniejącej domeny i Kontynuuj, aby włączyć usługi Azure AD domeny.

> [AZURE.WARNING] Po włączeniu usługi, nie można przenieść usług domenowych z inną siecią wirtualną.


## <a name="network-security-groups-and-subnet-design"></a>Grupy zabezpieczeń sieci i podsieci projektu
[Grupa zabezpieczeń sieci (NSG)](../virtual-network/virtual-networks-nsg.md) zawiera listę reguł listy kontroli dostępu (ACL), które umożliwiają lub odmówić ruch sieciowy usługi wystąpieniach maszyn wirtualnych w wirtualnej sieci. NSGs może być skojarzone z podsieci lub poszczególne wystąpienia maszyn wirtualnych w tej podsieci. Gdy NSG jest skojarzony z podsieci, reguły ACL dotyczą wszystkich wystąpień maszyn wirtualnych w tej podsieci. Ponadto można ograniczyć ruch do poszczególnych maszyn wirtualnych dalsze kojarząc NSG bezpośrednio do tego maszyn wirtualnych.

![Zalecane podsieci](./media/active-directory-domain-services-design-guide/vnet-subnet-design.png)


### <a name="best-practices-for-choosing-a-subnet"></a>Najważniejsze wskazówki dotyczące wybierania podsieci
- Wdrażanie usług domenowych AD Azure **osobną podsieć dedykowane** Azure wirtualnej sieci.

- Nie dotyczą NSGs dedykowane podsieć zarządzaną domeny. Jeśli NSGs należy zastosować do dedykowane podsieci, zapewnić, że **nie blokują porty wymagane do obsługi i zarządzania domeną**.

- Nie nadmiernie ograniczyć liczbę adresów IP dostępnych w dedykowanej podsieci zarządzanych domeny. To ograniczenie zapobiega usługę Udostępnianie dwa kontrolery domen dla swojej domeny zarządzane.

- **Nie należy włączać usługach domenowych AD Azure w podsieci brama** wirtualnej sieci.


> [AZURE.WARNING] Gdy powiążesz NSG podsieci, w której usługi Azure AD domeny jest włączona, może zakłócać firmy Microsoft możliwość obsługi i Zarządzanie domeną. Ponadto synchronizacji między dzierżawy usługi Azure AD i zarządzane domeny zostanie zerwane. **Umowa dotycząca poziomu usług nie dotyczą wdrożeń miejsce, w którym NSG zastosowano która blokuje usług domenowych AD Azure z aktualizacji i zarządzania domeną.**


### <a name="ports-required-for-azure-ad-domain-services"></a>Porty wymagane dla usług domenowych AD Azure
Następujące porty są wymagane dla usług Azure AD domeny do usługi i zachować zarządzanych domeny. Upewnij się, porty te nie zostaną zablokowane dla podsieci, w której są włączone zarządzanych domeny.

| Numer portu | Cel |
|---|---|
| 443 | Synchronizacja z dzierżawy usługi Azure AD |
| 3389 | Zarządzanie domeny |
| 5986 | Zarządzanie domeny |
| 636 | Bezpieczny dostęp LDAP (LDAPS) z domeną zarządzanych |



## <a name="network-connectivity"></a>Łączność sieciowa
Azure AD domenie usług domenowych zarządzanych mogą być włączone tylko w jednym klasyczny wirtualnej sieci platformy Azure. Wirtualnych sieci utworzony przy użyciu Menedżera zasobów Azure nie są obsługiwane.


### <a name="scenarios-for-connecting-azure-networks"></a>Scenariusze dotyczące łączenia sieci Azure
Łączenie Azure wirtualnych sieci korzystać z domeny zarządzanych w jednym z następujących scenariuszy wdrażania:

#### <a name="use-the-managed-domain-in-more-than-one-azure-classic-virtual-network"></a>Używanie domeny zarządzanych w więcej niż jeden Azure klasyczny wirtualnej sieci
Inne Azure klasyczny wirtualnych sieci można nawiązać Azure klasyczny wirtualną sieć w którym są włączone usługi Azure AD domeny. To połączenie VPN umożliwia zarządzanych domeny za pomocą usługi obciążenia wdrożony w innych sieciach wirtualnych.

![Łączność sieciowa wirtualnych klasyczny](./media/active-directory-domain-services-design-guide/classic-vnet-connectivity.png)

#### <a name="use-the-managed-domain-in-a-resource-manager-based-virtual-network"></a>Używanie zarządzanych domeny w sieci wirtualnych Menedżera zasobów
Menedżer zasobów wirtualnych sieci można nawiązać Azure klasyczny wirtualną sieć w którym są włączone usługi Azure AD domeny. To połączenie umożliwia zarządzanych domeny za pomocą usługi obciążenia wdrożony w wirtualnej sieci oparte na Menedżera zasobów.

![Menedżer zasobów łączności klasyczny wirtualnej sieci](./media/active-directory-domain-services-design-guide/classic-arm-vnet-connectivity.png)


### <a name="network-connection-options"></a>Opcje połączeń sieciowych

- **Połączeń VNet do VNet przy użyciu połączeń sieci VPN witryny do witryny**: nawiązywanie połączenia wirtualnej sieci do innej sieci wirtualnych (VNet-VNet) jest podobne do połączenia wirtualnej sieci lokalnej lokalizacja witryny. Oba typy łączności za pomocą bramy sieci VPN o podanie kanału bezpiecznego przy użyciu IPsec i IKE.

    ![Łączność wirtualnej sieci przy użyciu sieci VPN bramy](./media/active-directory-domain-services-design-guide/vnet-connection-vpn-gateway.jpg)

    [Więcej informacji — połączenia wirtualnej sieci przy użyciu sieci VPN bramy](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)


- **Połączeń VNet do VNet przy użyciu wirtualnej sieci zaglądanie**: zaglądanie wirtualnej sieci jest mechanizm, który łączy dwie wirtualnej sieci, w tym samym regionie za pośrednictwem sieci szkieletowego Azure. Po peered, dwa wirtualnych sieci pojawia się na celach łączności. Nadal zarządza nimi jako osobne zasoby, ale maszyn wirtualnych w tych wirtualnych sieci można komunikować się ze sobą bezpośrednio, stosując prywatnych adresów IP.

    ![Łączność wirtualnej sieci przy użyciu zaglądanie](./media/active-directory-domain-services-design-guide/vnet-peering.png)

    [Więcej informacji — wirtualnej sieci zaglądanie](../virtual-network/virtual-network-peering-overview.md)



<br>

## <a name="related-content"></a>Zawartość pokrewna

- [Zaglądanie Azure wirtualnej sieci](../virtual-network/virtual-network-peering-overview.md)

- [Konfigurowanie połączenia VNet do VNet modelu Klasyczny wdrażania](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)

- [Grupy zabezpieczeń sieci Azure](../virtual-network/virtual-networks-nsg.md)
