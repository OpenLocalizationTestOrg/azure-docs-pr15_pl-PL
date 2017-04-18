<properties
    pageTitle="Zarządzanie .NET SDK dla analizy strumieniu | Microsoft Azure"
    description="Rozpoczynanie pracy z SDK .NET zarządzania analizy strumieniu. Dowiedz się, jak skonfigurować i analizy zadań: Tworzenie projektu, nakładów, wyników oraz przekształcenia."
    keywords=".NET SDK, analizy interfejsu API"
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


# <a name="management-net-sdk-set-up-and-run-analytics-jobs-using-the-azure-stream-analytics-api-for-net"></a>Zarządzanie .NET SDK: Konfigurowanie i uruchamianie zadań analizy przy użyciu interfejsu API analizy strumieniu Azure dla środowiska .NET

Dowiedz się, jak skonfigurować zadania wykonywania analizy przy użyciu interfejsu API analizy strumieniu dla .NET przy użyciu zestawu SDK .NET zarządzania. Konfigurowanie projektu, tworzenie źródeł wejściowe i wyjściowe, przekształcenia i Rozpocznij i Zatrzymaj zadania. Dla zadań analizy możesz przesyłać strumieniowo danych z magazynem obiektów Blob lub koncentratora zdarzenia.

Zapoznaj się z [zarządzania Dokumentacja referencyjna dla interfejsu API analizy strumieniu dla środowiska .NET](https://msdn.microsoft.com/library/azure/dn889315.aspx).

Azure analizy strumieniu jest w pełni zarządzanych usług, dostarczając małym opóźnieniem wysokiej dostępności, skalowalna, złożonych przetwarzania przez strumieniowego przesyłania danych w chmurze. Analizy strumieniu umożliwia klientom ustawianie strumieniowego przesyłania zadań do analizowania strumieni danych oraz pozwala na dysku w czasie rzeczywistym analizy.  


## <a name="prerequisites"></a>Wymagania wstępne
Przed rozpoczęciem tego artykułu, musisz mieć następujące czynności:

- Zainstaluj program Visual Studio 2012 lub 2013.
- Pobierz i zainstaluj [Zestaw SDK programu .NET Azure](https://azure.microsoft.com/downloads/).
- Tworzenie grupy zasobów Azure w ramach subskrypcji. Poniżej przedstawiono przykładowy skrypt programu PowerShell Azure. Uzyskać Azure programu PowerShell, zobacz [Instalowanie i konfigurowanie programu PowerShell Azure](../powershell-install-configure.md);  


        # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group
        Select-AzureSubscription -SubscriptionName <subscription name>

            # If Stream Analytics has not been registered to the subscription, remove the remark symbol (#) to run the Register-AzureRMProvider cmdlet to register the provider namespace
            #Register-AzureRMProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>


-   Konfigurowanie źródła danych wejściowych i docelowy format wyjściowy korzystać. Dla dalszego instrukcje, zobacz [Dodawanie wartości wejściowych](stream-analytics-add-inputs.md) skonfigurować przykładowe dane wejściowe i [Wyjściowe Dodawanie](stream-analytics-add-outputs.md) Konfigurowanie przykładowe dane wyjściowe.


## <a name="set-up-a-project"></a>Konfigurowanie projektu

Aby utworzyć zadanie analizy za pomocą interfejsu API analizy strumieniu dla środowiska .NET, najpierw skonfiguruj projektu.

1. Tworzenie aplikacji konsoli Visual Studio C# .NET.
2. W konsoli Menedżera pakietów uruchom następujące polecenia, aby zainstalować pakiety NuGet. Pierwszy z nich jest Azure strumienia analizy Management .NET SDK. Drugi jest klienta usługi Azure Active Directory, który będzie używany do uwierzytelniania.

        Install-Package Microsoft.Azure.Management.StreamAnalytics
        Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory

4. W poniższej sekcji **appSettings** należy dodać do pliku App.config:

        <appSettings>
          <!--CSM Prod related values-->
          <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
          <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
          <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
          <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
          <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
          <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION" />
          <add key="ActiveDirectoryTenantId" value="YOU TENANT ID" />
        </appSettings>


    Zamienianie wartości **SubscriptionId** i **ActiveDirectoryTenantId** z subskrypcją usługi Azure i dzierżawa identyfikatorów. Te wartości można uzyskać, uruchamiając następujące polecenie cmdlet programu PowerShell Azure:

        Get-AzureAccount

5. Dodaj poniższe instrukcje **przy użyciu** z plikiem źródłowym (plik Program.cs) w projekcie:

        using System;
        using System.Configuration;
        using System.Threading;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

6.  Dodaj metodę uwierzytelniania pomocy:

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


## <a name="create-a-stream-analytics-management-client"></a>Tworzenie klienta zarządzania analizy strumieniu

Obiekt **StreamAnalyticsManagementClient** umożliwia zarządzanie zadania i składniki zadania, takie jak danych wejściowych, dane wyjściowe i przekształcania.

Dodaj następujący kod do początku metody **główne** :

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";
    string streamAnalyticsInputName = "<YOUR JOB INPUT NAME>";
    string streamAnalyticsOutputName = "<YOUR JOB OUTPUT NAME>";
    string streamAnalyticsTransformationName = "<YOUR JOB TRANSFORMATION NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());

    // Create Stream Analytics management client
    StreamAnalyticsManagementClient client = new StreamAnalyticsManagementClient(aadTokenCredentials);

Wartość zmiennej **resourceGroupName** powinna być taka sama jak nazwa grupy zasobów, utworzone lub pobrać czynności wstępne.

Aby zautomatyzować proporcji prezentacji poświadczeń miejsc pracy, skorzystaj z [uwierzytelniania wystawcy usługi przy użyciu Menedżera zasobów Azure](../resource-group-authenticate-service-principal.md).

Pozostałej części tego artykułu przyjęto założenie, że kod znajduje się na początku metody **głównego** .

## <a name="create-a-stream-analytics-job"></a>Utwórz zadanie analizy strumieniu

Poniższy kod tworzy zadanie analizy strumieniu w obszarze Grupa zasobów, które zostały zdefiniowane. Dodasz danych wejściowych, dane wyjściowe i przekształcania do zadania później.

    // Create a Stream Analytics job
    JobCreateOrUpdateParameters jobCreateParameters = new JobCreateOrUpdateParameters()
    {
        Job = new Job()
        {
            Name = streamAnalyticsJobName,
            Location = "<LOCATION>",
            Properties = new JobProperties()
            {
                EventsOutOfOrderPolicy = EventsOutOfOrderPolicy.Adjust,
                Sku = new Sku()
                {
                    Name = "Standard"
                }
            }
        }
    };

    JobCreateOrUpdateResponse jobCreateResponse = client.StreamingJobs.CreateOrUpdate(resourceGroupName, jobCreateParameters);


## <a name="create-a-stream-analytics-input-source"></a>Tworzenie źródła wprowadzania analizy strumieniu

Poniższy kod tworzy źródło wprowadzania analizy strumieniu typ źródła danych wejściowych obiektów blob oraz szeregowania CSV. Aby utworzyć źródło wprowadzania Centrum zdarzenia, użyj **EventHubStreamInputDataSource** zamiast **BlobStreamInputDataSource**. Podobnie można dostosować szeregowania typ źródła danych wejściowych.

    // Create a Stream Analytics input source
    InputCreateOrUpdateParameters jobInputCreateParameters = new InputCreateOrUpdateParameters()
    {
        Input = new Input()
        {
            Name = streamAnalyticsInputName,
            Properties = new StreamInputProperties()
            {
                Serialization = new CsvSerialization
                {
                    Properties = new CsvSerializationProperties
                    {
                        Encoding = "UTF8",
                        FieldDelimiter = ","
                    }
                },
                DataSource = new BlobStreamInputDataSource
                {
                    Properties = new BlobStreamInputDataSourceProperties
                    {
                        StorageAccounts = new StorageAccount[]
                        {
                            new StorageAccount()
                            {
                                AccountName = "<YOUR STORAGE ACCOUNT NAME>",
                                AccountKey = "<YOUR STORAGE ACCOUNT KEY>"
                            }
                        },
                        Container = "samples",
                        PathPattern = ""
                    }
                }
            }
        }
    };

    InputCreateOrUpdateResponse inputCreateResponse =
        client.Inputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobInputCreateParameters);

Źródeł danych wejściowych z magazynem obiektów Blob lub koncentratora zdarzenia powiązane są do określonego zadania. Aby użyć tego samego źródła wprowadzania różnych zadań, należy ponownie wywołać metodę i określ nazwę innego zadania.


## <a name="test-a-stream-analytics-input-source"></a>Testowanie źródła wprowadzania analizy strumieniu

Metoda **TestConnection** sprawdza, czy zadanie analizy strumieniu jest mógł połączyć ze źródła danych wejściowych, a także inne aspekty specyficzne dla typu źródła danych wejściowych. Na przykład w obiektów blob źródło wprowadzania utworzony w poprzednim kroku, metodę sprawdza, czy nazwę konta magazynu i pary kluczy może służyć do nawiązania połączenia z kontem miejsca do magazynowania, a także Sprawdź, czy istnieje określonym kontenerze.

    // Test input source connection
    DataSourceTestConnectionResponse inputTestResponse =
        client.Inputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsInputName);

