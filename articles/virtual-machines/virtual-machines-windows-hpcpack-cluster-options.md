<properties
 pageTitle="Opcje klaster systemu Windows HPC Pack w chmurze | Microsoft Azure"
 description="Więcej informacji na temat opcji w programie Microsoft HPC Pack do tworzenia i zarządzania wysokiej wydajności systemu Windows obliczeniowych klaster (HPC) w chmurze Azure"
 services="virtual-machines-windows,cloud-services,batch"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="09/26/2016"
 ms.author="danlep"/>

# <a name="options-with-hpc-pack-to-create-and-manage-a-windows-hpc-cluster-in-azure"></a>Opcje pakietem HPC tworzyć i zarządzać nimi klaster HPC systemu Windows Azure

[AZURE.INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

W tym artykule omówiono opcje, aby utworzyć klastrów HPC Pack do uruchomienia obciążenia systemu Windows. Istnieją również opcji do tworzenia klastrów, aby uruchomić [obciążenia Linux HPC pakietem HPC](virtual-machines-linux-hpcpack-cluster-options.md).


## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a>Uruchamianie klastrze HPC Pack w maszyny wirtualne Azure

### <a name="azure-templates"></a>Szablony Azure

* (Marketplace) [Klaster HPC Pack dla obciążenia systemu Windows](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/)

* (Marketplace) [Klaster HPC Pack dla programu Excel obciążenia](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterexcelcn/)

* (Szybki Start) [Utwórz klaster HPC](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster)

* (Szybki Start) [Utwórz klaster HPC z obrazem węzeł niestandardowych obliczeń](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-custom-image)

### <a name="azure-vm-images"></a>Azure obrazów maszyn wirtualnych

* [HPC dodatkiem Service Pack dla systemu Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/)

* [Węzeł obliczeń HPC Pack w systemie Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodeonwindowsserver2012r2/)

* [HPC Pack obliczyć węzeł z programem Excel w systemie Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodewithexcelonwindowsserver2012r2/)



### <a name="powershell-deployment-script"></a>Skrypt wdrożenia programu PowerShell

* [Tworzenie klastrze HPC z skrypt wdrożenia HPC Pack IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md)

### <a name="tutorials"></a>Samouczki

* [Samouczek: Wprowadzenie do klastrze HPC Pack platformy Azure, aby uruchomić program Excel i SOA obciążeń pracą](virtual-machines-windows-excel-cluster-hpcpack.md)



### <a name="manual-deployment-with-the-azure-portal"></a>Ręczne wdrożenia Portal Azure

* [Konfigurowanie węzła głównego klastrze HPC Pack w maszyn wirtualnych Azure](virtual-machines-windows-hpcpack-cluster-headnode.md)

### <a name="cluster-management"></a>Zarządzanie klastrem

* [Zarządzanie węzły do uruchamiania w klastrze HPC Pack platformy Azure](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md)

* [Powiększanie i zmniejszanie zasobów Azure do uruchamiania w klastrze HPC Pack](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md)

* [Przesyłanie zadań z klastrem HPC Pack platformy Azure](virtual-machines-windows-hpcpack-cluster-submit-jobs.md)

* [Zarządzanie zadaniami pakietu HPC](https://technet.microsoft.com/library/jj899585.aspx)


## <a name="add-worker-role-nodes-to-an-hpc-pack-cluster"></a>Dodawanie węzły roli pracownika do klastrów HPC Pack


* [Serii wystąpieniach Azure pracownik pakietem HPC](https://technet.microsoft.com/library/gg481749.aspx)

* [Samouczek: Konfigurowanie klastrze hybrydowego z dodatkiem Service Pack HPC platformy Azure](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)

* [Dodawanie węzły Azure "serii" do węzła głównego HPC Pack platformy Azure](virtual-machines-windows-classic-hpcpack-cluster-node-burst.md)


## <a name="integrate-with-azure-batch"></a>Integracja z partii Azure 

* [Serii Azure partii pakietem HPC](https://technet.microsoft.com/library/mt612877.aspx)

## <a name="create-rdma-clusters-for-mpi-workloads"></a>Tworzenie klastrów RDMA dla obciążenia MPI

* [Konfigurowanie klastrze RDMA systemu Windows z dodatkiem Service Pack HPC do uruchamiania aplikacji MPI](virtual-machines-windows-classic-hpcpack-rdma-cluster.md)
