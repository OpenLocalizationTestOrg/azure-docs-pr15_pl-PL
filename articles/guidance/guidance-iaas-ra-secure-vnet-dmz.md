<properties
   pageTitle="Architektura Azure odwołanie — IaaS: Implementacji strefy Zdemilitaryzowanej między Azure i Internet | Microsoft Azure"
   description="Jak zaimplementować architektury sieci bezpiecznego hybrydowego z dostępem do Internetu w Azure."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-dmz-between-azure-and-the-internet"></a>Implementacji strefy Zdemilitaryzowanej między Azure i Internet

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

W tym artykule opisano najważniejsze wskazówki dotyczące realizacji sieci bezpieczny hybrydowe, który rozszerza akceptującym ruch z sieci internet Azure sieci lokalnej. Ta architektura odwołanie rozszerza architektura opisane w artykule [implementacji strefy Zdemilitaryzowanej między Azure i centrum danych lokalnych][implementing-a-secure-hybrid-network-architecture]. Czytanie dokumentu i opis tego odwołania zalecane jest architektura przed czytania tego dokumentu.

> [AZURE.NOTE] Azure ma dwa różne wdrożenia modele: [Menedżer zasobów] [ resource-manager-overview] i klasyczny. Ta architektura odwołanie używa Menedżera zasobów, które firma Microsoft zaleca się w przypadku wdrożeń nowy. 

Typowe zastosowanie przypadkach dla tej architektury obejmują:

- Aplikacje hybrydowego miejsce, w którym obciążenia Uruchom częściowo lokalnego i częściowo w Azure.

- Infrastruktura Azure kieruje ruch przychodzący lokalnego i z Internetu.

## <a name="architecture-diagram"></a>Diagram architektury

Poniższy diagram wyróżnienie ważne składniki, w tym architektury:

> Dokument programu Visio, który zawiera ten diagram architektury jest dostępna do pobrania w [Centrum pobierania firmy Microsoft][visio-download]. Ten schemat jest na stronie "Strefy Zdemilitaryzowanej – publicznej".

[! [0]][0]

- **Publiczny adres IP (PIP).** To jest adres IP publicznej punktu końcowego. Użytkownicy zewnętrzni połączenie z Internetem dostęp do systemu za pośrednictwem tego adresu.

- **Wirtualna sieć urządzenia (NVA).**  NVA to ogólny termin opisujący maszyny wykonywania zadań, takich jak ma zezwolenie lub odmowa dostępu jako zapora, optymalizowanie operacji WAN (w tym kompresji sieci), niestandardowe routingu lub inne funkcje sieci.

- **Usługi równoważenia obciążenia Azure.** Wszystkie przychodzące żądania z Internetu przechodzą przez ten równoważenia obciążenia i są rozdzielane NVAs w publicznej strefy Zdemilitaryzowanej podsieci ruchu przychodzącego.

- **Strefy Zdemilitaryzowanej publicznej ruchu przychodzącego podsieci.** Tej podsieci odbiera z usługi równoważenia obciążenia Azure. Przychodzących żądań są przekazywane do jednego z NVAs w strefy Zdemilitaryzowanej.

- **Podsieć ruchu wychodzącego publicznej strefy Zdemilitaryzowanej.** Żądania, które zostały zatwierdzone przez NVA przechodzić do danej podsieci do równoważenia obciążenia wewnętrznych dla poziomu sieci web.

## <a name="recommendations"></a>Zalecenia

Azure oferuje wiele różnych zasobów i typów zasobów, aby ta architektura odwołanie może być obsługi administracyjnej wiele różnych sposobów. Utworzono szablon Menedżera zasobów Azure zainstalować architektura odwołania, znajdujący się tych zaleceń. Jeśli chcesz utworzyć własny architektura odwołania, należy wykonać tych zaleceń, jeśli nie masz określonych wymóg, aby zalecenie nie mieści się na.

### <a name="nva-recommendations"></a>Zalecenia dotyczące NVA

Implementowanie jednego zestawu NVAs ruchu pochodzącego z Internetem, druga ruchu pochodzących lokalnego. Należy używać tylko jeden zestaw NVAs dla obu, ponieważ zapewnia obwód nie zabezpieczeń między dwoma zestawami ruch sieciowy ryzyko związane z zabezpieczeniami. Jest korzyści Użyj tego projektu, ponieważ pozwala uniknąć złożoność zabezpieczeń reguły sprawdzania błędów i ułatwia wyraźny reguł, które odpowiadają każdego żądania sieci przychodzącego. Na przykład jeden zestaw NVAs wprowadza reguł sprawdzania ruch internetowy tylko podczas innego zestawu reguł Implementowanie NVAs dla ruchu lokalnego.

Dołączanie warstwy 7 NVA przerwanie połączenia aplikacji na poziomie NVA i zachować koligację z poziomów wewnętrznej bazy danych. Gwarantuje to symetrycznej łączności zwraca odpowiedź ruchu z poziomów wewnętrznej bazy danych za pośrednictwem NVA.  

