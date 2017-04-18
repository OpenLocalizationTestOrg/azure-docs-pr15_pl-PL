<properties
   pageTitle="Uruchamianie maszyn wirtualnych Linux w wielu regionach wysokiej dostępności | Dokumentacja dotycząca architektura | Microsoft Azure"
   description="Jak wdrożyć maszyny wirtualne w wielu regionach Azure wysokiej dostępności i elastyczność."
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
   ms.date="10/21/2016"
   ms.author="mwasson"/>

# <a name="running-linux-vms-in-multiple-regions-for-high-availability"></a>Uruchamianie maszyn wirtualnych Linux w wielu regionach wysokiej dostępności

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

> [AZURE.SELECTOR]
- [Uruchamianie maszyn wirtualnych Linux w wielu regionach wysokiej dostępności](guidance-compute-multiple-datacenters-linux.md)
- [Uruchamianie maszyn wirtualnych systemu Windows w wielu regionach wysokiej dostępności](guidance-compute-multiple-datacenters.md)

W tym artykule zalecamy zbiór wskazówek, aby uruchomić Linux wirtualnych maszyn w wielu regionów Azure, uzyskanie dostępności i infrastruktury odzyskiwania niezawodne awarii.

> [AZURE.NOTE] Azure ma dwa różne wdrożenia modele: [Menedżer zasobów] [ resource groups] i klasyczny. W tym artykule używa Menedżera zasobów, które firma Microsoft zaleca się w przypadku wdrożeń nowy.

Architektura wielu region zapewnia większą dostępność niż wdrażanie jeden z regionów. Jeśli awaria regionalne wpływa podstawowego regionu, możesz użyć [Menedżer ruchu] [ traffic-manager] przechodzić do obszaru pomocniczą. Ta architektura pomaga Jeśli poszczególnych podsystemu aplikacji nie powiedzie się.

Istnieje kilka rozwiązań ogólne do osiągnięcia wysokiej dostępności w centrach danych:   
  
- Aktywne/pasywne z stałej gotowości. Ruch prowadzi do jednego regionu, podczas innych czeka w stanie wstrzymania. Maszyny wirtualne w regionie pomocniczej są przydzielonego i uruchomiona w każdej chwili.

- Aktywne/pasywne z zimnej wstrzymania. Takie same, ale maszyny wirtualne w regionie pomocnicza nie są przydzielane do momentu pojawienia się do przełączania awaryjnego. Tej metody koszty mniejszy uruchomić, ale ogólnie ma już przestoje podczas awarii.

- Aktywny/aktywny. Dla obu regionów są aktywne, i żądania są obciążenia zbilansowane między nimi. Jeśli jednym centrum danych jest niedostępny, jest przyjmowana poza obrotu.

Ta architektura omówiono aktywne/pasywne z stałej gotowości za pomocą Menedżera ruch do przełączania awaryjnego. Uwaga można wdrożyć niewielka liczba maszyny wirtualne dla stałej gotowości i następnie skalowania stosownie do potrzeb.

## <a name="architecture-diagram"></a>Diagram architektury

Poniższy diagram tworzy w architekturze pokazano [niezawodności dodawanie do architektury N warstwowych Azure](guidance-compute-n-tier-vm-linux.md). 

> Dokument programu Visio, który zawiera ten diagram architektury jest dostępna do pobrania w [Centrum pobierania firmy Microsoft][visio-download]. Ten diagram znajduje się na "obliczeń - wielostronicowy obszar (Linux).

![[0]][0]

- **Regiony głównego i pomocniczego**. Ta architektura używa dwóch regionów w celu osiągnięcia wyższego poziomu dostępności. Jedna jest podstawowy region. Podczas normalnego działania ruch sieciowy jest przekierowywane do obszaru podstawowego. Jednak jeśli która staje się niedostępna, ruch jest kierowane do pomocniczej region.

