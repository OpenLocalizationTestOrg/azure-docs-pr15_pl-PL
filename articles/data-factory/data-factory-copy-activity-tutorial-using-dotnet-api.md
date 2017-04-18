<properties 
    pageTitle="Samouczek: Tworzenie potok aktywnością kopii za pomocą interfejsu API .NET | Microsoft Azure" 
    description="W tym samouczku możesz utworzyć potok Azure Factory danych z działaniem kopii za pomocą interfejsu API .NET." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="10/27/2016" 
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-net-api"></a>Samouczek: Tworzenie potok aktywnością kopii za pomocą interfejsu API .NET
> [AZURE.SELECTOR]
- [Omówienie i wymagania wstępne](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Kreator kopii](data-factory-copy-data-wizard-tutorial.md)
- [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Programu Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [Programu PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure szablonu Menedżera zasobów](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [INTERFEJSU API USŁUGI REST](data-factory-copy-activity-tutorial-using-rest-api.md)
- [INTERFEJS API PROGRAMU .NET](data-factory-copy-activity-tutorial-using-dotnet-api.md)


Ten samouczek pokazano, jak tworzyć i monitorowanie factory Azure danych za pomocą interfejsu API .NET. Planowana w factory danych użyto działaniem Kopiuj, aby skopiować dane z magazynem obiektów Blob platformy Azure do bazy danych SQL Azure.

Działanie kopii wykonuje przenoszenia danych w Azure danych Factory. To działanie jest obsługiwany przez ogólnie dostępna usługa, która można kopiować danych między różnych magazynów w sposób bezpieczna, niezawodne i skalowalna. Zobacz artykuł [Działania przepływu danych](data-factory-data-movement-activities.md) szczegółowe informacje na temat działania Kopiuj.   

> [AZURE.NOTE] 
> W tym artykule opisano wszystkie dane Factory .NET API. Zobacz [Data Factory.NET API Reference](https://msdn.microsoft.com/library/mt415893.aspx) szczegółowe informacje na temat SDK .NET Factory danych. 

## <a name="prerequisites"></a>Wymagania wstępne
- Przeglądanie [Omówienie samouczka i wymagania wstępne](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) zapoznaj się z omówieniem samouczka i wykonaj kroki **wymagania wstępne** . 
- Program Visual Studio 2012 lub 2013 lub 2015 r.
- Pobierz i zainstaluj [Zestaw SDK programu .NET Azure](http://azure.microsoft.com/downloads/)
- Azure programu PowerShell. Postępuj zgodnie z instrukcjami w artykule [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) , aby zainstalować Azure programu PowerShell na komputerze. Tworzenie aplikacji usługi Azure Active Directory za pomocą programu PowerShell Azure.

### <a name="create-an-application-in-azure-active-directory"></a>Tworzenie aplikacji w usłudze Active Directory platformy Azure
Tworzenie aplikacji usługi Azure Active Directory, tworzenie wystawcy usługi aplikacji i przypisać go do roli **Współautora Factory danych** .  

1. Uruchamianie **programu PowerShell**. 
1. Uruchom następujące polecenie, a następnie wprowadź nazwę użytkownika i hasło, którego używasz, aby zalogować się do portalu Azure.
    
        Login-AzureRmAccount   
2. Uruchom następujące polecenie, aby wyświetlić wszystkie subskrypcje dla tego konta.

        Get-AzureRmSubscription 
3. Uruchom następujące polecenie, aby Wybierz subskrypcję, którą chcesz pracować z. Zamienianie ** &lt;NameOfAzureSubscription** &gt; z nazwą subskrypcji Azure. 

        Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext

    > [AZURE.IMPORTANT] Zanotuj **SubscriptionId** i **TenantId** z zestawu wyników tego polecenia. 
4. Tworzenie grupy usługi Azure zasobów o nazwie **ADFTutorialResourceGroup** , uruchamiając następujące polecenie w programie PowerShell.  

        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    Jeśli grupa zasobów już istnieje, możesz określić, czy zaktualizować go (Y) lub przechowywana jako (N). 

    Jeśli korzystasz z różnych grupach zasobów, należy użyć nazwy grupy zasobów zamiast ADFTutorialResourceGroup w tym samouczku.
5. Tworzenie aplikacji usługi Azure Active Directory. 

        $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFCopyTutotiralApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfcopytutorialapp.org/example" -Password "Pass@word1"

    Jeśli zostanie wyświetlony następujący komunikat o błędzie, określ innego adresu URL, a następnie ponownie uruchom polecenie. 

        Another object with the same value for property identifierUris already exists.

6. Tworzenie AD wystawcy usługi. 

        New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

7. Dodawanie usługi kapitału do roli **Współautora Factory danych** . 

        New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

8. Uzyskiwanie identyfikatora aplikacji.

        $azureAdApplication

    Zanotuj identyfikator aplikacji (**Identyfikator aplikacji** z zestawu wyników).

Trzeba mieć po czterech wartości z następujących czynności: 

- Identyfikator dzierżawy
- Identyfikator subskrypcji
- Identyfikator aplikacji 
- Hasło (określonego w pierwszym poleceniu)   

## <a name="walkthrough"></a>Instruktaż
1. Przy użyciu programu Visual Studio 2012 2013 2015--, tworzenie aplikacji konsoli C# .NET.
    1. Uruchom program **Visual Studio** 2015-2012-2013.
    2. Kliknij pozycję **plik**, wskaż polecenie **Nowy**, a następnie kliknij pozycję **Projekt**.
    3. Rozwiń listę **Szablony**, a następnie wybierz **Visual C#**. W tym przykładzie użyto C#, ale można używać dowolnego języka .NET.
    4. Wybierz **Aplikację konsoli** z listy typów projektów po prawej stronie.
    5. Wprowadź nazwę **DataFactoryAPITestApp** .
    6. Wybierz **C:\ADFGetStarted** dla lokalizacji.
    7. Kliknij przycisk **OK** , aby utworzyć projekt.
2. Kliknij pozycję **Narzędzia**, wskaż pozycję **Menedżer pakietów Nuget**, a następnie kliknij **Konsoli Menedżera pakietów**.
3.  W **Konsoli Menedżera pakietów**wykonaj następujące czynności: 
    1.  Uruchom następujące polecenie, aby zainstalować pakiet Factory danych:`Install-Package Microsoft.Azure.Management.DataFactories`       
    2.  Uruchom następujące polecenie, aby zainstalować pakiet usługi Azure Active Directory (możesz użyć Active Directory interfejsu API w kodzie):`Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`
6. Dodawanie sekcji poniżej **appSetttings** do pliku **App.config** . Te ustawienia są używane przez metodę Pomocnik: **GetAuthorizationHeader**. 

    Zamienianie wartości dla ** &lt;identyfikator aplikacji&gt;**, ** &lt;hasło&gt;**, ** &lt;identyfikator subskrypcji&gt;**, i ** &lt;dzierżawa identyfikator&gt; ** własne wartościami. 

        <?xml version="1.0" encoding="utf-8" ?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
            </startup>
            <appSettings>
                <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
                <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
                <add key="WindowsManagementUri" value="https://management.core.windows.net/" />

                <add key="ApplicationId" value="your application ID" />
                <add key="Password" value="Password you used while creating the AAD application" />
                <add key="SubscriptionId" value= "Subscription ID" />
                <add key="ActiveDirectoryTenantId" value="Tenant ID" />
            </appSettings>
        </configuration>
6. Dodaj poniższe instrukcje **przy użyciu** z plikiem źródłowym (plik Program.cs) w projekcie.

        using System.Threading;
        using System.Configuration;
        using System.Collections.ObjectModel;
        
        using Microsoft.Azure.Management.DataFactories;
        using Microsoft.Azure.Management.DataFactories.Models;
        using Microsoft.Azure.Management.DataFactories.Common.Models;
        
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure;
6. Dodaj następujący kod, który tworzy wystąpienie klasy **DataPipelineManagementClient** metody **głównego** . Ten obiekt jest używany do tworzenia factory danych, usługi połączone, wejściowe i wyjściowe zestawy danych i potok. Możesz również monitorowanie za pomocą tego obiektu wycinków zestawu danych w czasie rzeczywistym.    

            // create data factory management client
            string resourceGroupName = "ADFTutorialResourceGroup";
            string dataFactoryName = "APITutorialFactory";

            TokenCloudCredentials aadTokenCredentials =
                new TokenCloudCredentials(
                    ConfigurationManager.AppSettings["SubscriptionId"],
                    GetAuthorizationHeader());

            Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

            DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);

    > [AZURE.IMPORTANT] 
    > Zamień wartość **resourceGroupName** nazwę grupy Azure zasobów. 
    > 
    > Aktualizacja nazwy fabryki danych (**dataFactoryName**), aby były unikatowe. Nazwa fabryki danych musi być globalnie unikatowa. Temat [Danych Factory - reguł nazewnictwa](data-factory-naming-rules.md) dla reguł nazewnictwa artefakty Factory danych. 

7. Dodaj następujący kod, który tworzy **factory danych** do metody **głównego** .

            // create a data factory
            Console.WriteLine("Creating a data factory");
            client.DataFactories.CreateOrUpdate(resourceGroupName,
                new DataFactoryCreateOrUpdateParameters()
                {
                    DataFactory = new DataFactory()
                    {
                        Name = dataFactoryName,
                        Location = "westus",
                        Properties = new DataFactoryProperties() { }
                    }
                }
            );

8. Dodaj następujący kod, który tworzy **Magazyn Azure połączone usługi** metody **głównego** . 

    > [AZURE.IMPORTANT] Zamień **storageaccountname** i **accountkey** nazwa i klucz konta magazynu platformy Azure. 

            // create a linked service for input data store: Azure Storage
            Console.WriteLine("Creating Azure Storage linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureStorageLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<storageaccountname>;AccountKey=<accountkey>")
                        )
                    }
                }
            );