### <a name="public-load-balancer-recommendations"></a>Zalecenia dotyczące usługi równoważenia obciążenia publicznej ###

Aby zachować skalowalność i dostępność, wdrażanie publicznej strefy Zdemilitaryzowanej ruch przychodzący NVAs [Ustawianie dostępności] [ availability-set] i używanie [internet przeciwległych równoważenia obciążenia] [ load-balancer] do rozpowszechniania żądania internetowe przez NVAs w zestawie dostępności.  

Konfigurowanie usługi równoważenia obciążenia do akceptowania wezwań tylko na porty niezbędne dla ruchu internetowego. Na przykład ograniczyć przychodzących żądań HTTP do portu 80 i przychodzących żądań HTTPS do na porcie 443.

## <a name="scalability-considerations"></a>Zagadnienia dotyczące skalowalność

Projektowanie infrastruktury z Internetem przeciwległych równoważenia obciążenia przed ruchu przychodzącego publicznej podsieci strefy Zdemilitaryzowanej od początku. Nawet w przypadku usługi initally architektura wymaga jednego NVA, będzie łatwiej Skaluj do wielu NVAs w przyszłości, jeśli internet przeciwległych równoważenia obciążenia jest już zainstalowany.

## <a name="availability-considerations"></a>Zagadnienia dotyczące dostępności

Internet przeciwległych równoważenia obciążenia wymaga każdego NVA w publicznej strefy Zdemilitaryzowanej podsieci przychodzących do wykonania [sondy kondycji][lb-probe]. Sonda kondycji, który nie odpowiada na ten punkt końcowy jest uważany za niedostępny, a równoważenia obciążenia kieruje żądania do innych NVAs w tym samym zestawie dostępności. Należy zauważyć, że jeśli wszystkie NVAs nie odpowiadają, aplikacja zakończy się niepowodzeniem, należy koniecznie monitorowanie skonfigurowany do DevOps alertu, gdy liczba wystąpień NVA prawidłowy przypada poniżej wartości progowej.

## <a name="manageability-considerations"></a>Zagadnienia dotyczące zarządzania

Ograniczanie funkcji monitorowania i zarządzania dla ruchu przychodzącego publicznej strefy Zdemilitaryzowanej NVA i reagować na żądania z pola skok tylko podsieć zarządzania. Zgodnie z opisem w [implementacji strefy Zdemilitaryzowanej między Azure i centrum danych lokalnych] [ implementing-a-secure-hybrid-network-architecture] dokumentu, definiowanie trasę jednej sieci z sieci lokalnej przez bramę do pola skok w podsieci zarządzania ograniczenia dostępu.

Jeśli połączenie bramy w Twojej sieci lokalnej Azure nie działa, można nadal osiągnąć polu skok wdrażając PIP, dodanie go do pola przeskoku i zdalnych w z Internetu.

## <a name="security-considerations"></a>Zagadnienia dotyczące zabezpieczeń

Ta architektura odwołanie wykonuje wiele poziomów zabezpieczeń:

- Internet przeciwległych równoważenia obciążenia kieruje żądania NVAs ruchu przychodzącego publicznej strefy Zdemilitaryzowanej tylko podsieć i tylko na porty niezbędne dla aplikacji.

- Reguły NSG dla przychodzących i wychodzących publicznej strefy Zdemilitaryzowanej podsieci uniemożliwiają złamania blokując żądania, które wykraczają poza reguły NSG NVAs.

- Konfiguracja routingu translatora adresów Sieciowych NVAs kieruje przychodzących wezwań na porcie 80 i 443 do równoważenia obciążenia poziomu sieci web, ale ignoruje żądania do innych portów.

Należy zauważyć, że należy rejestrować wszystkich przychodzących wezwań na wszystkie porty. Regularnie inspekcji dzienniki, zwracając uwagę na żądania, które wykraczają poza oczekiwanych parametrów jak te mogą wskazywać dostępu.

### <a name="using-nsgs-to-blockpass-traffic-between-application-tiers"></a>Za pomocą NSGs blok/pass ruch między warstwy aplikacji

Poszczególnych poziomów sieci web, firm i danych ograniczyć ruch między nimi przy użyciu NSGs. Oznacza to, że warstwa firm używa NSG do zablokowania całego ruchu, że nie pochodzi z poziomu sieci web oraz warstwy danych użyto NSG, aby zablokować cały ruch, że nie pochodzi z poziomu firm. Jeśli masz wymaganie rozwiń węzeł reguły NSG umożliwienie szerszego dostępu do tych poziomów, należy rozważyć te wymagania na wypadek zabezpieczeń. Każdy nowy ścieżki ruchu przychodzącego przedstawia szansy sprzedaży dla uszkodzenia danych przypadkowe lub purposeful wyciek lub aplikacji.

## <a name="solution-deployment"></a>Wdrożenie rozwiązania

