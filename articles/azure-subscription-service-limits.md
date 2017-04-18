<properties
    pageTitle="Subskrypcja Microsoft Azure i limity dotyczące usługi, przydziałów i ograniczeń"
    description="Lista typowych subskrypcji Azure i limity dotyczące usługi, przydziałów i ograniczeń. Dotyczy to informacji na temat zwiększania limity wraz z wartościami maksymalna."
    services=""
    documentationCenter=""
    authors="rothja"
    manager="jeffreyg"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="btardif"/>

# <a name="azure-subscription-and-service-limits-quotas-and-constraints"></a>Azure subskrypcji i limity dotyczące usługi, przydziałów i ograniczeń

## <a name="overview"></a>Omówienie

Ten dokument określa niektóre z najczęściej używanych limity Microsoft Azure. Nie obecnie dotyczy wszystkich usług Azure. Czasem limity te będą rozwinięta i została zaktualizowana do wersji obejmuje więcej platformy.

Odwiedź stronę [Omówienie ceny Azure](https://azure.microsoft.com/pricing/) , aby dowiedzieć się więcej o Azure ceny. Można oszacować koszty za pomocą [Kalkulatora ceny](https://azure.microsoft.com/pricing/calculator/) lub cennik strony szczegółów usługi (na przykład [Maszyny wirtualne systemu Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)).

> [AZURE.NOTE] Jeśli chcesz podwyższyć limit powyżej **Domyślny Limit**, możesz [otworzyć żądanie pomocy technicznej online klienta bezpłatnie](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). Nie można zwiększyć limity powyżej wartości **Maksymalnej** w poniższych tabelach. W przypadku kolumn **Maksymalny Limit** określony zasób nie ma ograniczenia dopasowywania.

## <a name="limits-and-the-azure-resource-manager"></a>Limity i Azure Menedżera zasobów

Jest teraz można połączyć wiele zasobów Azure w jednej grupie zasobów Azure. Podczas korzystania z grup zasobów, ograniczenia, które raz były globalnej stają się zarządzanych na poziomie regionalnym przy użyciu Menedżera zasobów Azure. Aby uzyskać więcej informacji dotyczących grup zasobów Azure zobacz [Omówienie Menedżera zasobów Azure](azure-resource-manager/resource-group-overview.md).

W obszarze poniżej ograniczenia dodano nową tabelę, aby odzwierciedlała różnice w limity, korzystając z Menedżera zasobów Azure. Na przykład istnieje tabela **Limity subskrypcji** i tabeli **Limity subskrypcji - Azure Menedżera zasobów** . Gdy ograniczenie dotyczy obu przypadkach, go jest wyświetlana tylko w pierwszej tabeli. Chyba że inaczej, limity są globalnej przez wszystkich regionów.

> [AZURE.NOTE] Należy wyróżnić przydziałów dla zasobów w grupach zasobów Azure są według regionów dostępne dla subskrypcji, a nie są na subskrypcji, jak przydziały usługi zarządzania. Jako przykład można użyć podstawowego przydziałów. W razie potrzeby żądanie zwiększona o przydział z obsługą rdzenie należy zdecydować, ile rdzenie chcesz użyć w regiony, a następnie wprowadź żądanie określonych dla przydziałów core Azure grupa zasobów dla kwot i regiony, które mają. W związku z tym, jeśli musisz użyć 30 rdzenie w Europie zachód na uruchomienie aplikacji; należy specjalnie wezwania 30 rdzenie w Europie Zachód. Ale nie będą mieć normę core wzrost innego regionu — tylko Zachodnia Europa ma przydział 30-core.
<!-- -->
W wyniku może być przydatne należy rozważyć, czy określanie, co przydziałami Azure grupa zasobów, muszą być dla swojego obciążenie pracą w dowolnym jednym regionie, a żądanie kwota w poszczególnych regionach, w której zamierzasz wdrożenia. Zobacz [Rozwiązywanie problemów z konfiguracją wdrożenia](./resource-manager-common-deployment-errors.md) , aby uzyskać więcej pomocy odkrycie Twojej bieżącej przydziałów dla określonych regionów.

## <a name="service-specific-limits"></a>Limity dotyczące usługi

- [Usługi Active Directory](#active-directory-limits)
- [Interfejs API zarządzania](#api-management-limits)
- [Aplikacji usługi](#app-service-limits)
- [Brama aplikacji](#application-gateway-limits)
- [Wnioski aplikacji](#application-insights-limits)
- [Automatyzacji](#automation-limits)
- [Azure Redis pamięci podręcznej](#azure-redis-cache-limits)
- [Funkcja RemoteApp Azure](#azure-remoteapp-limits)
- [Wykonywanie kopii zapasowych](#backup-limits)
- [Partii](#batch-limits)
- [BizTalk usług](#biztalk-services-limits)
- [SIECI CDN](#cdn-limits)
- [Usług w chmurze](#cloud-services-limits)
- [Factory danych](#data-factory-limits)
- [Analizy Lake danych](#data-lake-analytics-limits)
- [DNS](#dns-limits)
- [DocumentDB](#documentdb-limits)
- [Koncentratory zdarzenia](#event-hubs-limits)
- [Centrum IoT](#iot-hub-limits)
- [Kluczowe magazynu](#key-vault-limits)
- [Usługi multimediów](#media-services-limits)
- [Zaangażowania urządzeń przenośnych](#mobile-engagement-limits)
- [Usług mobilnych](#mobile-services-limits)
- [Monitorowanie](#monitoring-limits)
- [Uwierzytelnianie wieloskładnikowe](#multi-factor-authentication)
- [Sieci](#networking-limits)
- [Usługa powiadomień Centrum](#notification-hub-service-limits)
- [Wnioski operacyjne](#operational-insights-limits)
- [Grupa zasobów](#resource-group-limits)
- [Harmonogram](#scheduler-limits)
- [Wyszukiwanie](#search-limits)
- [Usługa Bus](#service-bus-limits)
- [Odzyskiwanie witryny](#site-recovery-limits)
- [Baza danych SQL](#sql-database-limits)
- [Miejsca do magazynowania](#storage-limits)
- [StorSimple System](#storsimple-system-limits)
- [Analizy strumieniu](#stream-analytics-limits)
- [Subskrypcji](#subscription-limits)
- [Menedżer ruchu](#traffic-manager-limits)
- [Maszyn wirtualnych](#virtual-machines-limits)
- [Zestawy skali maszyn wirtualnych](#virtual-machine-scale-sets-limits)


### <a name="subscription-limits"></a>Limity subskrypcji
#### <a name="subscription-limits"></a>Limity subskrypcji
[AZURE.INCLUDE [azure-subscription-limits](../includes/azure-subscription-limits.md)]

#### <a name="subscription-limits---azure-resource-manager"></a>Limity subskrypcji - Azure Menedżera zasobów

Następujące ograniczenia obowiązują w przypadku Azure Menedżera zasobów i grup zasobów Azure. Ograniczenia, które nie zmieniły się przy użyciu Menedżera zasobów Azure nie są widoczne poniżej. Zajrzyj do poprzedniej tabeli dla tych ograniczeń.

Aby dowiedzieć się, jak obsługa ograniczenia dotyczące żądania Menedżera zasobów zobacz [żądania ograniczania Menedżera zasobów](resource-manager-request-limits.md).

[AZURE.INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]


### <a name="resource-group-limits"></a>Limity grupa zasobów

[AZURE.INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]

### <a name="virtual-machines-limits"></a>Limity maszyn wirtualnych
#### <a name="virtual-machine-limits"></a>Limity maszyn wirtualnych
[AZURE.INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]


#### <a name="virtual-machines-limits---azure-resource-manager"></a>Limity maszyn wirtualnych - Azure Menedżera zasobów

Następujące ograniczenia obowiązują w przypadku Azure Menedżera zasobów i grup zasobów Azure. Ograniczenia, które nie zmieniły się przy użyciu Menedżera zasobów Azure nie są widoczne poniżej. Zajrzyj do poprzedniej tabeli dla tych ograniczeń.

[AZURE.INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### <a name="virtual-machine-scale-sets-limits"></a>Limity zestawów skali maszyn wirtualnych

[AZURE.INCLUDE [virtual-machine-scale-sets-limits](../includes/azure-virtual-machine-scale-sets-limits.md)]

### <a name="networking-limits"></a>Limity sieci

[AZURE.INCLUDE [expressroute-limits](../includes/expressroute-limits.md)]

#### <a name="networking-limits"></a>Limity sieci
[AZURE.INCLUDE [azure-virtual-network-limits](../includes/azure-virtual-network-limits.md)]

#### <a name="application-gateway-limits"></a>Limity bramy aplikacji

[AZURE.INCLUDE [application-gateway-limits](../includes/application-gateway-limits.md)]

#### <a name="traffic-manager-limits"></a>Limity Menedżer ruchu

[AZURE.INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### <a name="dns-limits"></a>Limity DNS

[AZURE.INCLUDE [dns-limits](../includes/dns-limits.md)]

### <a name="storage-limits"></a>Ograniczenia dotyczące magazynowania

Aby uzyskać dodatkowe informacje na limitów miejsca do magazynowania konta zobacz [skalowalność miejsca do magazynowania Azure i cele](../articles/storage/storage-scalability-targets.md).

#### <a name="storage-service-limits"></a>Ograniczenia dotyczące magazynowania usługi

[AZURE.INCLUDE [azure-storage-limits](../includes/azure-storage-limits.md)]

#### <a name="virtual-machine-disk-limits"></a>Limity dysku maszyn wirtualnych

[AZURE.INCLUDE [azure-storage-limits-vm-disks](../includes/azure-storage-limits-vm-disks.md)]

Aby uzyskać więcej informacji, zobacz temat [rozmiarów maszyn wirtualnych](../articles/virtual-machines/virtual-machines-linux-sizes.md) .

**Konta standardowego magazynu**

[AZURE.INCLUDE [azure-storage-limits-vm-disks-standard](../includes/azure-storage-limits-vm-disks-standard.md)]

**Kont miejsca do magazynowania Premium**

[AZURE.INCLUDE [azure-storage-limits-vm-disks-premium](../includes/azure-storage-limits-vm-disks-premium.md)]

#### <a name="storage-resource-provider-limits"></a>Ograniczenia dotyczące magazynowania dostawcy zasobów

[AZURE.INCLUDE [azure-storage-limits-azure-resource-manager](../includes/azure-storage-limits-azure-resource-manager.md)]


### <a name="cloud-services-limits"></a>Limity dotyczące usługi w chmurze

[AZURE.INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]


### <a name="app-service-limits"></a>Ograniczenia aplikacji usługi
Następujące ograniczenie aplikacji usługi obejmują limity dla aplikacji Web Apps, aplikacje Mobile aplikacje interfejsu API i logiki aplikacji.

[AZURE.INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### <a name="scheduler-limits"></a>Limity harmonogramu

[AZURE.INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### <a name="batch-limits"></a>Limity partii

[AZURE.INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]

###<a name="biztalk-services-limits"></a>Limity dotyczące usługi BizTalk
W poniższej tabeli przedstawiono limity dla usługi Azure Biztalk.

[AZURE.INCLUDE [biztalk-services-service-limits](../includes/biztalk-services-service-limits.md)]


### <a name="documentdb-limits"></a>Limity DocumentDB

[AZURE.INCLUDE [azure-documentdb-limits](../includes/azure-documentdb-limits.md)]

Przydziały na liście przy użyciu gwiazdki (*), [można dostosować, kontaktując się z Azure pomocy technicznej](./documentdb/documentdb-increase-limits.md).

### <a name="mobile-engagement-limits"></a>Limity zaangażowania Mobile

[AZURE.INCLUDE [azure-mobile-engagement-limits](../includes/azure-mobile-engagement-limits.md)]


### <a name="search-limits"></a>Limity wyszukiwania

Cennik poziomów określają wydajność i limity dotyczące usługi wyszukiwania. Poziomy obejmują:

- *Wolny* wielu dzierżawy usługi udostępnionych przez innych subskrybentów Azure, przeznaczone do oceny małych projektów i rozwoju.
- *Podstawowe* miejsce obliczeniowych w dedykowanej obciążenia produkcji w skali mniejszy, z maksymalnie trzy replikami dla obciążenia wysokiej dostępności kwerendy.
- *Standardowe (S1, S2, S3, S3 dużej gęstości)* jest większe obciążenia produkcji. Wielopoziomowe istnieją w warstwie standardowy, w którym można wybrać konfigurację zasobów dla określonych scenariuszach.

**Limity dla subskrypcji**

[AZURE.INCLUDE [azure-search-limits-per-subscription](../includes/azure-search-limits-per-subscription.md)]

**Limity dla danej usługi wyszukiwania**

[AZURE.INCLUDE [azure-search-limits-per-service](../includes/azure-search-limits-per-service.md)]

Aby uzyskać bardziej szczegółowe informacje inne limity rozmiaru dokumentu, kwerend na sekundę, klawiszy, zaproszenia i odpowiedzi, w tym zobacz [limity dotyczące usługi Azure wyszukiwania](search/search-limits-quotas-capacity.md).

### <a name="media-services-limits"></a>Limity dotyczące usługi multimediów

[AZURE.INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]

### <a name="cdn-limits"></a>Limity CDN

[AZURE.INCLUDE [cdn-limits](../includes/cdn-limits.md)]

### <a name="mobile-services-limits"></a>Limity dotyczące usługi Mobile

[AZURE.INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### <a name="monitoring-limits"></a>Limity monitorowania

[AZURE.INCLUDE [monitoring-limits](../includes/monitoring-limits.md)]

### <a name="notification-hub-service-limits"></a>Limity powiadomień Centrum usług

[AZURE.INCLUDE [notification-hub-limits](../includes/notification-hub-limits.md)]

### <a name="event-hubs-limits"></a>Limity koncentratory zdarzenia

[AZURE.INCLUDE [azure-servicebus-limits](../includes/event-hubs-limits.md)]

### <a name="service-bus-limits"></a>Limity dotyczące usługi Bus

[AZURE.INCLUDE [azure-servicebus-limits](../includes/service-bus-quotas-table.md)]

### <a name="iot-hub-limits"></a>Limity Centrum IoT

[AZURE.INCLUDE [azure-iothub-limits](../includes/iot-hub-limits.md)]

### <a name="data-factory-limits"></a>Limity Factory danych

[AZURE.INCLUDE [azure-data-factory-limits](../includes/azure-data-factory-limits.md)]

### <a name="data-lake-analytics-limits"></a>Limity Lake analizy danych
[AZURE.INCLUDE [azure-data-lake-analytics-limits](../includes/azure-data-lake-analytics-limits.md)]

### <a name="stream-analytics-limits"></a>Limity dotyczące analizy strumieniu
[AZURE.INCLUDE [stream-analytics-limits-table](../includes/stream-analytics-limits-table.md)]

### <a name="active-directory-limits"></a>Limity usługi Active Directory

[AZURE.INCLUDE [AAD-service-limits](../includes/active-directory-service-limits-include.md)]


### <a name="azure-remoteapp-limits"></a>Limity RemoteApp Azure

[AZURE.INCLUDE [azure-remoteapp-limits](../includes/azure-remoteapp-limits.md)]

### <a name="storsimple-system-limits"></a>Limity systemu StorSimple

[AZURE.INCLUDE [storsimple-limits-table](../includes/storsimple-limits-table.md)]


### <a name="operational-insights-limits"></a>Limity operacyjne wniosków

[AZURE.INCLUDE [operational-insights-limits](../includes/operational-insights-limits.md)]

### <a name="backup-limits"></a>Limity kopii zapasowej

[AZURE.INCLUDE [azure-backup-limits](../includes/azure-backup-limits.md)]

### <a name="site-recovery-limits"></a>Limity odzyskiwania witryny

[AZURE.INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### <a name="application-insights-limits"></a>Limity wniosków aplikacji

[AZURE.INCLUDE [application-insights-limits](../includes/application-insights-limits.md)]

### <a name="api-management-limits"></a>Limity dotyczące zarządzania interfejsu API

[AZURE.INCLUDE [api-management-service-limits](../includes/api-management-service-limits.md)]

### <a name="azure-redis-cache-limits"></a>Azure limity Redis pamięci podręcznej

[AZURE.INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### <a name="key-vault-limits"></a>Limity klucza magazynu

[AZURE.INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### <a name="multi-factor-authentication"></a>Uwierzytelnianie wieloskładnikowe
[AZURE.INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### <a name="automation-limits"></a>Limity automatyzacji
[AZURE.INCLUDE [automation-limits](../includes/azure-automation-service-limits.md)]

### <a name="sql-database-limits"></a>Limity bazy danych SQL

Limity bazy danych SQL zobacz [Limity zasobów bazy danych SQL](sql-database/sql-database-resource-limits.md).

## <a name="see-also"></a>Zobacz też

[Opis ograniczenia Azure i podwyżki](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)

[Maszyn wirtualnych i rozmiarów usługi Cloud Azure](virtual-machines/virtual-machines-linux-sizes.md)

[Rozmiary usług w chmurze](cloud-services/cloud-services-sizes-specs.md)
