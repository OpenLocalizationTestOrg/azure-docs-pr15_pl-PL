<properties
   pageTitle="Uruchamianie wielu maszyn wirtualnych | Dokumentacja dotycząca architektura | Microsoft Azure"
   description="Jak uruchomić wielu wystąpień maszyn wirtualnych Azure skalowalność, elastyczność, zarządzania i zabezpieczeń."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/19/2016"
   ms.author="mwasson"/>

# <a name="running-multiple-vms-on-azure-for-scalability-and-availability"></a>Uruchamianie wielu maszyn wirtualnych Azure skalowalność i dostępność 

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

W tym artykule omówiono zestaw sprawdzone wskazówki dotyczące uruchamiania wielu wystąpień maszyn wirtualnych (maszyn wirtualnych), aby zwiększyć skalowalność, dostępność, zarządzanie i zabezpieczeń.   

W tym architektura obciążenie pracą jest rozmieszczane wystąpienia maszyn wirtualnych. Jest pojedynczy publiczny adres IP i ruch internetowy jest rozdzielana maszyny wirtualne, za pomocą usługi równoważenia obciążenia. Ta architektura może służyć do aplikacji jednowarstwowych, takiej jak klaster aplikacji lub miejsce do magazynowania stateless sieci web. Należy również bloku konstrukcyjnego dla N warstwowych aplikacji. 

W tym artykule tworzy uruchamiania [Pojedynczy maszyn wirtualnych Azure][single vm]. Zalecenia w tym artykule dotyczą również tej architektury.

## <a name="architecture-diagram"></a>Diagram architektury

Maszyny wirtualne platformy Azure wymagają obsługi zasobów sieci i miejsca do magazynowania.

> Dokument programu Visio, który zawiera ten diagram architektury jest dostępna do pobrania w [Centrum pobierania firmy Microsoft][visio-download]. Ten diagram znajduje się na "obliczeń — karta maszyn wirtualnych wielokrotne." 

![[0]][0]

Architektura zawiera następujące składniki: 

- **Konfigurowanie dostępności.** [Ustawianie dostępności] [ availability set] zawiera maszyny wirtualne i jest wymagane do obsługi [dostępność Umowa dotycząca poziomu usług dla maszyny wirtualne Azure][vm-sla].

- **VNet**. Co maszyn wirtualnych w Azure zostanie wdrożony w wirtualnej sieci (VNet), podzielona na **podsieci**.

- **Usługi równoważenia obciążenia Azure.** [Usługa równoważenia obciążenia] rozdziela przychodzące żądania internetowe wystąpieniach maszyn wirtualnych w zestawie dostępności. Usługi równoważenia obciążenia zawiera niektóre zasoby pokrewne:

  - **Publiczny adres IP.** Publiczny adres IP jest wymagany dla usługi równoważenia obciążenia do odbierania ruchu internetowego.

  - **Konfiguracja zewnętrzną.** Przypisuje publiczny adres IP usługi równoważenia obciążenia.

  - **Pula adresów wewnętrznej.** Zawiera maszyny wirtualne, które otrzymają ruch przychodzący interfejsów sieciowych (NIC).

- **Załaduj równoważenia reguły.** Służą do rozpowszechniania ruchu sieciowego między wszystkie maszyny wirtualne w puli adresów wewnętrznej. 

- **Reguły translatora adresów Sieciowych.** Służy do kierowania ruchu do określonych maszyn wirtualnych. Na przykład w Aby włączyć protokołu pulpitu zdalnego (RDP) do maszyny wirtualne, Utwórz regułę oddzielnej sieci adres translatora adresów dla każdego maszyn wirtualnych. 

- **Interfejsów sieciowych (NIC)**. Każdy maszyn wirtualnych ma NIC, aby nawiązać połączenie z siecią.

- **Miejsce do magazynowania.** Magazyn kont przytrzymaj obrazów maszyn wirtualnych i innych zasobów związanych z plikami, takich jak dane diagnostyczne maszyn wirtualnych przechwycone przez Azure.