## <a name="create-a-stream-analytics-output-target"></a>Tworzenie analizy strumieniu docelowy format wyjściowy

Tworzenie obiekt docelowy format wyjściowy jest bardzo podobne do tworzenia źródło wprowadzania analizy strumieniu. Przykład źródeł danych wejściowych powiązane są docelowe dla danych wyjściowych do określonego zadania. Aby użyć samego docelowy format wyjściowy dla różnych zadań, należy ponownie wywołać metodę i określ nazwę innego zadania.

Poniższy kod tworzy obiekt docelowy format wyjściowy (baza danych Azure SQL). Możesz dostosować docelowy format wyjściowy typ danych i/lub rodzaj szeregowania.

    // Create a Stream Analytics output target
    OutputCreateOrUpdateParameters jobOutputCreateParameters = new OutputCreateOrUpdateParameters()
    {
        Output = new Output()
        {
            Name = streamAnalyticsOutputName,
            Properties = new OutputProperties()
            {
                DataSource = new SqlAzureOutputDataSource()
                {
                    Properties = new SqlAzureOutputDataSourceProperties()
                    {
                        Server = "<YOUR DATABASE SERVER NAME>",
                        Database = "<YOUR DATABASE NAME>",
                        User = "<YOUR DATABASE LOGIN>",
                        Password = "<YOUR DATABASE LOGIN PASSWORD>",
                        Table = "<YOUR DATABASE TABLE NAME>"
                    }
                }
            }
        }
    };

    OutputCreateOrUpdateResponse outputCreateResponse =
        client.Outputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobOutputCreateParameters);

