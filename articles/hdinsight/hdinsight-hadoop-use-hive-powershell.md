<properties
   pageTitle="Gałąź Hadoop za pomocą programu PowerShell w HDInsight | Microsoft Azure"
   description="Uruchamianie kwerend gałęzi w Hadoop na HDInsight za pomocą programu PowerShell."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/07/2016"
   ms.author="larryfr"/>

#<a name="run-hive-queries-using-powershell"></a>Uruchamianie kwerend gałęzi przy użyciu programu PowerShell

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

W tym dokumencie znajdują się z przykładem za pomocą Azure w trybie grupa zasobów Azure uruchamianie kwerend gałęzi w Hadoop w klastrze HDInsight.

> [AZURE.NOTE] Ten dokument nie umożliwia szczegółowy opis wykonaj instrukcje HiveQL, które są używane w przykładach. Aby uzyskać informacji na temat HiveQL, który jest używany w tym przykładzie zobacz [Gałęzi korzystanie z Hadoop na HDInsight](hdinsight-use-hive.md).


**Wymagania wstępne**

Aby wykonać czynności opisane w tym artykule, będą potrzebne następujące elementy.

- **Klaster Azure HDInsight (Hadoop na HDInsight) (opartych na systemie Windows lub systemem Linux)**
- **Pracy z programem PowerShell Azure**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

##<a name="run-hive-queries-using-azure-powershell"></a>Uruchamianie kwerend gałęzi przy użyciu programu PowerShell Azure

Azure programu PowerShell zawiera *polecenia cmdlet* umożliwiają zdalne uruchamianie kwerend gałęzi na HDInsight. Wewnętrznie można to osiągnąć przy użyciu połączenia pozostałych do [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (wcześniej nazywane Templeton) uruchamiania w klastrze HDInsight.

Następujące polecenia cmdlet są używane podczas uruchamiania kwerendy gałęzi w klastrze HDInsight zdalnego:

* **Dodaj AzureRmAccount**: uwierzytelnia Azure programu PowerShell do subskrypcji usługi Azure

* **Nowy AzureRmHDInsightHiveJobDefinition**: tworzy nową *definicję zadania* przy użyciu określonej instrukcji HiveQL

* **Rozpocznij AzureRmHDInsightJob**: wysyła definicji zadania usługi HDInsight, uruchamia zadanie, a następnie zwraca obiekt *zadania* , który może być używany, aby sprawdzić stan zadania

* **Czekaj AzureRmHDInsightJob**: używa obiektu zadania, aby sprawdzić stan zadania. Będzie on poczekaj, aż zakończeniu zadania lub czas oczekiwania został przekroczony.

* **Get-AzureRmHDInsightJobOutput**: używana do pobierania danych wyjściowych zadania

* **Wywołaj AzureRmHDInsightHiveJob**: używanych do uruchamiania instrukcji HiveQL. To blokuje kwerendy wykonuje, a następnie zwraca wynik

* **Używanie AzureRmHDInsightCluster**: Ustawia bieżącą klaster za pomocą polecenia **AzureRmHDInsightHiveJob Wywołaj**

Następująca procedura przedstawia sposób używania tych poleceń cmdlet do uruchamiania w klastrze HDInsight zadania:

1. Za pomocą edytora, Zapisz poniższy kod jako **hivejob.ps1**. **NAZWAKLASTRA** należy zamienić na nazwę klaster HDInsight.

        #Specify the values
        $clusterName = "CLUSTERNAME"
        $creds=Get-Credential

        # Login to your Azure subscription
        # Is there an active Azure subscription?
        $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            Add-AzureRmAccount
        }

        #HiveQL
        #Note: set hive.execution.engine=tez; is not required for
        #      Linux-based HDInsight
        $queryString = "set hive.execution.engine=tez;" +
                    "DROP TABLE log4jLogs;" +
                    "CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';" +
                    "SELECT * FROM log4jLogs WHERE t4 = '[ERROR]';"

        #Create an HDInsight Hive job definition
        $hiveJobDefinition = New-AzureRmHDInsightHiveJobDefinition -Query $queryString 

        #Submit the job to the cluster
        Write-Host "Start the Hive job..." -ForegroundColor Green

        $hiveJob = Start-AzureRmHDInsightJob -ClusterName $clusterName -JobDefinition $hiveJobDefinition -ClusterCredential $creds

        #Wait for the Hive job to complete
        Write-Host "Wait for the job to complete..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob -ClusterName $clusterName -JobId $hiveJob.JobId -ClusterCredential $creds

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        # Print the output
        Write-Host "Display the standard output..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $hiveJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds
            