9. Dodaj następujący kod, który tworzy **Azure SQL połączone usługi** metody **głównego** .
 
    > [AZURE.IMPORTANT] Zamień **nazwa_serwera**, **databasename**, **nazwy użytkownika**i **hasła** nazwy serwera Azure SQL, bazy danych, użytkownika i hasło.  

            // create a linked service for output data store: Azure SQL Database
            Console.WriteLine("Creating Azure SQL Database linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureSqlLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            new AzureSqlDatabaseLinkedService("Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30")
                        )
                    }
                }
            );
10. Dodaj następujący kod, który tworzy **zestawy danych wejściowe i wyjściowe** metody **głównego** . 

            // create input and output datasets
            Console.WriteLine("Creating input and output datasets");
            string Dataset_Source = "DatasetBlobSource";
            string Dataset_Destination = "DatasetAzureSqlDestination";

            Console.WriteLine("Creating input dataset of type: Azure Blob");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,


            new DatasetCreateOrUpdateParameters()
            {
                Dataset = new Dataset()
                {
                    Name = Dataset_Source,
                    Properties = new DatasetProperties()
                    {
                        Structure = new List<DataElement>()
                        {
                            new DataElement() { Name = "FirstName", Type = "String" },
                            new DataElement() { Name = "LastName", Type = "String" }
                        },
                        LinkedServiceName = "AzureStorageLinkedService",
                        TypeProperties = new AzureBlobDataset()
                        {
                            FolderPath = "adftutorial/",
                            FileName = "emp.txt"
                        },
                        External = true,
                        Availability = new Availability()
                        {
                            Frequency = SchedulePeriod.Hour,
                            Interval = 1,
                        },

                        Policy = new Policy()
                        {
                            Validation = new ValidationPolicy()
                            {
                                MinimumRows = 1
                            }
                        }
                    }
                }
            });


            Console.WriteLine("Creating output dataset of type: Azure SQL");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new DatasetCreateOrUpdateParameters()
                {
                    Dataset = new Dataset()
                    {
                        Name = Dataset_Destination,
                        Properties = new DatasetProperties()
                        {
                            Structure = new List<DataElement>()
                            {
                                new DataElement() { Name = "FirstName", Type = "String" },
                                new DataElement() { Name = "LastName", Type = "String" }
                            },
                            LinkedServiceName = "AzureSqlLinkedService",
                            TypeProperties = new AzureSqlTableDataset()
                            {
                                TableName = "emp"
                            },

                            Availability = new Availability()
                            {
                                Frequency = SchedulePeriod.Hour,
                                Interval = 1,
                            },
                        }
                    }
                });

