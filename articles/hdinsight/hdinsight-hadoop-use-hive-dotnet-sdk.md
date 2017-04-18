<properties
    pageTitle="Uruchamianie kwerend gałęzi przy użyciu zestawu SDK .NET HDInsight | Microsoft Azure"
    description="Dowiedz się, jak przesyłać zadania Hadoop do Azure HDInsight Hadoop przy użyciu zestawu SDK .NET HDInsight."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
   ms.date="09/14/2016"
    ms.author="jgao"/>

# <a name="run-hive-queries-using-hdinsight-net-sdk"></a>Uruchamianie kwerend gałęzi przy użyciu zestawu SDK .NET HDInsight

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]


Dowiedz się, jak przesyłać kwerendy gałęzi przy użyciu zestawu SDK .NET HDInsight.

> [AZURE.NOTE] Kroki opisane w tym artykule należy przeprowadzić z klienta systemu Windows. Informacji na temat korzystania z Linux, OS X lub klienta systemu Unix do pracy z gałęzi za pomocą selektor tabulatorów wyświetlane w górnej części tego artykułu.

##<a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego artykułu, musisz mieć następujące czynności:

- **Klaster Hadoop w HDInsight**. Zobacz [Rozpoczynanie pracy z systemem Linux Hadoop w HDInsight](hdinsight-use-sqoop.md#create-cluster-and-sql-database).
- **Program visual Studio 2012 2013 2015-**.

##<a name="submit-hive-queries-using-hdinsight-net-sdk"></a>Przesyłanie kwerend gałęzi przy użyciu zestawu SDK usługi HDInsight .NET

Zestaw SDK programu .NET HDInsight zawiera .NET bibliotek klienta, co pozwala na lepszą współpracę z usługi HDInsight klastrów z .NET. 

**Aby przesłać zadania**

1. Tworzenie aplikacji konsoli C# w programie Visual Studio.
2. Z poziomu konsoli Menedżera pakietów Nuget uruchom następujące polecenie.

        Install-Package Microsoft.Azure.Management.HDInsight.Job

2. Użyj następującego kodu:

        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;

        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;

                private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
                private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
                private const string ExistingClusterUsername = "<Cluster Username>";
                private const string ExistingClusterPassword = "<Cluster User Password>";

                private const string DefaultStorageAccountName = "<Default Storage Account Name>";
                private const string DefaultStorageAccountKey = "<Default Storage Account Key>";
                private const string DefaultStorageContainerName = "<Default Blob Container Name>";

                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");

                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);

                    SubmitHiveJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                private static void SubmitHiveJob()
                {
                    Dictionary<string, string> defines = new Dictionary<string, string> { { "hive.execution.engine", "ravi" }, { "hive.exec.reducers.max", "1" } };
                    List<string> args = new List<string> { { "argA" }, { "argB" } };
                    var parameters = new HiveJobSubmissionParameters
                    {
                        Query = "SHOW TABLES",
                        Defines = defines,
                        Arguments = args
                    };

                    System.Console.WriteLine("Submitting the Hive job to the cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitHiveJob(parameters);
                    var jobId = jobResponse.JobSubmissionJsonResponse.Id;
                    System.Console.WriteLine("Response status code is " + jobResponse.StatusCode);
                    System.Console.WriteLine("JobId is " + jobId);

                    System.Console.WriteLine("Waiting for the job completion ...");

                    // Wait for job completion
                    var jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    while (!jobDetail.Status.JobComplete)
                    {
                        Thread.Sleep(1000);
                        jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    }

                    // Get job output
                    var storageAccess = new AzureStorageAccess(DefaultStorageAccountName, DefaultStorageAccountKey,
                        DefaultStorageContainerName);
                    var output = (jobDetail.ExitValue == 0)
                        ? _hdiJobManagementClient.JobManagement.GetJobOutput(jobId, storageAccess) // fetch stdout output in case of success
                        : _hdiJobManagementClient.JobManagement.GetJobErrorLogs(jobId, storageAccess); // fetch stderr output in case of failure

                    System.Console.WriteLine("Job output is: ");

                    using (var reader = new StreamReader(output, Encoding.UTF8))
                    {
                        string value = reader.ReadToEnd();
                        System.Console.WriteLine(value);
                    }
                }
            }
        }

5. Naciśnij klawisz **F5** , aby uruchomić aplikację.


## <a name="next-steps"></a>Następne kroki

W tym artykule kiedy znasz już istnieje kilka sposobów tworzenia klaster HDInsight. Aby uzyskać więcej informacji, zobacz następujące artykuły:

* [Wprowadzenie do usługi HDInsight platformy Azure][hdinsight-get-started]
* [Tworzenie klastrów Hadoop w HDInsight][hdinsight-provision]
* [Zarządzanie klastrów Hadoop w HDInsight przy użyciu Azure Portal](hdinsight-administer-use-management-portal.md)
* [Odwołanie HDInsight .NET SDK](https://msdn.microsoft.com/library/mt271028.aspx)
* [Świnka korzystanie z usługi HDInsight](hdinsight-use-pig.md)
* [Używanie Sqoop z usługi HDInsight](hdinsight-use-sqoop-mac-linux.md)
* [Tworzenie-interactive uwierzytelniania aplikacji .NET HDInsight](hdinsight-create-non-interactive-authentication-dotnet-applications.md)


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md