## <a name="test-a-stream-analytics-output-target"></a>Testowanie analizy strumieniu docelowy format wyjściowy

Docelowy format wyjściowy analizy strumieniu zawiera również metody **TestConnection** testowanie połączeń.

    // Test output target connection
    DataSourceTestConnectionResponse outputTestResponse =
        client.Outputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsOutputName);

## <a name="create-a-stream-analytics-transformation"></a>Tworzenie przekształcenia analizy strumieniu

Poniższy kod tworzy przekształcenie analizy strumieniu z zapytaniem "Wybierz * z wejścia" i określić przydzielić jedną jednostkę przesyłanie strumieniowe zadania analizy strumieniu. Aby uzyskać więcej informacji na dostosowanie przesyłanie strumieniowe jednostki zobacz [analizy strumieniu Azure skala zadania](stream-analytics-scale-jobs.md).


    // Create a Stream Analytics transformation
    TransformationCreateOrUpdateParameters transformationCreateParameters = new TransformationCreateOrUpdateParameters()
    {
        Transformation = new Transformation()
        {
            Name = streamAnalyticsTransformationName,
            Properties = new TransformationProperties()
            {
                StreamingUnits = 1,
                Query = "select * from Input"
            }
        }
    };

    var transformationCreateResp =
        client.Transformations.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, transformationCreateParameters);

