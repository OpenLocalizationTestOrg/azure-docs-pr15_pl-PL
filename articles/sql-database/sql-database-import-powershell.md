<properties
    pageTitle="Importowanie pliku PLECAK tworzenie bazy danych programu Azure SQL za pomocą programu PowerShell | Microsoft Azure"
    description="Importowanie pliku PLECAK tworzenie bazy danych programu Azure SQL za pomocą programu PowerShell"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="08/31/2016"
    ms.author="sstein"/>

# <a name="import-a-bacpac-file-to-create-an-azure-sql-database-by-using-powershell"></a>Importowanie pliku PLECAK tworzenie bazy danych programu Azure SQL za pomocą programu PowerShell

**Pojedynczy bazy danych**

> [AZURE.SELECTOR]
- [Azure portal](sql-database-import.md)
- [Programu PowerShell](sql-database-import-powershell.md)
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)

Ten artykuł zawiera wskazówki dotyczące tworzenia bazy danych programu Azure SQL, importując plik [PLECAK](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) przy użyciu programu PowerShell.

Baza danych jest tworzona z pliku PLECAK (.bacpac) zaimportowane z kontenera obiektów blob platformy Azure magazynu. Jeśli nie masz pliku PLECAK w magazynie Azure, zobacz [archiwum bazy danych programu Azure SQL w pliku PLECAK przy użyciu programu PowerShell](sql-database-export-powershell.md). Jeśli masz już pliku PLECAK, który nie jest w magazynie Azure, [Użyj AzCopy, aby łatwo przekazać go do swojego konta magazynu platformy Azure](../storage/storage-use-azcopy.md#blob-upload).

> [AZURE.NOTE] Baza danych SQL Azure automatycznie tworzy i zachowuje kopie zapasowe dla każdej bazy danych użytkowników można przywrócić. Aby uzyskać szczegółowe informacje zobacz [Baza danych SQL automatycznego wykonywania kopii zapasowych](sql-database-automated-backups.md).


Aby zaimportować z bazą danych SQL, są potrzebne następujące elementy:

- Subskrypcję usługi Azure. W razie potrzeby subskrypcji usługi Azure po prostu kliknij **Bezpłatną wersję próbną** u góry strony, a następnie wróć do końca w tym artykule.
- PLECAK pliku bazy danych, który chcesz zaimportować. PLECAK musi się znajdować w kontenerze obiektów blob [konto Azure miejsca do magazynowania](../storage/storage-create-storage-account.md) .



[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]



## <a name="set-up-the-variables-for-your-environment"></a>Ustawianie zmiennych w środowisku usługi

Istnieje kilka zmiennych miejsce, w którym należy zastąpić przykładowe wartości określonych wartości dla bazy danych i konta miejsca do magazynowania.

Nazwa serwera musi być serwera, który istnieje obecnie w wybranych w poprzednim kroku subskrypcji. Należy serwera, który ma zostać utworzone w bazie danych. Importowanie bazy danych bezpośrednio w puli elastyczne nie jest obsługiwane. Ale najpierw zaimportować do jednej bazie danych, a następnie przenieść bazę danych w puli.

Nazwa bazy danych jest nazwę, która ma dla nowej bazy danych.

    $ResourceGroupName = "resource group name"
    $ServerName = "server name"
    $DatabaseName = "database name"


Poniższe zmienne są z konta miejsca do magazynowania, miejsce, w którym znajduje się z PLECAK. W [Azure portal](https://portal.azure.com)przejdź do swojego konta miejsca do magazynowania, aby uzyskać te wartości. Klucz podstawowy dostęp można znaleźć, klikając pozycję **wszystkie ustawienia** , a następnie **klawisze** z karta konta miejsca do magazynowania.

Nazwy obiektów blob jest nazwą pliku PLECAK, który chcesz utworzyć bazę danych z. Musisz rozszerzenie .bacpac.

    $StorageName = "storageaccountname"
    $StorageKeyType = "StorageAccessKey"
    $StorageUri = "http://$StorageName.blob.core.windows.net/containerName/filename.bacpac"
    $StorageKey = "primaryaccesskey"


Uruchamianie [Get-poświadczenia] (https://msdn.microsoft.com/library/azure/hh849815(v=azure.300\).aspx) polecenia cmdlet powoduje otwarcie okna monitu o podanie nazwy użytkownika i hasła usługi. Wprowadź identyfikator logowania administratora i hasło dla serwera bazy danych SQL ($ServerName z powyżej), a nie nazwę użytkownika i hasło do konta Azure.

    $credential = Get-Credential


## <a name="import-the-database"></a>Importowanie bazy danych

To polecenie przesyła żądanie Importowanie bazy danych usługi. W zależności od rozmiaru bazy danych operacji importowania może potrwać do wykonania.

    $importRequest = New-AzureRmSqlDatabaseImport –ResourceGroupName $ResourceGroupName –ServerName $ServerName –DatabaseName $DatabaseName –StorageKeytype $StorageKeyType –StorageKey $StorageKey -StorageUri $StorageUri –AdministratorLogin $credential.UserName –AdministratorLoginPassword $credential.Password –Edition Standard –ServiceObjectiveName S0 -DatabaseMaxSizeBytes 50000


## <a name="monitor-the-progress-of-the-operation"></a>Monitorowanie postępu operacji

Po uruchomieniu [nowy AzureRmSqlDatabaseImport] (https://msdn.microsoft.com/library/azure/mt707793(v=azure.300\).aspx), możesz sprawdzić stan żądania, uruchamiając [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx).

    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink



## <a name="sql-database-powershell-import-script"></a>Skrypt importu programu PowerShell bazy danych SQL


    $ResourceGroupName = "resourceGroupName"
    $ServerName = "servername"
    $DatabaseName = "databasename"

    $StorageName = "storageaccountname"
    $StorageKeyType = "StorageAccessKey"
    $StorageUri = "http://$StorageName.blob.core.windows.net/containerName/filename.bacpac"
    $StorageKey = "primaryaccesskey"

    $credential = Get-Credential

    $importRequest = New-AzureRmSqlDatabaseImport –ResourceGroupName $ResourceGroupName –ServerName $ServerName –DatabaseName $DatabaseName –StorageKeytype $StorageKeyType –StorageKey $StorageKey -StorageUri $StorageUri –AdministratorLogin $credential.UserName –AdministratorLoginPassword $credential.Password –Edition Standard –ServiceObjectiveName S0 -DatabaseMaxSizeBytes 50000

    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink



## <a name="next-steps"></a>Następne kroki

- Aby dowiedzieć się łączyć się i kwerenda zaimportowanych bazy danych SQL, zobacz [Nawiązywanie połączenia z bazą danych SQL z programu SQL Server Management Studio i wykonać przykładowa kwerenda T-SQL](sql-database-connect-query-ssms.md)
