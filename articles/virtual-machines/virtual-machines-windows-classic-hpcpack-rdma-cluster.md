<properties
 pageTitle="Konfigurowanie klastrze RDMA systemu Windows do uruchamiania aplikacji MPI | Microsoft Azure"
 description="Dowiedz się, jak utworzyć klaster systemu Windows HPC Pack o rozmiarze H16r, H16mr, A8 lub maszyny wirtualne A9, za pomocą sieci Azure RDMA uruchamianie aplikacji MPI."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="09/20/2016"
 ms.author="danlep"/>

# <a name="set-up-a-windows-rdma-cluster-with-hpc-pack-to-run-mpi-applications"></a>Konfigurowanie klastrze RDMA systemu Windows z dodatkiem Service Pack HPC do uruchamiania aplikacji MPI

Skonfiguruj klaster RDMA systemu Windows Azure z [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) i [H serii lub obliczeniowych wystąpienia serii odpowiedzi](virtual-machines-windows-a8-a9-a10-a11-specs.md) na uruchamianie aplikacji równoległych wiadomości przechodzące Interface (MPI). Po skonfigurowaniu obsługującym RDMA, oparte na serwerze Windows węzły w klastrze HPC Pack aplikacji MPI wydajne komunikowanie się przez krótki czas oczekiwania wysokiej wydajności sieci w Azure, oparty na technologii programu access (RDMA) zdalnego bezpośredni pamięci.

Jeśli chcesz uruchomić obciążenia MPI maszyny wirtualne Linux, że dostęp do sieci Azure RDMA, zobacz [Konfigurowanie klastrze Linux RDMA do uruchamiania aplikacji MPI](virtual-machines-linux-classic-rdma-cluster.md).


## <a name="hpc-pack-cluster-deployment-options"></a>Opcje wdrażania klaster HPC Pack
Microsoft HPC Pack to narzędzie bez dodatkowych opłat w celu utworzenia HPC klastrów lokalnego lub platformy Azure do uruchamiania systemu Windows i Linux oraz HPC aplikacji. Pakiet HPC zawiera środowiska wykonawczego implementację firmy Microsoft wiadomości przechodzące interfejsu systemu Windows (MS-MPI). Gdy używana z obsługą RDMA wystąpień obsługiwanego systemu operacyjnego Windows Server, HPC Pack zawiera wydajność opcji uruchamiania aplikacji MPI systemu Windows, które uzyskiwać dostęp do sieci Azure RDMA. 

W tym artykule przedstawiono dwa scenariusze i łącza do szczegółowe wytyczne, aby skonfigurować klastrze RDMA systemu Windows z Microsoft HPC Pack. 

* Scenariusz 1. Wdrażanie wystąpienia roli Pracownik obliczeniowych (PaaS)

* Scenariusz 2. Wdrażanie obliczeń węzłów na obliczeniowych maszyny wirtualne (IaaS)

Aby uzyskać ogólne warunki wstępne wystąpienia obliczeniowych z systemem Windows zobacz [informacje o serii H i obliczeniowych maszyny wirtualne A serii](virtual-machines-windows-a8-a9-a10-a11-specs.md) .



## <a name="scenario-1-deploy-compute-intensive-worker-role-instances-paas"></a>Scenariusz 1. Wdrażanie wystąpienia roli Pracownik obliczeniowych (PaaS)

Z istniejącym klastrem HPC Pack dodanie zasobów dodatkowych obliczeń w przypadkach roli Azure pracownika (węzłach Azure) działa w usłudze w chmurze (PaaS). Ta funkcja, nazywany "serii Azure" z HPC Pack obsługuje zakres rozmiarów wystąpień roli pracownika. Podczas dodawania Azure węzły, po prostu Określ jeden z obsługą RDMA rozmiarów.

Poniżej przedstawiono zagadnienia oraz czynności, aby serii do RDMA dostępem do Azure wystąpienia z istniejącego (zwykle lokalnego) klaster. Umożliwia dodawanie wystąpienia roli pracownika do węzła głównego HPC Pack, który został wdrożony w maszyn wirtualnych Azure podobnych procedur.

