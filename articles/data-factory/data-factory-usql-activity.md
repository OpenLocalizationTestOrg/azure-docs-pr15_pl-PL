<properties 
    pageTitle="Uruchamianie skryptu U SQL podczas analizy Lake danych Azure z fabryki danych Azure" 
    description="Dowiedz się, jak przetwarzanie danych, uruchamiając skrypty U SQL Azure danych Lake analizy do uruchamiania usługi." 
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
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="spelluru"/>

# <a name="run-u-sql-script-on-azure-data-lake-analytics-from-azure-data-factory"></a>Uruchamianie skryptu U SQL podczas analizy Lake danych Azure z fabryki danych Azure
> [AZURE.SELECTOR]
[Gałąź](data-factory-hive-activity.md)  
[Świnka](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Przesyłanie strumieniowe Hadoop](data-factory-hadoop-streaming-activity.md)
[Maszynowego uczenia](data-factory-azure-ml-batch-execution-activity.md) 
[Procedura przechowywana](data-factory-stored-proc-activity.md)
[Analizy Lake danych U-SQL](data-factory-usql-activity.md)
[.NET niestandardowe](data-factory-use-custom-activities.md)
 
Potok w factory Azure danych przetwarza danych w usługach magazynowania połączonych za pomocą usług połączonych obliczeń. Zawiera on kolejność miejsce, w którym każdej czynności wykonuje operację przetwarzania określone czynności. Ten artykuł zawiera opis **Działania U SQL analizy Lake danych** uruchamia skrypt **U SQL** **Azure danych Lake analizy** obliczeń połączone usługi. 

> [AZURE.NOTE] 
> Utwórz konto Azure danych Lake analizy przed utworzeniem potok z działaniem danych Lake analizy U-SQL. Aby uzyskać informacje o analizy Lake danych Azure, zobacz [Wprowadzenie do analizy Lake danych Azure](../data-lake-analytics/data-lake-analytics-get-started-portal.md).
>  
> Przeglądanie, [Tworzenie pierwszej samouczek planowana](data-factory-build-your-first-pipeline.md) Aby uzyskać szczegółowe instrukcje można tworzyć factory danych, usługi połączone, zestawy danych i potok. Użyj wstawek JSON Edytor Factory danych lub Visual Studio lub Azure programu PowerShell, aby utworzyć podmioty Factory danych.

## <a name="azure-data-lake-analytics-linked-service"></a>Usługi połączone analizy Lake Azure danych
Tworzenie usługi **Azure danych Lake analizy** połączone utworzyć łącze usługi obliczeń analizy Lake danych Azure factory Azure danych. Działania danych Lake analizy U-SQL w potoku odwołuje się do tej usługi połączone. 

W poniższym przykładzie zestawiono definicji JSON usługi Azure danych Lake analizy połączone. 

    {
        "name": "AzureDataLakeAnalyticsLinkedService",
        "properties": {
            "type": "AzureDataLakeAnalytics",
            "typeProperties": {
                "accountName": "adftestaccount",
                "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
                "authorization": "<authcode>",
                "sessionId": "<session ID>", 
                "subscriptionId": "<subscription id>",
                "resourceGroupName": "<resource group name>"
            }
        }
    }


Poniższa tabela zawiera opisy właściwości używana w definicji JSON. 

Właściwość | Opis | Wymagane
-------- | ----------- | --------
Typ | Właściwość Typ powinna być równa: **AzureDataLakeAnalytics**. | Tak
Nazwa konta | Nazwa konta analizy Lake Azure danych. | Tak
dataLakeAnalyticsUri | Identyfikator URI Lake analizy danych Azure. |  Brak 
Autoryzacja | Kod autoryzacji są pobierane automatycznie po kliknięciu pozycji przycisk **Autoryzuj** w edytorze Factory danych i kończenie pracy logowania OAuth.  | Tak 
subscriptionId | Identyfikator subskrypcji Azure | Nie (Jeśli nie zostanie określony, subskrypcji fabryki danych jest używany). 
resourceGroupName | Nazwa grupy zasobów Azure |  Nie (Jeśli nie zostanie określony, grupa zasobów fabryki danych jest używany).
Identyfikator sesji | Identyfikator sesji z sesji autoryzacji OAuth. Każdy identyfikator sesji jest unikatowy i może być użyty tylko raz. Sesja identyfikator jest generowane automatycznie w edytorze Factory danych. | Tak

