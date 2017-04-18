<properties
   pageTitle="Wytyczne techniczne: odzyskiwania błędy lokalne platformy Azure | Microsoft Azure"
   description="Artykuł na opis i projektowanie mechanizm, wysokiej dostępności, odporność na uszkodzenia aplikacji, a także planowania awarii dotyczyła błędy lokalne w Azure."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="azure-resiliency-technical-guidance-recovery-from-local-failures-in-azure"></a>Wytyczne techniczne Azure elastyczność: odzyskiwania błędy lokalne platformy Azure

Istnieją dwa podstawowe zagrożenia dostępności aplikacji:

* Błąd urządzeń, takich jak dyski i serwerów
* Nasycenia krytyczne zasoby, takie jak obliczeń warunkach obciążenia Szczyt

Azure udostępnia kombinacją zarządzania zasobami, elastyczność, równoważenia obciążenia i podziału umożliwiające wysoką dostępność w następujących okolicznościach. Niektóre z tych funkcji odbywa się automatycznie dla wszystkich usług Azure. Jednak w niektórych przypadkach deweloper aplikacji należy wykonać kilka dodatkowych akcji w celu korzystania z nich.

##<a name="cloud-services"></a>Usług w chmurze

Azure usług w chmurze składa się z kolekcji jednego lub większej liczby ról sieci web lub pracownika. Co najmniej jeden wystąpienia roli można uruchomić jednocześnie. Konfiguracja określa liczbę wystąpień. Rola wystąpienia są monitorowane i zarządzane przez składnik o nazwie kontroler tkaninie. Kontroler tkaninie wykrywa i automatycznie odpowiada błędy sprzętu i oprogramowania.

Każde wystąpienie roli działa w osobnym maszyn wirtualnych (maszyn wirtualnych) i komunikuje się z jej kontroler tkaninie za pośrednictwem agenta gościa. Agent gościa zbiera zasobów i węzeł miar, w tym maszyn wirtualnych zastosowania, stan, dzienniki, użycie zasobu, wyjątki i warunków uszkodzenia. Kontroler tkaninie kwerend agenta gościa w odstępach można konfigurować i ponownego uruchomienia maszyn wirtualnych jeśli agent gościa nie odpowiada. W przypadku awarii sprzętu kontroler tkaninie skojarzone powoduje przeniesienie wszystkich wystąpień dotyczy roli do nowego węzła sprzętu i ponownie konfiguruje sieci w celu rozsyłania ruchu.

Aby skorzystać z tych funkcji, deweloperów powinny zapewnić, że wszystkie role usługi nie należy przechowywać stanu w wystąpieniach roli. Zamiast tego wszystkie dane trwałych powinni mieć dostęp z magazynu trwałych, takich jak magazyn Azure lub bazy danych SQL Azure. Dzięki temu ról do obsługi żądań. Oznacza to również, że wystąpienia roli można przejść w dół w dowolnym czasie, bez tworzenia niespójności w stanie przejściowych lub trwałych usługi.

Wymóg przechowywane stanie zewnętrznie do ról ma wpływ kilka. Zakłada, na przykład, że wszystkie zmiany powiązanej z tabelą programu Magazyn Azure należy zmienić w ramach jednej transakcji grupa jednostek, jeśli to możliwe. Oczywiście nie zawsze jest możliwe wprowadzanie wszystkich zmian w ramach jednej transakcji. Należy zwrócić szczególną uwagę, aby upewnić się, że roli wystąpienia awarii nie powoduje problemy podczas ich przerywanie długotrwałych operacji, obejmujące dwa lub więcej aktualizacji trwała stan usługi. Jeśli inną rolę spróbuje ponów próbę takie działanie, powinien przewiduje się i obsługę w przypadku miejsce, w którym praca została wykonana częściowo.