11. Dodaj następujący kod **tworzy i włącza potok** metody **głównego** . Tego procesu ma **CopyActivity** , która ma **BlobSource** jako źródła i **BlobSink** jako zainstalowania stołu.

            // create a pipeline
            Console.WriteLine("Creating a pipeline");
            DateTime PipelineActivePeriodStartTime = new DateTime(2016, 8, 9, 0, 0, 0, 0, DateTimeKind.Utc);
            DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
            string PipelineName = "ADFTutorialPipeline";

            client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new PipelineCreateOrUpdateParameters()
                {
                    Pipeline = new Pipeline()
                    {
                        Name = PipelineName,
                        Properties = new PipelineProperties()
                        {
                            Description = "Demo Pipeline for data transfer between blobs",

                    // Initial value for pipeline's active period. With this, you won't need to set slice status
                    Start = PipelineActivePeriodStartTime,
                            End = PipelineActivePeriodEndTime,

                            Activities = new List<Activity>()
                            {
                                new Activity()
                                {
                                    Name = "BlobToAzureSql",
                                    Inputs = new List<ActivityInput>()
                                    {
                                        new ActivityInput() {
                                            Name = Dataset_Source
                                        }
                                    },
                                    Outputs = new List<ActivityOutput>()
                                    {
                                        new ActivityOutput()
                                        {
                                            Name = Dataset_Destination
                                        }
                                    },
                                    TypeProperties = new CopyActivity()
                                    {
                                        Source = new BlobSource(),
                                        Sink = new BlobSink()
                                        {
                                            WriteBatchSize = 10000,
                                            WriteBatchTimeout = TimeSpan.FromMinutes(10)
                                        }
                                    }
                                }
                            },
                        }
                    }
                }); 

