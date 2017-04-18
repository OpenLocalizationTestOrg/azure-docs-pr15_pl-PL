<properties
   pageTitle="Uruchamianie maszyn wirtualnych systemu Windows dla architektury N warstwowych | Dokumentacja dotycząca architektura | Microsoft Azure"
   description="Jak wdrażać architektura wielu Azure, płatności szczególną uwagę dostępność, bezpieczeństwo, skalowalność i łatwości zarządzania zabezpieczeń."
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
   ms.date="10/20/2016"
   ms.author="mwasson"/>

# <a name="running-windows-vms-for-an-n-tier-architecture-on-azure"></a>Uruchamianie maszyn wirtualnych systemu Windows dla architektury N warstwowych Azure

> [AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

> [AZURE.SELECTOR]
- [Uruchamianie maszyn wirtualnych Linux oraz dla architektury N warstwowych Azure](guidance-compute-n-tier-vm-linux.md)
- [Uruchamianie maszyn wirtualnych systemu Windows dla architektury N warstwowych Azure](guidance-compute-n-tier-vm.md)

W tym artykule omówiono zestaw sprawdzone wskazówki dotyczące uruchamiania systemu Windows wirtualnych maszyn dla aplikacji z architektury N warstwy. Opiera się na artykuł [Uruchamianie wielu maszyn wirtualnych Azure][multi-vm].

> [AZURE.NOTE] Azure ma dwa różne wdrożenia modele: [Menedżer zasobów] [ resource-manager-overview] i klasyczny. W tym artykule używa Menedżera zasobów, które firma Microsoft zaleca się w przypadku wdrożeń nowy.

## <a name="architecture-diagram"></a>Diagram architektury

Istnieje odmiany architektury N warstwowych. W większości przypadków różnice nie ma znaczenia, na potrzeby tych zaleceń. W tym artykule założono, aplikacji sieci web Typowa poziomu 3:

- **Warstwa sieci Web.** Obsługuje przychodzące żądania HTTP. Odpowiedzi są zwracane przez tego poziomu.

- **Warstwa firm.** Narzędzi procesów biznesowych i innych funkcjonalności logiki systemu.

- **Warstwa bazy danych.** Miejsce przechowywania danych trwałych, przy użyciu [Programu SQL Server zawsze na dostępność grup] [ sql-alwayson] wysokiej dostępności.

> Dokument programu Visio, który zawiera ten diagram architektury jest dostępna do pobrania w [Centrum pobierania firmy Microsoft][visio-download]. Ten diagram znajduje się na "obliczeń - wielostronicowy warstwa (Windows).

![[0]][0]

- **Zestawy dostępności.** Tworzenie [Zestawu dostępność] [ azure-availability-sets] dla każdego poziomu i obsługi administracyjnej co najmniej dwa maszyny wirtualne w każdej warstwie. Ta metoda jest wymagane do osiągnięcia dostępność [SLA] [ vm-sla] dla maszyny wirtualne.

- **Podsieci.** Utwórz osobną podsieć dla każdej warstwy. Określ adres zakresu i maskę podsieci przy użyciu notacji [CIDR] . 

- **Załaduj równoważenia.** Korzystanie [z Internetu równoważenia obciążenia] [ load-balancer-external] do dystrybucji przychodzącego ruchu internetowego warstwa i [równoważenia obciążenia wewnętrznych] [ load-balancer-internal] do dystrybucji ruchu w sieci z poziomu sieci web w warstwie firm.

- **Jumpbox**. _Jumpbox_, nazywany [host bastionu]jest maszyn wirtualnych sieci, którego administratorzy nawiązywania połączenia z innymi maszyny wirtualne. Jumpbox ma NSG, która zezwala na zdalny ruch tylko z whitelisted publicznych adresów IP. NSG należy zezwolić na ruch pulpitu zdalnego (RDP).

- **Monitorowanie**. Monitorowanie oprogramowania, takich jak [Nagios], [Zabbix]lub [Icinga] można uzyskać czas reakcji, czas pracy maszyn wirtualnych i ogólnej kondycji systemu. Zainstaluj oprogramowanie monitorowania na maszyny umieszczonego w osobnym zarządzania podsieci.

- **NSGs**. Używanie [grup zabezpieczeń sieci] [ nsg] (NSGs), aby ograniczyć ruch sieciowy w VNet. Na przykład w architekturze poziomu 3 pokazanego warstwy bazy danych nie akceptuje ruchu z frontonu sieci web, tylko z poziomu firm i podsieć zarządzania.

- **SQL Server zawsze na grupy dostępności.** Zapewnia wysoką dostępność w warstwie danych, włączając replikacji i pracy awaryjnej.

- **Serwerów (AD DS) usługi active Directory Domain Services**. Usługach domenowych Active Directory (AD DS) są przechowywane dane katalogów i zarządza komunikacji między użytkownikami a domeną, w tym procesami logowania użytkowników, uwierzytelniania i wyszukiwania w katalogu. Kontroler domeny usługi Active Directory jest serwer usług AD DS. Przed Windows Server 2016 zawsze na grupy dostępności musi być dołączony do domeny. Jest to spowodowane grupy dostępność zależą od technologii systemu Windows Server pracy awaryjnej klaster (WSFC). Windows Server 2016 wprowadzono możliwość tworzenia klastrze pracy awaryjnej bez usługi Active Directory. Aby uzyskać więcej informacji zobacz [Co nowego w awaryjnej w systemie Windows Server 2016][wsfc-whats-new]

## <a name="recommendations"></a>Zalecenia

Azure oferuje wiele różnych zasobów i typów zasobów, aby ta architektura odwołanie może być obsługi administracyjnej wiele różnych sposobów. Utworzono szablon Menedżera zasobów Azure zainstalować architektura odwołania, znajdujący się tych zaleceń. Jeśli chcesz utworzyć własny architektura odwołanie tych zaleceń bez określonej wymóg, aby zalecenie nie mieści się.

### <a name="vnet--subnets"></a>VNet-podsieci

Po utworzeniu VNet przydzielić miejsca adres dla podsieci, które będą potrzebne. Określ maskę zakres i podsieć adresów VNet przy użyciu notacji [CIDR] . Przestrzeń adresów, która znajduje się w standardowej [prywatne bloki adresów IP]za pomocą[private-ip-space], które są 10.0.0.0/8, 172.16.0.0/12 i 192.168.0.0/16.

Wybierz zakres adresów, aby nie nakładały się z siecią w wersji lokalnej, w przypadku, gdy trzeba skonfigurować bramę między VNet a siecią lokalna później. Po utworzeniu VNet, nie można zmienić zakres adresów.

Projektowanie podsieci wymagania funkcjonalności i zabezpieczeń pamiętać. Wszystkie maszyny wirtualne w tej samej roli lub warstwy należy przejść do tej samej podsieci, co może być granicę zabezpieczeń. Aby uzyskać więcej informacji o projektowaniu VNets podsieci, zobacz [Planowanie i projektowanie Azure wirtualnych sieci][plan-network].

Dla każdej podsieci Określ przestrzeni adresów podsieci w notacji CIDR. Na przykład "10.0.0.0/24" tworzy zakres 256 adresów IP. (Maszyny wirtualne można używać 251 tych; 5 są zarezerwowane. [Wirtualna sieć często zadawane pytania dotyczące][vnet faq].) Upewnij się, że zakresy adresów nie nakładały się na podsieci.

### <a name="network-security-groups"></a>Grupy zabezpieczeń sieci

Aby ograniczyć ruch między warstwy za pomocą reguł NSG. Na przykład w architekturze poziomu 3, jak pokazano powyżej warstwie sieci web komunikuje się bezpośrednio z poziomu bazy danych. Aby wymusić to, warstwy bazy danych należy zablokować ruch przychodzący z podsieci poziomu sieci web.  

  1. Tworzenie NSG i kojarzenie go do podsieci Warstwa bazy danych.

  2. Dodaj regułę, która blokuje cały ruch przychodzący z VNet. (Użyj `VIRTUAL_NETWORK` znacznika w regule.) 

  3. Dodawanie reguły o wyższym priorytecie, umożliwiająca ruchu przychodzącego z podsieci warstwa firm. Ta reguła zastępuje poprzedniego reguły, a umożliwia warstwie firm porozmawiać z poziomu bazy danych.

  4. Dodawanie reguły, która zezwala na ruch przychodzący z poziomu bazy danych warstwa podsieć. Ta reguła umożliwia komunikacji między maszyny wirtualne w warstwie bazy danych, który jest wymagany Replikacja bazy danych i pracy awaryjnej.

  5. Dodaj regułę, która zezwala na ruch RDP z podsieci jumpbox. Ta reguła umożliwia administratorom nawiązywanie połączenia z poziomu bazy danych z jumpbox.

  > [AZURE.NOTE] [Domyślne reguły] ma NSG[ nsg-rules] umożliwiające dowolnego ruchu przychodzącego z poziomu VNet. Nie można usunąć tych reguł, ale można je zmienić, tworząc reguły wyższy priorytet.

### <a name="load-balancers"></a>Urządzenia do równoważenia obciążenia

Usługi równoważenia obciążenia zewnętrznych rozdziela ruch internetowy warstwie sieci web. Tworzenie publiczny adres IP dla tej usługi równoważenia obciążenia. Zobacz [Tworzenie równoważenia obciążenia z Internetu][lb-external-create].

Usługi równoważenia obciążenia wewnętrznych rozdziela ruch sieciowy z poziomu sieci web w warstwie firm. Aby przekazać ten równoważenia obciążenia prywatny adres IP, Tworzenie konfiguracji IP serwera sieci Web i skojarzyć podsieć warstwie firm. Zobacz [Wprowadzenie do tworzenia równoważenia obciążenia wewnętrznych][lb-internal-create].

### <a name="sql-server-always-on-availability-groups"></a>Program SQL Server zawsze na grupy dostępności

Zalecamy, aby [Zawsze na grupy dostępności] [ sql-alwayson] wysokiej dostępności programu SQL Server. Zawsze na grupy dostępności wymagają kontrolera domeny. Wszystkie węzły w grupie dostępność musi być w tej samej domeny AD.

Inne poziomy połączenia z bazą danych za pomocą [dostępności grupy odbiornika][sql-alwayson-listeners]. Odbiornik umożliwia programu SQL client nawiązania połączenia bez znajomości nazwisk fizycznie wystąpienie programu SQL Server. Maszyny wirtualne, że baza danych programu access muszą być dołączone do domeny. Klient (w tym przypadku innej warstwie) używa DNS do rozpoznawania nazw wirtualną sieć odbiornika do adresów IP.

Konfigurowanie programu SQL Server zawsze włączone w następujący sposób:

- Tworzenie klaster systemu Windows Server pracy awaryjnej klaster (WSFC) i grupy dostępności SQL Server zawsze włączone. Aby uzyskać więcej informacji, zobacz [Wprowadzenie do zawsze na grupy dostępności][sql-alwayson-getting-started].

- Tworzenie równoważenia obciążenia wewnętrznych przy użyciu statycznego adresu IP prywatne.

- Utwórz detektor grupy dostępności i mapowania nazwy DNS odbiornika adres IP usługi równoważenia obciążenia wewnętrzny. 

- Tworzenie reguły równoważenia obciążenia portu słuchanie programu SQL Server (port TCP 1433 domyślnie). Reguła równoważenia obciążenia, należy włączyć _przestawny IP_, nazywany serwer bezpośredni zwrócić. Ta opcja powoduje maszyn wirtualnych odpowiedzieć bezpośrednio do klienta, który umożliwia bezpośrednie połączenie replice podstawowego.

    > [AZURE.NOTE] Jeśli opadające IP jest włączona, numer portu zewnętrzną musi być taki sam jak numer portu wewnętrznej w regule równoważenia obciążenia.

Próba połączenia przez klienta SQL równoważenia obciążenia kieruje żądanie połączenia do replice jest to podstawowa bieżącego. W przypadku awarię na innej replice równoważenia obciążenia automatycznie kieruje kolejne żądania nowego replice podstawowego. Aby uzyskać więcej informacji, zobacz [Konfigurowanie Usługa równoważenia obciążenia dla SQL zawsze na][sql-alwayson-ilb].

W trybie awaryjnym istniejące połączenia klienta są zamknięte. Po zakończeniu tym przełączeniu nowych połączeń będą kierowane do nowego replice podstawowego.

Jeśli aplikacji powoduje odczytuje znacznie więcej niż zapisu, możesz zwolnienie niektóre z kwerendami tylko do odczytu w replice pomocniczą. Zobacz [przy użyciu detektor nawiązywania połączenia z pomocniczym tylko do odczytu (tylko do odczytu kierowanie) replice][sql-alwayson-read-only-routing].

Testowanie wdrożenia przez [cały ręczne przejęcie awaryjne][sql-alwayson-force-failover].

### <a name="jumpbox"></a>Jumpbox

Nie zezwalaj na RDP dostęp z publicznego Internetu do maszyny wirtualne uruchamianych obciążenie pracą aplikacji. Zamiast tego RDP-SSH dostęp do te maszyny wirtualne muszą pochodzić za pośrednictwem jumpbox. Administrator loguje się do jumpbox i zarejestrować do maszyn wirtualnych z jumpbox. Jumpbox zezwala na ruch RDP z Internetu, ale tylko z znane, adresy IP whitelisted.

Umieść jumpbox, w tym samym VNet jako inne maszyny wirtualne, ale w osobnym zarządzania podsieci.

Utwórz [publiczny adres IP] dla jumpbox.

Używanie mały rozmiar pamięci Wirtualnej dla jumpbox, takich jak A1 standardowy.

Konfigurowanie NSGs warstwa, warstwa firm i podsieci Warstwa bazy danych umożliwia przekazywanie za pośrednictwem z podsieci zarządzania administracyjne ruchu (RDP).

Aby zabezpieczyć jumpbox, tworzenie NSG i zastosować go do podsieci jumpbox. Dodawanie reguły NSG, która zezwala na połączenia RDP tylko z zestawu whitelisted publicznych adresów IP.

NSG może zostać dołączony do podsieci lub jumpbox karty sieciowej. W tym przypadku zalecamy dołączanie do NIC, aby ruch RDP jest dozwolone tylko do jumpbox, nawet jeśli inne maszyny wirtualne zostanie dodany do tej samej podsieci.

## <a name="availability-considerations"></a>Zagadnienia dotyczące dostępności

Należy umieścić każdego poziomu lub roli maszyn wirtualnych w zestawie osobnych dostępności. Nie umieszczaj maszyny wirtualne z różnych poziomów w ten sam zestaw dostępności. 

Na warstwie bazy danych o wielu maszyny wirtualne nie automatycznie tłumaczyć do wysokiej dostępności bazy danych. Relacyjne bazy danych należy zwykle przy użyciu replikacji i pracy awaryjnej wysokiej dostępności. Dla programu SQL Server, zaleca się używanie [Zawsze na grupy dostępności][sql-alwayson]. 

Jeśli potrzebujesz wyższej dostępności niż [Azure Umowa dotycząca poziomu usług dla maszyny wirtualne] [ vm-sla] udostępnia, powielić ją w dwóch regionów i za pomocą Menedżera ruch Azure do przełączania awaryjnego. Aby uzyskać więcej informacji, zobacz [Uruchamianie maszyny wirtualne systemu Windows w wielu regionach wysokiej dostępności][multi-dc].   

## <a name="security-considerations"></a>Zagadnienia dotyczące zabezpieczeń

Szyfrowanie danych na pozostałych. Za pomocą [Magazynu klucza Azure] [ azure-key-vault] do zarządzania kluczami szyfrowania bazy danych. Klucz magazynu można przechowywać kluczy szyfrowania w sprzętowe moduły zabezpieczeń (HSM). Aby uzyskać więcej informacji, zobacz [Konfigurowanie Azure klucza magazynu integracja dla programu SQL Server na maszyny wirtualne Azure] [ sql-keyvault] również zalecamy przechowywanie hasła aplikacji, takich jak parametry połączenia bazy danych, w klucza magazynu.

Rozważ dodanie sieci urządzenia wirtualnych (NVA), aby utworzyć strefy Zdemilitaryzowanej między publicznego Internetu i Azure wirtualnych sieci. NVA jest ogólne określenie wirtualnych wyposażenia, które można wykonywać zadania związane z siecią, takie jak zapora, inspekcję pakietów, inspekcji, niestandardowe routingu lub różnych innych operacji. Aby uzyskać więcej informacji, zobacz [implementacji strefy Zdemilitaryzowanej między Azure i Internet][dmz].

## <a name="scalability-considerations"></a>Zagadnienia dotyczące skalowalność

Urządzenia do równoważenia obciążenia dystrybucji ruchu w sieci do warstwy sieci web i małych firm. Skalowanie w poziomie, dodając nowe wystąpienia maszyn wirtualnych. Należy zauważyć, że można skalować warstwy sieci web i małych firm niezależnie od tego, oparte na ładowania. Aby zmniejszyć możliwych komplikacji spowodowany potrzeby utrzymania koligacji klienta, powinny być stateless maszyny wirtualne w warstwie sieci web. Należy również stateless maszyny wirtualne, hostingu warunków logicznych.

## <a name="manageability-considerations"></a>Zagadnienia dotyczące zarządzania

Uproszczone zarządzanie całego systemu przy użyciu narzędzi scentralizowane administrowanie, takich jak [Automatyzacji Azure][azure-administration], [Pakiet zarządzania operacje Microsoft][operations-management-suite], [Kuchmistrz][chef], lub [Puppet][puppet]. Te narzędzia można skonsolidować informacje diagnostyczne i kondycji przechwycone z wielu pośrednictwem SMS do przedstawiają ogólny przegląd systemu.

## <a name="solution-deployment"></a>Wdrożenie rozwiązania

Wdrożenie dla architektury odwołanie, które wykonuje następujące zalecenia jest dostępny na [Github][github-folder]. Ta architektura odwołanie zawiera warstwa, warstwa firm, warstwy danych, a także jumpbox kontrolera domeny maszyn wirtualnych i usługi Active Directory.

Architektura informacyjna mogą być rozmieszczone postępując zgodnie z instrukcjami poniżej: 

1. Kliknij przycisk poniżej.  
[! ["Wdrażanie Azure"] [1]][2]

2. Po otwarciu łącze w portalu Azure, wprowadź wartości Obserwuj: 
  - Nazwa **grupy zasobów** jest już zdefiniowana w pliku parametrów, więc wybierz polecenie **Utwórz nowy** i wprowadź `ra-ntier-sql-network-rg` w polu tekstowym.
  - Wybierz region w polu rozwijanym **lokalizacji** .
  - Nie należy edytować **Uri głównego szablonu** lub pól tekstowych **Uri głównego parametru** .
  - Przejrzyj warunki umowy, a następnie kliknij pole wyboru **zgodę na warunki umowy podanych powyżej** .
  - Kliknij przycisk **Kup** .

3. Zaznacz pole wyboru zakończeniu Azure powiadomienie portalu wiadomości wdrożenia.

4. Kliknij przycisk poniżej.  
[! ["Wdrażanie Azure"] [1]][3]

5. Po otwarciu łącze w portalu Azure, wprowadź wartości Obserwuj: 
  - Nazwa **grupy zasobów** jest już zdefiniowana w pliku parametrów, dlatego wybierz pozycję **Użyj istniejącej** i wprowadź `ra-ntier-sql-workload-rg` w polu tekstowym.
  - Wybierz region w polu rozwijanym **lokalizacji** .
  - Nie należy edytować **Uri głównego szablonu** lub pól tekstowych **Uri głównego parametru** .
  - Przejrzyj warunki umowy, a następnie kliknij pole wyboru **zgodę na warunki umowy podanych powyżej** .
  - Kliknij przycisk **Kup** .

6. Zaznacz pole wyboru zakończeniu Azure powiadomienie portalu wiadomości wdrożenia.

7. Kliknij przycisk poniżej.  
[! ["Wdrażanie Azure"] [1]][4]

8. Po otwarciu łącze w portalu Azure, wprowadź wartości Obserwuj: 
  - Nazwa **grupy zasobów** jest już zdefiniowana w pliku parametrów, więc wybierz polecenie **Utwórz nowy** i wprowadź `ra-ntier-sql-network-rg` w polu tekstowym.
  - Wybierz region w polu rozwijanym **lokalizacji** .
  - Nie należy edytować **Uri głównego szablonu** lub pól tekstowych **Uri głównego parametru** .
  - Przejrzyj warunki umowy, a następnie kliknij pole wyboru **zgodę na warunki umowy podanych powyżej** .
  - Kliknij przycisk **Kup** .

9. Zaznacz pole wyboru zakończeniu Azure powiadomienie portalu wiadomości wdrożenia.

10. Dołączanie plików z parametrami stałe administrator nazwy użytkowników i hasła i jego zdecydowanie zaleca się aby natychmiast zmienić na wszystkie maszyny wirtualne. Kliknij na poszczególnych maszyn wirtualnych w Azure Portal, a następnie kliknij na **Resetowanie hasła** w karta **pomocy technicznej + rozwiązywania problemów** . Zaznacz pole wyboru **Resetuj hasło** w polu listy rozwijanej **Tryb** , a następnie wybierz nową **nazwę użytkownika** i **hasło**. Kliknij przycisk **Aktualizuj** , aby nową nazwę użytkownika i hasło. 

W pliku readme w [orientacji jedno-maszyn wirtualnych] dla informacji na temat dodatkowe sposoby wdrożenia tej architektury odwołanie,[ github-folder] Github folder. 

## <a name="next-steps"></a>Następne kroki

Aby osiągnąć wysoką dostępność dla tej architektury odwołania, zaleca się [Wdrażanie do wielu regionów][multi-dc].

<!-- links -->

[arm-templates]: https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/
[azure-administration]: ../automation/automation-intro.md
[azure-audit-logs]: ../resource-group-audit.md
[azure-availability-sets]: ../virtual-machines/virtual-machines-windows-manage-availability.md#configure-each-application-tier-into-separate-availability-sets
[azure-cli]: ../virtual-machines-command-line-tools.md
[azure-key-vault]: https://azure.microsoft.com/services/key-vault.md
[azure-load-balancer]: ../load-balancer/load-balancer-overview.md
[host bastionu]: https://en.wikipedia.org/wiki/Bastion_host
[CIDR]: https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing
[chef]: https://www.chef.io/solutions/azure/
[dmz]: guidance-iaas-ra-secure-vnet-dmz.md
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier-sql
[lb-external-create]: ../load-balancer/load-balancer-get-started-internet-portal.md
[lb-internal-create]: ../load-balancer/load-balancer-get-started-ilb-arm-portal.md
[load-balancer-external]: ../load-balancer/load-balancer-internet-overview.md
[load-balancer-internal]: ../load-balancer/load-balancer-internal-overview.md
[multi-dc]: guidance-compute-multiple-datacenters.md
[multi-vm]: guidance-compute-multi-vm.md
[n-tier]: guidance-compute-n-tier-vm.md
[naming conventions]: guidance-naming-conventions.md
[nsg]: ../virtual-network/virtual-networks-nsg.md
[nsg-rules]: ../best-practices-resource-manager-security.md#network-security-groups
[operations-management-suite]: https://www.microsoft.com/en-us/server-cloud/operations-management-suite/overview.aspx
[plan-network]: ../virtual-network/virtual-network-vnet-plan-design-arm.md
[private-ip-space]: https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces
[publiczny adres IP]: ../virtual-network/virtual-network-ip-addresses-overview-arm.md
[puppet]: https://puppetlabs.com/blog/managing-azure-virtual-machines-puppet
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[sql-alwayson]: https://msdn.microsoft.com/en-us/library/hh510230.aspx
[sql-alwayson-force-failover]: https://msdn.microsoft.com/en-us/library/ff877957.aspx
[sql-alwayson-getting-started]: https://msdn.microsoft.com/en-us/library/gg509118.aspx
[sql-alwayson-ilb]: https://blogs.msdn.microsoft.com/igorpag/2016/01/25/configure-an-ilb-listener-for-sql-server-alwayson-availability-groups-in-azure-arm/
[sql-alwayson-listeners]: https://msdn.microsoft.com/en-us/library/hh213417.aspx
[sql-alwayson-read-only-routing]: https://technet.microsoft.com/en-us/library/hh213417.aspx#ConnectToSecondary
[sql-keyvault]: ../virtual-machines/virtual-machines-windows-ps-sql-keyvault.md
[vm-planned-maintenance]: ../virtual-machines/virtual-machines-windows-planned-maintenance.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines
[vnet faq]: ../virtual-network/virtual-networks-faq.md
[wsfc-whats-new]: https://technet.microsoft.com/windows-server-docs/failover-clustering/whats-new-in-failover-clustering
[Nagios]: https://www.nagios.org/
[Zabbix]: http://www.zabbix.com/
[Icinga]: http://www.icinga.org/
[VM-sizes]: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/deploy-reference-architecture.sh
[vnet-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/virtualNetwork.parameters.json
[vnet-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/virtualNetwork.parameters.json
[nsg-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/networkSecurityGroups.parameters.json
[nsg-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/networkSecurityGroups.parameters.json
[webtier-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/webTier.parameters.json
[webtier-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/webTier.parameters.json
[businesstier-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/businessTier.parameters.json
[businesstier-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/businessTier.parameters.json
[datatier-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/dataTier.parameters.json
[datatier-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/dataTier.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[0]: ./media/blueprints/compute-n-tier.png "Architektura N warstwowych za pomocą Microsoft Azure"
[1]: ./media/blueprints/deploybutton.png 
[2]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier-sql%2FvirtualNetwork.azuredeploy.json
[3]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier-sql%2Fworkload.azuredeploy.json
[4]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier-sql%2Fsecurity.azuredeploy.json