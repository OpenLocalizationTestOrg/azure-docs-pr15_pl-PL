<properties
 pageTitle="Autoscale HPC Pack węzłach | Microsoft Azure"
 description="Automatyczne powiększanie i zmniejszanie liczby HPC Pack klaster komputerowe węzłów na Azure"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="07/22/2016"
 ms.author="danlep"/>

# <a name="automatically-grow-and-shrink-the-hpc-pack-cluster-resources-in-azure-according-to-the-cluster-workload"></a>Automatyczne powiększanie i zmniejszanie zasobów klaster HPC Pack w Azure zgodnie z pracą klaster




Jeśli wdrażanie Azure "serii" węzły w klastrze HPC Pack lub tworzenie klastrze HPC Pack w maszyny wirtualne Azure, warto umożliwia automatyczne Zwiększ lub Zmniejsz liczbę zasobów Azure obliczeń, takich jak węzły lub rdzenie zgodnie z bieżącym obciążenie pracą w klastrze. Pozwala korzystać z usługi Azure zasobów zwiększyć wydajność i kontrolowania kosztów.
W tym celu ustawiania właściwości klaster HPC Pack **AutoGrowShrink**. Możesz również uruchomić skrypt programu PowerShell HPC **AzureAutoGrowShrink.ps1** , który jest zainstalowany wraz z dodatkiem Service Pack HPC.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Ponadto obecnie możesz można tylko automatycznie powiększanie i zmniejszanie węzły obliczeń HPC Pack uruchomione systemu operacyjnego Windows Server.

## <a name="set-the-autogrowshrink-cluster-property"></a>Należy ustawić właściwość klaster AutoGrowShrink

### <a name="prerequisites"></a>Wymagania wstępne

* **HPC Pack 2012 R2 aktualizacji 2 lub nowszy klaster** - głowy węzła może być używany obsługiwanych lokalnie lub maszyn wirtualnych Azure. Zobacz [Konfigurowanie klastrze hybrydowego z dodatkiem Service Pack HPC](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) , aby rozpocząć pracę z lokalnego węzła głównego i węzłów Azure "serii". Zobacz [skrypt wdrożenia HPC Pack IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) szybko wdrożenia klastrze HPC Pack w maszyny wirtualne Azure.


