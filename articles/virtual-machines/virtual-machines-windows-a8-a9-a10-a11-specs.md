<properties
 pageTitle="Temat obliczeniowych maszyny wirtualne z systemem Windows | Microsoft Azure"
 description="Informacje ogólne i zagadnienia dotyczące korzystania z usługi Windows maszyny wirtualne i chmura Azure rozmiarów obliczeniowych serii H i A8, A9 A10 i A11"
 services="virtual-machines-windows, cloud-services"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="about-h-series-and-compute-intensive-a-series-vms"></a>Informacje o serii H i obliczeniowych maszyny wirtualne A serii

Oto podstawowe informacje i zagadnienia dotyczące korzystania z nowszej Azure H-serii i wcześniejszych wystąpienia A8, A9 A10 i A11, nazywane także wystąpienia *obliczeniowych* . W tym artykule omówiono przy użyciu tych wystąpień dla maszyny wirtualne systemu Windows. Ten artykuł jest także dostępny dla [Maszyny wirtualne Linux](virtual-machines-linux-a8-a9-a10-a11-specs.md).


[AZURE.INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-to-the-rdma-network"></a>Dostęp do sieci RDMA

Można utworzyć klastrów wystąpień RDMA dostępem do systemu Windows Server i wdrażanie jednego z obsługiwanych implementacji MPI, aby można było korzystać z siecią Azure RDMA. Ten czas oczekiwania niska, wysokiej przepustowości sieci jest zarezerwowane dla ruchu MPI.

* **System operacyjny**
    * **Maszyn wirtualnych** - Windows Server 2012 R2, Windows Server 2012
    * **Usług w chmurze** — Windows Server 2012 R2, Windows Server 2012, system operacyjny Windows Server 2008 R2 gościa rodziny

* **MPI** - MPI firmy Microsoft (MS-MPI) 2012 R2 lub nowszych wersjach biblioteki MPI firmy Intel 5.x

Obsługiwane implementacji MPI za pomocą interfejsu Microsoft sieci bezpośrednio do komunikowania się między wystąpieniami. Zobacz [Konfigurowanie klastrze RDMA systemu Windows z HPC Pack do uruchamiania aplikacji MPI](virtual-machines-windows-classic-hpcpack-rdma-cluster.md) i [Używanie wielu wystąpień zadania do uruchamiania aplikacji wiadomości przechodzące Interface (MPI) w partii Azure](../batch/batch-mpi.md) opcje wdrażania i czynności konfiguracyjne próbki.


>[AZURE.NOTE]Na RDMA dostępem do obliczeniowych maszyny wirtualne rozszerzenie HpcVmDrivers musi zostać dodana do maszyny wirtualne, aby zainstalować sterowniki urządzeń sieci systemu Windows, które są wymagane przez RDMA łączności. W większości wdrożeń rozszerzenia HpcVmDrivers jest dodawany automatycznie. Jeśli musisz samodzielnie dodać rozszerzenie, zobacz [Zarządzanie maszyn wirtualnych rozszerzenia](virtual-machines-windows-classic-manage-extensions.md).

## <a name="considerations-for-hpc-pack-and-windows"></a>Zagadnienia związane z dodatkiem Service Pack HPC i systemu Windows

[Microsoft HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), bezpłatne klaster HPC firmy Microsoft i rozwiązanie do zarządzania zadania, nie jest wymagane do użycia wystąpienia obliczeniowych w systemie Windows Server. Jest jednak jednej opcji do tworzenia klastrów obliczeniowych w Azure powoduje uruchomienie aplikacji MPI systemu Windows i innych obciążenia HPC. HPC Pack 2012 R2 i nowszych wersjach zawierają środowiska wykonawczego dla MS-MPI, którego można używać z siecią Azure RDMA po wdrożeniu miejscu RDMA dostępem do maszyny wirtualne.

Aby uzyskać więcej informacji i listy kontrolne, aby użyć wystąpienia obliczeniowych pakietem HPC w systemie Windows Server zobacz [Konfigurowanie klastrze RDMA systemu Windows z HPC Pack do uruchamiania aplikacji MPI](virtual-machines-windows-classic-hpcpack-rdma-cluster.md).




## <a name="next-steps"></a>Następne kroki

* Aby uzyskać szczegółowe informacje o dostępności i ceny rozmiarów obliczeniowych zobacz [maszyn wirtualnych ceny](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows) i [ceny usług w chmurze](https://azure.microsoft.com/pricing/details/cloud-services/).

* Pojemność i dysku szczegóły zobacz [rozmiarów maszyn wirtualnych](virtual-machines-linux-sizes.md).

* Aby rozpocząć pracę, wdrażania i za pomocą wystąpień obliczeniowych pakietem HPC w systemie Windows, zobacz [Konfigurowanie klastrze RDMA systemu Windows z HPC Pack do uruchamiania aplikacji MPI](virtual-machines-windows-classic-hpcpack-rdma-cluster.md).

* Aby dowiedzieć się, jak za pomocą wystąpień A8 i A9 do uruchamiania aplikacji MPI z partii Azure zobacz [Użyj wielu wystąpień zadania do uruchamiania aplikacji wiadomości przechodzące Interface (MPI) w partii Azure](../batch/batch-mpi.md).
