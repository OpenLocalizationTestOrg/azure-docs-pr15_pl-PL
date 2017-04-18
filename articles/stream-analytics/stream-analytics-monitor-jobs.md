<properties
    pageTitle="programowy monitorowania zadania dotyczące analizy strumieniu | Microsoft Azure"
    description="Dowiedz się, jak programowo monitorowania zadania analizy strumieniu utworzone za pośrednictwem interfejsów API pozostałych, Azure SDK lub programu Powershell."
    keywords=".NET monitora, monitorować zadania, monitorowanie aplikacji"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="programmatically-create-a-stream-analytics-job-monitor"></a>Programowo utworzyć analizy strumieniu monitor zadania
 W tym artykule przedstawiono sposób włączania monitorowania zadania analizy strumieniu. Analizy strumieniu, które wykonywać zadania utworzone za pośrednictwem interfejsów API pozostałych, Azure SDK lub programu Powershell nie mają monitorowania domyślnie włączony.  Można ręcznie włączyć to w Azure Portal Przechodzenie do strony Monitor to zadanie, a następnie klikając przycisk Włącz lub ten proces można zautomatyzować, wykonując czynności opisane w tym artykule. Monitorowanie danych będą widoczne na karcie "Monitor" w Portal Azure dla zadania analizy strumieniu.

![Monitorowanie zadania karty zadania](./media/stream-analytics-monitor-jobs/stream-analytics-monitor-jobs-tab.png)

## <a name="prerequisites"></a>Wymagania wstępne
Przed rozpoczęciem tego artykułu, musisz mieć następujące czynności:

- Program Visual Studio 2012 lub 2013.
- Pobierz i zainstaluj [Zestaw SDK programu .NET Azure](https://azure.microsoft.com/downloads/).
- Istniejące zadanie analizy strumieniu wymagającego monitorowania włączone.

## <a name="setup-a-project"></a>Ustawienia projektu

1.  Tworzenie aplikacji konsoli Visual Studio C# .net.
2.  W konsoli Menedżera pakietów uruchom następujące polecenia, aby zainstalować pakiety NuGet. Pierwszy z nich jest Azure strumienia analizy Management .NET SDK. Drugi jest SDK Monitor Azure, która będzie używana do umożliwienia monitorowania. Ostatnie jest klienta usługi Azure Active Directory, który będzie używany do uwierzytelniania.

    ```
    Install-Package Microsoft.Azure.Management.StreamAnalytics
    Install-Package Microsoft.Azure.Insights -Pre
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

3.  Dodawanie sekcji appSettings poniżej do pliku App.config.

    ```
    <appSettings>
        <!--CSM Prod related values-->
        <add key="ResourceGroupName" value="RESOURCE GROUP NAME" />
        <add key="JobName" value="YOUR JOB NAME" />
        <add key="StorageAccountName" value="YOUR STORAGE ACCOUNT"/>
        <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
        <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
        <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
        <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
        <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
        <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION ID" />
        <add key="ActiveDirectoryTenantId" value="YOUR TENANT ID" />
    </appSettings>
    ```
Zamienianie wartości *SubscriptionId* i *ActiveDirectoryTenantId* z subskrypcją usługi Azure i dzierżawa identyfikatorów. Te wartości można uzyskać, uruchamiając następujące polecenie cmdlet programu PowerShell:

    ```
    Get-AzureAccount
    ```
4.  Dodaj następujący za pomocą instrukcji plik źródłowy (plik Program.cs) w projekcie.

    ```
        using System;
        using System.Configuration;
        using System.Threading;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.Insights;
        using Microsoft.Azure.Management.Insights.Models;
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
    ```
5.  Dodawanie metody uwierzytelniania Pomocnik.

        public static string GetAuthorizationHeader()
            {
                AuthenticationResult result = null;
                var thread = new Thread(() =>
                {
                    try
                    {
                        var context = new AuthenticationContext(
                            ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] +
                            ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

                        result = context.AcquireToken(
                            resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
                            clientId: ConfigurationManager.AppSettings["AsaClientId"],
                            redirectUri: new Uri(ConfigurationManager.AppSettings["RedirectUri"]),
                            promptBehavior: PromptBehavior.Always);
                    }
                    catch (Exception threadEx)
                    {
                        Console.WriteLine(threadEx.Message);
                    }
                });

                thread.SetApartmentState(ApartmentState.STA);
                thread.Name = "AcquireTokenThread";
                thread.Start();
                thread.Join();

                if (result != null)
                {
                    return result.AccessToken;
                }

                throw new InvalidOperationException("Failed to acquire token");
        }

## <a name="create-management-clients"></a>Tworzenie zarządzania klientami
Poniższy kod skonfiguruje niezbędne zmienne i zarządzania klientami.

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());

    Uri resourceManagerUri = new
    Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    // Create Stream Analytics and Insights management client
    StreamAnalyticsManagementClient streamAnalyticsClient = new
    StreamAnalyticsManagementClient(aadTokenCredentials, resourceManagerUri);
    InsightsManagementClient insightsClient = new
    InsightsManagementClient(aadTokenCredentials, resourceManagerUri);

## <a name="enable-monitoring-for-an-existing-stream-analytics-job"></a>Włączanie monitorowania istniejące zadanie analizy strumieniu

Poniższy kod umożliwi monitorowania **istniejące** zadanie analizy strumieniu. Pierwsza część kod wykonuje żądania GET usługi analizy strumieniu pobieranie informacji o zadaniu określonego analizy strumieniu. Używa właściwości "Identyfikator" (pobrane z żądania GET) jako parametr metody położenie w drugiej połowie kodu, który wysyła położenie żądanie usługi wniosków Włącz monitorowanie zadania analizy strumieniu.

> [AZURE.WARNING]
> Jeśli wcześniej włączono monitorowanie różne zakresy analizy strumieniu przez Azure Portal lub programowo przy użyciu poniżej kod **zaleca się podane samą nazwę konta magazynu podczas wcześniej włączone monitorowanie.**
>
> Konto miejsca do magazynowania jest połączone w regionie utworzone zadania analizy strumieniu, nie do samego zadania.
>
> Wszystkie analizy strumieniu zadania (i inne zasoby Azure) w tym samym regionie udostępnianie tego konta miejsca do magazynowania do przechowywania danych monitorowania. Podane konta magazynu innego może spowodować niezamierzone efekty strony do monitorowania inne zadania analizy strumieniu i/lub inne zasoby Azure.
>
> Nazwę konta magazynu używać zamiast ```“<YOUR STORAGE ACCOUNT NAME>”``` poniżej powinny być konta miejsca do magazynowania, który znajduje się w tej samej subskrypcji podczas analizy strumieniu zadania możesz włączenia monitorowania.

    // Get an existing Stream Analytics job
    JobGetParameters jobGetParameters = new JobGetParameters()
    {
        PropertiesToExpand = "inputs,transformation,outputs"
    };
    JobGetResponse jobGetResponse = streamAnalyticsClient.StreamingJobs.Get(resourceGroupName, streamAnalyticsJobName, jobGetParameters);

    // Enable monitoring
    ServiceDiagnosticSettingsPutParameters insightPutParameters = new ServiceDiagnosticSettingsPutParameters()
    {
            Properties = new ServiceDiagnosticSettings()
            {
                StorageAccountName = "<YOUR STORAGE ACCOUNT NAME>"
            }
    };
    insightsClient.ServiceDiagnosticSettingsOperations.Put(jobGetResponse.Job.Id, insightPutParameters);



## <a name="get-support"></a>Uzyskiwanie pomocy technicznej
Aby uzyskać dodatkową pomoc spróbuj nasze [forum Azure analizy strumieniu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Następne kroki

- [Wprowadzenie do analizy strumieniu Azure](stream-analytics-introduction.md)
- [Wprowadzenie do korzystania z analizy strumieniu Azure](stream-analytics-get-started.md)
- [Skalowanie Azure analizy strumieniu zadania](stream-analytics-scale-jobs.md)
- [Dokumentacja języka kwerend analizy strumieniu Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Strumień Azure analizy zarządzania pozostałych interfejsu API odwołania](https://msdn.microsoft.com/library/azure/dn835031.aspx)