Kod autoryzacji, który został wygenerowany przy użyciu przycisku **Autoryzuj** wygasa po upływie pewnego czasu. Zobacz następującą tabelę dla czasu wygaśnięcia dla różnych typów kont użytkowników. Może zostać wyświetlony następujący komunikat o błędzie wiadomości, kiedy uwierzytelniania **wygasa token**: poświadczeń błąd operacji: invalid_grant - AADSTS70002: błąd podczas sprawdzania poprawności poświadczeń. AADSTS70008: Udzielanie dostępu dostarczonych wygasła lub odwołany. Identyfikator śledzenia: Identyfikator korelacji d18629e8-af88-43c5-88e3-d8419eb1fca1: sygnatury czasowej fac30a0c-6be6-4e02-8d69-a776d2ffefd7: 2015-12-15 21:09:31Z

 
| Typ użytkownika | Dezaktualizuje się po |
| :-------- | :----------- | 
| Konta użytkowników nie zarządza usługi Azure Active Directory (@hotmail.com, @live.com, itp.) | 12 godzin |
| Użytkownicy kont zarządzanych przez Azure Active Directory (AAD) | Uruchom 14 dni od ostatniego wycinek. <br/><br/>90 dni, jeśli wycinek, oparte na podstawie OAuth powiązanych z jest uruchamiany co najmniej raz na 14 dni. |

Aby uniknąć i Rozwiąż ten błąd, ponownie autoryzować przy użyciu **Autoryzuj** przycisk kiedy **wygaśnie token** i ponownego wdrażania usługi połączone. Można również tworzyć wartości dla właściwości **identyfikator sesji** i **autoryzacji** programowo przy użyciu kodu w następnej sekcji. 

  
### <a name="to-programmatically-generate-sessionid-and-authorization-values"></a>Programowy generować wartości identyfikatora sesji i autoryzacji 

    if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
        linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
    {
        AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

        WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
        string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

        AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
        if (azureDataLakeStoreProperties != null)
        {
            azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
            azureDataLakeStoreProperties.Authorization = authorization;
        }

        AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
        if (azureDataLakeAnalyticsProperties != null)
        {
            azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
            azureDataLakeAnalyticsProperties.Authorization = authorization;
        }
    }