## <a name="recommendations"></a>Zalecenia

Azure oferuje wiele różnych zasobów i typów zasobów, aby ta architektura odwołanie może być obsługi administracyjnej wiele różnych sposobów. Utworzono szablon Menedżera zasobów Azure zainstalować architektura odwołania, znajdujący się zaleceń przedstawionych poniżej. Jeśli chcesz utworzyć własny architektura odwołania, należy wykonać tych zaleceń, jeśli nie masz konkretnego wymogu, która nie obsługuje zalecenia. 

### <a name="availability-set-recommendations"></a>Dostępność Ustawianie zalecenia

Należy utworzyć co najmniej dwa maszyny wirtualne dostępności ustawiono pod kątem obsługi [dostępność Umowa dotycząca poziomu usług dla maszyny wirtualne Azure][vm-sla]. Należy zauważyć, że usługi równoważenia obciążenia Azure wymaga również, że równoważenia obciążenia maszyny wirtualne należą do tego samego zestawu dostępność.

### <a name="network-recommendations"></a>Zalecenia dotyczące sieci

Maszyny wirtualne za usługi równoważenia obciążenia należy umieścić w tej samej podsieci. Nie są uwidaczniane maszyny wirtualne bezpośrednio do Internetu, ale zamiast tego udostępnia poszczególnych maszyn wirtualnych prywatny adres IP. Łączą się klienci przy użyciu publicznego adresu IP usługi równoważenia obciążenia.

### <a name="load-balancer-recommendations"></a>Zalecenia dotyczące usługi równoważenia obciążenia

Dodaj wszystkie maszyny wirtualne dostępności Ustaw puli adresów wewnętrznej równoważenia obciążenia.

Definiowanie reguł równoważenia obciążenia bezpośredni ruchu sieciowego do maszyny wirtualne. Na przykład aby umożliwić ruch HTTP, należy utworzyć regułę, mapowany port 80 z zewnętrzną konfiguracji do portu 80 na puli adresów wewnętrznej. Po otrzymaniu żądania na porcie 80 publiczny adres IP usługi równoważenia obciążenia go będzie skierować żądanie do portu 80 na jednym z nic w puli adresów wewnętrznej.

Definiowanie reguł translatora adresów Sieciowych do rozsyłania ruchu do określonych maszyn wirtualnych. Na przykład aby włączyć RDP do maszyny wirtualne utworzyć regułę osobnych translatora adresów Sieciowych dla każdego maszyn wirtualnych. Każda reguła powinna być zamapowana numer portu odrębnych portu 3389, domyślny port RDP. (Na przykład korzysta z portu 50001 dla "VM1", "VM2," port 50002 itd.) Przypisz reguły translatora adresów Sieciowych do karty maszyny wirtualne sieciowe. 

### <a name="storage-account-recommendations"></a>Zalecenia dotyczące konta miejsca do magazynowania

Tworzenie kont oddzielnych Azure miejsca do magazynowania dla każdego maszyn wirtualnych do przechowywania wirtualnych dyskach twardych (VHD), aby uniknąć naciśnięcie operacji wejścia i wyjścia na drugim [limity (operacji i/o na SEKUNDĘ)] [ vm-disk-limits] w przypadku kont miejsca do magazynowania. 

Utwórz jedno konto miejsca do magazynowania dla dzienniki diagnostyczne. To konto przestrzeni dyskowej są udostępniane za pośrednictwem SMS.

## <a name="scalability-considerations"></a>Zagadnienia dotyczące skalowalność

Istnieją dwie opcje skalowania się maszyny wirtualne platformy Azure: 

- Rozpowszechnianie ruchu sieciowego w zestawie maszyny wirtualne za pomocą usługi równoważenia obciążenia. Możliwość skalowania, obsługi administracyjnej dodatkowe maszyny wirtualne i umieść je za usługi równoważenia obciążenia. 

