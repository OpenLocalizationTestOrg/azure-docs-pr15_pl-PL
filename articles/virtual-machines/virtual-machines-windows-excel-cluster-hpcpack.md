<properties
 pageTitle="Klaster HPC Pack dla programu Excel i SOA | Microsoft Azure"
 description="Rozpoczynanie pracy uruchomionych na dużą skalę obciążenia programu Excel i SOA klastrze HPC Pack platformy Azure"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,hpc-pack"/>

<tags
 ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="08/25/2016"
 ms.author="danlep"/>

# <a name="get-started-running-excel-and-soa-workloads-on-an-hpc-pack-cluster-in-azure"></a>Wprowadzenie do uruchamiania programu Excel i SOA obciążenia w klastrze HPC Pack platformy Azure

W tym artykule pokazano, jak wdrożyć klastrze Microsoft HPC Pack w przypadku Azure maszyn wirtualnych przy użyciu szablonu Azure Szybki Start lub opcjonalnie skrypt wdrożenia programu Azure PowerShell. Klaster używa obrazów Azure Marketplace w maszyn wirtualnych, przeznaczonych do uruchamiania programu Microsoft Excel lub obciążenia opartych na usługach architektura (SOA) HPC Pack. Klaster umożliwia uruchamianie prostego HPC programu Excel i usługach SOA z lokalnym komputerze klienckim. Usługi programu Excel HPC obejmują Odciążanie skoroszytu programu Excel i funkcji zdefiniowanych przez użytkownika programu Excel i funkcji zdefiniowanych przez użytkownika.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Na poniższym diagramie przedstawiono na wysokim poziomie, klaster HPC Pack tworzyć.

![Klaster HPC z węzłów z systemem obciążenia programu Excel][scenario]

## <a name="prerequisites"></a>Wymagania wstępne

*   **Komputer kliencki** — należy przesyłać zadania programu Excel i SOA próbki z klastrem komputer kliencki systemu Windows. Ponadto potrzebny dla systemu Windows, aby uruchomić skrypt programu Azure PowerShell klaster wdrożenia (po wybraniu tej metody wdrażania) i

*   **Azure subskrypcji** — Jeśli nie masz subskrypcji usługi Azure, możesz utworzyć [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial/) na kilka minut.

*   **Przydział rdzenie** - może być konieczne zwiększyć przydział rdzeni, zwłaszcza jeśli wdrażanie kilku węzłach z wielordzeniowych maszyn wirtualnych o rozmiarach. Jeśli używasz szablonu Azure Szybki Start, przydział rdzenie w Menedżerze zasobów jest rozbiciu na regiony Azure. W takim przypadku może być konieczne zwiększyć przydział w określonym regionie. Zobacz [limity Azure subskrypcji, przydziały oraz ograniczenia](../azure-subscription-service-limits.md). Aby zwiększyć przydziału, [Otwórz żądanie pomocy technicznej online klienta](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) bezpłatnie.

