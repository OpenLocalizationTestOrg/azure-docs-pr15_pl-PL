<properties 
   pageTitle="Zarządzanie analiz Lake danych Azure za pomocą programu PowerShell Azure | Azure" 
   description="Dowiedz się, jak zarządzać zadaniami danych Lake analizy, źródeł danych, użytkownicy. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="manage-azure-data-lake-analytics-using-azure-powershell"></a>Zarządzanie analiz Lake danych Azure za pomocą programu PowerShell Azure

[AZURE.INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Dowiedz się, jak zarządzać kontami Azure danych Lake analizy, źródeł danych użytkowników i zadań przy użyciu programu PowerShell Azure. Aby wyświetlić tematy dotyczące zarządzania przy użyciu innych narzędzi, kliknij pozycję Wybierz kartę powyżej.

**Wymagania wstępne**

Przed rozpoczęciem tego samouczka, musisz mieć następujące czynności:

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/pricing/free-trial/).


<!-- ################################ -->
<!-- ################################ -->


##<a name="install-azure-powershell-10-or-greater"></a>Instalowanie Azure programu PowerShell wersji 1.0 lub nowszej

Zobacz sekcję wstępne [Za pomocą programu Azure przy użyciu Menedżera zasobów Azure](powershell-azure-resource-manager.md#prerequisites).
    
## <a name="manage-accounts"></a>Zarządzanie kontami

Przed uruchomieniem zadania analizy Lake danych, musisz mieć konto analizy Lake danych. W odróżnieniu od Azure HDInsight nie zapłacić dla konta analizy zadanie nie jest uruchomiony.  Płacisz tylko raz, gdy działa zadanie.  Aby uzyskać więcej informacji zobacz [Omówienie analizy Lake danych Azure](data-lake-analytics-overview.md).  

###<a name="create-accounts"></a>Tworzenie kont

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeStoreName = "<DataLakeAccountName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $location = "<Microsoft Data Center>"
    
    Write-Host "Create a resource group ..." -ForegroundColor Green
    New-AzureRmResourceGroup `
        -Name  $resourceGroupName `
        -Location $location
    
    Write-Host "Create a Data Lake account ..."  -ForegroundColor Green
    New-AzureRmDataLakeStoreAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $dataLakeStoreName `
        -Location $location 
    
    Write-Host "Create a Data Lake Analytics account ..."  -ForegroundColor Green
    New-AzureRmDataLakeAnalyticsAccount `
        -Name $dataLakeAnalyticsAccountName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -DefaultDataLake $dataLakeStoreName
    
    Write-Host "The newly created Data Lake Analytics account ..."  -ForegroundColor Green
    Get-AzureRmDataLakeAnalyticsAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $dataLakeAnalyticsAccountName  

Mogą również używać szablonu Azure grupa zasobów. Szablon do tworzenia konto analizy Lake danych oraz zależne magazynu Lake danych znajduje się w [dodatku A](#appendix-a). Zapisz szablon do pliku za pomocą szablonu .json, a następnie nadaj jej, skorzystaj z tego skryptu programu PowerShell:


    $AzureSubscriptionID = "<Your Azure Subscription ID>"
    
    $ResourceGroupName = "<New Azure Resource Group Name>"
    $Location = "EAST US 2"
    $DefaultDataLakeStoreAccountName = "<New Data Lake Store Account Name>"
    $DataLakeAnalyticsAccountName = "<New Data Lake Analytics Account Name>"
    
    $DeploymentName = "MyDataLakeAnalyticsDeployment"
    $ARMTemplateFile = "E:\Tutorials\ADL\ARMTemplate\azuredeploy.json"  # update the Json template path 
    
    Login-AzureRmAccount
    
    Select-AzureRmSubscription -SubscriptionId $AzureSubscriptionID
    
    # Create the resource group
    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location
    
    # Create the Data Lake Analytics account with the default Data Lake Store account.
    $parameters = @{"adlAnalyticsName"=$DataLakeAnalyticsAccountName; "adlStoreName"=$DefaultDataLakeStoreAccountName}
    New-AzureRmResourceGroupDeployment -Name $DeploymentName -ResourceGroupName $ResourceGroupName -TemplateFile $ARMTemplateFile -TemplateParameterObject $parameters 

 
###<a name="list-account"></a>Lista konta

Kont analizy Lake danych listy w obrębie bieżącej subskrypcji

    Get-AzureRmDataLakeAnalyticsAccount
    
Wynik:

    Id         : /subscriptions/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/resourceGroups/learn1021rg/providers/Microsoft.DataLakeAnalytics/accounts/learn1021adla
    Location   : eastus2
    Name       : learn1021adla
    Properties : Microsoft.Azure.Management.DataLake.Analytics.Models.DataLakeAnalyticsAccountProperties
    Tags       : {}
    Type       : Microsoft.DataLakeAnalytics/accounts

Kont analizy Lake danych listy w obrębie danej grupy zasobów

    Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName

Wyświetlanie szczegółów określonego konta analizy Lake danych

    Get-AzureRmDataLakeAnalyticsAccount -Name $adlAnalyticsAccountName

Testowanie istnienia określonego konta analizy Lake danych

    Test-AzureRmDataLakeAnalyticsAccount -Name $adlAnalyticsAccountName

Polecenia cmdlet może zwrócić **wartość PRAWDA** lub **FAŁSZ**.

###<a name="delete-data-lake-analytics-accounts"></a>Usuwanie kont analizy Lake danych

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    
    Remove-AzureRmDataLakeAnalyticsAccount -Name $dataLakeAnalyticsAccountName 

Usuń dane Lake analizy konta nie spowoduje usunięcia zależne konta magazynowanie Lake danych. Poniższy przykład usuwa konto analizy Lake danych oraz domyślnego magazynu Lake danych

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticAccountName).Properties.DefaultDataLakeAccount

    Remove-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticAccountName 
    Remove-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName

<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-account-data-sources"></a>Zarządzanie źródłami danych konta

Analizy Lake danych obsługuje obecnie następujących źródeł danych:

- [Magazyn Lake danych Azure](../data-lake-store/data-lake-store-overview.md)
- [Azure miejsca do magazynowania](storage-introduction.md)

Po utworzeniu konta analizy należy wyznaczyć konto Azure masowej Lake był domyślnym kontem miejsca do magazynowania. Domyślne konto magazynu Lake danych służy do przechowywania dzienników inspekcji metadanych i zlecenia zadania. Po utworzeniu konta analizy, możesz dodać kolejne konta magazynowanie Lake danych i/lub konto Azure miejsca do magazynowania. 

### <a name="find-the-default-data-lake-store-account"></a>Znajdowanie domyślnego konta magazynu Lake danych

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticAccountName).Properties.DefaultDataLakeAccount