- Korzystanie z [zestawów skali maszyn wirtualnych][vmss]. Ustawianie skali zawiera wiele Transaction Service identyczne maszyny wirtualne za usługi równoważenia obciążenia. Skala maszyn wirtualnych ustawia na podstawie wydajności metryk autoscaling pomocy technicznej. Jak zwiększa obciążenie maszyny wirtualne, dodatkowe maszyny wirtualne są automatycznie dodawane do równoważenia obciążenia. 

Następny porównano te dwie opcje.

### <a name="load-balancer-without-vm-scale-sets"></a>Usługi równoważenia obciążenia bez zestawy skali maszyn wirtualnych

Usługi równoważenia obciążenia pobiera przychodzących żądań sieci i rozdziela je w nic w puli adresów wewnętrznej. Skalowanie w poziomie, dodać kolejne wystąpienia maszyn wirtualnych do zestawu dostępność (lub cofnąć maszyny wirtualne skali). 

Załóżmy, że korzystasz z serwera sieci web. Trzeba dodać regułę równoważenia obciążenia portu 80 i/lub na porcie 443 (w przypadku SSL). Jeśli klient wysyła żądanie HTTP, równoważenia obciążenia wybiera adresu IP wewnętrznej przy użyciu [algorytmu mieszania] [ load balancer hashing] zawierającej źródłowy adres IP. W ten sposób żądania klienta są rozmieszczane maszyny wirtualne. 

> [AZURE.TIP] Po dodaniu nowych maszyn wirtualnych dostępność Ustaw, upewnij się, że tworzenie NIC dla maszyn wirtualnych i dodać NIC do puli adresów wewnętrznej w module równoważenia obciążenia. W przeciwnym razie ruchu internetowego nie będą kierowane do nowych maszyn wirtualnych.

Każdy Azure Subskrypcja obejmuje limity domyślne, w tym maksymalna liczba maszyny wirtualne rozbiciu na regiony. Można zwiększyć limit, wypełniając żądanie pomocy technicznej. Aby uzyskać więcej informacji, zobacz [Azure subskrypcji i limity dotyczące usługi, przydziałów i ograniczeń][subscription-limits].  

### <a name="vm-scale-sets"></a>Zestawy skali maszyn wirtualnych 

Zestawy skali maszyn wirtualnych ułatwiają wdrażanie i zarządzanie nią zestawu identyczne maszyny wirtualne. Z wszystkimi maszyny wirtualne skonfigurowane tak samo, zestawy skali maszyn wirtualnych obsługuje PRAWDA autoscale bez wstępnie inicjowania obsługi administracyjnej maszyny wirtualne, co ułatwia tworzenie dużych usług kierowanie duży obliczeń, big data i konteneryzowanego obciążenia. 

Aby uzyskać więcej informacji na temat zestawów skali maszyn wirtualnych, zobacz [Omówienie ustawia skali maszyn wirtualnych][vmss].

Zagadnienia dotyczące korzystania z zestawów skali maszyn wirtualnych:

- Jeśli chcesz szybko skalowania maszyny wirtualne lub konieczne autoscale, weź pod uwagę zestawy Skala. 

- Obecnie zestawy skali nie obsługują dyski danych. Opcje przechowywania danych są magazyn plików Azure, dysk systemu operacyjnego, dysk Temp lub przechowywanie zewnętrznych, takich jak magazyn Azure. 

- Wszystkie wystąpienia maszyn wirtualnych w skali ustawiane automatycznie należeć do tego samego zestawu dostępności z 5 błędów a domenami 5 aktualizacji.

- Domyślnie skala zestawów za pomocą "overprovisioning", co oznacza zestaw skali początkowo przepisy więcej maszyny wirtualne niż poproś o, a następnie usuwa dodatkowe maszyny wirtualne. Ulepszony ogólny odsetek, podczas inicjowania obsługi administracyjnej maszyny wirtualne. 

