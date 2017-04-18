<properties 
    pageTitle="Kod XEvent zdarzenia pliku bazy danych SQL | Microsoft Azure" 
    description="Przykład dwuetapowa kod demonstrujący obiekt docelowy plik zdarzeń rozszerzonego zdarzenia bazy danych SQL Azure zapewnia programu PowerShell i Transact-SQL. Azure magazyn jest wymaganym w ramach tego scenariusza." 
    services="sql-database" 
    documentationCenter="" 
    authors="MightyPen" 
    manager="jhubbard" 
    editor="" 
    tags=""/>


<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="genemi"/>


# <a name="event-file-target-code-for-extended-events-in-sql-database"></a>Zdarzenia plik docelowy kod rozszerzone zdarzenia w bazie danych SQL

[AZURE.INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Chcesz próbki kompletny kod niezawodne umożliwia przechwytywanie i raportu informacje dotyczące zdarzenia rozszerzonego.


Program Microsoft SQL Server [docelowej plik zdarzeń](http://msdn.microsoft.com/library/ff878115.aspx) służy do przechowywania wyjściowe zdarzenia w pliku lokalnym dysku twardym. Jednak tych plików nie są dostępne dla bazy danych SQL Azure. Użyj zamiast tego możemy usługą Azure magazyn do obsługi docelowej plik zdarzeń.


W tym temacie przedstawiono przykład dwuetapowa kodu:


- Programu PowerShell, aby utworzyć kontenera Azure magazynu w chmurze.

- Transact-SQL:
 - Aby przypisać kontenera magazynu Azure do obiekt docelowy plik zdarzeń.
 - Tworzenie i uruchamianie sesji zdarzenia i tak dalej.


## <a name="prerequisites"></a>Wymagania wstępne


- Konto Azure i subskrypcji. Możesz zalogować do [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).


- Dowolnej bazy danych, które można utworzyć tabelę w.
 - Opcjonalnie możesz [utworzyć bazę danych programu **AdventureWorksLT** pokaz](sql-database-get-started.md) w minutach.


- SQL Server Management Studio (ssms.exe), najlepiej najnowszej miesięczny wersji aktualizacji. Możesz pobrać najnowszą ssms.exe z:
 - Temacie [Pobierz program SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).
 - [Bezpośrednie łącze do pobierania.](http://go.microsoft.com/fwlink/?linkid=616025)


- Musi być [moduły Azure programu PowerShell](http://go.microsoft.com/?linkid=9811175) zainstalowany.
 - Moduły zapewniają poleceń, takich jak - **AzureStorageAccount nowy**.


## <a name="phase-1-powershell-code-for-azure-storage-container"></a>Fazy 1: Kod programu PowerShell kontenera magazynu platformy Azure


Ten PowerShell to faza 1 Przykładowy kod dwuetapowa.

Skrypt zaczyna się od poleceń, aby oczyścić, gdy możliwe poprzedniego uruchomić i jest rerunnable.



1. Wklej skrypt programu PowerShell do edytora tekstu zwykłego, takich jak Notepad.exe, a następnie Zapisz skrypt jako plik z rozszerzeniem **.ps1**.

2. Środowiska PowerShell ISE Uruchom jako Administrator.

3. W wierszu polecenia wpisz<br/>`Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser`<br/>a następnie naciśnij klawisz Enter.

4. W środowiska PowerShell ISE Otwórz plik **.ps1** . Uruchom skrypt.

5. Skrypt pierwszego uruchomienia nowe okno, w którym zalogowaniu się do Azure.
 - Jeśli bez przerywania sesji programu Uruchom ponownie skrypt, masz wygodny możliwości komentowania polecenie **Dodaj AzureAccount** .


![Środowiska PowerShell ISE z modułem Azure zainstalowane i gotowe do uruchomienia skryptu.][30_powershell_ise]


&nbsp;


```
## TODO: Before running, find all 'TODO' and make each edit!!

#--------------- 1 -----------------------


# You can comment out or skip this Add-AzureAccount
# command after the first run.
# Current PowerShell environment retains the successful outcome.

'Expect a pop-up window in which you log in to Azure.'


Add-AzureAccount

#-------------- 2 ------------------------


'
TODO: Edit the values assigned to these variables, especially the first few!
'

# Ensure the current date is between
# the Expiry and Start time values that you edit here.

$subscriptionName    = 'YOUR_SUBSCRIPTION_NAME'
$policySasExpiryTime = '2016-01-28T23:44:56Z'
$policySasStartTime  = '2015-08-01'


$storageAccountName     = 'gmstorageaccountxevent'
$storageAccountLocation = 'West US'
$contextName            = 'gmcontext'
$containerName          = 'gmcontainerxevent'
$policySasToken         = 'gmpolicysastoken'


# Leave this value alone, as 'rwl'.
$policySasPermission = 'rwl'

#--------------- 3 -----------------------


# The ending display lists your Azure subscriptions.
# One should match the $subscriptionName value you assigned
#   earlier in this PowerShell script. 

'Choose an existing subscription for the current PowerShell environment.'


Select-AzureSubscription -SubscriptionName $subscriptionName


#-------------- 4 ------------------------


'
Clean up the old Azure Storage Account after any previous run, 
before continuing this new run.'


If ($storageAccountName)
{
    Remove-AzureStorageAccount -StorageAccountName $storageAccountName
}

#--------------- 5 -----------------------

[System.DateTime]::Now.ToString()

'
Create a storage account. 
This might take several minutes, will beep when ready.
  ...PLEASE WAIT...'

New-AzureStorageAccount `
    -StorageAccountName $storageAccountName `
    -Location           $storageAccountLocation

[System.DateTime]::Now.ToString()

[System.Media.SystemSounds]::Beep.Play()


'
Get the primary access key for your storage account.
'


$primaryAccessKey_ForStorageAccount = `
    (Get-AzureStorageKey `
        -StorageAccountName $storageAccountName).Primary

"`$primaryAccessKey_ForStorageAccount = $primaryAccessKey_ForStorageAccount"

'Azure Storage Account cmdlet completed.
Remainder of PowerShell .ps1 script continues.
'

#--------------- 6 -----------------------


# The context will be needed to create a container within the storage account.

'Create a context object from the storage account and its primary access key.
'

$context = New-AzureStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey  $primaryAccessKey_ForStorageAccount


'Create a container within the storage account.
'


$containerObjectInStorageAccount = New-AzureStorageContainer `
    -Name    $containerName `
    -Context $context


'Create a security policy to be applied to the SAS token.
'

New-AzureStorageContainerStoredAccessPolicy `
    -Container  $containerName `
    -Context    $context `
    -Policy     $policySasToken `
    -Permission $policySasPermission `
    -ExpiryTime $policySasExpiryTime `
    -StartTime  $policySasStartTime 

'
Generate a SAS token for the container.
'
Try
{
    $sasTokenWithPolicy = New-AzureStorageContainerSASToken `
        -Name    $containerName `
        -Context $context `
        -Policy  $policySasToken
}
Catch 
{
    $Error[0].Exception.ToString()
}

#-------------- 7 ------------------------


'Display the values that YOU must edit into the Transact-SQL script next!:
'

"storageAccountName: $storageAccountName"
"containerName:      $containerName"
"sasTokenWithPolicy: $sasTokenWithPolicy"

'
REMINDER: sasTokenWithPolicy here might start with "?" character, which you must exclude from Transact-SQL.
'

'
(Later, return here to delete your Azure Storage account. See the preceding - Remove-AzureStorageAccount -StorageAccountName $storageAccountName)'

'
Now shift to the Transact-SQL portion of the two-part code sample!'

# EOFile
```


&nbsp;


Weź pod uwagę kilka nazwanych wartości, które skrypt programu PowerShell wyświetla po jego zakończeniu. Należy edytować te wartości do skrypt języka Transact-SQL, który następuje fazy 2.


## <a name="phase-2-transact-sql-code-that-uses-azure-storage-container"></a>Faza 2: Transact-SQL kod, który używa kontenera magazynu platformy Azure


- W fazie 1 Ten przykładowy kod uruchomiono skrypt programu PowerShell, aby utworzyć kontener magazyn Azure.
- Następny w fazy 2 następujący skrypt języka Transact-SQL należy użyć kontenerze.


Skrypt zaczyna się od poleceń, aby oczyścić, gdy możliwe poprzedniego uruchomić i jest rerunnable.


Skrypt programu PowerShell wydrukowane kilka wartości nazwanych po jego zakończeniu. Należy edytować skrypt języka Transact-SQL, aby użyć tych wartości. Znajdowanie **zadania** w obszarze skrypt języku Transact-SQL, aby zlokalizować Edytuj punkty.


1. Otwórz program SQL Server Management Studio (ssms.exe).

2. Nawiązywanie połączenia z bazą danych bazy danych SQL Azure.

3. Kliknij, aby otworzyć nowe okienko kwerendy.

4. Wklej następujący skrypt języka Transact-SQL w okienku zapytania.

5. Znajdź każdego **zadania** skryptu i wprowadź odpowiednie zmiany.

6. Zapisz, a następnie uruchom skrypt.


&nbsp;


> [AZURE.WARNING] Wartość klucza skojarzeń zabezpieczeń wygenerowane przez powyższy skrypt programu PowerShell może rozpoczynać "?" (znak zapytania). Użycie klucza skojarzeń zabezpieczeń w następujący skrypt T-SQL, należy *usunąć interlinię "?"*. W przeciwnym razie z biznesową może być blokowany przez zabezpieczenia.


&nbsp;


```
---- TODO: First, run the PowerShell portion of this two-part code sample.
---- TODO: Second, find every 'TODO' in this Transact-SQL file, and edit each.

---- Transact-SQL code for Event File target on Azure SQL Database.


SET NOCOUNT ON;
GO


----  Step 1.  Establish one little table, and  ---------
----  insert one row of data.


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'gmTabEmployee')
BEGIN
    DROP TABLE gmTabEmployee;
END
GO


CREATE TABLE gmTabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO gmTabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO


------  Step 2.  Create key, and  ------------
------  Create credential (your Azure Storage container must already exist).


IF NOT EXISTS
    (SELECT * FROM sys.symmetric_keys
        WHERE symmetric_key_id = 101)
BEGIN
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '0C34C960-6621-4682-A123-C7EA08E3FC46' -- Or any newid().
END
GO


IF EXISTS
    (SELECT * FROM sys.database_scoped_credentials
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        WHERE name = 'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent')
BEGIN
    DROP DATABASE SCOPED CREDENTIAL
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent] ;
END
GO


CREATE
    DATABASE SCOPED
    CREDENTIAL
        -- use '.blob.',   and not '.queue.' or '.table.' etc.
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    WITH
        IDENTITY = 'SHARED ACCESS SIGNATURE',  -- "SAS" token.
        -- TODO: Paste in the long SasToken string here for Secret, but exclude any leading '?'.
        SECRET = 'sv=2014-02-14&sr=c&si=gmpolicysastoken&sig=EjAqjo6Nu5xMLEZEkMkLbeF7TD9v1J8DNB2t8gOKTts%3D'
    ;
GO


------  Step 3.  Create (define) an event session.  --------
------  The event session has an event with an action,
------  and a has a target.

IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'gmeventsessionname240b')
BEGIN
    DROP
        EVENT SESSION
            gmeventsessionname240b
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE

    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE 'UPDATE gmTabEmployee%'
            )
    ADD TARGET
        package0.event_file
            (
            -- TODO: Assign AzureStorageAccount name, and the associated Container name.
            -- Also, tweak the .xel file name at end, if you like.
            SET filename =
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b.xel'
            )
    WITH
        (MAX_MEMORY = 10 MB,
        MAX_DISPATCH_LATENCY = 3 SECONDS)
    ;
GO


------  Step 4.  Start the event session.  ----------------
------  Issue the SQL Update statements that will be traced.
------  Then stop the session.

------  Note: If the target fails to attach,
------  the session must be stopped and restarted.

ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = START;
GO


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
GO


ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = STOP;
GO


-------------- Step 5.  Select the results. ----------

SELECT
        *, 'CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS!' as [CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS],
        CAST(event_data AS XML) AS [event_data_XML]  -- TODO: In ssms.exe results grid, double-click this cell!
    FROM
        sys.fn_xe_file_target_read_file
            (
                -- TODO: Fill in Storage Account name, and the associated Container name.
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b',
                null, null, null
            );
GO


-------------- Step 6.  Clean up. ----------

DROP
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE;
GO

DROP DATABASE SCOPED CREDENTIAL
    -- TODO: Assign AzureStorageAccount name, and the associated Container name.
    [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    ;
GO

DROP TABLE gmTabEmployee;
GO

PRINT 'Use PowerShell Remove-AzureStorageAccount to delete your Azure Storage account!';
GO
```


&nbsp;


Jeśli docelowej nie powiedzie się dołączyć podczas uruchamiania, należy zatrzymać i ponownie uruchomić sesji zdarzeń:


```
ALTER EVENT SESSION ... STATE = STOP;
GO
ALTER EVENT SESSION ... STATE = START;
GO
```


&nbsp;


## <a name="output"></a>Wynik


Po ukończeniu działania skryptu języku Transact-SQL, kliknij komórkę w obszarze nagłówka kolumny **event_data_XML** . Jeden **<event>** element jest wyświetlany przedstawia jeden instrukcja UPDATE.

W tym miejscu jest jednym **<event>** element, który został wygenerowany podczas testowania:


&nbsp;


```
<event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T19:18:45.420Z">
  <data name="state">
    <value>0</value>
    <text>Normal</text>
  </data>
  <data name="line_number">
    <value>5</value>
  </data>
  <data name="offset">
    <value>148</value>
  </data>
  <data name="offset_end">
    <value>368</value>
  </data>
  <data name="statement">
    <value>UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe'</value>
  </data>
  <action name="sql_text" package="sqlserver">
    <value>

SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
</value>
  </action>
</event>
```

&nbsp;


Powyższy skrypt w języku Transact-SQL używane następującą funkcję systemu do czytania event_file:

- [sys.fn_xe_file_target_read_file (w języku Transact-SQL)](http://msdn.microsoft.com/library/cc280743.aspx)


Wyjaśnienie zaawansowane opcje wyświetlania danych z rozszerzonego zdarzeń jest dostępna w:

- [Zaawansowane wyświetlania danych docelowej z rozszerzone zdarzenia](http://msdn.microsoft.com/library/mt752502.aspx)

&nbsp;


## <a name="converting-the-code-sample-to-run-on-sql-server"></a>Konwertowanie przykładowy kod do uruchamiania w programie SQL Server


Załóżmy, że ma być uruchomiony powyższego przykładu języku Transact-SQL w programie Microsoft SQL Server.


- Dla uproszczenia czy chcesz całkowicie zastąpić stosowania kontenera magazynu Azure prosty plik, na przykład **C:\myeventdata.xel**. Czy można zapisać pliku na lokalnym dysku twardym komputera obsługującego programu SQL Server.


- **Tworzenie klucza głównego** i **Tworzenie POŚWIADCZEŃ**nie jest wymagane dowolnego rodzaju instrukcji Transact-SQL.


- W instrukcji **Tworzenie zdarzeń sesji** w jego klauzuli **Dodawanie docelowej** należy zastąpić wartość Http przypisana do **filename =** z ciągiem pełną ścieżkę, takich jak **C:\myfile.xel**.
 - Brak konta magazynu platformy Azure muszą uczestniczyć.


## <a name="more-information"></a>Więcej informacji


Aby uzyskać więcej informacji o kontach i kontenerów w usłudze Azure miejsca do magazynowania zobacz:

- [Jak używać magazyn obiektów Blob z .NET](../storage/storage-dotnet-how-to-use-blobs.md)
- [Nadawanie nazw i odwoływanie się do kontenerów, obiektów blob i metadanych](http://msdn.microsoft.com/library/azure/dd135715.aspx)
- [Praca z kontenera Root](http://msdn.microsoft.com/library/azure/ee395424.aspx)
- [Lekcja 1: Tworzenie zasadę dostępu przechowywane i podpis udostępniania w kontenerze Azure](http://msdn.microsoft.com/library/dn466430.aspx)
    - [Lekcja 2: Tworzenie poświadczeń programu SQL Server, korzystając z podpisu udostępniania](http://msdn.microsoft.com/library/dn466435.aspx)




<!--
Image references.
-->

[30_powershell_ise]: ./media/sql-database-xevent-code-event-file/event-file-powershell-ise-b30.png