* **Klaster z węzła głównego w Azure** - użycie skrypt wdrożenia HPC Pack IaaS Aby utworzyć klaster, Włącz właściwość klaster **AutoGrowShrink** przez ustawienie opcji AutoGrowShrink w pliku konfiguracji klaster. Aby uzyskać szczegółowe informacje Zobacz dokumentację towarzyszących [Pobierz skrypt](https://www.microsoft.com/download/details.aspx?id=44949). 

    Możesz również włączyć właściwość klaster **AutoGrowShrink** po wdrożeniu klaster przy użyciu polecenia programu PowerShell HPC opisane w następnej sekcji. Aby przygotować się do tego, najpierw należy wykonać następujące czynności:
    1. Konfigurowanie certyfikatu usługi zarządzania Azure na węzła głównego i Azure subskrypcji. Wdrożenie test za pomocą certyfikatu podpisem domyślne Microsoft Azure HPC HPC Pack instaluje je na węzła głównego i po prostu przekazać ten certyfikat do subskrypcji usługi Azure. Opcje oraz czynności zobacz [wskazówki biblioteki w witrynie TechNet](https://technet.microsoft.com/library/gg481759.aspx).
    2. Uruchom **polecenie regedit** na węzła głównego, przejdź do HKLM\SOFTWARE\Micorsoft\HPC\IaasInfo i Dodaj nową wartość ciągu. Ustaw nazwę wartości do "Odcisku palca", a wartość danych do odcisku palca certyfikatu w kroku 1.


### <a name="hpc-powershell-commands-to-set-the-autogrowshrink-property"></a>Polecenia programu PowerShell HPC, aby ustawić właściwość AutoGrowShrink

Poniższe czynności są przykładowe polecenia programu PowerShell HPC, aby ustawić **AutoGrowShrink** i dostosować jego zachowanie dodatkowe parametry. Zobacz [Parametry AutoGrowShrink](#AutoGrowShrink-parameters) w dalszej części tego artykułu pełną listę ustawień. 

Aby wykonać te polecenia, należy uruchomić HPC programu PowerShell dla głowy węzła jako administrator.

**Aby włączyć właściwość AutoGrowShrink**

    Set-HpcClusterProperty –EnableGrowShrink 1

**Aby wyłączyć właściwość AutoGrowShrink**

    Set-HpcClusterProperty –EnableGrowShrink 0

**Aby zmienić interwał zwiększania w minutach**

    Set-HpcClusterProperty –GrowInterval <interval>

**Aby zmienić interwał Zmniejszaj w minutach**

    Set-HpcClusterProperty –ShrinkInterval <interval>

**Aby wyświetlić bieżącą konfigurację AutoGrowShrink**

    Get-HpcClusterProperty –AutoGrowShrink

### <a name="autogrowshrink-parameters"></a>Parametry AutoGrowShrink

Poniżej przedstawiono parametry AutoGrowShrink, które można modyfikować za pomocą polecenia **HpcClusterProperty zestawu** .

* **EnableGrowShrink** - przełącznika, aby włączyć lub wyłączyć właściwość **AutoGrowShrink** .
* **ParamSweepTasksPerCore** - liczbę zadań parametrów w celu usprawnienia jeden. Wartość domyślna to zwiększenia jeden core każdego zadania. 
 
    >[AZURE.NOTE] HPC Pack QFE KB3134307 zmienia **ParamSweepTasksPerCore** **TasksPerResourceUnit**. Jest oparty na typie zasobu zadanie, a może być węzeł, socket lub core.
    
* **GrowThreshold** - progu zadania w kolejce wyzwalać automatyczne wzrostu. Wartością domyślną jest 1, co oznacza, że w przypadku 1 lub więcej zadań w kolejce stanie automatycznej zmiany rozmiaru węzły.
* **GrowInterval** - interwał w minutach, aby wyzwolić automatyczne wzrostu. Domyślny interwał jest 5 minut.
* **ShrinkInterval** - interwał w minutach, aby wyzwolić automatyczne zmniejszanie. Domyślny interwał jest 5 minut. |
* **ShrinkIdleTimes** — liczba ciągły kontroli Zmniejszaj, aby wskazać, że węzły są bezczynne. Wartość domyślna to 3 razy. Na przykład jeśli **ShrinkInterval** jest 5 minut, HPC Pack sprawdza, czy węzeł jest bezczynny co 5 minut. Jeśli węzły są w stanie bezczynności po 3 ciągły sprawdza (15 minut), HPC Pack zmniejsza tego węzła.
* **ExtraNodesGrowRatio** - dodatkową wartość procentową węzłów w celu usprawnienia dla zadań wiadomości przechodzące Interface (MPI). Wartością domyślną jest 1, co oznacza, że HPC Pack rozwoju węzły 1% dla zadań MPI. 
* **GrowByMin** - przełącznika, aby wskazać, czy zasady pomieścić jest oparty na zasoby minimalne wymagane dla zadania. Wartość domyślna to false, co oznacza, że HPC Pack rozwoju węzłów dla zadania w oparciu o maksymalna zasoby wymagane dla zadań.
* **SoaJobGrowThreshold** - progu przychodzących żądań SOA wyzwalać automatyczne Powiększ proces. Wartość domyślna to 50000.  
    
    >[AZURE.NOTE] Ten parametr jest obsługiwana, począwszy od HPC Pack 2012 R2 aktualizacji 3.
    
* **SoaRequestsPerCore** — liczba SOA przychodzących żądań się rozrośnie jeden. Wartość domyślna to 20000.  

    >[AZURE.NOTE] Ten parametr jest obsługiwana, począwszy od HPC Pack 2012 R2 aktualizacji 3.

### <a name="mpi-example"></a>Przykład MPI

Domyślnie HPC Pack rozwoju 1% dodatkowe węzły dla zadań MPI (**ExtraNodesGrowRatio** jest ustawiona na 1). Przyczyny jest MPI może wymagać wiele węzłów, oraz zadania można uruchomić tylko po zakończeniu wszystkich węzłów. Po uruchomieniu węzły Azure od czasu do czasu jeden węzeł może być konieczne więcej czasu, aby rozpocząć od innych powodujące węzłach ze stanu bezczynności podczas oczekiwania dla tego węzła na przygotowanie. Według rosnących dodatkowe węzły, HPC Pack skraca czas oczekiwania tego zasobu i zapisuje potencjalnie kosztów. Aby zwiększyć wartość procentową dodatkowych węzłów na MPI zadań (na przykład 10%), uruchom polecenie podobne do

    Set-HpcClusterProperty -ExtraNodesGrowRatio 10

### <a name="soa-example"></a>Przykład SOA

Domyślnie **SoaJobGrowThreshold** jest ustawiona na 50000 i **SoaRequestsPerCore** jest ustawiona na 200000. Po przesłaniu jedno zadanie SOA żądaniami 70000 będzie jednego zadania w kolejce i przychodzących żądań są 70000. W tym przypadku rozwoju HPC Pack 1 podstawowe zadania w kolejce, a także dla przychodzących żądań wzrost (70000 50000)-podstawowe 20000 = 1, aby ogólnie wzrost 2 rdzenie dla tego zadania SOA.

## <a name="run-the-azureautogrowshrinkps1-script"></a>Uruchamianie skryptu AzureAutoGrowShrink.ps1

### <a name="prerequisites"></a>Wymagania wstępne

* **HPC Pack 2012 R2 aktualizacji 1 lub nowszy klaster** - skrypt **AzureAutoGrowShrink.ps1** jest zainstalowany w folderze Kosz % CCP_HOME %. Głowy węzła może być używany obsługiwanych lokalnie lub maszyn wirtualnych Azure. Zobacz [Konfigurowanie klastrze hybrydowego z dodatkiem Service Pack HPC](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) , aby rozpocząć pracę z lokalnego węzła głównego i węzłów Azure "serii". Zobacz [skrypt wdrożenia HPC Pack IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) szybko wdrożenia klastrze HPC Pack w maszyny wirtualne Azure lub za pomocą [szablonu Azure Szybki Start](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/).

* **Azure programu PowerShell 0.8.12** — obecnie skrypt zależy od tej określonej wersji programu Azure PowerShell. Jeśli korzystasz z nowszej wersji na węzła głównego, może być na starszą [wersję 0.8.12](http://az412849.vo.msecnd.net/downloads03/azure-powershell.0.8.12.msi) , aby uruchomić skrypt Azure programu PowerShell. 

* **Klaster z serii Azure węzły** - skrypt są uruchomione na komputerze klienckim, na którym jest zainstalowany pakiet HPC lub węzła głównego. Jeśli uruchomione na komputerze klienckim, upewnij się, że Ustaw zmienną $env: CCP_SCHEDULER poprawnie, aby wskazywały węzła głównego. Węzły Azure "serii" muszą być już dodani do klastrów, ale mogą być w stanie wdrożony nie.


* **Klaster wdrożony w maszyny wirtualne Azure** - Uruchom skrypt na węzła głównego Głosowa, ponieważ zależy od **HpcIaaSNode.ps1 Rozpocznij** i **Zatrzymaj HpcIaaSNode.ps1** skrypty, które są instalowane. Tych skryptów ponadto wymagają certyfikatu usługi zarządzania Azure lub opublikować plik ustawień (zobacz [Zarządzanie obliczyć węzły w klastrze HPC Pack platformy Azure](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md)). Sprawdź, czy wszystkie węzeł obliczeń maszyny wirtualne należy zostały już dodane do klaster. Mogą być w stanie zatrzymania.

### <a name="syntax"></a>W składni

```
AzureAutoGrowShrink.ps1
[[-NodeTemplates] <String[]>] [[-JobTemplates] <String[]>] [[-NodeType] <String>]
[[-NumOfQueuedJobsPerNodeToGrow] <Int32>]
[[-NumOfQueuedJobsToGrowThreshold] <Int32>]
[[-NumOfActiveQueuedTasksPerNodeToGrow] <Int32>]
[[-NumOfActiveQueuedTasksToGrowThreshold] <Int32>]
[[-NumOfInitialNodesToGrow] <Int32>] [[-GrowCheckIntervalMins] <Int32>]
[[-ShrinkCheckIntervalMins] <Int32>] [[-ShrinkCheckIdleTimes] <Int32>]
[-UseLastConfigurations] [[-ArgFile] <String>] [[-LogFilePrefix] <String>]
[<CommonParameters>]

```
### <a name="parameters"></a>Parametry

 * **NodeTemplates** - nazw szablonów węzeł, aby określić zakres dla węzłów na powiększanie i zmniejszanie. Jeśli nie określono (wartość domyślna to @()), wszystkie węzły w grupie węzeł **AzureNodes** się w określonym zakresie, gdy **Typ węzła** ma wartość AzureNodes, a wszystkie węzły w grupie węzeł **ComputeNodes** się w określonym zakresie, gdy **Typ węzła** ma wartość ComputeNodes.

 * **JobTemplates** - nazwy szablonów zadań, aby określić zakres dla węzłów w celu usprawnienia.

 * **Typ węzła** — typ węzła powiększanie i zmniejszanie. Obsługiwane są następujące wartości:

     * **AzureNodes** — dla węzłów Azure PaaS (serii) w wersji lokalnej lub klaster Azure IaaS.

     * **ComputeNodes** - obliczeń węzła maszyny wirtualne tylko w klastrze Azure IaaS.

* **NumOfQueuedJobsPerNodeToGrow** — liczba kolejce wymagane w celu usprawnienia jeden węzeł.

* **NumOfQueuedJobsToGrowThreshold** - progowa liczba kolejce, aby rozpocząć proces zwiększania.

* **NumOfActiveQueuedTasksPerNodeToGrow** — liczba aktywnych zadań kolejce wymagane w celu usprawnienia jeden węzeł. Jeśli określono **NumOfQueuedJobsPerNodeToGrow** o wartości większej niż 0, ten parametr jest ignorowany.

* **NumOfActiveQueuedTasksToGrowThreshold** - progowa liczba aktywnych zadań kolejce, aby rozpocząć proces zwiększania.

* **NumOfInitialNodesToGrow** - początkowej minimalnej liczby węzłów w celu usprawnienia w przypadku wszystkich węzłów w zakresie **Wdrożony nie** lub **zatrzymania (Deallocated)**.

* **GrowCheckIntervalMins** - interwał w minutach między sprawdza się rozrośnie.

* **ShrinkCheckIntervalMins** - interwał w minutach między testy, aby zmniejszyć.

* **ShrinkCheckIdleTimes** — liczba ciągły zmniejszając testy (oddzielone **ShrinkCheckIntervalMins**) oznaczającą, że węzły są bezczynne.

* **UseLastConfigurations** - poprzednie konfiguracje zapisane w pliku argumentów.

* **ArgFile**- nazwę pliku argument można zapisać i zaktualizować konfiguracji, aby uruchomić skrypt.

* **LogFilePrefix** - Nazwa prefiksu pliku dziennika. Można określić ścieżkę. Domyślnie dziennik jest zapisywany bieżącego katalogu roboczego.

### <a name="example-1"></a>Przykład 1

Poniższy przykład konfiguruje węzły Azure serii są wdrażane za pomocą szablonu AzureNode domyślne, aby powiększanie i zmniejszanie automatycznie. Jeśli wszystkie węzły początkowo są w stanie **Wdrożony nie** , co najmniej 3 węzły są uruchamiane. Jeśli liczba kolejce większa niż 8, skrypt zostanie uruchomiony węzły do momentu ich liczba przekroczy stosunek kolejce do **NumOfQueuedJobsPerNodeToGrow**. Węzeł znajduje się ze stanu bezczynności w 3 razy bezczynne, zatrzymaniu.

```
.\AzureAutoGrowShrink.ps1 -NodeTemplates @('Default AzureNode
 Template') -NodeType AzureNodes -NumOfQueuedJobsPerNodeToGrow 5
 -NumOfQueuedJobsToGrowThreshold 8 -NumOfInitialNodesToGrow 3
 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 3
```

### <a name="example-2"></a>Przykład 2

Poniższy przykład konfiguruje węzeł Azure obliczeń maszyny wirtualne wdrożony za powiększanie i zmniejszanie automatycznie szablonu ComputeNode domyślne.
Zadania skonfigurowane za pomocą domyślnego szablonu zadania zdefiniuj zakres obciążenie pracą w klastrze. Jeśli wszystkie węzły początkowo są wstrzymywane, co najmniej 5 węzły są uruchamiane. Jeśli liczba aktywnych zadań kolejce jest większa niż 15, skrypt zostanie uruchomiony węzły do momentu ich liczba przekroczy stosunek aktywnych zadań kolejce do **NumOfActiveQueuedTasksPerNodeToGrow**. Węzeł znajduje się ze stanu bezczynności w 10 razy bezczynne, zatrzymaniu.

```
.\AzureAutoGrowShrink.ps1 -NodeTemplates 'Default ComputeNode Template' -JobTemplates 'Default' -NodeType ComputeNodes -NumOfActiveQueuedTasksPerNodeToGrow 10 -NumOfActiveQueuedTasksToGrowThreshold 15 -NumOfInitialNodesToGrow 5 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 10 -ArgFile 'IaaSVMComputeNodes_Arg.xml' -LogFilePrefix 'IaaSVMComputeNodes_log'
```