2. Otwórz wiersz polecenia **Programu PowerShell Azure** . Zmienianie katalogów do lokalizacji pliku **hivejob.ps1** , a następnie użyj następującego polecenia, aby uruchomić skrypt:

        .\hivejob.ps1

    Po uruchomieniu skrypt pojawi o podanie poświadczeń konta HTTPS-administratora dla klaster. Użytkownik może też monitowany się zalogować do subskrypcji usługi Azure.
    
7. Po zakończeniu zadania, informacje o powinna zwrócić podobny do następującego:

        Display the standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. Jak wspomniano wcześniej, **Wywołaj gałąź** można uruchomić kwerendę i Oczekiwanie na odpowiedź. Zamień **NAZWAKLASTRA** nazwę klaster i używać następujących poleceń:

        Use-AzureRmHDInsightCluster -ClusterName $clusterName -HttpCredential $creds
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        $queryString = "set hive.execution.engine=tez;" +
                    "DROP TABLE log4jLogs;" +
                    "CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';" +
                    "SELECT * FROM log4jLogs WHERE t4 = '[ERROR]';"
        Invoke-AzureRmHDInsightHiveJob `
            -StatusFolder "statusout" `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -Query $queryString

    Wynik będzie wyglądać następująco:

        2012-02-03  18:35:34    SampleClass0    [ERROR] incorrect   id
        2012-02-03  18:55:54    SampleClass1    [ERROR] incorrect   id
        2012-02-03  19:25:27    SampleClass4    [ERROR] incorrect   id

    > [AZURE.NOTE] W przypadku dłuższych HiveQL kwerend możesz używać polecenia cmdlet programu PowerShell Azure **Ciągów tutaj** lub pliki skryptów HiveQL. Poniższy fragment pokazano, jak używać polecenia cmdlet **Programu Hive Wywołaj** do przeprowadzania plik skryptu HiveQL. Plik skryptu HiveQL należy przekazać do wasbs: / /.
    >
    > `Invoke-AzureRmHDInsightHiveJob -File "wasbs://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
    >
    > Aby uzyskać więcej informacji na temat **Ciągów tutaj**zobacz <a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">Przy użyciu systemu Windows PowerShell tutaj-ciągów</a>.

##<a name="troubleshooting"></a>Rozwiązywanie problemów

Jeśli nie informacje są zwracane po zakończeniu zadania, mógł wystąpić błąd podczas przetwarzania. Aby wyświetlić informacje o błędzie dla tego zadania, Dodaj następujący tekst na końcu pliku **hivejob.ps1** , zapisz go, a następnie uruchom go ponownie.

    # Print the output of the Hive job.
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

To zwraca informacje, które są zapisywane STDERR na serwerze po uruchomieniu zadania, a może pomóc ustalić, dlaczego zadanie kończy się niepowodzeniem.

##<a name="summary"></a>Podsumowanie

Jak widać, Azure programu PowerShell umożliwia łatwe uruchamianie kwerend gałęzi w klastrze HDInsight, monitorować stan zadania i pobrać dane wyjściowe.

##<a name="next-steps"></a>Następne kroki

Aby uzyskać ogólne informacje na temat gałąź w HDInsight:

* [Gałąź za pomocą Hadoop na HDInsight](hdinsight-use-hive.md)

Aby uzyskać informacje o innych sposobach możesz pracować z Hadoop na HDInsight:

* [Używanie świnka z Hadoop na HDInsight](hdinsight-use-pig.md)

* [MapReduce za pomocą Hadoop na HDInsight](hdinsight-use-mapreduce.md)