- ** [Menedżer ruchu azure] [ traffic-manager] ** kieruje przychodzących wezwań do obszaru podstawowego. Jeśli tego regionu jest niedostępny, ruch Menedżer nie może na pomocniczym region. Aby uzyskać więcej informacji zobacz sekcję [Konfigurowanie Menedżer ruchu](#configuring-traffic-manager).

- **Grupy zasobów**. Tworzenie osobnych [grup zasobów] [ resource groups] dla danego regionu podstawowego obszaru pomocniczej i dla Menedżera ruch. Pozwala zarządzać każdego regionu jako pojedynczy zbiór zasobów. Na przykład można ponownie wdróż jednego regionu, bez konieczności w dół do drugiego. [Łączenie grup zasobów][resource-group-links], tak aby można uruchomić kwerendę, aby wyświetlić listę wszystkich zasobów dla aplikacji.

- **VNets**. Tworzenie osobnych VNet dla każdego regionu. Upewnij się, że obszary adresów nie pokrywają się.

- **Apache Cassandra** wdrożona w danych centra między różnymi regionami Azure. Centra danych Cassandra wdrożonych w różnych regionów Azure wysokiej dostępności. W każdym obszarze węzły są skonfigurowane w trybie pamiętać o stojaków błędów i uaktualnianie domen dla elastyczność obszarze.

## <a name="recommendations"></a>Zalecenia

Azure oferuje wiele różnych zasobów i typów zasobów, aby ta architektura odwołanie może być obsługi administracyjnej wiele różnych sposobów. Utworzono szablon Menedżera zasobów Azure zainstalować architektura odwołania, znajdujący się tych zaleceń. Jeśli chcesz utworzyć własny architektura odwołanie tych zaleceń bez określonej wymóg, aby zalecenie nie mieści się.

### <a name="regional-pairing"></a>Kojarzenie regionalnych

Każdego regionu Azure wraz z innego regionu w tym samym geograficznych. Ogólnie wybierz regionów z tej samej pary regionalnego (na przykład wschodniego Stany Zjednoczone 2 i nam centralnej). Korzyści wynikające w ten sposób:

- W przypadku awarii ogólne priorytety są przypisywane odzyskiwania regionu co najmniej jedno z każdej pary.

- Aktualizacje planowanej systemu Azure wdrażania sparowany regionów kolejno, aby zminimalizować przestoje możliwe.

- Pary znajdują się w tym samym geograficzne, aby spełnić wymagania dotyczące siedziby danych.

Jednak należy się upewnić, że dla obu regionów obsługuje wszystkie wymagane dla aplikacji usługi Azure (zobacz [usługi według regionów][services-by-region]). Aby uzyskać więcej informacji na temat pary regionalne zobacz [firm ciągłości i awarii odzyskiwania (BCDR): regiony sparowane Azure][regional-pairs].

### <a name="traffic-manager-configuration"></a>Ruch Menedżera konfiguracji

Należy rozważyć następujące punkty podczas konfigurowania ruch menedżera dla danego scenariusza:

- **Routing.** Menedżer ruchu obsługuje kilka [algorytmy routingu][tm-routing]. W scenariuszu opisane w tym artykule za pomocą _priorytet_ routingu (dawniej nazywanych routingu _pracy awaryjnej_ ). Za pomocą tych ustawień, Menedżer ruchu wysyła wszystkie żądania podstawowego regionu, chyba że podstawowy obszar stanie się niedostępny. W tym momencie go automatycznie przełącza do obszaru pomocniczą. Zobacz [Konfigurowanie pracy awaryjnej metody routingu][tm-configure-failover].

- **Sonda kondycji.** Ruch przez menedżerów HTTP (lub HTTPS) [sondy] [ tm-monitoring] monitorowanie dostępność każdego regionu. Sonda sprawdza, czy odpowiedź HTTP 200 dla określonej ścieżce adresu URL. Zgodnie z zaleceniami dotyczącymi Tworzenie punktu końcowego, który zgłasza ogólny stan aplikacji i używać tego punktu końcowego do sondy kondycji. W przeciwnym razie sondy może raport "prawidłowy" punkt końcowy, gdy krytycznych części aplikacji są faktyczną kończy się niepowodzeniem. Aby uzyskać więcej informacji, zobacz [Kondycja punktu końcowego monitorowania wzorca][health-endpoint-monitoring-pattern].

Gdy Menedżer ruchu przez nie powiedzie się, to rozłożonym w czasie, gdy klienci nie mogą uzyskać dostępu aplikacji, która może potrwać kilka minut. Dwa czynniki wpływać na całkowity czas trwania:

- Sonda kondycji musi wykrywa, że centrum danych podstawowych ma stają się niedostępne.

- Serwery DNS, musisz zaktualizować pamięci podręcznej rekordy DNS dotyczące adresu IP, która jest zależna od DNS time to live (TTL). Domyślny czas TTL jest 300 sekund (5 minut), ale można skonfigurować tę wartość, podczas tworzenia profilu Menedżer ruchu.

Aby uzyskać szczegółowe informacje, zobacz [Temat monitorowania Menedżer ruchu][tm-monitoring].

Jeśli Menedżer ruchu nie powiedzie się nad, zalecamy wykonywanie powrotu ręcznego, zamiast automatycznie awarii powrotem. Sprawdź, czy wszystkie podsystemów aplikacji są najpierw prawidłowy. W przeciwnym razie możesz utworzyć sytuacji, gdy aplikacja przerzucony i z powrotem między centrami danych.

Domyślnie Menedżer ruchu automatycznie nie powiedzie się ponownie. Aby tego uniknąć, należy ręcznie obniża priorytet podstawowy obszar po zdarzenie pracy awaryjnej. Załóżmy na przykład, podstawowy obszar jest priorytet 1 i pomocniczej jest priorytet 2. Po przełączeniu Ustaw obszar podstawowy priorytet 3, aby uniemożliwić automatyczne awarii. Możesz przystąpić do przełączenia, aktualizację priorytet 1.

Następujące polecenie polecenie Azure aktualizuje priorytet:

```bat
azure network traffic-manager  endpoint set --resource-group <resource-group> --profile-name <profile>
    --name <traffic-manager-name> --type AzureEndpoints --priority 3
```    

Innym sposobem uniknięcia przerzutnika jest tymczasowo wyłączyć punkt końcowy:

```bat
azure network traffic-manager  endpoint set --resource-group <resource-group> --profile-name <profile>
    --name <traffic-manager-name> --type AzureEndpoints --status Disabled
```    

W zależności od przyczynę trybie awaryjnym, może być konieczne redploy zasobów w regionie. Przed niepowodzeniem ponownie, należy wykonać testu gotowość do działania. Test należy sprawdzić elementów takich jak:

- Maszyny wirtualne są poprawnie skonfigurowane. (Wszystkie wymagane oprogramowanie jest zainstalowany, usług IIS jest uruchomiony, itd.)

- Podsystemów aplikacji jest prawidłowy.

- Testy funkcjonalności. (Na przykład Warstwa bazy danych jest dostępne z poziomu sieci web).

### <a name="cassandra-deployment-across-multiple-regions"></a>Cassandra wdrażania na wielu regionów

Cassandra centrach danych są podziałek obciążenia: grupy powiązanych węzłów, które są skonfigurowane razem w klastrze replikacji i obciążenie pracą podziału.

Zalecamy [DataStax Enterprise] [ datastax] produkcji za pomocą. Aby uzyskać więcej informacji na temat uruchamiania DataStax platformy Azure, zobacz [Podręcznik wdrażania Enterprise DataStax dla Azure][cassandra-in-azure]. Następujące ogólne zalecenia dotyczą dowolnej wersji Cassandra.

- Przypisz publiczny adres IP do każdego węzła. Dzięki temu klastrów do komunikowania się przez regiony przy użyciu infrastruktury Azure podstawy dostarczając wysokiej wydajności w niskiej cenie.

- Bezpieczny węzły za pomocą zapory odpowiednie i konfiguracji NSG zezwolenia na ruch tylko do i z hostów znane, takich jak klienci i innych węzłach. Należy zauważyć, że Cassandra używa różnych portów dla komunikacji, OpsCenter Spark i tak dalej. Użycia portu w Cassandra, zobacz [Konfigurowanie zapory portu dostępu][cassandra-ports].

- Użyj szyfrowania SSL dla wszystkich [Węzeł Klient] [ ssl-client-node] i [węzła do węzła] [ ssl-node-node] komunikacji.

- W obszarze postępuj zgodnie z wytycznymi zawartymi w [Cassandra zalecenia](guidance-compute-n-tier-vm-linux.md#cassandra-recommendations).

## <a name="availability-considerations"></a>Zagadnienia dotyczące dostępności

Dzięki złożonych aplikacji N warstwowych nie konieczne może być powielić całą aplikację w regionie pomocniczą. Zamiast tego może po prostu replikować podsystemu krytyczne, który jest potrzebny do obsługi ciągłości.

Menedżer ruchu jest punktem możliwe niepowodzenie w systemie. Jeśli usługa nie powiedzie się, klienci nie mają dostępu aplikacji podczas przestoje. Przejrzyj [SLA Menedżer ruchu][tm-sla]i sprawdź, czy za pomocą Menedżera ruch tylko spełnia wymagań firm wysokiej dostępności. W przeciwnym razie należy rozważyć dodanie innego rozwiązania do zarządzania ruch jako awarii. Jeśli usługa Menedżer ruchu Azure kończy się niepowodzeniem, zmień rekordy CNAME w systemie DNS, aby wskazywały inne usługi zarządzania ruch. (Należy ręcznie wykonać ten krok, a aplikacja jest niedostępna, dopóki zmiany DNS są przenoszone).

Klaster Cassandra scenariuszach pracy awaryjnej brać pod uwagę zależy od poziomu spójności używane przez aplikacji, a także liczbę repliki używane. Poziomy spójności i zastosowania w Cassandra, zobacz [Konfigurowanie spójności danych] [ cassandra-consistency] i [Cassandra: liczbę węzłów są zawsze mówię do z kworum?][cassandra-consistency-usage] Dostępność danych w Cassandra zależy od poziomu spójności używane przez aplikację, a mechanizm replikacji. [Replikacja danych w NoSQL omówienie baz danych]dla replikacji w Cassandra,[cassandra-replication].

## <a name="manageability-considerations"></a>Zagadnienia dotyczące zarządzania

Gdy zaktualizujesz wdrożenia, zaktualizuj jeden region w danym momencie, aby zminimalizować możliwość globalnego awarii z niepoprawnej konfiguracji lub komunikat o błędzie w aplikacji.

Testowanie elastyczność systemu błędy. Oto kilka typowych scenariuszy błąd, aby przetestować:

- Zamknij wystąpienia maszyn wirtualnych.

- Ciśnienia zasoby takie jak Procesora i pamięci.

- Rozłączanie opóźnienie sieć.

- Ulec awarii procesów.

- Wygasa certyfikaty.

- Symulacja błędów sprzętu.

- Zamknij usługę DNS na kontrolerach domeny.

Zmierz razy odzyskiwania i upewnij się, że dostosować do własnych potrzeb biznesowych. Testowanie połączenia Tryby awaryjne, a także.

## <a name="next-steps"></a>Następne kroki

Tę serię ma ograniczony do wdrożenia wyłącznie chmury. Scenariusze przedsiębiorstwa często wymagają sieci hybrydowych połączenia sieci lokalnej z Azure wirtualną sieć. Aby dowiedzieć się, jak tworzyć sieci hybrydowe, zobacz [implementacji hybrydowych architektury sieci przy użyciu Azure i lokalnych VPN][hybrid-vpn].

<!-- Links -->

[azure-sla]: https://azure.microsoft.com/support/legal/sla/
[cassandra-in-azure]: https://academy.datastax.com/resources/deployment-guide-azure
[cassandra-consistency]: http://docs.datastax.com/en/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html
[cassandra-replication]: http://www.planetcassandra.org/data-replication-in-nosql-databases-explained/
[cassandra-consistency-usage]: https://medium.com/@foundev/cassandra-how-many-nodes-are-talked-to-with-quorum-also-should-i-use-it-98074e75d7d5#.b4pb4alb2
[cassandra-ports]: http://docs.datastax.com/en/latest-dse/datastax_enterprise/sec/secConfFirePort.html
[datastax]: https://www.datastax.com/products/datastax-enterprise
[health-endpoint-monitoring-pattern]: https://msdn.microsoft.com/library/dn589789.aspx
[hybrid-vpn]: guidance-hybrid-network-vpn.md
[regional-pairs]: ../best-practices-availability-paired-regions.md
[resource groups]: ../azure-resource-manager/resource-group-overview.md
[resource-group-links]: ../resource-group-link-resources.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[ssl-client-node]: http://docs.datastax.com/en/cassandra/2.0/cassandra/security/secureSSLClientToNode_t.html
[ssl-node-node]: http://docs.datastax.com/en/cassandra/2.0/cassandra/security/secureSSLNodeToNode_t.html
[tablediff]: https://msdn.microsoft.com/en-us/library/ms162843.aspx
[tm-configure-failover]: ../traffic-manager/traffic-manager-configure-failover-routing-method.md
[tm-monitoring]: ../traffic-manager/traffic-manager-monitoring.md
[tm-routing]: ../traffic-manager/traffic-manager-routing-methods.md
[tm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/traffic-manager/v1_0/
[traffic-manager]: https://azure.microsoft.com/en-us/services/traffic-manager/
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vnet-dns]: ../virtual-network/virtual-networks-manage-dns-in-vnet.md
[vnet-to-vnet]: ../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[vpn-gateway]: ../vpn-gateway/vpn-gateway-about-vpngateways.md
[wsfc]: https://msdn.microsoft.com/en-us/library/hh270278.aspx
[0]: ./media/blueprints/compute-multi-dc-linux.png "Architektura wysokiej dostępności sieci Azure N warstwowych aplikacji"