12. Dodaj następujący kod do metody **główne** Aby sprawdzić stan wycinek dane wyjściowe zestawu danych. Istnieje tylko wycinek poprawnie w tym przykładzie.   
 
            // Pulling status within a timeout threshold
            DateTime start = DateTime.Now;
            bool done = false;

            while (DateTime.Now - start < TimeSpan.FromMinutes(5) && !done)
            {
                Console.WriteLine("Pulling the slice status");
                // wait before the next status check
                Thread.Sleep(1000 * 12);

                var datalistResponse = client.DataSlices.List(resourceGroupName, dataFactoryName, Dataset_Destination,
                    new DataSliceListParameters()
                    {
                        DataSliceRangeStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString(),
                        DataSliceRangeEndTime = PipelineActivePeriodEndTime.ConvertToISO8601DateTimeString()
                    });

                foreach (DataSlice slice in datalistResponse.DataSlices)
                {
                    if (slice.State == DataSliceState.Failed || slice.State == DataSliceState.Ready)
                    {
                        Console.WriteLine("Slice execution is done with status: {0}", slice.State);
                        done = true;
                        break;
                    }
                    else
                    {
                        Console.WriteLine("Slice status is: {0}", slice.State);
                    }
                }
            }

14. Dodaj następujący kod, aby uzyskać uruchamianie szczegóły dotyczące wycinek danych do metody **głównego** .

            Console.WriteLine("Getting run details of a data slice");

            // give it a few minutes for the output slice to be ready
            Console.WriteLine("\nGive it a few minutes for the output slice to be ready and press any key.");
            Console.ReadKey();

            var datasliceRunListResponse = client.DataSliceRuns.List(
                    resourceGroupName,
                    dataFactoryName,
                    Dataset_Destination,
                    new DataSliceRunListParameters()
                    {
                        DataSliceStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString()
                    }
                );

            foreach (DataSliceRun run in datasliceRunListResponse.DataSliceRuns)
            {
                Console.WriteLine("Status: \t\t{0}", run.Status);
                Console.WriteLine("DataSliceStart: \t{0}", run.DataSliceStart);
                Console.WriteLine("DataSliceEnd: \t\t{0}", run.DataSliceEnd);
                Console.WriteLine("ActivityId: \t\t{0}", run.ActivityName);
                Console.WriteLine("ProcessingStartTime: \t{0}", run.ProcessingStartTime);
                Console.WriteLine("ProcessingEndTime: \t{0}", run.ProcessingEndTime);
                Console.WriteLine("ErrorMessage: \t{0}", run.ErrorMessage);
            }

            Console.WriteLine("\nPress any key to exit.");
            Console.ReadKey();

