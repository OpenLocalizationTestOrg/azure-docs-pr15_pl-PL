<properties
 pageTitle="Klaster Linux RDMA do uruchamiania aplikacji MPI | Microsoft Azure"
 description="Utworzyć klaster Linux rozmiar H16r, H16mr, A8 lub maszyny wirtualne A9, za pomocą sieci Azure RDMA uruchamianie aplikacji MPI"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="set-up-a-linux-rdma-cluster-to-run-mpi-applications"></a>Konfigurowanie klastrze Linux RDMA do uruchamiania aplikacji MPI


Dowiedz się, jak skonfigurować klaster Linux RDMA platformy Azure z [serii H lub obliczeniowych serii A maszyny wirtualne](virtual-machines-linux-a8-a9-a10-a11-specs.md) do uruchamiania aplikacji równoległych wiadomości przechodzące Interface (MPI). W tym artykule opisano procedury przygotowania obrazu Linux HPC do uruchamiania w klastrze MPI firmy Intel. Następnie należy wdrożyć klastrze maszyny wirtualne za pomocą ten obraz i jedną rozmiarów RDMA dostępem do Azure maszyn wirtualnych (obecnie H16r, H16mr, A8 lub A9). Uruchamianie aplikacji MPI, które wydajne komunikowanie się przez krótki czas oczekiwania za pomocą klaster, wysokiej wydajności sieci opartych na zdalnym bezpośredni dostęp do pamięci technologii (RDMA).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="cluster-deployment-options"></a>Opcje wdrażania klaster

Poniżej przedstawiono metody można utworzyć klaster Linux RDMA lub bez harmonogram zadań.


* **Polecenie azure skryptów** — jak to przedstawiono w dalszej części w tym artykule za pomocą [interfejsu wiersza polecenia Azure](../xplat-cli-install.md) (polecenie) skrypt wdrożenia klastrze RDMA dostępem do maszyny wirtualne. Polecenie w trybie Zarządzanie usługą pojedynczo tworzy w węzłach w modelu Klasyczny wdrożenia dzięki wdrażania wielu węzłów obliczeń może potrwać kilka minut. Aby włączyć połączenie sieciowe RDMA, korzystając z modelu Klasyczny wdrożenia, wdrożyć maszyny wirtualne w tym samym usługi w chmurze.

