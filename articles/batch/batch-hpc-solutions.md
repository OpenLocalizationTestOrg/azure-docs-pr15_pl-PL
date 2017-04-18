<properties
   pageTitle="Rozwiązania partii i HPC w chmurze | Microsoft Azure"
   description="Dowiedz się więcej o wysokiej wydajności obliczeniowej (HPC i obliczyć duży) scenariusze i opcje rozwiązanie Azure i partii"
   services="batch, virtual-machines, cloud-services"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="big-compute"
   ms.date="07/27/2016"
   ms.author="danlep"/>

# <a name="batch-and-hpc-solutions-in-the-azure-cloud"></a>Rozwiązania partii i HPC w chmurze Azure

Azure oferuje rozwiązania cloud wydajność, skalowalna partii i wysokiej wydajności (HPC) — skrót *Duży obliczenia*. Dowiedz się więcej o duży obliczyć obciążenia i usługi Azure firmy do obsługi je tutaj lub przejść bezpośrednio do [scenariusze rozwiązanie](#scenarios) w dalszej części tego artykułu. Ten artykuł dotyczy głównie techniczne decydentów, menedżerów i niezależnych dostawców oprogramowania, ale inne informatyków i deweloperów służy do zapoznania się z tych rozwiązań.

Organizacji ma skalę problemów komputerowych: rysunek techniczny projektu i analizy, renderowania obrazów złożonych modelowania, Monte Carlo symulacji, obliczenia finansowe ryzyka i innych osób. Azure ułatwia organizacjom rozwiązywania tych problemów z zasobów, skali i harmonogram, które są potrzebne. Azure organizacje następujące możliwości:

* Tworzenie rozwiązania hybrydowego, wydłużając klastrze HPC lokalnego odciążania obciążenia szczyt w chmurze

* Uruchamianie narzędzia klaster HPC i obciążenia całkowicie platformy Azure

* Uruchamianie obciążenia obliczeniowych bez konieczności wdrażania i zarządzania infrastrukturą obliczeń przy użyciu zarządzanych i skalowalna Azure usług, takich jak [partii](https://azure.microsoft.com/documentation/services/batch/)

Mimo że poza zakres tego artykułu Azure programiści i partnerów pełny zestaw możliwości, opcji architektura i narzędzi do tworzenia Tworzenie dużych niestandardowe obliczenia duży przepływy pracy. I rosnącej ekosystemu partnerów jest gotowa do wysłania ułatwiające obciążenie pracą z obliczenia ogólnej wydajności w chmurze Azure.


## <a name="batch-and-hpc-applications"></a>Partia i HPC aplikacji

W przeciwieństwie do sieci web aplikacji i wiele linii firmie, partii i aplikacji HPC mają zdefiniowane początek i koniec i działają zgodnie z harmonogramem lub na żądanie, czasami dla godzin lub już. Większość podzielić na dwie główne kategorie: *leżą równoległa* (nazywane "embarrassingly równoległa", ponieważ ich rozwiązywania problemów nadają się do uruchamiania równolegle na wielu komputerach lub procesorów) i *ściśle związane*. Aby uzyskać więcej informacji na temat tych typów aplikacji, zobacz poniższej tabeli. Niektóre rozwiązania Azure osiąga działania dla jednego typu lub inny.

>[AZURE.NOTE] W partii i HPC rozwiązania uruchomionego wystąpienia aplikacji zwykle nosi nazwę *zadania*i każdego zadania może uzyskać podzielone na *zadania*. I zasoby grupowany obliczeń dla aplikacji są często nazywane *obliczyć węzły*.

Typ | Właściwości | Przykłady
------------- | ----------- | ---------------
**Bardzo równoległa**<br/><br/>![Bardzo równoległa][parallel] |• Poszczególne komputery uruchamiać niezależnie logiki aplikacji<br/><br/> • Dodawanie komputerów umożliwia aplikacji skalowania i przyspieszanie szczegółowej obliczeń<br/><br/>• Aplikacji składa się z osobnych plików wykonywalnych lub jest podzielony na grupy usługi wywołany przez klienta (architekturę usługowych lub SOA, aplikacji) |• Modelowanie finansowe ryzyka<br/><br/>• Renderowanie obrazu i przetwarzania obrazów<br/><br/>• Kodowania multimediów i przekodowanie<br/><br/>• Monte Carlo symulacji<br/><br/>• Testów oprogramowania
**Ściśle związane**<br/><br/>![Ściśle związane][coupled] |• Aplikacji wymaga węzły obliczeń do interakcji lub pośredniej wyników w programie exchange<br/><br/>• Obliczeń węzły może komunikować się przy użyciu wiadomości przechodzące Interface (MPI), typowe protokół komunikacji dla przetwarzania równoległego<br/><br/>• Aplikacji jest wielkość liter do czasu oczekiwania w sieci i przepustowość<br/><br/>• Wydajność aplikacji można poprawić za pomocą szybkich technologii sieciowych, takich jak InfiniBand i zdalny bezpośredni dostęp do pamięci (RDMA) |• Oil i gazu zbiorniku modelowania<br/><br/>• Techniczny projektowanie i analizy, takich jak obliczeniowa dynamics objętości<br/><br/>• Fizycznie symulacji, takich jak awarie samochodu i reakcji Atomowej<br/><br/>• Prognozowanie pogody

### <a name="considerations-for-running-batch-and-hpc-applications-in-the-cloud"></a>Zagadnienia związane z systemem partii i aplikacjach HPC w chmurze

Możesz łatwo przeprowadzić migrację wielu aplikacji, które są przeznaczone do uruchamiania w lokalnym klastrów Azure lub do środowiska hybrydowego (lokalnej infrastrukturze). Jednak czasami może występować pewne ograniczenia lub zagadnienia, w tym:


* **Dostępność zasobów w chmurze** — w zależności od typu zasobów obliczeń chmury, którego używasz, nie można polega na dostępność ciągły komputera podczas wykonywania zadania. Postęp i stan obsługi Sprawdź wskazującą są typowe technik obsługę możliwych błędów przejściowych i bardziej niezbędne podczas korzystania z zasobów w chmurze.


* **Dostęp do danych** - danych programu access technik powszechnie dostępny w klastrów przedsiębiorstwa, takich jak NFS, mogą wymagać specjalnej konfiguracji w chmurze. Lub może być konieczne podjęcie innych danych programu access wskazówki i deseni dla chmury.

* **Przenoszenie danych** — dla aplikacji, że proces dużych ilości danych, strategie są potrzebne, aby przenieść dane do magazynu w chmurze, a do obliczenia zasobów. Może być konieczne szybkie wersji lokalnej sieci, takich jak [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/). Rozważ również prawne, prawnych lub ograniczenia zasad przechowywania lub uzyskiwania dostępu do danych.


* **Licencjonowanie** - skontaktuj się z dostawcą komercyjnego stosowania licencji lub innych ograniczeń do uruchamiania w chmurze. Nie wszystkie dostawców oferuje, płatne licencjonowania. Może być konieczne Planowanie serwera licencjonowania w chmurze dla tego rozwiązania lub nawiązywanie połączenia z lokalnego serwera licencji.


### <a name="big-compute-or-big-data"></a>Duży obliczeń lub duży danych?

Linii podziału między aplikacjami duży obliczenia i duży danych nie zawsze jest Wyczyść, a niektóre aplikacje mogą zachowywać cechy obydwu. Obejmują zarówno uruchomionych na dużą skalę obliczeń zazwyczaj klastrów komputerów. Ale metod rozwiązanie i obsługi narzędzi mogą się różnić.

• **Duży obliczyć** zwykle obejmują aplikacje korzystające z dodatku Procesora i pamięci, takich jak konstrukcyjną symulacji, ryzyka finansowego modelowania i renderowanie cyfrowego. Infrastruktura rozwiązanie duży obliczyć mogą zawierać komputerach z specjalistyczne procesorów wielordzeniowych w celu wykonywania obliczeń nieprzetworzonych i specjalistyczne, szybkich sprzętu sieciowego, aby połączyć komputery.

• **Big Data** rozwiązuje problemy analizy danych, które wymagają dużych ilości danych, które nie mogą być zarządzane za pomocą jednego systemu zarządzania komputer lub baza danych. Jako przykład można wymienić dużych ilości dzienniki sieci web lub innych danych analiz biznesowych. Duży danych powoduje bardziej opierają się na dysku i wydajność wejścia/wyjścia niż power Procesora. Dostępne są także specjalnych narzędzi duży danych, takich jak Hadoop Apache do zarządzania klaster i partycją danych. (Dla informacji o Azure HDInsight i innych rozwiązań Azure Hadoop, zobacz [Hadoop](https://azure.microsoft.com/solutions/hadoop/)).

## <a name="compute-management-and-job-scheduling"></a>Zarządzanie obliczeń i planowanie zadań

Uruchamianie partii i HPC aplikacji często zawiera *Menedżer klastrów* i *Harmonogram zadań* ułatwiające zarządzanie zasobami grupowany obliczeń i przydzielanie ich do aplikacji, które zadań. Te funkcje mogą osiągnąć przy osobnych narzędzia lub narzędzia zintegrowane lub usługi.

* **Menedżer klastrów** — postanowienia, wydanie i zarządza obliczyć zasobów (lub obliczyć węzły). Menedżer klaster może zautomatyzować instalacji obrazów systemu operacyjnego i aplikacji w węzłach obliczeń skalowanie obliczeń zasobów według potrzeb, a monitorowanie wydajności węzłów.

* **Harmonogram zadań** — określa zasobów (na przykład procesorów lub pamięć) aplikację potrzeb i warunków, gdy zostanie uruchomiony. Harmonogram zadań obsługuje kolejki zadań i przydzielania zasobów do nich na podstawie przydzielonego priorytetu lub inne cechy.

Klaster i narzędzia do klastrów systemu Windows i Linux oraz planowania zadań można migrować do Azure. Na przykład [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029), rozwiązanie klaster komputerowe bezpłatnej firmy Microsoft dla systemu Windows i Linux oraz HPC obciążeń pracą, oferuje kilka opcji uruchamiania platformy Azure. Można również tworzyć klastrów Linux do uruchomienia narzędzia Otwórz źródło takich jak moment i SLURM. Możesz również zabrać ze sobą komercyjnego rozwiązań Azure, takich jak [TIBCO DataSynapse GridServer](http://www.tibco.com/company/news/releases/2016/tibco-to-accelerate-cloud-adoption-of-banking-and-capital-markets-customers-via-microsoft-collaboration), [Symphony platformy IBM](http://www-01.ibm.com/support/docview.wss?uid=isg3T1023592)i [Aparat siatki Univa](http://www.univa.com/products/grid-engine).

Jak przedstawiono w poniższych sekcjach, możesz także korzystać z usług Azure zarządzanie zasobami obliczeń i planowanie zadań bez (lub oprócz) narzędzi do zarządzania tradycyjnych klaster.


## <a name="scenarios"></a>Scenariusze

Oto trzy typowe scenariusze, aby uruchomić obliczenia duży obciążeń pracą w Azure za pomocą istniejących rozwiązań klaster HPC, usług Azure lub połączenie dwóch. Kluczowe zagadnienia dotyczące wybierania każdego scenariusza są wyświetlane, ale nie są pełne. Więcej informacji o dostępnych usług Azure, których można użyć w rozwiązaniu jest późniejsza tego artykułu.

  | Scenariusz | Dlaczego warto wybrać go?
------------- | ----------- | ---------------
**Serii klastrze HPC Azure**<br/><br/>[! [Klaster serii] [burst_cluster]](./media/batch-hpc-solutions/burst_cluster.png) <br/><br/> Dowiedz się więcej:<br/>• [Serii wystąpieniach Azure pracownik pakietem HPC](https://technet.microsoft.com/library/gg481749.aspx)<br/><br/>• [Konfigurowanie hybrydowego obliczyć klaster z dodatkiem Service Pack HPC](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)<br/><br/>• [Serii Azure partii pakietem HPC](https://technet.microsoft.com/library/mt612877.aspx)<br/><br/>|• Połączyć usługi [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) lub innych klaster lokalnego z dodatkowych zasobów Azure rozwiązania hybrydowego.<br/><br/>• Rozszerzanie do obliczenia duży obciążenia do uruchomienia na platformie usług (PaaS) wystąpień maszyn wirtualnych (obecnie Windows Server tylko).<br/><br/>• Dostęp lokalnego serwera lub dane magazyn licencji przy użyciu opcjonalne Azure wirtualnej sieci|• Masz istniejącym klastrem HPC i potrzebujesz więcej zasobów <br/><br/>• Nie chcesz kupić i zarządzanie infrastrukturą klaster HPC dodatkowe<br/><br/>• Przejściowych okresów szczytowego zapotrzebowania lub projekty specjalne
**Tworzenie klastrze HPC całkowicie platformy Azure**<br/><br/>[! [Klaster w IaaS] [iaas_cluster]](./media/batch-hpc-solutions/iaas_cluster.png)<br/><br/>Dowiedz się więcej:<br/>• [Rozwiązań klaster HPC platformy Azure](./big-compute-resources.md)<br/><br/>|• Szybkie i spójne wdrażanie aplikacji i narzędzia klaster na standardowych lub niestandardowych infrastruktury systemu Windows i Linux oraz jako maszyn wirtualnych usługi (IaaS).<br/><br/>• Uruchom różne obciążenia duży obliczyć przy użyciu zadania planowania rozwiązanie wybranych przez użytkownika.<br/><br/>• Umożliwia tworzenie rozwiązań opartych na chmurze pełną dodatkowe usługi Azure, łącznie z sieci i miejsca do magazynowania. |• Nie chcesz kupić i zarządzanie nimi dodatkowa infrastruktura klaster Linux lub HPC systemu Windows<br/><br/>• Przejściowych okresów szczytowego zapotrzebowania lub projekty specjalne<br/><br/>• Potrzebny dodatkowy klaster czas, ale nie zamierzasz inwestycji w komputerach oraz miejsce do wdrożenia<br/><br/>• Chcesz offload aplikacji obliczeniowych, aby była uruchamiana jako usługa wyłącznie w chmurze
**Możliwość skalowania równoległe zastosowanie Azure**<br/><br/>[! [Azure partii] [batch_proc]](./media/batch-hpc-solutions/batch_proc.png)<br/><br/>Dowiedz się więcej:<br/>• [Podstawy partia Azure](./batch-technical-overview.md)<br/><br/>• [Rozpoczynanie pracy z biblioteką partii Azure dla środowiska .NET](./batch-dotnet-get-started.md)|• Tworzenie z [Partii Azure](https://azure.microsoft.com/documentation/services/batch/) różne obciążenia obliczyć duży do uruchomienia na pul maszyn wirtualnych systemu Windows i Linux oraz możliwość skalowania.<br/><br/>• Przy użyciu usługi platformy Azure wdrożenia i autoscaling maszyn wirtualnych, planowanie zadań, odzyskiwanie, przenoszenia danych, zarządzania zależności i wdrażanie aplikacji do zarządzania.|•, Których nie chcesz, aby zarządzać obliczyć zasoby i harmonogram zadań; Zamiast tego chcesz skupić się na uruchamianie aplikacji<br/><br/>• Chcesz offload aplikacji obliczeniowych, aby była uruchamiana jako usługa w chmurze<br/><br/>• Chcesz automatyczne skalowanie zasobów obliczeń zgodnie z pracą obliczeń


## <a name="azure-services-for-big-compute"></a>Duży obliczyć usługi Azure

Poniżej przedstawiono więcej informacji na temat obliczeń, dane, sieci i pokrewne usługi, które można łączyć dla rozwiązań duży obliczenia i przepływy pracy. Aby uzyskać szczegółowe wskazówki dotyczące usług Azure zapoznaj się z usługi Azure [dokumentacji](https://azure.microsoft.com/documentation/). [Scenariusze](#scenarios) , w tym artykule pokazano tylko kilka sposobów użycia tych usług.

>[AZURE.NOTE] Azure regularnie wprowadza nowych usług, które mogą być przydatne w przypadku rozwiązania. Jeśli masz pytania, skontaktuj się [Azure partnera](https://pinpoint.microsoft.com/en-US/search?keyword=azure) lub wiadomości e-mail *bigcompute@microsoft.com*.

### <a name="compute-services"></a>Obliczanie usług

Usługi Azure obliczeń jest podstawową rozwiązanie duży obliczenia i korzyściach oferty usług różnych obliczeń dla różnych scenariuszach. Na poziomie podstawowym tych usług oferują różne tryby uruchamiania aplikacji na wystąpień oparte na maszyn wirtualnych obliczeń, które Azure oferuje przy użyciu technologii systemu Windows Server Hyper-V. Te wystąpienia można uruchamiać standardowych i niestandardowych systemy operacyjne Windows i Linux i narzędzia. Azure umożliwia wybór rozmiarów [wystąpienie](../virtual-machines/virtual-machines-windows-sizes.md) w różnych konfiguracjach rdzenie Procesora, pamięci, dysku i inne cechy. W zależności od potrzeb można skalować wystąpienia do tysięcy rdzeni i następnie skalowanie w dół, gdy potrzebujesz mniej zasobów.

>[AZURE.NOTE] Skorzystaj z zalet Azure wystąpień obliczeniowych, aby zwiększyć wydajność i skalowalność obciążenia HPC tym równoległe aplikacji MPI, które wymagają krótki czas oczekiwania i wysokiej wydajności aplikacji sieci. Zobacz [informacje o serii H i obliczeniowych maszyny wirtualne A serii](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md).  

Usługa | Opis
------------- | -----------
**[Maszyn wirtualnych](https://azure.microsoft.com/documentation/services/virtual-machines/)**<br/><br/> |• Podaj obliczyć infrastruktury jako usługa (IaaS) za pomocą technologii Microsoft Hyper-V<br/><br/>• Umożliwiają elastycznie obsługi administracyjnej i zarządzać komputerami trwałych chmury standardowych serwera systemu Windows i Linux oraz obrazów z [Usługi Azure Marketplace](https://azure.microsoft.com/marketplace/)lub obrazy i dyski danych, reprezentujących<br/><br/>• Może być używany i zarządzać jako [Zestawy skali maszyn wirtualnych](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/) do utworzenia na dużą skalę usługi z identycznymi maszyn wirtualnych, z autoscaling, aby zwiększyć lub zmniejszyć wydajność automatycznie<br/><br/>• Uruchom lokalnego obliczyć klaster narzędzi i aplikacji całkowicie w chmurze<br/><br/>
**[Usług w chmurze](https://azure.microsoft.com/documentation/services/cloud-services/)**<br/><br/> |• Można uruchamiać duży obliczenia aplikacji w pracownik wystąpienia roli, maszyn wirtualnych wersją systemu Windows Server i zarządza całkowicie Azure<br/><br/>• Włączyć skalowalna, niezawodne aplikacje z niskim administrację, uruchomione na platformie jako model usługi (PaaS)<br/><br/>• Mogą wymagać dodatkowych narzędzi lub rozwoju integrację z lokalnego HPC klaster rozwiązań
**[Partii](https://azure.microsoft.com/documentation/services/batch/)**<br/><br/> |• Działa na dużą skalę obciążenia równolegle i partii w pełni zarządzanych usług<br/><br/>• Umożliwia planowanie zadań i autoscaling puli zarządzanych maszyn wirtualnych<br/><br/>• Umożliwia tworzenie i uruchamianie aplikacji usługi lub chmury Włączanie aplikacji<br/>

### <a name="storage-services"></a>Usług magazynu

Rozwiązanie duży obliczyć zwykle działa w zestawie danych wejściowych i generuje dane dotyczące jej wyniki. Oto niektóre z usług Azure magazynowania używane w duży obliczyć rozwiązań:

* [Blob, tabeli i przechowywania kolejki](https://azure.microsoft.com/documentation/services/storage/) — zarządzanie dużymi ilościami danych niestrukturalne, NoSQL danych oraz wiadomości dla przepływu pracy i komunikacji, odpowiednio. Na przykład można użyć magazyn obiektów blob dużych zestawów danych technicznych lub obrazów wejściowych lub procesami aplikacji pliki multimedialne. Można użyć kolejki komunikacji asynchroniczne w rozwiązaniu. Zobacz [Wprowadzenie do magazynu platformy Microsoft Azure](../storage/storage-introduction.md).

* [Magazyn plików azure](https://azure.microsoft.com/services/storage/files/) — udostępnia typowych pliki i dane w Azure za pomocą protokołu SMB standardowy, który jest wymagany dla rozwiązania klaster HPC.

* [Magazyn Lake danych](https://azure.microsoft.com/services/data-lake-store/) — udostępnia informatycznych Hadoop Apache rozproszony System plików w chmurze, przydatne w przypadku partii w czasie rzeczywistym, i interakcyjne analizy.

### <a name="data-and-analysis-services"></a>Usługi i analiza danych

Kilka scenariuszy duży obliczyć obejmować przepływów danych na dużą skalę lub wygenerować danych, który wymaga dalszego przetwarzania lub analizy. Azure oferuje kilka usług i analiza danych, w tym:

* [Dane Factory](https://azure.microsoft.com/documentation/services/data-factory/) - przepływów pracy opartych na danych w wersjach (rurociągi) tego sprzężenia, sumaryczne i przekształcania danych z lokalnego, oparte na chmurze i Internet magazynów.

* [Baza danych SQL](https://azure.microsoft.com/documentation/services/sql-database/) - zawiera kluczowe funkcje systemu zarządzania relacyjnej bazy danych programu Microsoft SQL Server w zarządzanych usług.

* [Usługa HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/) — wdrożenie i przepisy Windows Server lub systemem Linux Hadoop Apache klastrów w chmurze, aby zarządzać, analizowanie i raporty dotyczące ogólnej danych.

* [Maszynowego uczenia](https://azure.microsoft.com/documentation/services/machine-learning/) się - ułatwia tworzenie i testowanie, działają oraz zarządzanie przewidywanych rozwiązań analitycznych w pełni zarządzanych usług.

### <a name="additional-services"></a>Dodatkowe usługi

Rozwiązanie obliczyć duży, może być konieczne innych usług Azure, aby połączyć się z zasobami lokalnego lub w innych środowiskach. Przykłady:

* [Wirtualna sieć](https://azure.microsoft.com/documentation/services/virtual-network/) — tworzy logicznie odizolowanych sekcji w Azure, aby połączyć Azure zasobów lub centrum danych lokalnych. Z wirtualnej sieci lokalnej krzyżowe, obliczyć duży aplikacje można uzyskiwać dostęp do danych lokalnych, usługi Active Directory i serwerów licencji

* [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) - tworzy prywatne połączenie między centrach danych firmy Microsoft i infrastruktury z lokalną lub w środowisku Współtworzenie lokalizacji. ExpressRoute zapewnia lepsze zabezpieczenia, więcej niezawodności, szybsze szybkości i opóźnienia niższej niż typowy połączenia przez Internet.

* [Usługa Bus](https://azure.microsoft.com/documentation/services/service-bus/) - udostępnia kilka mechanizmy aplikacjom komunikowanie się lub wymiany danych, czy znajdują się one z platformy Azure na innej platformie chmury lub w centrum danych.

## <a name="next-steps"></a>Następne kroki

* Zobacz [Techniczną dla partii i HPC](big-compute-resources.md) , aby znaleźć wskazówki techniczne tworzyć rozwiązania.

* Dyskusje Azure opcje partnerom w tym cyklu przetwarzania i UberCloud.

* Przeczytaj o Azure duży obliczyć rozwiązań większością [Wieże Watson](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18222), [Altair](https://azure.microsoft.com/blog/availability-of-altair-radioss-rdma-on-microsoft-azure/), [ANSYS](https://azure.microsoft.com/blog/ansys-cfd-and-microsoft-azure-perform-the-best-hpc-scalability-in-the-cloud/)i [d3VIEW](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=22088).

* Aby uzyskać najnowsze ogłoszenia zobacz [HPC firmy Microsoft i blogu zespołu partii](http://blogs.technet.com/b/windowshpc/) i [Azure blogu](https://azure.microsoft.com/blog/tag/hpc/).

<!--Image references-->
[parallel]: ./media/batch-hpc-solutions/parallel.png
[coupled]: ./media/batch-hpc-solutions/coupled.png
[iaas_cluster]: ./media/batch-hpc-solutions/iaas_cluster.png
[burst_cluster]: ./media/batch-hpc-solutions/burst_cluster.png
[batch_proc]: ./media/batch-hpc-solutions/batch_proc.png
