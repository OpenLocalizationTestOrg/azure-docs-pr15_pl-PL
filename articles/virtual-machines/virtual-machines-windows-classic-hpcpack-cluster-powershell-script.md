<properties
   pageTitle="Skrypt programu PowerShell do wdrożenia klaster Windows HPC | Microsoft Azure"
   description="Za pomocą skryptu programu PowerShell do wdrożenia klastrze Windows HPC Pack w środowisku maszyn wirtualnych Azure"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""
   tags="azure-service-management,hpc-pack"/>
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="big-compute"
   ms.date="07/07/2016"
   ms.author="danlep"/>

# <a name="create-a-windows-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a>Tworzenie wysokiej wydajności (HPC) klaster systemu Windows z skrypt wdrożenia HPC Pack IaaS

Uruchom wdrożenie HPC Pack IaaS skrypt programu PowerShell do wdrożenia klastrze pełną HPC obciążenia systemu Windows w środowisku maszyn wirtualnych Azure. Klaster składa się z usługi Active Directory połączonych węzła głównego systemem Windows Server i Microsoft HPC Pack i dodatkowe okna obliczyć zasoby, które można określić. Jeśli chcesz wdrożyć klastrze HPC Pack Azure dla obciążenia Linux, zobacz [Tworzenie klastrze Linux HPC z skrypt wdrożenia HPC Pack IaaS](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md). Za pomocą szablonu Menedżera zasobów Azure wdrożenia klastrze HPC Pack. Przykłady zobacz [Tworzenie klastrze HPC](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/) i [Utwórz klaster HPC z niestandardowymi obliczyć węzeł obrazu](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-custom-image/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-files"></a>Przykład pliku konfiguracji

W poniższych przykładach należy zastąpić wartości dla subskrypcji identyfikator lub nazwę oraz nazwy konto i usługa.

### <a name="example-1"></a>Przykład 1

Plik konfiguracyjny następujące wdrożenie klastrze HPC Pack jest węzła głównego z lokalnej bazy danych i pięć obliczyć węzły systemu operacyjnego Windows Server 2012 R2. Z usługami w chmurze są tworzone bezpośrednio w lokalizacji, w Stanach Zjednoczonych Zachód. Drugi węzeł pełni rolę kontrolera domeny domenami.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionId>08701940-C02E-452F-B0B1-39D50119F267</SubscriptionId>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%1000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>5</NodeCount>
    <OSVersion>WindowsServer2012R2</OSVersion>
  </ComputeNodes>
</IaaSClusterConfig>
```

### <a name="example-2"></a>Przykład 2

Plik konfiguracyjny następujące wdraża klastrze HPC Pack w istniejącej domenami. Klaster ma 1 węzła głównego z lokalnej bazy danych i 12 obliczyć węzły z rozszerzeniem maszyn wirtualnych BGInfo zastosowane.
Automatyczne instalowanie aktualizacji systemu Windows jest wyłączona dla wszystkich maszyny wirtualne domenami. Z usługami w chmurze są tworzone bezpośrednio w lokalizacji, w Azji Wschodniej. Węzły obliczeniowej są tworzone w trzech usługami w chmurze i trzy konta miejsca do magazynowania: _MyHPCCN-0001_ do _MyHPCCN 0005_ w _MyHPCCNService01_ i _mycnstorage01_; _MyHPCCN 0006_ do _MyHPCCN0010_ _MyHPCCNService02_ i _mycnstorage02_; i _MyHPCCN 0011_ do _MyHPCCN 0012_ w _MyHPCCNService03_ i _mycnstorage03_). Węzły obliczeniowej są tworzone na podstawie istniejącego obrazu prywatne przechwycone z węzła obliczeń. Automatyczne powiększanie i zmniejszanie włączono usługę z domyślnym powiększanie i zmniejszanie odstępach.

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
     <NoWindowsAutoUpdate />
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
  </Certificates>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0001%</VMNamePattern>
<ServiceNamePattern>MyHPCCNService%01%</ServiceNamePattern>
<MaxNodeCountPerService>5</MaxNodeCountPerService>
<StorageAccountNamePattern>mycnstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>12</NodeCount>
    <ImageName HPCPackInstalled=”true”>MyHPCComputeNodeImage</ImageName>
    <VMExtensions>
       <VMExtension>
          <ExtensionName>BGInfo</ExtensionName>
          <Publisher>Microsoft.Compute</Publisher>
          <Version>1.*</Version>
       </VMExtension>
    </VMExtensions>
  </ComputeNodes>
  <AutoGrowShrink>
    <CertificateId>1</CertificateId>
  </AutoGrowShrink>
</IaaSClusterConfig>

```

### <a name="example-3"></a>Przykład 3

Plik konfiguracyjny następujące wdraża klastrze HPC Pack w istniejącej domenami. Klaster zawiera jednego węzła głównego, serwer bazy danych jeden z dyskiem danych 500 GB, 2 węzły brokera systemu operacyjnego Windows Server 2012 R2 i pięć węzły do uruchamiania systemu operacyjnego Windows Server 2012 R2. Usługa w chmurze MyHPCCNService zostanie utworzona w grupie koligacji *MyIBAffinityGroup*i innych usług w chmurze są tworzone w grupie koligacji *MyAffinityGroup*. Interfejsu API usługi REST harmonogramu zadania HPC i portalu HPC są włączone na węzła głównego.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>    
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>NewRemoteDB</DBOption>
    <DBVersion>SQLServer2014_Enterprise</DBVersion>
    <DBServer>
      <VMName>MyDBServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>ExtraLarge</VMSize>
      <DataDiskSizeInGB>500</DataDiskSizeInGB>
    </DBServer>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>A8</VMSize>
<NodeCount>5</NodeCount>
<AffinityGroup>MyIBAffinityGroup</AffinityGroup>
  </ComputeNodes>
  <BrokerNodes>
    <VMNamePattern>MyHPCBN-%0000%</VMNamePattern>
    <ServiceName>MyHPCBNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>2</NodeCount>
  </BrokerNodes>
</IaaSClusterConfig>
```



### <a name="example-4"></a>Przykład 4

Plik konfiguracyjny następujące wdraża klastrze HPC Pack w istniejącej domenami. Klaster ma dwa węzła głównego z lokalnej bazy danych, są tworzone dwa szablony węzeł Azure i trzy węzły Azure średni rozmiar są tworzone dla szablonu Azure węzeł _AzureTemplate1_. Plik skryptu działa na węzła głównego po skonfigurowaniu węzła głównego.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
<VMSize>ExtraLarge</VMSize>
    <PostConfigScript>c:\MyHNPostActions.ps1</PostConfigScript>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
    <Certificate>
      <Id>2</Id>
      <PfxFile>d:\mytestcert2.pfx</PfxFile>
    </Certificate>    
  </Certificates>
  <AzureBurst>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate1</TemplateName>
      <SubscriptionId>bb9252ba-831f-4c9d-ae14-9a38e6da8ee4</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc1</ServiceName>
      <StorageAccount>myteststorage1</StorageAccount>
      <NodeCount>3</NodeCount>
      <RoleSize>Medium</RoleSize>
    </AzureNodeTemplate>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate2</TemplateName>
      <SubscriptionId>ad4b9f9f-05f2-4c74-a83f-f2eb73000e0b</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc2</ServiceName>
      <StorageAccount>myteststorage2</StorageAccount>
      <Proxy>
        <UsesStaticProxyCount>false</UsesStaticProxyCount>     
        <ProxyRatio>100</ProxyRatio>
        <ProxyRatioBase>400</ProxyRatioBase>
      </Proxy>
      <OSVersion>WindowsServer2012</OSVersion>
    </AzureNodeTemplate>
  </AzureBurst>
</IaaSClusterConfig>
```

## <a name="troubleshooting"></a>Rozwiązywanie problemów


* **Błąd "Brak VNet"** — po uruchomieniu skryptu wdrażania wielu klastrów platformy Azure jednocześnie w obszarze jedną subskrypcję, co najmniej jeden wdrożeń może zakończyć się niepowodzeniem z powodu błędu "VNet *VNet\_nazwa* nie istnieje".
Jeśli wystąpi ten błąd, uruchom skrypt ponownie, aby niepowodzenie wdrożenia.

* **Problem, dostęp do Internetu z Azure wirtualną sieć** - utworzyć klaster za pomocą nowego kontrolera domeny za pomocą skryptu wdrażania, czy ręcznie awansować węzła głównego maszyn wirtualnych do kontrolera domeny, mogą wystąpić problemy maszyny wirtualne nawiązywanie połączenia z Internetem. Ten problem może wystąpić, jeśli usługi przesyłania dalej serwera DNS jest automatycznie konfigurowane na kontrolerze domeny i usługi przesyłania dalej serwera DNS nie rozwiąże poprawnie.

    Aby obejść ten problem, zaloguj się do kontrolera domeny i albo Usuń ustawienie konfiguracji usługi przesyłania dalej lub skonfigurować prawidłowe usługi przesyłania dalej serwera DNS. Aby skonfigurować to ustawienie, w Menedżerze serwera kliknij pozycję **Narzędzia** >
    **DNS** , aby otworzyć Menedżera DNS, a następnie kliknij dwukrotnie pozycję **usługi przesyłania dalej**.

* **Problem, uzyskiwanie dostępu do sieci RDMA z maszyny wirtualne obliczeniowych** — w przypadku dodawania do uruchamiania systemu Windows Server lub brokera węzeł maszyny wirtualne za pomocą RDMA dostępem do rozmiaru, takich jak A8 lub A9, mogą wystąpić problemy te maszyny wirtualne nawiązywanie połączenia z siecią aplikacji RDMA. Jeden przyczynę tego problemu jest, jeśli rozszerzenie HpcVmDrivers nie jest poprawnie zainstalowane po dodaniu maszyny wirtualne z klastrem. Na przykład rozszerzenie może być zablokowany w trakcie instalacji.

    Aby obejść ten problem, najpierw sprawdź stan rozszerzenie w maszyny wirtualne. Jeśli rozszerzenie nie jest zainstalowany poprawnie, spróbuj usunąć węzły z klaster HPC, a następnie dodaj węzły ponownie. Można na przykład dodać węzłów przetwarzania maszyny wirtualne, uruchamiając skrypt HpcIaaSNode.ps1 Dodaj na węzła głównego.
    
## <a name="next-steps"></a>Następne kroki

* Spróbuj uruchomić test obciążenie pracą w klastrze. Na przykład zobacz HPC Pack [Przewodnik wprowadzenie](https://technet.microsoft.com/library/jj884144).

* Samouczek skrypt wdrożenia klaster i uruchomić HPC obciążenie pracą zobacz [Rozpoczynanie pracy z klastrem HPC Pack platformy Azure, aby uruchomić program Excel i SOA obciążenia](virtual-machines-windows-excel-cluster-hpcpack.md).

* Spróbuj użyć narzędzi HPC Pack, aby uruchomić, zatrzymać, dodawanie i usuń węzły obliczeń z klastrem utworzone. Zobacz [Zarządzanie obliczyć węzły w klastrze HPC Pack platformy Azure](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md).

* Aby uzyskać skonfigurować przesyłać zadania z klastrem z komputera lokalnego, zobacz [Przesyłanie HPC zadania z komputera lokalnego do klastrów HPC Pack platformy Azure](virtual-machines-windows-hpcpack-cluster-submit-jobs.md).
