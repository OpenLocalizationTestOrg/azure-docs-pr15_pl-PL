<properties
   pageTitle="Uruchamianie maszyn wirtualnych Linux oraz dla architektury N warstwowych Azure | Microsoft Azure"
   description="Jak uruchomić maszyny wirtualne Linux oraz dla architektury N warstwowych platformy Microsoft Azure?"
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
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

# <a name="running-linux-vms-for-an-n-tier-architecture-on-azure"></a>Uruchamianie maszyn wirtualnych Linux oraz dla architektury N warstwowych Azure

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]


> [AZURE.SELECTOR]
- [Uruchamianie maszyn wirtualnych Linux oraz dla architektury N warstwowych Azure](guidance-compute-n-tier-vm-linux.md)
- [Uruchamianie maszyn wirtualnych systemu Windows dla architektury N warstwowych Azure](guidance-compute-n-tier-vm.md)

W tym artykule omówiono zestaw sprawdzone wskazówki dotyczące systemem Linux wirtualnych maszyn dla aplikacji z architektury N warstwy. Opiera się na artykuł [Uruchamianie wielu maszyn wirtualnych Azure][multi-vm].

> [AZURE.NOTE] Azure ma dwa różne wdrożenia modele: [Menedżer zasobów] [ resource-manager-overview] i klasyczny. W tym artykule używa Menedżera zasobów, które firma Microsoft zaleca się w przypadku wdrożeń nowy.

## <a name="architecture-diagram"></a>Diagram architektury

Istnieje odmiany architektury N warstwowych. W większości przypadków różnice nie ma znaczenia, na potrzeby tych zaleceń. W tym artykule założono, aplikacji sieci web Typowa poziomu 3:

- **Warstwa sieci Web.** Obsługuje przychodzące żądania HTTP. Odpowiedzi są zwracane przez tego poziomu.

- **Warstwa firm.** Narzędzi procesów biznesowych i innych funkcjonalności logiki systemu.

- **Warstwa bazy danych.** Miejsce przechowywania danych trwałych, za pomocą Apache Cassandra wysokiej dostępności.

> Dokument programu Visio, który zawiera ten diagram architektury jest dostępna do pobrania w [Centrum pobierania firmy Microsoft][visio-download]. Ten diagram znajduje się na "obliczeń - wielostronicowy warstwa (Linux).

![[0]][0]

- **Zestawy dostępności.** Tworzenie [Zestawu dostępność] [ azure-availability-sets] dla każdego poziomu i obsługi administracyjnej co najmniej dwa maszyny wirtualne w każdej warstwie. Ta metoda jest wymagane do osiągnięcia dostępność [SLA] [ vm-sla] dla maszyny wirtualne.

- **Podsieci.** Utwórz osobną podsieć dla każdej warstwy. Określ adres zakresu i maskę podsieci przy użyciu notacji [CIDR] . 

- **Załaduj równoważenia.** Korzystanie [z Internetu równoważenia obciążenia] [ load-balancer-external] do dystrybucji przychodzącego ruchu internetowego warstwa i [równoważenia obciążenia wewnętrznych] [ load-balancer-internal] do dystrybucji ruchu w sieci z poziomu sieci web w warstwie firm.

- **Jumpbox**. _Jumpbox_, nazywany [host bastionu]jest maszyn wirtualnych sieci, którego administratorzy nawiązywania połączenia z innymi maszyny wirtualne. Jumpbox ma NSG, która zezwala na zdalny ruch tylko z whitelisted publicznych adresów IP. NSG należy zezwolić na ruch bezpiecznego powłoki (SSH).

- **Monitorowanie**. Monitorowanie oprogramowania, takich jak [Nagios], [Zabbix]lub [Icinga] można uzyskać czas reakcji, czas pracy maszyn wirtualnych i ogólnej kondycji systemu. Zainstaluj oprogramowanie monitorowania na maszyny umieszczonego w osobnym zarządzania podsieci.

- **NSGs**. Używanie [grup zabezpieczeń sieci] [ nsg] (NSGs), aby ograniczyć ruch sieciowy w VNet. Na przykład w architekturze poziomu 3 pokazanego warstwy bazy danych nie akceptuje ruchu z frontonu sieci web, tylko z poziomu firm i podsieć zarządzania.

- **Apache Cassandra bazy danych**. Zapewnia wysoką dostępność w warstwie danych, włączając replikacji i pracy awaryjnej.

## <a name="recommendations"></a>Zalecenia

Azure oferuje wiele różnych zasobów i typów zasobów, aby ta architektura odwołanie może być obsługi administracyjnej wiele różnych sposobów. Utworzono szablon Menedżera zasobów Azure zainstalować architektura odwołania, znajdujący się tych zaleceń. Jeśli chcesz utworzyć własny architektura odwołanie tych zaleceń bez określonej wymóg, aby zalecenie nie mieści się.

