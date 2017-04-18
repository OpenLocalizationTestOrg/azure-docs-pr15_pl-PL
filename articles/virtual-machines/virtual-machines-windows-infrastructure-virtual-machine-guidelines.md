<properties
    pageTitle="wskazówki dotyczące maszyn wirtualnych systemu Windows | Microsoft Azure"
    description="Dowiedz się więcej o najważniejszych wskazówek projektowanie i wdrażanie do wdrażania maszyn wirtualnych systemu windows do Azure"
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

# <a name="virtual-machines-guidelines"></a>Wskazówki dotyczące maszyn wirtualnych

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

W tym artykule omówiono opis wymaganych planowania czynności dotyczące tworzenia i zarządzania wirtualnych maszyn w środowisku Azure.

## <a name="implementation-guidelines-for-vms"></a>Implementacja wskazówki dotyczące maszyny wirtualne
Decyzje:

- Ile maszyny wirtualne są wymagane do różnych poziomów aplikacji i składników infrastruktury?
- Jakie zasoby Procesora i pamięci jest konieczne poszczególnych maszyn wirtualnych, i jakie są wymagania dotyczące miejsca do magazynowania?

Zadania:

- Definiowanie obciążenia dla aplikacji i zasobów, które wymagają maszyny wirtualne.
- Wyrównywanie do potrzeb zasobów dla każdego maszyn wirtualnych przy użyciu odpowiedniego typu rozmiar i miejsca do magazynowania maszyn wirtualnych.
- Definiowanie grup zasobów do różnych poziomów i elementów infrastruktury.
- Definiowanie Konwencja nazewnictwa maszyn wirtualnych.
- Tworzenie usługi maszyny wirtualne przy użyciu programu PowerShell Azure sieci web portalu, lub z szablonami Menedżera zasobów.

## <a name="virtual-machines"></a>Maszyn wirtualnych

Jedną z głównych części w środowisku Azure jest prawdopodobnie maszyny wirtualne. Jest to miejsce, w którym uruchamianie aplikacji, baz danych, usług uwierzytelniania, itp.

Należy zrozumieć [różne rozmiary maszyn wirtualnych](virtual-machines-windows-sizes.md) poprawnie rozmiar środowiska z widzenia kosztów i wydajności. Jeśli do maszyny wirtualne nie masz za mało rdzenie Procesora i pamięci, wydajność aplikacji cierpi niezależnie od tego, jak jest zaprojektowanych i opracowanych. Jak zdecydować, rozmiar, w którym maszyn wirtualnych, aby użyć dla każdego składnika w infrastrukturze, przejrzyj sugerowane obciążenia dla każdej serii maszyn wirtualnych jako punktu początkowego. Możesz [zmienić rozmiar maszyny](https://azure.microsoft.com/blog/resize-virtual-machines/) po wdrożeniu.

Magazyn jest odtwarzany kluczową rolę maszyn wirtualnych wydajności. Za pomocą standardowej miejscem do magazynowania, używającym dysków Obracająca zwykła lub miejsce do magazynowania Premium wysokiego obciążenia We/Wy i najwyższą wydajność używającym dysków SSD. Jako o rozmiarze maszyn wirtualnych są kosztu zagadnienia do zaznaczania nośnik przechowywania danych. Więcej [miejsca do magazynowania infrastruktury wskazówki artykuł](virtual-machines-windows-infrastructure-storage-solutions-guidelines.md) Aby dowiedzieć się, jak zaprojektować odpowiednie miejsca do magazynowania dla optymalną wydajność usługi maszyny wirtualne.


## <a name="resource-groups"></a>Grupy zasobów
Składniki, takie jak maszyny wirtualne logicznie są pogrupowane w celu ułatwienia zarządzania i konserwacja przy użyciu [Grup zasobów Azure](../azure-resource-manager/resource-group-overview.md). Przy użyciu grup zasobów, można utworzyć, zarządzanie i monitorowanie wszystkie zasoby, które składają się danej aplikacji. Można także zaimplementować [Kontrola dostępu oparta na rolach](../active-directory/role-based-access-control-what-is.md) w celu udostępnienia innym osobom w obrębie organizacji do zasobów, które potrzebują. Czas Zaplanuj grupy zasobów i przypisania roli. Istnieją różne sposoby faktycznie projektowanie i implementowanie grup zasobów, należy więc przeczytaj [artykuł wskazówki grup zasobów](virtual-machines-windows-infrastructure-resource-groups-guidelines.md) , aby dowiedzieć się, jak najlepiej tworzenia pośrednictwem usługi SMS.


## <a name="templates"></a>Szablony 
Można tworzyć szablony, zdefiniowane przez deklaracyjnych pliki JSON, aby utworzyć pośrednictwem usługi SMS. Szablony zazwyczaj również tworzyć wymagane miejsca do magazynowania, sieci, interfejsów, adresów IP, itp wraz z maszyny wirtualne się. Korzystanie z szablonów do tworzenia spójności i powtarzalnych środowiskach rozwoju i badania celów łatwo replikacji środowisku produkcyjnym i odwrotnie. Więcej informacji o [tworzenia i używania szablonów](../azure-resource-manager/resource-group-overview.md#template-deployment) Aby dowiedzieć się, jak można używać ich do tworzenia i wdrażanie usługi maszyny wirtualne.


## <a name="next-steps"></a>Następne kroki
[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 