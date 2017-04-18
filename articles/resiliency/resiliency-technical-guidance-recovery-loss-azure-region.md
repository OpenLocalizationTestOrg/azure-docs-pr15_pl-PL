<properties
   pageTitle="Elastyczność odzyskiwania przed utratą wytycznych technicznych Azure region | Microsoft Azure"
   description="Artykuł w sprawie opis i projektowanie aplikacji na uszkodzenia mechanizm, wysokiej dostępności, błędów, a także planowania awarii"
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

#<a name="azure-resiliency-technical-guidance-recovery-from-a-region-wide-service-disruption"></a>Wytyczne techniczne Azure elastyczność: odzyskiwania zakłócenia usługi regionu

Azure podzielić fizycznie i logicznie jednostki o nazwie regionów. Obszar składa się z co najmniej jeden centrach danych w pobliżu. Podczas pisania tego dokumentu Azure ma 24 regionów świata.

W rzadkich okolicznościach jest możliwe, że urządzeń w całym regionie może stać się niedostępne, na przykład z powodu awarii sieci. Lub urządzenia mogą być utracone całkowicie, na przykład z powodu naturalne awarii. W tej sekcji wyjaśniono możliwości Azure do tworzenia aplikacji, które są rozmieszczane regionów. Taki podział pomaga ograniczyć możliwość, że awarię w jednym regionie może mieć wpływ na pozostałe regiony.

##<a name="cloud-services"></a>Usług w chmurze

###<a name="resource-management"></a>Zarządzanie zasobami

Należy rozpowszechnić wystąpienia obliczeń między różnymi regionami, tworząc usługi w chmurze osobnych w każdym regionie docelowej, a następnie publikowanie pakietu wdrażania w każdej usługi w chmurze. Jednak pamiętać, że dystrybucję ruchu usług w chmurze w różnych regionach muszą zostać wykonane przez dewelopera aplikacji lub za pomocą usługi zarządzania ruch.

Określanie liczby wystąpień zamiennych roli wdrożenia z wyprzedzeniem awarii jest ważnych aspektów planowania pojemności. Po wdrożeniu pomocniczej całych zapewnia, że wydajność pracy jest już dostępna, gdy potrzebne; Jednak to skutecznie zwiększa dwukrotnie koszt. Typowe deseń ma mały, pomocniczej wdrożenia, wystarczająco duży, aby uruchomić usługi krytyczne. Małe wdrażania pomocniczej jest dobrym pomysłem, oba, aby zarezerwować pojemności i testowanie konfiguracji środowiska pomocniczą.

>[AZURE.NOTE]Przydział subskrypcji nie jest gwarancję wydajność. Przydział jest po prostu limit kredytu. Aby zagwarantować wydajność, należy zdefiniować wymaganą liczbę ról w modelu usługi i ról należy wdrożyć.

###<a name="load-balancing"></a>Równoważenia obciążenia

