<properties
   pageTitle="Skrypt programu PowerShell do wdrożenia klaster Linux HPC | Microsoft Azure"
   description="Za pomocą skryptu programu PowerShell do wdrożenia klastrze Linux HPC Pack w środowisku maszyn wirtualnych Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""
   tags="azure-service-management,hpc-pack"/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="big-compute"
   ms.date="07/07/2016"
   ms.author="danlep"/>

# <a name="create-a-linux-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a>Tworzenie Linux wysokiej wydajności przetwarzania klaster (HPC) z skrypt wdrożenia HPC Pack IaaS

Uruchom wdrożenie HPC Pack IaaS skrypt programu PowerShell do wdrożenia klastrze pełną HPC obciążenia Linux w środowisku maszyn wirtualnych Azure. Klaster składa się z sprzężone usługi Active Directory węzła głównego systemem Windows Server i Microsoft HPC Pack i węzłów obliczeń, uruchamianych dystrybucji Linux obsługiwanych przez HPC Pack. Jeśli chcesz wdrożyć klastrze HPC Pack w obciążenia Azure dla systemu Windows, zobacz [Tworzenie klastrze HPC systemu Windows z skrypt wdrożenia HPC Pack IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md). Za pomocą szablonu Menedżera zasobów Azure wdrożenia klastrze HPC Pack. Na przykład zobacz [Tworzenie klastrze HPC z systemem Linux obliczyć węzły](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-file"></a>Przykład pliku konfiguracji

Następujące plik konfiguracyjny tworzy nowy kontroler domeny i domenami i wdraża klastrze HPC Pack, który ma 1 węzła głównego z lokalnej bazy danych i 10 węzły obliczeń Linux. Z usługami w chmurze są tworzone bezpośrednio w lokalizacji, w Azji Wschodniej. Węzły obliczeń Linux są tworzone w 2 usług w chmurze i 2 konta miejsca do magazynowania (to znaczy _MyLnxCN 0001_ do _MyLnxCN 0005_ w _MyLnxCNService01_ i _mylnxstorage01_) i _MyLnxCN 0006_ do _MyLnxCN 0010_ w _MyLnxCNService02_ i _mylnxstorage02_. Węzły obliczeniowej są tworzone na podstawie obrazu OpenLogic CentOS w wersji 7.0 Linux. 

Należy zastąpić wartości nazwę subskrypcji i konto i usługa nazw.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>MyDCServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>Large</VMSize>
    </DomainController>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>MyLnxCN-%0001%</VMNamePattern>
    <ServiceNamePattern>MyLnxCNService%01%</ServiceNamePattern>
    <MaxNodeCountPerService>5</MaxNodeCountPerService>
    <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>10</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20150325 </ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```
## <a name="troubleshooting"></a>Rozwiązywanie problemów

* **Błąd "Brak VNet"** — po uruchomieniu skryptu wdrażania HPC Pack IaaS wdrożenia wielu klastrów platformy Azure jednocześnie w obszarze jedną subskrypcję, co najmniej jeden wdrożeń może zakończyć się niepowodzeniem z powodu błędu "VNet *VNet\_nazwa* nie istnieje".
Jeśli wystąpi ten błąd, uruchom ponownie skrypt wdrożenia nie powiodło się.

* **Problem, dostęp do Internetu z Azure wirtualną sieć** — tworzenie klastrze HPC Pack za pomocą nowego kontrolera domeny za pomocą skryptu wdrażania, czy ręcznie awansować węzła głównego maszyn wirtualnych do kontrolera domeny, mogą wystąpić problemy maszyny wirtualne w Azure wirtualną sieć nawiązywanie połączenia z Internetem. Przyczyną może być automatycznie skonfigurowano usługi przesyłania dalej serwera DNS na kontrolerze domeny i usługi przesyłania dalej serwera DNS nie rozwiąże poprawnie.

    Aby obejść ten problem, zaloguj się do kontrolera domeny i albo Usuń ustawienie konfiguracji usługi przesyłania dalej lub skonfigurować prawidłowe usługi przesyłania dalej serwera DNS. W tym celu należy w Menedżerze serwera kliknij pozycję **Narzędzia** >
    **DNS** , aby otworzyć Menedżera DNS, a następnie kliknij dwukrotnie pozycję **usługi przesyłania dalej**.
    
## <a name="next-steps"></a>Następne kroki

* Zobacz [Rozpoczynanie pracy z Linux węzły do uruchamiania w klastrze HPC Pack platformy Azure](virtual-machines-linux-classic-hpcpack-cluster.md) informacji na temat obsługiwanych dystrybucji Linux, przenoszenie danych i przesyłania zadań do klastrze HPC Pack z systemem Linux obliczyć węzły.
* Samouczki, które klastrze tworzenie i uruchamianie obciążenie pracą Linux HPC za pomocą skryptu zobacz:
    * [Uruchamianie NAMD z Microsoft HPC Pack w węzłach obliczeń Linux platformy Azure](virtual-machines-linux-classic-hpcpack-cluster-namd.md)
    * [Uruchamianie OpenFOAM z Microsoft HPC Pack w węzłach obliczeń Linux platformy Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)
    * [Uruchamianie GWIAZDA-CCM + z Microsoft HPC Pack w systemie Linux obliczyć węzły platformy Azure](virtual-machines-linux-classic-hpcpack-cluster-starccm.md)
