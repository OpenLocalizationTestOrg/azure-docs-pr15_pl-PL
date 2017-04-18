<properties
   pageTitle="Menedżer zasobów i klasycznym wdrożenia | Microsoft Azure"
   description="W tym artykule opisano różnice między model wdrożenia Menedżera zasobów i klasycznego (lub Zarządzanie usługą) model wdrożenia."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/27/2016"
   ms.author="tomfitz"/>

# <a name="azure-resource-manager-vs-classic-deployment-understand-deployment-models-and-the-state-of-your-resources"></a>Azure Menedżera zasobów a klasyczny wdrażania: Opis modeli wdrażania i stan zasobów

W tym temacie Informacje o Menedżer zasobów Azure i modeli klasyczny wdrożenia, stan zasobów, i dlaczego wdrożono zasobów z jednego lub drugiego. Menedżer zasobów i modeli wdrażania klasyczny reprezentują dwa różne sposoby, wdrażania i zarządzania Azure rozwiązanie. Praca z nimi za pośrednictwem dwa różne zestawy interfejsu API, a wdrożonym zasobów może zawierać istotne różnice. Dwa modele nie są całkowicie zgodne ze sobą. W tym temacie opisano te różnice.

W celu uproszczenia wdrażania i zarządzania zasobami, firma Microsoft zaleca używanie Menedżera zasobów dla wszystkich nowych zasobów. Jeśli to możliwe firma Microsoft zaleca ponownie wdróż istniejące zasobów za pomocą Menedżera zasobów.

Jeśli korzystasz z Menedżera zasobów, warto najpierw należy przejrzeć terminologia zdefiniowane w [Omówienie Menedżera zasobów Azure](azure-resource-manager/resource-group-overview.md).

## <a name="history-of-the-deployment-models"></a>Historia modeli wdrażania

Azure udostępniana pierwotnie modelu Klasyczny wdrożenia. W tym modelu każdemu zasobowi istniała niezależnie; Wystąpił sposobem zgrupować zasoby pokrewne. Zamiast tego trzeba było ręcznie śledzić zasoby, które składa się z rozwiązań lub aplikacji i pamiętaj, aby zarządzać nimi w podejściu skoordynowanego. Aby wdrożyć rozwiązanie, trzeba było utworzyć każdemu zasobowi oddzielnie za pośrednictwem portalu klasyczny lub utworzyć skrypt, który wdrożony wszystkich zasobów w odpowiedniej kolejności. Aby usunąć rozwiązanie, trzeba było usunąć każdemu zasobowi pojedynczo. Nie można zastosować i aktualizowanie zasad kontroli dostępu dla zasoby pokrewne. Ponadto nie możesz zastosować znaczników w celu zasoby, aby nadać mu etykietę z warunki, które ułatwiają monitorowanie zasobów i Zarządzanie rozliczeniami.

W 2014 Azure wprowadzona Menedżera zasobów, dodanego pojęcia grupy zasobów. Grupa zasobów jest kontenerem dla zasobów, które współużytkują typowe cyklu życia. Model wdrażania Menedżera zasobów daje następujące korzyści:

- Możesz wdrożyć, zarządzanie i monitorowanie wszystkich usług dotyczących rozwiązania grupowo, zamiast obsługi tych usług pojedynczo.
- Można wielokrotnie wdrażanie rozwiązania w całej jej cyklu życia i mieć pewność, które zasoby są rozmieszczane spójna.
- Kontrola dostępu można stosować do wszystkich zasobów w grupie zasobów, a te zasady są stosowane są automatycznie podczas dodawania nowych zasobów do grupy zasobów.
- Znaczniki można zastosować do zasobów logicznie organizowanie wszystkich zasobów w ramach subskrypcji.
- Za pomocą notacji obiektu JavaScript (JSON) do definiowania infrastrukturę rozwiązanie. Plik JSON nosi nazwę szablonu Menedżera zasobów.
- Można zdefiniować zależności między zasoby, są one wdrażane w odpowiedniej kolejności.

