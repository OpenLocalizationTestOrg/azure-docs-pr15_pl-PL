<properties
 pageTitle="NAMD pakietem Microsoft HPC na maszyny wirtualne Linux | Microsoft Azure"
 description="Wdrażanie klastrze Microsoft HPC Pack Azure i uruchamiać symulacji NAMD z charmrun na wiele węzłów obliczeń Linux"
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
 ms.date="10/13/2016"
 ms.author="danlep"/>

# <a name="run-namd-with-microsoft-hpc-pack-on-linux-compute-nodes-in-azure"></a>Uruchamianie NAMD z Microsoft HPC Pack w węzłach obliczeń Linux platformy Azure

Ten artykuł zawiera jeden sposób, aby uruchomić obciążenie pracą wysokiej wydajności (HPC) Linux Azure maszyn wirtualnych. Teraz możesz skonfigurować klaster [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) Azure węzłów obliczeń Linux i uruchamianie symulacji [NAMD](http://www.ks.uiuc.edu/Research/namd/) , aby obliczyć i wizualizowanie struktury systemu dużych biomolekularnych.  

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

* **NAMD** (w przypadku programu Dynamics molekularnej Nanoscale) jest pakietem równoległe dynamics molekularnej przeznaczone do symulacji wysokiej wydajności systemów dużych biomolekularnych zawierających miliony atomów. Przykładami tych systemów wirusów, struktur komórki i dużych białka. NAMD skale setki rdzenie dla symulacji typowe i ponad 500 000 rdzenie dla największych symulacji.

* **Microsoft HPC Pack** zapewnia funkcje do uruchamiania HPC na dużą skalę i równoległych aplikacji w klastrów komputerów lokalnego lub Azure maszyn wirtualnych. Teraz obsługuje aplikacje Linux HPC w systemie Linux pierwotnie opracowany jako rozwiązanie obciążeń pracą HPC systemu Windows, HPC Pack obliczyć maszyny wirtualne węzeł wdrożony w klastrze HPC Pack. Zobacz [Rozpoczynanie pracy z Linux węzły do uruchamiania w klastrze HPC Pack platformy Azure](virtual-machines-linux-classic-hpcpack-cluster.md) wprowadzenie.

Inne opcje uruchomić Linux HPC obciążeń pracą w Azure uzyskać [obliczeniowych techniczną dla partii i wysokiej wydajności](../batch/batch-hpc-solutions.md).




## <a name="prerequisites"></a>Wymagania wstępne

* **Klaster HPC Pack z systemem Linux obliczyć węzły** - wdrażanie klastrze HPC Pack węzłów obliczeń Linux Azure za pomocą [Menedżera zasobów Azure szablonu](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) lub [skrypt programu PowerShell Azure](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md). Zobacz [Rozpoczynanie pracy z Linux węzły do uruchamiania w klastrze HPC Pack platformy Azure](virtual-machines-linux-classic-hpcpack-cluster.md) dla wymagania wstępne i instrukcje dotyczące obu opcji. Jeśli wybierzesz opcję wdrożenia skrypt programu PowerShell, zobacz przykładowy plik konfiguracyjny w przykładowe pliki na końcu tego artykułu. Ten plik konfiguruje klaster oparte na platformie Azure HPC Pack składa się z systemu Windows Server 2012 R2 węzła głównego i cztery węzły obliczeń 6.6 CentOS duży rozmiar. Dostosuj ten plik, w razie potrzeby w środowisku usługi.


* **Pliki oprogramowania i samouczek NAMD** - NAMD pobieranie oprogramowania Linux z witryny [NAMD](http://www.ks.uiuc.edu/Research/namd/) (wymagana rejestracja). Ten artykuł jest oparty na NAMD w wersji 2.10 i korzysta z archiwum [Linux-x86_64 (64-bitowa firmy Intel-AMD z Ethernet)](http://www.ks.uiuc.edu/Development/Download/download.cgi?UserID=&AccessCode=&ArchiveID=1310) . Również pobrać [pliki samouczka NAMD](http://www.ks.uiuc.edu/Training/Tutorials/#namd). Pliki do pobrania pliki .tar i potrzebujesz narzędzie systemu Windows do wyodrębnienia plików na głowy węzła. Aby wyodrębnić pliki, postępuj zgodnie z instrukcjami w dalszej części tego artykułu. 

* **VMD** (opcjonalnie) — aby wyświetlić wyniki pracy NAMD, Pobierz i zainstaluj program molekularnej wizualizacji [VMD](http://www.ks.uiuc.edu/Research/vmd/) na komputerze wybranych przez użytkownika. Bieżąca wersja jest 1.9.2. Zobacz VMD witryny, aby rozpocząć pobieranie.  


## <a name="set-up-mutual-trust-between-compute-nodes"></a>Konfigurowanie wzajemnego zaufania między węzły obliczeń
Uruchamianie zadania węzeł między na wielu węzłach Linux wymaga węzły ufać sobie wzajemnie (przez **rsh** lub **ssh**). Po utworzeniu klaster HPC Pack scenariusz wdrażania programu Microsoft HPC Pack IaaS skrypt automatycznie konfiguruje trwały wzajemnego zaufania dla konta administratora, który można określić. Użytkownicy niebędący administratorami tworzonych w domenie klaster musisz skonfigurować tymczasowe wzajemnego zaufania między węzły, gdy zadanie jest przydzielone do nich. Następnie destroy relacji, po ukończeniu zadania. Aby to zrobić dla każdego użytkownika, podaj pary kluczy RSA z klastrem używający HPC Pack w celu ustanowienia relacji zaufania. Postępuj zgodnie z instrukcjami.

### <a name="generate-an-rsa-key-pair"></a>Generowanie pary kluczy RSA
Jest proste wygenerować para kluczy RSA, która zawiera klucz publiczny i klucz prywatny, uruchamiając polecenie **ssh-keygen** Linux.

1.  Zaloguj się do komputera Linux.

2.  Uruchom następujące polecenie:

    ```bash
    ssh-keygen -t rsa
    ```

    >[AZURE.NOTE] Naciśnij klawisz **Enter** , aby korzystać z domyślnych ustawień, dopóki nie zakończy się polecenie. Nie wprowadzaj hasło miejscu; Po wyświetleniu monitu o podanie hasła, po prostu naciśnij klawisz **Enter**.

    ![Generowanie pary kluczy RSA][keygen]

3.  Zmień katalog do katalogu ~/.ssh. Klucz prywatny jest przechowywany w id_rsa i kluczem publicznym w id_rsa.pub.

    ![Klucze prywatne i publiczne][keys]

### <a name="add-the-key-pair-to-the-hpc-pack-cluster"></a>Dodawanie do klastrów HPC Pack pary kluczy
1.  [Nawiązywanie połączenia przez pulpitu zdalnego](virtual-machines-windows-connect-logon.md) węzła głównego maszyn wirtualnych przy użyciu domeny poświadczeń, pod warunkiem po wdrożeniu klaster (na przykład hpc\clusteradmin). Możesz zarządzać klaster z węzła głównego.

2. Standardowe procedury Windows Server umożliwia tworzenie konta domeny w domenie usługi Active Directory klaster. Na przykład narzędzie użytkowników usługi Active Directory i komputery na węzła głównego. W przykładach w tym artykule założono, że tworzenie domeny użytkownika o nazwie hpcuser w domenie hpclab (hpclab\hpcuser).

3. Dodaj użytkownika domeny do klastrów HPC Pack jako użytkownik klaster. Aby uzyskać instrukcje zobacz [Dodawanie lub usuwanie klaster użytkowników](https://technet.microsoft.com/library/ff919330.aspx).

2.  Tworzenie pliku o nazwie C:\cred.xml i skopiuj danych klucza RSA do niego. Przykład można znaleźć w przykładowe pliki na końcu tego artykułu.

    ```
    <ExtendedData>
        <PrivateKey>Copy the contents of private key here</PrivateKey>
        <PublicKey>Copy the contents of public key here</PublicKey>
    </ExtendedData>
    ```

3.  Otwórz wiersz polecenia i wpisz następujące polecenie, aby ustawić danych poświadczeń konta hpclab\hpcuser. Możesz użyć parametru **extendeddata** w celu przekazania nazwę pliku C:\cred.xml utworzonego dla najważniejszych danych.

    ```command
    hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
    ```

    To polecenie zakończy się pomyślnie bez danych wyjściowych. Po ustawieniu poświadczeń dla kont użytkowników, które są potrzebne do uruchamiania zadań, przechowywać plik cred.xml w bezpiecznym miejscu lub go usunąć.

5.  Jeśli pary kluczy RSA został wygenerowany na jednym z węzły Linux, pamiętaj, aby usunąć klucze po zakończeniu korzystania z nich. HPC Pack nie skonfigurowany wzajemnego zaufania, jeśli znajdzie istniejącego pliku id_rsa lub id_rsa.pub pliku.

>[AZURE.IMPORTANT] Nie zalecamy uruchamiania zadanie Linux jako administrator klastrów w klastrze udostępnionego, ponieważ zadanie przesłane przez administratora uruchamiana konta głównego w węzłach Linux. Zadanie przesłany przez użytkownika niebędący administratorami uruchamiana lokalnego konta użytkownika Linux o takiej samej nazwie jak użytkownik zadania. W tym przypadku HPC Pack automatycznie konfiguruje wzajemnego zaufania dla tego użytkownika Linux we wszystkich węzłach przydzielonych do zadania. Możesz skonfigurować użytkownika Linux ręcznie w węzłach Linux przed uruchomieniem zadania lub HPC Pack użytkownik automatycznie tworzy podczas przesyłania zadania. Jeśli HPC Pack tworzy użytkownika, HPC Pack usunie ją, po zakończeniu zadania. Aby zmniejszyć zagrożenie, nazwy klawiszy są usuwane po zakończeniu zadania w węzłach.

## <a name="set-up-a-file-share-for-linux-nodes"></a>Konfigurowanie udziału plików dla węzłów Linux

Teraz skonfigurować udziale plików SMB i zainstalować folder udostępniony na wszystkich węzłach Linux, aby umożliwić węzły Linux oraz dostęp do plików NAMD ze ścieżką typowych. Poniżej przedstawiono kroki, aby zainstalować folderu udostępnionego na węzła głównego. Udostępnianie jest zalecane dla dystrybucji takich jak CentOS 6.6, które obecnie nie obsługują usługę Azure plików. Jeśli węzły Linux obsługuje udziale plików Azure, zobacz, [jak używać Azure magazyn plików z systemem Linux](../storage/storage-how-to-use-files-linux.md). Pliku dodatkowe opcje udostępniania z HPC Pack zobacz [Rozpoczynanie pracy z Linux węzły do uruchamiania w klastrze HPC Pack platformy Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

1.  Utwórz folder na węzła głównego i udostępnij go dla wszystkich osób, ustawiając uprawnienia do odczytu/zapisu. W tym przykładzie \\ \\CentOS66HN\Namd to nazwa folderu, w którym CentOS66HN to nazwa hosta węzła głównego.

2. Tworzenie podfolderu o nazwie namd2 w folderze udostępnionym. W namd2 Utwórz inny podfolder namdsample.

3. Wyodrębnianie plików NAMD w folderze przy użyciu wersji systemu Windows **tar** lub innego narzędzia Windows działająca na archiwami .tar. 
    * Wyodrębnianie archiwum tar NAMD do \\ \\CentOS66HN\Namd\namd2.
    
    * Wyodrębnianie plików samouczka w obszarze \\ \\CentOS66HN\Namd\namd2\namdsample.

4. Otwórz okno programu Windows PowerShell i uruchom następujące polecenia, aby zainstalować folder udostępniony w węzłach Linux.

    ```command
    clusrun /nodegroup:LinuxNodes mkdir -p /namd2

    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS66HN/Namd/namd2 /namd2 -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

Pierwsze polecenie tworzy folder o nazwie /namd2 na wszystkich węzłach w grupie LinuxNodes. Drugie polecenie instaluje //CentOS66HN/Namd/namd2 folderu udostępnionego do folderu z dir_mode i file_mode bitów Ustaw 777. *Nazwy użytkownika* i *hasła* , w tym poleceniu należy poświadczenia użytkownika na węzła głównego.

>[AZURE.NOTE]"\`" Symbol w drugim poleceniu jest symbolem anulowania dla programu PowerShell. "\`," oznacza "," (znak przecinka) jest częścią polecenia.


## <a name="create-a-bash-script-to-run-a-namd-job"></a>Utworzyć skrypt imprezie, aby uruchomić zadanie NAMD

Zadanie NAMD musi plik *wstawienia* **charmrun** w celu określenia liczby węzłów korzystać podczas uruchamiania procesów NAMD. Możesz użyć skrypt imprezie, który umożliwia generowanie pliku wstawienia i uruchamia **charmrun** z tym plikiem wstawienia. Następnie można przesłać zadanie NAMD w HPC klaster Menedżer połączeń tego skryptu.

Przy użyciu dowolnego edytora tekstu, utworzyć skrypt imprezie w folderze /namd2 zawierającym pliki programów NAMD i nadaj mu nazwę hpccharmrun.sh. Szybkie dowodu koncepcji Skopiuj przykładowy skrypt hpccharmrun.sh zamieszczonej na końcu tego artykułu i przejdź do [przesyłania zadania NAMD](#submit-a-namd-job).

>[AZURE.TIP] Zapisz skrypt jako plik tekstowy z systemem Linux linii zakończenia (tylko wysuwu wiersza, nie CR wysuwu wiersza). Dzięki temu został prawidłowo uruchomiony w węzłach Linux.

Poniżej przedstawiono szczegółowe informacje o działanie tego skryptu imprezie. 

1.  Definiowanie niektóre zmienne.

    ```bash
    #!/bin/bash

    # The path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    # Charmrun command
    CHARMRUN=${SCRIPT_PATH}/charmrun
    # Argument of ++nodelist
    NODELIST_OPT="++nodelist"
    # Argument of ++p
    NUMPROCESS="+p"
    ```

2.  Uzyskiwanie informacji węzeł z zmiennych środowiska. $NODESCORES przechowuje listę dzielenie wyrazów z $CCP_NODES_CORES. $COUNT jest rozmiar $NODESCORES.
    ```
    # Get node information from the environment variables
    NODESCORES=(${CCP_NODES_CORES})
    COUNT=${#NODESCORES[@]}
    ```    
    
    Format dla zmiennej CCP_NODES_CORES $ jest w następujący sposób:

    ```
    <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>…
    ```

    Tej zmiennej lista całkowitą liczbę węzłów, nazwy węzłów i liczby rdzeni na każdym węźle przydzielonych do zadania. Na przykład jeśli zadanie wymaga 10 rdzenie do uruchomienia, wartość $CCP_NODES_CORES jest podobne do:

    ```
    3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 2
    ```
        
3.  Jeśli nie ustawiono zmiennej CCP_NODES_CORES $, Rozpocznij **charmrun** bezpośrednio. (To należy tylko wówczas, gdy uruchomieniu tego skryptu bezpośrednio na węzły Linux.)

    ```
    if [ ${COUNT} -eq 0 ]
    then
        # CCP_NODES is_CORES is not found or is empty, so just run charmrun without nodelist arg.
        #echo ${CHARMRUN} $*
        ${CHARMRUN} $*
    ```

4.  Lub utworzyć plik wstawienia **charmrun**.

    ```
    else
        # Create the nodelist file
        NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

        # Write the head line
        echo "group main" > ${NODELIST_PATH}

        # Get every node name and number of cores and write into the nodelist file
        I=1
        while [ ${I} -lt ${COUNT} ]
        do
            echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
            let "I=${I}+2"
        done
```
5.  Uruchom **charmrun** z plikiem wstawienia, sprawdzić jego stan zwrotu i usuń plik wstawienia na końcu.

    ${CCP_NUMCPUS} jest innej zmiennej środowiska określonych przez węzła głównego HPC Pack. Liczba całkowita rdzenie przydzielone do tego zadania jest przechowywana. Firma Microsoft korzysta Określ liczbę procesów dla charmrun.

    ```
    # Run charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
    fi

    ```
6.  Zakończ działanie ze stanem zwrotnym **charmrun** .

    ```
    exit ${RTNSTS}
    ```



Oto informacje znajdujące się w pliku wstawienia, który generuje skrypt:

```
group main
host <Name of node1> ++cpus <Cores of node1>
host <Name of node2> ++cpus <Cores of node2>
…
```

Na przykład:

```
group main
host CENTOS66LN-00 ++cpus 4
host CENTOS66LN-01 ++cpus 4
host CENTOS66LN-03 ++cpus 2
```

## <a name="submit-a-namd-job"></a>Prześlij zadanie NAMD

Teraz możesz przystąpić do Prześlij zadanie NAMD w Menedżerze klaster HPC.

1.  Nawiązywanie połączenia z węzła głowy i uruchomić Menedżera klaster HPC.

2.  **Zarządzanie zasobami**w upewnij się, że węzły obliczeń Linux są w stanie **Online** . Jeśli nie są one, zaznacz je i kliknij przycisk **Przejdź do trybu Online**.

2.  W obszarze **Zarządzanie zadaniami**kliknij przycisk **Nowe zadanie**.

3.  Wprowadź nazwę zadania, takie jak *hpccharmrun*.

    ![Nowe zadanie HPC][namd_job]

4.  Na stronie **Szczegóły zadania** w obszarze **Zasobów zadania**jako **węzeł** wybierz odpowiedni typ zasobu i ustawić **Minimalny** 3. , możemy uruchomić zadania na trzy węzły Linux i każdy węzeł ma cztery rdzenie.

    ![Zasoby zadania][job_resources]

5. Kliknij pozycję **Edytuj zadania** w lewym okienku nawigacji, a następnie kliknij przycisk **Dodaj** , aby dodać zadanie do zadania.    


6. Na stronie **Szczegóły zadania i przekierowywania we/wy** ustaw następujące wartości:

    * **Wiersz polecenia** -
`/namd2/hpccharmrun.sh ++remote-shell ssh /namd2/namd2 /namd2/namdsample/1-2-sphere/ubq_ws_eq.conf > /namd2/namd2_hpccharmrun.log`

        >[AZURE.TIP] Poprzedniego wiersza polecenia jest jedno polecenie bez podziałów wierszy. Jest zawijany wyświetlany w wielu wierszach w obszarze **wiersza polecenia**.

    * **Katalog roboczy** - /namd2

    * **Minimalna** — 3

    ![Szczegóły zadania][task_details]

    >[AZURE.NOTE] Możesz ustawić katalogu roboczego tutaj ponieważ **charmrun** próbuje przejść do tego samego katalogu pracy w każdym węźle. Jeśli nie skonfigurowano katalogu roboczego, HPC Pack uruchamia polecenie w folderze losowo nazwanych utworzonej na jednym z węzłów Linux. Powoduje to, że następujący komunikat o błędzie na innych węzłach: `/bin/bash: line 37: cd: /tmp/nodemanager_task_94_0.mFlQSN: No such file or directory.` Aby uniknąć tego problemu, określ ścieżkę folderu, które są dostępne dla wszystkich węzłów jako katalogu roboczego.

5.  Kliknij **przycisk OK** , a następnie kliknij przycisk **Prześlij** , aby uruchomić to zadanie.

    Domyślnie HPC Pack przesyła zadanie jako bieżące konto logowania użytkownika. Okno dialogowe może zostać wyświetlony monit o wprowadzenie nazwy użytkownika i hasła, kliknij przycisk **Prześlij**.

    ![Poświadczenia zadania][creds]

    W niektórych sytuacjach HPC Pack zapamiętuje informacje o użytkowniku wprowadzania przed i nie wyświetla to okno dialogowe. Aby ponownie wyświetlić HPC Pack, wprowadź poniższe polecenie w wierszu polecenia, a następnie przesłać zadanie.

    ```command
    hpccred delcreds
    ```

6.  Zadanie trwa kilka minut, aby zakończyć.

7.  Znajdowanie dziennik zadań w \\ <headnodeName>\Namd\namd2\namd2_hpccharmrun.log i pliki wyjściowe w \\ <headnodeName>\Namd\namd2\namdsample\1-2-sphere\.

8.  Opcjonalnie można uruchomić VMD, aby wyświetlić wyniki zadania. Instrukcje dotyczące wizualizacji NAMD wyjściowe pliki (w tym przypadku ubiquitin białka związku w dziedzinie wody) wykraczają poza zakres tego artykułu. Aby uzyskać szczegółowe informacje, zobacz [Samouczek NAMD](http://www.life.illinois.edu/emad/biop590c/namd-tutorial-unix-590C.pdf) .

    ![Wyniki zadania][vmd_view]

## <a name="sample-files"></a>Przykładowe pliki

### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>Przykładowy plik konfiguracyjny XML do wdrożenia klaster przez skrypt programu PowerShell

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
      <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>CentOS66HN</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>CentOS66LN-%00%</VMNamePattern>
    <ServiceName>MyLnxCNService</ServiceName>
     <VMSize>Large</VMSize>
     <NodeCount>4</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-66-20150325</ImageName>
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

### <a name="sample-hpccharmrunsh-script"></a>Przykładowy skrypt hpccharmrun.sh

```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
# Charmrun command
CHARMRUN=${SCRIPT_PATH}/charmrun
# Argument of ++nodelist
NODELIST_OPT="++nodelist"
# Argument of ++p
NUMPROCESS="+p"

# Get node information from ENVs
# CCP_NODES_CORES=3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 4
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # If CCP_NODES_CORES is not found or is empty, just run the charmrun without nodelist arg.
    #echo ${CHARMRUN} $*
    ${CHARMRUN} $*
else
    # Create the nodelist file
    NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

    # Write the head line
    echo "group main" > ${NODELIST_PATH}

    # Get every node name & cores and write into the nodelist file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}
```

 





<!--Image references-->
[keygen]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/keygen.png
[keys]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/keys.png
[namd_job]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/namd_job.png
[job_resources]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/job_resources.png
[creds]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/creds.png
[task_details]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/task_details.png
[vmd_view]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/vmd_view.png
