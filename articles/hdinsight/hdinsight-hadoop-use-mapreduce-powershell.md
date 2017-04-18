<properties
   pageTitle="Używanie programu PowerShell i MapReduce z Hadoop | Microsoft Azure"
   description="Dowiedz się, jak za pomocą programu PowerShell zdalnie uruchamiania zadań MapReduce z Hadoop na HDInsight."
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
   ms.date="08/29/2016"
   ms.author="larryfr"/>

# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-powershell"></a>Uruchamianie zadań MapReduce z Hadoop na HDInsight przy użyciu programu PowerShell

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

W tym dokumencie znajdują się z przykładem przy użyciu programu PowerShell Azure, aby uruchomić zadanie MapReduce w Hadoop w klastrze HDInsight.

##<a id="prereq"></a>Wymagania wstępne

Aby wykonać czynności opisane w tym artykule, będą potrzebne następujące elementy:

- **Klaster Azure HDInsight (Hadoop na HDInsight) (opartych na systemie Windows lub systemem Linux)**

- **Pracy z programem PowerShell Azure**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

##<a id="powershell"></a>Uruchom zadanie MapReduce przy użyciu programu PowerShell Azure

Azure programu PowerShell zawiera *polecenia cmdlet* umożliwiają zdalne uruchamiania zadań MapReduce na HDInsight. Wewnętrznie można to osiągnąć przy użyciu połączenia pozostałych do [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (wcześniej nazywane Templeton) uruchamiania w klastrze HDInsight.

Następujące polecenia cmdlet są używane podczas wykonywania zadań MapReduce w klastrze HDInsight zdalnym.

* **AzureRmAccount logowania**: uwierzytelnianie Azure programu PowerShell do subskrypcji usługi Azure

* **Nowy AzureRmHDInsightMapReduceJobDefinition**: tworzy nową *definicję zadania* , korzystając z określonego informacji MapReduce

* **Rozpocznij AzureRmHDInsightJob**: wysyła definicji zadania usługi HDInsight, uruchamia zadanie, a następnie zwraca obiekt *zadania* , który może być używany, aby sprawdzić stan zadania

* **AzureRmHDInsightJob oczekiwania**: używa obiektu zadania, aby sprawdzić stan zadania. Oczekuje, aż zakończeniu zadania lub czas oczekiwania został przekroczony.

* **Get-AzureRmHDInsightJobOutput**: używana do pobierania danych wyjściowych zadania

Następująca procedura przedstawia sposób używania tych poleceń cmdlet do uruchamiania w klastrze HDInsight zadania.

1. Za pomocą edytora, Zapisz poniższy kod jako **mapreducejob.ps1**. **NAZWAKLASTRA** należy zamienić na nazwę klaster HDInsight.

        #Specify the values
        $clusterName = "CLUSTERNAME"
                
        # Login to your Azure subscription
        # Is there an active Azure subscription?
        $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            Login-AzureRmAccount
        }

        #Get HTTPS/Admin credentials for submitting the job later
        $creds = Get-Credential
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value

        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        #Define the MapReduce job
        #NOTE: If using an HDInsight 2.0 cluster, use hadoop-examples.jar instead.
        # -JarFile = the JAR containing the MapReduce application
        # -ClassName = the class of the application
        # -Arguments = The input file, and the output directory
        $wordCountJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
            -JarFile "wasbs:///example/jars/hadoop-mapreduce-examples.jar" `
            -ClassName "wordcount" `
            -Arguments `
                "wasbs:///example/data/gutenberg/davinci.txt", `
                "wasbs:///example/data/WordCountOutput"

        #Submit the job to the cluster
        Write-Host "Start the MapReduce job..." -ForegroundColor Green
        $wordCountJob = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $wordCountJobDefinition `
            -HttpCredential $creds

        #Wait for the job to complete
        Write-Host "Wait for the job to complete..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $wordCountJob.JobId `
            -HttpCredential $creds
        # Download the output
        Get-AzureStorageBlobContent `
            -Blob 'example/data/WordCountOutput/part-r-00000' `
            -Container $container `
            -Destination output.txt `
            -Context $context
        # Print the output
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $wordCountJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds
            
2. Otwórz wiersz polecenia **Programu PowerShell Azure** . Zmienianie katalogów do lokalizacji pliku **mapreducejob.ps1** , a następnie użyj następującego polecenia, aby uruchomić skrypt:

        .\mapreducejob.ps1
    
    Po uruchomieniu skrypt, może być monitowany do uwierzytelnienia do subskrypcji usługi Azure. Również zostanie wyświetlony monit o podanie HTTPS-konta nazwy i hasła administratora dla klastrów HDInsight.

3. Po zakończeniu zadania, należy zostanie wyświetlony komunikat podobny do następującego:

        Cluster         : CLUSTERNAME
        ExitCode        : 0
        Name            : wordcount
        PercentComplete : map 100% reduce 100%
        Query           :
        State           : Completed
        StatusDirectory : f1ed2028-afe8-402f-a24b-13cc17858097
        SubmissionTime  : 12/5/2014 8:34:09 PM
        JobId           : job_1415949758166_0071

    Te dane wyjściowe wskazuje pomyślnie wykonano zadanie.

    > [AZURE.NOTE] Jeśli **ExitCode** jest wartość inną niż 0, zobacz [Rozwiązywanie problemów](#troubleshooting).

    W tym przykładzie również będą przechowywane pobrane pliki do pliku **output.txt** w katalogu, w którym możesz uruchomić skrypt z.

###<a name="view-output"></a>Widok danych wyjściowych

Otwórz plik **output.txt** w edytorze tekstów wyrazów i zlicza tworzone przez zadanie.

> [AZURE.NOTE] Pliki wyjściowe zadania MapReduce są niezmienne. Jeśli ponownie w tym przykładzie, należy więc zmienić nazwę pliku wyjściowego.

##<a id="troubleshooting"></a>Rozwiązywanie problemów

Jeśli nie informacje są zwracane po zakończeniu zadania, mógł wystąpić błąd podczas przetwarzania. Aby wyświetlić informacje o błędzie dla tego zadania, na końcu pliku **mapreducejob.ps1** należy dodać następujące polecenie, zapisz go, a następnie uruchom go ponownie.

    # Print the output of the WordCount job.
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $wordCountJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

To zwraca informacje, które zostały zapisane stderr na serwerze po uruchomieniu zadania, a może pomóc ustalić, dlaczego zadanie kończy się niepowodzeniem.

##<a id="summary"></a>Podsumowanie

Jak widać, Azure programu PowerShell zawiera łatwym sposobem uruchamiania zadań MapReduce na klastrze HDInsight, monitorować stan zadania i pobrać dane wyjściowe.

##<a id="nextsteps"></a>Następne kroki

Aby uzyskać ogólne informacje o zadaniach MapReduce w HDInsight:

* [Używanie MapReduce na HDInsight Hadoop](hdinsight-use-mapreduce.md)

Aby uzyskać informacje o innych sposobach możesz pracować z Hadoop na HDInsight:

* [Gałąź za pomocą Hadoop na HDInsight](hdinsight-use-hive.md)

* [Używanie świnka z Hadoop na HDInsight](hdinsight-use-pig.md)
