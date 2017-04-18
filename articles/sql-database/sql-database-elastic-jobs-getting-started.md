<properties
    pageTitle="Wprowadzenie do zadań elastyczne baz danych"
    description="jak używać zadań elastyczne baz danych"
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="ddove"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="ddove" />

# <a name="getting-started-with-elastic-database-jobs"></a>Wprowadzenie do zadań elastyczne baz danych

Elastyczne zadań baz danych (wersja preview) dla bazy danych SQL Azure umożliwia o niezawodności wykonywanie skryptów T-SQL, obejmujące wiele baz danych podczas automatycznie ponawianie próby i ich dostarczania gwarancje ostatecznego zakończenia. Aby uzyskać więcej informacji na temat funkcji zadania elastyczne bazy danych można znaleźć na [stronie Omówienie funkcji](sql-database-elastic-jobs-overview.md).

W tym temacie rozszerza przykładowe znaleziony w [Wprowadzenie do narzędzia elastyczne bazy danych](sql-database-elastic-scale-get-started.md). Po zakończeniu, konieczne będzie: Dowiedz się, jak tworzyć i zarządzanie zadaniami, które zarządzają grupy powiązanych baz danych. Nie jest to wymagane, aby móc skorzystać z zalet elastyczne zadań za pomocą narzędzi elastyczne Skala.

## <a name="prerequisites"></a>Wymagania wstępne

Pobierz i uruchom [wprowadzenie z przykładowymi narzędzia elastyczne bazy danych](sql-database-elastic-scale-get-started.md).

## <a name="create-a-shard-map-manager-using-the-sample-app"></a>Tworzenie shard menedżera map za pomocą aplikacji próbki

Tutaj możesz utworzyć mapę shard manager razem z kilku odłamki, a po nim wstawiania danych do odłamki. Jeśli masz już odłamki Konfigurowanie danymi sharded w nich, możesz pominąć czynności i przejście do następnej sekcji.

1. Tworzenie i uruchamianie aplikacji przykładowej **Wprowadzenie do narzędzia elastyczne bazy danych** . Postępuj zgodnie z instrukcjami, aż do kroku 7 w sekcji [pobrać i uruchomić aplikację próbki](sql-database-elastic-scale-get-started.md#Getting-started-with-elastic-database-tools). Na końcu kroku 7 zostanie wyświetlony następujący wiersz polecenia:

    ![Wiersz polecenia][1]

2.  W oknie polecenia wpisz "1", a następnie naciśnij klawisz **Enter**. Tworzy shard menedżera map i dodaje dwa odłamki na serwerze. Następnie należy wpisać "3" i nacisnąć klawisz **Enter**. Powtórz tę akcję cztery razy. Spowoduje to wstawienie wierszy danych przykładowych usługi odłamki.

3.  [Azure Portal](https://portal.azure.com) powinien wskazywać trzech nowych baz danych znajdujących się na serwerze w wersji 12:

    ![Potwierdzenie programu Visual Studio][2]

    W tym momencie zostanie utworzony kolekcja niestandardową bazę danych, które odzwierciedlają pochodzące wszystkich baz danych na mapie shard. Dzięki temu będzie nam tworzyć i wykonywać zadania, które Dodaj nową tabelę przez odłamki.

W tym miejscu będzie zwykle tworzymy elementu docelowego mapy shard przy użyciu polecenia cmdlet **New-AzureSqlJobTarget** . Bazie danych Menedżera mapy shard musi być ustawiony jako docelowej bazy danych, a następnie na mapie określonych shard określono jako miejsce docelowe. Zamiast tego chwilę wyliczanie wszystkich baz danych na serwerze i dodać bazy danych do nowej kolekcji niestandardowej z wyjątkiem główną bazą danych.

##<a name="creates-a-custom-collection-and-adds-all-databases-in-the-server-to-the-custom-collection-target-with-the-exception-of-master"></a>Tworzy kolekcję niestandardową i dodaje wszystkie bazy danych na serwerze w miejscu docelowym kolekcji niestandardowej, z wyjątkiem wzorca.


    $customCollectionName = "dbs_in_server"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 
    $ResourceGroupName = "ddove_samples"
    $ServerName = "samples"
    $dbsinserver = Get-AzureRMSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName 
    $dbsinserver | %{
    $currentdb = $_.DatabaseName 
    $ErrorActionPreference = "Stop"
    Write-Output ""

    Try
    {
       New-AzureSqlJobTarget -ServerName $ServerName -DatabaseName $currentdb | Write-Output
    }
    Catch 
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason
                
        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already a database target." 
        }

        else
        {
            throw $_
        }
    
    }

    Try
    {
        if ($currentdb -eq "master")
        {
            Write-Host $currentdb "will not be added custom collection target" $CustomCollectionName "."
        }

        else
        {
            Add-AzureSqlJobChildTarget -CustomCollectionName $CustomCollectionName -ServerName $ServerName -DatabaseName $currentdb
            Write-Host $currentdb "was added to" $CustomCollectionName "."
        }

    }
    Catch 
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason

        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already in the custom collection target" $CustomCollectionName"."
        }

        else
        {
            throw $_
        }
    }
    $ErrorActionPreference = "Continue"
}
    
## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Utworzyć skrypt T-SQL, wykonywania wielu baz danych

    $scriptName = "NewTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'Test')
    BEGIN
        CREATE TABLE Test(
            TestId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO Test(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

##<a name="create-the-job-to-execute-a-script-across-the-custom-group-of-databases"></a>Utwórz zadanie do wykonania skryptu całej grupy niestandardowej baz danych

    $jobName = "create on server dbs"
    $scriptName = "NewTable"
    $customCollectionName = "dbs_in_server"
    $credentialName = "ddove66"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job


##<a name="execute-the-job"></a>Wykonywanie zadania 

Poniższy skrypt programu PowerShell może służyć do wykonywania istniejącego zadania:

Aktualizacja następującą zmienną, aby odzwierciedlała nazwę odpowiedniej zadania, która zostały wykonane:

    $jobName = "create on server dbs"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution
 
## <a name="retrieve-the-state-of-a-single-job-execution"></a>Pobieranie stan realizacji pojedynczego zadania

Za pomocą samego polecenia cmdlet **Get-AzureSqlJobExecution** z parametrem **IncludeChildren** wyświetlić stan wykonania zadania podrzędnego, takich jak stan określonych dla każdej wykonywania zadań w bazie danych każdego udostępniane przez to zadanie.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="view-the-state-across-multiple-job-executions"></a>Sprawdzanie stanu u wielu wykonania zadania

Polecenia cmdlet **Get-AzureSqlJobExecution** ma wiele parametrów, których można używać do wyświetlania wielu wykonania zadania, filtrowane za pośrednictwem podanych parametrach. Poniżej przedstawiono niektóre możliwe sposoby używania Get-AzureSqlJobExecution:

Pobierz wszystkie wykonania aktywnego najwyższego poziomu zadania:

    Get-AzureSqlJobExecution

Pobierz wszystkie wykonania zadania najwyższego poziomu, w tym Zadanie nieaktywne wykonania:

    Get-AzureSqlJobExecution -IncludeInactive

Pobierz wszystkie wykonania zadania podrzędnego identyfikatora wykonanie zadania dostarczonych wykonania zadania nieaktywne w tym:

    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId –IncludeInactive -IncludeChildren

Wszystkie wykonania zadania utworzone za pomocą serii rozłożonych w czasie pobierania / zadania zestaw łącznie nieaktywne zadania:

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

## <a name="retrieve-failures-within-job-task-executions"></a>Pobieranie błędy w ramach zadania wykonania zadania

Obiekt JobTaskExecution zawiera właściwości dla zarządzania cyklem życia zadania, a także właściwości wiadomości. Jeśli wykonanie zadania zadania nie powiodło się, właściwość cyklu życia zostanie ustawiona na *nie powiodło się* i właściwość wiadomość zostanie ustawiona do wyniku komunikat wyjątku i jego stosie. Jeśli nie powiodła się zadanie, jest ważne wyświetlić szczegóły zadania, które nie powiodła się dla danego zadania.

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="waiting-for-a-job-execution-to-complete"></a>Oczekiwanie na wykonywania zadań do wykonania

Poniższy skrypt programu PowerShell można oczekiwać zadania zadanie do wykonania:

    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId 

## <a name="create-a-custom-execution-policy"></a>Tworzenie zasad wykonywanie niestandardowe

Elastyczne zadań baz danych obsługuje tworzenie zasad wykonywanie niestandardowe, które mogą być stosowane podczas uruchamiania zadania.
  
Zasady uruchamiania umożliwia obecnie określających:

* Nazwa: Identyfikator zasad wykonywania.
* Limit czasu zadania: Całkowity czas przed zadanie zostanie anulowana przez elastyczne zadania bazy danych.
* : Początkowa ponów próbę interwał oczekiwania pierwsza próba.
* Maksymalny interwał ponawiania: Zakończenie interwałów ponawiania korzystać.
* Ponowienia współczynnik wycofywania interwał: Współczynnik używanych do obliczania następny interwał między kolejnymi próbami.  Następująca formuła została użyta: (początkowej ponawiania) * Math.pow ((interwał wycofywania współczynnik) (liczba prób) - 2). 
* Maksymalna liczba prób: Maksymalna liczba ponawiania próby wykonania w ramach danego zadania.

Domyślną zasadę wykonanie jest używane następujące wartości:

* Nazwa: Domyślnych wykonanie zasad
* Limit czasu zadania: 1 tydzień
* Początkowej interwał ponawiania: 100 milisekund
* Maksymalna liczba interwał ponawiania: 30 minut
* Ponów próbę współczynnik interwał: 2
* Maksymalna liczba prób: 2,147,483,647

Tworzenie zasad odpowiednie wykonanie:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
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

Elastyczne zadań baz danych obsługuje żądania anulowania zadania.  Elastyczne zadań baz danych wykryje żądanie anulowania dla zadania obecnie wykonywane, spróbuje zatrzymać zadanie.

Istnieją dwa różne sposoby, że elastyczne zadań baz danych można wykonywać anulowania:

1. Anulowanie aktualnie wykonywanych zadań: wykrycie o anulowaniu zadania jest uruchomiona obecnie o anulowaniu wypróbowywania w proporcji aktualnie zadania.  Na przykład: w przypadku długotrwałych kwerendy obecnie wykonywane przy próbie anulowania, będzie próba umożliwia anulowanie zapytania.
2. Anulowanie zadań ponowne próby: Jeśli o anulowaniu zostanie wykryta przez wątek sterowania przed uruchomieniu zadania do wykonania, wątku kontrolki będzie unikaj uruchamiania zadania i zadeklarować żądanie jako anulowany.

Jeśli komunikat o anulowaniu zadania jest wymagany dla zadania nadrzędnego, żądanie anulowania będzie uznane dla zadania nadrzędnego i wszystkie jego zadania podrzędnego.
 
Aby wysłać żądanie anulowania, należy użyć polecenia cmdlet **AzureSqlJobExecution tabulatora** i ustaw parametr **JobExecutionId** .

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="delete-a-job-by-name-and-the-jobs-history"></a>Usuwanie zadania według nazwy i historię zatrudnienia

Elastyczne zadań baz danych obsługuje asynchroniczne usuwania zadań. Zadanie może zostać oznaczony do usunięcia i system usunie to zadanie i jego historii zadań po kroków wszystkie wykonania zadania dla zadania. System nie spowoduje automatyczne anulowanie wykonania aktywnego zadania.  

Zamiast tego Zatrzymaj AzureSqlJobExecution muszą być wywoływane anulować wykonania aktywnego zadania.

Aby wyzwolić usuwania zadania, należy użyć polecenia cmdlet **AzureSqlJob Usuń** i ustaw parametr **JobName** .

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
 
## <a name="create-a-custom-database-target"></a>Tworzenie elementu docelowego niestandardową bazę danych
Cele niestandardowej bazie danych można zdefiniować w zadań elastyczne baz danych, które mogą być używane do wykonywania bezpośrednio lub do uwzględnienia w grupie niestandardowej bazy danych. Ponieważ **Pule elastyczne bazy danych** nie są jeszcze bezpośrednio obsługiwane za pośrednictwem interfejsów API programu PowerShell, możesz po prostu utworzyć niestandardową bazę danych docelowych i niestandardową bazę danych zbioru obejmującym wszystkie bazy danych w puli.

Możesz ustawić następujące zmienne, aby odzwierciedlała informacje o odpowiedniej bazy danych:

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="create-a-custom-database-collection-target"></a>Tworzenie elementu docelowego zbioru niestandardową bazę danych
Elementu docelowego zbioru niestandardowej bazie danych można zdefiniować umożliwia włączenie wykonania u wielu elementów docelowych zdefiniowanej bazy danych. Po utworzeniu grupy bazy danych bazy danych można skojarzyć z docelowej kolekcji niestandardowej.

Możesz ustawić następujące zmienne, aby odzwierciedlała konfiguracji docelowej odpowiedniej kolekcji niestandardowej:

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="add-databases-to-a-custom-database-collection-target"></a>Dodawanie baz danych do elementu docelowego zbioru niestandardową bazę danych

Cele bazy danych może być skojarzone z elementów docelowych zbioru niestandardową bazę danych, aby utworzyć grupę baz danych. Gdy zadanie zostanie utworzony jest przeznaczony dla elementu docelowego zbioru niestandardową bazę danych, będzie można rozwinąć kierowania bazy danych związanych z grupy w czasie wykonywania.

Dodaj odpowiednią bazę danych do określonej kolekcji niestandardowej:

    $serverName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a>Przeglądanie bazy danych w elemencie docelowym zbiorze niestandardową bazę danych

Użyj polecenia cmdlet **Get-AzureSqlJobTarget** , aby pobrać baz danych podrzędny w elemencie docelowym zbiorze niestandardową bazę danych. 
 
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a>Utwórz zadanie do wykonania skryptu w docelowej zbioru niestandardową bazę danych

Aby utworzyć zadanie przed grupy bazach danych dla określonego elementu docelowego zbioru niestandardową bazę danych, należy użyć polecenia cmdlet **AzureSqlJob nowy** . Elastyczne zadań bazy danych zostanie rozwinąć zadania do wielu zadań podrzędnych każda odpowiada bazy danych skojarzony z docelowego zbioru niestandardową bazę danych i upewnij się, wykonaniu skryptu dla każdej bazy danych. Ponownie należy pamiętać, że skrypty są idempotent być mechanizm do ponowne próby.

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a>Zbieranie danych w bazach danych

**Elastyczne bazy danych zadania** obsługuje wykonywanie kwerendy w obrębie grupy baz danych i wysyła wyniki do określonej tabeli bazy danych. Tabelę można wyszukiwać po fakt, aby wyświetlić wyniki kwerendy z każdej bazy danych. Dzięki temu mechanizm asynchroniczny, aby wykonać kwerendę przez wiele baz danych. Błąd przypadki jedną z baz danych, jest tymczasowo niedostępny są obsługiwane automatycznie przez ponowne próby.

Tabela docelowa określonej będą tworzone automatycznie, jeśli go jeszcze nie istnieje, pasujących schematu zestaw zwróconych wyników. Jeśli wykonanie skryptu zwraca wiele zestawów wyników, zadań elastyczne baz danych wysyła pierwsza do tabeli docelowej dostarczonych.

Poniższy skrypt programu PowerShell może służyć do wykonywania skryptu zbierania jej wyniki do określonej tabeli. Ten skrypt założono, że utworzono skrypt T-SQL, który wyświetla zestaw pojedynczy wynik i utworzeniu docelowej zbioru niestandardową bazę danych.

Możesz ustawić następujące czynności, aby odzwierciedlała odpowiedniej skrypt, poświadczenia i wykonanie docelowej:

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

### <a name="create-and-start-a-job-for-data-collection-scenarios"></a>Tworzenie i rozpocząć zadanie dla scenariuszy zbioru danych
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $executionCredentialName -ContentName $scriptName -ResultSetDestinationServerName $destinationServerName -ResultSetDestinationDatabaseName $destinationDatabaseName -ResultSetDestinationSchemaName $destinationSchemaName -ResultSetDestinationTableName $destinationTableName -ResultSetDestinationCredentialName $destinationCredentialName -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution

## <a name="create-a-schedule-for-job-execution-using-a-job-trigger"></a>Utworzenie harmonogramu do wykonywania zadań za pomocą wyzwalacza zadania

Poniższy skrypt programu PowerShell umożliwia utworzenie harmonogramu reoccurring. Ten skrypt używa jednego minut, ale nowy AzureSqlJobSchedule obsługuje także parametry - DayInterval, - HourInterval, - MonthInterval i - WeekInterval. Arkusze, które wykonują tylko raz mogą być tworzone przechodzące przez - jednorazowe.

Utworzenie nowego harmonogramu:

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule -MinuteInterval $minuteInterval -ScheduleName $scheduleName -StartTime $startTime 
    Write-Output $schedule

### <a name="create-a-job-trigger-to-have-a-job-executed-on-a-time-schedule"></a>Tworzenie wyzwalacza zlecenia zadania wykonywane na harmonogram

Wyzwalacz zadania można zdefiniować zadania wykonywane zgodnie z harmonogramem. Poniższy skrypt programu PowerShell umożliwia tworzenie wyzwalacza zadania.

Możesz ustawić następujące zmienne odpowiadające im odpowiednie zadanie i harmonogram:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger -ScheduleName $scheduleName –JobName $jobName
    Write-Output $jobTrigger

### <a name="remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a>Usuwanie zaplanowanych skojarzenia, aby zatrzymać zadania wykonywanie zgodnie z harmonogramem

Aby przerwać pojawiał wykonywania zadań za pomocą wyzwalacza zadania, można usunąć wyzwalacza zadania. Usuwanie wyzwalacza zadania, aby zakończyć pracę z wykonywane zgodnie z harmonogramem przy użyciu polecenia cmdlet **AzureSqlJobTrigger Usuń** .

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName





## <a name="import-elastic-database-query-results-to-excel"></a>Importowanie wyników kwerendy elastyczne bazy danych do programu Excel

 Możesz zaimportować wyniki kwerendy do pliku programu Excel.

1. Uruchom program Excel 2013.
2.  Przejdź do wstążki za pomocą **danych** .
3.  Kliknij pozycję **Z innych źródeł** , a następnie kliknij przycisk **Z programu SQL Server**.

    ![Import z programu Excel z innych źródeł][5]
4.  W oknie **Kreatora połączenia danych** wpisz poświadczenia logowania i nazwa serwera. Następnie kliknij przycisk **Dalej**.
5.  W oknie dialogowym **Wybierz bazę danych, która zawiera dane, które chcesz**wybierz bazę danych **ElasticDBQuery** .
6.  Zaznacz tabelę **kontrahenci** w widoku listy, a następnie kliknij przycisk **Dalej**. Następnie kliknij przycisk **Zakończ**.
7.  W formularzu **Importowanie danych** w obszarze **Wybierz sposób wyświetlania tych danych w skoroszycie**zaznacz **tabelę** , a następnie kliknij **przycisk OK**.

Wszystkie wiersze z tabeli **Klienci** , przechowywane w różnych odłamki Wypełnij arkusza programu Excel.

## <a name="next-steps"></a>Następne kroki
Za pomocą funkcji danych programu Excel można teraz. Nawiązywanie połączenia narzędzia integracji BI i danych z elastycznych kwerendy bazy danych za pomocą parametrów połączenia z nazwę serwera, nazwę bazy danych i poświadczenia. Upewnij się, czy program SQL Server jest obsługiwany jako źródła danych narzędzia. Zapoznaj się z elastycznych kwerendy bazy danych i tabele zewnętrzne tak samo jak dowolną inną bazę danych programu SQL Server i tabele programu SQL Server, które chcesz połączyć się z narzędziem.

### <a name="cost"></a>Koszt
Istnieje bez dodatkowych opłat dotyczące korzystania z funkcji kwerendy elastyczne bazy danych. Jednak w tej chwili ta funkcja jest dostępna tylko w bazach danych premium jako punkt końcowy, ale odłamki może być dowolnej wersji usługi.

Aby uzyskać informacje o cenach zobacz [Szczegóły ceny bazy danych SQL](https://azure.microsoft.com/pricing/details/sql-database/).


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
