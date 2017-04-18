<properties
    pageTitle="Dostępność Ustawianie wskazówki | Microsoft Azure"
    description="Informacje na temat ważnych wskazówek projektowanie i wdrażanie wdrażania zestawów dostępność w usługach Azure infrastruktury."
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

# <a name="availability-sets-guidelines"></a>Dostępność ustawia wskazówki

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

W tym artykule omówiono opis wymaganych czynności planowania dla zestawów dostępność zapewnienie usługi pozostaje aplikacje dostępne podczas planowanej lub niezaplanowane zdarzeń.

## <a name="implementation-guidelines-for-availability-sets"></a>Zasady implementacji dla zestawów dostępności

Decyzje:

- Ile zestawy dostępności są potrzebne dla poszczególnych ról i warstw w infrastrukturze aplikacji?

Zadania:

- Definiowanie liczby maszyny wirtualne w każdej warstwie aplikacji, które są wymagane.
- Określ, czy jest potrzebny dostosowywać liczbę błędów lub zaktualizować domen ma być używany dla aplikacji.
- Definiowanie zestawów wymagane dostępności za pomocą konwencji nazewnictwa i co maszyny wirtualne znajdują się w nich. Maszyny tylko może znajdować się w zestawie jednej dostępności. 

## <a name="availability-sets"></a>Zestawy dostępności

Platformy Azure wirtualnych maszyn może znajdować się w grupę logiczną nazywaną zestawu dostępność. Po utworzeniu maszyny wirtualne w zestawie dostępność platformie Azure rozdziela położenie tych maszyny wirtualne w źródłowym infrastruktury. Powinny być planowanych zdarzeń konserwacji na platformie Azure lub używanego sprzętu / infrastruktury błędów, użyj zestawy dostępność gwarantuje, że co najmniej jeden maszyn wirtualnych pozostanie uruchomiony.

Zgodnie z zaleceniami dotyczącymi aplikacji nie powinny znajdować się na jednym maszyn wirtualnych. Zestaw dostępność, zawierający pojedynczy maszyn wirtualnych nie uzyskać ochrony z poziomu platformie Azure zdarzeń planowanego lub niezaplanowane. [Umowa dotycząca poziomu usług Azure](https://azure.microsoft.com/support/legal/sla/virtual-machines) wymaga dwóch lub więcej maszyny wirtualne dostępność zezwalają rozkład maszyny wirtualne w źródłowym infrastruktury.

Infrastruktura źródłowych platformy Azure dzieli się na zaktualizować a domenami błędów. Te domeny są definiowane przez hosty jakie udostępniać typowych cyklu aktualizacji lub udostępnianie podobne fizycznej infrastruktury takich jak power i sieci. Azure automatycznie rozdziela pośrednictwem usługi SMS dostępność ustawiany domen, aby zachować dostępność i odporność na uszkodzenia. W zależności od rozmiaru aplikacji i liczba maszyny wirtualne w zestawie dostępności można dostosować liczby domen, którego chcesz użyć. Więcej informacji o [zarządzaniu dostępności i korzystanie z domen aktualizacji i błędów](virtual-machines-windows-manage-availability.md).

Podczas projektowania infrastruktury aplikacji, należy też zaplanować warstwy aplikacji, których używasz. Ustawia maszyny wirtualne grupy, które obsługują tę samą funkcję w dostępności, takie jak dostępność, ustaw dla usługi frontonu maszyny wirtualne usług IIS. Tworzenie osobnych dostępność, ustaw dla swojego maszyny wirtualne wewnętrznej uruchomiony program SQL Server. Celem jest zapewnienie każdą część aplikacji jest chroniony zestawu dostępność i co najmniej raz wystąpienie zawsze pozostanie uruchomiony.

Urządzenia do równoważenia obciążenia może być wykorzystana przed każdej warstwy aplikacji pracy obok zestawu dostępności i upewnij się, że dane zawsze mogły być przesyłane do uruchomionego wystąpienia. Bez równoważenia obciążenia pośrednictwem usługi SMS może nadal działać w całym zdarzeń konserwacji zaplanowane i niezaplanowane, ale użytkownik końcowy może okazać się niemożliwe je rozwiązać, jeśli podstawowy maszyn wirtualnych jest niedostępny.


## <a name="next-steps"></a>Następne kroki
[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 