*   **Licencja pakietu Microsoft Office** — po wdrożeniu węzły obliczeń przy użyciu obrazu maszyn wirtualnych Pack HPC Marketplace w programie Microsoft Excel wersji próbnej 30-dniową programu Microsoft Excel Professional Plus 2013 jest zainstalowany. Po okresu próbnego musisz podać prawidłową licencję Microsoft Office, aby aktywować nadal działać obciążeń pracą w programie Excel. Zobacz [Aktywacja programu Excel](#excel-activation) w dalszej części tego artykułu. 


## <a name="step-1-set-up-an-hpc-pack-cluster-in-azure"></a>Krok 1. Konfigurowanie klastrze HPC Pack platformy Azure

Możemy wyświetlić dwie opcje, aby skonfigurować klaster: pierwszego, przy użyciu szablonu Azure Szybki Start i portal Azure. a druga, za pomocą skryptu wdrażania Azure programu PowerShell.


### <a name="option-1-use-a-quickstart-template"></a>Opcja 1. Korzystanie z szablonu Szybki Start
Szybkie i łatwe wdrażanie klastrze HPC Pack w portalu Azure przy użyciu szablonu Azure Szybki Start. Jeśli szablon zostanie otwarty w portalu, otrzymujesz Interfejsu prostego, gdzie wprowadzić ustawienia dla klaster. Wykonaj poniższe kroki. 

>[AZURE.TIP]Jeśli chcesz, użyj [szablonu Azure Marketplace](https://portal.azure.com/?feature.relex=*%2CHubsExtension#create/microsofthpc.newclusterexcelcn) , który tworzy klaster podobne specjalnie dla obciążenia programu Excel. Kroki się nieco różnić z następujących czynności.

1.  Odwiedź [strony szablonu utworzyć klaster HPC na GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster). Jeśli chcesz, przejrzyj informacje dotyczące szablonu i kod źródłowy.

2.  Kliknij pozycję **Deploy Azure** rozpoczynać wdrożeniu szablon Azure portal.

    ![Wdrażanie szablonu Azure][github]

3.  W portalu wykonaj poniższe czynności, aby wprowadzić parametry szablonu klaster HPC.

    . Na stronie **Parametry** wprowadź lub zmień wartości parametrów szablonu. (Kliknij ikonę obok każdego ustawienia, aby uzyskać pomoc). Wartości przykładowych przedstawiono na poniższym obrazie. W tym przykładzie tworzy klaster o nazwie *hpc01* w domenie *hpc.local* składający się z węzła głównego i 2 obliczyć węzły. Węzły obliczeniowej są tworzone na podstawie obrazu HPC maszyn wirtualnych z dodatkiem Service Pack, który zawiera program Microsoft Excel.

    ![Wprowadź parametry][parameters]

    >[AZURE.NOTE]Węzła głównego maszyn wirtualnych jest tworzony automatycznie z [najnowszych obraz Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) HPC Pack 2012 R2 w systemie Windows Server 2012 R2. Obecnie obrazu zależy od HPC Pack 2012 R2 aktualizacji 3.
    >
    >Maszyny wirtualne węzeł obliczeniowej są tworzone z najnowszych obrazu rodzinie węzeł zaznaczonego obliczeń. Wybierz opcję **ComputeNodeWithExcel** najnowszą HPC Pack obliczeń węzeł obraz, który zawiera wersję próbną programu Microsoft Excel Professional Plus 2013. Aby wdrożyć klastrze ogólne sesje SOA lub Odciążanie UDF programu Excel, wybierz opcję **ComputeNode** (bez zainstalowanego programu Excel).

    b. Wybierz subskrypcję.

    c. Tworzenie grupy zasobów dla klaster, na przykład *hpc01RG*.

    d. Wybierz lokalizację dla grupy zasobów, takich jak centralnej USA.

    e. Na stronie **warunki prawne** zapoznaj się z warunkami. Jeśli akceptujesz, kliknij przycisk **Zakup**. Następnie po zakończeniu ustawiania wartości dla szablonu kliknij przycisk **Utwórz**.

4.  Po zakończeniu rozmieszczania (zwykle trwa około 30 minut), wyeksportować plik certyfikatu klaster z głowy węzła. W ostatnim kroku możesz zaimportować ten certyfikat publicznej na komputerze klienckim do uwierzytelniania po stronie serwera bezpiecznego wiązanie protokołu HTTP.

    . Nawiązywanie połączenia z węzła głównego przez pulpitu zdalnego z portalem Azure.

     ![Nawiązywanie połączenia z węzła głównego][connect]

    b. Aby wyeksportować certyfikat węzła głównego (znajdujące się w obszarze certyfikatu: \LocalMachine\My) bez klucz prywatny, należy użyć standardowe procedury w Menedżerze certyfikatów. W tym przykładzie eksportowanie *CN = hpc01.eastus.cloudapp.azure.com*.

    ![Eksportowanie certyfikatu][cert]

### <a name="option-2-use-the-hpc-pack-iaas-deployment-script"></a>Opcja 2. Skorzystaj z tego skryptu HPC Pack IaaS wdrażania

Skrypt wdrożenia HPC Pack IaaS udostępnia inny sposób uniwersalny wdrażanie klastrze HPC Pack. Tworzy go klaster w modelu Klasyczny wdrożenia, dlatego używane model wdrożenia Azure Menedżera zasobów w szablonie. Ponadto skrypt jest zgodny z subskrypcji w usłudze Azure globalny lub Chiny Azure.

**Dodatkowe wymagania wstępne**

* **Azure programu PowerShell** - [Instalowanie i konfigurowanie programu PowerShell Azure](../powershell-install-configure.md) (wersja 0.8.10 lub nowsza) na komputerze klienckim.

* **Skrypt wdrożenia HPC Pack IaaS** — Pobierz i Rozpakuj najnowszą wersję pakietu skryptu z [Centrum pobierania Microsoft](https://www.microsoft.com/download/details.aspx?id=44949). Sprawdzanie wersji skrypt, uruchamiając `New-HPCIaaSCluster.ps1 –Version`. Ten artykuł jest oparty na wersji 4.5.0 lub nowszej skrypt.

**Tworzenie pliku konfiguracji**

 Skrypt wdrożenia HPC Pack IaaS używa plik konfiguracyjny XML jako danych wejściowych, opisującą infrastruktury klaster HPC. Aby wdrożyć klaster składa się z węzła głównego i 18 węzły obliczeń utworzone na podstawie obrazu węzeł obliczeń, który zawiera program Microsoft Excel, należy zastąpić wartości dla środowiska do przykładowy plik konfiguracyjny. Aby uzyskać więcej informacji o pliku konfiguracji zobacz plik Manual.rtf w folderze skryptu i [Utwórz klaster HPC scenariusz wdrażania HPC Pack IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md).

```
<?xml version="1.0" encoding="utf-8"?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>MySubscription</SubscriptionName>
    <StorageAccount>hpc01</StorageAccount>
  </Subscription>
  <Location>West US</Location>
  <VNet>
    <VNetName>hpc-vnet01</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>HPCExcelDC01</VMName>
      <ServiceName>HPCExcelDC01</ServiceName>
      <VMSize>Medium</VMSize>
    </DomainController>
  </Domain>
   <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>HPCExcelHN01</VMName>
    <ServiceName>HPCExcelHN01</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI/>
    <EnableWebPortal/>
    <PostConfigScript>C:\tests\PostConfig.ps1</PostConfigScript>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>HPCExcelCN%00%</VMNamePattern>
    <ServiceName>HPCExcelCN01</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>18</NodeCount>
    <ImageName>HPCPack2012R2_ComputeNodeWithExcel</ImageName>
  </ComputeNodes>
</IaaSClusterConfig>
```

**Uwagi dotyczące pliku konfiguracji**

* **VMName** głowy węzła **musi** być taka sama jak **NazwaUsługi**lub SOA zadania nie można uruchomić.

* Upewnij się, że możesz określić **EnableWebPortal** , tak aby certyfikat węzła głównego jest generowany i eksportować.

* Plik Określa skrypt programu PowerShell po przeprowadzeniu konfiguracji PostConfig.ps1 uruchamianej na węzła głównego. Następujący przykładowy skrypt konfiguruje parametry połączenia Azure miejsca do magazynowania, usuwa roli węzeł obliczeń węzła głównego i przełącza wszystkie węzły w trybie online, kiedy są one używane. 

```
    # add the HPC Pack powershell cmdlets
        Add-PSSnapin Microsoft.HPC

    # set the Azure storage connection string for the cluster
        Set-HpcClusterProperty -AzureStorageConnectionString 'DefaultEndpointsProtocol=https;AccountName=<yourstorageaccountname>;AccountKey=<yourstorageaccountkey>'

    # remove the compute node role for head node to make sure the Excel workbook won’t run on head node
        Get-HpcNode -GroupName HeadNodes | Set-HpcNodeState -State offline | Set-HpcNode -Role BrokerNode

    # total number of nodes in the deployment including the head node and compute nodes, which should match the number specified in the XML configuration file
        $TotalNumOfNodes = 19

        $ErrorActionPreference = 'SilentlyContinue'

    # bring nodes online when they are deployed until all nodes are online
        while ($true)
        {
          Get-HpcNode -State Offline | Set-HpcNodeState -State Online -Confirm:$false
          $OnlineNodes = @(Get-HpcNode -State Online)
          if ($OnlineNodes.Count -eq $TotalNumOfNodes)
          {
             break
          }
          sleep 60
        }
```

**Uruchamianie skryptu**

1.  Otwórz konsolę programu PowerShell na komputerze klienckim jako administrator.

2.  Zmień katalog folder skryptów (E:\IaaSClusterScript w tym przykładzie).

    ```
    cd E:\IaaSClusterScript
    ```
    
3.  Aby wdrożyć klaster HPC Pack, uruchom następujące polecenie. W tym przykładzie założono, że plik konfiguracyjny znajduje się w E:\HPCDemoConfig.xml.

    ```
    .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
    ```

Skrypt wdrożenia HPC Pack jest uruchamiany przez dłuższy czas. Jeden element, który oznacza skrypt jest eksportowanie i Pobierz certyfikat klaster i zapisz go w folderze dokumenty bieżącego użytkownika na komputerze klienckim. Skrypt generuje komunikat podobny do następującego. Następujące czynności możesz zaimportować certyfikatu w odpowiednim magazynie certyfikatów.    
    
    You have enabled REST API or web portal on HPC Pack head node. Please import the following certificate in the Trusted Root Certification Authorities certificate store on the computer where you are submitting job or accessing the HPC web portal:
    C:\Users\hpcuser\Documents\HPCWebComponent_HPCExcelHN004_20150707162011.cer

## <a name="step-2-offload-excel-workbooks-and-run-udfs-from-an-on-premises-client"></a>Krok 2. Odciążanie skoroszytów programu Excel i uruchamianie funkcji zdefiniowanych przez użytkownika w kliencie lokalnego

### <a name="excel-activation"></a>Aktywacja programu Excel

Używając obrazu ComputeNodeWithExcel maszyn wirtualnych dla obciążenia produkcji, należy podać prawidłowego klucza licencji programu Microsoft Office o aktywowanie programu Excel w węzłach obliczeń. W przeciwnym razie wygaśnięciu wersji próbnej programu Excel po 30 dniach, a systemem skoroszytów programu Excel nie powiedzie się COMException (0x800AC472). 

Program Excel może licencjonowania dla innego 30 dni okresu próbnego: Zaloguj się do węzła głównego i clusrun `%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe` na wszystkich Excel obliczenia węzły za pośrednictwem HPC klaster Manager. Czy licencjonowania maksymalnie dwa razy. Następnie należy podać prawidłowego klucza licencji pakietu Office.

Office Professional Plus 2013 zainstalowana na obraz maszyn wirtualnych jest wersje z licencją zbiorczą z ogólnego głośność licencji klucz (GVLK). Można aktywować za pomocą usługi zarządzania Kluczami / Aktywacja Active Directory-Based (AD BA) lub aktywacji klucza Wielokrotnej. 

    * Za pomocą usługi zarządzania Kluczami-AD-BA, za pomocą istniejącego serwera usługi zarządzania Kluczami lub skonfigurować nową przy użyciu pakietu Microsoft Office 2013 głośność licencji. (Jeśli chcesz, skonfigurować serwer na węzła głównego). Następnie Aktywuj klucz hosta usługi zarządzania Kluczami za pośrednictwem Internetu lub telefonu. Następnie clusrun `ospp.vbs` serwer usługi zarządzania Kluczami oraz portu i aktywowania pakietu Office na wszystkich programu Excel obliczenia węzły. 
    
    * Aby użyć klucza aktywacji Wielokrotnej, pierwszy clusrun `ospp.vbs` należy wprowadzić klucz, a następnie uaktywnić wszystkich programu Excel obliczenia węzły za pośrednictwem Internetu lub telefonu. 

>[AZURE.NOTE]Detalicznej kluczy produktów pakietu Office Professional Plus 2013 nie można używać z tym obrazem maszyn wirtualnych. Jeśli masz klucze i nośnik instalacyjny dla wersji pakietu Office lub program Excel inną niż ta wersja pakietu Office Professional Plus 2013 głośność, możesz ich użyć. Najpierw odinstaluj tego wydania głośności i zainstalować wersję, do której masz. Węzeł ponownie obliczeń programu Excel można przechwycić jako obraz maszyn wirtualnych niestandardowych do użycia w wdrożenia w skali.

### <a name="offload-excel-workbooks"></a>Odciążanie skoroszytów programu Excel

Wykonaj poniższe czynności, aby offload skoroszytu programu Excel, tak aby była uruchamiana w klastrze HPC Pack platformy Azure. Aby to zrobić, musisz mieć programu Excel 2010 lub 2013 już zainstalowany na komputerze klienckim.

1. Użyj jednej z opcji w kroku 1, aby wdrożyć klastrze HPC Pack z programu Excel obliczyć węzeł obrazu. Uzyskaj plik certyfikatu klaster (cer) i klaster nazwa użytkownika i hasło.

2. Na komputerze klienckim Importowanie certyfikatu klaster w obszarze certyfikatu: \CurrentUser\Root.

3. Upewnij się, że jest zainstalowany program Excel. Utwórz plik Excel.exe.config z następujące zagadnienia w tym samym folderze co Excel.exe na komputerze klienckim. Wykonanie tej czynności gwarantuje, że dodatek HPC Pack 2012 R2 Excel COM załadowaniu.

    ```
    <?xml version="1.0"?>
    <configuration>
        <startup useLegacyV2RuntimeActivationPolicy="true">
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.0"/>
        </startup>
    </configuration>
    ```
    
4.  Konfigurowanie klienta, aby przesłać zadania do klastrów HPC Pack. Jedną z opcji jest pełnej [instalacji aktualizacji HPC SP3 2012 R2](http://www.microsoft.com/download/details.aspx?id=49922) pobrać i zainstalować klienta HPC Pack. Możesz też pobrać i zainstalować [Narzędzia klienta HPC Pack 2012 R2 aktualizacji 3](https://www.microsoft.com/download/details.aspx?id=49923) i odpowiednie programu Visual C++ 2010 pakiet redystrybucyjny dla Twojego komputera ([x64](http://www.microsoft.com/download/details.aspx?id=14632), [x86](https://www.microsoft.com/download/details.aspx?id=5555)).

5.  W tym przykładzie używamy przykładowy skoroszyt programu Excel o nazwie ConvertiblePricing_Complete.xlsb. Możesz pobrać ją [tutaj](https://www.microsoft.com/en-us/download/details.aspx?id=2939).

6.  Skopiuj skoroszyt programu Excel do folderu roboczego, takich jak D:\Excel\Run.

7.  Otwórz skoroszyt programu Excel. Na wstążce **opracowanie** kliknij pozycję **Dodatki COM** , a następnie upewnij się, że dodatek COM programu Excel Pack HPC jest załadowany pomyślnie.

    ![Dodatek pakietu HPC programu Excel][addin]

8.  Edytowanie makra języka VBA HPCControlMacros w programie Excel, zmieniając wiersze komentarzy, jak pokazano w poniższej skrypt. Należy zastąpić odpowiednie wartości w środowisku usługi.

    ![Makra programu Excel HPC Pack][macro]

    ```
    'Private Const HPC_ClusterScheduler = "HEADNODE_NAME"
    Private Const HPC_ClusterScheduler = "hpc01.eastus.cloudapp.azure.com"

    'Private Const HPC_NetworkShare = "\\PATH\TO\SHARE\DIRECTORY"
    Private Const HPC_DependFiles = "D:\Excel\Upload\ConvertiblePricing_Complete.xlsb=ConvertiblePricing_Complete.xlsb"

    'HPCExcelClient.Initialize ActiveWorkbook
    HPCExcelClient.Initialize ActiveWorkbook, HPC_DependFiles

    'HPCWorkbookPath = HPC_NetworkShare & Application.PathSeparator & ActiveWorkbook.name
    HPCWorkbookPath = "ConvertiblePricing_Complete.xlsb"

    'HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath
    HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath, UserName:="hpc\azureuser", Password:="<YourPassword>"
```

9.  Skopiuj skoroszyt programu Excel do katalogu przekazywania, takich jak D:\Excel\Upload. Ten katalog jest określona w stałej HPC_DependsFiles makra języka VBA.

10. Aby uruchomić skoroszyt w klastrze platformy Azure, kliknij przycisk **klaster** w arkuszu.

### <a name="run-excel-udfs"></a>Uruchamianie funkcji zdefiniowanych przez użytkownika w programie Excel

Aby uruchomić program Excel funkcji zdefiniowanych przez użytkownika, wykonaj poprzednie kroki 1 – 3, aby skonfigurować komputer kliencki. Dla programu Excel funkcji zdefiniowanych przez użytkownika nie musisz mieć zainstalowany na węzłach do uruchamiania aplikacji Excel. Tak podczas tworzenia klaster obliczyć węzły, można wybrać zwykłym obliczyć węzeł obrazu zamiast obrazu węzeł obliczeń przy użyciu programu Excel.

>[AZURE.NOTE] Istnieje ograniczenie 34 znaków w programie Excel 2010 i 2013 klaster łącznik okno dialogowe. To okno dialogowe umożliwia określ klaster uruchamianej funkcji zdefiniowanych przez użytkownika. Jeśli nazwa pełny klaster jest dłuższa (na przykład hpcexcelhn01.southeastasia.cloudapp.azure.com), nie mieści się w oknie dialogowym. Obejście jest zmienną komputera takich jak *CCP_IAASHN* z wartością nazwy długie klaster. Następnie wprowadź *CCP_IAASHN %* w oknie dialogowym jako nazwę węzła głównego klaster. 

Po pomyślnym wdrożeniu klaster, wykonaj następujące czynności, aby uruchomić przykład wbudowane UDF programu Excel. Aby uzyskać dostosowany przez program Excel Zobacz te [zasoby](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) do tworzenia XLL i wdrażania ich w klastrze IaaS.

1.  Otwórz nowy skoroszyt programu Excel. Na wstążce **opracowanie** kliknij pozycję **Dodatki**. Następnie, w oknie dialogowym kliknij przycisk **Przeglądaj**, przejdź do folderu %CCP_HOME%Bin\XLL32 i wybrać próbkę ClusterUDF32.xll. Jeśli ClusterUDF32 nie istnieje na komputerze klienckim, skopiuj go do folderu %CCP_HOME%Bin\XLL32 na węzła głównego.

    ![Wybierz pozycję UDF][udf]

2.  Kliknij pozycję **plik** > **Opcje** > **Zaawansowane**. W obszarze **formuły**zaznacz **Zezwalaj zdefiniowanych przez użytkownika funkcji XLL do uruchamiania w klastrze**. Następnie kliknij pozycję **Opcje** i wprowadź nazwę pełny klaster w polu **Nazwa węzła głównego klaster**. (Jako uwagę na wspomniane wcześniej to pole wprowadzania jest ograniczone do 34 znaków tak, aby nazwy długie klaster nie może być widoczne. Można użyć zmiennej komputera poniżej nazwy długie klaster.)

    ![Konfigurowanie UDF][options]

3.  Aby uruchomić obliczenie UDF w klastrze, kliknij komórkę zawierającą wartość =XllGetComputerNameC(), a następnie naciśnij klawisz Enter. Funkcja po prostu pobiera nazwę węzła obliczeń, na którym jest uruchomiona UDF. Dla przy pierwszym uruchomieniu okno dialogowe poświadczeń monituje o podanie nazwy użytkownika i hasła do nawiązania połączenia z klastrem IaaS.

    ![Uruchamianie UDF][run]

    W przypadku wielu komórek w celu obliczenia naciśnij klawisze Alt-Shift-Ctrl + F9, aby uruchomić obliczenia na wszystkich komórek.

## <a name="step-3-run-a-soa-workload-from-an-on-premises-client"></a>Krok 3. Uruchamianie SOA obciążenie pracą z klientem lokalnego

Aby uruchomić ogólne aplikacji SOA w klastrze HPC Pack IaaS, najpierw za pomocą jednej z metod w kroku 1 wdrożenia klaster. Określenie rodzajowy obliczyć węzeł obrazu, w tym przypadku, ponieważ program Excel nie ma potrzeby w węzłach obliczeń. Następnie wykonaj poniższe czynności.

1. Po pobraniu certyfikatu klaster, zaimportuj go na komputerze klienckim, w obszarze certyfikatu: \CurrentUser\Root.

2. Zainstaluj [HPC Pack 2012 R2 aktualizacji 3 SDK](http://www.microsoft.com/download/details.aspx?id=49921) i [narzędzi klienta HPC Pack 2012 R2 aktualizacji 3](https://www.microsoft.com/download/details.aspx?id=49923). Te narzędzia umożliwiają tworzenie i uruchamianie aplikacji klienckich SOA.

3. Pobierz HelloWorldR2 [Przykładowy kod](https://www.microsoft.com/download/details.aspx?id=41633). Otwórz HelloWorldR2.sln w Visual Studio 2010 lub 2012.

4. Tworzenie projektu EchoService najpierw. Następnie wdrożyć usługę klaster IaaS w taki sam sposób, który Wdroż klaster lokalnego. Aby uzyskać szczegółowe instrukcje Zobacz Readme.doc w HelloWordR2. Modyfikowanie i tworzenie HellWorldR2 i innych projektów, zgodnie z opisem w poniższej sekcji, aby wygenerować aplikacje klienckie SOA, które działają w klastrze Azure IaaS.

### <a name="use-http-binding-with-azure-storage-queue"></a>Wiązanie protokołu Http za pomocą kolejki Azure miejsca do magazynowania

Aby użyć protokołu Http powiązania z kolejkę Azure miejsca do magazynowania, wprowadź zmiany kilka przykładowy kod.

* Aktualizacja nazwy klaster.

    ```
// Before
const string headnode = "[headnode]";
// After e.g.
const string headnode = "hpc01.eastus.cloudapp.azure.com";
or
const string headnode = "hpc01.cloudapp.net";
```

* Opcjonalnie można użyć domyślnego TransportScheme w SessionStartInfo lub jawnie ustawione protokołu HTTP.

```
    info.TransportScheme = TransportScheme.Http;
```

* Użyj domyślnych powiązanie dla BrokerClient.

    ```
// Before
using (BrokerClient<IService1> client = new BrokerClient<IService1>(session, binding))
// After
using (BrokerClient<IService1> client = new BrokerClient<IService1>(session))
```

    Lub zestaw jawnie przy użyciu klasy basicHttpBinding.

    ```
BasicHttpBinding binding = new BasicHttpBinding(BasicHttpSecurityMode.TransportWithMessageCredential);
binding.Security.Message.ClientCredentialType = BasicHttpMessageCredentialType.UserName;    binding.Security.Transport.ClientCredentialType = HttpClientCredentialType.None;
```

* Opcjonalnie możesz ustawić flagę UseAzureQueue na Prawda w SessionStartInfo. Jeśli nie ustawiono go jest równa PRAWDA domyślnie nazwę klaster sufiksy domeny Azure i TransportScheme jest Http.

    ```
    info.UseAzureQueue = true;
```

###<a name="use-http-binding-without-azure-storage-queue"></a>Użyj protokołu Http powiązanie bez kolejki Azure miejsca do magazynowania

Aby użyć wiązanie protokołu Http bez kolejkę Azure miejsca do magazynowania, ustawić flagę UseAzureQueue ma wartość FAŁSZ w SessionStartInfo.

```
    info.UseAzureQueue = false;
```

### <a name="use-nettcp-binding"></a>Używanie powiązanie NetTcp

Aby użyć powiązanie NetTcp, konfiguracja jest podobne do łączenia się z klastrem w lokalnym. Musisz otworzyć kilka punkty końcowe węzła głównego maszyn wirtualnych. Jeśli skrypt wdrożenia HPC Pack IaaS umożliwia tworzenie klaster, na przykład skonfigurować punkty końcowe w portalu klasyczny Azure w następujący sposób.


1. Zatrzymywanie maszyn wirtualnych.

2. Dodaj porty TCP 9090, 9087, 9091, 9094 dla tej sesji, pośrednik, brokera pracownika i usług danych odpowiednio

    ![Konfigurowanie punktów końcowych][endpoint]

3. Rozpocznij maszyn wirtualnych.

Z aplikacją kliencką SOA wymaga żadnych zmian, z wyjątkiem zmiany głowy nazwę, aby IaaS klaster imię i nazwisko.

## <a name="next-steps"></a>Następne kroki

* Zobacz [następujące zasoby](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) , aby uzyskać więcej informacji o uruchamianiu obciążenia programu Excel z dodatkiem Service Pack HPC.

* Aby uzyskać więcej informacji o wdrażaniu i zarządzanie usługami SOA pakietem HPC, zobacz [Zarządzanie usługami SOA w Microsoft HPC Pack](https://technet.microsoft.com/library/ff919412.aspx) .

<!--Image references-->
[scenario]: ./media/virtual-machines-windows-excel-cluster-hpcpack/scenario.png
[github]: ./media/virtual-machines-windows-excel-cluster-hpcpack/github.png
[template]: ./media/virtual-machines-windows-excel-cluster-hpcpack/template.png
[parameters]: ./media/virtual-machines-windows-excel-cluster-hpcpack/parameters.png
[create]: ./media/virtual-machines-windows-excel-cluster-hpcpack/create.png
[connect]: ./media/virtual-machines-windows-excel-cluster-hpcpack/connect.png
[cert]: ./media/virtual-machines-windows-excel-cluster-hpcpack/cert.png
[addin]: ./media/virtual-machines-windows-excel-cluster-hpcpack/addin.png
[macro]: ./media/virtual-machines-windows-excel-cluster-hpcpack/macro.png
[options]: ./media/virtual-machines-windows-excel-cluster-hpcpack/options.png
[run]: ./media/virtual-machines-windows-excel-cluster-hpcpack/run.png
[endpoint]: ./media/virtual-machines-windows-excel-cluster-hpcpack/endpoint.png
[udf]: ./media/virtual-machines-windows-excel-cluster-hpcpack/udf.png