13. Dodaj następujące Pomocnik metodę przy użyciu metody **główne** klasy **programu** .  
 
        public static string GetAuthorizationHeader()
        {
            AuthenticationResult result = null;
            var thread = new Thread(() =>
            {
                try
                {
                    var context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

                    ClientCredential credential = new ClientCredential(ConfigurationManager.AppSettings["ApplicationId"], ConfigurationManager.AppSettings["Password"]);
                    result = context.AcquireToken(resource: ConfigurationManager.AppSettings["WindowsManagementUri"], clientCredential: credential);
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


15. W Eksploratorze rozwiązań rozwiń projektu (**DataFactoryAPITestApp**), kliknij prawym przyciskiem myszy **odwołania**, a następnie kliknij przycisk **Dodaj odwołanie**. Zaznacz pole wyboru dla zestawu "**System.Configuration**", a następnie kliknij **przycisk OK**. 
16. Tworzenie aplikacji konsoli. Kliknij przycisk **Konstruuj** w menu, a następnie kliknij przycisk **Konstruuj rozwiązanie**. 
16. Upewnij się, że istnieje co najmniej jeden plik w kontenerze **adftutorial** w magazynie obiektów blob platformy Azure. W przeciwnym razie utworzyć plik **Emp.txt** w Notatniku o następującej treści i przekaż go do kontenera adftutorial.

        John, Doe
        Jane, Doe
     
17. Uruchamianie próbce, klikając pozycję **Debugowanie** -> **Rozpocznij debugowanie** w menu. Po wyświetleniu **Wprowadzenie Uruchamianie szczegóły wycinek**, poczekaj kilka minut, a następnie naciśnij klawisz **ENTER**. 
18. Sprawdź, czy factory danych **APITutorialFactory** , zostanie utworzony przy użyciu poniższych artefaktów za pomocą portalu Azure: 
    - Usługi połączone: **LinkedService_AzureStorage** 
    - Zestaw danych: **DatasetBlobSource** i **DatasetBlobDestination**.
    - Potok: **PipelineBlobSample** 
18. Upewnij się, że rekordy dwóch pracowników są tworzone w tabeli "**emp**" w określonej bazie danych Azure SQL.

## <a name="next-steps"></a>Następne kroki

- Zapoznaj się z artykułem [Działania przepływu danych](data-factory-data-movement-activities.md) , który zawiera szczegółowe informacje na temat działania kopii używanych w samouczku.
- Zobacz [Data Factory.NET API Reference](https://msdn.microsoft.com/library/mt415893.aspx) szczegółowe informacje na temat SDK .NET Factory danych. W tym artykule opisano wszystkie dane Factory .NET API. 

 