Podczas dodawania Menedżera zasobów, wszystkie zasoby wstecz zostały dodane do domyślnych grup zasobów. Po utworzeniu zasobu przez klasyczny rozmieszczenie teraz zasobu są tworzone w domyślnej grupie zasobów w tej usłudze, mimo że nie określi tej grupy zasobów wdrożenia. Jednak tylko istniejące w grupie zasobów nie oznacza przekonwertowany zasobu do modelu Menedżera zasobów. Przyjrzymy jak każdej usługi obsługuje modeli dwóch wdrażania w następnej sekcji. 

## <a name="understand-support-for-the-models"></a>Opis pomocy technicznej dla modeli 

Wybierając model wdrożenia, który do użycia dla zasobów, istnieją trzy scenariusze, o których warto pamiętać:

1. Usługa obsługuje Menedżera zasobów i zawiera tylko jeden typ.
2. Usługa obsługuje Menedżera zasobów, ale udostępnia dwa typy - Menedżera zasobów i jedną dla classic. Ten scenariusz dotyczy tylko maszyn wirtualnych, konta miejsca do magazynowania i wirtualnych sieci.
3. Usługa nie obsługuje Menedżera zasobów.

Aby odkryć, czy usługa obsługuje Menedżera zasobów, zobacz [Menedżer zasobów obsługiwane dostawców](resource-manager-supported-services.md).

Jeśli usługę, którą chcesz użyć nie obsługuje Menedżera zasobów, musisz nadal za pomocą klasyczny.

Jeśli usługa obsługuje Menedżera zasobów i **nie jest** maszyny wirtualnej, konto miejsca do magazynowania lub wirtualnej sieci, można użyć Menedżera zasobów bez dowolnego komplikacji.

Maszyn wirtualnych, kont miejsca do magazynowania i wirtualnych sieci Jeśli zasobu został utworzony przez rozmieszczenie klasyczny, musisz nadal do pracy nad nim za pośrednictwem klasyczny operacji. Jeśli maszyn wirtualnych, konta miejsca do magazynowania lub wirtualną sieć została utworzona przez rozmieszczenie Menedżera zasobów, musisz nadal przy użyciu operacji Menedżera zasobów. To rozróżnienie można uzyskać skomplikowana po subskrypcji występują zasoby utworzone za pomocą Menedżera zasobów i wdrażania klasyczny. Tej kombinacji zasobów można utworzyć nieoczekiwane wyniki, ponieważ zasoby te same operacje nie są obsługiwane.

W niektórych przypadkach polecenie Menedżer zasobów można pobrać informacji o zasobie utworzone przez rozmieszczenie klasyczny lub mogą wykonywać zadań administracyjnych, takich jak przenoszenie klasyczny zasobu do innej grupy zasobów. Jednak w takich przypadkach nie powinien podać wrażenia, że typ obsługuje operacje Menedżera zasobów. Załóżmy na przykład, grupa zasobów, która zawiera maszyn wirtualnych o został utworzony w programie klasycznym wdrożenia. Po uruchomieniu następującego polecenia programu PowerShell Menedżera zasobów:

    Get-AzureRmResource -ResourceGroupName ExampleGroup -ResourceType Microsoft.ClassicCompute/virtualMachines

Zwraca maszyny wirtualnej:
    
    Name              : ExampleClassicVM
    ResourceId        : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.ClassicCompute/virtualMachines/ExampleClassicVM
    ResourceName      : ExampleClassicVM
    ResourceType      : Microsoft.ClassicCompute/virtualMachines
    ResourceGroupName : ExampleGroup
    Location          : westus
    SubscriptionId    : {guid}

Menedżer zasobów polecenia cmdlet **Get-AzureRmVM** zwraca tylko maszyn wirtualnych wdrożony za pomocą Menedżera zasobów. Następujące polecenie nie zwraca maszyny wirtualnej utworzone przez rozmieszczenie klasyczny.

    Get-AzureRmVM -ResourceGroupName ExampleGroup

Tylko zasoby utworzonych za pomocą Menedżera zasobów pomocy technicznej znaczniki. Nie można zastosować znaczniki do klasyczny zasobów.

