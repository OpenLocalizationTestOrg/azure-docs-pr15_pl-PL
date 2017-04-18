<properties
 pageTitle="Uruchom OpenFOAM pakietem HPC na maszyny wirtualne Linux | Microsoft Azure"
 description="Wdrażanie klastrze Microsoft HPC Pack Azure i uruchomić zadanie OpenFOAM na wielu węzłach obliczeń Linux przez sieć RDMA."
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager,hpc-pack"/>
<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="07/22/2016"
 ms.author="danlep"/>

# <a name="run-openfoam-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>Uruchamianie OpenFoam z Microsoft HPC Pack w klastrze Linux RDMA platformy Azure

Ten artykuł zawiera jeden sposób, aby uruchomić OpenFoam w środowisku maszyn wirtualnych Azure. W tym miejscu należy wdrożyć klastrze Microsoft HPC Pack Linux obliczeń węzłów na Azure i uruchom [OpenFoam](http://openfoam.com/) zadania z MPI firmy Intel. Umożliwia RDMA dostępem do Azure maszyny wirtualne dla węzłów obliczeń, aby węzły obliczeń komunikować się przez sieć Azure RDMA. Inne opcje, aby uruchomić OpenFoam w Azure zawierać pełni skonfigurowanym komercyjnego obrazów dostępnych na rynku, takich jak osoby UberCloud [2.3 OpenFoam CentOS 6](https://azure.microsoft.com/marketplace/partners/ubercloud/openfoam-v2dot3-centos-v6/), uruchamiając na [Partię Azure](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/). 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

OpenFOAM (w przypadku otwarcia pracy i manipulowania) jest pakiet oprogramowania dynamics objętości obliczeniowa open source (CFD), który jest często używane w techniczny i naukowych w organizacjach prowadzących i academic. Zawiera narzędzia dla meshing, szczególnie snappyHexMesh, parallelized mesher złożonych CAD mają geometrię i przed i po przetwarzania. Prawie wszystkie procesy równolegle, umożliwiając użytkownikom w pełni korzystać z komputera dysponują.  

Microsoft HPC Pack zawiera funkcje, które można uruchomić HPC na dużą skalę i równoległe aplikacji, w tym aplikacji MPI na klastrów środowisku maszyn wirtualnych systemu Microsoft Azure. HPC Pack obsługuje również uruchomionego HPC Linux aplikacji w systemie Linux obliczyć węzeł, które są wdrażane za pośrednictwem SMS w klastrze HPC Pack. Wprowadzenie do Linux obliczeń węzły za pomocą HPC Pack, zobacz [Rozpoczynanie pracy z Linux węzły do uruchamiania w klastrze HPC Pack platformy Azure](virtual-machines-linux-classic-hpcpack-cluster.md) .

>[AZURE.NOTE] W tym artykule pokazano, jak przeprowadzić Linux MPI obciążenie pracą z dodatkiem Service Pack HPC. Przyjęto założenie, że masz kilka znanych z administrowanie systemem Linux i obciążenia MPI uruchomionych dla klastrów Linux. Jeśli korzystasz z wersji MPI i OpenFOAM inne niż przedstawione w tym artykule, może być konieczne modyfikowanie kilka czynności, instalacja i konfiguracja. 

## <a name="prerequisites"></a>Wymagania wstępne

*   **Klaster HPC Pack z systemem Linux RDMA dostępem do obliczenia węzły** - wdrożyć klastrze HPC Pack rozmiar A8, A9, H16r lub H16rm Linux obliczeń węzły za pomocą [Menedżera zasobów Azure szablonu](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) lub [skrypt programu PowerShell Azure](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md). Zobacz [Rozpoczynanie pracy z Linux węzły do uruchamiania w klastrze HPC Pack platformy Azure](virtual-machines-linux-classic-hpcpack-cluster.md) dla wymagania wstępne i instrukcje dotyczące obu opcji. Jeśli wybierzesz opcję wdrożenia skrypt programu PowerShell, zobacz przykładowy plik konfiguracyjny w przykładowe pliki na końcu tego artykułu. Wdrażanie klaster oparte na platformie Azure HPC Pack składa się z węzła głównego A8 Windows Server 2012 R2 rozmiar i 2 węzły obliczeń A8 SUSE Linux Enterprise Server 12 rozmiar za pomocą tej konfiguracji. Należy zastąpić odpowiednie wartości dla nazwy subskrypcji i usługi. 

    **Co jeszcze należy wiedzieć**

    *   Dla Linux RDMA sieci wymagania wstępne dotyczące platformy Azure zobacz [informacje o serii H i obliczeniowych maszyny wirtualne A serii](virtual-machines-windows-a8-a9-a10-a11-specs.md).

    *   Jeśli używasz opcji wdrażania skrypt programu Powershell wdrażać wszystkich węzłów obliczeń Linux w ramach usługi w chmurze jeden umożliwia połączenie sieciowe RDMA.

    *   Po wdrożeniu węzły Linux, łączenie przez SSH do wykonywania wszystkich dodatkowych zadań administracyjnych. Znajdź szczegóły połączenia SSH dla każdego maszyn wirtualnych Linux w portalu Azure.  
        
*   **Intel MPI** - uruchamianie OpenFOAM na SLES 12 HPC obliczyć węzłów na Azure, należy zainstalować środowisko uruchomieniowe firmy Intel MPI biblioteki 5 z [witryny Intel.com](https://software.intel.com/en-us/intel-mpi-library/). (5 MPI firmy Intel jest preinstalowany na podstawie CentOS HPC obrazów).  W ostatnim kroku w razie potrzeby zainstalować MPI firmy Intel na węzły obliczeń Linux. Aby przygotować się do tego kroku, po zarejestrowaniu z firmy Intel, wykonaj łącze w wiadomości e-mail z potwierdzeniem na stronie sieci web pokrewnych. Następnie należy skopiować łącze pobierania pliku .tgz dla odpowiedniej wersji MPI firmy Intel. Ten artykuł jest oparty na MPI firmy Intel wersji 5.0.3.048.

*   **Pakiet źródła OpenFOAM** — pobieranie oprogramowania pakiecie źródłowym OpenFOAM Linux z [witryny OpenFOAM Foundation](http://openfoam.org/download/2-3-1-source/). Ten artykuł jest oparty na pakiecie źródłowym wersji 2.3.1 dostępna do pobrania jako OpenFOAM 2.3.1.tgz. Postępuj zgodnie z instrukcjami w dalszej części tego artykułu, aby Rozpakowywanie i kompilowania OpenFOAM w węzłach obliczeń Linux.

*   **EnSight** (opcjonalnie) — aby wyświetlić wyniki symulacji OpenFOAM, Pobierz i zainstaluj program wizualizacji i analiz [EnSight](https://www.ceisoftware.com/download/) . Licencjonowanie i Pobierz informacje są w witrynie EnSight.


## <a name="set-up-mutual-trust-between-compute-nodes"></a>Konfigurowanie wzajemnego zaufania między węzły obliczeń

Uruchamianie zadania węzeł między na wielu węzłach Linux wymaga węzły ufać sobie wzajemnie (przez **rsh** lub **ssh**). Po utworzeniu klaster HPC Pack scenariusz wdrażania programu Microsoft HPC Pack IaaS skrypt automatycznie konfiguruje trwały wzajemnego zaufania dla konta administratora, który można określić. Dla użytkowników niebędących administratorami tworzonych w domenie klaster należy skonfigurować tymczasowe wzajemnego zaufania między węzły, gdy zadanie jest przydzielone do nich, a destroy relacji po ukończeniu zadania. Aby ustanowić zaufanie dla każdego użytkownika, podaj pary kluczy RSA do klastrów, która HPC Pack używa relacji zaufania.

### <a name="generate-an-rsa-key-pair"></a>Generowanie pary kluczy RSA

Jest proste wygenerować para kluczy RSA, która zawiera klucz publiczny i klucz prywatny, uruchamiając polecenie **ssh-keygen** Linux.

1.  Zaloguj się do komputera Linux.

2.  Uruchom następujące polecenie:

    ```
    ssh-keygen -t rsa
    ```

    >[AZURE.NOTE] Naciśnij klawisz **Enter** , aby korzystać z domyślnych ustawień, dopóki nie zakończy się polecenie. Nie wprowadzaj hasło miejscu; Po wyświetleniu monitu o podanie hasła, po prostu naciśnij klawisz **Enter**.

    ![Generowanie pary kluczy RSA][keygen]

3.  Zmień katalog do katalogu ~/.ssh. Klucz prywatny jest przechowywany w id_rsa i kluczem publicznym w id_rsa.pub.

    ![Klucze prywatne i publiczne][keys]

### <a name="add-the-key-pair-to-the-hpc-pack-cluster"></a>Dodawanie do klastrów HPC Pack pary kluczy
1.  Nawiązywanie połączenia pulpitu zdalnego z węzła głównego przy użyciu konta administratora HPC Pack (konta administratora, które możesz skonfigurować po uruchomieniu skrypt wdrożenia).

2. Standardowe procedury Windows Server umożliwia tworzenie konta domeny w domenie usługi Active Directory klaster. Na przykład narzędzie użytkowników usługi Active Directory i komputery na węzła głównego. W przykładach w tym artykule założono, że tworzenie domeny użytkownika o nazwie hpclab\hpcuser.

3.  Tworzenie pliku o nazwie C:\cred.xml i skopiuj danych klucza RSA do niego. Przykładowy plik cred.xml znajduje się na końcu tego artykułu.

    ```
    <ExtendedData>
        <PrivateKey>Copy the contents of private key here</PrivateKey>
        <PublicKey>Copy the contents of public key here</PublicKey>
    </ExtendedData>
    ```

4.  Otwórz wiersz polecenia i wpisz następujące polecenie, aby ustawić danych poświadczeń konta hpclab\hpcuser. Możesz użyć parametru **extendeddata** w celu przekazania nazwę C:\cred.xml plik, który został utworzony najważniejszych danych.

    ```
    hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
    ```

    To polecenie zakończy się pomyślnie bez danych wyjściowych. Po ustawieniu poświadczeń dla kont użytkowników, które są potrzebne do uruchamiania zadań, przechowywać plik cred.xml w bezpiecznym miejscu lub go usunąć.

5.  Jeśli pary kluczy RSA został wygenerowany na jednym z węzły Linux, pamiętaj, aby usunąć klucze po zakończeniu korzystania z nich. Jeśli HPC Pack zostaną wykryte istniejący plik id_rsa lub plik id_rsa.pub, nie ustawiono wzajemnej relacji zaufania.

>[AZURE.IMPORTANT] Nie zalecamy uruchamiania zadanie Linux jako administrator klastrów w klastrze udostępnionego, ponieważ zadanie przesłane przez administratora uruchamiana konta głównego w węzłach Linux. Jednak zadanie przesłany przez użytkownika niebędący administratorami uruchamiana lokalnego konta użytkownika Linux o takiej samej nazwie jak użytkownik zadania. W tym przypadku HPC Pack automatycznie konfiguruje wzajemnego zaufania dla tego użytkownika Linux w węzłach przydzielonych do zadania. Możesz skonfigurować użytkownika Linux ręcznie w węzłach Linux przed uruchomieniem zadania lub HPC Pack użytkownik automatycznie tworzy podczas przesyłania zadania. Jeśli HPC Pack tworzy użytkownika, HPC Pack usunie ją, po zakończeniu zadania. Aby zmniejszyć zagrożeniami, HPC Pack usuwa klucze zakończenia zadania.

## <a name="set-up-a-file-share-for-linux-nodes"></a>Konfigurowanie udziału plików dla węzłów Linux

Teraz skonfigurować standardowy udziałem SMB w folderze w węzła głównego. Aby umożliwić węzły Linux oraz dostęp do plików aplikacji ze ścieżką typowych, należy zainstalować folder udostępniony na węzły Linux. Można również użyć innego pliku udostępniania opcję, na przykład udostępnianie plików Azure - zalecane dla wielu scenariuszy - lub udziału NFS. Zobacz plik udostępniania informacji i uzyskać szczegółowe instrukcje na [wprowadzenie Linux węzły do uruchamiania w klastrze HPC Pack platformy Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

1.  Utwórz folder na węzła głównego i udostępnij go dla wszystkich osób, ustawiając uprawnienia do odczytu/zapisu. Na przykład udostępnianej C:\OpenFOAM węzła głównego jako \\ \\SUSE12RDMA HN\OpenFOAM. W tym miejscu *SUSE12RDMA HN* to nazwa hosta węzła głównego.

2.  Otwórz okno programu Windows PowerShell i uruchom następujące polecenia:

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /openfoam

    clusrun /nodegroup:LinuxNodes mount -t cifs //SUSE12RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

Pierwsze polecenie tworzy folder o nazwie /openfoam na wszystkich węzłach w grupie LinuxNodes. Drugie polecenie instaluje //SUSE12RDMA-HN/OpenFOAM folderu udostępnionego w węzłach Linux z dir_mode i file_mode bitów Ustaw 777. *Nazwy użytkownika* i *hasła* , w tym poleceniu należy poświadczenia użytkownika na węzła głównego.

>[AZURE.NOTE]"\`" Symbol w drugim poleceniu jest symbolem anulowania dla programu PowerShell. "\`," oznacza "," (znak przecinka) jest częścią polecenia.

## <a name="install-mpi-and-openfoam"></a>Zainstaluj MPI i OpenFOAM

Aby uruchomić OpenFOAM jako zadanie MPI sieci RDMA, musisz skompilować OpenFOAM z bibliotekami MPI firmy Intel. 

Należy uruchomić kilka poleceń **clusrun** , aby zainstalować bibliotek MPI firmy Intel (Jeśli nie jest jeszcze zainstalowany) i OpenFOAM na węzły Linux. Za pomocą Udostępnij węzła głównego wcześniej skonfigurowane do udostępniania plików instalacji między węzły Linux.

>[AZURE.IMPORTANT]Te instalacji i kompilowanie kroki przedstawiają. Potrzebujesz pewnej wiedzy administrowania systemem Linux, aby upewnić się, że zależne jak i w bibliotekach są poprawnie zainstalowane. Może być konieczne modyfikowanie pewnych zmiennych środowiska i innych ustawień Twojej wersji MPI firmy Intel i OpenFOAM. Aby uzyskać szczegółowe informacje zobacz [Biblioteki MPI firmy Intel Linux instalacji przewodnika](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html) i [Instalacja pakietu źródła OpenFOAM](http://openfoam.org/download/2-3-1-source/) w środowisku usługi.


### <a name="install-intel-mpi"></a>Instalowanie firmy Intel MPI

Zapisywanie pobranego pakietu instalacyjnego dla MPI firmy Intel (l_mpi_p_5.0.3.048.tgz w tym przykładzie) w C:\OpenFoam na węzła głównego tak, aby węzły Linux oraz skorzystać z tego pliku przy użyciu /openfoam. Następnie uruchom **clusrun** do zainstalowania biblioteki MPI firmy Intel w węzłach Linux.

1.  Następujące polecenia Skopiuj pakiet instalacyjny i wyodrębnić je do /opt/intel w każdym węźle.

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /opt/intel

    clusrun /nodegroup:LinuxNodes cp /openfoam/l_mpi_p_5.0.3.048.tgz /opt/intel/

    clusrun /nodegroup:LinuxNodes tar -xzf /opt/intel/l_mpi_p_5.0.3.048.tgz -C /opt/intel/
    ```

2.  Aby zainstalować biblioteki MPI firmy Intel w trybie cichym, należy użyć pliku silent.cfg. Przykład można znaleźć w przykładowe pliki na końcu tego artykułu. W /openfoam folderu udostępnionego, należy umieścić ten plik. Aby uzyskać szczegółowe informacje o pliku silent.cfg zobacz [Bibliotekę MPI firmy Intel Linux instalacji przewodnika - odbiorcze instalacji](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html#silentinstall).

    >[AZURE.TIP]Upewnij się, że Zapisz plik jako plik tekstowy z systemem Linux silent.cfg linii zakończenia (tylko wysuwu wiersza, nie CR wysuwu wiersza). Wykonanie tej czynności gwarantuje został prawidłowo uruchomiony w węzłach Linux.

3.  Zainstaluj biblioteki MPI firmy Intel w trybie cichym.
 
    ```
    clusrun /nodegroup:LinuxNodes bash /opt/intel/l_mpi_p_5.0.3.048/install.sh --silent /openfoam/silent.cfg
    ```
    
### <a name="configure-mpi"></a>Konfigurowanie MPI

Do testowania, należy dodać kolejne wiersze do /etc/security/limits.conf w węzłach Linux:


    clusrun /nodegroup:LinuxNodes echo "*               hard    memlock         unlimited" `>`> /etc/security/limits.conf
    clusrun /nodegroup:LinuxNodes echo "*               soft    memlock         unlimited" `>`> /etc/security/limits.conf


Po zaktualizowaniu pliku limits.conf, uruchom ponownie węzły Linux. Na przykład użyj następującego polecenia **clusrun** :

```
clusrun /nodegroup:LinuxNodes systemctl reboot
```

Po ponownym uruchomieniu komputera, upewnij się, że folder udostępniony jest zainstalowany jako /openfoam.

### <a name="compile-and-install-openfoam"></a>Skompilować i zainstalować OpenFOAM

Zapisać na węzła głównego pobranego pakietu instalacyjnego pakietu źródła OpenFOAM (w tym przykładzie OpenFOAM-2.3.1.tgz) w celu C:\OpenFoam, tak aby węzły Linux oraz skorzystać z tego pliku przy użyciu /openfoam. Następnie uruchomić polecenia **clusrun** skompilować OpenFOAM na wszystkich węzłach Linux.


1.  Tworzenie folder /opt/OpenFOAM w każdym węźle Linux, skopiuj pakiet źródła do tego folderu i wyodrębnij go.

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /opt/OpenFOAM

    clusrun /nodegroup:LinuxNodes cp /openfoam/OpenFOAM-2.3.1.tgz /opt/OpenFOAM/
    
    clusrun /nodegroup:LinuxNodes tar -xzf /opt/OpenFOAM/OpenFOAM-2.3.1.tgz -C /opt/OpenFOAM/
    ```

2.  Aby skompilować OpenFOAM z biblioteką MPI firmy Intel, najpierw skonfiguruj niektóre zmienne środowiska dla firmy Intel MPI i OpenFOAM. Za pomocą skryptu imprezie o nazwie settings.sh ustawienie zmiennych. Przykład można znaleźć w przykładowe pliki na końcu tego artykułu. W /openfoam folderu udostępnionego, należy umieścić ten plik (zapisany z końców Linux). Ten plik zawiera również ustawienia obsługi MPI i OpenFOAM, umożliwiające później uruchomić zadanie OpenFOAM.

3. Zainstalować pakiety zależne niezbędne do opracowania OpenFOAM. W zależności od usługi rozkład Linux najpierw należy dodawanie repozytorium. Uruchom polecenia **clusrun** podobny do następującego:

    ```
    clusrun /nodegroup:LinuxNodes zypper ar http://download.opensuse.org/distribution/13.2/repo/oss/suse/ opensuse
    
    clusrun /nodegroup:LinuxNodes zypper -n --gpg-auto-import-keys install --repo opensuse --force-resolution -t pattern devel_C_C++
    ```
    
    W razie potrzeby SSH każdy węzeł Linux, aby uruchomić polecenia, aby potwierdzić, że działają poprawnie.

4.  Uruchom następujące polecenie, aby skompilować OpenFOAM. Proces kompilacji trwa niektórych i generuje dużej ilości informacji dziennika pojawi się, tak aby wyświetlić wynik interleaved za pomocą opcji **/ interleaved** .

    ```
    clusrun /nodegroup:LinuxNodes /interleaved source /openfoam/settings.sh `&`& /opt/OpenFOAM/OpenFOAM-2.3.1/Allwmake
    ```
    
    >[AZURE.NOTE]"\`" Symbol w poleceniu jest symbolem anulowania dla programu PowerShell. "\`&" oznacza "&" jest częścią polecenia.

## <a name="prepare-to-run-an-openfoam-job"></a>Przygotowywanie się do wykonywania zadania OpenFOAM

Przygotuj teraz uruchomić zadanie MPI o nazwie sloshingTank3D, który jest jednym z próbki OpenFoam, w dwóch węzłów Linux. 

### <a name="set-up-the-runtime-environment"></a>Konfigurowanie środowiska runtime

Aby skonfigurować środowisko uruchomieniowe środowiskach MPI i OpenFOAM w węzłach Linux, uruchom następujące polecenie w oknie programu Windows PowerShell na węzła głównego. (To polecenie jest prawidłowe SUSE Linux.)

```
clusrun /nodegroup:LinuxNodes cp /openfoam/settings.sh /etc/profile.d/
```

### <a name="prepare-sample-data"></a>Przygotowywanie przykładowych danych

Za pomocą Udostępnij węzła głównego, które skonfigurowano wcześniej do udostępniania plików między węzły Linux (zainstalowany jako /openfoam).

1.  SSH do jednego z Linux obliczyć węzły.

2.  Uruchom następujące polecenie, aby skonfigurować środowisko środowisko uruchomieniowe OpenFOAM, jeśli nie zrobiono tego.

    ```
    $ source /openfoam/settings.sh
    ```
    
3.  Skopiuj przykładowe sloshingTank3D do folderu udostępnionego i przejdź do niej.

    ```
    $ cp -r $FOAM_TUTORIALS/multiphase/interDyMFoam/ras/sloshingTank3D /openfoam/

    $ cd /openfoam/sloshingTank3D
    ```

4.  Użycie domyślne parametry w tym przykładzie, może minąć dziesiątki minut, więc warto zmodyfikować niektóre parametry, aby stał się szybciej. Jeden wybór proste, jest zmodyfikowanie czasu kroku zmienne deltaT i writeInterval w pliku system-controlDict. Ten plik zawiera wszystkie dane wejściowe odnoszących się do kontroli nad czasu i czytania i pisania dane rozwiązanie. Na przykład możesz zmienić wartość deltaT z 0,05 do 0,5 i wartość writeInterval z 0,05 do 0,5.

    ![Modyfikowanie zmiennych kroku][step_variables]

5.  Określ żądane wartości dla zmiennych w pliku system-decomposeParDict. W tym przykładzie użyto dwóch węzłów Linux, każdy z 8 rdzeniom, więc równa numberOfSubdomains 16, a n hierarchicalCoeffs do (1 1 16), co oznacza równolegle OpenFOAM z 16 procesów. Aby uzyskać więcej informacji, zobacz [OpenFOAM User Guide: 3.4 aplikacji uruchomiony równolegle](http://cfd.direct/openfoam/user-guide/running-applications-parallel/#x12-820003.4).

    ![Których przedstawiany procesów][decompose]

6.  Uruchom następujące polecenia z katalogu sloshingTank3D przygotowywanie przykładowych danych.

    ```
    $ . $WM_PROJECT_DIR/bin/tools/RunFunctions

    $ m4 constant/polyMesh/blockMeshDict.m4 > constant/polyMesh/blockMeshDict

    $ runApplication blockMesh

    $ cp 0/alpha.water.org 0/alpha.water

    $ runApplication setFields  
    ```
    
7.  W węźle głowy powinien pojawić się ekran przykładowe pliki danych są kopiowane do C:\OpenFoam\sloshingTank3D. (C:\OpenFoam jest folder udostępniony na węzła głównego).

    ![Pliki danych na węzła głównego][data_files]

### <a name="host-file-for-mpirun"></a>Plik hosta dla mpirun

W tym kroku utworzysz pliku hosta (Lista węzłów obliczeń), który używa polecenia **mpirun** .

1.  Na jednym z węzłów Linux oraz tworzenie pliku o nazwie hostfile w obszarze /openfoam, aby ten plik można się z Tobą skontaktować /openfoam/hostfile we wszystkich węzłach Linux.

2.  Napisz nazwiska węzła Linux do tego pliku. W tym przykładzie plik zawiera następujące nazwy:
    
    ```       
    SUSE12RDMA-LN1
    SUSE12RDMA-LN2
    ```
    
    >[AZURE.TIP]Można także utworzyć ten plik na C:\OpenFoam\hostfile na węzła głównego. Jeśli wybierzesz tę opcję, zapisz go jako plik tekstowy z systemem Linux zakończenia wierszy (tylko wysuwu wiersza, nie CR wysuwu wiersza). Dzięki temu został prawidłowo uruchomiony w węzłach Linux.

    **Opakowanie skrypt imprezie**

    Jeśli masz wiele węzłów Linux i uruchomić tylko niektóre z nich do zadania, nie jest dobrym pomysłem jest używany plik stały hosta ponieważ nie wiadomo, które węzły zostaną przydzielone do zadania. W tym przypadku Napisz opakowanie skrypt imprezie dla **mpirun** automatycznie utworzyć plik hosta. Możesz znaleźć opakowanie skrypt imprezie przykład o nazwie hpcimpirun.sh na końcu tego artykułu i zapisz go jako /openfoam/hpcimpirun.sh. Ten przykładowy skrypt wykonuje następujące czynności:

    1.  Konfiguruje zmienne środowiska **mpirun**i niektórych parametrów polecenia dodanie do wykonywania zadania MPI przez sieć RDMA. W tym przypadku ustawia następujących czynników:

        *   I_MPI_FABRICS = shm:dapl
        *   I_MPI_DAPL_PROVIDER = ustawienia — w wersji 2 ib0
        *   I_MPI_DYNAMIC_CONNECTION = 0

    2.  Tworzy plik hosta według środowiska zmiennych $CCP_NODES_CORES, który jest ustawiany przez węzła głównego HPC, gdy zadanie jest aktywna.

        Format $CCP_NODES_CORES tego wzorca są następujące:

        ```
        <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
        ```

        gdzie

        * `<Number of nodes>`-liczby węzłów przydzielone do tego zadania.  
        
        * `<Name of node_n_...>`-Nazwa każdego węzła przydzielone do tego zadania.
        
        * `<Cores of node_n_...>`-liczby rdzeni w węźle przydzielone do tego zadania.

        Na przykład jeśli zadanie wymaga dwóch węzłów do uruchomienia, $CCP_NODES_CORES jest podobne do
        
        ```
        2 SUSE12RDMA-LN1 8 SUSE12RDMA-LN2 8
        ```
        
    3.  Wywołuje polecenie **mpirun** i dołącza dwoma parametrami w wierszu polecenia.

        * `--hostfile <hostfilepath>: <hostfilepath>`-Ścieżka pliku hosta skrypt tworzy

        * `-np ${CCP_NUMCPUS}: ${CCP_NUMCPUS}`-zmiennej środowiska określonych przez węzła głównego HPC Pack, który przechowuje liczbę całkowitą rdzeniom przydzielone do tego zadania. W tym przypadku określa liczbę procesów dla **mpirun**.


## <a name="submit-an-openfoam-job"></a>Prześlij zadanie OpenFOAM

Teraz można przesłać zadanie w Menedżerze klaster HPC. Należy przekazać hpcimpirun.sh skryptu w wierszu polecenia w przypadku niektórych zadań zadania.

1. Nawiązywanie połączenia z węzła głowy i uruchomić Menedżera klaster HPC.

2. **W zarządzania zasobami**, upewnij się, że węzły obliczeń Linux są w stanie **Online** . Jeśli nie są one, zaznacz je i kliknij przycisk **Przejdź do trybu Online**.

3.  W obszarze **Zarządzanie zadaniami**kliknij przycisk **Nowe zadanie**.

4.  Wprowadź nazwę zadania, takie jak _sloshingTank3D_.

    ![Szczegóły zadania][job_details]

5.  W **zasobów zadania**wybierz typ zasobu jako "Węzeł" i wartość minimalna 2. Ta konfiguracja uruchamia zadanie w dwóch węzłach Linux, z których każda ma osiem rdzenie w tym przykładzie.

    ![Zasoby zadania][job_resources]

6. Kliknij pozycję **Edytuj zadania** w lewym okienku nawigacji, a następnie kliknij przycisk **Dodaj** , aby dodać zadanie do zadania. Dodaj cztery zadania do zadania z następującymi wiersze polecenia i ustawienia.

    >[AZURE.NOTE]Uruchamianie `source /openfoam/settings.sh` konfiguruje środowiska środowisko uruchomieniowe OpenFOAM i MPI, więc wszystkich następujących zadań połączeń przed poleceniem OpenFOAM.

    *   **Zadanie 1**. Uruchom **decomposePar** do generowania plików danych do uruchamiania **interDyMFoam** równolegle.
    
        *   Przypisywanie jeden węzeł do zadania

        *   **Wiersz polecenia** - `source /openfoam/settings.sh && decomposePar -force > /openfoam/decomposePar${CCP_JOBID}.log`
    
        *   **Katalog roboczy** - / openfoam/sloshingTank3D
        
        Zobacz poniższej ilustracji. Podobnie można skonfigurować pozostałych zadań.

        ![Szczegóły zadania 1][task_details1]

    *   **Zadanie 2**. Równolegle **interDyMFoam** do obliczenia próbki.

        *   Przypisywanie dwa węzły do zadania

        *   **Wiersz polecenia** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh interDyMFoam -parallel > /openfoam/interDyMFoam${CCP_JOBID}.log`

        *   **Katalog roboczy** - / openfoam/sloshingTank3D

    *   **Zadanie 3**. Uruchom **reconstructPar** , aby scalić zestawy katalogów czasu z każdego katalogu processor_N_ w jeden zestaw.

        *   Przypisywanie jeden węzeł do zadania

        *   **Wiersz polecenia** - `source /openfoam/settings.sh && reconstructPar > /openfoam/reconstructPar${CCP_JOBID}.log`

        *   **Katalog roboczy** - / openfoam/sloshingTank3D

    *   **Zadanie 4**. Równolegle **foamToEnsight** Aby przekonwertować pliki wyników OpenFOAM na EnSight format i umieść pliki EnSight w katalogu o nazwie Ensight w katalogu wielkość liter.

        *   Przypisywanie dwa węzły do zadania

        *   **Wiersz polecenia** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh foamToEnsight -parallel > /openfoam/foamToEnsight${CCP_JOBID}.log`

        *   **Katalog roboczy** - / openfoam/sloshingTank3D

6.  Dodawanie zależności do tych zadań rosnąco zadania.

    ![Zależności między zadaniami][task_dependencies]

7.  Kliknij przycisk **Prześlij** , aby uruchomić to zadanie.

    Domyślnie HPC Pack przesyła zadanie jako bieżące konto logowania użytkownika. Kliknij przycisk **Prześlij**, może zostać wyświetlony monit o wprowadzenie nazwy użytkownika i hasła okna dialogowego.

    ![Poświadczenia zadania][creds]

    W niektórych sytuacjach HPC Pack zapamiętuje informacje o użytkowniku wprowadzania przed i nie wyświetla to okno dialogowe. Aby ponownie wyświetlić HPC Pack, wprowadź poniższe polecenie w wierszu polecenia, a następnie przesłać zadanie.

    ```
    hpccred delcreds
    ```

8.  Zadanie pobiera dziesiątki minut do kilku godzin zgodnie z parametrami ustawione dla próbki. W konturowy można zobaczyć zadania uruchomionych w węzłach Linux. 

    ![Konturowy][heat_map]

    W każdym węźle osiem procesy są uruchomione.

    ![Procesy Linux][linux_processes]

9.  Po zakończeniu pracy zadania można znaleźć wyników zadania w foldery w folderze C:\OpenFoam\sloshingTank3D i pliki dziennika na C:\OpenFoam.


## <a name="view-results-in-ensight"></a>Wyświetlanie wyników w EnSight

Opcjonalnie przy użyciu [EnSight](https://www.ceisoftware.com/) wizualizacji i analizowanie wyników zadania OpenFOAM. Aby uzyskać więcej informacji o wizualizacji i animacji w EnSight zobacz ten [Przewodnik wideo](http://www.ceisoftware.com/wp-content/uploads/screencasts/vof_visualization/vof_visualization.html).

1.  Po zainstalowaniu EnSight na węzła głównego, uruchom go.

2.  Otwórz C:\OpenFoam\sloshingTank3D\EnSight\sloshingTank3D.case.

    Zobacz zbiorniku w przeglądarce.

    ![Zbiorniku w EnSight][tank]

3.  Tworzenie **Isosurface** z **internalMesh**, a następnie wybierz zmiennych **alpha_water**.

    ![Tworzenie isosurface][isosurface]

4.  Ustawianie koloru dla **Isosurface_part** utworzony w poprzednim kroku. Na przykład ustaw ją do wody niebieski.

    ![Edytowanie koloru isosurface][isosurface_color]

5.  Utwórz **Głośność Iso** z **wzornika ściany** , wybierając pozycję **ściany** w panelu **części** , a następnie kliknij przycisk **Isosurfaces** na pasku narzędzi.

6.  W oknie dialogowym wybierz **Typ** jako **Isovolume** i ustaw Min zakresu **Isovolume** 0,5. Aby utworzyć isovolume, kliknij przycisk **Utwórz z wybranych częściach**.

7.  Ustawianie koloru dla **Iso_volume_part** utworzony w poprzednim kroku. Na przykład ustaw ją do głębokości wody niebieski.

8.  Ustaw kolor dla **ścian**. Na przykład ustaw ją na przejrzysty biały.

9. Kliknij przycisk **Odtwórz** , aby wyświetlić wyniki symulacji.

    ![Wynik zbiorniku na paliwo][tank_result]

## <a name="sample-files"></a>Przykładowe pliki

### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>Przykładowy plik konfiguracyjny XML do wdrożenia klaster przez skrypt programu PowerShell

 ```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>allvhdsje</StorageAccount>
  </Subscription>
  <Location>Japan East</Location>  
  <VNet>
    <VNetName>suse12rdmavnet</VNetName>
    <SubnetName>SUSE12RDMACluster</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>SUSE12RDMA-HN</VMName>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>SUSE12RDMA-LN%1%</VMNamePattern>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <NodeCount>2</NodeCount>
      <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```

### <a name="sample-credxml-file"></a>Przykładowy plik cred.xml

```
<ExtendedData>
  <PrivateKey>-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEAxJKBABhnOsE9eneGHvsjdoXKooHUxpTHI1JVunAJkVmFy8JC
qFt1pV98QCtKEHTC6kQ7tj1UT2N6nx1EY9BBHpZacnXmknpKdX4Nu0cNlSphLpru
lscKPR3XVzkTwEF00OMiNJVknq8qXJF1T3lYx3rW5EnItn6C3nQm3gQPXP0ckYCF
Jdtu/6SSgzV9kaapctLGPNp1Vjf9KeDQMrJXsQNHxnQcfiICp21NiUCiXosDqJrR
AfzePdl0XwsNngouy8t0fPlNSngZvsx+kPGh/AKakKIYS0cO9W3FmdYNW8Xehzkc
VzrtJhU8x21hXGfSC7V0ZeD7dMeTL3tQCVxCmwIDAQABAoIBAQCve8Jh3Wc6koxZ
qh43xicwhdwSGyliZisoozYZDC/ebDb/Ydq0BYIPMiDwADVMX5AqJuPPmwyLGtm6
9hu5p46aycrQ5+QA299g6DlF+PZtNbowKuvX+rRvPxagrTmupkCswjglDUEYUHPW
05wQaNoSqtzwS9Y85M/b24FfLeyxK0n8zjKFErJaHdhVxI6cxw7RdVlSmM9UHmah
wTkW8HkblbOArilAHi6SlRTNZG4gTGeDzPb7fYZo3hzJyLbcaNfJscUuqnAJ+6pT
iY6NNp1E8PQgjvHe21yv3DRoVRM4egqQvNZgUbYAMUgr30T1UoxnUXwk2vqJMfg2
Nzw0ESGRAoGBAPkfXjjGfc4HryqPkdx0kjXs0bXC3js2g4IXItK9YUFeZzf+476y
OTMQg/8DUbqd5rLv7PITIAqpGs39pkfnyohPjOe2zZzeoyaXurYIPV98hhH880uH
ZUhOxJYnlqHGxGT7p2PmmnAlmY4TSJrp12VnuiQVVVsXWOGPqHx4S4f9AoGBAMn/
vuea7hsCgwIE25MJJ55FYCJodLkioQy6aGP4NgB89Azzg527WsQ6H5xhgVMKHWyu
Q1snp+q8LyzD0i1veEvWb8EYifsMyTIPXOUTwZgzaTTCeJNHdc4gw1U22vd7OBYy
nZCU7Tn8Pe6eIMNztnVduiv+2QHuiNPgN7M73/x3AoGBAOL0IcmFgy0EsR8MBq0Z
ge4gnniBXCYDptEINNBaeVStJUnNKzwab6PGwwm6w2VI3thbXbi3lbRAlMve7fKK
B2ghWNPsJOtppKbPCek2Hnt0HUwb7qX7Zlj2cX/99uvRAjChVsDbYA0VJAxcIwQG
TxXx5pFi4g0HexCa6LrkeKMdAoGAcvRIACX7OwPC6nM5QgQDt95jRzGKu5EpdcTf
g4TNtplliblLPYhRrzokoyoaHteyxxak3ktDFCLj9eW6xoCZRQ9Tqd/9JhGwrfxw
MS19DtCzHoNNewM/135tqyD8m7pTwM4tPQqDtmwGErWKj7BaNZARUlhFxwOoemsv
R6DbZyECgYEAhjL2N3Pc+WW+8x2bbIBN3rJcMjBBIivB62AwgYZnA2D5wk5o0DKD
eesGSKS5l22ZMXJNShgzPKmv3HpH22CSVpO0sNZ6R+iG8a3oq4QkU61MT1CfGoMI
a8lxTKnZCsRXU1HexqZs+DSc+30tz50bNqLdido/l5B4EJnQP03ciO0=
-----END RSA PRIVATE KEY-----</PrivateKey>
  <PublicKey>ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDEkoEAGGc6wT16d4Ye+yN2hcqigdTGlMcjUlW6cAmRWYXLwkKoW3WlX3xAK0oQdMLqRDu2PVRPY3qfHURj0EEellpydeaSekp1fg27Rw2VKmEumu6Wxwo9HddXORPAQXTQ4yI0lWSerypckXVPeVjHetbkSci2foLedCbeBA9c/RyRgIUl227/pJKDNX2Rpqly0sY82nVWN/0p4NAyslexA0fGdBx+IgKnbU2JQKJeiwOomtEB/N492XRfCw2eCi7Ly3R8+U1KeBm+zH6Q8aH8ApqQohhLRw71bcWZ1g1bxd6HORxXOu0mFTzHbWFcZ9ILtXRl4Pt0x5Mve1AJXEKb username@servername;</PublicKey>
</ExtendedData>
```
### <a name="sample-silentcfg-file-to-install-mpi"></a>Przykładowy plik silent.cfg, aby zainstalować MPI

```
# Patterns used to check silent configuration file
#
# anythingpat - any string
# filepat     - the file location pattern (/file/location/to/license.lic)
# lspat       - the license server address pattern (0123@hostname)
# snpat       - the serial number pattern (ABCD-01234567)

# accept EULA, valid values are: {accept, decline}
ACCEPT_EULA=accept

# optional error behavior, valid values are: {yes, no}
CONTINUE_WITH_OPTIONAL_ERROR=yes

# install location, valid values are: {/opt/intel, filepat}
PSET_INSTALL_DIR=/opt/intel

# continue with overwrite of existing installation directory, valid values are: {yes, no}
CONTINUE_WITH_INSTALLDIR_OVERWRITE=yes

# list of components to install, valid values are: {ALL, DEFAULTS, anythingpat}
COMPONENTS=DEFAULTS

# installation mode, valid values are: {install, modify, repair, uninstall}
PSET_MODE=install

# directory for non-RPM database, valid values are: {filepat}
#NONRPM_DB_DIR=filepat

# Serial number, valid values are: {snpat}
#ACTIVATION_SERIAL_NUMBER=snpat

# License file or license server, valid values are: {lspat, filepat}
#ACTIVATION_LICENSE_FILE=

# Activation type, valid values are: {exist_lic, license_server, license_file, trial_lic, serial_number}
ACTIVATION_TYPE=trial_lic

# Path to the cluster description file, valid values are: {filepat}
#CLUSTER_INSTALL_MACHINES_FILE=filepat

# Intel(R) Software Improvement Program opt-in, valid values are: {yes, no}
PHONEHOME_SEND_USAGE_DATA=no

# Perform validation of digital signatures of RPM files, valid values are: {yes, no}
SIGNING_ENABLED=yes

# Select yes to enable mpi-selector integration, valid values are: {yes, no}
ENVIRONMENT_REG_MPI_ENV=no

# Select yes to update ld.so.conf, valid values are: {yes, no}
ENVIRONMENT_LD_SO_CONF=no


```

### <a name="sample-settingssh-script"></a>Przykładowy skrypt settings.sh

```
#!/bin/bash

# impi
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# openfoam
export FOAM_INST_DIR=/opt/OpenFOAM
source /opt/OpenFOAM/OpenFOAM-2.3.1/etc/bashrc
export WM_MPLIB=INTELMPI
```


###<a name="sample-hpcimpirunsh-script"></a>Przykładowy skrypt hpcimpirun.sh

```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"

# Set mpirun runtime evironment
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# mpirun command
MPIRUN=mpirun
# Argument of "--hostfile"
NODELIST_OPT="--hostfile"
# Argument of "-np"
NUMPROCESS_OPT="-np"

# Get node information from ENVs
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # CCP_NODES_CORES is not found or is empty, just run the mpirun without hostfile arg.
    ${MPIRUN} $*
else
    # Create the hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$

    # Get every node name and write into the hostfile file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the mpirun with hostfile arg
    ${MPIRUN} ${NUMPROCESS_OPT} ${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}

```





<!--Image references-->
[keygen]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/keygen.png
[keys]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/keys.png
[step_variables]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/step_variables.png
[data_files]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/data_files.png
[decompose]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/decompose.png
[job_details]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/job_details.png
[job_resources]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/job_resources.png
[task_details1]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/task_details1.png
[task_dependencies]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/task_dependencies.png
[creds]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/creds.png
[heat_map]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/heat_map.png
[tank]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/tank.png
[tank_result]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/tank_result.png
[isosurface]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/isosurface.png
[isosurface_color]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/isosurface_color.png
[linux_processes]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/linux_processes.png
