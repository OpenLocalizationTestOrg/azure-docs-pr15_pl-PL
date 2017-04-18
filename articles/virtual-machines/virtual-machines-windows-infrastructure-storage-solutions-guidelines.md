<properties
    pageTitle="Wskazówki dotyczące rozwiązania magazynowania | Microsoft Azure"
    description="Informacje na temat ważnych wskazówek projektowanie i wdrażanie wdrażania rozwiązania miejsca do magazynowania w usługach Azure infrastruktury."
    documentationCenter=""
    services="virtual-machines-windows"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="storage-infrastructure-guidelines"></a>Wskazówki dotyczące infrastruktury miejsca do magazynowania

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

W tym artykule omówiono na potrzeby przechowywania i zagadnienia projektowe uzyskania optymalnych maszyn wirtualnych (maszyn wirtualnych) wydajności.


## <a name="implementation-guidelines-for-storage"></a>Implementacja wskazówki dotyczące miejsca do magazynowania

Decyzje:

- Należy użyć magazynu Standard lub Premium dla swojego obciążenie pracą?
- Czy potrzebujesz rozkładanie do tworzenia dysków większych niż 1023 GB?
- Czy potrzebujesz rozkładanie uzyskanie optymalnej wydajności we/wy dla swojego obciążenie pracą?
- Jakie zestaw kont przestrzeni dyskowej są potrzebne do obsługi infrastruktury lub z pracą IT?

Zadania:

- Przeglądanie wymagań we/wy aplikacji, wdrażania i planowanie odpowiedni numer i typy kont miejsca do magazynowania.
- Utwórz zestaw przy użyciu Konwencja nazewnictwa kont miejsca do magazynowania. Za pomocą programu PowerShell Azure lub portalu.


## <a name="storage"></a>Miejsca do magazynowania

Azure magazyn jest klucza część wdrażanie i zarządzanie wirtualnych maszyn i aplikacje. Magazyn Azure oferuje usługi przechowywania plików, niestrukturalne danych i wiadomości, a także jest częścią infrastruktury pomocnicze maszyny wirtualne.

Istnieją dwa typy kont miejsca do magazynowania do obsługi maszyny wirtualne:

- Przedstawianie konta standardowego magazynu uzyskiwania dostępu do magazyn obiektów blob (używane do przechowywania dysków maszyn wirtualnych Azure), Magazyn tabel, magazyn kolejek i przechowywania plików.
- Konta [magazynu Premium](../storage/storage-premium-storage.md) przeprowadzania obsługa wysokiej wydajności i małych opóźnień dysku we/wy intensywnie obciążeń pracą, takich jak serwery SQL w klastrze (AlwaysOn). Magazyn Premium obsługuje obecnie tylko dysków maszyn wirtualnych Azure.

Azure tworzy maszyny wirtualne z dysku system operacyjny, dysku tymczasowym i zero lub więcej dyskach dane opcjonalne. System operacyjny dysku i dyskach danych są blob Azure strony, a dysku tymczasowym jest przechowywany lokalnie w węźle miejsce, w którym znajduje się komputer. Zajmować się podczas projektowania aplikacjom tylko używać dysku tymczasowe nietrwałe danych jako maszyn wirtualnych mogą przeprowadzić migrację między tabelami hosts podczas zdarzeń konserwacji. Dane przechowywane na dysku tymczasowym zostanie utracony.

Wytrzymałości i wysoką dostępność jest udostępniany przez źródłowych środowiska magazyn Azure, upewnij się, że dane pozostają chronione przed niezaplanowane awariami konserwacji lub sprzętu. Podczas projektowania środowiska magazyn Azure można replikować maszyn wirtualnych miejsca do magazynowania:

- lokalnie w danym Azure centrum danych
- przez Azure centrach danych w danym regionie
- przez Azure centrach danych w wielu różnych regionów

