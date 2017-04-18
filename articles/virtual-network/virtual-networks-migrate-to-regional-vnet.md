<properties 
   pageTitle="Jak przeprowadzić migrację z grup koligacji do regionalnych wirtualnej sieci (VNet)"
   description="Dowiedz się, jak przeprowadzić migrację z grup koligacji do vnets regionalnych"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-migrate-from-affinity-groups-to-a-regional-virtual-network-vnet"></a>Jak przeprowadzić migrację z grup koligacji do regionalnych wirtualnej sieci (VNet)

Grupa koligacji umożliwia upewnij się, że zasoby utworzonych w obrębie tej samej grupy koligacji fizycznie są obsługiwane przez serwery, które są blisko siebie, włączanie te zasoby do komunikowania się szybszy. W przeszłości grupy koligacji te wymagania dotyczące tworzenia wirtualnych sieci (VNets). W tym czasie usługę Menedżer sieci, które zarządzane VNets można pracować tylko w zestawie serwerów fizycznych lub Skala jednostki. Ulepszenia architektury wzrosnąć zakresu zarządzania sieci do obszaru.

Wyniku następujące ulepszenia architektury koligacji grup są już zalecane lub wymagane wirtualnych sieci. Korzystania z grup koligacji dla VNets została zastąpiona regionów. VNets, które są skojarzone z regionów są nazywane VNets regionalne.

Ponadto zaleca się, że nie używasz koligacji grup ogólnie. Oprócz wymagania VNet koligacji grup były również należy upewnić się, że zasoby, takie jak obliczeń i miejscem do magazynowania, zostały umieszczone obok siebie za pomocą. Jednak z bieżącym architektury sieci Azure, te wymagania położenie są już potrzebne. Zobacz [koligacji grupy i maszyny wirtualne](#Affinity-groups-and-VMs) dla kilku pozostały w szczególnych przypadkach miejsce, w którym można użyć grupy koligacji.

## <a name="creating-and-migrating-to-regional-vnets"></a>Tworzenie i przeprowadzania migracji do VNets regionalnych

Przechodzenie do przodu, podczas tworzenia nowego VNets, użyj *regionu*. Zobaczysz to jako opcja w portalu zarządzania. Należy zauważyć, że w pliku konfiguracji sieci pokazuje jako *lokalizacji*.

>[AZURE.IMPORTANT] Nadal technicznego umożliwia utworzenie wirtualnej sieci, który jest skojarzony z grupą koligacji, ale istnieje nie istotny powód, aby to zrobić. Wiele nowych funkcji, takich jak grup zabezpieczeń sieci, są dostępne tylko podczas korzystania z regionalnych VNet i nie są dostępne dla wirtualnych sieci skojarzonych z grupami koligacji.

### <a name="about-vnets-currently-associated-with-affinity-groups"></a>Informacje o VNets aktualnie skojarzone z koligacji grup

VNets obecnie skojarzonych z grupami koligacji są włączone do migracji do VNets regionalne. Aby przeprowadzić migrację do regionalnych VNet, wykonaj następujące czynności:

1. Eksportowanie pliku konfiguracji sieci. Za pomocą programu PowerShell lub portalu zarządzania. Aby uzyskać instrukcje za pomocą portalu zarządzania zobacz [Konfigurowanie usługi VNet za pomocą pliku konfiguracji sieci](virtual-networks-using-network-configuration-file.md).

1. Edytowanie pliku konfiguracji sieci, zamieniając starych wartości nowe wartości. 

    > [AZURE.NOTE] **Lokalizacja** jest region, określone grupy koligacji, który jest skojarzony z VNet. Na przykład jeśli usługi VNet jest skojarzony z grupą koligacji znajdującego się w USA Zachód, podczas migracji, muszą wskazywać swojej lokalizacji zachód USA. 
    
    Edytuj następujące wiersze w pliku konfiguracji sieci, zamieniając wartości na własny: 

    **Stara wartość:** \<VirtualNetworkSitename = AffinityGroup "VNetUSWest" = "VNetDemoAG"\> 

    **Nową wartość:** \<VirtualNetworkSitename = lokalizacja "VNetUSWest" = "Zachód US"\>

1. Zapisz zmiany i [zaimportować](virtual-networks-using-network-configuration-file.md) konfigurację sieci Azure.

>[AZURE.NOTE] Tej migracji nie powoduje dowolnego przestoje usługach.

## <a name="affinity-groups-and-vms"></a>Grupy koligacji i maszyny wirtualne

Jak już wspomniano, grup koligacji są już ogólnie zalecane dla maszyny wirtualne. Grupa koligacji należy używać tylko wtedy, gdy zestaw maszyny wirtualne musi mieć bezwzględne najniższe opóźnienie sieci między maszyny wirtualne. Zaznaczając maszyny wirtualne w grupie koligacji, maszyny wirtualne zostaną umieszczone w samej jednostki klaster lub Skala obliczeń.

Należy pamiętać, że przy użyciu grupy koligacji można mieć dwie, prawdopodobnie liczbą ujemną, konsekwencje:

- Ustawianie rozmiarów maszyn wirtualnych będą ograniczone do zestawu rozmiarów maszyn wirtualnych oferowanych przez jednostkę skali obliczeń.

- Istnieje wyższe prawdopodobieństwo niemożności przydzielić nowych maszyn wirtualnych. Dzieje się tak, gdy jednostki określonej skali dla grupy koligacji znajduje się poza wydajność.

### <a name="what-to-do-if-you-have-a-vm-in-an-affinity-group"></a>Co zrobić, jeśli masz maszyny w grupie koligacji

Maszyny wirtualne, które są obecnie w grupie koligacja nie trzeba usunąć się z grupy koligacji.

Po wdrożeniu maszyny zostanie wdrożony jednostka pojedynczy Skala. Grupy koligacji mogą ograniczyć zestaw dostępnych rozmiarów maszyn wirtualnych do nowego wdrożenia maszyn wirtualnych, ale istniejące maszyn wirtualnych, wdrożoną już jest ograniczona do zestawu rozmiarów maszyn wirtualnych dostępne w skali jednostką, w której zostanie wdrożony maszyn wirtualnych. Z tego powodu usunięcie maszyny z grupy koligacji wywoła żadnych skutków.
 