- Zaleca się nie więcej, a następnie niż 20 maszyny wirtualne na miejsca do magazynowania konto z overprovisioning maszyny wirtualne włączony lub nie więcej niż 40 z overprovisioning wyłączone.  

- Menedżer zasobów szablony można znaleźć na skali Wdrażanie zestawów w [Szablony Szybki Start Azure][vmss-quickstart].

- Istnieją dwa sposoby podstawowe, aby skonfigurować maszyny wirtualne wdrożony w zestawie skali: tworzenie obraz niestandardowy i używać rozszerzeń w celu skonfigurowania maszyn wirtualnych po zainicjowaniu obsługi administracyjnej.

    - Zestaw skali oparty na obraz niestandardowy należy utworzyć wszystkie OS dysku wirtualnych dysków twardych w ramach jednego konta miejsca do magazynowania. 

    - Przy użyciu niestandardowych obrazów należy zachować aktualność obrazu.

    - Rozszerzenia może potrwać dłużej nowo ustanawianie maszyn wirtualnych do obracania w górę.

Aby uzyskać dodatkowe informacje, zobacz [Projektowanie maszyn wirtualnych skali zestawy dla skali][vmss-design].

> [AZURE.TIP]  Podczas korzystania z każdego rozwiązania automatyczne skalowanie, należy je przetestować z obciążenie pracą poziom produkcji wyprzedzeniem. 

## <a name="availability-considerations"></a>Zagadnienia dotyczące dostępności

Dostępność zestawu sprawia, że aplikacji bardziej elastyczne, aby planowane i niezaplanowane zdarzeń konserwacji.

- _Planowana konserwacja_ występuje, gdy firma Microsoft aktualizuje podstawowej platformy czasami powoduje maszyny wirtualne należy ponownie uruchomić. Azure umożliwia się maszyny wirtualne w zestawie dostępność nie znajdują się ponownie uruchomić w tym samym czasie, co najmniej jeden przechowywanej uruchomione podczas innym ponownego uruchamiania.

- _Konserwacja niezaplanowane_ się dzieje, gdy występuje błąd sprzętowy. Azure służy do sprawdzenia, czy maszyny wirtualne w zestawie dostępność zainicjowano obsługę administracyjną w więcej niż jeden stojaków serwera. Dzięki temu zmniejszenia wpływu awarii sprzętu, sieci dostawie, power przerwań i tak dalej.

Aby uzyskać więcej informacji, zobacz [Zarządzanie dostępność maszyn wirtualnych][availability set]. Przystępny Przegląd zestawy dostępność dysponuje poniższym klipie wideo: [Jak skonfigurować dostępność ustawiono maszyny wirtualne skali][availability set ch9]. 

> [AZURE.WARNING]  Upewnij się skonfigurować dostępność ustawianie podczas obsługę maszyn wirtualnych. Obecnie jest sposobem Dodawanie maszyny Menedżera zasobów do dostępność po zainicjowano obsługę administracyjną maszyn wirtualnych.

Usługi równoważenia obciążenia używa [sondy kondycji] monitorowanie dostępność maszyn wirtualnych wystąpień. Jeśli sonda nie mają dostępu wystąpienie w limicie czasu, równoważenia obciążenia zatrzymuje wysyłanie ruch do tego maszyn wirtualnych. Jednak równoważenia obciążenia będzie w dalszym ciągu sondy, a jeśli maszyn wirtualnych zostanie ponownie udostępnione, równoważenia obciążenia życiorysy, wysyłając ruch do tego maszyn wirtualnych.

Poniżej przedstawiono kilka zaleceń na sondy kondycji usługi równoważenia obciążenia:

- Sondy przetestować HTTP lub TCP. Jeśli uruchomieniem maszyny wirtualne serwera HTTP (usług IIS, nginx, Node.js aplikacji i tak dalej), utworzyć sondy HTTP. W przeciwnym razie utwórz sondy TCP.