## <a name="resource-manager-characteristics"></a>Właściwości Menedżera zasobów

Aby ułatwić zrozumienie dwóch modeli, Przeanalizujmy cech typy Menedżera zasobów:

- Utworzony przez [Azure portal](https://portal.azure.com/).

     ![Azure portal](./media/resource-manager-deployment-model/portal.png)

     Do obliczeń, przechowywania i sieci zasoby istnieje możliwość korzystania z Menedżera zasobów lub klasyczna wdrożenia. Wybierz pozycję **Menedżer zasobów**.

     ![Wdrożenie Menedżera zasobów](./media/resource-manager-deployment-model/select-resource-manager.png)

- Utworzone za pomocą wersji Menedżera zasobów poleceń cmdlet programu PowerShell Azure. Te polecenia mają format *AzureRmNoun czasownika*.

        New-AzureRmResourceGroupDeployment

- Utworzone za pomocą [Interfejsu API usługi REST Menedżera zasobów Azure](https://msdn.microsoft.com/library/azure/dn790568.aspx) dla pozostałych operacji.

- Utworzony przez Azure poleceń interfejsu wiersza polecenia Uruchom w trybie **arm** .

        azure config mode arm
        azure group deployment create 

- Typ zasobu nie zawiera **(klasyczny)** w polu Nazwa. Poniższa ilustracja przedstawia typ konta **miejsca do magazynowania**.

    ![w przeglądarce](./media/resource-manager-deployment-model/resource-manager-type.png)

## <a name="classic-deployment-characteristics"></a>Właściwości wdrożenia klasyczny

Znasz może również modelu Klasyczny wdrożenia jako model zarządzania usługą.

Zasoby utworzone w modelu Klasyczny wdrożenia udostępnić następujące właściwości:

- Utworzone za pomocą [portalu klasyczny](https://manage.windowsazure.com)

     ![Klasyczny portalu](./media/resource-manager-deployment-model/classic-portal.png)

     Lub Azure portal i określ wdrożenia **Klasyczny** (w przypadku obliczeń, miejsca do magazynowania i sieci).

     ![Klasyczny wdrażania](./media/resource-manager-deployment-model/select-classic.png)

- Utworzone za pomocą wersji Zarządzanie usługą Azure programu PowerShell poleceń cmdlet. Te nazwy polecenia mają format *AzureNoun czasownika*.

        New-AzureVM 

- Utworzone za pomocą [Interfejsu API usługi REST zarządzania usługi](https://msdn.microsoft.com/library/azure/ee460799.aspx) dla pozostałych operacji.
- Utworzony przez Azure poleceń interfejsu wiersza polecenia Uruchom w trybie **asm** .

        azure config mode asm
        azure vm create 

- Typ zasobu zawiera **(klasyczny)** w polu Nazwa. Poniższa ilustracja przedstawia typ konta **miejsca do magazynowania (klasyczny)**.

    ![Typ klasyczny](./media/resource-manager-deployment-model/classic-type.png)

Azure portal umożliwia zarządzanie zasoby, które zostały utworzone przez rozmieszczenie klasyczny.

## <a name="changes-for-compute-network-and-storage"></a>Zmiany dotyczące obliczeń, sieci i miejsca do magazynowania

Poniższy diagram Wyświetla zasoby obliczeń, sieci i miejsca do magazynowania, wdrożony za pomocą Menedżera zasobów.

![Architektura Menedżera zasobów](./media/virtual-machines-azure-resource-manager-architecture/arm_arch3.png)

Zwróć uwagę następujące relacje między zasoby:

- Istnieje wszystkie zasoby w grupie zasobów.
- Maszyny wirtualnej zależy od konta określonego miejsca do magazynowania, zdefiniowane przez dostawcę zasobu miejsca do magazynowania do przechowywania jej dysków w magazynie obiektów blob (wymagany).
- Maszyny wirtualnej odwołuje się do określonych NIC, zdefiniowane w dostawcy zasobów sieci (wymagany) i dostępności, ustaw się zdefiniowane przez dostawcę zasobu obliczeń (opcjonalnie).
- NIC odwołuje się maszyny wirtualnej przypisany adres IP (wymagane), podsieci wirtualnej sieci maszyny wirtualnej (wymagany), a także do grupy zabezpieczeń sieci (opcjonalnie).
- Podsieć w obrębie wirtualnej sieci odwołuje się do grupy zabezpieczeń sieci (opcjonalnie).
- Wystąpienie usługi równoważenia obciążenia odwołuje się do wewnętrznej bazy danych puli adresów IP, które zawiera NIC maszyny wirtualnej (opcjonalnie) i odwołuje się do ładowania równoważenia publiczny czy prywatny adres IP (opcjonalnie).

Poniżej przedstawiono składniki i ich relacji wdrożenia klasyczny:

![Architektura klasyczny](./media/virtual-machines-azure-resource-manager-architecture/arm_arch1.png)

Klasyczny rozwiązanie do obsługi maszyny wirtualnej zawiera:

- Usługa w chmurze wymagane, działająca jako kontenera do obsługi maszyn wirtualnych (obliczeń). Maszyn wirtualnych automatycznie otrzymują kartę sieciową (NIC) i adres IP przypisane przez Azure. Ponadto usługa w chmurze zawiera wystąpienie równoważenia obciążenia zewnętrznych, publiczny adres IP i domyślne punkty końcowe umożliwia zdalny ruch komputerowych i zdalne programu PowerShell dla maszyn wirtualnych systemu Windows i ruch Secure Shell (SSH) dla maszyn wirtualnych z systemem Linux.
- Konto magazynowania wymagane są przechowywane wirtualnych dysków twardych dla maszyny wirtualnej, tym system operacyjny dysków tymczasowe i dodatkowych danych (Magazynowanie).
- Opcjonalnie wirtualnej sieci, działająca jako dodatkowe kontener, w którym można tworzyć podsieci struktury i wyznaczyć podsieci, na którym znajduje się maszyny wirtualnej (sieć).

W poniższej tabeli opisano zmiany w interakcja obliczeń, sieci i miejsca do magazynowania dostawców zasobu:

 Element | Klasyczny | Menedżer zasobów
 ---|---|---
| Usługa w chmurze dla maszyn wirtualnych |  Usługa w chmurze był do przechowywania maszyn wirtualnych, które wymagane dostępności z platformy i równoważenia obciążenia. | Usługa w chmurze już nie jest obiektem wymagane do tworzenia maszyny wirtualnej przy użyciu nowego modelu. |
| Wirtualnych sieci | Wirtualna sieć jest opcjonalna dla maszyny wirtualnej. Jeśli zawiera, wirtualną sieć nie można wdrożyć przy użyciu Menedżera zasobów. | Maszyna wirtualna wymaga wirtualnej sieci, który został wdrożony przy użyciu Menedżera zasobów. |
| Konta miejsca do magazynowania | Maszyna wirtualna wymaga konta miejsca do magazynowania, które są przechowywane wirtualnych dysków twardych dotyczące systemu operacyjnego, dyski tymczasowe i dodatkowych danych. | Maszyna wirtualna wymaga konta miejsca do magazynowania do przechowywania jej dysków w magazynie obiektów blob. |
| Zestawy dostępności | Dostępność dla platformy wskazano przez skonfigurowanie samej "AvailabilitySetName" w przypadku maszyn wirtualnych. Maksymalna liczba domen błędów to 2. | Konfigurowanie dostępności jest zasobu ujawnionego przez dostawcę Microsoft.Compute. Maszyn wirtualnych, które wymagają wysokiej dostępności musi zawierać w zestawie dostępności. Maksymalna liczba domen błędów jest teraz 3. |
| Koligacji grup | Grupy koligacji były wymagane do tworzenia wirtualnych sieci. Jednak wraz z wprowadzeniem regionalne wirtualnych sieci, który nie był wymagany już. |W celu uproszczenia, koncepcja koligacji grup nie istnieje w za pośrednictwem interfejsów API dostępne za pomocą Menedżera zasobów Azure. |
| Równoważenia obciążenia    | Tworzenie usługi w chmurze przewiduje równoważenia obciążenia niejawnej maszyn wirtualnych wdrożony. | Usługi równoważenia obciążenia jest zasobu ujawnionego przez dostawcę Microsoft.Network. Podstawowy sieciowej maszyn wirtualnych, które musi być równoważenia obciążenia należy odwołuje równoważenia obciążenia. Urządzenia do równoważenia obciążenia może być wewnętrznych i zewnętrznych. W przypadku wystąpienia równoważenia obciążenia odwołuje się do wewnętrznej bazy danych puli adresów IP, które zawiera NIC maszyny wirtualnej (opcjonalnie) i odwołuje się do ładowania równoważenia publiczny czy prywatny adres IP (opcjonalnie). [Dowiedz się więcej.](../articles/resource-groups-networking.md) |
|Wirtualny adres IP | Podczas maszyny jest dodawana do usługi w chmurze, usług w chmurze wyświetlony VIP domyślne (wirtualny adres IP). Wirtualny adres IP jest adres skojarzony z usługi równoważenia obciążenia niejawne.    | Publiczny adres IP jest zasobu ujawnionego przez dostawcę Microsoft.Network. Publiczny adres IP może być statyczne (zarezerwowane) lub dynamiczne. Dynamiczne publiczne adresy IP można przypisywać do równoważenia obciążenia. Publiczne adresy IP mogą być chronione za pomocą grupy zabezpieczeń. |
|Zastrzeżony adres IP|   Można zarezerwować adresu IP w Azure i skojarzyć usługi w chmurze w celu zapewnienia lepkich z adresem IP.   | Publiczny adres IP mogą być tworzone w trybie "Statyczne" i zapewnia on tę samą możliwość jako "Zastrzeżonego adresu IP". Statyczne adresy IP publicznej mogą być tylko przypisywane do równoważenia obciążenia teraz. |
|Publiczny adres IP (PIP) na maszyn wirtualnych | Publiczne adresy IP można także skojarzyć z maszyny bezpośrednio. | Publiczny adres IP jest zasobu ujawnionego przez dostawcę Microsoft.Network. Publiczny adres IP może być statyczne (zarezerwowane) lub dynamiczne. Jednak tylko dynamiczne publiczne adresy IP można przypisywać do karty sieciowej od razu pobrać publiczny adres IP na maszyn wirtualnych. |
|Punkty końcowe| Wprowadzania punkty końcowe są wymagane do można skonfigurować na komputerze wirtualnych być otwarte w górę łączności dla niektórych portów. Jeden z typowych trybów nawiązywanie połączeń maszyn wirtualnych gotowe, konfigurując wprowadzania punktów końcowych. | Reguły przychodzące translatora adresów Sieciowych można skonfigurować dla urządzenia do równoważenia obciążenia uzyskanie tę samą możliwość włączenia punkty końcowe w określonych portów do łączenia się z maszyny wirtualne. |
|Nazwa DNS| Usługa w chmurze jak niejawne globalnie unikatowych nazw DNS. Na przykład: `mycoffeeshop.cloudapp.net`. | Nazwy DNS są parametry opcjonalne, które można określić dla zasobu publiczny adres IP. Nazwy FQDN znajduje się w następującym formacie - `<domainlabel>.<region>.cloudapp.azure.com`. |
|Interfejsy | Podstawowy i pomocniczy interfejsu sieci i jego właściwości, które zostały zdefiniowane jako Konfiguracja sieciowa maszyny wirtualnej. | Interfejs sieciowy jest zasobu ujawnionego przez dostawcę Microsoft.Network. Cykl życia interfejsu sieci nie jest związany maszyn wirtualnych. Odwołuje się maszyny wirtualnej przypisany adres IP (wymagane), podsieci wirtualnej sieci maszyny wirtualnej (wymagany), a także do grupy zabezpieczeń sieci (opcjonalnie). |

Aby informacje o nawiązywaniu połączeń wirtualnych sieci z modelami różnych wdrażania, zobacz [Łączenie wirtualnych sieci z modeli rozmieszczania w portalu](./vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

## <a name="migrate-from-classic-to-resource-manager"></a>Migrowanie z klasycznym do Menedżera zasobów

Jeśli możesz przystąpić do migracji zasobów z klasyczny wdrożenia do wdrożenia Menedżera zasobów, zobacz:

1. [Techniczne głębokości dive obsługiwane platformy migracji od klasycznych do Menedżera zasobów Azure](./virtual-machines/virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
2. [Platformy obsługiwane migracji zasobów IaaS od klasycznych do Menedżera zasobów Azure](./virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
3. [Migrowanie IaaS zasoby z klasycznym do Menedżera zasobów Azure przy użyciu programu PowerShell Azure](./virtual-machines/virtual-machines-windows-ps-migration-classic-resource-manager.md)
4. [Migrowanie IaaS zasoby z klasycznym do Menedżera zasobów Azure za pomocą interfejsu wiersza polecenia Azure](./virtual-machines/virtual-machines-linux-cli-migration-classic-resource-manager.md)

## <a name="frequently-asked-questions"></a>Często zadawane pytania

**Czy można utworzyć za pomocą Menedżera zasobów Azure w wirtualnej sieci lub konto miejsca do magazynowania utworzone przy użyciu klasycznej wdrożenia maszyny wirtualnej?**

Nie jest obsługiwane. Nie używaj Menedżera zasobów Azure do wdrożenia maszyny wirtualnej w wirtualnej sieci utworzonej przy użyciu klasycznej wdrożenia.

**Czy mogę utworzyć maszyny wirtualnej za pomocą Menedżera zasobów Azure z obrazu użytkownika, który został utworzony przy użyciu interfejsy API zarządzania usługi Azure?**

Nie jest obsługiwane. Można jednak skopiować pliki wirtualnego dysku twardego z konta miejsca do magazynowania, które zostało utworzone przy użyciu interfejsów API usługi zarządzania i dodać je do nowego konta utworzone za pomocą Menedżera zasobów Azure.

**Jaki jest wpływ na przydziału dla subskrypcji?**

Przydziałów dla kont miejsca do magazynowania utworzone za pomocą Menedżera zasobów Azure maszyn wirtualnych, wirtualnych sieci i są oddzielone od innych przydziałów. Każdej subskrypcji otrzymuje przydziałów, aby utworzyć zasoby za pomocą nowych interfejsów API. Więcej informacji o dodatkowe przydziały [w tym miejscu](../articles/azure-subscription-service-limits.md).

**Czy można kontynuować umożliwiający ustanawianie maszyn wirtualnych, wirtualnych sieci i konta miejsca do magazynowania za pośrednictwem interfejsów API Menedżera zasobów za pomocą mojej skrypty automatyczne?**

Automatyzacja i skryptów, które zostały wbudowane będą nadal działać dla istniejących maszyn wirtualnych wirtualnych sieci utworzone w trybie Zarządzanie usługą Azure. Jednak skrypty muszą być zaktualizowane użyty do nowego schematu do utworzenia tych samych zasobów w trybie Menedżera zasobów.

**Gdzie znajdują się przykłady szablonów Menedżera zasobów Azure?**

Pełna zestaw szablonów starter można znaleźć w [Azure Menedżera zasobów Szybki Start szablonów](https://azure.microsoft.com/documentation/templates/).

## <a name="next-steps"></a>Następne kroki

- Aby szczegółową Tworzenie szablonu, który definiuje maszyn wirtualnych, konto miejsca do magazynowania i wirtualną sieć, zobacz [Instruktaż szablonu Menedżera zasobów](resource-manager-template-walkthrough.md).
- Aby wyświetlić polecenia służące do wdrażania szablonu, zobacz [Wdrażanie aplikacji za pomocą szablonu Azure Menedżera zasobów](resource-group-template-deploy.md).