Można znaleźć [więcej informacji na temat opcji replikacji wysokiej dostępności](../storage/storage-introduction.md#replication-for-durability-and-high-availability).

System operacyjny dyski i dane mają maksymalny rozmiar 1023 gigabajtów (GB). Maksymalny rozmiar obiektu blob wynosi 1024 GB i które muszą zawierać metadanych (stopka) pliku wirtualnego dysku twardego (GB jest 1024 bajtów<sup>3</sup> ). Za pomocą spacji miejsca do magazynowania w systemie Windows Server 2012 powoduje przekroczenie limitu, grupując dyski danych na prezentowanie logiczne ilości większych niż 1023GB do maszyn wirtualnych.

Obowiązują pewne ograniczenia skalowalność podczas projektowania wdrożeń magazyn Azure — zobacz [Limity subskrypcji i usługi Microsoft Azure, przydziały oraz ograniczenia](azure-subscription-service-limits.md#storage-limits) , aby uzyskać więcej informacji. Zobacz też [elementów docelowych wydajność i skalowalność Azure miejsca do magazynowania](../storage/storage-scalability-targets.md).

Do przechowywania aplikacji mogą zawierać dane obiektu niestrukturalne, takie jak dokumenty, obrazy, kopie zapasowe danych konfiguracji, dzienniki, itp. Używanie magazyn obiektów blob. Zamiast aplikacji zapisywania na dysku wirtualnego dołączone do maszyn wirtualnych aplikacja będzie mogła zapisywać bezpośrednio z magazynem obiektów blob platformy Azure. Magazyn obiektów blob oferuje opcję [poziomów ciepłej i atrakcyjne miejsca do magazynowania](../storage/storage-blob-storage-tiers.md) w zależności od potrzeby dostępność i ograniczeń kosztowych.


## <a name="striped-disks"></a>Rozłożone dysków
Oprócz pozwala na tworzenie dysków większych niż 1023 GB w wielu przypadkach przy użyciu rozkładanie dysków danych zwiększa wydajność, umożliwiając wielu obiektów blob na spód magazynowania dla jednego woluminu. Z rozkładanie, we/wy wymagane do zapisywania i odczytywania danych z jednego dysku logiczne przechodzi równolegle.

Azure nakłada ograniczenia liczby dyski danych i przepustowość dostępną w zależności od rozmiaru maszyn wirtualnych. Aby uzyskać szczegółowe informacje zobacz [rozmiarów maszyn wirtualnych](virtual-machines-windows-sizes.md).

Jeśli korzystasz z dysków danych Azure rozkładanie, należy wziąć pod uwagę następujące wskazówki:

- Dyski danych powinna być zawsze maksymalny rozmiar (1023 GB).
- Dołączanie dysków danych maksymalna dozwolona dla rozmiar pamięci Wirtualnej.
- Za pomocą spacji miejsca do magazynowania.
- Unikanie używania opcji buforowania dysku Azure danych (zasad buforowania = Brak).

Aby uzyskać więcej informacji zobacz [spacje miejsca do magazynowania — projektowanie dla wydajności](http://social.technet.microsoft.com/wiki/contents/articles/15200.storage-spaces-designing-for-performance.aspx).


## <a name="multiple-storage-accounts"></a>Wiele kont miejsca do magazynowania

Podczas projektowania środowiska magazyn Azure, można użyć wielu kont przestrzeni dyskowej jako liczba maszyny wirtualne wdrażanie podwyżki. Dzięki temu rozłożenie się we/wy w źródłowych infrastruktury magazyn Azure, aby utrzymać optymalną wydajność maszyny wirtualne i aplikacji. Podczas projektowania aplikacji, które są wdrażania należy rozważyć, czy wymagania we/wy, który ma poszczególnych maszyn wirtualnych i saldo się te maszyny wirtualne na kontach magazyn Azure. Należy unikać grupowania wszystkich wysoka we/wy wymagających maszyny wirtualne w tylko jednej lub dwóch kont miejsca do magazynowania.

Więcej informacji na temat możliwości we/wy różne opcje magazynowania Azure i niektóre zalecamy maksymalne szybkości w trybie, zobacz [elementów docelowych wydajność i skalowalność Azure miejsca do magazynowania](../storage/storage-scalability-targets.md).


## <a name="next-steps"></a>Następne kroki

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 