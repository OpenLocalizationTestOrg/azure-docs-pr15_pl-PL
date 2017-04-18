<properties
   pageTitle="Świnka Hadoop za pomocą programu PowerShell w HDInsight | Microsoft Azure"
   description="Dowiedz się, jak przesyłać zadania świnka z klastrem Hadoop na HDInsight przy użyciu programu PowerShell Azure."
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
   ms.date="10/11/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-using-powershell"></a>Wykonywanie zadań świnka przy użyciu programu PowerShell

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

W tym dokumencie znajdują się z przykładem przy użyciu programu PowerShell Azure, aby przesłać zadania świnka do Hadoop w klastrze HDInsight. Świnka umożliwia pisanie MapReduce zadań przy użyciu modeli danych przekształcenia, języka (łaciński świnka), zamiast mapowanie i zmniejszyć funkcje.

> [AZURE.NOTE] Ten dokument nie umożliwia szczegółowy opis wykonaj instrukcje łaciński świnka używane w przykładach. Uzyskać informacji na temat łaciński świnka, w tym przykładzie użyto zobacz [Używanie świnka z Hadoop na HDInsight](hdinsight-use-pig.md).

##<a id="prereq"></a>Wymagania wstępne

Aby wykonać czynności opisane w tym artykule, będą potrzebne następujące elementy.

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Pracy z programem PowerShell Azure**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


##<a id="powershell"></a>Wykonywanie zadań świnka przy użyciu programu PowerShell

Azure programu PowerShell zawiera *polecenia cmdlet* umożliwiają zdalne uruchamiania zadań świnka na HDInsight. Wewnętrznie można to osiągnąć przy użyciu połączenia pozostałych do [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (wcześniej nazywane Templeton) uruchamiania w klastrze HDInsight.

Następujące polecenia cmdlet są używane podczas uruchamiania w klastrze zdalnego HDInsight świnka zadań:

* **AzureRmAccount logowania**: uwierzytelnianie Azure programu PowerShell do subskrypcji usługi Azure

* **Nowy AzureRmHDInsightPigJobDefinition**: tworzy nową *definicję zadania* przy użyciu określonej instrukcji świnka łacińskim

* **Rozpocznij AzureRmHDInsightJob**: wysyła definicji zadania usługi HDInsight, uruchamia zadanie, a następnie zwraca obiekt *zadania* , który może być używany, aby sprawdzić stan zadania

* **Czekaj AzureRmHDInsightJob**: używa obiektu zadania, aby sprawdzić stan zadania. Zostanie on poczekaj, aż ukończeniu zadania lub przekroczono czas oczekiwania.

* **Get-AzureRmHDInsightJobOutput**: używana do pobierania danych wyjściowych zadania

Następująca procedura przedstawia sposób używania tych poleceń cmdlet do uruchamiania w klastrze HDInsight zadania.

1. Za pomocą edytora, Zapisz poniższy kod jako **pigjob.ps1**. **NAZWAKLASTRA** należy zamienić na nazwę klaster HDInsight.

        #Login to your Azure subscription
        Login-AzureRmAccount
        #Get credentials for the admin/HTTPs account
        $creds = Get-Credential

        #Specify the cluster name
        $clusterName = "CLUSTERNAME"
        
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName = $clusterInfo.DefaultStorageAccount.split('.')[0]
        $container = $clusterInfo.DefaultStorageContainer
        $storageAccountKey = (Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value

        #Store the Pig Latin into $QueryString
        $QueryString =  "LOGS = LOAD 'wasbs:///example/data/sample.log';" +
        "LEVELS = foreach LOGS generate REGEX_EXTRACT(`$0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;" +
        "FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;" +
        "GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;" +
        "FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;" +
        "RESULT = order FREQUENCIES by COUNT desc;" +
        "DUMP RESULT;"


        #Create a new HDInsight Pig Job definition
        $pigJobDefinition = New-AzureRmHDInsightPigJobDefinition `
            -Query $QueryString `
            -Arguments "-w"

        # Start the Pig job on the HDInsight cluster
        Write-Host "Start the Pig job ..." -ForegroundColor Green
        $pigJob = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $pigJobDefinition `
            -HttpCredential $creds

        # Wait for the Pig job to complete
        Write-Host "Wait for the Pig job to complete ..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds

        # Display the output of the Pig job.
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
            -ClusterName $clusterName `
            -JobId $pigJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds

2. Otwórz wiersz polecenia programu Windows PowerShell. Zmienianie katalogów do lokalizacji pliku **pigjob.ps1** , a następnie użyj następującego polecenia, aby uruchomić skrypt:

        .\pigjob.ps1
        
    Najpierw należy pojawi się zalogować do subskrypcji usługi Azure. Następnie zostanie wyświetlony monit dla HTTPs-konta nazwy i hasła administratora dla klastrów HDInsight.

7. Po zakończeniu zadania, informacje o powinna zwrócić podobny do następującego:

        Start the Pig job ...
        Wait for the Pig job to complete ...
        Display the standard output ...
        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="troubleshooting"></a>Rozwiązywanie problemów

Jeśli nie informacje są zwracane po zakończeniu zadania, mógł wystąpić błąd podczas przetwarzania. Aby wyświetlić informacje o błędzie dla tego zadania, na końcu pliku **pigjob.ps1** należy dodać następujące polecenie, zapisz go, a następnie uruchom go ponownie.

    # Print the output of the Pig job.
    Write-Host "Display the standard error output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $pigJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Spowoduje to przywrócenie informacje, które zostały zapisane stderr na serwerze po uruchomieniu zadania, a mogą pomóc ustalić, dlaczego zadanie kończy się niepowodzeniem.

##<a id="summary"></a>Podsumowanie

Jak widać, Azure programu PowerShell zawiera łatwym sposobem uruchamiania zadań świnka na klastrze HDInsight, monitorować stan zadania i pobrać dane wyjściowe.

##<a id="nextsteps"></a>Następne kroki

Aby uzyskać ogólne informacje o świnka w HDInsight:

* [Używanie świnka z Hadoop na HDInsight](hdinsight-use-pig.md)

Aby uzyskać informacje o innych sposobach możesz pracować z Hadoop na HDInsight:

* [Gałąź za pomocą Hadoop na HDInsight](hdinsight-use-hive.md)

* [MapReduce za pomocą Hadoop na HDInsight](hdinsight-use-mapreduce.md)