- Sonda HTTP Określ ścieżkę dostępu do punktu końcowego HTTP. Sonda sprawdza, czy odpowiedź HTTP 200 z następującą ścieżkę. Może to być ścieżkę katalogu głównego ("/") lub monitorowanie kondycji punkt końcowy korzystający niektórych logiki niestandardowej, aby sprawdzić kondycję aplikacji. Punkt końcowy musi zezwolić na anonimowe żądania HTTP.

- Sonda jest wysyłana z [znane] [ health-probe-ip] w polu adres IP 168.63.129.16. Upewnij się, że nie blokowania ruchu do i z tego adresu IP w dowolnej zasady zapory lub zasad grupy (NSG) zabezpieczeń sieci.

- Za pomocą [dzienników sondy kondycji] [ health probe log] Aby wyświetlić stan kondycji sondy. Włącz rejestrowanie w portalu Azure dla każdej usługi równoważenia obciążenia. Dzienniki są zapisywane w magazyn obiektów Blob platformy Azure. Dzienniki Pokaż ile maszyny wirtualne na wewnętrzna nie odbierają ruch sieciowy z powodu odpowiedzi sondy nie powiodło się.

## <a name="manageability-considerations"></a>Zagadnienia dotyczące zarządzania

Przy użyciu wielu maszyny wirtualne staje się ważne automatyzowanie procesów, dzięki czemu są one niezawodne i powtarzalnych. Korzystając z [Automatyzacji Azure] [ azure-automation] do automatycznego wdrożenia, poprawianie systemu operacyjnego i innych zadań. Automatyzacja Azure to usługa automatyzacji działa na Azure, która jest oparty na programu Windows PowerShell. Przykład skryptów automatyzacji są dostępne w [Galerii działań aranżacji] w witrynie TechNet.

## <a name="security-considerations"></a>Zagadnienia dotyczące zabezpieczeń

Wirtualnych sieci są granicę izolacji ruch platformy Azure. Maszyny wirtualne w jednym VNet nie można komunikować się bezpośrednio do maszyny wirtualne w różnych VNet. Maszyny wirtualne w tym samym VNet komunikowania się między sobą, chyba że tworzenie [grup zabezpieczeń sieci] [ nsg] (NSGs), aby ograniczyć ruch. Aby uzyskać więcej informacji, zobacz [usług w chmurze firmy Microsoft i zabezpieczeń sieci][network-security].

Dla ruchu przychodzącego Internet zasady równoważenia obciążenia określić, jaki ruch można osiągnięcia wewnętrzna. Zasady równoważenia obciążenia nie obsługuje jednak IP whitelisting, tak aby listy sprawdzonej adresy niektórych publiczny adres IP, dodawanie NSG do podsieci.

## <a name="solution-deployment"></a>Wdrożenie rozwiązania

Wdrożenie dla architektury odwołanie, które wykonuje następujące zalecenia jest dostępna na GitHub. Ta architektura odwołanie zawiera wirtualnej sieci (VNet), grupa zabezpieczeń sieci (NSG), równoważenia obciążenia i dwóch wirtualnych maszyn.

Architektura informacyjna mogą być rozmieszczone albo z systemu Windows i Linux oraz maszyny wirtualne postępując zgodnie z instrukcjami poniżej: 