### <a name="add-additional-azure-blob-storage-accounts"></a>Dodawanie kont magazyn obiektów Blob platformy Azure

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $AzureStorageAccountName = "<AzureStorageAccountName>"
    $AzureStorageAccountKey = "<AzureStorageAccountKey>"
    
    Add-AzureRmDataLakeAnalyticsDataSource -ResourceGroupName $resourceGroupName -Account $dataLakeAnalyticAccountName -AzureBlob $AzureStorageAccountName -AccessKey $AzureStorageAccountKey

### <a name="add-additional-data-lake-store-accounts"></a>Dodać kolejne konta magazynu Lake danych

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $AzureDataLakeName = "<DataLakeStoreName>"
    
    Add-AzureRmDataLakeAnalyticsDataSource -ResourceGroupName $resourceGroupName -Account $dataLakeAnalyticAccountName -DataLake $AzureDataLakeName 

### <a name="list-data-sources"></a>Lista źródeł danych:

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"

    (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticAccountName).Properties.DataLakeStoreAccounts
    (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticAccountName).Properties.StorageAccounts
    


<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-jobs"></a>Zarządzanie zadaniami

Aby można było utworzyć zadanie, musisz mieć konto analizy Lake danych.  Aby uzyskać więcej informacji zobacz [Zarządzanie danymi analizy Lake konta](#manage-data-lake-analytics-accounts).

### <a name="list-jobs"></a>Wyświetlanie listy zadań

    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName
    
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName -State Running, Queued
    #States: Accepted, Compiling, Ended, New, Paused, Queued, Running, Scheduling, Starting
    
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName -Result Cancelled
    #Results: Cancelled, Failed, None, Successed 
    
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName -Name <Job Name>
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName -Submitter <Job submitter>

    # List all jobs submitted on January 1 (local time)
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -SubmittedAfter "2015/01/01"
        -SubmittedBefore "2015/01/02"   

    # List all jobs that succeeded on January 1 after 2 pm (UTC time)
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -State Ended
        -Result Succeeded
        -SubmittedAfter "2015/01/01 2:00 PM -0"
        -SubmittedBefore "2015/01/02 -0"

    # List all jobs submitted in the past hour
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -SubmittedAfter (Get-Date).AddHours(-1)

### <a name="get-job-details"></a>Wyświetlanie szczegółów zadania

    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName -JobID <Job ID>
    
### <a name="submit-jobs"></a>Przesyłanie zadań

    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"

    #Pass script via path
    Submit-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -Name $jobName `
        -ScriptPath $scriptPath

    #Pass script contents
    Submit-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -Name $jobName `
        -Script $scriptContents

> [AZURE.NOTE] Domyślny priorytet zadania wynosi 1000, a domyślne stopień równoległego wykonywania zadania jest 1.


### <a name="cancel-jobs"></a>Anulowanie zadań

    Stop-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -JobID $jobID


## <a name="manage-catalog-items"></a>Zarządzanie elementami wykazu

Wykaz U SQL służy do struktury danych i kodu, może być udostępniane przez skrypty U-SQL. Wykaz umożliwia najwyższą wydajność możliwe z danymi w Lake danych Azure. Aby uzyskać więcej informacji zobacz [Używanie U-SQL wykazu](data-lake-analytics-use-u-sql-catalog.md).

###<a name="list-catalog-items"></a>Lista elementów wykazu

    #List databases
    Get-AzureRmDataLakeAnalyticsCatalogItem `
        -Account $adlAnalyticsAccountName `
        -ItemType Database
    
    
    
    #List tables
    Get-AzureRmDataLakeAnalyticsCatalogItem `
        -Account $adlAnalyticsAccountName `
        -ItemType Table `
        -Path "master.dbo"

###<a name="get-catalog-item-details"></a>Wyświetlanie szczegółów elementu wykazu 

    #Get a database
    Get-AzureRmDataLakeAnalyticsCatalogItem `
        -Account $adlAnalyticsAccountName `
        -ItemType Database `
        -Path "master"
    
    #Get a table
    Get-AzureRmDataLakeAnalyticsCatalogItem `
        -Account $adlAnalyticsAccountName `
        -ItemType Table `
        -Path "master.dbo.mytable"

###<a name="test-existence-of--catalog-item"></a>Testowanie istnienia elementu wykazu

    Test-AzureRmDataLakeAnalyticsCatalogItem  `
        -Account $adlAnalyticsAccountName `
        -ItemType Database `
        -Path "master"

###<a name="create-catalog-secret"></a>Tworzenie hasła wykazu
    New-AzureRmDataLakeAnalyticsCatalogSecret  `
            -Account $adlAnalyticsAccountName `
            -DatabaseName "master" `
            -Secret (Get-Credential -UserName "username" -Message "Enter the password")

### <a name="modify-catalog-secret"></a>Modyfikowanie hasła wykazu
    Set-AzureRmDataLakeAnalyticsCatalogSecret  `
            -Account $adlAnalyticsAccountName `
            -DatabaseName "master" `
            -Secret (Get-Credential -UserName "username" -Message "Enter the password")



###<a name="delete-catalog-secret"></a>Usuwanie hasła wykazu
    Remove-AzureRmDataLakeAnalyticsCatalogSecret  `
            -Account $adlAnalyticsAccountName `
            -DatabaseName "master"


## <a name="use-azure-resource-manager-groups"></a>Używanie Menedżera zasobów Azure grup

Aplikacje zwykle składają się wiele elementów, na przykład aplikacji sieci web, bazy danych, serwer bazy danych, magazynowania i 3 usług innych firm. Menedżer zasobów Azure (ARM) umożliwia pracę z zasobami w aplikacji grupowo, określane jako grupa zasobów Azure. Możesz wdrożyć, aktualizowanie, monitorować lub usunąć wszystkie zasoby aplikacji w jednym, skoordynowanego operacji. Używanie szablonu do wdrożenia i tego szablonu można pracować w różnych środowiskach takich jak testowania, organizowanie i produkcji. Może zawierać wyjaśnienie rozliczenia dla Twojej organizacji, wyświetlając rzutowane koszty dla całej grupy. Aby uzyskać więcej informacji zobacz [Omówienie Menedżera zasobów Azure](../azure-resource-manager/resource-group-overview.md). 

Usługi analizy Lake danych może zawierać następujące składniki:

- Lake danych Azure analizy konta
- Konto Azure masowej Lake niezbędnych ustawień domyślnych
- Dodatkowe Azure danych Lake magazynu kont
- Kolejne konta magazynu platformy Azure

Możesz utworzyć wszystkie te elementy w jednej grupie ARM aby ułatwić zarządzanie.

![Lake danych Azure analizy konta i miejsca do magazynowania](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-arm-structure.png)

Konto analizy Lake danych i kont zależne miejsca do magazynowania musi znajdować się w tym samym centrum danych Azure.
Jednak grupy ARM może znajdować się w centrum danych.  

##<a name="see-also"></a>Zobacz też 

- [Omówienie analizy danych Lake bazy wiedzy Microsoft Azure](data-lake-analytics-overview.md)
- [Wprowadzenie do analiz Lake danych za pomocą Azure Portal](data-lake-analytics-get-started-portal.md)
- [Zarządzanie analiz Lake danych Azure za pomocą Azure Portal](data-lake-analytics-manage-use-portal.md)
- [Monitorowanie i rozwiązywanie problemów z Azure danych Lake analizy zadań przy użyciu Azure Portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

##<a name="appendix-a---data-lake-analytics-arm-template"></a>Dodatek A - szablon ARM analizy Lake danych

Następujące szablon ARM może być używany do wdrożenia analizy Lake dane konto i jego zależne magazynu Lake danych.  Zapisz go jako plik json, a następnie użyj skrypt programu PowerShell, aby nawiązać połączenie z szablonu. Aby uzyskać więcej informacji zobacz [Wdrażanie aplikacji za pomocą szablonu Menedżera zasobów Azure](../resource-group-template-deploy.md#deploy-with-powershell) i [szablonów do tworzenia Azure Menedżera zasobów](../resource-group-authoring-templates.md).

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "adlAnalyticsName": {
          "type": "string",
          "metadata": {
            "description": "The name of the Data Lake Analytics account to create."
          }
        },
        "adlStoreName": {
          "type": "string",
          "metadata": {
            "description": "The name of the Data Lake Store account to create."
          }
        }
      },
      "resources": [
        {
          "name": "[parameters('adlStoreName')]",
          "type": "Microsoft.DataLakeStore/accounts",
          "location": "East US 2",
          "apiVersion": "2015-10-01-preview",
          "dependsOn": [ ],
          "tags": { }
        },
        {
          "name": "[parameters('adlAnalyticsName')]",
          "type": "Microsoft.DataLakeAnalytics/accounts",
          "location": "East US 2",
          "apiVersion": "2015-10-01-preview",
          "dependsOn": [ "[concat('Microsoft.DataLakeStore/accounts/',parameters('adlStoreName'))]" ],
          "tags": { },
          "properties": {
            "defaultDataLakeStoreAccount": "[parameters('adlStoreName')]",
            "dataLakeStoreAccounts": [
              { "name": "[parameters('adlStoreName')]" }
            ]
          }
        }
      ],
      "outputs": {
        "adlAnalyticsAccount": {
          "type": "object",
          "value": "[reference(resourceId('Microsoft.DataLakeAnalytics/accounts',parameters('adlAnalyticsName')))]"
        },
        "adlStoreAccount": {
          "type": "object",
          "value": "[reference(resourceId('Microsoft.DataLakeStore/accounts',parameters('adlStoreName')))]"
        }
      }
    }