### <a name="vnet--subnets"></a>VNet-podsieci

Po utworzeniu VNet przydzielić miejsca adres dla podsieci, które będą potrzebne. Określ maskę zakres i podsieć adresów VNet przy użyciu notacji [CIDR] . Przestrzeń adresów, która znajduje się w standardowej [prywatne bloki adresów IP]za pomocą[private-ip-space], które są 10.0.0.0/8, 172.16.0.0/12 i 192.168.0.0/16.

Wybierz zakres adresów, aby nie nakładały się z siecią w wersji lokalnej, w przypadku, gdy trzeba skonfigurować bramę między VNet a siecią lokalna później. Po utworzeniu VNet, nie można zmienić zakres adresów.

Projektowanie podsieci wymagania funkcjonalności i zabezpieczeń pamiętać. Wszystkie maszyny wirtualne w tej samej roli lub warstwy należy przejść do tej samej podsieci, co może być granicę zabezpieczeń. Aby uzyskać więcej informacji o projektowaniu VNets podsieci, zobacz [Planowanie i projektowanie Azure wirtualnych sieci][plan-network].

Dla każdej podsieci Określ przestrzeni adresów podsieci w notacji CIDR. Na przykład "10.0.0.0/24" tworzy zakres 256 adresów IP. (Maszyny wirtualne można używać 251 tych; 5 są zarezerwowane. [Wirtualna sieć często zadawane pytania dotyczące][vnet faq].) Upewnij się, że zakresy adresów nie nakładały się na podsieci.

### <a name="load-balancers"></a>Urządzenia do równoważenia obciążenia

Usługi równoważenia obciążenia zewnętrznych rozdziela ruch internetowy warstwie sieci web. Tworzenie publiczny adres IP dla tej usługi równoważenia obciążenia. Zobacz [Tworzenie równoważenia obciążenia z Internetu][lb-external-create].

Usługi równoważenia obciążenia wewnętrznych rozdziela ruch sieciowy z poziomu sieci web w warstwie firm. Aby przekazać ten równoważenia obciążenia prywatny adres IP, Tworzenie konfiguracji IP serwera sieci Web i skojarzyć podsieć warstwie firm. Zobacz [Wprowadzenie do tworzenia równoważenia obciążenia wewnętrznych][lb-internal-create].

### <a name="cassandra"></a>Cassandra

Zalecamy [DataStax Enterprise] [ datastax] produkcji używanie, ale tych zaleceń dotyczą dowolnej wersji Cassandra. Aby uzyskać więcej informacji na temat uruchamiania DataStax platformy Azure, zobacz [Podręcznik wdrażania Enterprise DataStax dla Azure][cassandra-in-azure]. 

Umieszczenie maszyny wirtualne dla klastrów Cassandra w zestawie dostępność, aby zapewnić repliki Cassandra są rozmieszczane wiele domen błędów i uaktualnianie domen. Aby uzyskać więcej informacji na temat uaktualniania a domenami błędów, zobacz [Zarządzanie dostępność maszyn wirtualnych][availability-sets-manage]. 

Konfigurowanie 3 według dostępności błędów domen (wartość maksymalna). 

Konfigurowanie 18 uaktualnienia domen według dostępności. Umożliwia maksymalna liczba uaktualnienia domeny nie można nadal być równomiernie domenach błędów.   

Skonfiguruj węzły w trybie pamiętać o stojaków. Mapowanie domeny błędów stojaków w `cassandra-rackdc.properties` plik.

Nie musisz równoważenia obciążenia przed klaster. Klient nawiązuje połączenie bezpośrednio do węzła w klastrze.

### <a name="jumpbox"></a>Jumpbox

Umieść jumpbox, w tym samym VNet jako inne maszyny wirtualne, ale w osobnym zarządzania podsieci.

Utwórz [publiczny adres IP] dla jumpbox.

Używanie mały rozmiar pamięci Wirtualnej dla jumpbox, takich jak A1 standardowy.

Konfigurowanie NSGs warstwa, warstwa firm i podsieci Warstwa bazy danych umożliwia przekazywanie za pośrednictwem z podsieci zarządzania administracyjne ruchu (SSH).

Aby zabezpieczyć jumpbox, tworzenie NSG i zastosować go do podsieci jumpbox. Dodawanie reguły NSG, która zezwala na połączenia SSH tylko z zestawu whitelisted publicznych adresów IP.

NSG może zostać dołączony do podsieci lub jumpbox karty sieciowej. W tym przypadku zalecamy dołączanie do NIC, aby ruch SSH jest dozwolone tylko do jumpbox, nawet jeśli inne maszyny wirtualne zostanie dodany do tej samej podsieci.

