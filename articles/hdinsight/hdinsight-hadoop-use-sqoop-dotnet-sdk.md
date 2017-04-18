<properties
    pageTitle="Używanie Hadoop Sqoop w HDInsight | Microsoft Azure"
    description="Dowiedz się, jak za pomocą HDInsight .NET SDK uruchamianie Sqoop importowanie i eksportowanie między klaster Hadoop i bazy danych programu Azure SQL."
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

#<a name="run-sqoop-jobs-using-net-sdk-for-hadoop-in-hdinsight"></a>Wykonywanie zadań Sqoop przy użyciu zestawu SDK usługi .NET dla Hadoop w HDInsight

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Dowiedz się, jak za pomocą HDInsight .NET SDK wykonywanie zadań Sqoop HDInsight do importowania i eksportowania między HDInsight klaster i baza danych Azure SQL lub bazy danych programu SQL Server.

> [AZURE.NOTE] Kroki opisane w tym artykule można używać za pomocą programu Windows lub Linux HDInsight klastrze; Jednak te kroki działa tylko w kliencie systemu Windows. Aby wybrać inne metody za pomocą selektor tabulatorów w górnej części tego artykułu.

###<a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego samouczka, musisz mieć następujące czynności:

- **Klaster Hadoop w HDInsight**. Zobacz [Utwórz klaster i baza danych SQL](hdinsight-use-sqoop.md#create-cluster-and-sql-database).

## <a name="run-sqoop-using-net-sdk"></a>Uruchamianie Sqoop przy użyciu zestawu SDK .NET

Zestaw SDK programu .NET HDInsight zawiera .NET bibliotek klienta, co pozwala na lepszą współpracę z usługi HDInsight klastrów z .NET. W tej sekcji utworzysz aplikacji konsoli C# do wyeksportowania hivesampletable do tabeli bazy danych SQL, utworzony wcześniej w tym samouczki.

**Aby przesłać zadanie Sqoop**

1. Tworzenie aplikacji konsoli C# w programie Visual Studio.
2. Z konsoli Menedżera pakietów Visual Studio uruchom następujące polecenie Nuget, aby zaimportować pakietu.

        Install-Package Microsoft.Azure.Management.HDInsight.Job
        
3. W pliku Plik Program.cs, należy użyć następującego kodu:

        using System.Collections.Generic;
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
        
                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");
        
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
        
                    SubmitSqoopJob();
        
                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }
        
                private static void SubmitSqoopJob()
                {
                    var sqlDatabaseServerName = "<SQLDatabaseServerName>";
                    var sqlDatabaseLogin = "<SQLDatabaseLogin>";
                    var sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>";
                    var sqlDatabaseDatabaseName = "<DatabaseName>";
        
                    var tableName = "<TableName>";
                    var exportDir = "/tutorials/usesqoop/data";
        
                    // Connection string for using Azure SQL Database.
                    // Comment if using SQL Server
                    var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ".database.windows.net;user=" + sqlDatabaseLogin + "@" + sqlDatabaseServerName + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
                    // Connection string for using SQL Server.
                    // Uncomment if using SQL Server
                    //var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ";user=" + sqlDatabaseLogin + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
        
                    var parameters = new SqoopJobSubmissionParameters
                    {
                        Files = new List<string> { "/user/oozie/share/lib/sqoop/sqljdbc41.jar" }, // This line is required for Linux-based cluster.
                        Command = "export --connect " + connectionString + " --table " + tableName + "_mobile --export-dir " + exportDir + "_mobile --fields-terminated-by \\t -m 1"
                    };
        
                    System.Console.WriteLine("Submitting the Sqoop job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitSqoopJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }
        
4. Naciśnij klawisz **F5** , aby uruchomić program. 

##<a name="limitations"></a>Ograniczenia

* Zbiorcze export - systemem Linux z usługi HDInsight, łącznik Sqoop umożliwia eksportowanie danych do programu Microsoft SQL Server lub bazy danych SQL Azure nie obsługuje obecnie wstawia zbiorczo.

* Tworzeniu partii — z systemem Linux HDInsight podczas korzystania z `-batch` przełączanie podczas wykonywania instrukcji INSERT, Sqoop wykona wielu wstawia zamiast tworzeniu partii operacje wstawiania.

##<a name="next-steps"></a>Następne kroki

Teraz zapoznaniu umożliwia Sqoop. Aby uzyskać więcej informacji, zobacz:

- [Oozie korzystanie z usługi HDInsight](hdinsight-use-oozie.md): Sqoop użyj akcji w przepływie pracy Oozie.
- [Analiza lotu opóźnienie danych przy użyciu HDInsight](hdinsight-analyze-flight-delay-data.md): używanie gałęzi do analizowania lotu opóźnienia danych, a następnie użyj Sqoop Aby wyeksportować dane do bazy danych programu Azure SQL.
- [Przekazywanie danych do HDInsight](hdinsight-upload-data.md): znajdowanie innych metod przekazywania danych z magazynem obiektów Blob usługi HDInsight/Azure.


