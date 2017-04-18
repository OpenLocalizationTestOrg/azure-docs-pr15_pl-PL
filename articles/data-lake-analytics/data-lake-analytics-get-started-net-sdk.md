<properties 
   pageTitle="Wprowadzenie do analiz Lake danych Azure za pomocą .NET SDK | Azure" 
   description="Dowiedz się, jak utworzyć konto w sklepie Lake danych, tworzenie zadań analizy Lake danych za pomocą .NET SDK i przesyłać zadania napisane w języku SQL U. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/26/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-net-sdk"></a>Samouczek: wprowadzenie do analiz Lake danych Azure za pomocą .NET SDK

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


Dowiedz się, jak przesyłać zadania zapisywane w [Języku SQL U](data-lake-analytics-u-sql-get-started.md) analizy Lake danych za pomocą Azure .NET SDK. Aby uzyskać więcej informacji na temat analizy Lake danych zobacz [Omówienie analizy Lake danych Azure](data-lake-analytics-overview.md).

W tym samouczku będzie rozwijanie aplikacji konsoli C# przesłać zadanie U SQL, który odczytuje karty pliku wartości (TSV) i konwertuje ją do pliku w formacie CSV wartości przecinkami (CSV). Do wykonywania kroków samej samouczka przy użyciu innych obsługiwanych narzędzi, kliknij karty u góry tego artykułu.

##<a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego samouczka, musisz mieć następujące czynności:

- **Visual Studio 2015, Visual Studio 2013 aktualizacja 4, lub program Visual Studio 2012 ze zainstalowany program Visual C++**.
- **Microsoft Azure SDK dla .NET wersji 2.5 lub nowszej**.  Zainstaluj go za pomocą [Instalatora platformy sieci Web](http://www.microsoft.com/web/downloads/platform.aspx).
- **Konto Azure danych Lake analizy**. Zobacz [Zarządzanie danymi Lake analizy przy użyciu zestawu SDK .NET Azure](data-lake-analytics-manage-use-dotnet-sdk.md).

##<a name="create-console-application"></a>Tworzenie aplikacji konsoli

W tym samouczku proces niektóre dzienniki wyszukiwania.  Dziennik wyszukiwania mogą być przechowywane w magazynie Lake danych lub magazyn obiektów Blob platformy Azure. 

Przykładowy dziennik wyszukiwania znajduje się w kontenerze obiektów Blob platformy Azure publicznej. W aplikacji będzie pobrać plik na miejscu pracy, a następnie przekaż plik do domyślnego konta magazynu Lake danych konta analizy Lake danych.

**Aby utworzyć skrypt U SQL**

Zadania analizy Lake danych są zapisywane w języku U SQL. Aby dowiedzieć się więcej na temat U SQL, zobacz [Wprowadzenie do języka U SQL](data-lake-analytics-u-sql-get-started.md) i [Dokumentacja języka U SQL](http://go.microsoft.com/fwlink/?LinkId=691348).

Tworzenie pliku **SampleUSQLScript.txt** z tego skryptu U SQL i umieścić go w **C:\temp\* * ścieżki.  Ścieżka jest ustalony w aplikacji .NET, które są tworzone w następnej procedurze.  

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();
    
    OUTPUT @searchlog   
        TO "/Output/SearchLog-from-Data-Lake.csv"
    USING Outputters.Csv();

Ten skrypt U SQL odczytuje plik źródła danych przy użyciu **Extractors.Tsv()**, a następnie utworzy plik csv przy użyciu **Outputters.Csv()**. 

W programie C# należy przygotować plik **/Samples/Data/SearchLog.tsv** i **/Output/** folder.    

Jest prostsze Użyj ścieżki względne do plików przechowywanych w domyślnych danych Lake konta. Można także używać ścieżek bezwzględnych.  Na przykład 

    adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
    
Aby uzyskać dostęp do plików w połączonych kont miejsca do magazynowania, należy użyć ścieżki bezwzględne.  Składnia plików przechowywanych w połączony klient magazyn Azure jest następująca:

    wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

>[AZURE.NOTE] Obecnie jest to znany problem z usługą Azure danych Lake.  Jeśli aplikacja przykładowa zostanie przerwany lub wystąpi błąd, może być konieczne ręczne usuwanie konta magazynu Lake danych i analizy Lake danych, które tworzy skrypt.  Jeśli nie znasz Portal Azure, przewodnik [Zarządzanie analizy Lake danych Azure za pomocą portalu Azure](data-lake-analytics-manage-use-portal.md) spowoduje rozpoczęcie pracy.       

**Aby utworzyć aplikację**

1. Otwórz program Visual Studio.
2. Tworzenie aplikacji konsoli C#.
3. Otwórz konsolę zarządzania pakiet NuGet, a następnie uruchom następujące polecenia:

        Install-Package Microsoft.Azure.Management.DataLake.Analytics -Pre
        Install-Package Microsoft.Azure.Management.DataLake.Store -Pre
        Install-Package Microsoft.Azure.Management.DataLake.StoreUploader -Pre
        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
        Install-Package WindowsAzure.Storage


       
5. W polu Plik Program.cs Wklej następujący kod:

        using System;
        using System.IO;
        using System.Collections.Generic;
        using System.Threading;
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.StoreUploader;
        using Microsoft.Azure.Management.DataLake.Analytics;
        using Microsoft.Azure.Management.DataLake.Analytics.Models;
        using Microsoft.WindowsAzure.Storage.Blob;

        namespace SdkSample
        {
          class Program
          {
            private const string SUBSCRIPTIONID = "<Enter Your Azure Subscription ID>";
            private const string CLIENTID = "1950a258-227b-4e31-a9cf-717495945fc2";
            private const string DOMAINNAME = "common"; // Replace this string with the user's Azure Active Directory tenant ID or domain name, if needed.

            private static string _adlaAccountName = "<Enter an Existing Data Lake Analytics Account Name>";
            private static string _adlsAccountName = "<Enter the default Data Lake Store Account Name>";

            private static DataLakeAnalyticsAccountManagementClient _adlaClient;
            private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;
            private static DataLakeAnalyticsJobManagementClient _adlaJobClient;
        
            private static void Main(string[] args)
            {
                string localFolderPath = @"c:\temp\";

                // Connect to Azure
                var creds = AuthenticateAzure(DOMAINNAME, CLIENTID);

                SetupClients(creds, SUBSCRIPTIONID);

                // Transfer the source file from a public Azure Blob container to Data Lake Store.
                CloudBlockBlob blob = new CloudBlockBlob(new Uri("https://adltutorials.blob.core.windows.net/adls-sample-data/SearchLog.tsv"));
                blob.DownloadToFile(localFolderPath + "SearchLog.tsv", FileMode.Create); // from WASB
                UploadFile(localFolderPath + "SearchLog.tsv", "/Samples/Data/SearchLog.tsv"); // to ADLS
                WaitForNewline("Source data file prepared.", "Submitting a job.");


                // Submit the job
                Guid jobId = SubmitJobByPath(localFolderPath + "SampleUSQLScript.txt", "My First ADLA Job");
                WaitForNewline("Job submitted.", "Waiting for job completion.");

                // Wait for job completion
                WaitForJob(jobId);
                WaitForNewline("Job completed.", "Downloading job output.");

                // Download job output
                DownloadFile(@"/Output/SearchLog-from-Data-Lake.csv", localFolderPath + "SearchLog-from-Data-Lake.csv");
        
                WaitForNewline("Job output downloaded. You can now exit.");
            }
        
            public static ServiceClientCredentials AuthenticateAzure(
                string domainName,
                string nativeClientAppCLIENTID)
            {
                // User login via interactive popup
                SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
                // Use the client ID of an existing AAD "Native Client" application.
                var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientAppCLIENTID, new Uri("urn:ietf:wg:oauth:2.0:oob"));
                return UserTokenProvider.LoginWithPromptAsync(domainName, activeDirectoryClientSettings).Result;
            }

            public static void SetupClients(ServiceClientCredentials tokenCreds, string subscriptionId)
            {
                _adlaClient = new DataLakeAnalyticsAccountManagementClient(tokenCreds);
                _adlaClient.SubscriptionId = subscriptionId;

                _adlaJobClient = new DataLakeAnalyticsJobManagementClient(tokenCreds);

                _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(tokenCreds);
            }

            public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
            {
                var parameters = new UploadParameters(srcFilePath, destFilePath, _adlsAccountName, isOverwrite: force);
                var frontend = new DataLakeStoreFrontEndAdapter(_adlsAccountName, _adlsFileSystemClient);
                var uploader = new DataLakeStoreUploader(parameters, frontend);
                uploader.Execute();
            }

            public static void DownloadFile(string srcPath, string destPath)
            {
                var stream = _adlsFileSystemClient.FileSystem.Open(_adlsAccountName, srcPath);
                var fileStream = new FileStream(destPath, FileMode.Create);

                stream.CopyTo(fileStream);
                fileStream.Close();
                stream.Close();
            }

            // Helper function to show status and wait for user input
            public static void WaitForNewline(string reason, string nextAction = "")
            {
                Console.WriteLine(reason + "\r\nPress ENTER to continue...");

                Console.ReadLine();

                if (!String.IsNullOrWhiteSpace(nextAction))
                    Console.WriteLine(nextAction);
            }


            // List all Data Lake Analytics accounts within the subscription
            public static List<DataLakeAnalyticsAccount> ListADLAAccounts()
            {
                var response = _adlaClient.Account.List();
                var accounts = new List<DataLakeAnalyticsAccount>(response);

                while (response.NextPageLink != null)
                {
                    response = _adlaClient.Account.ListNext(response.NextPageLink);
                    accounts.AddRange(response);
                }

                Console.WriteLine("You have %i Data Lake Analytics account(s).", accounts.Count);
                for (int i = 0; i < accounts.Count; i++)
                {
                    Console.WriteLine(accounts[i].Name);
                }

                return accounts;
            }
            public static Guid SubmitJobByPath(string scriptPath, string jobName)
            {
                var script = File.ReadAllText(scriptPath);

                var jobId = Guid.NewGuid();
                var properties = new USqlJobProperties(script);
                var parameters = new JobInformation(jobName, JobType.USql, properties, priority: 1, degreeOfParallelism: 1, jobId: jobId);
                var jobInfo = _adlaJobClient.Job.Create(_adlaAccountName, jobId, parameters);

                return jobId;
            }

            public static JobResult WaitForJob(Guid jobId)
            {
                var jobInfo = _adlaJobClient.Job.Get(_adlaAccountName, jobId);
                while (jobInfo.State != JobState.Ended)
                {
                    jobInfo = _adlaJobClient.Job.Get(_adlaAccountName, jobId);
                }
                return jobInfo.Result.Value;
            }
          }
        }

6. Naciśnij klawisz **F5** , aby uruchomić aplikację. Dane wyjściowe są takie jak:

    ![Wyjściowy SDK .NET U SQL Azure danych Lake analizy zadania](./media/data-lake-analytics-get-started-net-sdk/data-lake-analytics-dotnet-job-output.png)

7. Zaznacz plik docelowy.  Domyślne ścieżkę i nazwę pliku jest c:\Temp\SearchLog-from-Data-Lake.csv.

## <a name="see-also"></a>Zobacz też

- Aby wyświetlić samej samouczka przy użyciu innych narzędzi, kliknij selektory kartę w górnej części strony.
- Aby uzyskać bardziej złożonych kwerend, zobacz [Dzienniki analiza witryny sieci Web przy użyciu analizy Lake danych Azure](data-lake-analytics-analyze-weblogs.md).
- Aby rozpocząć tworzenie aplikacji U SQL, zobacz [skryptów można opracowywać U-SQL przy użyciu narzędzia Lake danych dla programu Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Aby dowiedzieć się U SQL, zobacz [Wprowadzenie do języka Azure danych Lake analizy U-SQL](data-lake-analytics-u-sql-get-started.md)i [Dokumentacja języka U SQL](http://go.microsoft.com/fwlink/?LinkId=691348).
- Do zadań zarządzania zobacz [Zarządzanie analizy Lake danych Azure za pomocą portalu Azure](data-lake-analytics-manage-use-portal.md).
- Aby uzyskać omówienie analizy Lake danych, zobacz [Omówienie analizy Lake danych Azure](data-lake-analytics-overview.md).