Do równoważenia obciążenia ruchu między różnymi regionami wymaga rozwiązanie do zarządzania ruch. Azure udostępnia [Menedżer ruchu Azure](https://azure.microsoft.com/services/traffic-manager/). Można także korzystać z usług innych firm, które zapewniają ruch podobne funkcje zarządzania.

###<a name="strategies"></a>Strategie

Wiele alternatywnych strategii są dostępne wykonywania obliczeń rozłożone między różnymi regionami. Te muszą być dostosowane do specyficznych wymagań biznesowych i okoliczności aplikacji. Na wysokim poziomie strony można podzielić na następujące kategorie:

  * __Ponownie Wdróż na danych__: W tej metody aplikacji jest ponownego rozmieszczania serwera od podstaw w momencie awarii. Jest to właściwe dla niekrytyczne aplikacji, które nie wymagają gwarantowanej czasu.

  * __Ciepły zamiennych (aktywne/pasywne)__: pomocniczej hostowana usługa zostanie utworzona w regionie alternatywny i ról są rozmieszczane zagwarantowanie minimalnego zdolności; Jednak role nie odbierać danych produkcji. Ta metoda jest przydatna dla aplikacji, które nie jest przeznaczony do dystrybucji ruchu między różnymi regionami.

  * __Najpopularniejsze zamiennych (aktywne/aktywne)__: aplikacja jest zaprojektowane do odbierania ładowania produkcji w wielu regionów. Może być skonfigurowany z usługami w chmurze w każdym regionie dla większej pojemności niż jest to wymagane do operacji odzyskiwania. Również z usługami w chmurze może przeskalować out stosownie do potrzeb w momencie awarii i pracy awaryjnej. Ta metoda wymaga znacznych inwestycji w projekcie aplikacji, ale został znaczące korzyści. Należą do czasu odzyskiwania niskiego oraz gwarantowanej ciągły, testowanie wszystkich lokalizacji odzyskiwania i wydajne korzystanie z możliwości.

Szczegółowe omówienie projektu rozłożone wykracza poza zakres tego dokumentu. Aby uzyskać więcej informacji zobacz [Odzyskiwanie i wysokiej dostępności aplikacji Azure](https://aka.ms/drtechguide).

##<a name="virtual-machines"></a>Maszyn wirtualnych

Odzyskiwanie infrastruktury jako usługi (IaaS) wirtualnych maszyn jest podobna do platformy zgodnie z usługą (PaaS) obliczyć odzyskiwania pod wieloma względami. Istnieją istotne różnice, jednak z faktu, że maszyn wirtualnych IaaS składa się z maszyn wirtualnych i dysku maszyn wirtualnych.

  * __Kopia zapasowa Azure Użyj do tworzenia kopii zapasowych krzyżowego region, które są zgodne aplikacji__.
  [Kopia zapasowa Azure](https://azure.microsoft.com/services/backup/) służy do tworzenia kopii zapasowych spójne aplikacji na wielu dyskach maszyn wirtualnych i Obsługa replikacji kopii zapasowych wielu regionów. Możesz to zrobić, wybierając geo kopii zapasowej magazynu w momencie tworzenia. Należy zauważyć, że replikacja kopii zapasowej magazynu musi być skonfigurowany w momencie tworzenia. Nie można ustawić później. Jeśli obszar został utracony, firma Microsoft udostępni kopie zapasowe dla klientów. Klienci będą mogli przywrócić ich punktów przywracania skonfigurowane.

  * __Oddzielanie dysku danych z dysku systemu operacyjnego__. Ważnym czynniku dla maszyny wirtualne IaaS to, że nie można zmienić dysku system operacyjny, bez konieczności ponownego tworzenia maszyn wirtualnych. Nie jest to problem, jeśli strategii odzyskiwania jest ponownie rozmieścić po awarii. Jednak jeśli korzystasz z ciepłą zamiennych podejście do rezerwowania zdolności może być problem. Do wykonania tych poprawnie, musi być wyposażony dysk poprawny system operacyjny używany do głównego i pomocniczego lokalizacji, a dane aplikacji muszą być przechowywane w oddzielnym dysku. Jeśli to możliwe Użyj konfiguracji standardowej systemu operacyjnego, umieszczoną na obu tych lokalizacji. Po przełączeniu możesz następnie dołączyć dysk danych do usługi istniejące maszyny wirtualne IaaS w pomocniczą kontrolera domeny. Kopiowanie migawki dyskach danych do witryny zdalnej za pomocą AzCopy.

  * __Należy pamiętać o potencjalnych problemach spójności po przełączeniu geo wielu dysków maszyn wirtualnych__. Dyski maszyn wirtualnych są wykonywane jako Azure magazyn obiektów blob i tym samym identyfikatorem geo replikacji. O ile nie jest używana [Kopia zapasowa Azure](https://azure.microsoft.com/services/backup/) , istnieją żadnych gwarancji spójności na dyskach, ponieważ geo replikacji jest asynchroniczny i replikuje niezależnie. Poszczególne dyski maszyn wirtualnych zapewniona jest spójna awarię stanu po przełączeniu geo, ale nie jest zgodny na dyskach. Może to powodować problemy w niektórych przypadkach (na przykład w przypadku rozkładanie).

##<a name="storage"></a>Miejsca do magazynowania

###<a name="recovery-by-using-geo-redundant-storage-of-blob-table-queue-and-vm-disk-storage"></a>Odzyskiwanie przy użyciu zbędne Geo przechowywania obiektów blob, tabeli, kolejki i maszyn wirtualnych ilość miejsca do magazynowania

W Azure, obiektów blob, tabele, kolejkach i maszyn wirtualnych dyski są wszystkich przy replikowane geo domyślnie. To jest określone jako zbędne Geo przestrzeni dyskowej (GRS). GRS replikuje dane miejsca do magazynowania iloczynów centrum danych setki programu mila od siebie w ramach określonego regionu geograficznego. GRS umożliwia dodatkowe wytrzymałości w przypadku awarii głównych centrum danych. Formanty firmy Microsoft, gdy wystąpi awaryjne przeniesienie i pracy awaryjnej jest ograniczone do głównych awarii, w których pierwotnej lokalizacji podstawowego uznano odzyskać w odpowiednim ilość czasu. W niektórych scenariuszach może to być kilka dni. Dane są zwykle replikowane za kilka minut, ale interwał synchronizacji nie jest jeszcze objęte umowa dotycząca poziomu usług.

W przypadku geo przypadku awarii, będzie bez zmian sposób dostępu do konta (nie zmieni klucz adresu URL i konto). Konto miejsca do magazynowania jednak będą w innym regionie po przełączeniu. To może mieć wpływ na aplikacje, które wymagają regionalne koligacji przy użyciu swojego konta miejsca do magazynowania. Nawet w przypadku usługi i aplikacje, które nie wymagają konta miejsca do magazynowania, w tym samym centrum danych centrum danych między opóźnienie i opłaty przepustowość może być istotne powody aby tymczasowo przenieść ruch do regionu pracy awaryjnej. Może to współczynnika w ogólnej strategii odzyskiwania danych.

Oprócz automatyczne przejście dostarczony przez GRS Azure została wprowadzona usługa, która umożliwia dostęp do odczytu do kopię danych w lokalizacji przechowywania pomocniczą. Jest to odczytu zbędne Geo miejsca do magazynowania (Pomoc Zdalna GRS).

Aby uzyskać więcej informacji na temat przechowywania zarówno GRS, jak i GRS pomoc Zdalna zobacz [replikacji magazyn Azure](../storage/storage-redundancy.md).

###<a name="geo-replication-region-mappings"></a>Replikacja Geo region mapowania:

Należy wiedzieć, gdzie się dane geo replikowane, aby wiedzieć, gdzie wdrażanie wystąpienia wymagające regionalne koligacji z magazynu danych. W poniższej tabeli przedstawiono par głównego i pomocniczego lokalizacji:

[AZURE.INCLUDE [paired-region-list](../../includes/paired-region-list.md)]

###<a name="geo-replication-pricing"></a>Replikacja Geo ceny:

Replikacja Geo znajduje się w bieżącym cennik usługi Magazyn Azure. Jest to zbędne Geo przestrzeni dyskowej (GRS). Jeśli nie chcesz, dane replikowane geo geo replikacji można wyłączyć dla Twojego konta. Jest to lokalnie zbędne miejsca do magazynowania, a jest naliczany po obniżonej cenie w porównaniu z GRS.

###<a name="determining-if-a-geo-failover-has-occurred"></a>Określanie, czy pojawił się geo trybie awaryjnym

Jeśli wystąpi geo awaryjne przeniesienie, to zostanie umieszczona na [Pulpit nawigacyjny kondycji usługi Azure](https://azure.microsoft.com/status/). Aplikacje można zaimplementować zautomatyzowany wykrywania, jednak przez monitorowanie obszaru geo dla swojego konta miejsca do magazynowania. To może służyć do wyzwalanie innych operacji odzyskiwania, takich jak aktywacji zasobów obliczeń w regionie geo gdzie przeniesiony ich przechowywania. Kwerendy można wykonywać w tym z zarządzania usługą interfejsu API, przy użyciu [Uzyskaj właściwości konta miejsca do magazynowania](https://msdn.microsoft.com/library/ee460802.aspx). Odpowiednie właściwości są następujące:

    <GeoPrimaryRegion>primary-region</GeoPrimaryRegion>
    <StatusOfPrimary>[Available|Unavailable]</StatusOfPrimary>
    <LastGeoFailoverTime>DateTime</LastGeoFailoverTime>
    <GeoSecondaryRegion>secondary-region</GeoSecondaryRegion>
    <StatusOfSecondary>[Available|Unavailable]</StatusOfSecondary>

###<a name="vm-disks-and-geo-failover"></a>Dyski maszyn wirtualnych i geo przypadku awarii

Opisane w sekcji na dyskach maszyn wirtualnych, istnieje żadnych gwarancji spójności danych na dyskach maszyn wirtualnych po przełączeniu. W celu zapewnienia poprawności kopii zapasowych, produkt kopii zapasowych takich jak Data Protection Manager należy wykonywanie kopii zapasowych i przywracanie danych aplikacji.

##<a name="database"></a>Bazy danych

###<a name="sql-database"></a>Baza danych SQL

Baza danych SQL Azure udostępnia dwa typy odzyskiwania: Przywracanie Geo i replikacja Geo Active.

####<a name="geo-restore"></a>Przywracanie Geo

[Przywracanie Geo](../sql-database/sql-database-recovery-using-backups.md#geo-restore) jest również dostępne z bazami danych Basic, Standard i Premium. Jeśli baza danych jest niedostępny z powodu zdarzenia w regionie, gdzie jest hostowana bazy danych w imieniu domyślna opcja odzyskiwania. Podobnie jak w chwili przywrócić, przywracanie Geo zależy od kopie zapasowe bazy danych w magazynie Azure geo zbędne. Przywraca z kopii zapasowej replikowane geo i przez to mechanizm do dostawie miejsca do magazynowania w regionie podstawowego. Aby uzyskać więcej informacji zobacz [Przywracanie bazy danych SQL Azure lub przełączanie awaryjne do pomocniczym](../sql-database/sql-database-disaster-recovery.md).

####<a name="active-geo-replication"></a>Replikacja Geo Active

[Replikacja Geo Active](../sql-database/sql-database-geo-replication-overview.md) jest dostępny dla wszystkich poziomów bazy danych. Jest przeznaczony dla aplikacji, które mają skuteczniejsze odzyskiwanie wymagania niż Przywróć Geo mogą oferować. Przy użyciu aktywnej replikacji Geo możesz utworzyć do czterech pomocnicze czytelny na serwerach w różnych regionów. Można zainicjować przełączanie awaryjne do dowolnego z pomocnicze. Ponadto aktywne replikacji Geo może służyć do obsługi aplikacji scenariuszy uaktualnienia lub przeniesienia, a także równoważenia obciążenia dla obciążenia tylko do odczytu. Aby uzyskać szczegółowe informacje, zobacz [Konfigurowanie replikacji Geo](../sql-database/sql-database-geo-replication-portal.md) i przechodzić [do pomocniczego bazy danych](../sql-database/sql-database-geo-replication-failover-portal.md). Odwołują się do [projektowania aplikacji chmury awarii przy użyciu aktywnej replikacji Geo w bazie danych SQL](../sql-database/sql-database-designing-cloud-solutions-for-disaster-recovery.md) i [Zarządzanie uaktualnianie przewijane aplikacje w chmurze używanie SQL Replikacja bazy danych Active Geo —](../sql-database/sql-database-manage-application-rolling-upgrade.md) Aby uzyskać szczegółowe informacje na temat Projektowanie i implementowanie zastosowania uaktualnienia aplikacji bez przestojów.

###<a name="sql-server-on-virtual-machines"></a>Program SQL Server w środowisku maszyn wirtualnych systemu

Różne opcje są dostępne dla odzyskiwania i wysoką dostępność dla programu SQL Server 2012 (lub nowszy) działa w maszyn wirtualnych Azure. Aby uzyskać więcej informacji zobacz [wysokiej dostępności i odzyskiwanie w przypadku programu SQL Server w maszyn wirtualnych Azure](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

##<a name="other-azure-platform-services"></a>Inne usługi platformy Azure

Podczas próby uruchomienia usługi w chmurze w wielu regionach Azure, należy rozważyć konsekwencje dla każdej usługi zależności. W poniższych sekcjach wskazówki specyficzne dla usługi przyjęto założenie, że należy użyć tej samej usługi Azure w alternatywny Azure centrum danych. Ten proces obejmuje zarówno konfiguracji, jak i replikacja danych zadań.

>[AZURE.NOTE]W niektórych przypadkach czynności te mogą pomóc w zmniejszeniu awarii specyficznych zamiast zdarzenia całego centrum danych. Z perspektywy aplikacji może być awaria specyficznych tylko jako ograniczenie i wymaga tymczasowo migracji usługi do naprzemiennych regionu Azure.

###<a name="service-bus"></a>Usługa Bus

Azure Bus usługa używa unikatowych nazw nie obejmuje Azure regionów. Dlatego pierwszego zapotrzebowania jest konfigurowanie nazw bus niezbędne usługi w regionie alternatywny. Istnieją jednak również zagadnienia związane z wytrzymałości kolejce wiadomości. Istnieje kilka strategii replikacji wiadomości między różnymi regionami Azure. Aby uzyskać szczegółowe informacje dotyczące tych strategii replikacji i innych strategii odzyskiwania danych zobacz [Najważniejsze wskazówki dotyczące izolacji aplikacji przed dostawie Bus usługi i awarii](../service-bus-messaging/service-bus-outages-disasters.md). Aby uzyskać inne zagadnienia dostępność zobacz [Usługi Bus (dostępności)](./resiliency-technical-guidance-recovery-local-failures.md#other-azure-platform-services).

###<a name="app-service"></a>Aplikacji usługi

Aby przeprowadzić migrację aplikacji Azure aplikacji usługi, takie jak aplikacje sieci Web lub aplikacji Mobile do pomocniczej region Azure, musisz mieć kopii zapasowej dostępne do opublikowania witryny sieci Web. Awaria nie wiąże się z całą Azure centrum danych, może być możliwe za pomocą FTP Pobieranie ostatniej kopii zapasowej zawartości witryny. Następnie utworzenia nowej aplikacji w regionie alternatywny, chyba że wcześniej zdefiniowaniu zarezerwować zdolności produkcyjnych. Publikowanie witryny nowy region, a następnie wprowadź odpowiednie zmiany konfiguracji niezbędne. Te zmiany mogą zawierać parametry połączenia bazy danych lub inne ustawienia regionu. W razie potrzeby Dodaj certyfikat SSL witryny i zmienić rekord DNS CNAME, tak aby nazwę domeny niestandardowej wskazuje redeployed adres URL aplikacji sieci Web Azure.

###<a name="hdinsight"></a>Usługa HDInsight

Domyślnie w magazynie obiektów Blob platformy Azure są przechowywane dane skojarzone z usługi HDInsight. Usługa HDInsight wymaga klastrze Hadoop przetwarzanie zadań MapReduce musi znajdować Współtworzenie się w tym samym regionie jako konto miejsca do magazynowania, która zawiera dane analizowane. Pod warunkiem korzystania z funkcji replikacji geo dostępne do magazynu Azure, masz dostęp do danych w regionie pomocniczej miejsce, w którym został replikowane dane, jeśli z jakiegoś powodu podstawowego region nie jest już dostępny. Możesz utworzyć nowy klaster Hadoop w regionie miejsce, w którym dane zostały replikowane i kontynuować przetwarzanie go. Aby uzyskać inne informacje o dostępności zobacz [HDInsight (dostępności)](./resiliency-technical-guidance-recovery-local-failures.md#other-azure-platform-services).

###<a name="sql-reporting"></a>Raportowanie SQL

W tej chwili odzyskiwanie przed utratą Azure regionu wymaga wielu wystąpień raportowania SQL w różnych regionach Azure. Te wystąpienia raportowania SQL powinien uzyskiwać dostęp do tych samych danych, a dane powinny mieć własny odzyskiwania planowanie wypadku awarii. Można także utrzymać zewnętrznych kopii pliku RDL dla każdego raportu.

###<a name="media-services"></a>Usługi multimediów

Usługi multimediów Azure ma podejście różnych odzyskiwania kodowania i przesyłania strumieniowego. Zazwyczaj streaming jest bardziej krytyczne podczas awaria regionalne. Aby przygotować się do tego, należy mieć konto usługi multimediów w dwóch różnych regionów Azure. Zakodowany zawartości powinien znajdować się w obu regionów. Podczas awarii można przekierowywać ruch przesyłanie strumieniowe do naprzemiennych region. Kodowanie mogą być wykonywane w dowolnym regionie Azure. Kodowanie jest zależne od czasu, na przykład podczas przetwarzania zdarzenia na żywo, musi być gotowe przesłać zadania do naprzemiennych centrum danych podczas awarii.

###<a name="virtual-network"></a>Wirtualna sieć

Pliki konfiguracji zapewniają najszybszym sposobem wirtualna sieć w regionie Azure alternatywny. Po skonfigurowaniu sieci wirtualnej w obszarze podstawowy Azure [Eksportowanie ustawień wirtualnej sieci](../virtual-network/virtual-networks-create-vnet-classic-portal.md) dla danej sieci do pliku konfiguracji sieci. W przypadku awarii w regionie podstawowego, [Przywracanie wirtualnej sieci](../virtual-network/virtual-networks-create-vnet-classic-portal.md) z pliku konfiguracji przechowywane. Następnie skonfiguruj innych usług w chmurze, maszyn wirtualnych lub ustawienia lokalne krzyżowe do pracy z nowego wirtualnej sieci.

##<a name="checklists-for-disaster-recovery"></a>Listy kontrolne dotyczące awarii

##<a name="cloud-services-checklist"></a>Lista kontrolna dotycząca cloud Services

  1. Zapoznaj się z sekcją usług w chmurze tego dokumentu.
  2. Tworzenie strategii odzyskiwania danych między region.
  3. Opis korzystnych rozwiązań w rezerwowania zdolności w regionach alternatywny.
  4. Korzystanie z narzędzi routingu ruchu, taki jak Menedżer ruchu Azure.

##<a name="virtual-machines-checklist"></a>Lista kontrolna maszyn wirtualnych

  1. Zapoznaj się z sekcją maszyn wirtualnych tego dokumentu.
  2. [Kopia zapasowa Azure](https://azure.microsoft.com/services/backup/) należy utworzyć kopie zapasowe spójne aplikacji między różnymi regionami.

##<a name="storage-checklist"></a>Lista kontrolna miejsca do magazynowania

  1. Zapoznaj się z sekcją miejsca do magazynowania w niniejszym dokumencie.
  2. Nie można wyłączyć geo replikacji zasobów magazynowania.
  3. Opis alternatywny region geo replikacji w przypadku pracy awaryjnej.
  4. Tworzenie niestandardowych strategii tworzenia kopii zapasowych dla strategii pracy awaryjnej kontrolowane przez użytkownika.

##<a name="sql-database-checklist"></a>Lista kontrolna bazy danych SQL

  1. Zapoznaj się z sekcją bazy danych SQL tego dokumentu.
  2. Użyj [Przywracania Geo](../sql-database/sql-database-recovery-using-backups.md#geo-restore) lub [Replikacji Geo](../sql-database/sql-database-geo-replication-overview.md) zależnie od potrzeb.

##<a name="sql-server-on-virtual-machines-checklist"></a>Program SQL Server w środowisku maszyn wirtualnych systemu — Lista kontrolna

  1. Przejrzyj programu SQL Server w środowisku maszyn wirtualnych systemu części tego dokumentu.
  2. Użyj grupy dostępności (AlwaysOn) między regionu lub odzwierciedlające bazy danych.
  3. Alternatywnie używanie narzędzia Kopia zapasowa i przywracanie blob miejsca do magazynowania.

##<a name="service-bus-checklist"></a>Lista kontrolna Bus usługi

  1. Zapoznaj się z sekcją Bus usługi tego dokumentu.
  2. Konfigurowanie nazw Bus usługi w regionie alternatywny.
  3. Należy rozważyć strategie replikacji niestandardowego dla wiadomości między różnymi regionami.

##<a name="app-service-checklist"></a>Lista kontrolna aplikacji usługi

  1. Zapoznaj się z sekcją aplikacji usługi tego dokumentu.
  2. Obsługa kopie zapasowe witryny poza podstawowym region.
  3. W przypadku częściowej awarii próbę pobrania bieżącej witryny FTP.
  4. Zamierzasz wdrożyć witrynę do nowej lub istniejącej witryny w regionie alternatywny.
  5. Planowanie zmian w konfiguracji dla aplikacji i rekordy DNS CNAME.

##<a name="hdinsight-checklist"></a>Lista kontrolna HDInsight

  1. Zapoznaj się z sekcją HDInsight tego dokumentu.
  2. Utwórz nowy klaster Hadoop w regionie zreplikowanej danych.

##<a name="sql-reporting-checklist"></a>Lista kontrolna raportowania SQL

  1. Zapoznaj się z sekcją raportowania SQL tego dokumentu.
  2. Obsługa alternatywny wystąpienie raportowania SQL w innym regionie.
  3. Obsługa planu osobnych replikacji docelowej do tego regionu.

##<a name="media-services-checklist"></a>Lista kontrolna dotycząca usługi multimediów

  1. Zapoznaj się z sekcją usługi multimediów tego dokumentu.
  2. Utwórz konto usługi multimediów w regionie alternatywny.
  3. Kodowanie tę samą zawartość pomocy technicznej przesyłanie strumieniowe pracy awaryjnej dla obu regionów.
  4. Przesyłanie zadań kodowania do naprzemiennych region wypadku awarii usługi.

##<a name="virtual-network-checklist"></a>Wirtualna sieć, lista kontrolna

  1. Zapoznaj się z sekcją wirtualną sieć tego dokumentu.
  2. Użyj eksportowane ustawienia wirtualną sieć, aby ponownie utworzyć w innym regionie.

##<a name="next-steps"></a>Następne kroki

Ten artykuł jest częścią serii ograniczony do [wytycznych technicznych elastyczność Azure](./resiliency-technical-guidance.md). Następny artykuł z tej serii omówiono [odzyskiwania z centrum danych lokalnych Azure](./resiliency-technical-guidance-recovery-on-premises-azure.md).
