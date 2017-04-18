<properties
    pageTitle="Wskazówki dotyczące grupy zasobów | Microsoft Azure"
    description="Informacje na temat ważnych wskazówek projektowanie i wdrażanie wdrażania grup zasobów w usługach Azure infrastruktury."
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

# <a name="azure-resource-group-guidelines"></a>Zasady dla grup zasobów Azure

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

W tym artykule omówiono wiedzieć, jak logiczny sposób tworzenia środowiska i grupowanie wszystkich składników grup zasobów.


## <a name="implementation-guidelines-for-resource-groups"></a>Zasady implementacji dla grup zasobów

Decyzje:

- Zamierzasz tworzenia grup zasobów przez podstawowe składniki infrastruktury lub wdrażanie aplikacji wykonania?
- Należy ograniczyć dostęp do grup zasobów za pomocą kontrola dostępu oparta na rolach?

Zadania:

- Definiowanie co podstawowe składniki infrastruktury i dedykowane potrzebnych grup zasobów.
- Przejrzyj jak wdrażać szablony Menedżera zasobów w przypadku wdrożeń spójności i powtarzalnych.
- Definiowanie role dostępu użytkowników, z potrzebne do kontrolowania dostępu do grup zasobów.
- Utwórz zestaw przy użyciu Konwencja nazewnictwa grup zasobów. Za pomocą programu PowerShell Azure lub portalu.


## <a name="resource-groups"></a>Grupy zasobów

W Azure logicznie grupy powiązanych zasobów, takich jak klienci miejsca do magazynowania, wirtualnych sieci i wirtualnych maszyn wdrażać, zarządzanie i zachować je jako całość. Ta metoda ułatwia rozmieszczanie aplikacji przy zachowaniu wszystkie zasoby pokrewne razem z perspektywy zarządzania lub udzielenia osobom dostępu do tej grupy zasobów. Aby uzyskać bardziej zaawansowane opis grup zasobów, zapoznanie się z [Omówienie Menedżera zasobów Azure](../azure-resource-manager/resource-group-overview.md).

Kluczowy do grup zasobów jest możliwość tworzenia środowiska korzystania z szablonów. Szablon jest po prostu plik JSON, który deklaruje przestrzeni dyskowej, sieci i obliczyć zasobów. Można również zdefiniować wszystkie powiązane skrypty i konfiguracji, aby zastosować. Za pomocą tych szablonów, możesz utworzyć jednolity, powtarzalnych wdrożeń dla aplikacji. Tej metody można w prosty sposób tworzenia środowisku w fazie projektowania, a następnie za pomocą tego samego szablonu do utworzenia wdrożeniu produkcji lub odwrotnie. Lepsze zrozumienie korzystania z szablonów, można znaleźć w [instruktażu szablonu](../resource-manager-template-walkthrough.md) , który przeprowadzi Cię przez każdy krok budynku szablon.

Istnieją dwie metody różnych, które można wykonać podczas projektowania środowiska z grup zasobów:

- Grupy zasobów dla każdego rozmieszczenia aplikacji łączący kont miejsca do magazynowania, wirtualnych sieci i podsieci, maszyny wirtualne, załaduj równoważenia itp.
- Scentralizowane grupy zasobów zawierają podstawowe wirtualnej sieci i podsieci lub magazynu konta. Aplikacje są następnie własnych grup zasobów, zawierające tylko maszyny wirtualne urządzenia do równoważenia obciążenia, interfejsów itp.

Jak możesz skali, tworzenie scentralizowane grup zasobów wirtualnych sieci i podsieci ułatwia tworzenie połączenia dla opcji łączności hybrydowych sieciowe między lokalnej. Alternatywny podejście jest dla każdej aplikacji ma własne wirtualnej sieci, która wymaga konfiguracji i konserwacji.  [Kontrola dostępu oparta na rolach](../active-directory/role-based-access-control-what-is.md) szczegółowego umożliwiają kontrolowanie dostępu do grup zasobów. W przypadku aplikacji produkcji można kontrolować użytkowników, którzy mogą uzyskać dostęp do tych zasobów lub dla zasobów infrastruktury podstawowych można ograniczyć tylko infrastruktury inżynierów z nimi pracować. Właściciele Twojej aplikacji masz dostęp tylko do składniki aplikacji w ich grupy zasobów i nie podstawową Azure infrastruktury środowiska. Podczas projektowania środowiska należy rozważyć, czy użytkownicy, którzy potrzebują dostępu do zasobów i odpowiednio zaprojektować grup zasobów. 


## <a name="next-steps"></a>Następne kroki

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 