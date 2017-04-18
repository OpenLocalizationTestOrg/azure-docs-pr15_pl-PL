<properties
 pageTitle="Opcje klaster Linux HPC Pack w chmurze | Microsoft Azure"
 description="Więcej informacji na temat opcji w programie Microsoft HPC Pack tworzyć i zarządzać nimi Linux wysokiej wydajności przetwarzania klaster (HPC) w chmurze Azure"
 services="virtual-machines-linux,cloud-services"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="09/26/2016"
 ms.author="danlep"/>

# <a name="options-with-hpc-pack-to-create-and-manage-an-hpc-cluster-in-azure-for-linux-workloads"></a>Opcje pakietem HPC tworzyć i zarządzać nimi klastrze HPC Azure dla obciążenia Linux

[AZURE.INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

W tym artykule omówiono opcji używać HPC Pack do przeprowadzania obciążenia Linux. Istnieją również opcje uruchamiania [obciążenia HPC systemu Windows z dodatkiem Service Pack HPC](virtual-machines-windows-hpcpack-cluster-options.md).

## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a>Uruchamianie klastrze HPC Pack w maszyny wirtualne Azure

### <a name="azure-templates"></a>Szablony Azure


* (Marketplace) [Klaster HPC Pack dla obciążenia Linux](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/)

* (Szybki Start) [Utwórz klaster HPC z systemem Linux obliczyć węzły](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn)


### <a name="powershell-deployment-script"></a>Skrypt wdrożenia programu PowerShell

* [Tworzenie klastrze Linux HPC z skrypt wdrożenia HPC Pack IaaS](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md)

### <a name="tutorials"></a>Samouczki

* [Samouczek: Wprowadzenie do Linux węzły do uruchamiania w klastrze HPC Pack platformy Azure](virtual-machines-linux-classic-hpcpack-cluster.md)

* [Samouczek: Uruchom NAMD z Microsoft HPC Pack na Linux obliczyć węzły platformy Azure](virtual-machines-linux-classic-hpcpack-cluster-namd.md)

* [Samouczek: Uruchom OpenFOAM z Microsoft HPC Pack w klastrze Linux RDMA platformy Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)

* [Samouczek: Uruchom GWIAZDA-CCM + z Microsoft HPC Pack na Linux RDMA klaster platformy Azure](virtual-machines-linux-classic-hpcpack-cluster-starccm.md)

### <a name="cluster-management"></a>Zarządzanie klastrem

* [Przesyłanie zadań z klastrem HPC Pack platformy Azure](virtual-machines-windows-hpcpack-cluster-submit-jobs.md)

* [Zarządzanie zadaniami pakietu HPC](https://technet.microsoft.com/library/jj899585.aspx)


## <a name="create-rdma-clusters-for-mpi-workloads"></a>Tworzenie klastrów RDMA dla obciążenia MPI

* [Samouczek: Uruchom OpenFOAM z Microsoft HPC Pack w klastrze Linux RDMA platformy Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)

* [Konfigurowanie klastrze Linux RDMA do uruchamiania aplikacji MPI](virtual-machines-linux-classic-rdma-cluster.md)