Na przykład można rozważyć usługa, która partycje danych przez wiele sklepów. Jeśli roli Pracownik awarii podczas jest przenoszenie shard, przeniesienie shard nie może zakończyć się pomyślnie. Lub przeniesienie mogą być powtarzane od jej rozpoczęcia według roli innego pracownika może powodować danych lub uszkodzenia danych. Aby uniknąć problemów długotrwałych operacji muszą być co najmniej jedną z następujących czynności:

 * *Idempotent*: powtarzalnych bez strony efekty. Należy idempotent operacji długim powinien mieć ten sam efekt, niezależnie od tego, ile razy wykonania, nawet wtedy, gdy zostanie przerwany podczas wykonywania.
 * *Stopniowo ponownego uruchamiania*: Możesz kontynuować od punktu to najnowszy błąd. Się stopniowo ponownego uruchamiania operacji długim powinny się składać z sekwencji mniejszych Atomowej operacji. Go należy również rejestrowanie postępu w magazynie trwałe aby każdego kolejne wywołania przejmuje, gdzie zatrzymana poprzednik.

Na koniec wszystkie operacje długotrwałe powinny być używane wielokrotnie, dopóki nie są poprawne. Na przykład operacji obsługi administracyjnej może być umieszczone w kolejce Azure, a następnie usunięta z kolejki według roli Pracownik tylko wtedy, gdy skutku. Śmieci może być konieczne do oczyszczania danych przerwane operacje tworzenia.

###<a name="elasticity"></a>Elastyczność

Numer początkowy wystąpień uruchamianie dla poszczególnych ról jest określona w konfiguracji poszczególnych ról. Administratorzy początkowo należy skonfigurować każdej roli uruchomienie z dwa lub więcej obiektów według oczekiwanych ładowania. Ale można łatwo skalować wystąpienia ról w górę lub w dół jako zastosowania desenie Zmień. Zrobić to ręcznie w portalu Azure lub proces można zautomatyzować przy użyciu programu Windows PowerShell, interfejs API usługi zarządzania lub narzędzi innych firm. Aby uzyskać więcej informacji zobacz [jak autoscale aplikacji](../cloud-services/cloud-services-how-to-scale.md).

###<a name="partitioning"></a>Podziału

Kontroler Azure tkaninie używa dwa typy podziałów:

* *Aktualizowanie domeny* jest używana do uaktualniania usługi roli wystąpienia w grupach. Azure wdraża wystąpienia usług do wiele domen aktualizacji. Aktualizacji w miejscu kontroler tkaninie połączenie zawartych w dół wszystkie wystąpienia w domenie jedna aktualizacja, aktualizuje je i ponownie uruchamiane przed przejściem do następnej domeny aktualizacji. Ta metoda uniemożliwia całej usługi niedostępne podczas procesu aktualizacji.
* *Błąd domeny* określa potencjalnych punktów awarii sprzętu lub sieci. Dla dowolnego roli, która ma więcej niż jedno wystąpienie kontroler tkaninie zapewnia wystąpienia są rozmieszczane wiele domen błędów, aby uniemożliwić awarie sprzętu odizolowanych zakłóca działania usługi. Błąd domen określają wszystkie narażenie na serwer i klaster błędy.

