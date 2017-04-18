<properties 
   pageTitle="Interfejsy sieciowej | Microsoft Azure"
   description="Informacje na temat Azure interfejsów w Menedżerze zasobów Azure."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="jdial" />

# <a name="network-interfaces"></a>Interfejsy

Karty sieciowej (NIC) jest połączenia między maszyn wirtualnych (maszyny) i źródłowych siecią oprogramowania. W tym artykule opisano interfejs sieciowy jest i jak są one używane w modelu wdrożenia Azure Menedżera zasobów.

Firma Microsoft zaleca wdrożeniem nowych zasobów przy użyciu modelu wdrożenia Menedżera zasobów, ale można także wdrożyć maszyny wirtualne z łączność sieciowa w [klasycznym](virtual-network-ip-addresses-overview-classic.md) modelu wdrożenia. Użytkownicy zaznajomieni z modelem klasyczny istnieją istotne różnice w sieci maszyn wirtualnych w modelu wdrożenia Menedżera zasobów. Dowiedz się więcej o różnicach między, czytając artykuł [maszyn wirtualnych sieci — klasyczny](virtual-network-ip-addresses-overview-classic.md#differences-between-resource-manager-and-classic-deployments) .

Platformy Azure, karty sieciowej:

1. W przypadku zasobu, który można utworzyć, usunięte, i ma swoje własne można konfigurować ustawienia.
2. Musi być połączony z jednej podsieci Azure wirtualnej sieci (VNet) po jego utworzeniu. Jeśli nie znasz VNets, Dowiedz się więcej o nich, czytając artykuł [Omówienie wirtualnych sieci](virtual-networks-overview.md) . Karta sieciowa musi być połączony z VNet, która istnieje w tym samym Azure [lokalizacji](https://azure.microsoft.com/regions) i [subskrypcji](../azure-glossary-cloud-terminology.md#subscription) jako karty sieciowej. Po utworzeniu NIC, możesz zmienić podsieci, który jest połączony, ale nie można zmienić VNet jest podłączone do.
3. Nazwy przydzielonych do niego, którego nie można zmienić po utworzeniu karty Sieciowej. Nazwa musi być unikatowa w usłudze Azure [Grupa zasobów](../azure-resource-manager/resource-group-overview.md#resource-groups), ale nie musi być unikatowa w subskrypcji, Azure lokalizacji, w której została utworzona lub VNet jest podłączone do. Kilka nic są zwykle tworzone w ramach subskrypcji usługi Azure. Zalecane jest, aby opracować Konwencja nazewnictwa, która umożliwia zarządzanie łatwiejsze niż użycie nazw domyślnych kart. Zobacz artykuł [zalecane konwencje nazewnictwa dla zasobów Azure](../guidance/guidance-naming-conventions.md) sugestii dotyczących.
4. Może zostać dołączony maszyny, ale może być dołączony tylko do jednego maszyn wirtualnych znajdujące się w tym samym miejscu jako karty sieciowej.
5. Zawiera adresu MAC, który jest zachowywane z Sieciowej w celu, dopóki pozostaje dołączony do maszyny. Adres MAC jest zachowywane czy uruchomieniu maszyn wirtualnych (z w systemie operacyjnym) lub zatrzymana (cofnięcie przydziału) i pracę przy użyciu Azure Portal, Azure programu PowerShell lub interfejsu wiersza polecenia Azure. Jeśli jest odłączony od maszyny oraz dołączone do różnych maszyn wirtualnych, NIC otrzymuje innego adresu MAC. Jeśli NIC zostanie usunięty, adres MAC przydzielono nic innego.
6. Musi mieć jeden podstawowy **prywatne** *IP protokołu IPv4* statyczną lub dynamiczną przypisany adres IP do niego.
8. Mogą być zasobu adresu IP publicznej powiązane z nim.
9. Obsługa przyspieszona sieć z jednym głównym we/wy wirtualizacji (SR IOV) dla określonych rozmiarów maszyn wirtualnych określonej wersji systemu operacyjnego Microsoft Windows Server. Aby dowiedzieć się więcej o tej funkcji PODGLĄDU, przeczytaj artykuł [akcelerowanego sieć dla maszyny wirtualnej](virtual-network-accelerated-networking-powershell.md) .
10. Mogą odbierać dane nie są przeznaczone do prywatnych adresów IP do niego przypisana, jeśli przekazywanie adresów IP jest włączona dla karty sieciowej. Jeśli maszyny działa na przykład oprogramowanie zapory, kieruje pakiety nie są przeznaczone do własnych adresów IP. Maszyn wirtualnych nadal musi uruchomić oprogramowanie do routingu lub przesyłania ruchu, ale w tym celu musi być włączony przekazywanie adresów IP dla karty sieciowej.
11. Często zostanie utworzona w tej samej grupy zasobów jako maszyn wirtualnych, jest on połączony z lub sam VNet, że jest podłączone do, ale nie musi być.

## <a name="vms-with-multiple-network-interfaces"></a>Maszyny wirtualne interfejsy wielu sieci

Kart może zostać dołączony do maszyny, ale po to, należy pamiętać o następujących czynności:  

- Rozmiar pamięci Wirtualnej musi obsługiwać kart. Aby dowiedzieć się więcej na temat rozmiarów maszyn wirtualnych obsługuje kart, przeczytaj artykuły [rozmiarów maszyn wirtualnych serwera systemu Windows](../virtual-machines/virtual-machines-windows-sizes.md) i [Linux maszyn wirtualnych rozmiary](../virtual-machines/virtual-machines-linux-sizes.md) .   
- Co najmniej dwa nic muszą być tworzone maszyn wirtualnych. Jeśli maszyn wirtualnych jest tworzone tylko jeden NIC, nawet jeśli rozmiar pamięci Wirtualnej obsługuje więcej niż jeden, dodatkowe nic nie można dołączyć maszyn wirtualnych po jego utworzeniu. Jak długo maszyn wirtualnych została utworzona z co najmniej dwóch nic, można dołączać dodatkowe nic do maszyn wirtualnych po jego utworzeniu, ile rozmiar pamięci Wirtualnej obsługuje nic więcej niż dwóch.  
- Pomocnicza nic (podstawowy NIC nie odłączony) można odłączyć maszyny, jeśli maszyn wirtualnych zawiera co najmniej trzy nic dołączone do niego. Nie można odłączyć nic, jeśli maszyn wirtualnych zawiera dwa lub mniej nic dołączone do niego.  
- Kolejność nic z wewnątrz maszyn wirtualnych będzie losowe i może również zmienić między aktualizacjami Azure infrastruktury. Jednak adresów IP, a odpowiadające im ethernet MAC adresy, pozostaną niezmienione. Załóżmy na przykład, że system operacyjny identyfikuje Azure NIC1 jako Eth1. Eth1 ma adres IP 10.1.0.100 i adres MAC 00-0D-3A-B0-39-0D. Po Azure infrastruktury aktualizacji i uruchom ponownie, system operacyjny może teraz zidentyfikować NIC1 Azure jako Eth2, ale adresy IP i komputerów MAC jest taki sam jak system operacyjny określenia Azure NIC1 jako Eth1. Jeśli ponowne uruchomienie jest zainicjowany przez klienta, kolejności NIC pozostaną niezmienione w systemie operacyjnym.  
- Jeśli maszyn wirtualnych należy [ustawić dostępności](../azure-glossary-cloud-terminology.md#availability-set), wszystkie maszyny wirtualne w zestawie dostępność musi mieć pojedynczy NIC lub kart. Jeśli maszyny wirtualne kart, numer mają nie musi być taka sama, jak każdy maszyn wirtualnych zawiera co najmniej dwa nic.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak utworzyć maszyny z jednym NIC, w artykule [Tworzenie maszyn wirtualnych](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .
- Dowiedz się, jak utworzyć maszyn wirtualnych przy użyciu kart, czytając artykuł [Rozmieszczanie maszyn wirtualnych z wielu NIC](virtual-network-deploy-multinic-arm-ps.md) .
- Dowiedz się, jak utworzyć NIC z wielu konfiguracji adresów IP, czytając artykuł [adresów IP wielu Azure maszyn wirtualnych](virtual-network-multiple-ip-addresses-powershell.md) .