Nie zezwalaj na SSH dostęp z publicznego Internetu do maszyny wirtualne uruchamianych obciążenie pracą aplikacji. Zamiast tego SSH dostęp do te maszyny wirtualne muszą pochodzić za pośrednictwem jumpbox. Administrator loguje się do jumpbox i zarejestrować do maszyn wirtualnych z jumpbox. Jumpbox zezwala na ruch SSH z Internetu, ale tylko z znane, adresy IP whitelisted.

## <a name="availability-considerations"></a>Zagadnienia dotyczące dostępności

Należy umieścić każdego poziomu lub roli maszyn wirtualnych w zestawie osobnych dostępności. Nie umieszczaj maszyny wirtualne z różnych poziomów w ten sam zestaw dostępności. 

Na warstwie bazy danych o wielu maszyny wirtualne nie automatycznie tłumaczyć do wysokiej dostępności bazy danych. Relacyjne bazy danych należy zwykle przy użyciu replikacji i pracy awaryjnej wysokiej dostępności.  

Jeśli potrzebujesz wyższej dostępności niż [Azure Umowa dotycząca poziomu usług dla maszyny wirtualne] [ vm-sla] udostępnia, powielić ją w dwóch regionów i za pomocą Menedżera ruch Azure do przełączania awaryjnego. Aby uzyskać więcej informacji, zobacz [Systemem Linux maszyny wirtualne w wielu regionach wysokiej dostępności][multi-dc].  

## <a name="security-considerations"></a>Zagadnienia dotyczące zabezpieczeń

Aby ograniczyć ruch między warstwy za pomocą reguł NSG. Na przykład w architekturze poziomu 3, jak pokazano powyżej warstwie sieci web komunikuje się bezpośrednio z poziomu bazy danych. Aby wymusić to, warstwy bazy danych należy zablokować ruch przychodzący z podsieci poziomu sieci web.  

  1. Tworzenie NSG i kojarzenie go do podsieci Warstwa bazy danych.

  2. Dodaj regułę, która blokuje cały ruch przychodzący z VNet. (Użyj `VIRTUAL_NETWORK` znacznika w regule.) 

  3. Dodawanie reguły o wyższym priorytecie, umożliwiająca ruchu przychodzącego z podsieci warstwa firm. Ta reguła zastępuje poprzedniego reguły, a umożliwia warstwie firm porozmawiać z poziomu bazy danych.

  4. Dodawanie reguły, która zezwala na ruch przychodzący z poziomu bazy danych warstwa podsieć. Ta reguła umożliwia komunikacji między maszyny wirtualne w warstwie bazy danych, który jest wymagany Replikacja bazy danych i pracy awaryjnej.

  5. Dodaj regułę, która zezwala na ruch SSH z podsieci jumpbox. Ta reguła umożliwia administratorom nawiązywanie połączenia z poziomu bazy danych z jumpbox.

  > [AZURE.NOTE] [Domyślne reguły] ma NSG[ nsg-rules] umożliwiające dowolnego ruchu przychodzącego z poziomu VNet. Nie można usunąć tych reguł, ale można je zmienić, tworząc reguły wyższy priorytet.

Rozważ dodanie sieci urządzenia wirtualnych (NVA), aby utworzyć strefy Zdemilitaryzowanej między publicznego Internetu i Azure wirtualnych sieci. NVA jest ogólne określenie wirtualnych wyposażenia, które można wykonywać zadania związane z siecią, takie jak zapora, inspekcję pakietów, inspekcji, niestandardowe routingu lub różnych innych operacji. Aby uzyskać więcej informacji, zobacz [implementacji strefy Zdemilitaryzowanej między Azure i Internet][dmz].

## <a name="scalability-considerations"></a>Zagadnienia dotyczące skalowalność

Urządzenia do równoważenia obciążenia dystrybucji ruchu w sieci do warstwy sieci web i małych firm. Skalowanie w poziomie, dodając nowe wystąpienia maszyn wirtualnych. Należy zauważyć, że można skalować warstwy sieci web i małych firm niezależnie od tego, oparte na ładowania. Aby zmniejszyć możliwych komplikacji spowodowany potrzeby utrzymania koligacji klienta, powinny być stateless maszyny wirtualne w warstwie sieci web. Należy również stateless maszyny wirtualne, hostingu warunków logicznych.

## <a name="manageability-considerations"></a>Zagadnienia dotyczące zarządzania

Uproszczone zarządzanie całego systemu przy użyciu narzędzi scentralizowane administrowanie, takich jak [Automatyzacji Azure][azure-administration], [Pakiet zarządzania operacje Microsoft][operations-management-suite], [Kuchmistrz][chef], lub [Puppet][puppet]. Te narzędzia można skonsolidować informacje diagnostyczne i kondycji przechwycone z wielu pośrednictwem SMS do przedstawiają ogólny przegląd systemu.

