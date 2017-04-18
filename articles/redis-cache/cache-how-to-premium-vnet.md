<properties 
    pageTitle="Jak skonfigurować wirtualną sieć obsługę bufor Redis Azure Premium | Microsoft Azure" 
    description="Dowiedz się, jak tworzyć i zarządzać nimi wirtualnej sieci pomocy technicznej dla usługi Azure Redis w pamięci podręcznej wystąpień warstwa Premium" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="how-to-configure-virtual-network-support-for-a-premium-azure-redis-cache"></a>Jak skonfigurować wirtualnej sieci pomocy technicznej dla bufor Redis Azure Premium
Azure Redis pamięci podręcznej ma ofertą różnych pamięci podręcznej, które zapewniają elastyczność w wyborze rozmiar pamięci podręcznej i funkcji, takich jak nowy poziom Premium.

Funkcje warstwa premium Azure Redis w pamięci podręcznej obejmują, klaster utrzymywanie się i wsparcie wirtualnej sieci (VNet). VNet jest sieć prywatną w chmurze. Po skonfigurowaniu wystąpienie pamięci podręcznej Azure Redis VNet, jest publicznie adresach i są dostępne tylko w środowisku maszyn wirtualnych systemu i aplikacjach pakietu VNet. Ten artykuł zawiera opis sposobu konfigurowania wirtualnej sieci pomocy technicznej w przypadku wystąpienia Azure Redis w pamięci podręcznej premium.

>[AZURE.NOTE] Azure Redis pamięci podręcznej obsługuje zarówno klasyczny i ARM VNets.

Informacji o innych funkcjach pamięci podręcznej premium zobacz [Wprowadzenie do poziomu Azure Redis pamięci podręcznej Premium](cache-premium-tier-intro.md).

