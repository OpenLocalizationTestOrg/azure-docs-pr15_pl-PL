<properties 
   pageTitle="Wprowadzenie do analiz Lake danych Azure za pomocą programu PowerShell Azure | Azure" 
   description="Dowiedz się, jak utworzyć konto magazynu Lake danych, utworzyć zadanie analizy Lake danych za pomocą U-SQL za pomocą programu PowerShell Azure i przesłać zadanie. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/21/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-powershell"></a>Samouczek: wprowadzenie do analiz Lake danych Azure za pomocą programu PowerShell Azure

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Dowiedz się, jak za pomocą programu PowerShell Azure utworzyć konta Azure danych Lake analizy, definiowanie zadań analizy Lake danych w [Języku SQL U](data-lake-analytics-u-sql-get-started.md)i przesyłać zadania do danych Lake analityczny konta. Aby uzyskać więcej informacji na temat analizy Lake danych zobacz [Omówienie analizy Lake danych Azure](data-lake-analytics-overview.md).

W tym samouczku będzie rozwijanie zadanie, który odczytuje karty rozdzielając pliku wartości (TSV) i konwertuje ją do pliku (CSV wartości) przecinkami. Do wykonywania kroków samej samouczka przy użyciu innych obsługiwanych narzędzi, kliknij karty u góry tej sekcji.

##<a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego samouczka, musisz mieć następujące czynności:

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/pricing/free-trial/).
- **Pracy z programem PowerShell Azure**. Dowiedz się, [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).
    
##<a name="create-data-lake-analytics-account"></a>Tworzenie konta analizy Lake danych

Przed uruchomieniem zadania, musisz mieć konto analizy Lake danych. Aby utworzyć konto analizy Lake danych, określ następujące czynności:

- **Grupa zasobów Azure**: musi zostać utworzone konto analizy Lake dane w grupie Azure zasobów. [Menedżer zasobów Azure](../azure-resource-manager/resource-group-overview.md) umożliwia pracę z zasobami w aplikacji jako grupy. Możesz wdrożyć, aktualizowanie lub usuwanie wszystkich zasobów dla aplikacji w jednym, skoordynowanego operacji.  

    Wyliczyć grup zasobów w ramach subskrypcji:
    
        Get-AzureRmResourceGroup
    
    Aby utworzyć nową grupę zasobów:

        New-AzureRmResourceGroup `
            -Name "<Your resource group name>" `
            -Location "<Azure Data Center>" # For example, "East US 2"

- **Nazwa konta analizy Lake danych**
- **Lokalizacja**: jeden z centrami danych Azure, obsługiwanych analizy Lake danych.
- **Konto domyślne dane Lake**: każde konto analizy Lake danych ma domyślne konto Lake danych.

    Aby utworzyć nowe konto Lake danych:

        New-AzureRmDataLakeStoreAccount `
            -ResourceGroupName "<Your Azure resource group name>" `
            -Name "<Your Data Lake account name>" `
            -Location "<Azure Data Center>"  # For example, "East US 2"

    > [AZURE.NOTE] Nazwa konta Lake danych musi zawierać tylko małe litery i cyfry.



**Aby utworzyć konto analizy Lake danych**

1. Otwórz środowiska PowerShell ISE w miejscu pracy systemu Windows.
2. Uruchom następujący skrypt:

        $resourceGroupName = "<ResourceGroupName>"
        $dataLakeStoreName = "<DataLakeAccountName>"
        $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
        $location = "East US 2"
        
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
            -Name $dataLakeAnalyticsName `
            -ResourceGroupName $resourceGroupName `
            -Location $location `
            -DefaultDataLake $dataLakeStoreName
        
        Write-Host "The newly created Data Lake Analytics account ..."  -ForegroundColor Green
        Get-AzureRmDataLakeAnalyticsAccount `
            -ResourceGroupName $resourceGroupName `
            -Name $dataLakeAnalyticsName  

##<a name="upload-data-to-data-lake"></a>Przekazywanie danych do Lake danych

W tym samouczku będzie przetwarzał niektóre dzienniki wyszukiwania.  Dziennik wyszukiwania mogą być przechowywane w magazynie Lake danych lub magazyn obiektów Blob platformy Azure. 

Przykładowy plik dziennika wyszukiwania została skopiowana do publicznej kontenera obiektów Blob platformy Azure. Skorzystaj z tego skryptu programu PowerShell o pobranie pliku do pracy, a następnie przekaż plik do domyślnego konta magazynu Lake danych konta analizy Lake danych.

    $dataLakeStoreName = "<The default Data Lake Store account name>"
    
    $localFolder = "C:\Tutorials\Downloads\" # A temp location for the file. 
    $storageAccount = "adltutorials"  # Don't modify this value.
    $container = "adls-sample-data"  #Don't modify this value.

    # Create the temp location  
    New-Item -Path $localFolder -ItemType Directory -Force 

    # Download the sample file from Azure Blob storage
    $context = New-AzureStorageContext -StorageAccountName $storageAccount -Anonymous
    $blobs = Azure\Get-AzureStorageBlob -Container $container -Context $context
    $blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder

    # Upload the file to the default Data Lake Store account    
    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $localFolder"SearchLog.tsv" -Destination "/Samples/Data/SearchLog.tsv"

