<properties 
    pageTitle="Tworzenie i zarządzanie zadaniami nieelastyczne bazy danych przy użyciu programu PowerShell" 
    description="Programu PowerShell służące do zarządzania pul bazy danych SQL Azure" 
    services="sql-database" documentationCenter=""  
    manager="jhubbard" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove" />

# <a name="create-and-manage-a-sql-database-elastic-database-jobs-using-powershell-preview"></a>Tworzenie i zarządzanie zadaniami elastyczne bazy danych SQL Database, przy użyciu programu PowerShell (wersja preview)

> [AZURE.SELECTOR]
- [Azure portal](sql-database-elastic-jobs-create-and-manage.md)
- [Programu PowerShell](sql-database-elastic-jobs-powershell.md)



Interfejsy API programu PowerShell dla **zadań elastyczne baz danych** (wersja preview), umożliwiają definiowanie grupy baz danych, które będzie wykonywać skryptów. W tym artykule przedstawiono sposób tworzenia i zarządzania nimi **zadań elastyczne baz danych** przy użyciu poleceń cmdlet programu PowerShell. Zobacz [Omówienie elastyczne zadania](sql-database-elastic-jobs-overview.md). 

## <a name="prerequisites"></a>Wymagania wstępne
* Subskrypcję usługi Azure. W bezpłatnej wersji próbnej zobacz [bezpłatną wersję próbną jednego miesiąca](https://azure.microsoft.com/pricing/free-trial/).
* Zestaw baz danych utworzonych przy użyciu narzędzia nieelastyczne bazy danych. Zobacz [Rozpoczynanie pracy z narzędzia elastyczne bazy danych](sql-database-elastic-scale-get-started.md).
* Azure programu PowerShell. Aby uzyskać szczegółowe informacje zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).
* **Zadania nieelastyczne bazy danych** Pakiet PowerShell: zobacz [Instalowanie elastyczne bazy danych zadania](sql-database-elastic-jobs-service-installation.md)

### <a name="select-your-azure-subscription"></a>Wybierz subskrypcję Azure

Aby zaznaczyć subskrypcji potrzebny identyfikator (**-SubscriptionId**) subskrypcji subskrypcji usługi lub nazwę (**-SubscriptionName**). Jeśli masz wiele subskrypcji można uruchomić polecenia cmdlet **Get-AzureRmSubscription** i skopiuj informacje o odpowiedniej subskrypcji z zestawu wyników. Po umieszczeniu informacji o subskrypcji, uruchom następujące polecenia, aby ustawić jako domyślny, czyli docelowej do tworzenia i zarządzania zadaniami tej subskrypcji:

    Select-AzureRmSubscription -SubscriptionId {SubscriptionID}

[Środowiska PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx) jest zalecany do zastosowania do rozwoju i wykonywanie skryptów programu PowerShell przed zadań nieelastyczne baz danych.

## <a name="elastic-database-jobs-objects"></a>Elastyczne obiektów zadań bazy danych

W poniższej tabeli wymieniono się wszystkie typy obiektów zadań **Nieelastyczne bazy danych** wraz z jego odpowiednich API programu PowerShell i opis.

<table style="width:100%">
  <tr>
    <th>Typ obiektu</th>
    <th>Opis</th>
    <th>Pokrewne API programu PowerShell</th>
  </tr>
  <tr>
    <td>Poświadczenia</td>
    <td>Nazwa użytkownika i hasło używane podczas łączenia się z bazami danych dla aplikacji DACPACs lub wykonywanie skryptów. <p>Hasło jest szyfrowane przed wysłaniem do i przechowywanie w bazie danych elastyczne zadań baz danych.  Hasło są odszyfrowywane przez usługę elastyczne zadań bazy danych przy użyciu poświadczeń utworzone i przekazane z skrypt instalacji.</td>
    <td><p>Get-AzureSqlJobCredential</p>
    <p>Nowy AzureSqlJobCredential</p><p>Ustawianie AzureSqlJobCredential</p></td></td>
  </tr>

  <tr>
    <td>Skrypt</td>
    <td>Skrypt języka Transact-SQL może być używany do wykonywania wielu baz danych.  Skrypt powinien tworzone za idempotent, ponieważ usługa będzie ponawiał próby wykonywanie skryptu po awarii.
    </td>
    <td>
    <p>Get-AzureSqlJobContent</p>
    <p>Get-AzureSqlJobContentDefinition</p>
    <p>Nowy AzureSqlJobContent</p>
    <p>Ustawianie AzureSqlJobContentDefinition</p>
    </td>
  </tr>

  <tr>
    <td>DACPAC</td>
    <td><a href="https://msdn.microsoft.com/library/ee210546.aspx">Aplikacja warstwy danych </a> pakiet, aby stosować w całej bazy danych.

    </td>
    <td>
    <p>Get-AzureSqlJobContent</p>
    <p>Nowy AzureSqlJobContent</p>
    <p>Ustawianie AzureSqlJobContentDefinition</p>
    </td>
  </tr>
  <tr>
    <td>Docelowej bazy danych</td>
    <td>Nazwa bazy danych i serwera wskazująca bazy danych SQL Azure.

    </td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Nowy AzureSqlJobTarget</p>
    </td>
  </tr>
  <tr>
    <td>Shard Mapa docelowej</td>
    <td>Kombinacja docelowej bazy danych i poświadczenia używane w celu określenia informacje przechowywane w mapę shard elastyczne bazy danych.
    </td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Nowy AzureSqlJobTarget</p>
    <p>Ustawianie AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Docelowy kolekcji niestandardowej</td>
    <td>Grupa zdefiniowane baz danych umożliwia zbiorczo do wykonania.</td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Nowy AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Docelowy podrzędne kolekcji niestandardowej</td>
    <td>Docelowej bazy danych, do którego odwołuje się ze zbioru niestandardowe.</td>
    <td>
    <p>Dodawanie AzureSqlJobChildTarget</p>
    <p>Usuń AzureSqlJobChildTarget</p>
    </td>
  </tr>