[Azure Umowa dotycząca poziomu usług (SLA)](https://azure.microsoft.com/support/legal/sla/) gwarantuje, że gdy dwa lub więcej obiektów roli sieci web są rozmieszczane różnych błędów i uaktualnianie domen, będą mieli połączeniami zewnętrznymi 99,95 procent czasu. W przeciwieństwie do domen aktualizacji jest sposobem decyduje o liczbie domen błędów. Azure automatycznie przydziela domen błędów i rozdziela wystąpienia ról w ich. Na co najmniej dwa wystąpienia każdej roli są umieszczane w różnych błędów i uaktualnianie domen, aby upewnić się, że wszelkie rolę z co najmniej dwa wystąpienia będzie odpowiadał Umowa dotycząca poziomu usług. Jest to przedstawione na poniższym diagramie.

![Uproszczony widok izolacji domeny błędów](./media/resiliency-technical-guidance-recovery-local-failures/partitioning-1.png)

###<a name="load-balancing"></a>Równoważenia obciążenia

Cały ruch przychodzący do ról w sieci web przechodzi przez usługi równoważenia obciążenia bezstanowym, które rozdzielanie żądań klientów między wystąpienia roli. Rola poszczególnych wystąpień nie mają publiczne adresy IP, a nie są bezpośrednio adresach z Internetu. Role w sieci Web są bezstanowe, dzięki czemu jakiekolwiek żądanie klienta można kierowane do każdego wystąpienia roli. Zdarzenie [StatusCheck](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.statuscheck.aspx) powstaje co 15 sekund. Umożliwia wskazują, czy jest gotowy do odbierania ruchu roli lub czy jest zajęty i powinien zostać wyłączony z obrotu równoważenia obciążenia.

##<a name="virtual-machines"></a>Maszyn wirtualnych

Azure maszyn wirtualnych różni się od platformy zgodnie z usługą (PaaS) obliczyć ról w pod kilkoma względami względem wysokiej dostępności. W niektórych przypadkach należy dodatkowych akcji w celu zapewnienia wysokiej dostępności.

###<a name="disk-durability"></a>Wytrzymałości dysku

W przeciwieństwie do wystąpienia roli PaaS danych przechowywanych na dyskach maszyn wirtualnych jest trwała, nawet wtedy, gdy jest przenoszony maszyny wirtualnej. Azure maszyn wirtualnych dysków maszyn wirtualnych istniejące jako obiektów blob w magazynie Azure. Ze względu na cech dostępności magazyn Azure danych przechowywanych na dyskach maszyny wirtualnej jest również bardzo dostępne.

Należy zauważyć, że dysku D (w maszyny wirtualne Windows) wyjątek od tej reguły. Dysku D jest faktycznie fizycznie miejsca do magazynowania na serwerze stojaków, obsługującym maszyn wirtualnych i jego dane zostaną utracone po usunięciu maszyn wirtualnych. Dysku D jest przeznaczona do tymczasowego przechowywania tylko. W Linux Azure "zwykle" (ale nie zawsze) udostępnia tymczasowe dysku jako /dev/sdb blok urządzenie. Często jest zainstalowany przez agenta Linux Azure w postaci /mnt/resource lub katalogu/mnt punktów instalacji (można konfigurować za pomocą /etc/waagent.conf).

###<a name="partitioning"></a>Podziału

Azure oryginalnie rozumie poziomów w aplikacji PaaS (ról w sieci web i roli Pracownik) i więc prawidłowo rozpowszechniania ich domen usterek i aktualizacji. Natomiast poziomów w infrastrukturze jako aplikacji usługi (IaaS) muszą być ręcznie zdefiniowane przez zestawami dostępności. Zestawy dostępności są wymagane na potrzeby SLA w obszarze IaaS.

![Dostępność ustawia Azure maszyn wirtualnych](./media/resiliency-technical-guidance-recovery-local-failures/partitioning-2.png)

Na powyższym diagramie warstwie Internetowe usługi informacyjne (IIS) (która działa jako warstwa aplikacji sieci web) i warstwie SQL (która działa jako warstwy danych) są przypisywane do różnych dostępność zestawów. Dzięki temu wszystkie wystąpienia każdej warstwy że nadmiarowości sprzętu przekazując maszyn wirtualnych w domenach błędów i całego poziomów nie podjęcie w dół podczas aktualizacji.

###<a name="load-balancing"></a>Równoważenia obciążenia

Jeśli maszyny wirtualne powinien mieć ruch rozkładany je, możesz grupować maszyny wirtualne w aplikacji i ładowanie równowagi między określonego punktu końcowego TCP i UDP. Aby uzyskać więcej informacji zobacz [maszyn wirtualnych równoważenia obciążenia](../virtual-machines/virtual-machines-linux-load-balance.md). Jeśli maszyny wirtualne wejściowych z innego źródła (na przykład Kolejkowanie mechanizm), równoważenia obciążenia nie jest wymagane. Usługi równoważenia obciążenia użyto podstawowe kontroli, aby ustalić, czy ruch powinny trafiać do węzła. Użytkownik może również tworzyć własne sondy do wykonania metryki kondycji specyficzne dla aplikacji, stwierdzić, czy maszyn wirtualnych otrzymujących ruch.

##<a name="storage"></a>Miejsca do magazynowania

Azure magazyn jest usługa Azure trwałymi danymi według planu bazowego. Umożliwia obiektów blob, tabeli kolejki i maszyn wirtualnych ilość miejsca do magazynowania. Aby zapewnić wysoką dostępność w jednym centrum danych użyto kombinację replikacji i zarządzania zasobami. Dostępność magazyn Azure SLA gwarantuje, że co najmniej 99,9% czasu:

* Poprawnie sformatowany żąda, aby dodać, aktualizacji, odczytu i usuwania danych zostanie pomyślnie i poprawnie przetworzony.
* Miejsca do magazynowania konta będą mieć połączenie bramy internetowej.

###<a name="replication"></a>Replikacji

Azure magazynowania ułatwia wytrzymałości danych przez obsługę wielu kopii wszystkich danych na różnych dyskach w podsystemów całkowicie niezależne magazyn fizyczny w regionie. Dane mają być replikowane synchronicznie, a wszystkie kopie są Projekt zatwierdzony —, zanim zostanie potwierdzone zapisu. Azure magazyn jest zdecydowanie spójne, co oznacza gwarancję Odczyt, aby odzwierciedlała najnowsze zapisy. Ponadto kopie danych są stale zeskanowany do wykrywania zaprojektowana bitowej, często pomijany zagrożenia dla integralności przechowywanych danych.

Usługi korzystać z replikacji tylko przy użyciu magazyn Azure. Projektanta usługi nie wymaga dodatkowej pracy, aby odzyskać lokalne awarii.

###<a name="resource-management"></a>Zarządzanie zasobami

Konta miejsca do magazynowania utworzone po maja 2014 można powiększać, aby maksymalnie 500 TB (maksymalnie poprzedniego był 200 TB). Jeśli wymagane jest dodatkowe miejsce, muszą być zaprojektowane aplikacje za pomocą wielu kont miejsca do magazynowania.

###<a name="virtual-machine-disks"></a>Dyski maszyn wirtualnych

Dysk maszyny wirtualnej jest przechowywana jako obiektu blob strony w magazynie Azure, podając go te same właściwości wytrzymałości i skalowalność jako magazyn obiektów Blob. Ten projekt umożliwia dane na dysku komputera wirtualnych trwałe, nawet jeśli serwerem maszyn wirtualnych kończy się niepowodzeniem i maszyn wirtualnych wymaga ponownego uruchomienia na innym serwerze.

##<a name="database"></a>Bazy danych

###<a name="sql-database"></a>Baza danych SQL

Baza danych SQL Azure udostępnia bazę danych jako usługa. Umożliwia aplikacjom szybko obsługi administracyjnej, Wstaw danych i kwerend relacyjnych baz danych. Udostępnia wiele znanych funkcji programu SQL Server i funkcji, podczas dostępnego obciążenia sprzętu, konfiguracji, poprawianie i elastyczność.

>[AZURE.NOTE] Baza danych SQL Azure nie zapewnić zgodność funkcji jeden z programem SQL Server. Jest ona przeznaczona do zrealizowania innego zestawu wymagania — jeden, który ma jednoznacznie dopasowane do aplikacje w chmurze (skala elastyczne, bazę danych jako usługi, aby zmniejszyć koszty obsługi i tak dalej). Aby uzyskać więcej informacji, zobacz [Wybierz chmury opcja programu SQL Server: bazy danych SQL Azure (PaaS) lub SQL Server na Azure maszyny wirtualne (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md).

####<a name="replication"></a>Replikacji

Baza danych SQL Azure zawiera wbudowane elastyczności błąd poziomu węzła. Wszystkie zapisy do bazy danych są automatycznie replikowane na dwa lub więcej węzłów tła za pomocą techniki Zatwierdź kworum. (Podstawowy i pomocniczy co najmniej jeden musi potwierdzić, że działania są zapisywane dziennik transakcji przed transakcji uważa, że pomyślnie i zwraca.) W przypadku awarii węzła bazy danych automatycznie przełącza do jednej z replik pomocniczą. Spowoduje to przerwanie połączeń przejściowych, w przypadku aplikacji klienckich. Z tego powodu wszystkich klientów bazy danych SQL Azure zaimplementować niektórych formularza obsługi połączeń przejściowych. Aby uzyskać więcej informacji zobacz [Ponów próbę wskazówki określonych usług](../best-practices-retry-service-specific.md).

####<a name="resource-management"></a>Zarządzanie zasobami

Każdej bazy danych po utworzeniu skonfigurowano limit rozmiaru górnym. Jest obecnie dostępna maksymalny rozmiar wynosi 1 TB (rozmiar limity różnią się według poziomu usług, zobacz [warstwy usługi i poziomy wydajności baz danych SQL Azure](../sql-database/sql-database-resource-limits.md#service-tiers-and-performance-levels). Gdy bazy danych trafienia limit rozmiaru górnym, odrzuci dodatkowych poleceń INSERT lub UPDATE. (Kwerend i usuwanie danych jest nadal możliwe).

W bazie danych bazy danych SQL Azure używa tkaninie do zarządzania zasobami. Jednak zamiast kontrolera tkaninie używa topologii pierścienia wykryto błędy. Każdej replice w klastrze ma dwóch sąsiadów i jest odpowiedzialny za wykrywanie, gdy znajdzie się w dół. Gdy replice przechodzi w dół, wokół wyzwalanie agenta zmiany konfiguracji, aby odtworzyć go na innym komputerze. Aby upewnić się, że serwer logiczne nie używać zbyt wiele zasobów na komputerze lub przekraczają limity fizycznie na komputerze znajduje się ograniczania aparat.

###<a name="elasticity"></a>Elastyczność

Jeśli aplikacja wymaga więcej niż 1 TB limitu bazy danych, należy go zaimplementować podejście skala w nowym oknie. To skalować się z bazą danych SQL Azure ręcznie podział, nazywane także sharding danych w wielu baz danych programu SQL. Tej metody poza skalowanie umożliwia uzyskanie wzrost kosztów prawie liniowy ze skalą. Elastyczne wzrostu lub pojemność na żądanie można powiększać z dodatkowe koszty, stosownie do potrzeb, ponieważ baz danych są wystawiona na podstawie średnia rzeczywistego rozmiaru używane dziennie, nie są oparte na maksymalny rozmiar.

##<a name="sql-server-on-virtual-machines"></a>Program SQL Server w środowisku maszyn wirtualnych systemu

Instalując program SQL Server (wersja 2014 lub nowsza) na maszyn wirtualnych Azure, można korzystać z funkcji tradycyjnych dostępność programu SQL Server. Te funkcje obejmują grupy dostępności (AlwaysOn) i odzwierciedlające bazy danych. Należy zauważyć, że maszyny wirtualne Azure, miejsca do magazynowania i sieci różne właściwości operacyjnych niż w lokalnym obsługą infrastrukturę informatyczną. Pomyślnego wysoka dostępność i odzyskiwanie rozwiązanie programu SQL Server (HA/DR) platformy Azure wymaga zrozumienie różnic i projektowanie rozwiązania do ich potrzeb.

###<a name="high-availability-nodes-in-an-availability-set"></a>Węzły wysokiej dostępności w zestawie dostępności

Podczas implementowania rozwiązania wysokiej dostępności platformy Azure, możesz za pomocą dostępność w Azure umieścić węzły wysokiej dostępności w osobnym błędów domen i uaktualnianie domen. Aby mieć Wyczyść, zestaw dostępność jest Azure pojęcia. Jest dobrym rozwiązaniem, które należy wykonać, aby upewnić się, że baz danych dostępnych w rzeczywistości wysoce, czy korzystasz z grupy dostępności (AlwaysOn), odzwierciedlające bazy danych lub czegoś innego. Jeśli nie wykonasz tej najważniejszej wskazówki, może być w obszarze FAŁSZ założeniu, że system jest wysokiej dostępności. Jednak w rzeczywistości węzły mogą nie powiedzie się jednocześnie ponieważ wystąpią mają być umieszczone w tej samej domeny błędów w regionie Azure.

Zalecenie nie dotyczy odpowiednio z wysyłką dziennika. Jako funkcja odzyskiwanie danych należy upewnić się, że serwerów są uruchomione w osobnym regionach Azure. Zgodnie z definicją te regiony są domenami osobnych błędów.

W przypadku Azure chmury usługi maszyny wirtualne wdrożony za pośrednictwem portalu klasyczny w ten sam zestaw dostępność należy wdrożyć je w tym samym usługi w chmurze. Maszyny wirtualne wdrożony za pomocą Menedżera zasobów Azure (portal bieżącego) nie ma to ograniczenie. W portalu klasycznym wdrożeniu maszyny wirtualne w usłudze w chmurze Azure tylko węzłów na tym samym usługi w chmurze mogą uczestniczyć w ten sam zestaw dostępności. Ponadto pośrednictwem chmury usługi SMS powinny być w tej samej sieci wirtualnej zapewniające zachowanie ich adresy IP nawet po korygujący usługi. Pozwala to uniknąć zakłóceń aktualizacji DNS.

###<a name="azure-only-high-availability-solutions"></a>Tylko do Azure: rozwiązań wysokiej dostępności

Możesz mieć rozwiązanie wysokiej dostępności baz danych programu SQL Server Azure za pomocą grupy dostępności (AlwaysOn) lub odzwierciedlające bazy danych.

Poniższy diagram przedstawia architektura grupy dostępności (AlwaysOn) uruchomione na maszyn wirtualnych Azure. Ten schemat został pobrany z artykułu ze szczegółowymi informacjami na ten temat, [wysokiej dostępności i odzyskiwanie w przypadku programu SQL Server na maszyn wirtualnych Azure](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

![Grupy dostępności (AlwaysOn) platformy Microsoft Azure](./media/resiliency-technical-guidance-recovery-local-failures/high_availability_solutions-1.png)

Można również automatycznie obsługi administracyjnej grupy dostępności (AlwaysOn) wdrożenia końca do końca na maszyny wirtualne Azure za pomocą szablonu (AlwaysOn) w portalu Azure. Aby uzyskać więcej informacji zobacz [Oferowanie (AlwaysOn) w programie SQL Server w galerii Portal Azure firmy Microsoft](https://blogs.technet.microsoft.com/dataplatforminsider/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery/).

Poniższy diagram przedstawia wykorzystanie na maszyn wirtualnych Azure odzwierciedlające bazy danych. Podjęto również w temacie szczegółowo [wysoką dostępność i odzyskiwanie w przypadku programu SQL Server na maszyn wirtualnych Azure](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

![Baza danych odzwierciedlające platformy Microsoft Azure](./media/resiliency-technical-guidance-recovery-local-failures/high_availability_solutions-2.png)

>[AZURE.NOTE] Oba architektury wymagają kontrolera domeny. Jednak z odzwierciedlające bazy danych, istnieje możliwość korzystania z certyfikatów serwera potrzebują dla kontrolera domeny.

##<a name="other-azure-platform-services"></a>Inne usługi platformy Azure

Aplikacje zaprojektowane Azure korzystać z możliwości platformy, aby naprawić błędy lokalne. W niektórych przypadkach może potrwać określonych akcji, aby zwiększyć dostępność dla określonego rozwiązania.

###<a name="service-bus"></a>Usługa Bus

Aby ograniczyć awaria tymczasowe Bus usługi Azure, rozważ utworzenie trwałe kolejki po stronie klienta. To tymczasowo przechowuje mechanizm alternatywnej, lokalnego przechowywania wiadomości, których nie można dodawać do kolejki Bus usługi. Aplikacja można zdecydować, jak obsługiwać tymczasowo przechowywanych wiadomości po przywróceniu usługi. Aby uzyskać więcej informacji zobacz [Najważniejsze wskazówki dotyczące ulepszenia dotyczące wydajności przy użyciu usługi Bus brokered wiadomości](../service-bus-messaging/service-bus-performance-improvements.md) i [Usługa Bus (odzyskiwanie)](./resiliency-technical-guidance-recovery-loss-azure-region.md#other-azure-platform-services).

###<a name="hdinsight"></a>Usługa HDInsight

Domyślnie w magazynie obiektów Blob platformy Azure znajduje się danych, który jest skojarzony z usługi HDInsight Azure. Azure magazynowania określa właściwości wysokiej dostępności i wytrzymałości magazyn obiektów Blob. Przetwarzanie wiele węzłów, który jest skojarzony z Hadoop MapReduce zadania przypada na przejściowych pliku usługi Hadoop Distributed System (HDFS) który jest zainicjowany, gdy HDInsight wymaga go. Wyniki z zadania MapReduce również są przechowywane domyślnie w magazynie obiektów Blob platformy Azure, tak, aby przetworzone dane jest trwały i pozostaje wysokiej dostępności, po wstrzymano obsługę administracyjną klaster Hadoop. Aby uzyskać więcej informacji zobacz [HDInsight (odzyskiwanie)](./resiliency-technical-guidance-recovery-loss-azure-region.md#other-azure-platform-services).

##<a name="checklists-for-local-failures"></a>Listy kontrolne błędy lokalne

###<a name="cloud-services"></a>Usług w chmurze

  1. Zapoznaj się z sekcją usług w chmurze tego dokumentu.
  2. Skonfiguruj co najmniej dwa wystąpienia dla poszczególnych ról.
  3. Stanu w magazynie trwałe, nie ma na wystąpienia roli utrwalenia.
  4. Nieprawidłowo obsługuje zdarzenia StatusCheck.
  5. Zawijanie powiązanych zmian w transakcji, jeśli jest to możliwe.
  6. Sprawdź, czy zadania roli Pracownik idempotent i ponownego uruchamiania.
  7. Przejdź do wywołania operacji, dopóki nie są poprawne.
  8. Należy rozważyć strategii autoscaling.

###<a name="virtual-machines"></a>Maszyn wirtualnych

  1. Zapoznaj się z sekcją maszyn wirtualnych tego dokumentu.
  2. Nie należy używać dysku D wygenerowanie.
  3. Komputery w warstwie usługi można grupować zestawu dostępności.
  4. Konfigurowanie równoważenia obciążenia i opcjonalnie sondy.

###<a name="storage"></a>Miejsca do magazynowania

  1. Zapoznaj się z sekcją miejsca do magazynowania w niniejszym dokumencie.
  2. Za pomocą wielu kont miejsca do magazynowania danych lub przepustowości przekracza przydziałów.

###<a name="sql-database"></a>Baza danych SQL

  1. Zapoznaj się z sekcją bazy danych SQL tego dokumentu.
  2. Wdrożenie zasady ponów próbę do obsługi błędów przejściowych.
  3. Użyj podziału sharding jako strategii skala w nowym oknie.

###<a name="sql-server-on-virtual-machines"></a>Program SQL Server w środowisku maszyn wirtualnych systemu

  1. Przejrzyj programu SQL Server w środowisku maszyn wirtualnych systemu części tego dokumentu.
  2. Wykonaj powyższe zalecenia dla maszyn wirtualnych.
  3. Za pomocą programu SQL Server dostępności, takie jak (AlwaysOn).

###<a name="service-bus"></a>Usługa Bus

  1. Zapoznaj się z sekcją Bus usługi tego dokumentu.
  2. Rozważ utworzenie trwałe kolejki po stronie klienta jako kopii zapasowej.

###<a name="hdinsight"></a>Usługa HDInsight

  1. Zapoznaj się z sekcją HDInsight tego dokumentu.
  2. Nie dostępność dodatkowe czynności są wymagane do błędy lokalne.

##<a name="next-steps"></a>Następne kroki

Ten artykuł jest częścią serii ograniczony do [wytycznych technicznych elastyczność Azure](./resiliency-technical-guidance.md). Następny artykuł z tej serii jest [odzyskiwania zakłócenia usługi region](./resiliency-technical-guidance-recovery-loss-azure-region.md).