## <a name="solution-deployment"></a>Wdrożenie rozwiązania

Wdrożenie dla architektury odwołanie, które wykonuje następujące zalecenia jest dostępny na [Github][github-folder]. Ta architektura informacyjna zawiera warstwa, warstwa firm i warstwy danych.

1. Kliknij przycisk poniżej.  
[! ["Wdrażanie Azure"] [1]][2]

2. Po otwarciu łącze w portalu Azure, wprowadź wartości Obserwuj: 
  - Nazwa **grupy zasobów** jest już zdefiniowana w pliku parametrów, więc wybierz polecenie **Utwórz nowy** i wprowadź `ra-ntier-sql-network-rg` w polu tekstowym.
  - Wybierz region w polu rozwijanym **lokalizacji** .
  - Nie należy edytować **Uri głównego szablonu** lub pól tekstowych **Uri głównego parametru** .
  - Przejrzyj warunki umowy, a następnie kliknij pole wyboru **zgodę na warunki umowy podanych powyżej** .
  - Kliknij przycisk **Kup** .

3. Zaznacz pole wyboru zakończeniu Azure powiadomienie portalu wiadomości wdrożenia.

4. Dołączanie plików z parametrami stałe administrator nazwy użytkowników i hasła i jego zdecydowanie zaleca się aby natychmiast zmienić na wszystkie maszyny wirtualne. Kliknij na poszczególnych maszyn wirtualnych w Azure Portal, a następnie kliknij na **Resetowanie hasła** w karta **pomocy technicznej + rozwiązywania problemów** . Zaznacz pole wyboru **Resetuj hasło** w polu listy rozwijanej **Tryb** , a następnie wybierz nową **nazwę użytkownika** i **hasło**. Kliknij przycisk **Aktualizuj** , aby nową nazwę użytkownika i hasło.

## <a name="next-steps"></a>Następne kroki

Aby osiągnąć wysoką dostępność dla tej architektury odwołania, zaleca się [Wdrażanie do wielu regionów][multi-dc].

<!-- links -->

[azure-administration]: ../automation/automation-intro.md
[azure-availability-sets]: ../virtual-machines/virtual-machines-windows-manage-availability.md#configure-each-application-tier-into-separate-availability-sets
[availability-sets-manage]: ../virtual-machines/virtual-machines-windows-manage-availability.md
[host bastionu]: https://en.wikipedia.org/wiki/Bastion_host
[cassandra-consistency]: http://docs.datastax.com/en/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html
[cassandra-consistency-usage]: http://medium.com/@foundev/cassandra-how-many-nodes-are-talked-to-with-quorum-also-should-i-use-it-98074e75d7d5#.b4pb4alb2
[cassandra-in-azure]: https://docs.datastax.com/en/datastax_enterprise/4.5/datastax_enterprise/install/installAzure.html
[cassandra-replication]: http://www.planetcassandra.org/data-replication-in-nosql-databases-explained/
[CIDR]: https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing
[chef]: https://www.chef.io/solutions/azure/
[datastax]: http://www.datastax.com/products/datastax-enterprise
[dmz]: guidance-iaas-ra-secure-vnet-dmz.md
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier
[lb-external-create]: ../load-balancer/load-balancer-get-started-internet-portal.md
[lb-internal-create]: ../load-balancer/load-balancer-get-started-ilb-arm-portal.md
[load-balancer-external]: ../load-balancer/load-balancer-internet-overview.md
[load-balancer-internal]: ../load-balancer/load-balancer-internal-overview.md
[multi-dc]: guidance-compute-multiple-datacenters-linux.md
[multi-vm]: guidance-compute-multi-vm.md
[naming conventions]: guidance-naming-conventions.md
[nsg]: ../virtual-network/virtual-networks-nsg.md
[nsg-rules]: ../best-practices-resource-manager-security.md#network-security-groups
[operations-management-suite]: https://www.microsoft.com/en-us/server-cloud/operations-management-suite/overview.aspx
[plan-network]: ../virtual-network/virtual-network-vnet-plan-design-arm.md
[private-ip-space]: https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces
[publiczny adres IP]: ../virtual-network/virtual-network-ip-addresses-overview-arm.md
[puppet]: https://puppetlabs.com/blog/managing-azure-virtual-machines-puppet
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines
[vnet faq]: ../virtual-network/virtual-networks-faq.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[Nagios]: https://www.nagios.org/
[Zabbix]: http://www.zabbix.com/
[Icinga]: http://www.icinga.org/
[0]: ./media/blueprints/compute-n-tier-linux.png "Architektura N warstwowych za pomocą Microsoft Azure"
[1]: ./media/blueprints/deploybutton.png 
[2]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier%2Fazuredeploy.json