Jak dane wejściowe i wyjściowe przekształcenia jest też powiązany z określonego zadania analizy strumieniu, który został utworzony w obszarze.

## <a name="start-a-stream-analytics-job"></a>Rozpocząć zadanie analizy strumieniu
Po utworzeniu zadanie analizy strumieniu i jego input(s), output(s) i przekształcenie, można rozpocząć zadania, dzwoniąc metoda **Start** .

Poniższy przykładowy kod uruchamia zadanie analizy strumieniu czas rozpoczęcia niestandardowy format wyjściowy równa 12 grudnia 2012 12:12:12 UTC:

    // Start a Stream Analytics job
    JobStartParameters jobStartParameters = new JobStartParameters
    {
        OutputStartMode = OutputStartMode.CustomTime,
        OutputStartTime = new DateTime(2012, 12, 12, 0, 0, 0, DateTimeKind.Utc)
    };

    LongRunningOperationResponse jobStartResponse = client.StreamingJobs.Start(resourceGroupName, streamAnalyticsJobName, jobStartParameters);



## <a name="stop-a-stream-analytics-job"></a>Zatrzymaj zadanie analizy strumieniu
Możesz wyłączyć uruchomione zadanie analizy strumieniu, dzwoniąc metoda **Stop** .

    // Stop a Stream Analytics job
    LongRunningOperationResponse jobStopResponse = client.StreamingJobs.Stop(resourceGroupName, streamAnalyticsJobName);

## <a name="delete-a-stream-analytics-job"></a>Usuwanie zadania analizy strumieniu
Metoda **Delete** usunie zadania, a także podrzędne zasobów, takich jak input(s), output(s) i przekształcania zadania.

    // Delete a Stream Analytics job
    LongRunningOperationResponse jobDeleteResponse = client.StreamingJobs.Delete(resourceGroupName, streamAnalyticsJobName);


## <a name="get-support"></a>Uzyskiwanie pomocy technicznej
Aby uzyskać dodatkową pomoc spróbuj nasze [forum Azure analizy strumieniu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Następne kroki

Udało Ci się nauki podstawy używania .NET SDK do tworzenia i uruchamiania zadań analizy. Aby uzyskać więcej informacji, zobacz następujące artykuły:

- [Wprowadzenie do analizy strumieniu Azure](stream-analytics-introduction.md)
- [Wprowadzenie do korzystania z analizy strumieniu Azure](stream-analytics-get-started.md)
- [Skalowanie Azure analizy strumieniu zadania](stream-analytics-scale-jobs.md)
- Zestaw [SDK programu .NET zarządzania analizy strumieniu azure](https://msdn.microsoft.com/library/azure/dn889315.aspx).
- [Dokumentacja języka kwerend analizy strumieniu Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Strumień Azure analizy zarządzania pozostałych interfejsu API odwołania](https://msdn.microsoft.com/library/azure/dn835031.aspx)


<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[azure.blob.storage]: http://azure.microsoft.com/documentation/services/storage/
[azure.blob.storage.use]: http://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/

[azure.event.hubs]: http://azure.microsoft.com/services/event-hubs/
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.forum]: http://go.microsoft.com/fwlink/?LinkId=512151

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.developer.guide]: stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