<tr>
    <td>Zadanie</td>
    <td>
    <p>Definiowanie parametrów dla zadania, które mogą być używane wyzwalać wykonanie lub do wypełnienia serii rozłożonych w czasie.</p>
    </td>
    <td>
    <p>Get-AzureSqlJob</p>
    <p>Nowy AzureSqlJob</p>
    <p>Ustawianie AzureSqlJob</p>
    </td>
  </tr>

<tr>
    <td>Wykonanie zadania</td>
    <td>
    <p>Kontener zadań, które są niezbędne do zrealizowania wykonywanie skryptu lub stosowanie DACPAC do miejsca docelowego przy użyciu poświadczeń dla połączenia z bazą danych z błędami obsługiwane zgodnie z zasadami wykonanie.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecution</p>
    <p>Rozpocznij AzureSqlJobExecution</p>
    <p>Zatrzymaj AzureSqlJobExecution</p>
    <p>AzureSqlJobExecution oczekiwania</p>
  </tr>

<tr>
    <td>Wykonywanie zadań zadania</td>
    <td>
    <p>Samodzielna jednostka pracy do realizacji zadania.</p>
    <p>Jeśli zadania nie będzie mógł pomyślnie wykonywanie, Wynikowy komunikat wyjątku będą rejestrowane i nowe zadanie pasujące zadanie zostanie utworzona i wykonywane zgodnie z zasad wykonywania określonych.</p></p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecution</p>
    <p>Rozpocznij AzureSqlJobExecution</p>
    <p>Zatrzymaj AzureSqlJobExecution</p>
    <p>AzureSqlJobExecution oczekiwania</p>
  </tr>

<tr>
    <td>Zasady wykonywania zadania</td>
    <td>
    <p>Formanty zadania limity czasu wykonania, ponów próbę ograniczenia i odstępy między ponowne próby.</p>
    <p>Elastyczne zadań baz danych zawiera domyślnych zasad wykonanie zadania, które zasadniczo nieskończonej prób o błędach zadań zadań z wykładniczego wycofywania odstępy między kolejnymi próbami.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecutionPolicy</p>
    <p>Nowy AzureSqlJobExecutionPolicy</p>
    <p>Ustawianie AzureSqlJobExecutionPolicy</p>
    </td>
  </tr>

<tr>
    <td>Harmonogram</td>
    <td>
    <p>Czas podstawie Specyfikacja wykonywania odbywać się w przedziale reoccurring lub jednocześnie.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobSchedule</p>
    <p>Nowy AzureSqlJobSchedule</p>
    <p>Ustawianie AzureSqlJobSchedule</p>
    </td>
  </tr>

<tr>
    <td>Wyzwalacze zadania</td>
    <td>
    <p>Mapowanie między zadania i harmonogram wyzwalacza wykonywania zadania według harmonogramu.</p>
    </td>
    <td>
    <p>Nowy AzureSqlJobTrigger</p>
    <p>Usuń AzureSqlJobTrigger</p>
    </td>
  </tr>
</table>