Poniższy skrypt programu PowerShell pokazano, jak uzyskać domyślną nazwę magazynu Lake danych konta analizy Lake danych:


    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticsName).Properties.DefaultDataLakeAccount
    echo $dataLakeStoreName

>[AZURE.NOTE] Azure Portal udostępnia interfejs użytkownika, aby skopiować przykładowych plików danych do domyślnego konta magazynu Lake danych. Aby uzyskać instrukcje zobacz [Wprowadzenie do analiz Lake danych Azure za pomocą Azure Portal](data-lake-analytics-get-started-portal.md#upload-data-to-the-default-data-lake-store-account).

Analizy Lake danych może też uzyskiwać dostęp Magazyn obiektów Blob platformy Azure.  Dotyczące przekazywania danych z magazynem obiektów Blob platformy Azure, zobacz [Przy użyciu programu PowerShell Azure z nośnikami Azure](../storage/storage-powershell-guide-full.md).

##<a name="submit-data-lake-analytics-jobs"></a>Przesyłanie danych Lake analizy zadań

Zadania analizy Lake danych są zapisywane w języku U SQL. Aby dowiedzieć się więcej na temat U SQL, zobacz [Wprowadzenie do języka U SQL](data-lake-analytics-u-sql-get-started.md) i [Dokumentacja języka U SQL](http://go.microsoft.com/fwlink/?LinkId=691348).

**Aby utworzyć skrypt zadanie analizy Lake danych**

- Tworzenie pliku tekstowego z poniższy skrypt U SQL i Zapisz plik tekstowy na miejscu pracy:

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();
        
        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    Ten skrypt U SQL odczytuje plik źródła danych przy użyciu **Extractors.Tsv()**, a następnie utworzy plik csv przy użyciu **Outputters.Csv()**. 
    
    Nie Modyfikuj dwie ścieżki, chyba że skopiuj plik źródłowy do innej lokalizacji.  Jeśli nie istnieje, analizy Lake danych utworzy folder wyjściowy.
    
    Jest prostsze używanie ścieżek względnych pliki zapisane w domyślnych danych Lake konta. Można także używać ścieżek bezwzględnych.  Na przykład 
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
        
    Aby uzyskać dostęp do plików w połączonych kont miejsca do magazynowania, należy użyć ścieżki bezwzględne.  Składnia plików przechowywanych w połączony klient magazyn Azure jest następująca:
    
        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Azure kontenera obiektów Blob z uprawnieniami dostępu publicznej kontenerów lub publicznej blob nie są obecnie obsługiwane.    
    
    
**Aby przesłać zadanie**

1. Otwórz środowiska PowerShell ISE w miejscu pracy systemu Windows.
2. Uruchom następujący skrypt:

        $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
        $usqlScript = "c:\tutorials\data-lake-analytics\copyFile.usql"
        
        $job = Submit-AzureRmDataLakeAnalyticsJob -Name "convertTSVtoCSV" -AccountName $dataLakeAnalyticsName –ScriptPath $usqlScript 

        Wait-AdlJob -Account $dataLakeAnalyticsName -JobId $job.JobId

        Get-AzureRmDataLakeAnalyticsJob -AccountName $dataLakeAnalyticsName -JobId $job.JobId

    W obszarze skrypt plik skryptu U SQL znajduje się w c:\tutorials\data-lake-analytics\copyFile.usql. Zaktualizuj odpowiednio ścieżkę pliku.
 
Po zakończeniu zadania można go za pomocą następujące polecenia cmdlet i pobrać plik:
    
    $resourceGroupName = "<Resource Group Name>"
    $dataLakeAnalyticName = "<Data Lake Analytic Account Name>"
    $destFile = "C:\tutorials\data-lake-analytics\SearchLog-from-Data-Lake.csv"
    
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticName).Properties.DefaultDataLakeAccount
    
    Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -path "/Output"
    
    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "/Output/SearchLog-from-Data-Lake.csv" -Destination $destFile

## <a name="see-also"></a>Zobacz też

- Aby wyświetlić samej samouczka przy użyciu innych narzędzi, kliknij selektory kartę w górnej części strony.
- Aby uzyskać bardziej złożonych kwerend, zobacz [Dzienniki analiza witryny sieci Web przy użyciu analizy Lake danych Azure](data-lake-analytics-analyze-weblogs.md).
- Aby rozpocząć tworzenie aplikacji U SQL, zobacz [skryptów można opracowywać U-SQL przy użyciu narzędzia Lake danych dla programu Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Aby dowiedzieć się U SQL, zobacz [Wprowadzenie do języka Azure danych Lake analizy U-SQL](data-lake-analytics-u-sql-get-started.md).
- Do zadań zarządzania zobacz [Zarządzanie Azure danych Lake analiz za pomocą Azure Portal](data-lake-analytics-manage-use-portal.md).
- Aby uzyskać omówienie analizy Lake danych, zobacz [Omówienie analizy Lake danych Azure](data-lake-analytics-overview.md).