Wdrożenie dla architektury odwołanie, które wykonuje następujące zalecenia jest dostępna na Github. Ta architektura odwołanie zawiera wirtualnej sieci (VNet), grupa zabezpieczeń sieci (NSG), równoważenia obciążenia i dwóch wirtualnych maszyn.

Architektura informacyjna mogą być rozmieszczone w albo z systemu Windows i Linux oraz maszyny wirtualne, postępując zgodnie z instrukcjami poniżej: 

1. Kliknij prawym przyciskiem myszy przycisk poniżej i wybierz pozycję "Otwórz łącze w nowej karcie" lub "Otwórz łącze w nowym oknie":  
[![Wdrażanie Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2FvirtualNetwork.azuredeploy.json)

2. Po otwarciu łącze w portalu Azure, należy wprowadzić wartości dla niektórych ustawień: 
    - Nazwa **grupy zasobów** jest już zdefiniowana w pliku parametrów, więc wybierz polecenie **Utwórz nowy** i wprowadź `ra-public-dmz-network-rg` w polu tekstowym.
    - Wybierz region w polu rozwijanym **lokalizacji** .
    - Nie należy edytować **Uri główny szablon** lub pól tekstowych **Uri głównego parametru** .
    - Wybierz **Typ systemu operacyjnego** z listy rozwijanej pole **systemu windows** i **linux**.
    - Przejrzyj warunki i postanowienia, a następnie kliknij pole wyboru **zgodę na warunki umowy podanych powyżej** .
    - Kliknij przycisk **Kup** .

3. Poczekaj, aż wdrożenia do wykonania.

4. Kliknij prawym przyciskiem myszy przycisk poniżej i wybierz pozycję "Otwórz łącze w nowej karcie" lub "Otwórz łącze w nowym oknie":  
[![Wdrażanie Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2Fworkload.azuredeploy.json)

5. Po otwarciu łącze w portalu Azure, należy wprowadzić wartości dla niektórych ustawień: 
    - Nazwa **grupy zasobów** jest już zdefiniowana w pliku parametrów, więc wybierz polecenie **Utwórz nowy** i wprowadź `ra-public-dmz-wl-rg` w polu tekstowym.
    - Wybierz region w polu rozwijanym **lokalizacji** .
    - Nie należy edytować **Uri główny szablon** lub pól tekstowych **Uri głównego parametru** .
    - Przejrzyj warunki umowy, a następnie kliknij pole wyboru **zgodę na warunki umowy podanych powyżej** .
    - Kliknij przycisk **Kup** .

6. Poczekaj, aż wdrożenia do wykonania.

7. Kliknij prawym przyciskiem myszy przycisk poniżej i wybierz pozycję "Otwórz łącze w nowej karcie" lub "Otwórz łącze w nowym oknie":  
[![Wdrażanie Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2Fsecurity.azuredeploy.json)

8. Po otwarciu łącze w portalu Azure, należy wprowadzić wartości dla niektórych ustawień: 
    - Nazwa **grupy zasobów** jest już zdefiniowana w pliku parametrów, dlatego wybierz pozycję **Użyj istniejącej** i wprowadź `ra-public-dmz-network-rg` w polu tekstowym.
    - Wybierz region w polu rozwijanym **lokalizacji** .
    - Nie należy edytować **Uri głównego szablonu** lub pól tekstowych **Uri głównego parametru** .
    - Przejrzyj warunki umowy, a następnie kliknij pole wyboru **zgodę na warunki umowy podanych powyżej** .
    - Kliknij przycisk **Kup** .

9. Poczekaj, aż wdrożenia do wykonania.

10. Pliki parametru zawierają stałe administratora nazwę użytkownika i hasło dla wszystkich maszyny wirtualne i zdecydowanie zalecane jest, aby natychmiast zmienić oba. Dla każdego Głosowa w ramach wdrożenia zaznacz go w portalu Azure, a następnie kliknij na **Resetowanie hasła** w karta **pomocy technicznej + rozwiązywania problemów** . Zaznacz pole wyboru **Resetuj hasło** w polu listy rozwijanej **Tryb** , a następnie wybierz nową **nazwę użytkownika** i **hasło**. Kliknij przycisk **Aktualizuj** do innej witryny.


<!-- links -->

[availability-set]: ../virtual-machines/virtual-machines-windows-manage-availability.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-3-tier-vm.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[iptables]: https://help.ubuntu.com/community/IptablesHowTo
[lb-probe]: ../load-balancer/load-balancer-custom-probe-overview.md
[load-balancer]: ../load-balancer/load-balancer-internet-overview.md
[network-security-group]: ../virtual-network/virtual-networks-nsg.md
[ra-vpn]: ./guidance-hybrid-network-vpn.md
[ra-expressroute]: ./guidance-hybrid-network-expressroute.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[0]: ./media/blueprints/hybrid-network-secure-vnet-dmz.png "Zabezpieczanie architektury sieci hybrydowego"
