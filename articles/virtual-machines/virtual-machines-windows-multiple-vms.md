<properties
    pageTitle="Tworzenie wielu maszyn wirtualnych | Microsoft Azure"
    description="Opcje tworzenia wielu maszyn wirtualnych w systemie Windows"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="guybo"/>

# <a name="create-multiple-azure-virtual-machines"></a>Tworzenie wielu Azure maszyn wirtualnych

Istnieje wiele scenariuszy, w którym potrzebne do utworzenia dużej liczby podobne wirtualnych maszyn. Przykładem mogą być komputerowych wysokiej wydajności (HPC), analizy dużych skalowalna i często bezpaństwowców serwery warstwy środkowej lub wewnętrznej bazy danych (na przykład webservers) i rozłożone baz danych.

W tym artykule omówiono dostępnych opcji, aby utworzyć wiele maszyny wirtualne w Azure. Te opcje wykracza poza proste przypadki, w którym ręcznie utworzyć serię maszyny wirtualne. Aby utworzyć wiele maszyny wirtualne, procesy, które zwykle używasz nie skalować oraz, jeśli jest to potrzebne do utworzenia więcej niż kilku maszyny wirtualne.

Jednym ze sposobów tworzenia wielu podobne maszyny wirtualne jest używanie konstrukcji Menedżera zasobów Azure: _pętle zasobów_.

## <a name="resource-loops"></a>Pętle zasobów

Pętle zasobów są syntaktycznych skróty w szablonach Azure Menedżera zasobów. Pętle zasobu można utworzyć zestaw zasobów, podobnie skonfigurowanym w pętli. Pętle zasobów umożliwia tworzenie wielu kont miejsca do magazynowania, interfejsy lub maszyn wirtualnych. Aby uzyskać więcej informacji na temat pętli zasobów dotyczą [Ustawia tworzenie maszyny wirtualne są dostępne za pomocą pętli zasobów](https://azure.microsoft.com/documentation/templates/201-vm-copy-index-loops/).

## <a name="challenges-of-scale"></a>Wyzwania skali

Mimo że pętle zasobu można ułatwić budowanie infrastruktury chmury, w skali i warzywa bardziej zwartym szablony, pozostaną niektórych problemów. Na przykład jeśli pętli zasobów umożliwia tworzenie 100 maszyn wirtualnych, musisz być zgodne z kont miejsca do magazynowania i odpowiadające im maszyny wirtualne Kontrolery interfejsów sieciowych (NIC). Ponieważ liczba maszyny wirtualne prawdopodobnie może różnić się od liczby kont miejsca do magazynowania, należy postępować z innego zasobu o rozmiarach pętli, zbyt. Są to solvable problemów, ale złożoność znacznie zwiększa ze skalą.

Wezwanie innej występuje, gdy potrzebne infrastrukturę skale elastically. Na przykład może być infrastrukturę autoscale, która automatycznie zwiększenie lub zmniejszenie liczby maszyny wirtualne w odpowiedzi na obciążenie pracą. Maszyny wirtualne nie są opisowe zintegrowane mechanizmu różnią się w numer (skala się i skalę). Skala w, usuwając maszyny wirtualne trudno zagwarantować dostępność wysoki, upewniając się, że maszyny wirtualne są zbilansowane domen aktualizacji i błędów.

Na koniec użycie pętli zasobów, wiele połączeń, aby utworzyć zasoby przejdź do podstawowej tkaninie. Wiele połączeń do utworzenia podobnych zasobów, Azure zawiera niejawne możliwość usprawnienia tego projektu i zoptymalizować wdrożenia niezawodności i wydajności. Jest to, gdzie _Ustawia skali maszyn wirtualnych_ okazać się.

## <a name="virtual-machine-scale-sets"></a>Zestawy skali maszyn wirtualnych

Zestawy skali maszyn wirtualnych są zasobami obliczyć Azure wdrażać i zarządzać zestawem identyczne maszyny wirtualne. Przy użyciu wszystkich maszyny wirtualne skonfigurowane takie same, skali maszyn wirtualnych, które zestawy można łatwo skalować w i skalowania. Po prostu zmienić liczbę maszyny wirtualne w zestawie. Można również skonfigurować zestawy skali maszyn wirtualnych autoscale, w zależności od potrzeb obciążenie pracą.

W przypadku aplikacji wymagających przeskalować zasobów obliczeń, a w skali operacji są niejawnie zbilansowane domen usterek i aktualizacji.

Zamiast korelacji wielu zasobów, takich jak karty sieciowe i maszyny wirtualne, ustawianie skali maszyn wirtualnych ma sieci, miejsca do magazynowania, maszyn wirtualnych i właściwości rozszerzenia, które można skonfigurować centralne.

Wprowadzenie do zestawów skali maszyn wirtualnych zapoznaj się z [skali maszyn wirtualnych ustawia strony produktu](https://azure.microsoft.com/services/virtual-machine-scale-sets/). Aby uzyskać więcej szczegółowych informacji przejdź do [skali maszyn wirtualnych zestawów dokumentów](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/).