## <a name="supported-elastic-database-jobs-group-types"></a>Typy grupy obsługiwane zadań elastyczne baz danych
Zadania wykonuje skryptów w języku Transact-SQL (T-SQL) lub stosowania DACPACs całej grupy baz danych. Po przesłaniu zadania do wykonania w obrębie grupy baz danych, zadania "powoduje rozszerzenie" na zadania podrzędnego, gdzie każdy z nich wykonuje wymagane wykonanie jednej bazie danych w grupie. 
 
Istnieją dwa typy grup, które można utworzyć: 

* Grupy [Mapy shard](sql-database-elastic-scale-shard-map-management.md) : gdy zadanie zostaje przesłane do mapy shard, zadanie kwerendy mapy shard, aby określić jego bieżący zestaw odłamki, a następnie zadania dla każdego shard tworzy podrzędny, na mapie shard.
* Grupy niestandardowej zbioru: niestandardowy zdefiniowane baz danych. Gdy zadanie jest przeznaczony dla kolekcji niestandardowej, który tworzy podrzędne zadania dla każdej bazy danych obecnie w kolekcji niestandardowej.

## <a name="to-set-the-elastic-database-jobs-connection"></a>Aby ustawić elastyczne bazy danych zadania połączenia

Połączenie musi być ustawiona z zadań *bazy danych sterowania* przed użyciem zadań interfejsów API. Uruchamianie tego polecenia cmdlet uaktywnia poświadczeń okna wyskakujące żąda nazwę użytkownika i hasło utworzone podczas instalowania zadań elastyczne baz danych. We wszystkich przykładach w tym temacie założono, że pierwsza już została wykonana.

Otwórz połączenie zadań elastyczne baz danych:

    Use-AzureSqlJobConnection -CurrentAzureSubscription 

## <a name="encrypted-credentials-within-the-elastic-database-jobs"></a>Zaszyfrowane poświadczenia w ramach zadań elastyczne baz danych

Poświadczenia bazy danych można wstawiać do zadań *kontrolki bazy danych* z jej hasło szyfrowane. Jest to konieczne do przechowywania poświadczeń, aby włączyć zadań do wykonania w późniejszym czasie (za pomocą harmonogramy zadań).
 
Szyfrowanie działa za pomocą certyfikatu utworzone jako część skryptu instalacji. Skrypt instalacji tworzy i wysyła certyfikat do usługi Azure w chmurze dla odszyfrowywanie przechowywane zaszyfrowane hasła. Usługa w chmurze Azure później przechowuje klucz publiczny w ramach zadań *kontrolki w bazie danych* , które włącza interfejs API programu PowerShell lub klasyczny Portal Azure szyfrowanie dostarczonych hasła bez konieczności certyfikat musi być zainstalowany lokalnie.
 
Hasła poświadczenia są szyfrowane i bezpieczne z użytkownicy mający dostęp tylko do odczytu do obiektów zadań elastyczne bazy danych. Ale jest możliwe złośliwy użytkownik z dostępem do odczytu i zapisu do obiektów nieelastyczne zadań baz danych wyodrębnić hasła. Poświadczenia są przeznaczone do można używać ponownie w wykonania zadania. Poświadczenia są przekazywane do docelowych baz danych podczas nawiązywania połączenia. Obecnie ma żadnych ograniczeń dotyczących docelowej bazy danych używane dla każdego poświadczenia, złośliwy użytkownik może dodać element docelowy bazy danych dla bazy danych w obszarze Kontrola złośliwego użytkownika. Użytkownik może następnie rozpoczęcia zadania kierowanie tej bazy danych do uzyskania hasła poświadczeń.

Najważniejsze wskazówki dotyczące zabezpieczeń dla zadań elastyczne baz danych należą:

* Ogranicz użycie interfejsów API zaufanym osobom.
* Poświadczenia powinien mieć co najmniej uprawnienia niezbędne do wykonania zadania.  Więcej informacji można wyświetlić w tym artykule program SQL Server w witrynie MSDN [Autoryzacja i uprawnienia](https://msdn.microsoft.com/library/bb669084.aspx) .

### <a name="to-create-an-encrypted-credential-for-job-execution-across-databases"></a>Aby utworzyć zaszyfrowaną poświadczeń do wykonywania zadań w bazach danych

Aby utworzyć nowy poświadczeń zaszyfrowanych, polecenia [**cmdlet Get-poświadczeń**](https://technet.microsoft.com/library/hh849815.aspx) monituje o podanie nazwy użytkownika i hasła, które mogą być przekazywane do [**polecenia cmdlet New-AzureSqlJobCredential**](https://msdn.microsoft.com/library/mt346063.aspx).

    $credentialName = "{Credential Name}"
    $databaseCredential = Get-Credential
    $credential = New-AzureSqlJobCredential -Credential $databaseCredential -CredentialName $credentialName
    Write-Output $credential

### <a name="to-update-credentials"></a>Aby zaktualizować poświadczeń

Po zmianie hasła, użyj polecenia [**cmdlet Set-AzureSqlJobCredential**](https://msdn.microsoft.com/library/mt346062.aspx) i ustaw parametr **CredentialName** .

    $credentialName = "{Credential Name}"
    Set-AzureSqlJobCredential -CredentialName $credentialName -Credential $credential 

## <a name="to-define-an-elastic-database-shard-map-target"></a>Aby zdefiniować obiekt docelowy mapy shard elastyczne bazy danych

Aby wykonać zadanie wszystkich bazach danych w zestawie shard (utworzone przy użyciu [Biblioteka klienta nieelastyczne bazy danych](sql-database-elastic-database-client-library.md)), należy użyć mapy shard jako docelowej bazy danych. W tym przykładzie wymaga sharded aplikacji utworzony przy użyciu Biblioteka klienta elastyczne bazy danych. Zobacz [Wprowadzenie do próbki narzędzia elastyczne bazy danych](sql-database-elastic-scale-get-started.md).

Shard Menedżera mapy z bazy danych należy ustawić jako docelowej bazy danych, a następnie mapy określonych shard musi być określony jako elementu docelowego.

    $shardMapCredentialName = "{Credential Name}"
    $shardMapDatabaseName = "{ShardMapDatabaseName}" #example: ElasticScaleStarterKit_ShardMapManagerDb
    $shardMapDatabaseServerName = "{ShardMapServerName}"
    $shardMapName = "{MyShardMap}" #example: CustomerIDShardMap
    $shardMapDatabaseTarget = New-AzureSqlJobTarget -DatabaseName $shardMapDatabaseName -ServerName $shardMapDatabaseServerName
    $shardMapTarget = New-AzureSqlJobTarget -ShardMapManagerCredentialName $shardMapCredentialName -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapDatabaseServerName -ShardMapName $shardMapName
    Write-Output $shardMapTarget

## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Utworzyć skrypt T-SQL, wykonywania wielu baz danych

Podczas tworzenia skryptów T-SQL do wykonania, jest zdecydowanie zalecane, aby utworzyć je [idempotent](https://en.wikipedia.org/wiki/Idempotence) i podatne na błędy. Elastyczne zadań baz danych będzie ponawiał próby wykonywanie skryptu przy każdym wykonywaniu wystąpi błąd, niezależnie od klasyfikacji awarii.

Aby utworzyć i zapisać skrypt do wykonania i ustawianie parametrów **- ContentName** i **- CommandText** za pomocą polecenia [**cmdlet New-AzureSqlJobContent**](https://msdn.microsoft.com/library/mt346085.aspx) .

    $scriptName = "Create a TestTable"

    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
        CREATE TABLE TestTable(
            TestTableId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="create-a-new-script-from-a-file"></a>Tworzenie nowego skryptu z pliku
Jeśli skrypt języka SQL T jest zdefiniowana w pliku, umożliwia importowanie skrypt:

    $scriptName = "My Script Imported from a File"
    $scriptPath = "{Path to SQL File}"
    $scriptCommandText = Get-Content -Path $scriptPath
    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="to-update-a-t-sql-script-for-execution-across-databases"></a>Aby zaktualizować skrypt T-SQL, wykonywania wielu baz danych  

Ten skrypt programu PowerShell aktualizacji tekst istniejący skrypt polecenia T-SQL.

Możesz ustawić następujące zmienne, aby odzwierciedlała definicję odpowiedniej skrypt określone:

    $scriptName = "Create a TestTable"
    $scriptUpdateComment = "Adding AdditionalInformation column to TestTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
    CREATE TABLE TestTable(
        TestTableId INT PRIMARY KEY IDENTITY,
        InsertionTime DATETIME2
    );
    END
    GO

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN
    ALTER TABLE TestTable
    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO"

### <a name="to-update-the-definition-to-an-existing-script"></a>Aby zaktualizować definicji istniejący skrypt

    Set-AzureSqlJobContentDefinition -ContentName $scriptName -CommandText $scriptCommandText -Comment $scriptUpdateComment 

## <a name="to-create-a-job-to-execute-a-script-across-a-shard-map"></a>Aby utworzyć zadanie do wykonania skryptu na mapie shard

Ten skrypt programu PowerShell zaczyna się zadanie do wykonania skryptu w każdej shard na mapie shard elastyczne Skala.

Możesz ustawić następujące zmienne w celu uwzględnienia odpowiedniej skryptu i docelowej:

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $credentialName = "{Credential Name}"
    $shardMapTarget = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName 
    $job = New-AzureSqlJob -ContentName $scriptName -CredentialName $credentialName -JobName $jobName -TargetId $shardMapTarget.TargetId
    Write-Output $job

## <a name="to-execute-a-job"></a>Do wykonania zadania 

Ten skrypt programu PowerShell wykonuje istniejącego zadania:

Aktualizacja następującą zmienną, aby odzwierciedlała nazwę odpowiedniej zadania, która zostały wykonane:

    $jobName = "{Job Name}"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution
 
## <a name="to-retrieve-the-state-of-a-single-job-execution"></a>Aby pobrać stan realizacji pojedynczego zadania

Użyj polecenia [**cmdlet Get-AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346058.aspx) i ustaw parametr **JobExecutionId** , aby wyświetlić stan realizacji zadania.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecution = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId
    Write-Output $jobExecution

Za pomocą samego polecenia cmdlet **Get-AzureSqlJobExecution** z parametrem **IncludeChildren** wyświetlić stan wykonania zadania podrzędnego, takich jak stan określonych dla każdej wykonywania zadań w bazie danych każdego udostępniane przez to zadanie.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="to-view-the-state-across-multiple-job-executions"></a>Aby wyświetlić stan wzdłuż wielu wykonania zadania

Polecenia [**cmdlet Get-AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346058.aspx) ma wiele parametrów, których można używać do wyświetlania wielu wykonania zadania, filtrowane za pośrednictwem podanych parametrach. Poniżej zaprezentowano niektóre możliwe sposoby używania Get-AzureSqlJobExecution:

Pobierz wszystkie wykonania aktywnego najwyższego poziomu zadania:

    Get-AzureSqlJobExecution

Pobierz wszystkie wykonania zadania najwyższego poziomu, w tym Zadanie nieaktywne wykonania:

    Get-AzureSqlJobExecution -IncludeInactive

Pobierz wszystkie wykonania zadania podrzędnego identyfikatora wykonanie zadania dostarczonych wykonania zadania nieaktywne w tym:

    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId –IncludeInactive -IncludeChildren

Wszystkie wykonania zadania utworzone za pomocą serii rozłożonych w czasie pobierania / zadań zestaw łącznie nieaktywne zadania:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive

Pobierz wszystkie zadania kierowanie mapy określonej shard tym nieaktywne zadania:

    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId –IncludeInactive

Pobierz wszystkie zadania kierowanie określonej kolekcji niestandardowej, łącznie z nieaktywne zadania:

    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId –IncludeInactive
 
Pobierz listę zadania wykonania zadania w ramach wykonywania określonych zadań:

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions 

Pobieranie szczegółów wykonywaniu zadań zadania:

Poniższy skrypt programu PowerShell można wyświetlić szczegółowe informacje o wykonywaniu zadań zadania, co jest szczególnie przydatne, gdy debugowanie błędy wykonania.

    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution

## <a name="to-retrieve-failures-within-job-task-executions"></a>Aby pobrać błędy w ramach zadania wykonania zadania

**Obiekt JobTaskExecution** zawiera właściwości do zarządzania cyklem życia zadania, a także właściwości wiadomości. Jeśli wykonanie zadania zadania nie powiodło się, właściwość cyklu życia zostanie ustawiona na *nie powiodło się* i właściwość wiadomość zostanie ustawiona do wyniku komunikat wyjątku i jego stosie. Jeśli nie powiodła się zadanie, jest ważne wyświetlić szczegóły zadania, które nie powiodła się dla danego zadania.

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="to-wait-for-a-job-execution-to-complete"></a>Czekać do wykonywania zadań do wykonania

Poniższy skrypt programu PowerShell mogą służyć czekać na zadania zadanie do wykonania:

    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId 

## <a name="create-a-custom-execution-policy"></a>Tworzenie zasad wykonywanie niestandardowe

Elastyczne zadań baz danych obsługuje tworzenie zasad wykonywanie niestandardowe, które mogą być stosowane podczas uruchamiania zadań.
  
Zasady uruchamiania umożliwia obecnie określających:

* Nazwa: Identyfikator zasad wykonywania.
* Limit czasu zadania: Całkowity czas przed zadanie zostanie anulowana przez elastyczne zadania bazy danych.
* : Początkowa ponów próbę interwał oczekiwania pierwsza próba.
* Maksymalny interwał ponawiania: Zakończenie interwałów ponawiania korzystać.
* Ponowienia współczynnik wycofywania interwał: Współczynnik używanych do obliczania następny interwał między kolejnymi próbami.  Następująca formuła została użyta: (początkowej ponawiania) * Math.pow ((współczynnik wycofywania interwał), (liczba prób) - 2). 
* Maksymalna liczba prób: Maksymalna liczba ponawiania próby wykonania w ramach danego zadania.

Domyślną zasadę wykonanie jest używane następujące wartości:

* Nazwa: Domyślnych wykonanie zasad
* Limit czasu zadania: 1 tydzień
* Początkowej interwał ponawiania: 100 milisekund
* Maksymalna liczba interwał ponawiania: 30 minut
* Spróbuj ponownie współczynnik interwał: 2
* Maksymalna liczba prób: 2,147,483,647

Tworzenie zasad odpowiednie wykonanie:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval 
    -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $executionPolicy

### <a name="update-a-custom-execution-policy"></a>Aktualizowanie zasad wykonywanie niestandardowe

Aktualizacja zasad wykonywania odpowiedniej aktualizacji:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy
 
## <a name="cancel-a-job"></a>Anulowanie zadania

Elastyczne zadań baz danych obsługuje żądania anulowania zadań.  Elastyczne zadań baz danych wykryje żądanie anulowania dla zadania obecnie wykonywane, spróbuje zatrzymać zadanie.

Istnieją dwa różne sposoby, że elastyczne zadań baz danych można wykonywać anulowania:

1. Anuluj obecnie wykonywanych zadań: wykrycie o anulowaniu zadania jest uruchomiona obecnie o anulowaniu wypróbowywania w aktualnie proporcji zadania.  Na przykład: w przypadku długotrwałych kwerendy obecnie wykonywane przy próbie anulowania, będzie próba umożliwia anulowanie zapytania.
2. Anulowanie zadań ponowne próby: Jeśli o anulowaniu zostanie wykryta przez wątek sterowania przed uruchomieniu zadania do wykonania, wątku kontrolki będzie można uniknąć uruchamianie zadania i deklarować żądanie jako anulowane.

Jeśli komunikat o anulowaniu zadania jest wymagany dla zadania nadrzędnego, żądanie anulowania będzie uznane dla zadania nadrzędnego i wszystkie jego zadania podrzędnego.
 
Aby wysłać żądanie anulowania, użyj polecenia [**cmdlet AzureSqlJobExecution Zatrzymaj**](https://msdn.microsoft.com/library/mt346053.aspx) i ustaw parametr **JobExecutionId** .

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="to-delete-a-job-and-job-history-asynchronously"></a>Aby usunąć zadania i Historia zadania asynchroniczne

Elastyczne zadań baz danych obsługuje asynchroniczne usuwania zadań. Zadanie może zostać oznaczony do usunięcia i system usunie to zadanie i jego historii zadań po kroków wszystkie wykonania zadania dla zadania. System nie spowoduje automatyczne anulowanie wykonania aktywnego zadania.  

Wywołania [**AzureSqlJobExecution tabulatora**](https://msdn.microsoft.com/library/mt346053.aspx) , aby anulować wykonania aktywnego zadania.

Aby wyzwolić usuwania zadania, użyj polecenia [**cmdlet AzureSqlJob Usuń**](https://msdn.microsoft.com/library/mt346083.aspx) i ustaw parametr **JobName** .

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
 
## <a name="to-create-a-custom-database-target"></a>Aby utworzyć niestandardową bazę danych cel.

Można określić niestandardową bazę danych celów wykonywania bezpośredni lub do uwzględnienia w grupie niestandardowej bazy danych. Na przykład **pul elastyczne bazy danych** nie są jeszcze bezpośrednio obsługiwane za pomocą programu PowerShell interfejsów API, możesz utworzyć niestandardową bazę danych docelowych i niestandardową bazę danych zbioru obejmującym wszystkie bazy danych w puli.

Możesz ustawić następujące zmienne, aby odzwierciedlała informacje o odpowiedniej bazy danych:

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="to-create-a-custom-database-collection-target"></a>Aby utworzyć niestandardową bazę danych cel kolekcji

Aby zdefiniować docelowej zbioru niestandardową bazę danych, aby umożliwić wykonywanie przez wiele elementów docelowych zdefiniowane bazy danych, należy użyć polecenia cmdlet [**New-AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) . Po utworzeniu grupy bazy danych, baz danych może być skojarzone z docelowej kolekcji niestandardowej.

Możesz ustawić następujące zmienne, aby odzwierciedlała konfiguracji docelowej odpowiedniej kolekcji niestandardowej:

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="to-add-databases-to-a-custom-database-collection-target"></a>Aby dodać bazy danych do elementu docelowego zbioru niestandardową bazę danych

Aby dodać bazy danych do określonej kolekcji niestandardowych użyć polecenia cmdlet [**AzureSqlJobChildTarget Dodaj**](https://msdn.microsoft.comlibrary/mt346064.aspx) .

    $databaseServerName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a>Przeglądanie bazy danych w elemencie docelowym zbiorze niestandardową bazę danych

Użyj polecenia cmdlet [**Get-AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) , aby pobrać baz danych podrzędny w elemencie docelowym zbiorze niestandardową bazę danych. 
 
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a>Utwórz zadanie do wykonania skryptu w docelowej zbioru niestandardową bazę danych

Aby utworzyć zadanie przed grupy bazach danych dla określonego elementu docelowego zbioru niestandardową bazę danych, należy użyć polecenia cmdlet [**AzureSqlJob nowy**](https://msdn.microsoft.com/library/mt346078.aspx) . Elastyczne zadań bazy danych zostanie rozwinąć zadania do wielu zadań podrzędnych każdej odpowiadającej bazy danych skojarzony z docelowego zbioru niestandardową bazę danych i upewnij się, wykonaniu skryptu dla każdej bazy danych. Ponownie należy pamiętać, że skrypty są idempotent być mechanizm do prób.

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a>Zbieranie danych w bazach danych

Za pomocą zadania do wykonania kwerendy w obrębie grupy baz danych i przesyła wyniki do określonej tabeli. Tabelę można wyszukiwać po fakt, aby wyświetlić wyniki kwerendy z każdej bazy danych. Dzięki temu metod asynchronicznych, aby wykonać kwerendę przez wiele baz danych. Niepomyślne są obsługiwane automatycznie przez ponowne próby.

Tabela docelowa określonej będą tworzone automatycznie, jeśli jeszcze nie istnieje. Nowa tabela zastępuje schematu zestaw zwróconych wyników. Jeśli skrypt zwraca wiele zestawów wyników, elastyczne bazy danych zadania wysyła pierwszej do tabeli docelowej.

Poniższy skrypt programu PowerShell wykonuje skrypt i zbiera jej wyniki do określonej tabeli. Ten skrypt przyjęto założenie, że został utworzony skrypt T-SQL który wyświetla zestaw pojedynczy wynik i utworzenia elementu docelowego zbioru niestandardową bazę danych.

Ten skrypt używa polecenia cmdlet [**Get-AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) . Ustawianie parametrów skryptu, poświadczenia i wykonanie docelowej:

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $executionCredentialName = "{Execution Credential Name}"
    $customCollectionName = "{Custom Collection Name}"
    $destinationCredentialName = "{Destination Credential Name}"
    $destinationServerName = "{Destination Server Name}"
    $destinationDatabaseName = "{Destination Database Name}"
    $destinationSchemaName = "{Destination Schema Name}"
    $destinationTableName = "{Destination Table Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName

### <a name="to-create-and-start-a-job-for-data-collection-scenarios"></a>Aby utworzyć i uruchomić zadania dla scenariuszy zbioru danych

Ten skrypt używa polecenia cmdlet [**Start AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346055.aspx) .
 
    $job = New-AzureSqlJob -JobName $jobName 
    -CredentialName $executionCredentialName 
    -ContentName $scriptName 
    -ResultSetDestinationServerName $destinationServerName 
    -ResultSetDestinationDatabaseName $destinationDatabaseName 
    -ResultSetDestinationSchemaName $destinationSchemaName 
    -ResultSetDestinationTableName $destinationTableName 
    -ResultSetDestinationCredentialName $destinationCredentialName 
    -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution

## <a name="to-schedule-a-job-execution-trigger"></a>Aby zaplanować wyzwalacza wykonanie zadania

Poniższy skrypt programu PowerShell umożliwia utworzenie harmonogramu cyklicznego. Ten skrypt używa przedział minuta, ale [**Nowy AzureSqlJobSchedule**](https://msdn.microsoft.com/library/mt346068.aspx) obsługuje także parametry - DayInterval, - HourInterval, - MonthInterval i - WeekInterval. Arkusze, które wykonują tylko raz mogą być tworzone przechodzące przez - jednorazowe.

Utworzenie nowego harmonogramu:

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule 
    -MinuteInterval $minuteInterval 
    -ScheduleName $scheduleName 
    -StartTime $startTime 
    Write-Output $schedule

### <a name="to-trigger-a-job-executed-on-a-time-schedule"></a>Aby wyzwolić zadanie wykonywane zgodnie z harmonogramem czasu

Wyzwalacz zadania można zdefiniować zadania wykonywane zgodnie z harmonogramem. Poniższy skrypt programu PowerShell umożliwia tworzenie wyzwalacza zadania.

Używanie [AzureSqlJobTrigger nowy](https://msdn.microsoft.com/library/mt346069.aspx) i ustaw następujące zmienne odpowiadające im odpowiednie zadanie i harmonogram:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger
    -ScheduleName $scheduleName
    –JobName $jobName
    Write-Output $jobTrigger

### <a name="to-remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a>Aby usunąć zaplanowane skojarzenia, aby zatrzymać zadania wykonywanie zgodnie z harmonogramem

Aby przerwać pojawiał wykonywania zadań za pomocą wyzwalacza zadania, można usunąć wyzwalacza zadania. Usuwanie wyzwalacza zadania, aby zakończyć pracę z wykonywane zgodnie z harmonogramem przy użyciu polecenia [**cmdlet AzureSqlJobTrigger Usuń**](https://msdn.microsoft.com/library/mt346070.aspx).

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger 
    -ScheduleName $scheduleName 
    -JobName $jobName

### <a name="retrieve-job-triggers-bound-to-a-time-schedule"></a>Pobieranie wyzwalacze zadania związane z harmonogramem

Poniższy skrypt programu PowerShell może służyć do wyświetlania wyzwalacze zadania zarejestrowane w harmonogramie określonego czasu.

    $scheduleName = "{Schedule Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -ScheduleName $scheduleName
    Write-Output $jobTriggers

### <a name="to-retrieve-job-triggers-bound-to-a-job"></a>Pobieranie wyzwalacze zadania związane z zadania 

Za pomocą [Get-AzureSqlJobTrigger](https://msdn.microsoft.com/library/mt346067.aspx) do wyświetlania harmonogramów zawierających zarejestrowanych zadania.

    $jobName = "{Job Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -JobName $jobName
    Write-Output $jobTriggers

## <a name="to-create-a-data-tier-application-dacpac-for-execution-across-databases"></a>Aby utworzyć aplikację warstwy danych (DACPAC) do wykonywania wielu baz danych

Aby utworzyć DACPAC, zobacz [aplikacji warstwy danych](https://msdn.microsoft.com/library/ee210546.aspx). Aby wdrożyć DACPAC, użyj polecenia [cmdlet New-AzureSqlJobContent](https://msdn.microsoft.com/library/mt346085.aspx). DACPAC muszą być dostępne usługi. Zalecane jest przekazać utworzonego DACPAC do magazynu Azure i tworzenie [Podpisu dostępu udostępnione](../storage/storage-dotnet-shared-access-signature-part-1.md) dla DACPAC.

    $dacpacUri = "{Uri}"
    $dacpacName = "{Dacpac Name}"
    $dacpac = New-AzureSqlJobContent -DacpacUri $dacpacUri -ContentName $dacpacName 
    Write-Output $dacpac

### <a name="to-update-a-data-tier-application-dacpac-for-execution-across-databases"></a>Aby zaktualizować aplikacji warstwy danych (DACPAC) do wykonywania wielu baz danych

Istniejące DACPACs zarejestrowanych w ramach elastyczne zadań baz danych mogą być aktualizowane, aby wskazywały nowe identyfikatory URI. Użyj polecenia [**cmdlet Set-AzureSqlJobContentDefinition**](https://msdn.microsoft.com/library/mt346074.aspx) , aby zaktualizować identyfikatora URI DACPAC w istniejących DACPAC zarejestrowanych:

    $dacpacName = "{Dacpac Name}"
    $newDacpacUri = "{Uri}"
    $updatedDacpac = Set-AzureSqlJobDacpacDefinition -ContentName $dacpacName -DacpacUri $newDacpacUri
    Write-Output $updatedDacpac

## <a name="to-create-a-job-to-apply-a-data-tier-application-dacpac-across-databases"></a>Aby utworzyć zadanie, aby zastosować aplikacji warstwy danych (DACPAC) we wszystkich bazach danych

Po utworzeniu DACPAC w nieelastyczne zadań baz danych można utworzyć zadanie stosowanie DACPAC w obrębie grupy baz danych. Poniższy skrypt programu PowerShell pozwala utworzyć zadanie DACPAC w kolekcji niestandardowej baz danych:

    $jobName = "{Job Name}"
    $dacpacName = "{Dacpac Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget 
    -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob 
    -JobName $jobName 
    -CredentialName $credentialName 
    -ContentName $dacpacName -TargetId $target.TargetId
    Write-Output $job 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-powershell/cmd-prompt.png
[2]: ./media/sql-database-elastic-jobs-powershell/portal.png
<!--anchors-->
