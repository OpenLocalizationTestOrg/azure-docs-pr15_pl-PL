<properties
    pageTitle="Archiwum bazy danych programu Azure SQL w pliku PLECAK przy użyciu programu PowerShell"
    description="Archiwum bazy danych programu Azure SQL w pliku PLECAK przy użyciu programu PowerShell"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="08/15/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="archive-an-azure-sql-database-to-a-bacpac-file-by-using-powershell"></a>Archiwum bazy danych programu Azure SQL w pliku PLECAK przy użyciu programu PowerShell

> [AZURE.SELECTOR]
- [Azure portal](sql-database-export.md)
- [Programu PowerShell](sql-database-export-powershell.md)


Ten artykuł zawiera wskazówki dotyczące archiwizowania bazy danych Azure SQL w pliku [PLECAK](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) (przechowywane w magazynie obiektów Blob platformy Azure) przy użyciu programu PowerShell.

Jeśli chcesz utworzyć archiwum bazy danych programu Azure SQL, schemat bazy danych i danych można wyeksportować do pliku PLECAK. Plik PLECAK jest po prostu pliku ZIP z rozszerzeniem .bacpac. Plik PLECAK później mogą być przechowywane, magazyn obiektów Blob platformy Azure lub magazynu lokalnego w lokalizacji lokalnej. Można również importować do bazy danych SQL Azure lub do instalacji programu SQL Server lokalnego.

**Zagadnienia dotyczące**

