<properties
 pageTitle="Informacje o obliczeniowych maszyny wirtualne z systemem Linux | Microsoft Azure"
 description="Uzyskaj informacje i uwagi dotyczące przy użyciu rozmiarów obliczeniowych serii H i A8, A9 A10 i A11 dla maszyny wirtualne Linux"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="about-h-series-and-compute-intensive-a-series-vms"></a>Informacje o serii H i obliczeniowych maszyny wirtualne A serii 

Oto podstawowe informacje i zagadnienia dotyczące korzystania z nowszej Azure H-serii i wcześniejszych rozmiarów A8, A9 A10 i A11, nazywane także wystąpienia *obliczeniowych* . W tym artykule omówiono przy użyciu rozmiary dla maszyny wirtualne Linux. Ten artykuł jest także dostępny dla [Maszyny wirtualne systemu Windows](virtual-machines-windows-a8-a9-a10-a11-specs.md).




[AZURE.INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-to-the-rdma-network"></a>Dostęp do sieci RDMA

Możesz utworzyć klastrów RDMA dostępem do Linux maszyny wirtualne że uruchamianie jedną z następujących obsługiwanych dystrybucji Linux HPC oraz obsługiwanej implementacji MPI, aby można było korzystać z siecią Azure RDMA. Opcje wdrażania i przykładowe kroków konfiguracyjnych, zobacz [Konfigurowanie klastrze Linux RDMA do uruchamiania aplikacji MPI](virtual-machines-linux-classic-rdma-cluster.md) .

* **Rozkład** — należy wdrożyć maszyny wirtualne z obsługą RDMA SUSE Linux Enterprise Server (SLES) lub oparte na OpenLogic CentOS HPC obrazów w Azure Marketplace. Tylko następujące obrazy Marketplace obsługuje niezbędne sterowniki Linux RDMA:

    * SLES 12 z dodatkiem SP1 dla HPC, SLES 12 z dodatkiem SP1 dla HPC (Premium)
    
    * SLES 12 dla HPC, SLES 12 dla HPC (Premium)
    
    * Oparte na centOS 7.1 HPC
    
    * Oparte na centOS HPC 6.5
    
    >[AZURE.NOTE]Maszyny wirtualne serii H zalecamy SP1 12 SLES HPC obrazu albo CentOS 7.1 HPC obrazu.
    >
    >W przypadku obrazów opartych na CentOS HPC aktualizacji jądra są wyłączone w pliku konfiguracji **yum** . Jest to spowodowane sterowniki Linux RDMA są rozdzielane jako pakiet obrotów na MINUTĘ, a aktualizacje sterowników może nie działać, jeśli jądrze zostanie zaktualizowany.

* **MPI** - biblioteki MPI Intel 5.x

    W zależności od obraz Marketplace możesz wybrać, osobnej licencji, instalacji lub konfiguracji MPI firmy Intel mogą być potrzebne w następujący sposób: 
    
    * **SLES 12 z dodatkiem SP1 HPC obrazu** — Zainstaluj pakietów MPI firmy Intel rozpowszechniane na maszyn wirtualnych, uruchamiając następujące polecenie:
    
            sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm

    * **12 SLES HPC obrazu** — należy oddzielnie zarejestrować pobrać i zainstalować MPI firmy Intel. Zobacz [Podręcznik instalacji biblioteki MPI firmy Intel](https://software.intel.com/sites/default/files/managed/7c/2c/intelmpi-2017-installguide-linux.pdf).
    
    * **Obrazy oparte na centOS HPC** - 5.1 MPI firmy Intel jest już zainstalowany.  

    Konfiguracji systemu dodatkowe jest potrzebne do uruchamiania zadań MPI na grupowany maszyny wirtualne. Na przykład w klastrze maszyny wirtualne należy ustanowić zaufanie między węzły obliczeń. Dla ustawienia typowe zobacz [Konfigurowanie klastrze Linux RDMA do uruchamiania aplikacji MPI](virtual-machines-linux-classic-rdma-cluster.md).


## <a name="considerations-for-hpc-pack-and-linux"></a>Zagadnienia związane z dodatkiem Service Pack HPC i Linux

[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), bezpłatne klaster HPC firmy Microsoft i rozwiązanie do zarządzania zadania, zawiera jedną opcję przy użyciu wystąpienia obliczeniowych z systemem Linux. Najnowszej wersji obsługi HPC Pack 2012 R2 kilka dystrybucji Linux do uruchomienia obliczyć węzły wdrożony w maszyny wirtualne Azure, zarządzane przez system Windows Server węzła głównego. Węzłów obliczeń Linux RDMA dostępem do uruchamiania MPI firmy Intel HPC Pack można planowanie i prowadzenie Linux MPI aplikacjom uzyskiwać dostęp do sieci RDMA. Aby rozpocząć pracę, zobacz [Rozpoczynanie pracy z Linux węzły do uruchamiania w klastrze HPC Pack platformy Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

## <a name="network-topology-considerations"></a>Uwagi dotyczące topologii sieciowych

* Na włączone RDMA Linux maszyny wirtualne platformy Azure Eth1 jest zarezerwowana RDMA ruchu w sieci. Nie należy zmieniać ustawienia Eth1 ani żadnych informacji w pliku konfiguracji, odwołując się do tej sieci. Eth0 jest zarezerwowana do zwykłego Azure ruch sieciowy.

* Platformy Azure IP nad InfiniBand (IB) nie jest obsługiwane. Tylko RDMA nad IB jest obsługiwana.

## <a name="rdma-driver-updates-for-sles-12"></a>Aktualizacje sterownika RDMA SLES 12

Po utworzeniu maszyny na podstawie obrazu SLES 12 HPC, może być konieczne aktualizować sterowniki RDMA na maszyny wirtualne łączności sieciowej RDMA. 

>[AZURE.IMPORTANT]Ten krok jest **wymagane** dla SLES 12 w przypadku wdrożeń maszyn wirtualnych HPC we wszystkich regionach Azure. 
>Ten krok nie jest potrzebny, jeśli wdrażanie SLES 12 SP1 HPC, oparte na CentOS HPC 7.1 lub oparte na CentOS HPC 6.5 maszyn wirtualnych. 

Przed zaktualizowaniem sterowniki Zatrzymaj wszystkie procesy **zypper** i wszystkie procesy, które Zablokuj baz danych repo SUSE w maszyn wirtualnych. W przeciwnym razie sterowniki nie może zaktualizować poprawnie.  

Aby zaktualizować sterowniki Linux RDMA na poszczególnych maszyn wirtualnych, uruchom jeden z następujących zestawów poleceń interfejsu Azure wiersza polecenia na komputerze klienckim.

**12 SLES dla maszyn wirtualnych HPC obsługi administracyjnej w modelu Klasyczny wdrażania**

```
azure config mode asm

azure vm extension set <vm-name> RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1
```

**12 SLES dla maszyn wirtualnych HPC obsługi administracyjnej w modelu wdrożenia Menedżera zasobów**

```
azure config mode arm

azure vm extension set <resource-group> <vm-name> RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1
```

>[AZURE.NOTE]Może upłynąć trochę czasu, aby zainstalować sterowniki, a polecenie jest zwracane bez danych wyjściowych. Po aktualizacji usługi maszyn wirtualnych zacznie i jest gotowa do użycia w kilka minut.

### <a name="sample-script-for-driver-updates"></a>Przykładowy skrypt aktualizacji sterownika

Jeśli masz klastrze SLES 12 pośrednictwem HPC SMS, możesz skrypt aktualizacji sterownika we wszystkich węzłach w klastrze. Na przykład poniższy skrypt zaktualizowane sterowniki w klastrze węzeł 8.

```

#!/bin/bash -x

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Plug in appropriate numbers in the for loop below.

for (( i=11; i<19; i++ )); do

# For VMs created in the classic deployment model, use the following command in your script.

azure vm extension set $vmname$i RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1

# For VMs created in the Resource Manager deployment model, use the following command in your script.

# azure vm extension set <resource-group> $vmname$i RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1

done

```


## <a name="next-steps"></a>Następne kroki

* Aby uzyskać szczegółowe informacje o dostępności i ceny rozmiarów obliczeniowych zobacz [ceny maszyn wirtualnych](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux).

* Pojemność i dysku szczegóły zobacz [rozmiarów maszyn wirtualnych](virtual-machines-linux-sizes.md).

* Aby rozpocząć pracę, wdrażania i rozmiary obliczeniowych przy użyciu RDMA w systemie Linux, zobacz [Konfigurowanie klastrze Linux RDMA do uruchamiania aplikacji MPI](virtual-machines-linux-classic-rdma-cluster.md).