>[AZURE.NOTE] Samouczek serii Azure z HPC Pack zobacz [Konfigurowanie klastrze hybrydowego z dodatkiem Service Pack HPC](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Uwaga zagadnienia w poniższych kroków, które dotyczą specjalnie RDMA dostępem do węzły Azure.

![Serii Azure][burst]

### <a name="steps"></a>Czynności

4. **Wdrażanie i konfigurowanie węzła głównego HPC Pack 2012 R2**

    Pobierz najnowszą pakiet instalacyjny HPC Pack z [Centrum pobierania Microsoft](https://www.microsoft.com/download/details.aspx?id=49922). Wymagania i instrukcje, aby przygotować się do wdrożenia usługi Azure serii zobacz [HPC Pack przewodnika wprowadzenie](https://technet.microsoft.com/library/jj884144.aspx) i [serii wystąpieniach pracownik Azure z Microsoft HPC Pack](https://technet.microsoft.com/library/gg481749.aspx).

5. **Konfigurowanie certyfikatu zarządzania Azure subskrypcji**

    Konfigurowanie certyfikatu w celu bezpiecznego połączenia między węzła głównego i Azure. Opcje i procedur zobacz [scenariuszy, aby skonfigurować certyfikat zarządzania Azure pakietu HPC](http://technet.microsoft.com/library/gg481759.aspx). W przypadku wdrożeń test HPC Pack instaluje domyślne Microsoft HPC Azure certyfikatu zarządzania szybko można przekazać do subskrypcji usługi Azure.

6. **Tworzenie nowej usługi w chmurze i konto miejsca do magazynowania**

    Umożliwia utworzenie usługi w chmurze i kontem miejsca do magazynowania dla wdrożenia w regionie, gdzie są dostępne wystąpienia RDMA dostępem do portalu klasyczny Azure.

7. **Tworzenie szablonu węzeł Azure**

    Użyj węzeł Kreator Tworzenie szablonu w Menedżerze klaster HPC. Aby uzyskać instrukcje zobacz [Tworzenie szablonu Azure węzeł](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Templ) w "Czynności do wdrożenia Azure węzły z Microsoft HPC Pack".

    Dla testów wstępnych Sugerujemy Konfigurowanie zasad dostępność ręcznego w szablonie.

8. **Dodawanie węzły z klastrem**

    Używanie Kreatora węzeł dodawanie w Menedżerze klaster HPC. Aby uzyskać więcej informacji zobacz [Dodawanie węzłów Azure klastrem HPC systemu Windows](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Add).

    Podczas określania rozmiaru węzły, wybierz jedną z obsługą RDMA rozmiarów wystąpienie.
    
    >[AZURE.NOTE]W każdej serii Azure wdrażania z wystąpieniami obliczeniowych HPC Pack automatycznie wdraża co najmniej 2 RDMA dostępem do wystąpienia (na przykład A8) jako węzły serwera proxy, oprócz wystąpienia roli Pracownik Azure zadanej. Węzły serwera proxy za pomocą rdzeni, które zostało przydzielonych do subskrypcji i ponoszenia opłat wraz z wystąpienia roli Azure pracownika.

9. **Rozpoczęcie (Obsługa administracyjna) węzły i przenieść je w trybie online do zadań**

    Zaznacz węzły i za pomocą akcji **Rozpocznij** w Menedżerze klaster HPC. Po zakończeniu inicjowania obsługi administracyjnej zaznacz węzły i za pomocą akcji **Przejdź do trybu Online** w Menedżerze klaster HPC. Węzły są gotowe do uruchomienia zadania.

10. **Przesyłanie zadań z klastrem**

    Narzędzia HPC Pack zadania przesyłania do uruchamiania zadań klaster. Zobacz [pakiet Microsoft HPC: zadań zarządzania](http://technet.microsoft.com/library/jj899585.aspx).

11. **Zatrzymywanie (deprovision) węzłów**

    Po zakończeniu zadania uruchomionego węzły przełączyć do trybu offline i użyć akcji **Zatrzymaj** w Menedżerze klaster HPC.





## <a name="scenario-2-deploy-compute-nodes-in-compute-intensive-vms-iaas"></a>Scenariusz 2. Wdrażanie obliczeń węzłów na obliczeniowych maszyny wirtualne (IaaS)

W tym scenariuszu wdrażanie węzła głównego HPC Pack i klaster obliczyć węzłów na maszyny wirtualne dołączony do domeny usługi Active Directory w Azure wirtualnej sieci. HPC Pack zapewnia różne [Opcje wdrażania w maszyny wirtualne Azure](virtual-machines-linux-hpcpack-cluster-options.md), takie jak skrypty automatycznego wdrażania i szablony Azure Szybki Start. Na przykład zagadnienia i kroki wymienione poniżej pomagają za pomocą [skryptu wdrażania HPC Pack IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) zautomatyzować większość tego procesu.

![Klaster w Azure maszyny wirtualne][iaas]



### <a name="steps"></a>Czynności

1. **Tworzenie węzła głowy i obliczyć maszyny wirtualne węzeł, uruchamiając skrypt wdrożenia HPC Pack IaaS na komputerze klienckim**

    Pobierz pakiet skrypt wdrożenia HPC Pack IaaS z [Centrum pobierania Microsoft](https://www.microsoft.com/download/details.aspx?id=49922).

    Aby przygotować komputer kliencki, tworzenie pliku konfiguracji skryptu i uruchomić skrypt, zobacz [Tworzenie klastrze HPC scenariusz wdrażania HPC Pack IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md). 
    
    Aby wdrożyć węzły obsługujące RDMA obliczeń, należy zwrócić uwagę następujące zagadnienia dodatkowe:
    
    * **Wirtualna sieć** — Określ sieci wirtualnej w regionie, w którym znajduje się RDMA dostępem do rozmiar wystąpienie, którego chcesz użyć.

    * **System operacyjny Windows Server** — do obsługi połączeń RDMA, określić system operacyjny Windows Server 2012 R2 lub Windows Server 2012 węzła obliczeń maszyny wirtualne.

    * **Usług w chmurze** — zaleca się wdrażanie usługi węzła głównego w usłudze w chmurze jeden i węzły obliczeń w usłudze w chmurze różnych.

    * **Rozmiar węzła głowy** — w tym scenariuszu należy rozważyć, czy o rozmiarze co najmniej A4 (bardzo duży) dla węzła głównego.

    * **Rozszerzenie HpcVmDrivers** - skrypt wdrożenia instaluje agenta maszyn wirtualnych Azure i rozszerzenia HpcVmDrivers automatycznie po wdrożeniu rozmiar A8 lub A9 obliczyć węzły z systemem operacyjnym Windows Server. HpcVmDrivers instaluje sterowniki w węźle obliczeń maszyny wirtualne, co pozwala im połączyć się z siecią RDMA.

    * **Konfiguracji sieci klastrów** — skrypt wdrożenia automatycznie konfiguruje klaster HPC Pack w topologii 5 (we wszystkich węzłach w sieci firmowej). Ta topologia jest wymagany dla wszystkich wdrażania klaster HPC Pack maszyny wirtualne. Nie należy zmieniać topologia sieci klaster później.

2. **Przesuń do uruchamiania zadań węzły obliczeń w trybie online**

    Zaznacz węzły i za pomocą akcji **Przejdź do trybu Online** w Menedżerze klaster HPC. Węzły są gotowe do uruchomienia zadania.

3. **Przesyłanie zadań z klastrem**

    Nawiązywanie połączenia z węzła głównego przesyłać zadania lub skonfigurować na komputerze lokalnym wykonaj następujące czynności. Aby uzyskać informacje zobacz [Przesyłanie zadania z klastrem HPC platformy Azure](virtual-machines-windows-hpcpack-cluster-submit-jobs.md).

4. **Węzły przełączyć do trybu offline i zatrzymywania (cofnąć) ich**

    Po zakończeniu zadania uruchomionego podjąć w Menedżerze klaster HPC węzły w trybie offline. Następnie należy użyć narzędzi do zarządzania Azure, aby je zamknąć.



## <a name="run-mpi-applications-on-the-cluster"></a>Uruchamianie aplikacji MPI w klastrze

### <a name="example-run-mpipingpong-on-an-hpc-pack-cluster"></a>Przykład: Uruchamianie mpipingpong w klastrze HPC Pack

Aby sprawdzić wdrożenia usługi HPC Pack RDMA dostępem do wystąpień, uruchom polecenie **mpipingpong** HPC Pack w klastrze. **mpipingpong** wysyła pakiety danych między iloczynów węzły wielokrotnie, aby obliczyć pomiary przepustowość i opóźnienie i statystyki sieci z obsługą RDMA aplikacji. W tym przykładzie przedstawiono typowe deseń uruchomienia zadania MPI (w tym przypadku **mpipingpong**) za pomocą polecenia **mpiexec** klaster.

W tym przykładzie założono, że dodana Azure węzły w konfiguracji "serii Azure" ([Scenariusz 1](#scenario-1.-deploy-compute-intensive-worker-role-instances-(PaaS) in this article). Jeśli wdrożono HPC Pack w klastrze maszyny wirtualne Azure, musisz zmodyfikować składni polecenia określ inny węzeł grupy i ustawiać zmienne środowiska dodatkowe, aby skierować ruch sieciowy do sieci RDMA.


Aby uruchomić mpipingpong w klastrze:


1. W węźle głowy lub na komputerze klienckim poprawnie skonfigurowany Otwórz wiersz polecenia.

2. Aby oszacować opóźnienie między parami węzłów na wdrożenia usługi Azure serii węzłów 4, wpisz następujące polecenie, aby przesłać zadanie do uruchomienia mpipingpong o rozmiarze małych pakietów i dużej liczby iteracji:

    ```
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 1:100000 -op -s nul
    ```

    Polecenie zwraca identyfikator zadania, który jest przesyłany.

    Wdrożenie klaster HPC Pack wdrożone na maszyny wirtualne Azure, określ grupę węzeł, która zawiera obliczenia węzeł maszyny wirtualne rozmieszczany w usłudze w chmurze pojedynczy i zmodyfikuj polecenie **mpiexec** w następujący sposób:

    ```
    job submit /nodegroup:vmcomputenodes /numnodes:4 mpiexec -c 1 -affinity -env MSMPI_DISABLE_SOCK 1 -env MSMPI_PRECONNECT all -env MPICH_NETMASK 172.16.0.0/255.255.0.0 mpipingpong -p 1:100000 -op -s nul
    ```

3. Po zakończeniu zadania, aby wyświetlić wynik (w tym przypadku wynik zadania 1 zadania), wpisz następujące polecenie

    ```
    task view <JobID>.1
    ```

    miejsce, w którym &lt; *JobID* &gt; jest identyfikator zadania, które zostało przesłane.

    Wynik będzie zawierać opóźnienie wyniki podobne do następujących.

    ![Polecenie ping pong opóźnienie][pingpong1]

4. Aby oszacować przepustowości między parami węzłów Azure serii, wpisz następujące polecenie, aby przesłać zadanie do uruchomienia **mpipingpong** z rozmiar pakietu dużych i małych liczba iteracji:

    ```
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 4000000:1000 -op -s nul
    ```

    Polecenie zwraca identyfikator zadania, który jest przesyłany.

    Na klastrze HPC Pack wdrożone na maszyny wirtualne Azure zmodyfikuj polecenia, które zostały wymienione w kroku 2.

5. Po zakończeniu zadania, aby wyświetlić wynik (w tym przypadku wynik zadania 1 zadania), wpisz następujące polecenie:

    ```
    task view <JobID>.1
    ```

  Wynik będzie zawierać przepustowość wyniki podobne do następujących.

  ![Polecenie ping pong przepustowość][pingpong2]


### <a name="mpi-application-considerations"></a>Uwagi dotyczące aplikacji MPI


Poniżej przedstawiono informacje dotyczące korzystania z aplikacji MPI pakietem HPC platformy Azure. Niektóre mają zastosowanie jedynie do wdrożenia węzłów Azure (wystąpienia roli Pracownik dodane w konfiguracji "serii Azure").

* Pracownik wystąpienia rola w usłudze w chmurze okresowo są reprovisioned bez wcześniejszego zawiadomienia przez Azure (na przykład obsługi systemu lub w przypadku wystąpienia awarii). Jeśli wystąpienie jest reprovisioned, gdy jest uruchomiona zadania MPI, wystąpienie traci wraz z danymi i zwraca do stanu, gdy został on najpierw rozmieszczony, co może spowodować MPI zadanie nie powiedzie się. Więcej węzłów używanego dla pojedynczego zadania MPI i już zadania jest uruchomiony, tym bardziej prawdopodobne, że będzie jednego wystąpienia reprovisioned podczas pracy jest uruchomiony. Warto także to jeśli wyznaczyć jeden węzeł w ramach wdrożenia na serwerze plików.


* Aby uruchomić zadania MPI w Azure, nie musisz użyć RDMA dostępem do wystąpienia. Możesz użyć dowolnego rozmiaru wystąpienie, obsługiwany przez pakiet HPC. Jednak zaleca się wystąpienia RDMA dostępem do uruchamiania stosunkowo dużych zadań MPI, które są poufne opóźnienie i przepustowość sieci, która łączy węzły. Jeśli inne rozmiary umożliwia wykonywanie zadań MPI zależne opóźnienie i przepustowość, zalecamy uruchamiania małych zadania, w których pojedynczego zadania działa na tylko kilka węzły.

* Aplikacje wdrożonych w wystąpieniach Azure będą podlegać postanowienia licencyjne skojarzone z aplikacją. Skontaktuj się z dostawcą komercyjnego stosowania licencji lub innych ograniczeń do uruchamiania w chmurze. Nie wszystkie dostawców oferuje, płatne licencjonowania.


* Azure wystąpienia potrzebujesz dalszej konfiguracji węzły lokalnej programu access, akcji i serwery licencji. Na przykład aby włączyć Azure węzły uzyskać dostęp do lokalnego serwera licencji, można skonfigurować Azure wirtualnej sieci witryny do witryny.


* Do uruchamiania aplikacji MPI na wystąpienia Azure, zarejestruj się w każdej aplikacji MPI Zapora systemu Windows na wystąpienia, uruchamiając polecenie **hpcfwutil** . Dzięki temu komunikacji MPI nastąpić na porcie przypisany dynamiczne przez zaporę.

    >[AZURE.NOTE] Dla serii do wdrożenia Azure można również skonfigurować polecenie wyjątku zapory są uruchamiane automatycznie dla wszystkich nowych węzłów Azure, które zostaną dodane do klaster. Po uruchomieniu polecenia **hpcfwutil** i sprawdź, czy aplikacja działa, Dodaj polecenie do skryptu uruchamiania dla usługi Azure węzłów. Aby uzyskać więcej informacji zobacz [Używanie skryptu uruchamiania dla węzłów Azure](https://technet.microsoft.com/library/jj899632.aspx).



* HPC Pack użyto zmiennej środowiska klaster CCP_MPI_NETMASK, aby określić zakres adresów dopuszczalne komunikacji MPI. Uruchamianie w HPC Pack 2012 R2, zmiennej środowiska klaster CCP_MPI_NETMASK dotyczy tylko MPI komunikacji między domeny obliczeń węzłach (obsługiwanych lokalnie lub w maszyny wirtualne Azure). Zmienna jest ignorowana przez węzły dodane w serii Azure konfiguracji.


* MPI zadania nie są uruchamiane w jego wystąpieniach Azure, wdrożonych w chmurze różnych usługach (na przykład w serii do wdrożenia Azure za pomocą szablonów inny węzeł lub węzły obliczeń maszyn wirtualnych Azure wdrożony w wielu usług w chmurze). Jeśli masz wiele Azure węzeł wdrożonych są uruchamiane przy użyciu innego węzła Szablony, zadania MPI musi działać na tylko jeden zestaw węzłów Azure.


* Po dodaniu Azure węzłów na klaster i je przełączyć do trybu online, usługa Harmonogram zadań HPC natychmiast próbuje uruchomić zadań w węzłach. Jeśli tylko część z pracą można uruchamiać na Azure, upewnij się, możesz zaktualizować lub tworzenie szablonów zadań, aby zdefiniować, jakie typy zadań można uruchamiać na Azure. Na przykład aby upewnić się, że zadania przekazane za pomocą szablonu zadania uruchamiać tylko na węzły Azure, Dodaj właściwość węzeł grupy do szablonu zadania i wybierz polecenie AzureNodes jako żądaną wartość. Aby utworzyć grupy niestandardowe dla węzły Azure, użyj polecenia cmdlet programu PowerShell HPC HpcGroup Dodaj.


## <a name="next-steps"></a>Następne kroki

* Zamiast HPC Pack opracować z usługą Azure partii do uruchamiania aplikacji MPI na zarządzanych pul węzłów obliczeń w Azure. Zobacz [Używanie wielu wystąpień zadania do uruchamiania aplikacji wiadomości przechodzące Interface (MPI) w partii Azure](../batch/batch-mpi.md).

* Jeśli chcesz uruchomić Linux MPI aplikacjom uzyskiwać dostęp do sieci Azure RDMA, zobacz [Konfigurowanie klastrze Linux RDMA do uruchamiania aplikacji MPI](virtual-machines-linux-classic-rdma-cluster.md).

<!--Image references-->
[burst]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/burst.png
[iaas]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/iaas.png
[pingpong1]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/pingpong1.png
[pingpong2]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/pingpong2.png