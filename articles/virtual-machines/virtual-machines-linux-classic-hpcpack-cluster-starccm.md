<properties
 pageTitle="Uruchamianie GWIAZDA-CCM + pakietem HPC na maszyny wirtualne Linux | Microsoft Azure"
 description="Wdrażać klastrze Microsoft HPC Pack Azure i uruchamiać GWIAZDA-CCM + zadania na wielu Linux obliczyć węzły przez sieć RDMA."
 services="virtual-machines-linux"
 documentationCenter=""
 authors="xpillons"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager,hpc-pack"/>
<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="09/13/2016"
 ms.author="xpillons"/>

# <a name="run-star-ccm-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>Uruchamianie GWIAZDA-CCM + z Microsoft HPC Pack na Linux RDMA klaster platformy Azure
W tym artykule pokazano, jak wdrożyć klastrze Microsoft HPC Pack na Azure i uruchom [GWIAZDA CD adapco-CCM +](http://www.cd-adapco.com/products/star-ccm%C2%AE) zadania na wielu węzłach Linux obliczeń, które są połączone z InfiniBand.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Microsoft HPC Pack zawiera funkcje, które można uruchomić różnych HPC na dużą skalę i równoległych aplikacje, w tym aplikacji MPI, dotyczących klastrów środowisku maszyn wirtualnych systemu Microsoft Azure. HPC Pack obsługuje również uruchomione aplikacje Linux HPC na maszyny wirtualne węzłów przetwarzania Linux wdrożonych w klastrze HPC Pack. Wprowadzenie do korzystania z Linux obliczyć węzły za pomocą HPC Pack, zobacz [Rozpoczynanie pracy z Linux węzły do uruchamiania w klastrze HPC Pack platformy Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

## <a name="set-up-an-hpc-pack-cluster"></a>Konfigurowanie klastrze HPC Pack
Pobierz skryptów wdrożenia HPC Pack IaaS z [Centrum pobierania](https://www.microsoft.com/en-us/download/details.aspx?id=44949) i wyodrębnić lokalnie.

Azure programu PowerShell jest wymagane. Jeśli nie skonfigurowano programu PowerShell na komputerze lokalnym, przeczytaj artykuł [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).

Podczas pisania tego dokumentu obrazy Linux z Azure Marketplace (zawierający sterowniki InfiniBand Azure) są SLES 12, CentOS 6.5 i CentOS 7.1. Ten artykuł jest oparty na zastosowania SLES 12. Aby pobrać nazwy wszystkich obrazów Linux, które obsługują HPC na rynku, można uruchomić następującego polecenia programu PowerShell:

```
    get-azurevmimage | ?{$_.ImageName.Contains("hpc") -and $_.OS -eq "Linux" }
```

Dane wyjściowe wyświetla listę lokalizacji, w której są dostępne te obrazy i nazwa obrazu (**Nazwa_obrazu**) może być używany w szablonie wdrożenia później.

Przed wdrożeniem klaster, musisz utworzyć plik szablonu wdrożenia HPC Pack. Ponieważ firma Microsoft docelowych małych klaster, węzła głównego zostaną kontrolera domeny i udostępniać lokalnej bazy danych SQL.

Następujący szablon wdrażanie węzła głównego, Utwórz plik XML o nazwie **MyCluster.xml**i Zamień wartości **SubscriptionId**, **StorageAccount**, **lokalizację**, **VMName**i **NazwaUsługi** z Twoimi zmianami.

    <?xml version="1.0" encoding="utf-8" ?>
    <IaaSClusterConfig>
      <Subscription>
        <SubscriptionId>99999999-9999-9999-9999-999999999999</SubscriptionId>
        <StorageAccount>mystorageaccount</StorageAccount>
      </Subscription>
      <Location>North Europe</Location>
      <VNet>
        <VNetName>hpcvnetne</VNetName>
        <SubnetName>subnet-hpc</SubnetName>
      </VNet>
      <Domain>
        <DCOption>HeadNodeAsDC</DCOption>
        <DomainFQDN>hpc.local</DomainFQDN>
      </Domain>
      <Database>
        <DBOption>LocalDB</DBOption>
      </Database>
      <HeadNode>
        <VMName>myhpchn</VMName>
        <ServiceName>myhpchn</ServiceName>
        <VMSize>Standard_D4</VMSize>
      </HeadNode>
      <LinuxComputeNodes>
        <VMNamePattern>lnxcn-%0001%</VMNamePattern>
        <ServiceNamePattern>mylnxcn%01%</ServiceNamePattern>
        <MaxNodeCountPerService>20</MaxNodeCountPerService>
        <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
        <VMSize>A9</VMSize>
        <NodeCount>0</NodeCount>
        <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
      </LinuxComputeNodes>
    </IaaSClusterConfig>

Rozpocznij tworzenie węzeł głowy przez uruchomienie polecenia programu PowerShell w wiersz polecenia z podwyższonym poziomem uprawnień:

```
    .\New-HPCIaaSCluster.ps1 -ConfigFile MyCluster.xml
```

Po 20 do 30 minut węzła głównego powinny być gotowe. Możesz połączyć się go z Azure portal, klikając ikonę **Połącz** maszyny wirtualnej.

Po pewnym czasie może być konieczne rozwiązywanie przesyłania. W tym celu należy uruchomić Menedżera DNS.

1.  Kliknij prawym przyciskiem myszy nazwę serwera w Menedżerze DNS, wybierz polecenie **Właściwości**, a następnie kliknij kartę **usługi przesyłania dalej** .

2.  Kliknij przycisk **Edytuj** , aby usunąć wszelkie usługi przesyłania dalej, a następnie kliknij **przycisk OK**.

3.  Upewnij się, że jest zaznaczone pole wyboru **Użyj wskazówek, jeśli są dostępne nie usługi przesyłania dalej** , a następnie kliknij **przycisk OK**.

## <a name="set-up-linux-compute-nodes"></a>Konfigurowanie węzły obliczeń Linux
Przy użyciu tego samego szablonu wdrożenia, który został użyty do utworzenia węzła głównego należy wdrożyć węzły obliczeń Linux.

Skopiuj plik **MyCluster.xml** z komputera lokalnego do węzła głównego i zaktualizować tagu **NodeCount** liczby węzłów, które chcesz wdrożyć (< = 20). Uważaj, aby mieć za mało rdzenie dostępne w Azure przydziału, ponieważ każde wystąpienie A9 zajmie 16 rdzenie w ramach subskrypcji. Jeśli chcesz używać więcej maszyny wirtualne w ten sam budżet, zamiast A9 można użyć wystąpienia A8 (rdzenie 8).

Skopiuj skryptów wdrożenia HPC Pack IaaS w węzła głównego.

Uruchom następujące polecenia programu PowerShell Azure w wiersz polecenia z podwyższonym poziomem uprawnień:

1.  Uruchom **AzureAccount Dodaj** nawiązywania połączenia z subskrypcji usługi Azure.

2.  Jeśli masz wiele subskrypcji, uruchom **Get-AzureSubscription** , aby był widoczny.

3.  Ustawianie domyślnego subskrypcji, uruchamiając **xxxx wybierz AzureSubscription - SubscriptionName-domyślne** polecenia.

4.  Uruchamianie **.\New-HPCIaaSCluster.ps1 - ConfigFile MyCluster.xml** zacząć wdrażanie Linux obliczyć węzły.

    ![Wdrożenie węzła głowy w działaniu][hndeploy]

Otwórz narzędzie HPC Pack klaster Manager. Po kilku minutach węzły obliczeń Linux regularnie pojawi się lista obliczeń węzłach. W trybie klasycznym wdrożenia IaaS maszyny wirtualne są tworzone kolejno. Tak Jeśli liczby węzłów jest ważne, wprowadzenie wszystkich wdrożony zajmuje dużo czasu.

![Węzły Linux HPC Pack klaster Manager][clustermanager]

Teraz, gdy wszystkie węzły są i rozpocząć pracę w klastrze, istnieje infrastruktury dodatkowe ustawienia, aby powiększyć.

## <a name="set-up-an-azure-file-share-for-windows-and-linux-nodes"></a>Konfigurowanie udziale plików Azure dla systemu Windows i Linux oraz węzłów
Usługa Azure plików służy do przechowywania skryptów, pakietów aplikacji i plików danych. Plik Azure zapewnia możliwości CIFS u góry magazyn obiektów Blob platformy Azure jako Magazyn trwały. Należy pamiętać, że nie jest to najbardziej skalowalna rozwiązanie, ale jest najprostszym i nie wymaga dedykowanego maszyny wirtualne.

Utwórz udział pliku Azure, postępując zgodnie z instrukcjami w artykule [Wprowadzenie do przechowywania plików Azure w systemie Windows](..\storage\storage-dotnet-how-to-use-files.md).

Zachowaj nazwę konta magazynu jako **saname**, nazwę udostępnianie pliku jako **udziału**i klucz konta magazynowania jako **sakey**.

### <a name="mount-the-azure-file-share-on-the-head-node"></a>Zainstaluj udziale plików Azure na węzła głównego
Otwórz wiersz polecenia z podwyższonym poziomem uprawnień, a następnie uruchom następujące polecenie, aby zapisać poświadczenia na komputerze lokalnym magazynu:

```
    cmdkey /add:<saname>.file.core.windows.net /user:<saname> /pass:<sakey>
```

Następnie Aby zainstalować udziale plików Azure, uruchom:

```
    net use Z: \\<saname>.file.core.windows.net\<sharename> /persistent:yes
```

### <a name="mount-the-azure-file-share-on-linux-compute-nodes"></a>Zainstaluj udziale plików Azure w węzłach obliczeń Linux
Jeden przydatne narzędzie dołączonej HPC Pack to narzędzie clusrun. To narzędzie wiersza polecenia służy do uruchamiania tego samego polecenia jednocześnie zestawu węzłów obliczeń. W naszym przypadku służy do instalowania udziale plików Azure i utrzymują je po ponownego uruchamiania.
W pełnych wiersza polecenia w węźle głowy uruchom następujące polecenia.

Aby utworzyć katalogu instalacji:

```
    clusrun /nodegroup:LinuxNodes mkdir -p /hpcdata
```

Aby zainstalować udziale plików Azure:

```
    clusrun /nodegroup:LinuxNodes mount -t cifs //<saname>.file.core.windows.net/<sharename> /hpcdata -o vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777
```

Aby udostępnianie instalacji:

```
    clusrun /nodegroup:LinuxNodes "echo //<saname>.file.core.windows.net/<sharename> /hpcdata cifs vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777 >> /etc/fstab"
```

## <a name="install-star-ccm"></a>Instalowanie GWIAZDA-CCM +
Azure wystąpienia maszyn wirtualnych A8 i A9 zapewniają InfiniBand pomocy technicznej i możliwości RDMA. Sterowniki jądra, umożliwiające te funkcje są dostępne dla systemu Windows Server 2012 R2, SUSE 12, CentOS 6.5 i obrazy CentOS 7.1 w Azure Marketplace. Microsoft MPI i MPI firmy Intel (wersja 5.x) są dwie bibliotek MPI, które obsługują te sterowniki platformy Azure.

Dysk CD adapco GWIAZDA-CCM + Zwolnij 11.x i później jest dołączany do wersji MPI firmy Intel 5.x, więc InfiniBand Obsługa Azure są uwzględniane.

Uzyskiwanie Linux64 GWIAZDA-pakiet CCM + z [portalu adapco dysk CD](https://steve.cd-adapco.com). W naszym przypadku użyliśmy wersja 11.02.010 w mieszanych dokładności.

Na węzła głównego w udziale plików Azure **/hpcdata** , Utwórz skrypt powłoki o nazwie **setupstarccm.sh** o następującej treści. Skrypt zostanie uruchomiona w każdym węźle obliczeń, aby skonfigurować GWIAZDA-CCM + lokalnie.

#### <a name="sample-setupstarcmsh-script"></a>Przykładowy skrypt setupstarcm.sh
```
    #!/bin/bash
    # setupstarcm.sh to set up STAR-CCM+ locally

    # Create the CD-adapco main directory
    mkdir -p /opt/CD-adapco

    # Copy the STAR-CCM package from the file share to the local directory
    cp /hpcdata/StarCCM/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz /opt/CD-adapco/

    # Extract the package
    tar -xzf /opt/CD-adapco/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz -C /opt/CD-adapco/

    # Start a silent installation of STAR-CCM without the FLEXlm component
    /opt/CD-adapco/starccm+_11.02.010/STAR-CCM+11.02.010_01_linux-x86_64-2.5_gnu4.8.bin -i silent -DCOMPUTE_NODE=true -DNODOC=true -DINSTALLFLEX=false

    # Update memory limits
    echo "*               hard    memlock         unlimited" >> /etc/security/limits.conf
    echo "*               soft    memlock         unlimited" >> /etc/security/limits.conf
```
Teraz, aby skonfigurować GWIAZDA-CCM + na wszystkich Linux oraz obliczenia węzły, otwórz wiersz polecenia z podwyższonym poziomem uprawnień i wpisz następujące polecenie:

```
    clusrun /nodegroup:LinuxNodes bash /hpcdata/setupstarccm.sh
```

Po uruchomieniu polecenia użycie Procesora można monitorować za pomocą mapy ciepła z Menedżera klaster. Po kilku minutach wszystkie węzły powinna być poprawnie ustawiona w górę.

## <a name="run-star-ccm-jobs"></a>Uruchamianie GWIAZDA-CCM + zadania
HPC Pack jest używane na potrzeby możliwości harmonogram zadań w celu uruchomienia GWIAZDA-CCM + zadania. W tym celu jest potrzebna obsługa kilka skryptów, które są używane do zadanie uruchomienia GWIAZDA-CCM +. Dane wejściowe przechowywanej w udziale plików Azure pierwszy dla uproszczenia.

Poniższy skrypt programu PowerShell jest używana w kolejce GWIAZDY-CCM + zadania. Trwa trzy argumenty:

*  Nazwa modelu

*  Liczby węzłów może być używany

*  Liczby rdzeni na każdym węźle ma być używany

Ponieważ GWIAZDA-CCM + można wypełniać przepustowość pamięci lepiej używać mniej rdzeni w jednym węzły obliczeń i dodawać nowe węzły. Dokładna liczba rdzeni w jednym węzeł zależy Rodzina procesorów i szybkości połączenia.

Węzły są przydzielane wyłącznie dla zadania i nie mogą być udostępniane przez inne zadania. Zadanie nie jest uruchomiona jako zadanie MPI bezpośrednio. Skrypt powłoki **runstarccm.sh** rozpocznie uruchamianie MPI.

Model wprowadzania i skrypt **runstarccm.sh** są przechowywane w sekcji udostępnianie **/hpcdata** , który został wcześniej zainstalowany.

Pliki dziennika są nazywane identyfikator zadania i są przechowywane w **/hpcdata udostępnianie**, wraz z GWIAZDY-CCM + pliki wyjściowe.


#### <a name="sample-submitstarccmjobps1-script"></a>Przykładowy skrypt SubmitStarccmJob.ps1
```
    Add-PSSnapin Microsoft.HPC -ErrorAction silentlycontinue
    $scheduler="headnodename"
    $modelName=$args[0]
    $nbCoresPerNode=$args[2]
    $nbNodes=$args[1]

    #---------------------------------------------------------------------------------------------------------
    # Create a new job; this will give us the job ID that's used to identify the name of the uploaded package in Azure
    #
    $job = New-HpcJob -Name "$modelName $nbNodes $nbCoresPerNode" -Scheduler $scheduler -NumNodes $nbNodes -NodeGroups "LinuxNodes" -FailOnTaskFailure $true -Exclusive $true
    $jobId = [String]$job.Id

    #---------------------------------------------------------------------------------------------------------
    # Submit the job    
    $workdir =  "/hpcdata"
    $execName = "$nbCoresPerNode runner.java $modelName.sim"

    $job | Add-HpcTask -Scheduler $scheduler -Name "Compute" -stdout "$jobId.log" -stderr "$jobId.err" -Rerunnable $false -NumNodes $nbNodes -Command "runstarccm.sh $execName" -WorkDir "$workdir"


    Submit-HpcJob -Job $job -Scheduler $scheduler
```
Zamień **runner.java** swojej preferowanej GWIAZDA-CCM + Java modelu uruchamiania i rejestrowanie kodu.

#### <a name="sample-runstarccmsh-script"></a>Przykładowy skrypt runstarccm.sh
```
    #!/bin/bash
    echo "start"
    # The path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    echo ${SCRIPT_PATH}
    # Set the mpirun runtime environment
    export CDLMD_LICENSE_FILE=1999@flex.cd-adapco.com

    # mpirun command
    STARCCM=/opt/CD-adapco/STAR-CCM+11.02.010/star/bin/starccm+

    # Get node information from ENVs
    NODESCORES=(${CCP_NODES_CORES})
    COUNT=${#NODESCORES[@]}
    NBCORESPERNODE=$1

    # Create the hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$
    echo ${NODELIST_PATH}

    # Get every node name and write into the hostfile file
    I=1
    NBNODES=0
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
        let "NBNODES=${NBNODES}+1"
    done
    let "NBCORES=${NBNODES}*${NBCORESPERNODE}"

    # Run STAR-CCM with the hostfile argument
    #  
    ${STARCCM} -np ${NBCORES} -machinefile ${NODELIST_PATH} \
        -power -podkey "<yourkey>" -rsh ssh \
        -mpi intel -fabric UDAPL -cpubind bandwidth,v \
        -mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0" \
        -batch $2 $3
    RTNSTS=$?
    rm -f ${NODELIST_PATH}

    exit ${RTNSTS}
```

W naszym test użyliśmy token licencji Power na żądanie. Token, należy Ustaw zmienną środowiska **$CDLMD_LICENSE_FILE** **1999@flex.cd-adapco.com** i klucza w opcji **- podkey** wiersza polecenia.

Po zainicjowaniu niektórych skrypt wyodrębnia — z **$CCP_NODES_CORES** środowiska zmienne, które HPC Pack — Lista węzłów do utworzenia hostfile, korzystającego z uruchamianie MPI. Ten hostfile będzie zawierać listę nazw węzeł obliczeń, które są używane dla zadania, jedną nazwę w wierszu.

Format **$CCP_NODES_CORES** tego wzorca są następujące:

```
<Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
```

W przypadku gdy:

* `<Number of nodes>`jest to liczba węzły przydzielone do tego zadania.

* `<Name of node_n_...>`to nazwa każdego węzła przydzielone do tego zadania.

* `<Cores of node_n_...>`jest to liczba rdzeni w węźle przydzielone do tego zadania.

Liczba rdzeni (**$NBCORES**) również jest obliczana na podstawie liczby węzłów (**$NBNODES**) i liczby rdzeni na węzeł (podane jako parametru **$NBCORESPERNODE**).

Opcje MPI są te, które są używane z MPI firmy Intel Azure:

*   `-mpi intel`Aby określić MPI firmy Intel.

*   `-fabric UDAPL`Aby używać zleceń Azure InfiniBand.

*   `-cpubind bandwidth,v`Aby zoptymalizować przepustowości dla MPI z GWIAZDA-CCM +.

*   `-mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0"`Aby wyświetlić MPI firmy Intel przetwarzania Azure InfiniBand i Ustaw wymaganą liczbę rdzeni w jednym węzeł.

*   `-batch`Aby rozpocząć GWIAZDA-CCM + w trybie partii z Brak interfejsu użytkownika.


Na koniec do rozpoczęcia zadania, upewnij się, że węzły są i rozpocząć pracę i jest w trybie online w Menedżerze klaster. Następnie w wierszu polecenia programu PowerShell, uruchom następująco:

```
    .\ SubmitStarccmJob.ps1 <model> <nbNodes> <nbCoresPerNode>
```

## <a name="stop-nodes"></a>Zatrzymywanie węzły
Później po zakończeniu testy, można następujące polecenia programu PowerShell z dodatkiem Service Pack HPC Aby zatrzymać i uruchomić węzły:

```
    Stop-HPCIaaSNode.ps1 -Name <prefix>-00*
    Start-HPCIaaSNode.ps1 -Name <prefix>-00*
```

## <a name="next-steps"></a>Następne kroki
Spróbuj uruchomić inne obciążenia Linux. Na przykład zobacz:

* [Uruchamianie NAMD z Microsoft HPC Pack w węzłach obliczeń Linux platformy Azure](virtual-machines-linux-classic-hpcpack-cluster-namd.md)

* [Uruchamianie OpenFOAM z Microsoft HPC Pack w klastrze Linux RDMA platformy Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)


<!--Image references-->
[hndeploy]: ./media/virtual-machines-linux-classic-hpcpack-cluster-starccm/hndeploy.png
[clustermanager]: ./media/virtual-machines-linux-classic-hpcpack-cluster-starccm/ClusterManager.png
