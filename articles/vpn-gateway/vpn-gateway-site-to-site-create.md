<properties
   pageTitle="Tworzenie wirtualnych sieci przy użyciu połączenie VPN brama witryny do witryny za pomocą portalu klasyczny Azure | Microsoft Azure"
   description="Tworzenie VNet z połączeniem Brama VPN S2S dla lokalnego krzyżowe i konfiguracji hybrydowych przy użyciu modelu Klasyczny wdrożenia."
   services="vpn-gateway"
   documentationCenter=""
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="create-a-vnet-with-a-site-to-site-connection-using-the-azure-classic-portal"></a>Tworzenie VNet z połączeniem witryny do witryny za pomocą portalu klasyczny Azure

> [AZURE.SELECTOR]
- [Menedżer zasobów — Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Menedżer zasobów — programu PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
- [Klasyczny — klasyczny portalu](vpn-gateway-site-to-site-create.md)

W tym artykule opisano tworzenie wirtualnych sieci i połączenie bramy VPN witryny do witryny sieci lokalnej przy użyciu modelu Klasyczny wdrożenia i klasyczny portalu. Połączenia witryny do witryny może być używany do krzyżowe lokalnego i hybrydowego konfiguracji.

![Diagram witryny do witryny] (./media/vpn-gateway-site-to-site-create/site2site.png "witryny do witryny")


### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Modele wdrożenia i metod połączenia witryny do witryny

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

W poniższej tabeli przedstawiono modeli jest obecnie dostępna wdrażania i metody konfiguracji witryny do witryny. Po udostępnieniu artykułu z kroków konfiguracji możemy łącze do niego bezpośrednio z tej tabeli.

[AZURE.INCLUDE [vpn-gateway-table-site-to-site-table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Dodatkowo 

Jeśli chcesz się połączyć ze sobą VNets, zobacz [Konfigurowanie połączenia VNet do VNet modelu Klasyczny wdrożenia](virtual-networks-configure-vnet-to-vnet-connection.md). Jeśli chcesz dodać połączenie witryny do witryny do VNet, który ma już połączenia, zobacz [Dodawanie połączenie S2S VNet z istniejącego połączenia VPN bramy](vpn-gateway-multi-site.md).
 
## <a name="before-you-begin"></a>Przed rozpoczęciem

Sprawdź, czy masz następujące elementy przed rozpoczęciem konfiguracji.

- Zgodne urządzenie VPN i osoby, która jest możliwość konfigurowania go. Zobacz [informacje o urządzeniach sieci VPN](vpn-gateway-about-vpn-devices.md). Nie znają programu Konfigurowanie urządzenia VPN, czy nie zna adres IP, który zakresów znajduje się w sieci lokalnej konfiguracja sieci, musisz będą dopasowane do osoby, która zapewnia te szczegóły dla Ciebie.

- Zewnętrznie przeciwległych publiczny adres IP dla danego urządzenia VPN. Ten adres IP nie może znajdować się za translatora adresów sieciowych.

- Subskrypcję usługi Azure. Jeśli nie masz jeszcze subskrypcji usługi Azure, można aktywować swoje [korzyści subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) lub Utwórz konto [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial/).


## <a name="CreateVNet"></a>Tworzenie wirtualnych sieci

1. Zaloguj się do [portalu klasyczny Azure](https://manage.windowsazure.com/).

2. W lewym dolnym rogu ekranu kliknij przycisk **Nowy**. W okienku nawigacji kliknij pozycję **Usługi sieciowe**, a następnie kliknij **Wirtualnej sieci**. Kliknij przycisk **Utwórz niestandardowy** , aby uruchomić Kreatora konfiguracji.

3. Aby utworzyć usługi VNet, wprowadź ustawienia konfiguracji na następujących stronach:

## <a name="Details"></a>Strona Szczegóły wirtualnej sieci

Wprowadź następujące informacje:

- **Nazwa**: Nazwa wirtualnej sieci. Na przykład *EastUSVNet*. Po wdrożeniu usługi wystąpienia maszyny wirtualne i PaaS, więc nie możesz nawiązać nazwę zbyt skomplikowane, będziesz używać tej nazwy wirtualnej sieci.
- **Lokalizacja**: lokalizacji dotyczy bezpośrednio do lokalizacji fizycznej (region), w którym chcesz zasobów pośrednictwem (SMS) do znajdują się. Na przykład jeśli chcesz maszyny wirtualne, które wdrażanie z tą siecią wirtualną się znajdować się w USA *Wschodniego*, zaznacz tej lokalizacji. Nie można zmienić regionu skojarzone z siecią wirtualną po jego utworzeniu.

## <a name="DNS"></a>Serwery DNS i strony połączenie VPN

Wprowadź następujące informacje, a następnie kliknij strzałkę następnego w prawej dolnej.

- **Serwery DNS**: Wprowadź nazwę serwera DNS i adres IP lub wybierz serwer DNS wcześniej zarejestrowanych w menu skrótów. To ustawienie nie tworzy serwera DNS. Umożliwia określenie serwerów DNS, które ma być używany do rozpoznawania nazw dla tej wirtualnej sieci.
- **Konfigurowanie sieci VPN witryny do witryny**: zaznacz pole wyboru dla **Konfigurowanie sieci VPN witryny do witryny**.
- **Sieci lokalnej**: sieci lokalnej reprezentuje swojej lokalizacji fizycznej lokalnego. Możesz wybrać sieci lokalnej, które zostało już wcześniej utworzone lub utworzyć sieci lokalnej. Jednak wybranie za pomocą sieci lokalnej uprzednio utworzony, przejdź do strony Konfiguracja **Sieci lokalnych** i sprawdź, czy adres IP urządzenia VPN (publiczny adres IP protokołu IPv4 podwójne) urządzenia VPN jest dokładne.

## <a name="Connectivity"></a>Strona łączności witryny do witryny

Jeśli tworzysz nowy sieci lokalnej pojawi się na stronie **Witryny do witryny łączności** . Jeśli chcesz użyć sieci lokalnej uprzednio utworzony, ta strona nie pojawią się w kreatorze i na można przenosić do następnej sekcji.

Wprowadź następujące informacje, a następnie kliknij strzałkę w następnym.

-   **Nazwa**: nazwa, którym chcesz nawiązać połączenie lokalne (lokalnego) sieci witryny.
-   **Adres IP urządzenia VPN**: przeciwległych publiczny adres IP protokołu IPv4 urządzenia VPN lokalnego, którego używasz, aby nawiązać połączenie Azure. Urządzenia VPN nie może znajdować się za translatora adresów sieciowych.
-   **Przestrzeń adresów**: obejmują uruchamianie IP i CIDR (liczba adresów). Możesz określić adres range(s) tego mają być wysyłane przez bramę wirtualną sieć w lokalizacji usługi lokalne lokalnego. Jeśli docelowy adres IP mieści się w żądane zakresy określone w tym miejscu, odbywa się za pośrednictwem wirtualnych sieci bramy.
-   **Dodaj adres miejsca**: Jeśli masz wiele zakresów adresów, które mają być wysyłane przez bramę wirtualnej sieci, określ zakres każdego dodatkowego adresu. Można dodać lub usunąć zakresy później na stronie **Sieci lokalnej** .

## <a name="Address"></a>Wirtualna sieć adres spacje strony

Określ zakres adres, którego chcesz użyć na potrzeby wirtualnej sieci. Są to dynamiczne adresy IP (SPADKU), które są przypisywane do maszyny wirtualne i inne wystąpienia roli, które wdrażanie z tą siecią wirtualną.

Jest szczególnie ważne zaznaczyć zakres, aby nie nakładały się z dowolnego z zakresów, które są używane w sieci lokalnej. Musisz będą dopasowane do administratora sieci. Administrator sieci może być konieczne dotycząca szczególnych się zakres adresów IP z obszaru adres sieci lokalnej należy użyć na potrzeby wirtualnej sieci.

Wprowadź następujące informacje, a następnie kliknij znacznik wyboru w prawym dolnym, aby skonfigurować sieć.

- **Przestrzeń adresów**: obejmują uruchamianie IP i liczba adresów. Upewnij się, że wybrane obszary adresów nie zachodzi obszary adresów znajdujących się w tej samej sieci lokalnej.
- **Dodaj podsieci**: obejmują uruchamianie IP i liczba adresów. Dodatkowe podsieci nie są wymagane, ale warto utworzyć osobną podsieć dla maszyny wirtualne zawierające SPADKU statyczne. Możesz też w podsieci, która różni się od innych wystąpień roli jest pośrednictwem usługi SMS.
- **Dodaj bramy podsieci**: kliknij, aby dodać podsieci bramy. Podsieci bramy jest używana tylko do bramy wirtualnej sieci i jest wymagany dla tej konfiguracji.

Kliknij znacznik wyboru u dołu strony i rozpocznie Tworzenie wirtualnych sieci. Po zakończeniu instalacji zostanie wyświetlona **utworzono** wyświetlana w obszarze **stanu** na stronie **sieci** w portalu klasyczny Azure. Po utworzeniu VNet następnie można skonfigurować Centrum wirtualnej sieci.

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)] 

## <a name="VNetGateway"></a>Brama wirtualnej sieci

Konfigurowanie bramy wirtualnej sieci w celu utworzenia bezpiecznego połączenia witryny do witryny. Zobacz [Konfigurowanie bramy wirtualną sieć w portalu klasyczny Azure](vpn-gateway-configure-vpn-gateway-mp.md).

## <a name="next-steps"></a>Następne kroki

Po zakończeniu połączenia można dodać do sieci wirtualnej maszyn wirtualnych. Zapoznaj się z dokumentacją [maszyn wirtualnych](https://azure.microsoft.com/documentation/services/virtual-machines/) , aby uzyskać więcej informacji.