Zobacz tematy [AzureDataLakeStoreLinkedService klasy](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [Klasy AzureDataLakeAnalyticsLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)i [AuthorizationSessionGetResponse klasy](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) szczegółowe informacje na temat klasy Factory dane używane w kodzie. Dodaj odwołanie do: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll dla klasy WindowsFormsWebAuthenticationDialog. 
 
 
## <a name="data-lake-analytics-u-sql-activity"></a>Działanie U SQL Lake analizy danych 

Poniższy fragment JSON określa potok z działaniem danych Lake analizy U-SQL. Definicja aktywności odwołuje się do usługi Azure danych Lake analizy połączone, utworzony wcześniej.   
  

    {
        "name": "ComputeEventsByRegionPipeline",
        "properties": {
            "description": "This is a pipeline to compute events for en-gb locale and date less than 2012/02/19.",
            "activities": 
            [
                {
                    "type": "DataLakeAnalyticsU-SQL",
                    "typeProperties": {
                        "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
                        "scriptLinkedService": "StorageLinkedService",
                        "degreeOfParallelism": 3,
                        "priority": 100,
                        "parameters": {
                            "in": "/datalake/input/SearchLog.tsv",
                            "out": "/datalake/output/Result.tsv"
                        }
                    },
                    "inputs": [
                        {
                            "name": "DataLakeTable"
                        }
                    ],
                    "outputs": 
                    [
                        {
                            "name": "EventsByRegionTable"
                        }
                    ],
                    "policy": {
                        "timeout": "06:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "EventsByRegion",
                    "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
                }
            ],
            "start": "2015-08-08T00:00:00Z",
            "end": "2015-08-08T01:00:00Z",
            "isPaused": false
        }
    }


W poniższej tabeli opisano nazwy i opisy właściwości, które są specyficzne dla tego działania. 

Właściwość | Opis | Wymagane
:-------- | :----------- | :--------
Typ | Właściwość Typ musi być równa **DataLakeAnalyticsU-SQL**. | Tak
scriptPath | Ścieżka do folderu zawierającego skrypt języka SQL U. Nazwa pliku jest uwzględniana wielkość liter. | Nie (Jeśli używasz skryptu)
scriptLinkedService | Usługi połączone, która łączy przestrzeni dyskowej, zawierającego skrypt do fabryki danych | Nie (Jeśli używasz skryptu)
skrypt | Określ skrypt w tekście zamiast określającą scriptPath i scriptLinkedService. Na przykład: "skrypt": "Test Utwórz bazę danych". | Nie (jeśli jest używany scriptPath i scriptLinkedService)
degreeOfParallelism | Maksymalna liczba węzłów jednocześnie używane do wykonywania zadania. | Brak
Priority (priorytet) | Określa zadania, które ze wszystkich, który znajduje się w kolejce powinna już być zaznaczona do pierwszego uruchomienia. Mniejsza liczba, tym wyższy priorytet. | Brak 
Parametry | Parametry skrypt języka SQL U | Brak 

Patrz [SearchLogProcessing.txt skrypt](#script-definition) definicja skrypt. 

## <a name="sample-input-and-output-datasets"></a>Przykładowe dane wejściowe i wyjściowe zestawy danych

### <a name="input-dataset"></a>Zestaw wprowadzania danych
W tym przykładzie danych wejściowych znajduje się w magazynie Lake Azure danych (pliku SearchLog.tsv w folderze datalake i wprowadzania). 

    {
        "name": "DataLakeTable",
        "properties": {
            "type": "AzureDataLakeStore",
            "linkedServiceName": "AzureDataLakeStoreLinkedService",
            "typeProperties": {
                "folderPath": "datalake/input/",
                "fileName": "SearchLog.tsv",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }   

### <a name="output-dataset"></a>Dane wyjściowe zestawu danych
W tym przykładzie danych wyjściowych generowanych przez skrypt U SQL znajduje się w magazynie Lake danych Azure (datalake/wyjścia folder). 

    {
        "name": "EventsByRegionTable",
        "properties": {
            "type": "AzureDataLakeStore",
            "linkedServiceName": "AzureDataLakeStoreLinkedService",
            "typeProperties": {
                "folderPath": "datalake/output/"
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="sample-data-lake-store-linked-service"></a>Usługi połączone magazynu Lake przykładowych danych
Poniżej przedstawiono definicję próbki magazynu Lake danych Azure połączone usługę używaną przez wejścia i wyjścia zestawy danych. 

    {
        "name": "AzureDataLakeStoreLinkedService",
        "properties": {
            "type": "AzureDataLakeStore",
            "typeProperties": {
                "dataLakeUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
                "sessionId": "<session ID>",
                "authorization": "<authorization URL>"
            }
        }
    }

Zobacz artykuł [Przenoszenie danych do i z magazynu Lake danych Azure](data-factory-azure-datalake-connector.md) opisy właściwości JSON. 

## <a name="sample-u-sql-script"></a>Przykładowy skrypt U SQL 

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv(nullEscape:"#NULL#");
    
    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";
    
    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start <= DateTime.Parse("2012/02/19");
    
    OUTPUT @rs1   
        TO @out
          USING Outputters.Tsv(quoting:false, dateTimeFormat:null);

Wartości **@in** i **@out** parametrów w skrypt języka SQL U są przekazywane dynamicznie za ADF zgodnie z sekcją "Parametry". Zobacz sekcję "Parametry" w definicji procesu.

Inne właściwości, takie jak degreeOfParallelism i priorytet można określić także w swojej definicji planowana dla zadań, które wykonane w usłudze Azure danych Lake analizy.

## <a name="dynamic-parameters"></a>Parametry dynamiczne
W definicji planowana próbki Powiększ i Pomniejsz parametry są przypisywane z ustalonych wartości. 

    "parameters": {
        "in": "/datalake/input/SearchLog.tsv",
        "out": "/datalake/output/Result.tsv"
    }

Istnieje możliwość zamiast tego użyj parametry dynamiczne. Na przykład: 

    "parameters": {
        "in": "$$Text.Format('/datalake/input/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)",
        "out": "$$Text.Format('/datalake/output/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)"
    }

W tym przypadku pliki wejściowe są nadal pobierane z folderu /datalake/input i pliki wyjściowe są generowane w folderze /datalake/output. Nazwy plików pojawiają się w zależności od godziny rozpoczęcia wycinek.  