- Archiwum być transakcyjnie spójne należy się upewnić, że nie zapisu aktywności występuje podczas eksportowania lub że są eksportowane [transakcyjnie spójne kopii](sql-database-copy.md) bazy danych Azure SQL.
- Maksymalny rozmiar pliku PLECAK archiwizowane magazynem obiektów Blob platformy Azure wynosi 200 GB. Aby archiwizować większy plik PLECAK do magazynu lokalnego, użyj narzędzia wiersza polecenia [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) . To narzędzie jest dostarczany z programu Visual Studio i SQL Server. Możesz też [pobrać](https://msdn.microsoft.com/library/mt204009.aspx) najnowszą wersję programu SQL Server Data Tools uzyskanie to narzędzie.
- Archiwizowanie do magazynu Azure premium za pomocą pliku PLECAK nie jest obsługiwane.
- Jeśli operacji eksportowania przekracza 20 godzin, mogą zostać anulowane. Aby zwiększyć wydajność podczas eksportowania, można wykonywać następujące czynności:
 - Tymczasowe Zwiększ poziom obsługi.
 - Zaprzestanie wszystkie odczytywanie i zapisywanie aktywności podczas eksportowania.
 - [Indeks grupowany](https://msdn.microsoft.com/library/ms190457.aspx) za pomocą wartość różną od null na wszystkich dużych tabel. Bez grupowany indeksy eksportu może nie działać Jeśli trwa dłużej niż 6 – 12 godzin. Jest to spowodowane usługę eksportu musi wykonać skanowanie tabeli, aby spróbować wyeksportować całą tabelę. Dobrym sposobem na określenie, jeśli tabele są zoptymalizowane dla eksportu jest Uruchom **DBCC SHOW_STATISTICS** i upewnij się, że *RANGE_HI_KEY* jest inna niż zero i wartość ma dobrej. Aby uzyskać szczegółowe informacje zobacz [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).

> [AZURE.NOTE] BACPACs nie mają być używane do tworzenia kopii zapasowych i przywracanie operacji. Baza danych SQL Azure automatycznie tworzy kopie zapasowe dla każdej bazy danych użytkownika. Aby uzyskać szczegółowe informacje zobacz [Baza danych SQL automatycznego wykonywania kopii zapasowych](sql-database-automated-backups.md).

Aby wykonać w tym artykule, są potrzebne następujące elementy:

- Subskrypcję usługi Azure.
- Baza danych Azure SQL.
- [Magazyn standardowy Azure konta](../storage/storage-create-storage-account.md)z kontenera obiektów blob do przechowywania PLECAK w magazynie standardowy.


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]




## <a name="export-your-database"></a>Eksportowanie do bazy danych

[AzureRmSqlDatabaseExport nowy] (polecenia cmdlet https://msdn.microsoft.com/library/azure/mt707796(v=azure.300\).aspx) przesyła żądanie eksportu bazy danych usługi. W zależności od rozmiaru bazy danych operacji eksportowania może zająć trochę czasu, aby zakończyć.

> [AZURE.IMPORTANT] W celu zagwarantowania pliku transakcyjnie spójne PLECAK, należy pierwszy, [Utwórz kopię bazy danych](sql-database-copy-powershell.md), a następnie wyeksportuj kopii bazy danych.


     $exportRequest = New-AzureRmSqlDatabaseExport –ResourceGroupName $ResourceGroupName –ServerName $ServerName `
       –DatabaseName $DatabaseName –StorageKeytype $StorageKeytype –StorageKey $StorageKey -StorageUri $BacpacUri `
       –AdministratorLogin $creds.UserName –AdministratorLoginPassword $creds.Password


## <a name="monitor-the-progress-of-the-export-operation"></a>Monitorowanie postępu operacji eksportowania

Po uruchomieniu [nowy AzureRmSqlDatabaseExport] (https://msdn.microsoft.com/library/azure/mt603644(v=azure.300\).aspx), możesz sprawdzić stan żądania, uruchamiając [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx). Uruchomiona zaraz po żądanie zazwyczaj zwraca **Stan: trakcie wykonywania**. Po wyświetleniu **Stan: zakończyła się pomyślnie** ukończeniu eksportowania.


    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink



## <a name="export-sql-database-example"></a>Eksportowanie przykładu bazy danych SQL

W poniższym przykładzie eksportuje istniejącej bazy danych SQL do PLECAK, a następnie pokazano, jak sprawdzić stan operacji eksportowania.

Aby uruchomić w przykładzie, istnieje kilka czynników, które należy zastąpić określonych wartości dla Twojego konta bazy danych i miejsca do magazynowania. W [Azure portal](https://portal.azure.com)przejdź do swojego konta przestrzeni dyskowej, aby uzyskać nazwę konta magazynu, nazwa kontenera obiektów blob i wartości klucza. Klucz można znaleźć, klikając pozycję **klawiszy dostępu** na karta konta z miejsca do magazynowania.

Zamienianie następujące `VARIABLE-VALUES` z wartościami dla określonych zasobów Azure. Nazwa bazy danych jest istniejącej bazy danych, które chcesz wyeksportować.



    $subscriptionId = "YOUR AZURE SUBSCRIPTION ID"

    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId

    # Database to export
    $DatabaseName = "DATABASE-NAME"
    $ResourceGroupName = "RESOURCE-GROUP-NAME"
    $ServerName = "SERVER-NAME"
    $serverAdmin = "ADMIN-NAME"
    $serverPassword = "ADMIN-PASSWORD" 
    $securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
    $creds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword

    # Generate a unique filename for the BACPAC
    $bacpacFilename = $DatabaseName + (Get-Date).ToString("yyyyMMddHHmm") + ".bacpac"

    # Storage account info for the BACPAC
    $BaseStorageUri = "https://STORAGE-NAME.blob.core.windows.net/BLOB-CONTAINER-NAME/"
    $BacpacUri = $BaseStorageUri + $bacpacFilename
    $StorageKeytype = "StorageAccessKey"
    $StorageKey = "YOUR STORAGE KEY"

    $exportRequest = New-AzureRmSqlDatabaseExport –ResourceGroupName $ResourceGroupName –ServerName $ServerName `
       –DatabaseName $DatabaseName –StorageKeytype $StorageKeytype –StorageKey $StorageKey -StorageUri $BacpacUri `
       –AdministratorLogin $creds.UserName –AdministratorLoginPassword $creds.Password
    $exportRequest

    # Check status of the export
    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink



## <a name="next-steps"></a>Następne kroki

- Aby dowiedzieć się, jak zaimportować bazy danych programu Azure SQL przy użyciu programu Powershell, zobacz [Importowanie PLECAK przy użyciu programu PowerShell](sql-database-import-powershell.md).


## <a name="additional-resources"></a>Dodatkowe zasoby

- [Nowy AzureRmSqlDatabaseExport] (https://msdn.microsoft.com/library/azure/mt707796(v=azure.300\).aspx)
- [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx)