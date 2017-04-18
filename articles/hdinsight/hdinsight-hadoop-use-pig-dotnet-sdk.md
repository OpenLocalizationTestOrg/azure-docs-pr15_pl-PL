<properties
   pageTitle="Świnka Hadoop za pomocą .NET w HDInsight | Microsoft Azure"
   description="Dowiedz się, jak za pomocą zestaw SDK programu .NET dla Hadoop przesyłać zadania świnka do Hadoop na HDInsight."
   services="hdinsight"
   documentationCenter=".net"
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/17/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-using-the-net-sdk-for-hadoop-in-hdinsight"></a>Wykonywanie zadań świnka przy użyciu zestawu SDK .NET dla Hadoop w HDInsight

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Niniejszy dokument zawiera przykład sposób przesyłania zadań świnka Hadoop w klastrze HDInsight za pomocą zestaw SDK programu .NET dla Hadoop.

Zestaw SDK programu .NET HDInsight zawiera .NET klienta bibliotek, które ułatwia Praca z usługi HDInsight klastrów z .NET. Świnka pozwala na tworzenie operacji MapReduce za pomocą modelowania serii przekształcenia danych. Będzie się, jak za pomocą podstawowe aplikacji C# Prześlij zadanie świnka z klastrem HDInsight.

## <a name="prerequisites"></a>Wymagania wstępne

Aby wykonać czynności opisane w tym artykule, będą potrzebne następujące elementy.

* Klaster Azure HDInsight (Hadoop na HDInsight) (Windows systemem Linux lub).
* Program Visual Studio 2012 lub 2013 lub 2015 r.

## <a name="create-the-application"></a>Tworzenie aplikacji

Zestaw SDK programu .NET HDInsight zawiera .NET bibliotek klienta, co pozwala na lepszą współpracę z usługi HDInsight klastrów z .NET. 


1. Otwórz program Visual Studio 2012 lub 2013
2. W menu **plik** wybierz pozycję **Nowy** , a następnie wybierz **Projekt**.
3. Dla nowego projektu wpisz lub wybierz następujące wartości.

    <table>
    <tr>
    <th>Właściwość</th>
    <th>Wartość</th>
    </tr>
    <tr>
    <th>Kategoria</th>
    <th>Szablony i Visual C# i systemu Windows</th>
    </tr>
    <tr>
    <th>Szablon</th>
    <th>Aplikacja konsoli</th>
    </tr>
    <tr>
    <th>Nazwa</th>
    <th>SubmitPigJob</th>
    </tr>
    </table>
4. Kliknij przycisk **OK** , aby utworzyć projekt.
5. W menu **Narzędzia** wybierz **Menedżera pakiet biblioteki** lub **pakiet Nuget**, a następnie wybierz **Konsoli Menedżera pakietów**.
6. Uruchom następujące polecenie w konsoli, aby zainstalować pakiety .NET SDK.

        Install-Package Microsoft.Azure.Management.HDInsight.Job

7. Korzystając z Eksploratora rozwiązanie kliknij dwukrotnie **Plik Program.cs** , aby go otworzyć. Zamień istniejący kod następujące czynności.

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

                    SubmitPigJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                private static void SubmitPigJob()
                {
                    var parameters = new PigJobSubmissionParameters
                    {
                        Query = @"LOGS = LOAD 'wasbs:///example/data/sample.log';
                                    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
                                    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
                                    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
                                    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
                                    RESULT = order FREQUENCIES by COUNT desc;
                                    DUMP RESULT;"
                    };

                    System.Console.WriteLine("Submitting the Pig job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitPigJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }


7. Naciśnij klawisz **F5** , aby uruchomić aplikację.
8. Naciśnij klawisz **ENTER** , aby zakończyć aplikację.

## <a name="summary"></a>Podsumowanie

Jak widać, zestaw SDK programu .NET dla Hadoop umożliwia tworzenie aplikacji .NET, które przesyłać zadania świnka z klastrem HDInsight i monitorować stan zadania.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać ogólne informacje na świnka w HDInsight.

* [Używanie świnka z Hadoop na HDInsight](hdinsight-use-pig.md)

Aby uzyskać informacje na inne sposoby pracy z Hadoop na HDInsight.

* [Gałąź za pomocą Hadoop na HDInsight](hdinsight-use-hive.md)

* [MapReduce korzystanie z Hadoop na HDInsight](hdinsight-use-mapreduce.md) [Podgląd portal]: https://portal.azure.com/