* **Menedżer zasobów azure szablony** - model wdrożenia Menedżera zasobów umożliwia wdrażanie klastrze maszyny wirtualne RDMA dostępem do nawiązania połączenia z siecią RDMA. Możesz [utworzyć własny szablon](../resource-group-authoring-templates.md), lub zaznacz [Azure Szybki Start szablonów](https://azure.microsoft.com/documentation/templates/) dla szablonów przyczynić się przez firmę Microsoft lub społeczności, aby rozmieścić rozwiązanie ma. Szablony Menedżera zasobów umożliwiają szybkie i niezawodne wdrażanie klastrze Linux. Aby włączyć połączenie sieciowe RDMA, korzystając z modelu wdrożenia Menedżera zasobów, wdrożyć maszyny wirtualne w ten sam zestaw dostępności.

* **HPC Pack** — utworzyć klaster Microsoft HPC Pack platformy Azure i Dodaj węzły obsługujące RDMA obliczeń, uruchamianych obsługiwane rozkładu Linux oraz uzyskiwać dostęp do sieci RDMA. Zobacz [Rozpoczynanie pracy z Linux węzły do uruchamiania w klastrze HPC Pack platformy Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

## <a name="sample-deployment-steps-in-classic-model"></a>Przykładowe kroków wdrażania w modelu Klasyczny

Poniższe kroki pokazują jak za pomocą interfejsu wiersza polecenia Azure wdrażać maszyny HPC SUSE Linux Enterprise Server (SLES) 12 z dodatkiem SP1 z Azure Marketplace, dostosuj go i utworzyć obraz niestandardowy maszyn wirtualnych. Następnie należy użyć obrazu skryptu wdrażania klastrze RDMA dostępem do maszyny wirtualne. 

>[AZURE.TIP]Wykonaj czynności podobne do wdrożenia klastrze RDMA dostępem do maszyny wirtualne oparte na innych obsługiwanych obrazów HPC w Azure Marketplace. Kilka czynności mogą się nieco różnić w, jak wspomniano. Na przykład MPI firmy Intel jest włączone i skonfigurowane w tylko niektóre z tych obrazów. A Jeśli wdrożono SLES 12 HPC maszyn wirtualnych zamiast maszyn wirtualnych HPC SLES 12 z dodatkiem SP1, musisz zaktualizować sterowniki RDMA. Aby uzyskać szczegółowe informacje zobacz [temat A8, A9, A10 i A11 wystąpienia obliczeniowych](virtual-machines-linux-a8-a9-a10-a11-specs.md#rdma-driver-updates-for-sles-12).


### <a name="prerequisites"></a>Wymagania wstępne

*   **Komputer kliencki** - potrzebny Mac, Linux lub klienckich z systemem Windows komputer, aby komunikować się z Azure. W tej procedurze założono, że używasz klienta Linux.

*   **Azure subskrypcji** — Jeśli nie masz subskrypcji, możesz utworzyć [bezpłatne konto](https://azure.microsoft.com/free/) na kilka minut. W przypadku większych klastrów należy rozważyć, czy płatne subskrypcji lub innych opcji zakupu. 

*   **Dostępność rozmiar maszyn wirtualnych** - następujące wymiary wystąpienia są obecnie RDMA stanie: H16r, H16mr A8 i A9. Sprawdzanie [dostępności według regionów produktów](https://azure.microsoft.com/regions/services/) dostępności w regionach Azure. 

*   **Przydział rdzenie** - może być konieczne zwiększyć przydział rdzenie wdrożenia klastrze maszyny wirtualne obliczeniowych. Na przykład konieczne co najmniej 128 rdzenie Jeśli chcesz wdrożyć 8 A9 maszyny wirtualne, jak pokazano w tym artykule. Twoja subskrypcja również może ograniczyć liczby rdzeni, który można wdrożyć w niektórych rodzin rozmiar maszyn wirtualnych, łącznie z serii H. Aby zażądać normę zwiększyć, [Otwórz żądanie pomocy technicznej online klienta](../azure-supportability/how-to-create-azure-support-request.md) bezpłatnie. 

*   **Polecenie Azure** - [zainstalować](../xplat-cli-install.md) polecenie Azure i [Nawiązywanie połączenia z subskrypcji usługi Azure](../xplat-cli-connect.md) na komputerze klienckim.


### <a name="step-1-provision-a-sles-12-sp1-hpc-vm"></a>Krok 1. Inicjowanie obsługi maszyny HPC SLES 12 z dodatkiem SP1

Po zalogowaniu się do Azure z polecenie Azure, uruchom `azure config list` o potwierdzenie, że wyświetlany jest tryb Zarządzanie usługą. Jeśli nie jest dostępne, należy ustawić tryb wykonanie tego polecenia:


    azure config mode asm


Wpisz poniższe czynności, aby wyświetlić wszystkie subskrypcje, które mogą być:


    azure account list

Identyfikuje bieżący aktywną subskrypcję z `Current` ustaw `true`. W przypadku tej subskrypcji nie, którego chcesz użyć do utworzenia klaster, należy ustawić odpowiedni identyfikator subskrypcji jako aktywną subskrypcję:

    azure account set <subscription-Id>

Aby wyświetlić publicznie obrazy SLES 12 z dodatkiem SP1 HPC Azure, uruchom polecenie podobny do następującego, przy założeniu usługi obsługuje środowisko powłoki **grep**:


    azure vm image list | grep "suse.*hpc"

Teraz Zapewnij RDMA dostępem do maszyn wirtualnych z obrazem SLES 12 z dodatkiem SP1 HPC za pomocą polecenia podobny do następującego:

    azure vm create -g <username> -p <password> -c <cloud-service-name> -l <location> -z A9 -n <vm-name> -e 22 b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824

gdzie

* Rozmiar (A9 w tym przykładzie) jest jednym z rozmiarów RDMA dostępem do maszyn wirtualnych.

* Numer portu SSH zewnętrznych (22 w tym przykładzie jest to ustawienie domyślne SSH) jest dowolny prawidłowy numer portu. Wewnętrzny numer portu SSH ustawiono 22.

* Nowe usługi w chmurze jest tworzona w danym regionie Azure określona przez lokalizację. Określ lokalizację, w której jest dostępny rozmiar pamięci Wirtualnej, które możesz wybrać.

* Nazwa obrazu z dodatkiem SP1 SLES 12 obecnie może być `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824` lub `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-priority-v20160824` obsługi priorytet SUSE (opłata jak za dodatkowe).

### <a name="step-2-customize-the-vm"></a>Krok 2. Dostosowywanie maszyn wirtualnych

Po zakończeniu maszyn wirtualnych inicjowania obsługi administracyjnej, SSH do maszyn wirtualnych, za pomocą maszyn zewnętrzny adres IP (lub nazwa DNS) i portu zewnętrznego można skonfigurować numer i dostosuj go. Aby uzyskać szczegóły połączenia zobacz [jak zalogować się do maszyn wirtualnych systemem Linux](virtual-machines-linux-mac-create-ssh-keys.md). Korzystanie z poleceń jako użytkownik, który skonfigurowano w Głosowa, chyba że dostępu na poziomie administratora jest wymagany do wykonania kroku.

>[AZURE.IMPORTANT]Microsoft Azure nie umożliwia dostępu na poziomie administratora do maszyny wirtualne Linux. Aby uzyskać dostępu administracyjnego po połączeniu użytkownik, który maszyn wirtualnych, należy uruchomić za pomocą polecenia `sudo`.

* **Aktualizacje** — Zainstaluj aktualizacje przy użyciu **zypper**. Można również zainstalować narzędzia NFS. 

    >[AZURE.IMPORTANT]W SLES 12 z dodatkiem SP1 HPC Głosowa zaleca się, że nie możesz zastosować aktualizacje jądra, które mogą powodować problemy z Linux RDMA sterowniki.

* **Intel MPI** - instalacji MPI firmy Intel na maszyn wirtualnych HPC programu SLES 12 z dodatkiem SP1, uruchamiając następujące polecenie:

        sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm

* **Blokady pamięci** - kody dla MPI Aby zablokować pamięci dostępnej dla RDMA, dodać lub zmienić następujące ustawienia w pliku /etc/security/limits.conf. (Potrzebne głównego dostępu do edytowania tego pliku). 

    ```
    <User or group name> hard    memlock <memory required for your application in KB>

    <User or group name> soft    memlock <memory required for your application in KB>
    ```

    >[AZURE.NOTE]Podczas testowania, można także ustawić memlock nieograniczone. Na przykład: `<User or group name>    hard    memlock unlimited`. Aby uzyskać więcej informacji zobacz [Najważniejsze znane metody ustawienie zablokowane rozmiar pamięci](https://software.intel.com/en-us/blogs/2014/12/16/best-known-methods-for-setting-locked-memory-size).

* **Klawisze SSH maszyny wirtualne SLES** - wygenerować SSH klawiszy ustanowić zaufanie dla swojego konta użytkownika między węzły do uruchamiania w klastrze SLES podczas wykonywania zadań MPI. (Jeśli wdrożono maszyn wirtualnych HPC CentOS, nie wykonuj tego kroku. Zobacz instrukcje w dalszej części tego artykułu, aby skonfigurować passwordless zaufania SSH między przez klaster po przechwycić obraz i wdrażanie klaster). 

    Uruchom następujące polecenie, aby utworzyć klucze SSH. Gdy zostanie wyświetlony monit o wprowadzenie informacji, naciśnij klawisz Enter do wygenerowania kluczy w lokalizacji domyślnej, bez ustawiania hasło.

        ssh-keygen

    Klucz publiczny dołączyć do pliku authorized_keys znane kluczy publicznych.

        cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

    W katalogu ~/.ssh Edytuj lub Utwórz plik "konfiguracji". Wprowadź zakres adresów IP sieci prywatnej, które ma zostać użyta w Azure (10.32.0.0/16 w tym przykładzie):

        host 10.32.0.*
        StrictHostKeyChecking no

    Możesz też listy adres IP sieci prywatnej poszczególnych maszyn wirtualnych w klastrze w następujący sposób:

    ```
    host 10.32.0.1
     StrictHostKeyChecking no
    host 10.32.0.2
     StrictHostKeyChecking no
    host 10.32.0.3
     StrictHostKeyChecking no
    ```

    >[AZURE.NOTE]Konfigurowanie `StrictHostKeyChecking no` można tworzyć zagrożenie bezpieczeństwa, jeśli nie określono określony adres IP lub zakresu.

* **Aplikacje** - Zainstaluj wszystkie aplikacje, musi być zainstalowany na tym maszyn wirtualnych lub wykonać inne dostosowania, aby przechwycić obraz.

### <a name="step-3-capture-the-image"></a>Krok 3. Przechwytywanie obrazu

Aby przechwycić obraz, należy najpierw uruchom następujące polecenie w maszyn wirtualnych Linux. To polecenie deprovisions maszyn wirtualnych, ale zachowuje konta użytkowników i klawiszy SSH, które zostały zdefiniowane.

```
sudo waagent -deprovision
```

Następnie uruchom następujące polecenia polecenie Azure, aby przechwycić obraz z komputera klienta. Aby uzyskać szczegółowe informacje, zobacz [jak przechwycić klasyczny maszyny wirtualnej Linux jako obraz](virtual-machines-linux-classic-capture-image.md) .  

```
azure vm shutdown <vm-name>

azure vm capture -t <vm-name> <image-name>

```

Po uruchomieniu polecenia obraz maszyn wirtualnych jest rejestrowany przeznaczony do użytku i maszyn wirtualnych zostanie usunięty. Teraz masz gotowe do wdrożenia klastrze obraz niestandardowy.

### <a name="step-4-deploy-a-cluster-with-the-image"></a>Krok 4. Wdrażanie klaster z obrazem

Zmodyfikuj następujące skryptu imprezie odpowiednie wartości w środowisku usługi, a następnie uruchom go na komputerze klienckim. Ponieważ Azure wdrożono maszyny wirtualne pojedynczo w modelu Klasyczny wdrożenia, wystarczy kilka minut 8 maszyny wirtualne A9 sugerowane w tym skrypt wdrożenia.

```
#!/bin/bash -x
# Script to create a compute cluster without a scheduler in a VNet in Azure
# Create a custom private network in Azure
# Replace 10.32.0.0 with your virtual network address space
# Replace <network-name> with your network identifier
# Replace "West US" with an Azure region where the VM size is available
# See Azure Pricing pages for prices and availability of compute-intensive VMs

azure network vnet create -l "West US" -e 10.32.0.0 -i 16 <network-name>

# Create a cloud service. All the compute-intensive instances need to be in the same cloud service for Linux RDMA to work across InfiniBand.
# Note: The current maximum number of VMs in a cloud service is 50. If you need to provision more than 50 VMs in the same cloud service in your cluster, contact Azure Support.

azure service create <cloud-service-name> --location "West US" –s <subscription-ID>

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Define a prefix for external port numbers. If you want to turn off external ports and use only internal ports to communicate between compute nodes via port 22, don’t use this option. Since port numbers up to 10000 are reserved, use numbers after 10000. Leave external port on for rank 0 and head node.

portnumber=101

# In this cluster there will be 8 size A9 nodes, named cluster11 to cluster18. Specify your captured image in <image-name>. Specify the username and password you used when creating the SSH keys.

for (( i=11; i<19; i++ )); do
        azure vm create -g <username> -p <password> -c <cloud-service-name> -z A9 -n $vmname$i -e $portnumber$i -w <network-name> -b Subnet-1 <image-name>
done

# Save this script with a name like makecluster.sh and run it in your shell environment to provision your cluster
```

## <a name="considerations-for-a-centos-hpc-cluster"></a>Zagadnienia dotyczące klastrze CentOS HPC

Jeśli chcesz skonfigurować klaster oparty na jednym z obrazy oparte na CentOS HPC Azure Marketplace zamiast SLES 12 dla HPC, wykonaj ogólne kroki opisane w poprzedniej sekcji. Podczas obsługi administracyjnej i konfigurowania maszyn wirtualnych, należy zwrócić uwagę na następujące różnice:

1. Intel MPI jest już zainstalowany na maszyny z obrazem oparte na CentOS HPC. 

2. Ustawienia pamięci blokowania zostaną dodane już w pliku /etc/security/limits.conf maszyn wirtualnych.

2. Nie wygenerowania kluczy SSH na maszyn wirtualnych, możesz dodawać do przechwytywania. Zalecany Konfigurowanie uwierzytelniania opartego na użytkownika po wdrożeniu cluser. Zobacz następną sekcję.  

### <a name="set-up-passwordless-ssh-trust-on-the-cluster"></a>Konfigurowanie passwordless zaufania SSH w klastrze

W klastrze HPC oparte na CentOS istnieją dwie metody ustalania zaufania między węzły obliczeń: uwierzytelnianie oparte na hoście i uwierzytelniania opartego na użytkownika. Uwierzytelnianie oparte na hoście wykracza poza zakres tego artykułu i ogólnie muszą być wykonywane przez skrypt rozszerzenia podczas wdrażania. Uwierzytelnianie oparte na użytkownika jest odpowiednim ustanawianie zaufania po wdrożeniu i wymaga tworzenia i udostępniania kluczy SSH między węzły do uruchamiania w klastrze. Ta metoda jest znany jako passwordless logowania SSH i jest wymagane podczas wykonywania zadań MPI. 

Przykładowy skrypt przyczynić się ze społeczności jest dostępna na [GitHub](https://github.com/tanewill/utils/blob/master/user_authentication.sh) , aby włączyć uwierzytelnianie użytkownika łatwe w klastrze HPC oparte na CentOS. Pobieranie i używanie tego skryptu, wykonując poniższe czynności. Możesz również zmodyfikować tego skryptu lub użyć innej metody ustanawiania passwordless uwierzytelniania SSH między przez klaster komputerowe.

    wget https://raw.githubusercontent.com/tanewill/utils/master/ user_authentication.sh
    
Aby uruchomić skrypt, należy wiedzieć prefiks podsieć adresów IP. Uzyskaj prefiks, uruchamiając następujące polecenie w jednym z węzłów klaster. Danych wyjściowych powinien wyglądać 10.1.3.5 i prefiks jest 10.1.3 części.

    ifconfig eth0 | grep -w inet | awk '{print $2}'

Teraz uruchamianie skryptu za pomocą trzy parametry: typowe nazwę użytkownika w węzłach obliczeń, wspólne hasło dla tego użytkownika w węzłach obliczeń i prefiks podsieci, który został zwrócony z poprzedniego polecenia.


    ./user_authentication.sh <myusername> <mypassword> 10.1.3

Ten skrypt wykonuje następujące czynności:

* Tworzy katalog w węźle hosta o nazwie .ssh, która jest wymagane na potrzeby logowania passwordless. 
* Tworzy plik konfiguracji w katalogu .ssh, który powoduje, że passwordless logowania, aby umożliwić logowanie z dowolnego węzła w klastrze. 
* Tworzy pliki zawierające węzeł nazwy i adresy IP węzła dla wszystkich węzłów w klastrze. Te pliki pozostają po skrypt jest uruchamiany w celu późniejszego przejrzenia. 
* Tworzy prywatne i publiczne parę klucza dla każdego węzła tym węzeł hosta oraz wpisy w pliku authorized_keys.

>[AZURE.WARNING]Skrypt można utworzyć zagrożenie bezpieczeństwa. Upewnij się, że publicznej kluczowe informacje w ~/.ssh nie jest rozpowszechniane.


## <a name="configure-intel-mpi"></a>Konfigurowanie Intel MPI

Do uruchamiania aplikacji MPI na Azure Linux RDMA, musisz skonfigurować pewnych zmiennych środowiska specyficzne dla MPI firmy Intel. Oto przykładowy skrypt imprezie, aby skonfigurować zmiennych, aby uruchomić aplikację. Zmień ścieżkę do mpivars.sh dla instalacji MPI firmy Intel.

```
#!/bin/bash -x

# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster 

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh

export I_MPI_FABRICS=shm:dapl

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB
# Setting the variable to shm:dapl gives best performance for some applications
# If your application doesn’t take advantage of shared memory and MPI together, then set only dapl

export I_MPI_DAPL_PROVIDER=ofa-v2-ib0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

export I_MPI_DYNAMIC_CONNECTION=0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

# Command line to run the job

mpirun -n <number-of-cores> -ppn <core-per-node> -hostfile <hostfilename>  /path <path to the application exe> <arguments specific to the application>

#end
```

Format pliku hosta wygląda następująco: Dodawanie jednego wiersza dla każdego węzła w klastrze. Określ, czy prywatnych adresów IP z VNet zdefiniowany wcześniej, nie nazw DNS. Na przykład dwóch hostów z adresami IP 10.32.0.1 i 10.32.0.2 plik zawiera następujące elementy:

```
10.32.0.1:16
10.32.0.2:16
```

## <a name="run-mpi-on-a-basic-two-node-cluster"></a>Uruchamianie MPI w klastrze węzeł dwa podstawowe

Jeśli jeszcze tego nie zrobiono, najpierw skonfiguruj środowisko dla MPI firmy Intel. 

```
# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster 

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh
```

### <a name="run-a-simple-mpi-command"></a>Uruchamianie prostego polecenia MPI

Uruchamianie prostego polecenia MPI w jednym z węzłów obliczeń do wskazują, że MPI jest prawidłowo zainstalowany i komunikować się między co najmniej że obliczyć dwa węzły. Następujące polecenie **mpirun** działa polecenia **hostname** na dwa węzły.

```
mpirun -ppn 1 -n 2 -hosts <host1>,<host2> -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 hostname
```
Danych wyjściowych należy wyświetlić listę nazw wszystkich węzłów, które przekazywane jako danych wejściowych dla `-hosts`. Na przykład polecenie **mpirun** z dwóch węzłów zwraca wynik podobny do następującego:

```
cluster11
cluster12
```

### <a name="run-an-mpi-benchmark"></a>Uruchamianie testu MPI

Następujące polecenie Intel MPI uruchamia wzorca pingpong do weryfikacji konfiguracji klaster i połączenia z siecią RDMA.

```
mpirun -hosts <host1>,<host2> -ppn 1 -n 2 -env I_MPI_FABRICS=dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 IMB-MPI1 pingpong
```

W klastrze pracy z dwóch węzłów powinien zostać wyświetlony wynik podobny do następującego. W sieci Azure RDMA oczekują, że opóźnienie lub poniżej 3 mikrosekund wiadomości o rozmiarze do 512 bajtów.

```
#------------------------------------------------------------
#    Intel (R) MPI Benchmarks 4.0 Update 1, MPI-1 part
#------------------------------------------------------------
# Date                  : Fri Jul 17 23:16:46 2015
# Machine               : x86_64
# System                : Linux
# Release               : 3.12.39-44-default
# Version               : #5 SMP Thu Jun 25 22:45:24 UTC 2015
# MPI Version           : 3.0
# MPI Thread Environment:
# New default behavior from Version 3.2 on:
# the number of iterations per message size is cut down
# dynamically when a certain run time (per message size sample)
# is expected to be exceeded. Time limit is defined by variable
# "SECS_PER_SAMPLE" (=> IMB_settings.h)
# or through the flag => -time

# Calling sequence was:
# /opt/intel/impi_latest/bin64/IMB-MPI1 pingpong
# Minimum message length in bytes:   0
# Maximum message length in bytes:   4194304
#
# MPI_Datatype                   :   MPI_BYTE
# MPI_Datatype for reductions    :   MPI_FLOAT
# MPI_Op                         :   MPI_SUM
#
#
# List of Benchmarks to run:
# PingPong
#---------------------------------------------------
# Benchmarking PingPong
# #processes = 2
#---------------------------------------------------
       #bytes #repetitions      t[usec]   Mbytes/sec
            0         1000         2.23         0.00
            1         1000         2.26         0.42
            2         1000         2.26         0.85
            4         1000         2.26         1.69
            8         1000         2.26         3.38
           16         1000         2.36         6.45
           32         1000         2.57        11.89
           64         1000         2.36        25.81
          128         1000         2.64        46.19
          256         1000         2.73        89.30
          512         1000         3.09       157.99
         1024         1000         3.60       271.53
         2048         1000         4.46       437.57
         4096         1000         6.11       639.23
         8192         1000         7.49      1043.47
        16384         1000         9.76      1600.76
        32768         1000        14.98      2085.77
        65536          640        25.99      2405.08
       131072          320        50.68      2466.64
       262144          160        80.62      3101.01
       524288           80       145.86      3427.91
      1048576           40       279.06      3583.42
      2097152           20       543.37      3680.71
      4194304           10      1082.94      3693.63

# All processes entering MPI_Finalize

```



## <a name="next-steps"></a>Następne kroki

* Spróbuj wdrażanie i uruchamianie aplikacji usługi MPI Linux w klastrze Linux.

* Zapoznaj się z [dokumentacją biblioteki MPI Intel](https://software.intel.com/en-us/articles/intel-mpi-library-documentation/) orientacji na MPI firmy Intel.

* [Szybki Start szablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/intel-lustre-clients-on-centos) próby utworzenia klaster połysk firmy Intel przy użyciu obrazu opartego na CentOS HPC. Aby uzyskać szczegółowe informacje Zobacz ten [wpis w blogu](https://blogs.msdn.microsoft.com/arsen/2015/10/29/deploying-intel-cloud-edition-for-lustre-on-microsoft-azure/).
