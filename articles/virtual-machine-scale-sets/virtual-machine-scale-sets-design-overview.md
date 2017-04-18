<properties
    pageTitle="Projektowanie skali maszyn wirtualnych zestawy dla skali | Microsoft Azure"
    description="Dowiedz się, jak zaprojektować z zestawów skali maszyn wirtualnych Skala"
    keywords="Ustawia skali maszyn wirtualnych maszyn wirtualnych Linux," 
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gatneil"
    manager="madhana"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="gatneil"/>

# <a name="designing-vm-scale-sets-for-scale"></a>Projektowanie skali maszyn wirtualnych zestawy dla skali

W tym temacie omówiono zagadnienia projektowe związane z zestawów skali maszyn wirtualnych. Informacji na temat zestawów skali maszyn wirtualnych są można znaleźć [Omówienie zestawy skali maszyn wirtualnych](virtual-machine-scale-sets-overview.md).


## <a name="storage"></a>Miejsca do magazynowania

Ustawianie skali używa konta miejsca do magazynowania do przechowywania dyskach systemu operacyjnego maszyny wirtualne w zestawie. Zalecamy stopień 20 maszyny wirtualne na koncie miejsca do magazynowania lub mniej. Zalecane jest również umieszczonych na alfabecie znaki początku nazwy kont miejsca do magazynowania. Ten sposób ułatwia rozciągnąć wewnętrznych systemów ładowania. Na przykład w szablonie następujące firma Microsoft korzysta z uniqueString funkcji szablonu Menedżera zasobów do generowania mieszania prefiks, które są poprzedzany do nazw kont miejsca do magazynowania: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat).


## <a name="overprovisioning"></a>Overprovisioning

Począwszy od wersji interfejsu API "2016-03-30", zestawy skali maszyn wirtualnych domyślnie "overprovisioning" maszyny wirtualne. Overprovisioning włączone, skali Ustawianie faktycznie obroty w górę więcej maszyny wirtualne niż prosisz o, a następnie usuwa dodatkowe maszyny wirtualne, które surowa ostatnio w górę. Overprovisioning zwiększa obsługi administracyjnej stawki sukcesu. Nie dotyczy rozliczenie dla te dodatkowe maszyny wirtualne, a nie są traktowane w kierunku limitów przydziału miejsca do.

Chociaż overprovisioning poprawić obsługi administracyjnej stawki sukcesu, może spowodować skomplikowana zachowanie dla aplikacji, która nie jest przeznaczona do obsługi maszyny wirtualne znikał, ale niezapowiedziane. Aby włączyć overprovisioning wyłączone, upewnij się, masz w szablonie następujący ciąg: "overprovision": "wartość false". Więcej informacji można znaleźć w [dokumentacji interfejsu API usługi REST Ustawianie maszyn wirtualnych Skala](https://msdn.microsoft.com/library/azure/mt589035.aspx).

Jeśli wyłączysz overprovisioning, możesz przejść z dala od komputera z większym stopień maszyny wirtualne dla każdego konta miejsca do magazynowania, ale nie zaleca się rozpoczęcie pracy nad 40.


## <a name="limits"></a>Ograniczenia
Zestaw skali oparty na obraz niestandardowy (jeden utworzoną przez Ciebie), należy utworzyć wszystkie OS dysku wirtualnych dysków twardych w ramach jednego konta miejsca do magazynowania. W wyniku wartość maksymalna zalecane liczbę maszyny wirtualne w zestawie skali oparty na obraz niestandardowy to 20. Jeśli wyłączysz overprovisioning, można przejść do 40.

Zestaw skali oparty na obraz platformy jest obecnie ograniczone do 100 maszyny wirtualne (zalecamy 5 kont miejsca do magazynowania dla tej skali).

Aby uzyskać więcej maszyny wirtualne niż te limity dozwolone, należy wdrożyć wiele zestawów skali przedstawionych w [tym szablonie](https://github.com/Azure/azure-quickstart-templates/tree/master/301-custom-images-at-scale).