1. Kliknij prawym przyciskiem myszy przycisk poniżej i wybierz pozycję "Otwórz łącze w nowej karcie" lub "Otwórz łącze w nowym oknie":  
[![Wdrażanie Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-multi-vm%2Fazuredeploy.json)

2. Po otwarciu łącze w portalu Azure, należy wprowadzić wartości dla niektórych ustawień: 
    - Nazwa **grupy zasobów** jest już zdefiniowana w pliku parametrów, dlatego wybierz pozycję **Użyj istniejącej** i wprowadź `ra-multi-vm-rg` w polu tekstowym.
    - Wybierz region w polu rozwijanym **lokalizacji** .
    - Nie należy edytować **Uri głównego szablonu** lub pól tekstowych **Uri głównego parametru** .
    - Wybierz **Typ systemu operacyjnego** z listy rozwijanej pole **systemu windows** i **linux**. 
    - Przejrzyj warunki umowy, a następnie kliknij pole wyboru **zgodę na warunki umowy podanych powyżej** .
    - Kliknij przycisk **Kup** .

3. Poczekaj, aż wdrożenia do wykonania.

4. Pliki parametru zawierają stałe administratora nazwę użytkownika i hasło, a zdecydowanie zalecane jest, aby natychmiast zmienić oba. Polecenie maszyn wirtualnych, o nazwie `ra-multi-vm1` w portalu Azure. Następnie kliknij **Resetowanie hasła** w karta **pomocy technicznej + rozwiązywania problemów** . Zaznacz pole wyboru **Resetuj hasło** w polu listy rozwijanej **Tryb** , a następnie wybierz nową **nazwę użytkownika** i **hasło**. Kliknij przycisk **Aktualizuj** , aby zapisać nową nazwę użytkownika i hasło. Powtórz dla maszyn wirtualnych, o nazwie `ra-multi-vm2`.

W pliku readme w [orientacji jedno-maszyn wirtualnych] dla informacji na temat dodatkowe sposoby wdrożenia tej architektury odwołanie,[ github-folder] GitHub folder. 

## <a name="next-steps"></a>Następne kroki

Umieszczanie kilku maszyny wirtualne za usługi równoważenia obciążenia jest bloku konstrukcyjnego do tworzenia wielu architektury. Aby uzyskać więcej informacji, zobacz [Uruchamianie maszyny wirtualne systemu Windows dla architektury N warstwowych Azure] [ n-tier-windows] i [Systemem Linux maszyny wirtualne dla architektury N warstwowych Azure][n-tier-linux]

<!-- Links -->
[availability set]: ../virtual-machines/virtual-machines-windows-manage-availability.md
[availability set ch9]: https://channel9.msdn.com/Series/Microsoft-Azure-Fundamentals-Virtual-Machines/08
[azure-automation]: https://azure.microsoft.com/en-us/documentation/services/automation/
[azure-cli]: ../virtual-machines-command-line-tools.md
[bastion host]: https://en.wikipedia.org/wiki/Bastion_host
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-multi-vm
[health probe log]: ../load-balancer/load-balancer-monitor-log.md
[sondy kondycji]: ../load-balancer/load-balancer-overview.md#service-monitoring
[health-probe-ip]: ../virtual-network/virtual-networks-nsg.md#special-rules
[Usługa równoważenia obciążenia]: ../load-balancer/load-balancer-get-started-internet-arm-cli.md
[load balancer hashing]: ../load-balancer/load-balancer-overview.md#hash-based-distribution
[n-tier-linux]: guidance-compute-n-tier-vm-linux.md
[n-tier-windows]: guidance-compute-n-tier-vm.md
[naming conventions]: guidance-naming-conventions.md
[network-security]: ../best-practices-network-security.md
[nsg]: ../virtual-network/virtual-networks-nsg.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md 
[Galeria działań aranżacji]: ../automation/automation-runbook-gallery.md#runbooks-in-runbook-gallery
[single vm]: guidance-compute-single-vm.md
[subscription-limits]: ../azure-subscription-service-limits.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vm-disk-limits]: ../azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines/v1_2/
[vmss]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md
[vmss-design]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview.md
[vmss-quickstart]: https://azure.microsoft.com/documentation/templates/?term=scale+set
[VM-sizes]: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
[0]: ./media/blueprints/compute-multi-vm.png "Architektura rozwiązania maszyn wirtualnych wielokrotne Azure obejmujące dostępność został za pośrednictwem dwóch SMS i równoważenia obciążenia"
