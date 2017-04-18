<properties
   pageTitle="Wprowadzenie do programu PowerShell wsadowe Azure | Microsoft Azure"
   description="Krótkie wprowadzenie do poleceń cmdlet programu PowerShell Azure, służącą do zarządzania usługą Azure partii"
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="powershell"
   ms.workload="big-compute"
   ms.date="10/20/2016"
   ms.author="marsma"/>

# <a name="get-started-with-azure-batch-powershell-cmdlets"></a>Wprowadzenie do poleceń cmdlet programu PowerShell partii Azure
Przy użyciu poleceń cmdlet programu PowerShell partii Azure można wykonywać i skryptów wielu zadań, które są wykonywane za pośrednictwem interfejsów API partii, Azure portal i interfejs wiersza polecenia Azure (polecenie). Oto krótkie wprowadzenie do poleceń cmdlet, który umożliwia zarządzanie konta partii i Praca z zasobami partii takie jak pul, zadania i zadania.

Aby uzyskać pełną listę partii apletów poleceń i składni szczegółowe polecenia cmdlet zobacz [informacje dotyczące poleceń cmdlet partii Azure](https://msdn.microsoft.com/library/azure/mt125957.aspx).

Ten artykuł jest oparty na poleceń cmdlet w programie PowerShell Azure wersji 3.0.0. Firma Microsoft zaleca aktualizacji usługi Azure programu PowerShell często, aby korzystać z usługi Aktualizacje i rozszerzenia.

## <a name="prerequisites"></a>Wymagania wstępne

Wykonać następujące czynności, aby użyć Azure programu PowerShell do zarządzania zasobami partię.

* [Instalowanie i konfigurowanie programu PowerShell Azure](../powershell-install-configure.md)

* Uruchom polecenie cmdlet **AzureRmAccount logowania** , aby nawiązać połączenie subskrypcji (Azure partii wysyłki poleceń cmdlet w module Menedżera zasobów Azure):

    `Login-AzureRmAccount`

* **Zarejestruj z nazw dostawcy partii**. Operacja tylko musi być wykonywana **raz na subskrypcję**.

    `Register-AzureRMResourceProvider -ProviderNamespace Microsoft.Batch`

## <a name="manage-batch-accounts-and-keys"></a>Zarządzanie kontami partii i klawiszy

### <a name="create-a-batch-account"></a>Tworzenie konta partii

**Nowy AzureRmBatchAccount** tworzy konto partii w grupie określonego zasobu. Jeśli nie masz jeszcze grupa zasobów, utworzyć za pomocą polecenia cmdlet [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/azure/mt603739.aspx) . Określ jeden z regionów Azure w parametrze **lokalizacji** , takie jak "Centralna USA". Na przykład:

    New-AzureRmResourceGroup –Name MyBatchResourceGroup –location "Central US"

Następnie utwórz konto partii w grupie zasobów, określając nazwę konta w <*nazwa_konta*> i lokalizację i nazwę grupy zasobów. Tworzenie konta partii może zająć trochę czasu, aby zakończyć. Na przykład:

    New-AzureRmBatchAccount –AccountName <account_name> –Location "Central US" –ResourceGroupName <res_group_name>

> [AZURE.NOTE] Nazwa konta partii musi być unikatowa w usłudze Azure regionu grupa zasobów, zawierać od 3 do 24 znaków, a następnie za pomocą wielkich liter i cyfr.

### <a name="get-account-access-keys"></a>Uzyskiwanie konta klawiszy dostępu
**Get-AzureRmBatchAccountKeys** zawiera skojarzone z kontem partii Azure klawiszy dostępu. Na przykład uruchom następujące czynności, aby uzyskać kluczy podstawowych i pomocniczych utworzone konto.

    $Account = Get-AzureRmBatchAccountKeys –AccountName <account_name>

    $Account.PrimaryAccountKey

    $Account.SecondaryAccountKey

### <a name="generate-a-new-access-key"></a>Generowanie nowego klucza dostępu
**Nowy AzureRmBatchAccountKey** generuje nowy klucz podstawowy i pomocniczy konta dla konta partii Azure. Na przykład aby wygenerować nowego klucza podstawowego dla Twojego konta partii, należy wpisać:

    New-AzureRmBatchAccountKey -AccountName <account_name> -KeyType Primary

> [AZURE.NOTE] Aby wygenerować nowy klucz pomocniczym, określ "Pomocniczej" parametru **KeyType** . Musisz ponownie wygenerować klucze głównego i pomocniczego oddzielnie.

### <a name="delete-a-batch-account"></a>Usuwanie konta partii
**Usuń AzureRmBatchAccount** Usuwa konto partię. Na przykład:

    Remove-AzureRmBatchAccount -AccountName <account_name>

Gdy zostanie wyświetlony monit, upewnij się, że chcesz usunąć konto. Usuwanie konta może zająć trochę czasu, aby zakończyć.

## <a name="create-a-batchaccountcontext-object"></a>Tworzenie obiektu BatchAccountContext

Uwierzytelnianie za pomocą polecenia cmdlet programu PowerShell partii podczas tworzenia i zarządzania nimi pul partię, zadania, zadania i inne zasoby, najpierw należy utworzyć obiektu BatchAccountContext do przechowywania nazwę konta i kluczy:

    $context = Get-AzureRmBatchAccountKeys -AccountName <account_name>

Obiekt BatchAccountContext należy przekazać do poleceń cmdlet należy użyć parametru **BatchContext** .

> [AZURE.NOTE] Domyślnie konta klucza podstawowego jest używany do uwierzytelniania, ale możesz wybrać jawnie klucz do użycia, zmieniając właściwość **KeyInUse** obiektu BatchAccountContext: `$context.KeyInUse = "Secondary"`.

## <a name="create-and-modify-batch-resources"></a>Tworzenie i modyfikowanie partii zasobów
Umożliwia utworzenie zasobów na koncie partii apletów poleceń, takich jak **AzureBatchPool nowy**, **Nowa AzureBatchJob**i **AzureBatchTask nowy** . Są to odpowiadające **Get -** i polecenia cmdlet **Set-** , aby zaktualizować właściwości istniejących zasobów i **Usuń-** poleceń cmdlet do usunięcia zasobów na koncie partię.

Podczas korzystania z wiele z tych poleceń cmdlet, oprócz przekazywania obiektu BatchContext, musisz utworzyć lub przekazać obiektów, które zawierają ustawienia szczegółowe zasobów, jak pokazano w poniższym przykładzie. Uzyskać szczegółową pomoc dla każdego polecenia cmdlet, aby uzyskać dodatkowe przykłady.

### <a name="create-a-batch-pool"></a>Tworzenie puli wsadowe

Tworzenie lub aktualizowanie puli partię, możesz wybrać Konfiguracja usługi cloud lub konfigurację maszyn wirtualnych systemu operacyjnego w węzłach obliczeń (zobacz [Omówienie funkcji wsadowe](batch-api-basics.md#pool)). Wybór określa, czy węzły obliczeń są obrazami z jednym z [wersjach systemu operacyjnego gościa Azure](../cloud-services/cloud-services-guestos-update-matrix.md#releases) lub jednego z obsługiwanych obrazy Linux lub maszyn wirtualnych systemu Windows Azure Marketplace.

Po uruchomieniu **Nowy AzureBatchPool**przekazać ustawienia systemu operacyjnego obiektu PSCloudServiceConfiguration lub PSVirtualMachineConfiguration. Na przykład następujące polecenie cmdlet tworzy nową pulę partii węzłów małych obliczeniowej rozmiaru w chmurze konfiguracji obrazami z najnowszą wersją systemu operacyjnego rodziny 3 (Windows Server 2012). W tym miejscu parametr **CloudServiceConfiguration** Określa zmienną *$configuration* jako obiekt PSCloudServiceConfiguration. Parametr **BatchContext** określa wcześniej zdefiniowanych zmiennych *$context* jako obiekt BatchAccountContext.

    $configuration = New-Object -TypeName "Microsoft.Azure.Commands.Batch.Models.PSCloudServiceConfiguration" -ArgumentList @(4,"*")

    New-AzureBatchPool -Id "AutoScalePool" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -AutoScaleFormula '$TargetDedicated=4;' -BatchContext $context

Docelowej liczby węzłów obliczeń w nowej puli jest określona przez formułę autoscaling. W tym przypadku formuła jest po prostu **$TargetDedicated = 4**, wskazująca liczbę węzłów obliczeń w puli to co najwyżej 4.

## <a name="query-for-pools-jobs-tasks-and-other-details"></a>Kwerenda dotycząca pul, zadania, zadania i inne szczegóły

Użyj polecenia cmdlet, takich jak **Get AzureBatchPool**, **Get-AzureBatchJob**i **Get-AzureBatchTask** do kwerendy dla obiektów utworzonych przy użyciu konta partię.

### <a name="query-for-data"></a>Kwerenda danych

Na przykład umożliwia znajdowanie swojej pul **Get-AzureBatchPools** . Domyślnie ten kwerend dla wszystkich pul na koncie, wykonując już przechowywany obiekt BatchAccountContext w *$context*:

    Get-AzureBatchPool -BatchContext $context

### <a name="use-an-odata-filter"></a>Używanie filtru OData

Można zastosować filtr OData używanie parametr **filtru** w celu znalezienia obiektów, który Cię interesuje. Na przykład możesz znaleźć wszystkie pule identyfikatorami, zaczynając od "myPool":

    $filter = "startswith(id,'myPool')"

    Get-AzureBatchPool -Filter $filter -BatchContext $context

Ta metoda nie jest tak elastyczne, jak za pomocą "Where-Object" w lokalnym potoku. Jednak kwerendy przesłane do usługi partii bezpośrednio tak, aby wszystkie filtry się dzieje po stronie serwera, zapisując przepustowości internetowej.

### <a name="use-the-id-parameter"></a>Używanie parametru ID.

Innym sposobem filtr OData należy użyć parametru **Id** . Aby wykonać kwerendę dotyczącą dla określonych puli przy użyciu identyfikatora "myPool":

    Get-AzureBatchPool -Id "myPool" -BatchContext $context

Parametr **identyfikatora** obsługuje tylko wyszukiwania identyfikatora pełnej, nie symboli wieloznacznych lub filtry OData styl.

### <a name="use-the-maxcount-parameter"></a>Parametr MaxCount

Domyślnie każdy polecenia cmdlet zwraca maksymalnie 1000 obiektów. Przekroczenie limitu Uściślanie filtr, Przesuń do tyłu mniej innych obiektów lub ustawić maksymalnie przy użyciu parametru **MaxCount** . Na przykład:

    Get-AzureBatchTask -MaxCount 2500 -BatchContext $context

Aby usunąć granica górna, ustaw **MaxCount** , 0 lub mniej.

### <a name="use-the-powershell-pipeline"></a>Używanie potoku programu PowerShell

Partii apletów poleceń można wykorzystać proces programu PowerShell do wysyłania danych między poleceń cmdlet. Działa tak samo jak określenie parametru, ale ułatwia pracę z wiele jednostek.

Na przykład znaleźć i wyświetlić wszystkie zadania w obszarze konta:

    Get-AzureBatchJob -BatchContext $context | Get-AzureBatchTask -BatchContext $context

Uruchom ponownie (Uruchom ponownie komputer) każdy węzeł obliczeń w puli:

    Get-AzureBatchComputeNode -PoolId "myPool" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

## <a name="application-package-management"></a>Zarządzanie aplikacjami pakietu

Pakiety aplikacji umożliwiają uproszczone wdrażanie aplikacji węzły obliczeń w puli usługi. Przy użyciu poleceń cmdlet programu PowerShell wsadowe Przekaż i zarządzanie pakietów aplikacji na swoim koncie partię, a wdrożyć wersje pakietu do obliczenia węzły.

**Tworzenie** aplikacji:

    New-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

**Dodawanie** pakietu aplikacji:

    New-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0" -Format zip -FilePath package001.zip

Ustawianie **domyślnej wersji** aplikacji:

    Set-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -DefaultVersion "1.0"

**Lista** pakietów aplikacji

    $application = Get-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

    $application.ApplicationPackages

**Usuwanie** pakietu aplikacji

    Remove-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0"

**Usuwanie** aplikacji

    Remove-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

>[AZURE.NOTE] Należy usunąć wszystkie wersje pakietu aplikacji aplikacji przed usunięciem aplikacji. Otrzymasz komunikat o błędzie "Konflikt", jeśli próby usunięcia aplikacji, która ma obecnie pakietów aplikacji.

### <a name="deploy-an-application-package"></a>Wdrażanie pakietu aplikacji

Po utworzeniu puli, można określić jeden lub więcej pakietów aplikacji do wdrożenia. Po określeniu pakiet w czasie tworzenia puli jako pula sprzężenia węzeł zostanie wdrożony w każdym węźle. Pakiety również są wdrażane, gdy węzeł zostanie ponownie uruchomić lub reimaged.

Określanie `-ApplicationPackageReference` opcji po utworzeniu puli wdrożenia pakietu aplikacji w puli węzły podczas dołączania puli. Najpierw należy utworzyć obiekt **PSApplicationPackageReference** i jest skonfigurowana przy użyciu aplikacji identyfikator oraz pakiet wersję, którą chcesz wdrożyć węzły obliczeń w puli:

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "1.0"

Utwórz teraz puli i określ obiekt odwołanie pakietu jako argumentu `ApplicationPackageReferences` opcję:

    New-AzureBatchPool -Id "PoolWithAppPackage" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -BatchContext $context -ApplicationPackageReferences $appPackageReference

Więcej informacji na temat pakietów aplikacji można znaleźć w [wdrażaniem aplikacji za pomocą pakietów aplikacji partii Azure](batch-application-packages.md).

>[AZURE.IMPORTANT] Należy [łącze konta magazynu platformy Azure](#linked-storage-account-autostorage) do swojego konta partii, aby skorzystać z pakietów aplikacji.

### <a name="update-a-pools-application-packages"></a>Aktualizowanie puli pakietów aplikacji

Aby zaktualizować aplikacje przypisane do istniejącej puli, najpierw należy utworzyć obiektu PSApplicationPackageReference z odpowiednimi właściwościami (wersja identyfikator oraz pakiet aplikacji):

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "2.0"

Następnie uzyskanie puli partii, czyszczenie wszelkie istniejące pakiety Dodawanie naszych odwołania do nowego pakietu i usługi partii aktualizacji z nowymi ustawieniami puli:

    $pool = Get-AzureBatchPool -BatchContext $context -Id "PoolWithAppPackage"

    $pool.ApplicationPackageReferences.Clear()

    $pool.ApplicationPackageReferences.Add($appPackageReference)

    Set-AzureBatchPool -BatchContext $context -Pool $pool

Teraz zostały zaktualizowane właściwości puli w usłudze partię. Aby rzeczywiście wdrożyć pakiet aplikacji do obliczenia węzły w puli, jednak należy ponownie uruchomić lub reimage tych węzłów. Można uruchomić ponownie każdy węzeł w puli przy użyciu tego polecenia:

    Get-AzureBatchComputeNode -PoolId "PoolWithAppPackage" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

>[AZURE.TIP] Węzły obliczeń w puli, należy wdrożyć wielu pakietów aplikacji. Jeśli chcesz *dodać* pakietu aplikacji zamiast zamieniania obecnie wdrożonej pakietów, pominąć `$pool.ApplicationPackageReferences.Clear()` wiersza powyżej.

## <a name="next-steps"></a>Następne kroki

* Polecenie cmdlet szczegółowe składnię i przykłady zobacz [informacje dotyczące poleceń cmdlet partii Azure](https://msdn.microsoft.com/library/azure/mt125957.aspx).

* Aby uzyskać więcej informacji na temat aplikacji i pakietów aplikacji w partii zobacz [wdrażaniem aplikacji za pomocą pakietów aplikacji partii Azure](batch-application-packages.md).