## <a name="why-vnet"></a>Dlaczego VNet?
Wdrożenie [Azure wirtualnej sieci (VNet)](https://azure.microsoft.com/services/virtual-network/) zawiera ulepszone zabezpieczenia i izolacji dla usługi Azure Redis pamięci podręcznej, a także podsieci, zasad kontroli dostępu i inne funkcje, dodatkowo ograniczać dostęp do Azure Redis w pamięci podręcznej.

## <a name="virtual-network-support"></a>Obsługa wirtualnej sieci
Obsługa wirtualnych sieci (VNet) jest skonfigurowana na karta **Nową pamięć podręczną Redis** podczas tworzenia pamięci podręcznej. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Po wybraniu premium ceny warstwy, można skonfigurować integracji Azure Redis VNet pamięci podręcznej, wybierając pozycję VNet, który jest w tej samej lokalizacji, w pamięci podręcznej i subskrypcji. Aby użyć nowego VNet, najpierw utworzyć, wykonując czynności opisane w [Tworzenie wirtualnych sieci przy użyciu Azure portal](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) lub [Tworzenie wirtualnych sieci (klasyczny) za pomocą portalu Azure](../virtual-network/virtual-networks-create-vnet-classic-portal.md) i powrócić do karta **Nową pamięć podręczną Redis** , aby utworzyć i skonfigurować pamięć podręczną premium.

Aby skonfigurować VNet dla swojego nową pamięć podręczną, kliknij **Wirtualną sieć** na karta **Nową pamięć podręczną Redis** , a z listy rozwijanej wybierz żądany VNet.

![Wirtualna sieć][redis-cache-vnet]

Wybierz odpowiednie podsieci z listy rozwijanej **podsieci** , a następnie określ odpowiednie **statyczny adres IP**. Jeśli korzystasz z klasycznego VNet pole **statyczny adres IP** jest opcjonalne, a jeśli nie określono wybrano jedną z zaznaczonej podsieci.

>[AZURE.IMPORTANT] Podczas wdrażania pamięci podręcznej Azure Redis VNet ARM, pamięci podręcznej musi być w dedykowanej podsieci, która nie zawiera żadnych innych zasobów, z wyjątkiem wystąpienia Azure Redis w pamięci podręcznej. Jeśli próby wdrażanie pamięci podręcznej Azure Redis VNet ARM do podsieci zawierającego inne zasoby, wdrażanie kończy się niepowodzeniem.

![Wirtualna sieć][redis-cache-vnet-ip]

>[AZURE.IMPORTANT] Pierwsze cztery adresy w podsieci są zarezerwowane i nie mogą być używane. Aby uzyskać więcej informacji, zobacz [Czy istnieją jakiekolwiek ograniczenia dotyczące używania adresów IP w tych podsieci?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)

Po utworzeniu pamięci podręcznej, możesz wyświetlić konfigurację VNet, klikając pozycję **Wirtualnej sieci** z karta **Ustawienia** .

![Wirtualna sieć][redis-cache-vnet-info]


Aby połączyć się wystąpienia pamięci podręcznej Azure Redis, korzystając z VNet, określ nazwę hosta pamięć podręczną w parametrach połączenia, jak pokazano w poniższym przykładzie.

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5premium.redis.cache.windows.net,abortConnect=false,ssl=true,password=password");
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

## <a name="azure-redis-cache-vnet-faq"></a>Azure Redis pamięci podręcznej VNet — często zadawane pytania

Poniższa lista zawiera odpowiedzi na często zadawane pytania dotyczące skalowania Azure Redis w pamięci podręcznej.

-   [Jakie są niektóre typowe problemy konfiguracji z Azure Redis w pamięci podręcznej i VNets?](#what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets)
-   [Czy mogę korzystać z pamięci podręcznej standardowy lub podstawowe VNets?](#can-i-use-vnets-with-a-standard-or-basic-cache)
-   [Dlaczego tworzenia pamięci podręcznej Redis nie powiodło się w niektórych podsieci, ale nie inne?](#why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others)
-   [Wszystkie funkcje pamięci podręcznej działają podczas hostingu pamięci podręcznej w VNET?](#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)


## <a name="what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets"></a>Jakie są niektóre typowe problemy konfiguracji z Azure Redis w pamięci podręcznej i VNets?

Kiedy Azure Redis w pamięci podręcznej znajduje się w VNet, są używane porty w poniższej tabeli. Zablokowanie tych portów pamięci podręcznej może nie działać prawidłowo. Co najmniej jedną z tych portów zablokowane jest najczęściej problem konfiguracji podczas korzystania z pamięci podręcznej Redis Azure w VNet.

| Porty     | Kierunek        | Protokół transportowy | Cel                                                                           | Zdalny adres IP                           |
|-------------|------------------|--------------------|-----------------------------------------------------------------------------------|-------------------------------------|
| porty 80, 443     | Wychodzące         | PORT TCP                | Redis zależności na Azure miejsca do magazynowania i infrastruktury kluczy publicznych (Internet)                                | *                                   |
| 53          | Wychodzące         | PORT TCP/UDP            | Redis zależności w DNS (Internet-VNet)                                         | *                                   |
| 6379, 6380  | Ruch przychodzący          | PORT TCP                | Komunikacja z klientem do Redis równoważenia obciążenia Azure                               | VIRTUAL_NETWORK, AZURE_LOADBALANCER |
| 8443        | Ruch przychodzący i wychodzący | PORT TCP                | Szczegóły implementacji dla Redis                                                   | VIRTUAL_NETWORK                     |
| 8500        | Ruch przychodzący          | PORT TCP/UDP            | Azure równoważenia obciążenia                                                              | AZURE_LOADBALANCER                  |
| 10221 10231 | Ruch przychodzący i wychodzący | PORT TCP                | Szczegóły implementacji dla Redis (mogą ograniczyć zdalny punkt końcowy do VIRTUAL_NETWORK) | VIRTUAL_NETWORK, AZURE_LOADBALANCER |
| 13000 13999 | Ruch przychodzący          | PORT TCP                | Komunikacja z klientem do Redis klastrów równoważenia obciążenia Azure                      | VIRTUAL_NETWORK, AZURE_LOADBALANCER |
| 15000 15999 | Ruch przychodzący          | PORT TCP                | Komunikacja z klientem do Redis klastrów równoważenia obciążenia Azure                      | VIRTUAL_NETWORK, AZURE_LOADBALANCER |
| 16001       | Ruch przychodzący          | PORT TCP/UDP            | Azure równoważenia obciążenia                                                              | AZURE_LOADBALANCER                  |
| 20226       | Ruch przychodzący + ruchu wychodzącego | PORT TCP                | Szczegóły implementacji dla klastrów Redis                                          | VIRTUAL_NETWORK                     |


Istnieją wymagania łączności sieci dla Azure Redis pamięci podręcznej, które mogą nie być początkowo spełnione wirtualnej sieci. Azure Redis pamięci podręcznej wymaga wszystkich następujących elementów w celu poprawnego używane w wirtualnej sieci.

-  Łączność sieciowa ruchu wychodzącego do punktów końcowych magazyn Azure na całym świecie. Ta opcja uwzględnia punkty końcowe znajduje się w tym samym regionie, gdy wystąpienie Azure Redis w pamięci podręcznej, a także punkty końcowe miejsca do magazynowania znajduje się w **innych** regionach Azure. Azure miejsca do magazynowania punkty końcowe rozwiązania w obszarze następujące domeny DNS: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net*i *file.core.windows.net*. 
-  Łączność sieciowa ruchu wychodzącego *ocsp.msocsp.com*, *mscrl.microsoft.com* i *crl.microsoft.com*. Ten łączności jest wymagane do obsługi funkcji SSL.
-  Konfiguracja DNS wirtualną sieć musi być ułatwiających rozwiązanie wszystkie punkty końcowe i domeny wymienionych w punktach wcześniejszych. Te wymagania DNS można uzyskać w celu zapewnienia prawidłowych infrastruktury DNS jest skonfigurowane i obsługiwane dla wirtualnej sieci.



### <a name="can-i-use-vnets-with-a-standard-or-basic-cache"></a>Czy mogę korzystać z pamięci podręcznej standardowy lub podstawowe VNets?

VNets można używać tylko z pamięci podręcznej premium.

### <a name="why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others"></a>Dlaczego tworzenia pamięci podręcznej Redis nie powiodło się w niektórych podsieci, ale nie inne?

W przypadku rozmieszczania pamięci podręcznej Azure Redis do VNet ARM, pamięci podręcznej musi być w dedykowanej podsieci, która zawiera inny typ zasobu. Jeśli próby wdrażanie pamięci podręcznej Azure Redis podsieci ARM VNet zawierającego inne zasoby, wdrażanie kończy się niepowodzeniem. Musisz usunąć istniejących zasobów wewnątrz podsieci, aby można było utworzyć nową pamięć podręczną Redis.

Wiele typów zasobów można wdrażać na klasyczny VNet, ile masz za mało dostępnych adresów IP.

### <a name="do-all-cache-features-work-when-hosting-a-cache-in-a-vnet"></a>Wszystkie funkcje pamięci podręcznej działają podczas hostingu pamięci podręcznej w VNET?

Jeśli pamięć podręczną jest częścią VNET, tylko klientów w VNET dostęp do pamięci podręcznej. W wyniku następujące funkcje zarządzania pamięci podręcznej nie działają w tej chwili.

-   Redis konsoli — ponieważ Redis konsoli korzysta z klienta redis cli.exe hostowana w maszyny wirtualne, które nie są częścią usługi VNET, nie może nawiązać połączenia z pamięci podręcznej.


## <a name="use-expressroute-with-azure-redis-cache"></a>Korzystanie z pamięci podręcznej Azure Redis ExpressRoute

Klienci mogą się łączyć obwód [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) infrastruktury wirtualnej sieci, a więc rozszerzenia sieci lokalnej Azure. 

Domyślnie nowo utworzone elektrycznego ExpressRoute anonsuje domyślną trasę, która umożliwia wychodzące połączenie z Internetem. W tej konfiguracji aplikacje klienckie są w stanie nawiązać połączenia z innymi Azure punkty końcowe w tym Azure Redis w pamięci podręcznej.

Jednak typowych konfiguracji klienta jest definiowanie własnych trasy domyślnej (0.0.0.0/0), który wymusza wychodzący ruch internetowy zamiast przepływ lokalnego. Ten ruch niezmiennie podziały łączność z Azure Redis w pamięci podręcznej, ponieważ ruchu wychodzącego jest zablokowane obsługiwanych lokalnie lub translatora adresów Sieciowych czy nierozpoznanym zbiór adresy, które nie są już współpracują z różnych Azure punkty końcowe.

Rozwiązanie jest określenie jednej (lub więcej) przekierowuje zdefiniowane przez użytkownika (UDRs) na podsieci, która zawiera pamięci podręcznej Redis Azure. UDR Określa trasy specyficzne dla podsieci, które będą uznane zamiast trasy domyślnej.

Jeśli to możliwe zaleca się następującej konfiguracji:

- Konfiguracja ExpressRoute anonsuje 0.0.0.0/0 i domyślnie życie tuneli wszystkich ruchu wychodzącego lokalnego.
- UDR zastosowane do podsieci zawierającej pamięci podręcznej Redis Azure określa 0.0.0.0/0 następnego przeskoku typu Internet (na przykład jest dostępne w dół, w tym artykule).

Scalonej tej procedury powoduje, że poziomie podsieci UDR ma pierwszeństwo przed ExpressRoute wymuszone tunelowanie zapewnić w ten sposób ruchu wychodzącego dostęp do Internetu z pamięci podręcznej Redis Azure.

Mimo że nawiązywanie połączenia z wystąpieniem Azure Redis w pamięci podręcznej ze źródeł lokalnych aplikacji przy użyciu ExpressRoute nie scenariusz typowe zastosowania z powodu powodów, dla których wydajności (Aby uzyskać optymalną wydajność Azure Redis pamięci podręcznej klientów powinny być w tym samym regionie jako pamięci podręcznej Redis Azure), w tym scenariuszu, które ścieżki ruchu wychodzącego sieciowej nie można przechodzić przez wewnętrznych firmowej serwery proxy, podobnie jak może być życie tunelowane do lokalnego. Spowoduje to zmianę skutecznych adresów NAT wychodzącego ruchu sieciowego z pamięci podręcznej Redis Azure. Zmiana adresu translatora adresów Sieciowych pamięci podręcznej Azure Redis wychodzącego ruchu sieciowego w wystąpieniu powoduje błędów połączenia do wielu punkty końcowe wymienionych powyżej. Powoduje niepomyślne tworzenia Azure Redis w pamięci podręcznej.

**Ważne:**  Przekierowuje zdefiniowane w UDR, **musi** spełniać określone wystarczająco pierwszeństwo dowolnego przekierowuje ogłaszane w konfiguracji ExpressRoute. Poniższy przykład używa 0.0.0.0/0 szeroki zakres adresów i jako takie mogą potencjalnie być przypadkowo zastąpione reklam rozsyłania za pomocą bardziej szczegółowych zakresy adresów.

**Ważne:**  Azure Redis pamięci podręcznej nie jest obsługiwane w przypadku konfiguracji ExpressRoute ten **niepoprawnie między ogłaszanie ścieżki z publicznej peering ścieżkę dostępu do prywatnych ścieżkę peering**. Konfiguracjach ExpressRoute publicznej zaglądanie skonfigurowane, otrzymają reklam rozsyłania od firmy Microsoft dla dużego zestawu zakresy adresów IP Azure firmy Microsoft. Jeśli te zakresy adresów są niepoprawnie przy ogłaszane krzyżowe na ścieżce peering prywatne, w wyniku tego są wszystkie pakiety wychodzącego z podsieci wystąpienie Azure Redis w pamięci podręcznej niepoprawnie przy tunelowane życie w infrastrukturze sieciowej lokalnego klienta. Ten przepływ sieci podziały Azure Redis w pamięci podręcznej. Rozwiązanie tego problemu jest zatrzymanie ścieżki reklamami krzyżowe z publicznej peering ścieżka prywatne ścieżkę peering.

Informacje na przekierowuje zdefiniowane przez użytkownika jest dostępna w [Przegląd](../virtual-network/virtual-networks-udr-overview.md). 

Aby uzyskać więcej informacji o ExpressRoute zobacz [Omówienie kwestii technicznych ExpressRoute](../expressroute/expressroute-introduction.md)

## <a name="next-steps"></a>Następne kroki
Dowiedz się, jak korzystać z większej liczby funkcji pamięci podręcznej premium.

-   [Wprowadzenie do poziomu Azure Redis pamięci podręcznej Premium](cache-premium-tier-intro.md)





  
<!-- IMAGES -->

[redis-cache-vnet]: ./media/cache-how-to-premium-vnet/redis-cache-vnet.png

[redis-cache-vnet-ip]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-ip.png

[redis-cache-vnet-info]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-info.png

